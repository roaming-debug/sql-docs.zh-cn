---
description: setBigDecimal 方法 (SQLServerCallableStatement)
title: setBigDecimal 方法 (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setBigDecimal
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b50a920c-3839-40f0-9411-c60bbc2a9f34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 600a9d354f3c14dc4b2240639ff4a136df2949be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173935"
---
# <a name="setbigdecimal-method-sqlservercallablestatement"></a>setBigDecimal 方法 (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数编号设置为给定 BigDecimal 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setBigDecimal(java.lang.String sCol,  
                          java.math.BigDecimal bd)  
```  
  
#### <a name="parameters"></a>参数  
 sCol  
  
 包含参数名称的字符串。  
  
 *bd*  
  
 BigDecimal 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setBigDecimal 方法是由 java.sql.CallableStatement 接口中的 setBigDecimal 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
