---
title: 执行异步操作 | Microsoft Docs
description: OLE DB Driver for SQL Server 支持异步数据库操作，从而能够在不阻止调用线程的情况下返回方法。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- initialization [OLE DB Driver for SQL Server]
- database connections [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], asynchronous operations
- connections [OLE DB Driver for SQL Server]
- asynchronous operations [OLE DB Driver for SQL Server]
- rowsets [SQL Server], initializing
- MSOLEDBSQL, asynchronous operations
- OLE DB Driver for SQL Server, asynchronous operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d71b9884b8a87ae05060073f9c2d47eb044387a5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742287"
---
# <a name="performing-asynchronous-operations"></a>执行异步操作
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 允许应用程序执行异步数据库操作。 异步处理将使方法能够立即返回，而不会阻塞调用线程。 这使得多线程机制能提供更强大的功能和灵活性，而不需要开发人员显式创建线程或处理同步。 应用程序将在初始化数据库连接时或初始化由执行命令所生成的结果时请求异步处理。  
  
## <a name="opening-and-closing-a-database-connection"></a>打开和关闭数据库连接  
 使用 适用于 SQL Server 的 OLE DB 驱动程序时，按设计以异步方式初始化数据源对象的应用程序可在调用 IDBInitialize::Initialize 之前，设置 DBPROP_INIT_ASYNCH 属性中的 DBPROPVAL_ASYNCH_INITIALIZE 位  。 设置此数据时，提供程序立即通过对 Initialize 的调用返回值。如果操作立即完成，则返回 S_OK；如果初始化以异步方式继续进行，则返回 DB_S_ASYNCHRONOUS  。 应用程序可查询数据源对象的 IDBAsynchStatus 或 [ISSAsynchStatus](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md) 接口，然后调用 IDBAsynchStatus::GetStatus 或 [ISSAsynchStatus::WaitForAsynchCompletion](../../oledb/ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md)，从而获得初始化的状态 。  
  
 此外，SSPROP_ISSAsynchStatus 属性已添加到 DBPROPSET_SQLSERVERROWSET 属性集。 支持 **ISSAsynchStatus** 接口的提供程序必须使用值 VARIANT_TRUE 实现此属性。  
  
 可调用 IDBAsynchStatus::Abort 或 [ISSAsynchStatus::Abort](../../oledb/ole-db-interfaces/issasynchstatus-abort-ole-db.md) 来取消 Initialize 异步调用 。 使用者必须显式请求异步数据源初始化。 否则，必须等到数据源对象完全初始化之后，IDBInitialize::Initialize 才返回值  。  
  
> [!NOTE]  
>  连接池使用的数据源对象无法在适用于 SQL Server 的 OLE DB 驱动程序中调用 ISSAsynchStatus 接口  。 ISSAsynchStatus 接口不对入池数据源对象公开  。  
>   
>  如果应用程序显式强制使用游标引擎，则 IOpenRowset::OpenRowset 和 IMultipleResults::GetResult 不支持异步处理   。  
>   
>  此外，远程代理（或 MDAC 2.8 中的存根 dll）无法在 OLE DB Driver for SQL Serve 中调用 ISSAsynchStatus 接口  。 ISSAsynchStatus 接口不通过远程公开  。  
>   
>  服务组件不支持 ISSAsynchStatus  。  
  
## <a name="execution-and-rowset-initialization"></a>执行和行集初始化  
 被设计成以异步方式打开由执行命令所生成的结果的应用程序可以设置 DBPROP_ROWSET_ASYNCH 属性中的 DBPROPVAL_ASYNCH_INITIALIZE 位。 在调用 IDBInitialize::Initialize、ICommand::Execute、IOpenRowset::OpenRowset 或 IMultipleResults::GetResult 之前设置该位时，必须将 riid 参数设置为 IID_IDBAsynchStatus、IID_ISSAsynchStatus 或 IID_IUnknown      。  
  
 该方法立即返回值，且 ppRowset 设置为行集上的请求接口。如果行集初始化立即完成，则返回 S_OK；如果行集继续异步初始化，则返回 DB_S_ASYNCHRONOUS  。 对于 OLE DB Driver for SQL Server，此接口只能是 IDBAsynchStatus 或 ISSAsynchStatus   。 在行集完全初始化之前，该接口的行为如同处于挂起状态一样，而且对 IID_IDBAsynchStatus 或 IID_ISSAsynchStatus 以外的接口调用 QueryInterface 可能返回 E_NOINTERFACE    。 除非使用者显式请求异步处理，否则行集将以同步方式执行初始化。 如果 IDBAsynchStaus::GetStatus 或 ISSAsynchStatus::WaitForAsynchCompletion 返回的内容指示异步操作已完成，则请求的所有接口均可用   。 这不一定意味着行集已完全填充，但它是完整的，且功能齐全。  
  
 即使执行的命令未返回行集，它仍会立即返回支持 IDBAsynchStatus 的对象  。  
  
 如果需要获得由异步命令执行所产生的多个结果，则应当：  
  
-   在执行命令之前，设置 DBPROP_ROWSET_ASYNCH 属性的 DBPROPVAL_ASYNCH_INITIALIZE 位。  
  
-   调用 ICommand::Execute，并请求 IMultipleResults   。  
  
 然后，可通过 QueryInterface 查询多个结果接口，从而获得 IDBAsynchStatus 和 ISSAsynchStatus 接口    。  
  
 命令执行完成后，IMultipleResults 可以正常使用，但在同步情况中有一个例外  ：可能会返回 DB_S_ASYNCHRONOUS，在这种情况下，可以使用 IDBAsynchStatus 或 ISSAsynchStatus 来确定何时完成操作   。  
  
## <a name="examples"></a>示例  
 在以下示例中，应用程序调用非阻止方法，执行一些其他处理，然后返回以处理结果。 ISSAsynchStatus::WaitForAsynchCompletion 等待内部事件对象，直到异步执行操作完成，或超过了 dwMilisecTimeOut 指定的时间   。  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
  
DBPROPSET CmdPropset[1];  
DBPROP CmdProperties[1];  
  
CmdPropset[0].rgProperties = CmdProperties;  
CmdPropset[0].cProperties = 1;  
CmdPropset[0].guidPropertySet = DBPROPSET_ROWSET;  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
CmdProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
  
hr = pICommandProps->SetProperties(1, CmdPropset);  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if ( hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 ISSAsynchStatus::WaitForAsynchCompletion 等待内部事件对象，直到异步执行操作完成，或传递了 dwMilisecTimeOut 值   。  
  
 以下示例显示有多个结果集的异步处理：  
  
```  
DBPROP CmdProperties[1];  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_IMultipleResults,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pIMultipleResults);  
  
// Use GetResults for ISSAsynchStatus.  
hr = pIMultipleResults->GetResult(IID_ISSAsynchStatus, (void **) &pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if (hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 若要阻止阻塞，客户端可以检查异步操作的运行状态，如以下示例所示：  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);   
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   do{  
      // Do some work...  
      hr = pISSAsynchStatus->GetStatus(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN, NULL, NULL, &ulAsynchPhase, NULL);  
   }while (DBASYNCHPHASE_COMPLETE != ulAsynchPhase)  
   if SUCCEEDED(hr)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
   }  
   pIDBAsynchStatus->Release();  
}  
```  
  
 以下示例演示如何才能取消当前正在运行的异步操作：  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work...  
   hr = pISSAsynchStatus->Abort(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN);  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [行集属性和行为](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
