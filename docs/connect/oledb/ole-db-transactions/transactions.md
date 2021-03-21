---
title: 事务（OLE DB 驱动程序）
description: 了解 OLE DB Driver for SQL Server 如何支持本地事务。 将 Microsoft 分布式事务处理协调器用于分布式事务。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- transactions [OLE DB]
- OLE DB Driver for SQL Server, transactions
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: da8716e20f060742f04fb628820ddc4aa7e70576
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754187"
---
# <a name="transactions"></a>事务
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server 实现本地事务支持。 使用者可借助 Microsoft 分布式事务处理协调器 (MS DTC) 来使用分布式事务或协调事务。 对于需要跨多个会话的事务控制权的使用者，适用于 SQL Server 的 OLE DB 驱动程序可以加入由 MS DTC 启动和维护的事务。  
  
 默认情况下，适用于 SQL Server 的 OLE DB 驱动程序使用自动提交事务模式，其中对使用者会话执行的每次离散操作均包含一个针对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的完整事务。 适用于 SQL Server 的 OLE DB 驱动程序的自动提交模式是本地的，并且自动提交事务从不会跨多个会话。  
  
 适用于 SQL Server 的 OLE DB 驱动程序公开 ITransactionLocal 接口，并允许使用者在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的单个连接上使用显式和隐式启动事务。 OLE DB Driver for SQL Server 不支持嵌套的本地事务。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [支持本地事务](../../oledb/ole-db-transactions/supporting-local-transactions.md)  
  
-   [支持分布式事务](../../oledb/ole-db-transactions/supporting-distributed-transactions.md)  
  
-   [隔离级别 (OLE DB)](../../oledb/ole-db-transactions/isolation-levels-ole-db.md)  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序编程](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
