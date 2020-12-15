---
description: 'ISSAbort：： Abort (Native Client OLE DB 提供程序) '
title: ISSAbort：： Abort (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSAbort::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
ms.assetid: a5bca169-694b-4895-84ac-e8fba491e479
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 276378972110515ef1ab397a9a494c50d5532970
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469408"
---
# <a name="issabortabort-native-client-ole-db-provider"></a>ISSAbort：： Abort (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  取消当前行集以及与当前命令关联的任何批处理命令。  
  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序中公开的 ISSAbort 接口提供 **ISSAbort：： Abort** 方法，该方法用于取消当前行集以及使用最初生成了行集的命令进行批处理，并且尚未完成执行的任何命令。  
  
 **ISSAbort** 是一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 种特定于 Native Client 提供程序的接口，可通过对 **ICommand：： Execute** 或 **IOpenRowset：： OpenRowset** 返回的 **IMultipleResults** 对象使用 **QueryInterface** 。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT Abort(void);  
```  
  
## <a name="remarks"></a>备注  
 如果要中止的命令位于存储过程中，则存储过程的执行 (，并且已调用该过程) 的所有过程都将终止，并且包含存储过程调用的命令批处理。 如果服务器正在将结果集传输到客户端，则将停止此过程。 如果客户端不希望使用结果集，则在释放行集之前调用 ISSAbort::Abort 将加快行集释放，但是，如果存在打开的事务且 XACT_ABORT 为 ON，则当调用 ISSAbort::Abort 时，将回滚此事务    
  
 在 ISSAbort::Abort 返回 S_OK 后，关联的 IMultipleResults 接口将进入不可用状态并对于所有方法调用返回 DB_E_CANCELED（由 IUnknown 接口定义的方法除外），直到释放它为止    。 如果在调用 Abort 之前已从 IMultipleResults 获取了 IRowset，则它也会进入不可用状态，并对所有方法调用返回 DB_E_CANCELED（由 IUnknown 接口和 IRowset::ReleaseRows 定义的方法除外），直到在成功调用 ISSAbort::Abort 之后释放它为止       。  
  
> [!NOTE]  
>  从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，如果服务器 XACT_ABORT 状态为 ON，则当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，ISSAbort::Abort 的执行将终止并回滚当前所有的隐式或显式事务。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的较早版本不中止当前事务。  
  
## <a name="arguments"></a>参数  
 无。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 如果取消批处理，则 ISSAbort::Abort 方法返回 S_OK，否则返回 DB_E_CANTCANCEL  。 如果已经取消了批处理，则返回 DB_E_CANCELED。  
  
 DB_E_CANCELED  
 已经取消批处理。  
  
 DB_E_CANTCANCEL  
 批处理未取消。  
  
 E_FAIL  
 出现访问接口特定的错误；若要获取详细信息，请使用 [ISQLServerErrorInfo](isqlservererrorinfo-geterrorinfo-ole-db.md) 接口。  
  
 E_UNEXPECTED  
 意外调用了该方法。 例如，因为已调用 ISSAbort::Abort，所以对象处于僵停状态  。  
  
 E_OUTOFMEMORY  
 内存不足错误。  
  
## <a name="see-also"></a>另请参阅  
 [ISSAbort &#40;OLE DB&#41;]()  
  
