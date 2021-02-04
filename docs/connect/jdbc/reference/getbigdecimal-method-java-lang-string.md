---
description: getBigDecimal 方法 (java.lang.String)
title: getBigDecimal 方法 (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getBigDecimal (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d0b29b48-9663-4de4-9fc2-82bc30e44aed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8a5b274d9d4c5248908fbd25d29d61ff4167cc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165733"
---
# <a name="getbigdecimal-method-javalangstring"></a>getBigDecimal 方法 (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定的参数名称，检索指定参数的值作为全精度的 java.math.BigDecimal。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.math.BigDecimal getBigDecimal(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>参数  
 sCol  
  
 包含参数名称的字符串。  
  
## <a name="return-value"></a>返回值  
 BigDecimal 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getBigDecimal 方法是由 java.sql.CallableStatement 接口中的 getBigDecimal 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [getBigDecimal 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getbigdecimal-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
