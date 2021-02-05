---
description: registerOutParameter 方法 (java.lang.String, int, int)
title: 注册类型和小数位数的 registerOutParameter 方法 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.registerOutParameter (java.lang.String, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8bddc557-4526-4843-9804-05dc83c8832d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a6db3c986aa771c4d54b393d71d173f7c6fe3e6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176688"
---
# <a name="registeroutparameter-method-javalangstring-int-int"></a>registerOutParameter 方法 (java.lang.String, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将具有指定名称的 OUT 参数注册为给定的 JDBC 类型和小数位数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void registerOutParameter(java.lang.String s,  
                                 int n,  
                                 int n1)  
```  
  
#### <a name="parameters"></a>参数  
 *s*  
  
 包含参数名称的字符串。  
  
 *sqlType*  
  
 在 java.sql.Types 中定义的 JDBC 类型代码。  
  
 *scale*  
  
 一个 int 值，此值指示要放在小数点右边的位数。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 registerOutParameter 方法是由 java.sql.CallableStatement 接口中的 registerOutParameter 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [registerOutParameter 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
