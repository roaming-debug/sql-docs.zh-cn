---
description: getConcurrency 方法 (SQLServerResultSet)
title: getConcurrency 方法 (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getConcurrency
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 207e25f4-769c-4ff3-913c-3517b06208e4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d8e3bc102bb7b169a6cf8029ba49a56ee084dd2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176034"
---
# <a name="getconcurrency-method-sqlserverresultset"></a>getConcurrency 方法 (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的并发模式。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getConcurrency()  
```  
  
## <a name="return-value"></a>返回值  
 指明并发类型的 int 值，可取值如下：  
  
 ResultSet.CONCUR_READ_ONLY  
  
 ResultSet.CONCUR_UPDATABLE  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getConcurrency 方法是由 java.sql.ResultSet 接口中的 getConcurrency 方法指定的。  
  
 使用的并发类型是由创建结果集的 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象决定的。  
  
 此方法可以用于确定实际并发类型。 如果应用程序选择了 CONCUR_READ_ONLY 或 CONCUR_UPDATABLE，将返回这些并发类型。 如果应用程序使用了默认并发类型，将返回 CONCUR_READ_ONLY。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
