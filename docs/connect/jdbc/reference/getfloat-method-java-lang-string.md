---
description: getFloat 方法 (java.lang.String)
title: getFloat 方法 (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getFloat (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b6492341-fdc2-449c-9d03-95a5dadf1bb0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d33f288609d0e1062e5fa9e00f77d4bacd610b77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163016"
---
# <a name="getfloat-method-javalangstring"></a>getFloat 方法 (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定的参数名称，检索指定参数作为 Java 编程语言中的 float 的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public float getFloat(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>参数  
 sCol  
  
 包含参数名称的字符串。  
  
## <a name="return-value"></a>返回值  
 float 值。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getFloat 方法是由 java.sql.CallableStatement 接口中的 getFloat 方法指定的。  
  
 此方法使用 Java float 精度返回基于数字的所有类型。  
  
## <a name="see-also"></a>另请参阅  
 [getFloat 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getfloat-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
