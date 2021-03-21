---
title: 隔离级别（OLE DB 驱动程序）| Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 使用者如何控制连接的事务隔离级别。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- isolation levels [OLE DB]
- transactions [OLE DB]
- OLE DB Driver for SQL Server, transactions
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42b1bb476bd96bfdd58bb5ad8d4badd9f8a56793
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754277"
---
# <a name="isolation-levels-ole-db"></a>隔离级别 (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端可以控制连接的事务隔离级别。 为了控制事务隔离级别，OLE DB Driver for SQL Server使用者使用：  
  
-   将 DBPROPSET_SESSION 属性 DBPROP_SESS_AUTOCOMMITISOLEVELS 用于适用于 SQL Server 的 OLE DB 驱动程序默认自动提交模式。  
  
     OLE DB Driver for SQL Server 的默认级别是 DBPROPVAL_TI_READCOMMITTED。  
  
-   将 ITransactionLocal::StartTransaction 方法的 isoLevel 参数用于本地手动提交事务   。  
  
-   将 ITransactionDispenser::BeginTransaction 方法的 isoLevel 参数用于 MS DTC 协调的分布式事务   。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 在脏读隔离级别允许只读访问。 所有其他级别通过将锁应用到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 对象来限制并发。 当客户端需要更高的并发级别时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 会为数据并发访问设置更多限制。 若要维护最高级别的数据并发访问，适用于 SQL Server 的 OLE DB 驱动程序使用者应对特定并发级别请求进行智能控制。  
  
> [!NOTE]  
>  [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 引入了快照隔离级别。 有关详细信息，请参阅[使用快照隔离](../../oledb/features/working-with-snapshot-isolation.md)。  
  
## <a name="see-also"></a>另请参阅  
 [中的](../../oledb/ole-db-transactions/transactions.md)  
  
  
