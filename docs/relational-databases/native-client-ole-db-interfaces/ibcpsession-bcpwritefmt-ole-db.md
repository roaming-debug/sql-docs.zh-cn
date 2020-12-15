---
description: 'IBCPSession：： BCPWriteFmt (Native Client OLE DB 提供程序) '
title: IBCPSession：： BCPWriteFmt (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPWriteFmt (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPWriteFmt method
ms.assetid: add50425-2ed6-411a-a391-4ce63c364892
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d6f69d1d82101d96725e79f65fcc6667986ecb81
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484949"
---
# <a name="ibcpsessionbcpwritefmt-native-client-ole-db-provider"></a>IBCPSession：： BCPWriteFmt (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将每一列的格式信息写入格式化文件。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT BCPWriteFmt(   
      const wchar_t *pwszFormatFile);  
```  
  
## <a name="remarks"></a>备注  
 格式化文件指定大容量复制所创建的数据文件的数据格式。 调用 [IBCPSession::BCPColumns](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) 和 [IBCPSession::BCPColFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) 方法将定义数据文件的格式。 BCPWriteFmt 方法将此定义保存在 pwszFormatFile 参数引用的文件中  。  
  
 BCPWriteFmt 方法可以通过 xml 或文本格式保存格式化文件  。 这必须通过将 BCP_OPTION_XML 控制选项用于 [IBCPSession::BCPControl](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md) 方法来指示。  
  
 若要加载已保存的格式化文件，请使用 [IBCPSession::BCPReadFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md) 方法。  
  
## <a name="arguments"></a>参数  
 pwszFormatFile  [in]  
 包含数据文件格式值的文件的路径和文件名。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功。  
  
 E_FAIL  
 出现访问接口特定的错误；若要获取详细信息，请使用 [ISQLServerErrorInfo](isqlservererrorinfo-geterrorinfo-ole-db.md) 接口。  
  
 E_OUTOFMEMORY  
 内存不足错误。  
  
 E_UNEXPECTED  
 意外调用了该方法。 例如，在调用该方法之前，未调用 [IBCPSession::BCPInit](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) 方法。  
  
## <a name="see-also"></a>另请参阅  
 [IBCPSession &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [执行大容量复制操作](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  
