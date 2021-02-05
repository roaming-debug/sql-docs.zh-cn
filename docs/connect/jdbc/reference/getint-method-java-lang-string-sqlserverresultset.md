---
description: getInt 方法 (java.lang.String) (SQLServerResultSet)
title: getInt 方法 (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getInt (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 76b7054d-46dd-4d87-93a4-a7ea2ae9b7fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8232952eff28063078e14dc68e7238fc9c69765b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175688"
---
# <a name="getint-method-javalangstring-sqlserverresultset"></a>getInt 方法 (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列名的值作为 Java 编程语言中的 int。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getInt(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>参数  
 *columnName*  
  
 一个包含列名的字符串  。  
  
## <a name="return-value"></a>返回值  
 int 值。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getInt 方法是由 java.sql.ResultSet 接口中的 getInt 方法指定的。  
  
 只有可以安全返回整数值的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型（例如 int、smallint、tinyint 和 bit）才支持此方法。 在任何其他数据类型上使用此方法会引发异常。  
  
## <a name="see-also"></a>另请参阅  
 [getInt 方法 (SQLServerResultSet)](../../../connect/jdbc/reference/getint-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
