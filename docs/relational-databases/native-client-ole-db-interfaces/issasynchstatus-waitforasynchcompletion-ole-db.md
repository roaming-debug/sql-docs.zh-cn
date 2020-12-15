---
description: 'ISSAsynchStatus：： WaitForAsynchCompletion SQL Server Native Client (OLE DB) '
title: ISSAsynchStatus：： WaitForAsynchCompletion (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSAsynchStatus::WaitForAsynchCompletion (OLE DB)
apitype: COM
helpviewer_keywords:
- WaitForAsynchCompletion method
ms.assetid: 9f65e9e7-eb93-47a1-bc42-acd4649fbd0e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8496a19a7ce6d532fbcf740368bbe35571789d0e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469308"
---
# <a name="issasynchstatuswaitforasynchcompletion-in-sql-server-native-client-ole-db"></a>ISSAsynchStatus：： WaitForAsynchCompletion SQL Server Native Client (OLE DB) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  一直等待，直到异步执行的操作完成或发生超时。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT WaitForAsynchCompletion(   
        DWORD dwMillisecTimeOut);  
```  
  
## <a name="arguments"></a>参数  
 dwMillisecTimeOut[in]   
 超时值（毫秒）。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功。  
  
 E_UNEXPECTED  
 由于已调用 **ITransaction：： Commit** 或 **ITransaction：： Abort** 或在其初始化阶段取消了行集，因此行集处于未使用状态。  
  
 DB_E_CANCELED  
 异步处理在行集填充或数据源对象初始化过程中被取消。  
  
 DB_S_ASYNCHRONOUS  
 此操作尚未完成，即使已达到指定的超时值。  
  
> [!NOTE]  
>  除了上面列出的返回代码值之外，ISSAsynchStatus::WaitForAsynchCompletion 方法还支持由核心 OLEDB ICommand::Execute 和 IDBInitialize::Initialize 方法返回的返回代码值    。  
  
## <a name="remarks"></a>备注  
 在经过超时值（毫秒）或完成挂起操作之前，ISSAsynchStatus::WaitForAsynchCompletion 方法将不会返回值  。 Command 对象具有 CommandTimeout 属性，该属性控制查询在超时之前将运行的秒数   。如果将 CommandTimeout 属性与 ISSAsynchStatus::WaitForAsynchCompletion 方法结合使用，则将忽略该属性   。  
  
 对于异步操作，将忽略超时属性。 ISSAsynchStatus::WaitForAsynchCompletion 的超时参数指定在将控制权返回到调用方之前将经过的最大时间量  。 如果此超时值已到期，将返回 DB_S_ASYNCHRONOUS。 超时从不会取消异步操作。 如果应用程序需要取消在超时期限内未完成的异步操作，则它必须等待发生超时，然后，如果返回 DB_S_ASYNCHRONOUS，则显式取消此操作。  
  
> [!NOTE]  
>  当使用 OLE DB 服务组件时，在应返回 DB_S_ASYNCHRONOUS 时可能返回 S_OK，因此，应用程序应调用 [ISSAsynchStatus::GetStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) 以检查在返回 S_OK 或 DB_S_ASYNCHRONOUS 时操作是否已完成。  
  
 如果 dwMillisecTimeOut 值设置为 INFINITE，则 ISSAsynchStatus::WaitForAsynchCompletion 方法将在该操作完成之前一直阻止其他操作   。 如果 dwMillisecTimeOut 值设置为 0，则该方法将立即返回并提供挂起操作的状态  。 如果在完成此操作之前超时值已到期，则将返回 DB_S_ASYNCHRONOUS。  
  
 如果操作在超时值到期之前完成，则返回的 HRESULT 将为由此操作返回的 HRESULT（如果此操作已以异步方式执行，则应已返回 HRESULT）。  
  
 此外，SSPROP_ISSAsynchStatus 属性已添加到 DBPROPSET_SQLSERVERROWSET 属性集。 支持 [ISSAsynchStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md) 接口的提供程序必须使用值 VARIANT_TRUE 实现此属性。  
  
## <a name="see-also"></a>另请参阅  
 [执行异步操作](../../relational-databases/native-client/features/performing-asynchronous-operations.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
