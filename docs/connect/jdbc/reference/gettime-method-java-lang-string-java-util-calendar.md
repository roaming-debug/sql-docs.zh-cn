---
description: getTime 方法 (java.lang.String, java.util.Calendar)
title: getTime 方法 (java.lang.String, java.util.Calendar) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getTime (java.lang.String, java.util.Calendar)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3d4c67c2-a3c8-4a26-a159-89c5d63fda0b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c546939efd6b6c4dd228019e0c03d5e34d8e87c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162141"
---
# <a name="gettime-method-javalangstring-javautilcalendar"></a>getTime 方法 (java.lang.String, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定的参数名称，通过使用给定的 Calendar 对象检索指定参数作为 Java 编程语言中的 java.sql.Time 对象的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.Time getTime(java.lang.String sCol,  
                             java.util.Calendar cal)  
```  
  
#### <a name="parameters"></a>参数  
 sCol  
  
 包含参数名称的字符串。  
  
 cal   
  
 Calendar 对象。  
  
## <a name="return-value"></a>返回值  
 Time 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getTime 方法是由 java.sql.CallableStatement 接口中的 getTime 方法指定的。  
  
 请参阅[了解数据类型转换](../../../connect/jdbc/understanding-data-type-conversions.md)中标题为“Getter 方法转换”的图表，了解使用此方法可检索哪些 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。  
  
## <a name="see-also"></a>另请参阅  
 [getTime 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/gettime-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
