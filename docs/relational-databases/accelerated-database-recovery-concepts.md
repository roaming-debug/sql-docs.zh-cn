---
description: 加速数据库恢复
title: 加速数据库恢复 (ADR) | Microsoft Docs
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- accelerated database recovery [SQL Server], recovery-only
- database recovery [SQL Server]
author: mashamsft
ms.author: mathoma
ms.reviewer: kfarlee
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 70b68fa62c523a4c65bb4e70538e1882bc67290d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350072"
---
# <a name="accelerated-database-recovery"></a>加速数据库恢复

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

通过重新设计 SQL 数据库引擎恢复过程，加速数据库恢复 (ADR) 可提高数据库可用性，尤其是存在长时间运行的事务时。 ADR 是 SQL Server 2019 中的新增内容。 

ADR 也适用于 Azure SQL 数据库、Azure SQL 托管实例和 Azure Synapse SQL 中的数据库。 ADR 在 SQL 数据库和 SQL 托管实例中会默认启用，并且无法禁用。 

## <a name="overview"></a>概述

ADR 的主要优势在于：

- **快速且一致的数据库恢复**

  使用 ADR，长时间运行的事务不会影响整体恢复时间，且无论系统中活动事务的数量或大小如何，都可以实现快速且一致的数据库恢复。

- **即时事务回滚**

  使用 ADR，事务回滚是即时的，与事务处于活动状态的时间或已执行的更新次数无关。

- **主动日志截断**

  即使存在活动且长时间运行的事务，ADR 也会主动截断事务日志，这可以防止其增长失控。

## <a name="the-current-database-recovery-process"></a>当前数据库恢复过程

如果没有 ADR，SQL Server 中的数据库恢复会遵循 [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) 恢复模式，包括三个阶段（如下图所示且在关系图下方会进行更详细的说明）。

![当前恢复过程](./media/accelerated-database-recovery-concepts/current-recovery-process.png)

- **分析阶段**

  SQL Server 从上一个成功的检查点（或最早的脏页 LSN）的开头起到末尾，执行事务日志的前向扫描，以确定 SQL Server 停止时每个事务的状态。

- **重做阶段**

  SQL Server 从最早的未提交事务起到末尾，执行事务日志的前向扫描，通过重做所有已提交的操作使数据库恢复到崩溃时的状态。

- **撤消阶段**

  对于在崩溃时处于活动状态的每个事务，SQL Server 向后遍历日志，从而撤消该事务执行的操作。

基于此设计，数据库引擎从意外重启中恢复所花费的时间（大约值）与崩溃时系统中最长活动事务的大小成正比。 恢复需要回滚所有未完成的事务。 所需的时间长度与事务已执行的工作及其处于活动状态的时间成正比。 因此，存在长时间运行的事务（例如大型表的大批量插入操作或索引生成操作）时，SQL Server 恢复过程可能需要很长时间。

此外，基于此设计的取消或回滚大型事务也可能需要较长时间，因为其使用的是与上面所述相同的撤消恢复阶段。

另外，当存在长时间运行的事务时，数据库引擎无法截断事务日志，因为恢复和回滚过程需要其相应的日志记录。 因此，某些事务日志会变得很大，并且会占用大量驱动器空间。

## <a name="the-accelerated-database-recovery-process"></a>加速数据库恢复过程

ADR 通过完全重新设计数据库引擎恢复过程来解决上述问题：

- 通过避免以最早的活动事务为起始点/结束点扫描日志，使其保持恒定时间/即时状态。 使用 ADR，仅从上一个成功的检查点 [或最早的脏页日志序列号 (LSN)] 处理事务日志。 因此，恢复时间不受长时间运行的事务影响。
- 由于不再需要为整个事务处理日志，因此可最大程度地减少所需的事务日志空间。 当检查点和备份出现时，可以主动截断事务日志。

从较高层次来看，ADR 可通过对所有物理数据库修改进行版本控制并仅撤消逻辑操作（逻辑操作比较有限，且几乎可以立即撤消）来实现快速数据库恢复。 崩溃时处于活动状态的任何事务都会被标记为已中止，因此，并发用户查询可忽略这些事务生成的任何版本。

ADR 恢复过程与当前恢复过程具有相同的三个阶段。 下图说明了如何在这些阶段使用 ADR 进行操作。

![ADR 恢复过程](./media/accelerated-database-recovery-concepts/adr-recovery-process.png)

- **分析阶段**

  除了为非版本控制操作重建 sLog 和复制日志记录，此过程与当前恢复过程相同。
  
- **重做阶段**

  分为两个子阶段
  - 子阶段 1

      从 sLog 重做（从最早的未提交事务到上一个检查点）。 重做是一种快速操作，因为它仅需要处理 sLog 中的一些记录。

  - 子阶段 2

     从上一个检查点（而不是最早的未提交事务）起的事务日志重做
     
- **撤消阶段**

   使用 ADR，撤消阶段几乎即时完成 - 通过使用 sLog 撤消非版本控制操作以及通过具有逻辑还原的持久版本存储 (PVS) 执行行级别基于版本的撤消。

还可以观看此 8 分钟的视频，了解加速数据库恢复

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Advanced-Database-Recovery--Data-Exposed/player?WT.mc_id=dataexposed-c9-niner]

## <a name="adr-recovery-components"></a>ADR 恢复组件

ADR 的四个关键组件是：

- **持久版本存储 (PVS)**

  持久版本存储是一种数据库引擎机制，用于持久保存数据库本身，而不是传统 `tempdb` 版本存储中生成的行版本。 PVS 可实现资源隔离，并提高可读辅助数据库的可用性。

- **逻辑还原**

  逻辑还原是负责执行行级别基于版本的撤消的异步过程，为所有版本控制操作提供即时事务回滚和撤消。

  - 跟踪所有已中止的事务
  - 使用 PVS 对所有用户事务执行回滚
  - 事务中止后立即释放所有锁

- **sLog**

  sLog 是一个辅助数据库内存中日志流，用于存储非版本控制操作（如元数据缓存无效、锁获取等）的日志记录。 sLog 具有以下特性：

  - 低容量和内存中
  - 通过在检查点过程中序列化保留在磁盘上
  - 提交事务时定期被截断
  - 通过仅处理非版本控制操作来加速重做和撤消  
  - 通过仅保留所需的日志记录来实现主动事务日志截断

- **清理器**

  清理器是定期唤醒并清除不需要的页面版本的异步过程。

## <a name="who-should-consider-accelerated-database-recovery"></a>谁应考虑加速数据库恢复

以下类型的客户应考虑启用 ADR：

- 具有带有长时间运行事务的工作负载的客户。
- 发现活动事务正在导致事务日志显著增大情况的客户。  
- 由于 SQL Server 长时间运行的恢复（如意外的 SQL Server 重启或手动事务回滚）而经历了长时间数据库不可用的客户。

>[!IMPORTANT]
>在数据库镜像中注册的数据库不支持 ADR。

## <a name="see-also"></a>另请参阅  

[管理加速数据库恢复](accelerated-database-recovery-management.md)
