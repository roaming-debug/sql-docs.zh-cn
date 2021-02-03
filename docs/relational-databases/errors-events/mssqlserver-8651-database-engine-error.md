---
description: MSSQLSERVER_8651
title: MSSQLSERVER_8651 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8651 (Database Engine error)
ms.assetid: 4cc3498d-5449-4c4e-b1f9-3271831c725a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1892dbdba3b39f9b5d66837f58c596e2f7a3b759
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203986"
---
# <a name="mssqlserver_8651"></a>MSSQLSERVER_8651
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|8651|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|MEMGRANT_ERR|  
|消息正文|未能执行所请求的操作，因为可用内存少于最小查询内存。 请减小“每次查询占用的最小内存”服务器配置选项的配置值。|  
  
## <a name="explanation"></a>说明  
其他进程正在占用服务器内存（在服务器中施加内存压力）。  
  
## <a name="user-action"></a>用户操作  
减小“每次查询占用的最小内存”服务器配置选项的配置值，或者减少服务器的查询负载。  
  
下面的列表概述了有助于解决内存错误的一般步骤：  
  
1.  验证其他应用程序或服务是否占用此服务器上的内存。 重新配置不太重要的应用程序或服务，使其占用更少的内存。  
  
2.  开始收集以下内容的性能监视器计数器：**SQL Server:Buffer Manager**、**SQL Server:Memory Manager**。  
  
3.  检查下面的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存配置参数：  
  
    -   **max server memory**  
  
    -   **min server memory**  
  
    -   **min memory per query**  
  
    注意不正常的设置。 根据需要更正它们。 SQL Server 联机丛书的“设置服务器配置选项”中列出了默认设置。  
  
4.  检查工作负荷（例如，并发会话数，当前执行的查询）。  

以下操作可能会为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供更多内存：  
  
-   如果除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 外的应用程序正在占用资源，请尝试停止运行这些应用程序，或者考虑在单独的服务器上运行它们。 这样做将消除外部内存压力。  
  
-   如果已配置 **max server memory**，请增大其设置。  
  
运行以下 DBCC 命令以释放一些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存缓存。  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
如果问题仍存在，则您将需要进一步调查，可能需要减小工作负荷。  
  
## <a name="see-also"></a>另请参阅  
[DBCC FREESYSTEMCACHE (Transact-SQL)](~/t-sql/database-console-commands/dbcc-freesystemcache-transact-sql.md)  
[DBCC FREESESSIONCACHE (Transact-SQL)](~/t-sql/database-console-commands/dbcc-freesessioncache-transact-sql.md)  
[DBCC FREEPROCCACHE (Transact-SQL)](~/t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)  
[服务器配置选项 (SQL Server)](~/database-engine/configure-windows/server-configuration-options-sql-server.md)  
[SQL Server - Buffer Manager 对象](~/relational-databases/performance-monitor/sql-server-buffer-manager-object.md)  
[SQL Server - Memory Manager 对象](~/relational-databases/performance-monitor/sql-server-memory-manager-object.md)  
  
