---
title: ISSAsynchStatus::WaitForAsynchCompletion（OLE DB 驱动程序）| Microsoft Docs
description: 了解 ISSAsynchStatus::WaitForAsynchCompletion 方法如何一直等待，直到异步操作完成或在 OLE DB Driver for SQL Server 中超时。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAsynchStatus::WaitForAsynchCompletion (OLE DB)
apitype: COM
helpviewer_keywords:
- WaitForAsynchCompletion method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 438c1f78240a641c0b860384db1f20841758a084
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183706"
---
# <a name="issasynchstatuswaitforasynchcompletion-ole-db"></a>ISSAsynchStatus::WaitForAsynchCompletion (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

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
 因为已调用 ITransaction::Commit 或 ITransaction::Abort 或在行集初始化阶段取消了行集，因此行集处于未使用状态   。  
  
 DB_E_CANCELED  
 异步处理在行集填充或数据源对象初始化过程中已取消。  
  
 DB_S_ASYNCHRONOUS  
 此操作尚未完成，即使已达到指定的超时值。  
  
> [!NOTE]  
>  除了上面列出的返回代码值之外，ISSAsynchStatus::WaitForAsynchCompletion 方法还支持由核心 OLEDB ICommand::Execute 和 IDBInitialize::Initialize 方法返回的返回代码值    。  
  
## <a name="remarks"></a>备注  
 在经过超时值（毫秒）或完成挂起操作之前，ISSAsynchStatus::WaitForAsynchCompletion 方法将不会返回值  。 Command 对象具有 CommandTimeout 属性，该属性控制查询在超时之前将运行的秒数   。如果将 CommandTimeout 属性与 ISSAsynchStatus::WaitForAsynchCompletion 方法结合使用，则将忽略该属性   。  
  
 对于异步操作，将忽略超时属性。 ISSAsynchStatus::WaitForAsynchCompletion 的超时参数指定在将控制权返回到调用方之前将经过的最大时间量  。 如果此超时值已到期，将返回 DB_S_ASYNCHRONOUS。 超时从不会取消异步操作。 如果应用程序需要取消在超时期限内未完成的异步操作，则它必须等待发生超时，然后，如果返回 DB_S_ASYNCHRONOUS，则显式取消此操作。  
  
> [!NOTE]  
>  当使用 OLE DB 服务组件时，在应返回 DB_S_ASYNCHRONOUS 时可能返回 S_OK，因此，应用程序应调用 [ISSAsynchStatus::GetStatus](../../oledb/ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) 以检查在返回 S_OK 或 DB_S_ASYNCHRONOUS 时操作是否已完成。  
  
 如果 dwMillisecTimeOut 值设置为 INFINITE，则 ISSAsynchStatus::WaitForAsynchCompletion 方法将在该操作完成之前一直阻止其他操作   。 如果 dwMillisecTimeOut 值设置为 0，则该方法将立即返回并提供挂起操作的状态  。 如果在完成此操作之前超时值已到期，则将返回 DB_S_ASYNCHRONOUS。  
  
 如果操作在超时值到期之前完成，则返回的 HRESULT 将为由此操作返回的 HRESULT（如果此操作已以异步方式执行，则应已返回 HRESULT）。  
  
 此外，SSPROP_ISSAsynchStatus 属性已添加到 DBPROPSET_SQLSERVERROWSET 属性集。 支持 [ISSAsynchStatus](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md) 接口的提供程序必须使用值 VARIANT_TRUE 实现此属性。  
  
## <a name="see-also"></a>另请参阅  
 [执行异步操作](../../oledb/features/performing-asynchronous-operations.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
