---
description: 在 SQL Server Native Client 中支持本地事务
title: " (Native Client OLE DB 提供程序支持本地事务) "
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- autocommit mode
- transactions [OLE DB]
- ITransactionLocal interface
- SQL Server Native Client OLE DB provider, transactions
- local transactions [OLE DB]
ms.assetid: 78f2e5fc-b6fb-4eda-9f71-991a4d6c4902
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 019df854b297659f98d16646271c6f2076f41aa9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748727"
---
# <a name="supporting-local-transactions-in-sql-server-native-client"></a>在 SQL Server Native Client 中支持本地事务
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  会话分隔 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端 OLE DB 提供程序本地事务的事务范围。 当使用者的方向时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client OLE DB 提供程序将请求提交给已连接的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，该请求构成了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client OLE DB 提供程序的工作单元。 本地事务始终 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OLE DB 提供程序会话中包装一个或多个工作单元。  
  
 使用默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序自动提交模式，单个工作单元被视为本地事务的作用域。 只能有一个单元参与本地事务。 当创建会话时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序会为会话启动一个事务。 在成功完成工作单元之后提交工作。 如果失败，则回滚任何已开始的任何工作，并向使用者报告错误。 在任一情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序都将为会话启动新的本地事务，以便在事务中执行所有工作。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序使用者可以通过 **ITransactionLocal** 接口来更精确地控制本地事务范围。 当使用者会话启动事务时，事务起点和最终 Commit 或 Abort 方法调用之间的所有会话工作单元都被视为原子单元   。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]当使用者定向到此时，Native Client OLE DB 提供程序将隐式启动事务。 如果使用者未请求保持，该会话恢复为父事务级行为，通常为自动提交模式。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持 **ITransactionLocal：： StartTransaction** 参数，如下所示。  
  
|参数|说明|  
|---------------|-----------------|  
| isoLevel[in]|用于该事务的隔离级别。 在本地事务中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序支持以下各项：<br /><br /> **ISOLATIONLEVEL_UNSPECIFIED**<br /><br /> **ISOLATIONLEVEL_CHAOS**<br /><br /> **ISOLATIONLEVEL_READUNCOMMITTED**<br /><br /> **ISOLATIONLEVEL_READCOMMITTED**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_CURSORSTABILITY**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_SERIALIZABLE**<br /><br /> **ISOLATIONLEVEL_ISOLATED**<br /><br /> **ISOLATIONLEVEL_SNAPSHOT**<br /><br /> <br /><br /> 注意：从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，ISOLATIONLEVEL_SNAPSHOT 对 isoLevel 参数有效，而不管是否对数据库启用了版本支持  。 但是，如果用户尝试执行语句，并且未启用版本支持和/或数据库不为只读，则将发生错误。 此外，如果在连接到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 以前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本时将 ISOLATIONLEVEL_SNAPSHOT 指定为 isoLevel，将发生 XACT_E_ISOLATIONLEVEL 错误。|  
| isoFlags[in]|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序返回除零以外的任何值的错误。|  
| pOtherOptions[in]|如果不为 NULL，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序从接口请求 options 对象。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果 options 对象的 *ulTimeout* 成员不为零，则 Native Client OLE DB 提供程序将返回 XACT_E_NOTIMEOUT。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序忽略 *szDescription* 成员的值。|  
| pulTransactionLevel[out]|如果不为 NULL，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将返回事务的嵌套级别。|  
  
 对于本地事务， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序实现 **ITransaction：： Abort** 参数，如下所示。  
  
|参数|说明|  
|---------------|-----------------|  
| pboidReason[in]|忽略（如果设置）。 可以安全地为 NULL。|  
| fRetaining[in]|当该参数为 TRUE 时，将针对会话隐式开始新的事务。 事务必须由使用者提交或中止。 为 FALSE 时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将恢复为会话的自动提交模式。|  
| fAsync[in]|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序不支持异步中止。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果值不为 FALSE，则 Native Client OLE DB 提供程序将返回 XACT_E_NOTSUPPORTED。|  
  
 对于本地事务， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序实现 **ITransaction：： Commit** 参数，如下所示。  
  
|参数|说明|  
|---------------|-----------------|  
| fRetaining[in]|当该参数为 TRUE 时，将针对会话隐式开始新的事务。 事务必须由使用者提交或中止。 为 FALSE 时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将恢复为会话的自动提交模式。|  
| grfTC[in]|Native Client OLE DB 提供程序不支持异步和阶段的一个返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序为除 XACTTC_SYNC 之外的任何值返回 XACT_E_NOTSUPPORTED。|  
| grfRM[in]|必须为 0。|  
  
 会话上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序行集基于行集属性的值在本地提交或中止操作上保留 DBPROP_ABORTPRESERVE 和 DBPROP_COMMITPRESERVE。 默认情况下，这些属性都是 VARIANT_FALSE 的，并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会话上的所有 Native Client OLE DB 提供程序行集在中止或提交操作后将丢失。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序未实现 **ITransactionObject** 接口。 尝试检索接口上的引用的使用者将返回 E_NOINTERFACE。  
  
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
 [事务](../../relational-databases/native-client-ole-db-transactions/transactions.md)   
 [使用快照隔离](../../relational-databases/native-client/features/working-with-snapshot-isolation.md)  
  
  
