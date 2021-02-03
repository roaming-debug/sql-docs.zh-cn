---
description: MSSQLSERVER_846
title: MSSQLSERVER_846 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 846 (Database Engine error)
ms.assetid: ccf367eb-06b0-42b8-b4d6-2b88f4a502d3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 94fc86afb22e8bcb3db6dea94b1f06b62bb43282
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186758"
---
# <a name="mssqlserver_846"></a>MSSQLSERVER_846
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|846|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|空值|  
|消息正文|等待缓冲区闩锁时出现超时 - 类型 %d，bp %p，页 %d:%d，stat %#x，数据库 ID: %d，分配单元 ID: %I64d%ls，任务 0x%p : %d，等待时间 %d，标志 0x%I64x，所属任务 0x%p。 将不继续等待。|  
  
## <a name="explanation"></a>说明  
计算机可能停止响应，或在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将缓冲区闩锁错误写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志的同时可能出现超时或某些其他常规操作中断。  
  
如果消息中的状态字段的值为 0x04 on，则表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在等待 I/O 操作。 也可能在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中收到消息 [MSSQLSERVER_833](~/relational-databases/errors-events/mssqlserver-833-database-engine-error.md)。  
  
如果消息中的状态字段的值为 0x04 off，则表示存在对某个页的大量争用。 如果对象是数据页，则错误可能是由低效的代码设计引起的。 如果是非数据页，则错误可能由服务器瓶颈引起，如硬件资源不足。  
  
## <a name="user-action"></a>用户操作  
若要解决此问题，根据环境的不同，采取以下一个或多个步骤可能会减少或消除错误消息：  
  
-   确定是否存在硬件瓶颈。 如有必要，请升级您的硬件以便能够支持环境的配置、查询和负载要求。 有关瓶颈的详细信息，请参阅[识别瓶颈](~/relational-databases/performance/identify-bottlenecks.md)。  
  
-   检查任何已记录的错误并运行硬件供应商提供的任何诊断程序。  
  
-   确保未压缩磁盘驱动器。 不支持将数据或日志文件存储在压缩驱动器上。 有关物理文件的详细信息，请参阅[数据库文件和文件组](~/relational-databases/databases/database-files-and-filegroups.md)。  
  
-   查看在您将以下选项设置为关闭时错误消息是否消失：  
  
    -   SQL Server priority boost 配置选项  
  
    -   Lightweight pooling（纤程模式）选项  
  
    -   set working set size 选项  
  
    > [!NOTE]  
    > 如果对以上设置的默认设置 OFF 进行更改，则这些设置可能会经常使系统效率降低。 有关设置的详细信息，请参阅[服务器配置选项 (SQL Server)](~/database-engine/configure-windows/server-configuration-options-sql-server.md)。  
  
-   优化查询，以减少占用的系统资源。 性能优化将有助于降低系统面临的压力，并缩短单个查询的响应时间。  
  
-   将 AUTO_SHRINK 选项设置为 OFF，以降低更改数据库大小的开销。  
  
-   确保将 FILEGROWTH 选项设置为足够大的增量，以便减少文件增长的频率。 制定一个检查数据库中可用空间的作业计划，然后在非高峰时间内增加数据库大小。  
  
