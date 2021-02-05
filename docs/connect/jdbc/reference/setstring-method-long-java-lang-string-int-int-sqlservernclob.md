---
description: setString 方法 (long, java.lang.String, int, int) (SQLServerNClob)
title: setString 方法 (long, java.lang.String, int, int) - NClob | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2d5e9f50-15b2-4c76-8bfc-3b5be49c2781
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0ac85bfb0742c298c215d95d0092dfe032a26139
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178596"
---
# <a name="setstring-method-long-javalangstring-int-int-sqlservernclob"></a>setString 方法 (long, java.lang.String, int, int) (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  基于指定的偏移量和长度，将指定的字符串写入到始于指定位置的 NCLOB。  
  
## <a name="syntax"></a>语法  
  
```  
  
int setString(long pos,  
              java.lang.String str,  
              int offset,  
              int len)  
```  
  
#### <a name="parameters"></a>参数  
 pos  
  
 写入 NCLOB 的起始位置；第一个位置为 1。  
  
 *str*  
  
 要写入 NCLOB 的 String。  
  
 *offset*  
  
 *str* 中的偏移量，从这个位置开始读取将要写入的字符。  
  
 len  
  
 将要写入的字符数。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setString 方法是由 java.sql.NClob 接口中的 setString 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerNClob 方法](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [SQLServerNClob 成员](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [SQLServerNClob 类](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
