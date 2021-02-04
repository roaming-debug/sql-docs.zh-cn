---
description: getMaxFieldSize 方法 (SQLServerStatement)
title: getMaxFieldSize 方法 (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getMaxFieldSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ed7bbcb8-660b-4e9b-8241-e216c42826f9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4b1cbb5bb3cb35b4286a0b822168c9991c228576
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162810"
---
# <a name="getmaxfieldsize-method-sqlserverstatement"></a>getMaxFieldSize 方法 (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索可为由此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象生成的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的字符和二进制列值返回的最大字节数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final int getMaxFieldSize()  
```  
  
## <a name="return-value"></a>返回值  
 指示列可包含的最大字节数的 int；如果没有限制，则为 0。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getMaxFieldSize 方法是由 java.sql.Statementt 接口中的 getMaxFieldSize 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerStatement 方法](../../../connect/jdbc/reference/sqlserverstatement-methods.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
