---
title: ISSAbort::Abort（OLE DB 驱动程序）| Microsoft Docs
description: 了解 ISSAbort::Abort 方法如何在 OLE DB Driver for SQL Server 中取消当前行集以及与当前命令相关联的任何批处理命令。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAbort::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a53a8e8257f95c34ea89f69813e1899ccb74846f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183751"
---
# <a name="issabortabort-ole-db"></a>ISSAbort::Abort (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  取消当前行集以及与当前命令关联的任何批处理命令。  
  
`ISSAbort` 接口是在 OLE DB Driver for SQL Server 中公开的，它提供 `ISSAbort::Abort` 方法，该方法用于取消当前行集以及与最初生成该行集的命令和尚未完成执行的命令一起成批处理的任何命令。  
  
 `ISSAbort` 是特定于 OLE DB Driver for SQL Server 接口，可通过在 `ICommand::Execute` 或 `IOpenRowset::OpenRowset` 返回的 `IMultipleResults` 对象上使用 `QueryInterface` 提供。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT Abort(void);  
```  
  
## <a name="remarks"></a>备注  
 如果要中止的命令位于存储过程中，则将终止存储过程（以及已调用该过程的任何过程）以及包含此存储过程调用的命令批处理的执行。 如果服务器正在将结果集传输到客户端，则将停止此传输。 如果客户端不想使用结果集，则调用正在调用的 ISSAbort::Abort`**`` before releasing the rowset will speed up the rowset release, but if there is an open transaction and XACT_ABORT is ON, the transaction will be rolled back when `ISSAbort::Abort`  
  
 在 `ISSAbort::Abort` 返回 S_OK 后，关联的 `IMultipleResults` 接口将进入不可用状态并对于所有方法调用返回 DB_E_CANCELED（由 `IUnknown` 接口定义的方法除外），直到释放它为止。 如果在调用 `Abort` 之前已从 `IMultipleResults` 获取了 `IRowset`，则它也会进入不可用状态，并对所有方法调用返回 DB_E_CANCELED（由 `IUnknown` 接口和 `IRowset::ReleaseRows` 定义的方法除外），直到在成功调用 `ISSAbort::Abort` 之后释放它为止。  
  
> [!NOTE]  
>  从 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 开始，如果服务器 XACT_ABORT 状态为 ON，则当连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时，`ISSAbort::Abort` 的执行将终止并回滚当前所有的隐式或显式事务。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的较早版本不中止当前事务。  
  
## <a name="arguments"></a>参数  
 无。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 如果取消批处理，则 `ISSAbort::Abort` 方法返回 S_OK，否则返回 DB_E_CANTCANCEL。 如果已经取消了批处理，则返回 DB_E_CANCELED。  
  
 DB_E_CANCELED  
 已经取消批处理。  
  
 DB_E_CANTCANCEL  
 批处理未取消。  
  
 E_FAIL  
 出现访问接口特定的错误；若要获取详细信息，请使用 [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md) 接口。  
  
 E_UNEXPECTED  
 意外调用了该方法。 例如，因为已调用 `ISSAbort::Abort`，所以对象处于僵停状态。  
  
 E_OUTOFMEMORY  
 内存不足错误。  
  
