---
title: 支持本地事务（OLE DB 驱动程序）
description: 了解 OLE DB Driver for SQL Server 如何支持本地事务，以及如何使用 ITransactionLocal 更精确地控制本地事务范围。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- autocommit mode
- transactions [OLE DB]
- ITransactionLocal interface
- OLE DB Driver for SQL Server, transactions
- local transactions [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41f797115262d928e9ec47ab13144dba1a98e6bb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754207"
---
# <a name="supporting-local-transactions"></a>支持本地事务
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  会话分隔 OLE DB Driver for SQL Server 本地事务的事务范围。 当 OLE DB Driver for SQL Server 根据使用者指示向连接的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例提交请求时，该请求构成 OLE DB Driver for SQL Server 的工作单元。 本地事务始终在一个 OLE DB Driver for SQL Server 会话中包装一个或多个工作单元。  
  
 使用默认的适用于 SQL Server 的 OLE DB 驱动程序自动提交模式，将一个工作单元视为本地事务的范围。 只能有一个单元参与本地事务。 创建会话时，OLE DB Driver for SQL Server 开始该会话的事务。 在成功完成工作单元之后提交工作。 如果失败，则回滚任何已开始的任何工作，并向使用者报告错误。 无论哪种情况，适用于 SQL Server 的 OLE DB 驱动程序都会为会话开始新的本地事务，以便在一个事务中执行所有工作。  
  
 适用于 SQL Server 的 OLE DB 驱动程序使用者可使用 ITransactionLocal 接口对本地事务范围进行更精确的控制  。 当使用者会话启动事务时，事务起点和最终 Commit 或 Abort 方法调用之间的所有会话工作单元都被视为原子单元   。 当使用者指示开始事务时，OLE DB Driver for SQL Server 将隐式执行此操作。 如果使用者未请求保持，该会话恢复为父事务级行为，通常为自动提交模式。  
  
 OLE DB Driver for SQL Server 支持以下 ITransactionLocal::StartTransaction  参数。  
  
|参数|说明|  
|---------------|-----------------|  
| isoLevel[in]|用于该事务的隔离级别。 在本地事务中，OLE DB Driver for SQL Server 支持以下各项：<br /><br /> **ISOLATIONLEVEL_UNSPECIFIED**<br /><br /> **ISOLATIONLEVEL_CHAOS**<br /><br /> **ISOLATIONLEVEL_READUNCOMMITTED**<br /><br /> **ISOLATIONLEVEL_READCOMMITTED**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_CURSORSTABILITY**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_SERIALIZABLE**<br /><br /> **ISOLATIONLEVEL_ISOLATED**<br /><br /> **ISOLATIONLEVEL_SNAPSHOT**<br /><br /> <br /><br /> 注意：从 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 开始，ISOLATIONLEVEL_SNAPSHOT 对 isoLevel 参数有效，而不管是否对数据库启用了版本支持  。 但是，如果用户尝试执行语句，并且未启用版本支持和/或数据库不为只读，则将发生错误。 此外，如果在连接到 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 以前的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本时将 ISOLATIONLEVEL_SNAPSHOT 指定为 isoLevel，将发生 XACT_E_ISOLATIONLEVEL 错误。|  
| isoFlags[in]|OLE DB Driver for SQL Server 针对任何非零值返回错误。|  
| pOtherOptions[in]|如果不是 NULL，OLE DB Driver for SQL Server 将从该接口请求选项对象。 如果 options 对象的 ulTimeout  成员不为零，OLE DB Driver for SQL Server 返回 XACT_E_NOTIMEOUT。 OLE DB Driver for SQL Server 忽略 szDescription  成员的值。|  
| pulTransactionLevel[out]|如果不为 NULL，OLE DB Driver for SQL Server 将返回事务的嵌套级别。|  
  
 对于本地事务，OLE DB Driver for SQL Server 按如下方式实现 ITransaction::Abort  参数。  
  
|参数|说明|  
|---------------|-----------------|  
| pboidReason[in]|忽略（如果设置）。 可以安全地为 NULL。|  
| fRetaining[in]|当该参数为 TRUE 时，将针对会话隐式开始新的事务。 事务必须由使用者提交或中止。 当该参数为 FALSE 时，OLE DB Driver for SQL Server 则恢复到会话的自动提交模式。|  
| fAsync[in]|OLE DB Driver for SQL Server 不支持异步中止。 如果该值不为 FALSE，OLE DB Driver for SQL Server 返回 XACT_E_NOTSUPPORTED。|  
  
 对于本地事务，OLE DB Driver for SQL Server 按如下方式实现 ITransaction::Commit  参数。  
  
|参数|说明|  
|---------------|-----------------|  
| fRetaining[in]|当该参数为 TRUE 时，将针对会话隐式开始新的事务。 事务必须由使用者提交或中止。 当该参数为 FALSE 时，OLE DB Driver for SQL Server 则恢复到会话的自动提交模式。|  
| grfTC[in]|OLE DB Driver for SQL Server 不支持第一阶段的异步返回。 OLE DB Driver for SQL Server 针对除 XACTTC_SYNC 以外的任何值返回 XACT_E_NOTSUPPORTED。|  
| grfRM[in]|必须为 0。|  
  
 会话上的适用于 SQL Server 的 OLE DB 驱动程序行集根据行集属性 DBPROP_ABORTPRESERVE 和 DBPROP_COMMITPRESERVE 的值保留在本地提交或中止操作中。 默认情况下，这些属性均为 VARIANT_FALSE，并且在执行中止或提交操作之后，将丢失该会话上的所有适用于 SQL Server 的 OLE DB 驱动程序行集。  
  
 OLE DB Driver for SQL Server 不实现 ITransactionObject  接口。 尝试检索接口上的引用的使用者将返回 E_NOINTERFACE。  
  
 此示例使用 ITransactionLocal  。  
  
```  
// Interfaces used in the example.  
IDBCreateSession*   pIDBCreateSession   = NULL;  
ITransaction*       pITransaction       = NULL;  
IDBCreateCommand*   pIDBCreateCommand   = NULL;  
IRowset*            pIRowset            = NULL;  
  
HRESULT             hr;  
  
// Get the command creation and local transaction interfaces for the  
// session.  
if (FAILED(hr = pIDBCreateSession->CreateSession(NULL,  
     IID_IDBCreateCommand, (IUnknown**) &pIDBCreateCommand)))  
    {  
    // Process error from session creation. Release any references and  
    // return.  
    }  
  
if (FAILED(hr = pIDBCreateCommand->QueryInterface(IID_ITransactionLocal,  
    (void**) &pITransaction)))  
    {  
    // Process error. Release any references and return.  
    }  
  
// Start the local transaction.  
if (FAILED(hr = ((ITransactionLocal*) pITransaction)->StartTransaction(  
    ISOLATIONLEVEL_REPEATABLEREAD, 0, NULL, NULL)))  
    {  
    // Process error from StartTransaction. Release any references and  
    // return.  
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
    // Get error from update, then terminate.  
    pITransaction->Abort(NULL, FALSE, FALSE);  
    }  
else  
    {  
    if (FAILED(hr = pITransaction->Commit(FALSE, XACTTC_SYNC, 0)))  
        {  
        // Get error from failed commit.  
        }  
    }  
  
if (FAILED(hr))  
    {  
    // Update of data or commit failed. Release any references and  
    // return.  
    }  
  
// Release any references and continue.  
```  
  
## <a name="see-also"></a>另请参阅  
 [事务](../../oledb/ole-db-transactions/transactions.md)   
 [使用快照隔离](../../oledb/features/working-with-snapshot-isolation.md)  
  
  
