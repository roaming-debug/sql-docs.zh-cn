---
description: 'IBCPSession：： BCPColumns (Native Client OLE DB 提供程序) '
title: IBCPSession：： BCPColumns (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPColumns (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPColumns method
ms.assetid: c338abe8-9e30-4853-a7c6-b1a6c00095e1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0e912bf8e1a7ef3d9bf620799f73ec120eca51bb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476078"
---
# <a name="ibcpsessionbcpcolumns-native-client-ole-db-provider"></a>IBCPSession：： BCPColumns (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  设置绑定到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中列的字段数。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT BCPColumns(   
      DBCOUNTITEM nColumns);  
```  
  
## <a name="remarks"></a>备注  
 在内部，它调用 [IBCPSession::BCPColFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) 以便为字段数据设置默认值。 这些默认值从当通过 [IBCPSession::BCPInit](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) 指定表名称时提供程序内部检索的 SQL Server 列信息中获取。  
  
> [!NOTE]  
>  只有在已用某一有效的文件名调用 BCPInit 后，才能调用此方法  。  
  
 只有在您要使用不同于默认设置的用户文件格式时，才应调用此方法。 有关默认用户文件格式的说明的详细信息，请参阅 BCPInit 方法  。  
  
 在调用 BCPColumns 方法后，必须为用户文件中的每一列都调用 BCPColFmt 方法，以便完全定义某一自定义文件格式   。  
  
## <a name="arguments"></a>参数  
 nColumns[in]   
 用户文件中字段的总数。 即使准备将数据从用户文件大容量复制到某一 SQL Server 表，并且不想复制用户文件中的所有字段，仍必须将 nColumns 参数设置为用户字段的总数  。 然后，可通过 BCPColFmt 指定跳过的字段  。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功。  
  
 E_FAIL  
 出现访问接口特定的错误；若要获取详细信息，请使用 [ISQLServerErrorInfo](isqlservererrorinfo-geterrorinfo-ole-db.md) 接口。  
  
 E_UNEXPECTED  
 意外调用了该方法。 例如，在调用该方法之前，未调用 BCPInit 方法  。 在为某一大容量复制操作多次调用此方法时，也会发生这一意外调用。  
  
 E_OUTOFMEMORY  
 内存不足错误。  
  
## <a name="see-also"></a>另请参阅  
 [IBCPSession &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [执行大容量复制操作](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  
