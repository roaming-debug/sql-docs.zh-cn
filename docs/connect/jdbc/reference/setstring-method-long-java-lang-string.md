---
description: setString 方法 (long, java.lang.String)
title: setString 方法 (long, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.setString (long, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1b2190e9-5ace-497a-8554-0e913ea9b0cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e2580de02d1cc0ba91bb36428d3309192f465c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173036"
---
# <a name="setstring-method-long-javalangstring"></a>setString 方法 (long, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将给定的 String 写入 CLOB（从给定位置开始）。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int setString(long pos,  
                     java.lang.String s)  
```  
  
#### <a name="parameters"></a>参数  
 pos  
  
 开始写入 CLOB 的位置。  
  
 *s*  
  
 要写入 CLOB 的 String。  
  
## <a name="return-value"></a>返回值  
 写入的字符数。  
  
## <a name="exceptions"></a>例外  
 java.sql.SQLException  
  
## <a name="remarks"></a>注解  
 此 setString 方法是由 java.sql.Clob 接口中的 setString 方法指定的。  
  
 从指定位置开始覆盖字符数据，并可以超过 CLOB 的初始长度。 指定“位置+1”值将追加到字符串末尾。 指定“位置+2”或更大值（或零或更小值）会引发位置错误。  
  
## <a name="see-also"></a>另请参阅  
 [setString 方法 (SQLServerClob)](../../../connect/jdbc/reference/setstring-method-sqlserverclob.md)   
 [SQLServerClob 方法](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [SQLServerClob 成员](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [SQLServerClob 类](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
