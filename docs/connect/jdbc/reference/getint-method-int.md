---
description: getInt 方法 (int)
title: getInt 方法 (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getInt (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c86792bb-096e-4c58-8b9e-74491ccf83a6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 46601652edeebb0f956b491ea4cef0ecc2637d56
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162951"
---
# <a name="getint-method-int"></a>getInt 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定的参数索引，检索指定参数的值作为 Java 编程语言中的一个 int。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getInt(int index)  
```  
  
#### <a name="parameters"></a>参数  
 *index*  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 int 值。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getInt 方法是由 java.sql.CallableStatement 接口中的 getInt 方法指定的。  
  
 只有可以安全返回整数值的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型（例如 int、smallint、tinyint 和 bit）才支持此方法。 在任何其他数据类型上使用此方法会引发异常。  
  
## <a name="see-also"></a>另请参阅  
 [getInt 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getint-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
