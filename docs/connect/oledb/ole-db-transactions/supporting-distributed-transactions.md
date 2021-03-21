---
title: 支持分布式事务（OLE DB 驱动程序）
description: 了解 OLE DB Driver for SQL Server 使用者如何使用 ITransactionJoin::JoinTransaction 方法参与分布式事务。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- distributed transactions [OLE DB]
- MS DTC
- transactions [OLE DB]
- OLE DB Driver for SQL Server, transactions
- ITransactionJoin interface
- MS DTC, about distributed transaction support
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 88c3b8414c854667ed5722a0fd044e8e28cfd4b9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754227"
---
# <a name="supporting-distributed-transactions"></a>支持分布式事务
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  适用于 SQL Server 的 OLE DB 驱动程序使用者可以使用 ITransactionJoin::JoinTransaction 方法参与由 Microsoft 分布式事务处理协调器 (MS DTC) 协调的分布式事务  。  
  
 MS DTC 公开 COM 对象，这些对象允许客户端跨各种数据存储的多个连接来启动和参与协调的事务。 为了启动事务，OLE DB Driver for SQL Server 使用者使用 MS DTC ITransactionDispenser  接口。 ITransactionDispenser 的 BeginTransaction 成员返回分布式事务对象的引用   。 此引用使用 JoinTransaction  传递给 OLE DB Driver for SQL Server。  
  
 MS DTC 支持对分布式事务的异步提交和中止。 为了通知异步事务状态，使用者实现 ITransactionOutcomeEvents 接口并将该接口与 MS DTC 事务对象连接  。  
  
 对于分布式事务，适用于 SQL Server 的 OLE DB 驱动程序按如下方式实现 ITransactionJoin::JoinTransaction 参数  。  
  
|参数|说明|  
|---------------|-----------------|  
|punkTransactionCoord |指向 MS DTC 事务对象的指针。|  
|IsoLevel |被 OLE DB Driver for SQL Server 忽略。 使用者在从 MS DTC 获取事务对象时，确定由 MS DTC 协调的事务的隔离级别。|  
|IsoFlags |必须为 0。 如果使用者指定了其他任何值，OLE DB Driver for SQL Server 返回 XACT_E_NOISORETAIN。|  
|POtherOptions |如果不是 NULL，OLE DB Driver for SQL Server 将从该接口请求选项对象。 如果 options 对象的 ulTimeout  成员不为零，OLE DB Driver for SQL Server 返回 XACT_E_NOTIMEOUT。 OLE DB Driver for SQL Server 忽略 szDescription  成员的值。|  
  
 下面的示例通过使用 MS DTC 来协调事务：  
  
```  
// Interfaces used in the example.  
IDBCreateSession*       pIDBCreateSession   = NULL;  
ITransactionJoin*       pITransactionJoin   = NULL;  
IDBCreateCommand*       pIDBCreateCommand   = NULL;  
IRowset*                pIRowset            = NULL;  
  
// Transaction dispenser and transaction from MS DTC.  
ITransactionDispenser*  pITransactionDispenser = NULL;  
ITransaction*           pITransaction       = NULL;  
  
    HRESULT             hr;  
  
// Get the command creation interface for the session.  
if (FAILED(hr = pIDBCreateSession->CreateSession(NULL,  
     IID_IDBCreateCommand, (IUnknown**) &pIDBCreateCommand)))  
    {  
    // Process error from session creation. Release any references and  
    // return.  
    }  
  
// Get a transaction dispenser object from MS DTC and  
// start a transaction.  
if (FAILED(hr = DtcGetTransactionManager(NULL, NULL,  
    IID_ITransactionDispenser, 0, 0, NULL,  
    (void**) &pITransactionDispenser)))  
    {  
    // Process error message from MS DTC, release any references,  
    // and then return.  
    }  
if (FAILED(hr = pITransactionDispenser->BeginTransaction(  
    NULL, ISOLATIONLEVEL_READCOMMITTED, ISOFLAG_RETAIN_DONTCARE,  
    NULL, &pITransaction)))  
    {  
    // Process error message from MS DTC, release any references,  
    // and then return.  
    }  
  
// Join the transaction.  
if (FAILED(pIDBCreateCommand->QueryInterface(IID_ITransactionJoin,  
    (void**) &pITransactionJoin)))  
    {  
    // Process failure to get an interface, release any references, and  
    // then return.  
    }  
if (FAILED(pITransactionJoin->JoinTransaction(  
    (IUnknown*) pITransaction, 0, 0, NULL)))  
    {  
    // Process join failure, release any references, and then return.  
    }  
  
// Get data into a rowset, then update the data. Functions are not  
// illustrated in this example.  
if (FAILED(hr = ExecuteCommand(pIDBCreateCommand, &pIRowset)))  
    {  
    // Release any references and return.  
    }  
  
// If rowset data update fails, then terminate the transaction, else  
// commit. The example doesn't retain the rowset.  
if (FAILED(hr = UpdateDataInRowset(pIRowset, bDelayedUpdate)))  
    {  
    // Get error from update, then abort.  
    pITransaction->Abort(NULL, FALSE, FALSE);  
    }  
else  
    {  
    if (FAILED(hr = pITransaction->Commit(FALSE, 0, 0)))  
        {  
        // Get error from failed commit.  
        //  
        // If a distributed commit fails, application logic could  
        // analyze failure and retry. In this example, terminate. The   
        // consumer must resolve this somehow.  
        pITransaction->Abort(NULL, FALSE, FALSE);  
        }  
    }  
  
if (FAILED(hr))  
    {  
    // Update of data or commit failed. Release any references and  
    // return.  
    }  
  
// Un-enlist from the distributed transaction by setting   
// the transaction object pointer to NULL.  
if (FAILED(pITransactionJoin->JoinTransaction(  
    (IUnknown*) NULL, 0, 0, NULL)))  
    {  
    // Process failure, and then return.  
    }  
  
// Release any references and continue.  
```  
  
## <a name="see-also"></a>另请参阅  
 [中的](../../oledb/ole-db-transactions/transactions.md)  
  
  
