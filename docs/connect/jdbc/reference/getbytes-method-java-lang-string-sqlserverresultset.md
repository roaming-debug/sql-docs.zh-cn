---
description: getBytes 方法 (java.lang.String) (SQLServerResultSet)
title: getBytes 方法 (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBytes (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ff617165-47f8-41c1-9c51-37ffc7714923
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2b19cb872d5a455fe55983630fe9bdba882efc00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168194"
---
# <a name="getbytes-method-javalangstring-sqlserverresultset"></a>getBytes 方法 (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列名称的值作为 Java 编程语言中的字节数组。  
  
## <a name="syntax"></a>语法  
  
```  
  
public byte[] getBytes(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>参数  
 *columnName*  
  
 一个包含列名的字符串  。  
  
## <a name="return-value"></a>返回值  
 byte 值的数组。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getBytes 方法是由 java.sql.ResultSet 接口中的 getBytes 方法指定的。  
  
 此方法支持从服务器将所有列作为原始读取的字节检索。 它采用在服务器上存储的格式直接从服务器返回一个字节数组。  
  
 在 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 的之前版本中，可以使用 SQLServerResultSet.getBytes 将字节数组和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型 date、time、datetime2 或 datetimeoffset 的值相互转换。 现在对于这些数据类型使用此方法将导致异常，指出不支持该转换。  
  
## <a name="see-also"></a>另请参阅  
 [getBytes 方法 &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getbytes-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
