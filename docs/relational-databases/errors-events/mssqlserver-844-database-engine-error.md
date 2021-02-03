---
description: MSSQLSERVER_844
title: MSSQLSERVER_844 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 844 (Database Engine error)
ms.assetid: 2060c886-1226-4066-bc0c-de90a1cfb82b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0ae116b0ef06d82111e8ca356ae3f9c9a272f729
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186789"
---
# <a name="mssqlserver_844"></a>MSSQLSERVER_844
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|844|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|BUFLATCH_TIMEOUT_CONTINUE|  
|消息正文|等待缓冲区闩锁时出现超时 - 类型 %d，bp %p，页 %d:%d，stat %#x，数据库 ID: %d，分配单元 ID: %I64d%ls，任务 0x%p : %d，等待时间 %d，标志 0x%I64x，所属任务 0x%p。  将继续等待。|  
  
## <a name="explanation"></a>说明
SQL 进程正在等待获取闩锁。 此问题可能是由所用时间太长而无法完成的 I/O 操作导致的。 通常，此类型的错误是其他任务阻塞系统进程的结果。 在某些情况下，此错误可能是由硬件故障引起的。  出现此错误消息时，你可能会注意到计算机和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 停止响应。

## <a name="cause"></a>原因
此错误消息取决于系统的整体环境。 以下任何情况都可能会导致系统过载：

- 硬件不符合输入/输出 (I/O) 和内存需求
- 未正确配置和测试设置
- 设计低效

 系统负载过大且无法满足工作负载需求时，可能会看到错误 844。 造成环境超载的部分常见原因如下：

- 硬件问题
- 压缩卷
- 非默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置设置
- 查询或索引设计低效
- 数据库自动增长或自动收缩操作频繁

## <a name="user-action"></a>用户操作  
尝试执行以下操作以防止出现此错误：  
  
- 确定是否存在任何硬件瓶颈。 为获得良好的开端，请参阅[识别瓶颈](../performance/identify-bottlenecks.md)。 如有必要，升级硬件，使其满足环境配置、查询和负载的需求。

- 验证所有硬件是否正常工作。 检查任何已记录的错误并运行硬件供应商提供的任何诊断程序。 在错误日志或事件日志中检查关联的 I/O 故障。 I/O 故障通常是指磁盘故障。  
- 确保未压缩磁盘卷。 不支持在压缩盘上存储数据和日志文件。有关信息，请参阅[数据库文件和文件组](../databases/database-files-and-filegroups.md)。 有关压缩盘支持的其他信息，请查看以下文章：[压缩卷上不支持 SQL Server 数据库](https://support.microsoft.com/EN-US/help/231347)

- 关闭以下所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置选项时，查看错误消息是否消失：
   - [优先级提升选项](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md)
   - [轻型池（纤程模式）选项](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)
   - [设置工作集大小选项](../../database-engine/configure-windows/set-working-set-size-server-configuration-option.md)

    有关详细信息，请参阅[如何：确定正确的 SQL Server 配置设置](https://support.microsoft.com/EN-US/help/319942)

- 优化查询，以减少占用的系统资源。 性能优化有助于降低系统面临的压力，并缩短单个查询的响应时间
- 将自动收缩属性设置为 OFF，以降低更改数据库大小的开销
- 确保将自动增长属性设置为足够大的增量，以便降低文件增长的频率。 制定作业计划来检查数据库中的可用空间，然后在非高峰时间内增加数据库大小。
- 检查错误日志以查找非生成任务和其他错误。 先解决这些错误，因为这些错误可能是导致根本问题的根本原因。
- 如果频繁出现诸如断言之类的严重错误，请解决这些问题
- 如果很少出现 844 错误消息，则可以忽略这些错误