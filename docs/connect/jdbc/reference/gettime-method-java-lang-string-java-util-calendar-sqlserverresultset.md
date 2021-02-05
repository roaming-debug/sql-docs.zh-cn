---
description: getTime 方法 (java.lang.String, java.util.Calendar) (SQLServerResultSet)
title: getTime 方法 (java.lang.String, java.util.Calendar) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getTime (java.lang.String, java.util.Calendar)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 13b51f77-cec9-45fc-862e-3d2bb2d718d7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d6d8d74f1f19c1294d0719e7438994aea1455990
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174867"
---
# <a name="gettime-method-javalangstring-javautilcalendar-sqlserverresultset"></a>getTime 方法 (java.lang.String, java.util.Calendar) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  通过使用给定的 Calendar 对象，检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列名称的值作为 Java 编程语言中的 java.sql.Time 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.Time getTime(java.lang.String colName,  
                             java.util.Calendar cal)  
```  
  
#### <a name="parameters"></a>parameters  
 *colName*  
  
 一个包含列名的字符串  。  
  
 cal   
  
 Calendar 对象。  
  
## <a name="return-value"></a>返回值  
 Time 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getTime 方法是由 java.sql.ResultSet 接口中的 getTime 方法指定的。  
  
 此方法返回 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] datetime 或 smalldatetime 数据类型的有效时间部分，日期部分设置为采用所提供的 Calendar 时区的 Java 基线日期 1970/01/01。  
  
## <a name="see-also"></a>另请参阅  
 [getTime 方法 &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/gettime-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
