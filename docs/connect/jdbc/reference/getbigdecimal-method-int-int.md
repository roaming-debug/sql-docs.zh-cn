---
description: getBigDecimal 方法 (int, int)
title: getBigDecimal 方法 (int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getBigDecimal (int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d9351b35-7046-4852-a612-72d4c46b2bbb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f9074eab0a822ba16a32f6e4c11d298fe5a304bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165771"
---
# <a name="getbigdecimal-method-int-int"></a>getBigDecimal 方法 (int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定的参数索引和小数位数，检索指定参数的值作为 java.math.BigDecimal。  
  
> [!NOTE]  
>  JDBC 规范已不推荐使用此方法， 应改用 [getBigDecimal (int)](../../../connect/jdbc/reference/getbigdecimal-method-int.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.math.BigDecimal getBigDecimal(int index,  
                                          int scale)  
```  
  
#### <a name="parameters"></a>参数  
 *index*  
  
 指示参数索引的 int。  
  
 *scale*  
  
 指示小数点右边的位数的 int。  
  
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
  
  
