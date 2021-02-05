---
description: getCharacterStream 方法 (int)
title: getCharacterStream 方法 (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getCharacterStream (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4f9f230d-be4c-469a-b3dc-f24531429aae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4616b4e984cf77f999265839e39b63edeb8189f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168125"
---
# <a name="getcharacterstream-method-int"></a>getCharacterStream 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列索引的值作为 java.io.Reader 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.io.Reader getCharacterStream(int columnIndex)  
```  
  
#### <a name="parameters"></a>parameters  
 columnIndex   
  
 指示列索引的 int  。  
  
## <a name="return-value"></a>返回值  
 Reader 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getCharacterStream 方法是由 java.sql.ResultSet 接口中的 getCharacterStream 方法指定的。  
  
 此方法将只读取 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Unicode 字符数据类型，例如 nchar、nvarchar、nvarchar(max) 和 ntext。 任何其他数据类型（包括 ASCII 字符类型）会引发异常。 若要读取 ASCII 数据类型，请使用 [getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlserverresultset.md) 方法。  
  
## <a name="see-also"></a>另请参阅  
 [getCharacterStream 方法 &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
