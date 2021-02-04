---
description: getBinaryStream 方法 (java.lang.String)
title: getBinaryStream 方法 (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBinaryStream (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 149609b5-a6de-4e23-a440-7061775d0899
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d9457c8042d41b3044e3ceaeb25ffe2bc5d36a97
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165727"
---
# <a name="getbinarystream-method-javalangstring"></a>getBinaryStream 方法 (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列名称的值作为未解释字节的二进制流。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.io.InputStream getBinaryStream(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>参数  
 *columnName*  
  
 一个包含列名的字符串  。  
  
## <a name="return-value"></a>返回值  
 InputStream 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getBinaryStream 方法是由 java.sql.ResultSet 接口中的 getBinaryStream 方法指定的。  
  
 此方法只能用于 binary、varbinary、varbinary(max) 和 image 的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。 尝试将它用于其他数据类型会引发异常。  
  
 此方法获取作为流的值后，可以以块区的形式从流中读取该值。 此方法特别适合检索大型 LONGVARBINARY 值。  
  
> [!NOTE]  
>  必须在获取任何其他列的值前读取返回的流中的所有数据。 对 getter 方法的下一次调用隐式关闭该流。 此外，在调用方法 InputStream.available 时，无论数据是否可用，流都可以返回 0。  
  
## <a name="see-also"></a>另请参阅  
 [getBinaryStream 方法 &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getbinarystream-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
