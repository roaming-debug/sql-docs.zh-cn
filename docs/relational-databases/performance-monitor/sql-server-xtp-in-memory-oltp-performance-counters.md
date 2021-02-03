---
title: XTP（内存中 OLTP）性能计数器
description: SQL Server 提供了对象和计数器，性能监视器可以使用它们来监视内存中 OLTP 活动。
ms.custom: seo-dt-2019
ms.date: 04/06/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: fe3cbaf4-65f4-44c5-acc6-7b735cda0c5d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4c6e97837d26e680f7f94bcdc60132127a11afe4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237715"
---
# <a name="sql-server-xtp-in-memory-oltp-performance-counters"></a>SQL Server XTP（内存中 OLTP）性能计数器
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了对象和计数器，性能监视器可以使用它们来监视内存中 OLTP 活动。 这些对象和计数器在计算机上给定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的所有实例之间共享，从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]启动。  
  
 对象和计数器的名称在过去以 *XTP* 开头，如“XTP 游标”中所示。 现在，自 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]开始，其名称将遵循以下模式：  
  
-   SQL Server \<version> XTP 游标  
  
 对于 \<version>其值类似于 2016。  
  
##  <a name="sql-server-xtp-performance-objects"></a><a name="SQLServerPOs"></a> SQL Server XTP 性能对象  
 下表介绍了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 性能对象。  
  
|性能对象|说明|  
|------------------------|-----------------|  
|[SQL Server XTP 游标](../../relational-databases/performance-monitor/sql-server-xtp-cursors.md)|SQL Server XTP 游标性能对象包含与内部内存中 OLTP 引擎游标相关的计数器。 游标是内存中 OLTP 引擎用于处理 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询的低级构建基块。 因此，您通常不能直接控制游标。|  
|[SQL Server XTP 数据库](../../relational-databases/performance-monitor/sql-server-xtp-databases.md)|SQL Server XTP 数据库性能对象提供内存中 OLTP 数据库特定计数器。|  
|[SQL Server XTP 垃圾收集](../../relational-databases/performance-monitor/sql-server-xtp-garbage-collection.md)|SQL Server XTP 垃圾回收性能对象包含与内存中 OLTP 引擎的垃圾回收器相关的计数器。|  
|[SQL Server 2016 XTP IO 调控器](../../relational-databases/performance-monitor/sql-server-xtp-io-governor.md)|SQL Server XTP IO 调控器性能对象包含与内存中 OLTP IO 速率调控器相关的计数器。|
|[SQL Server XTP 虚拟处理器](../../relational-databases/performance-monitor/sql-server-xtp-phantom-processor.md)|SQL Server XTP 虚拟处理器性能对象包含与内存中 OLTP 引擎的虚拟处理子系统相关的计数器。 此组件用于检测在 SERIALIZABLE 隔离级别运行的事务中的虚拟行。|  
|[SQL Server XTP 存储](../../relational-databases/performance-monitor/sql-server-xtp-storage.md)|SQL Server XTP 存储性能对象包含与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的内存中 OLTP 存储相关的计数器。|  
|[SQL Server XTP 事务日志](../../relational-databases/performance-monitor/sql-server-xtp-transaction-log.md)|SQL Server XTP 事务日志性能对象包含与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的内存中 OLTP 事务日志相关的计数器。|  
|[SQL Server XTP 事务](../../relational-databases/performance-monitor/sql-server-xtp-transactions.md)|SQL Server XTP 事务日志性能对象包含与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的内存中 OLTP 引擎事务相关的计数器。|  
  
  
