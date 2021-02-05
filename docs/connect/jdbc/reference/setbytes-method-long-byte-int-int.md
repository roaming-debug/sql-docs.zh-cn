---
description: setBytes 方法 (long, byte, int, int)
title: setBytes 方法 (long, byte, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.setBytes (long.byte[], int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7def226c-b211-459e-8c1a-08592d75d4a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7210c363f8fefca7022fa018378f5d1bceeaa375
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173774"
---
# <a name="setbytes-method-long-byte-int-int"></a>setBytes 方法 (long, byte, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  从给定的位置开始根据偏移量和长度，将给定字节数组的全部或部分写入 BLOB，然后返回写入的字节数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int setBytes(long pos,  
                    byte[] bytes,  
                    int offset,  
                    int len)  
```  
  
#### <a name="parameters"></a>参数  
 pos  
  
 BLOB 中开始写入数据的位置（从 1 开始）。  
  
 *bytes*  
  
 要写入 BLOB 的字节的数组。  
  
 *offset*  
  
 字节数组中要从 byte 数组开始读取数据的位置的偏移量。  
  
 len  
  
 要尝试从字节数组读入 BLOB 的字节数。  
  
## <a name="return-value"></a>返回值  
 包含写入的字节数的 int。  
  
## <a name="exceptions"></a>例外  
 java.sql.SQLException  
  
## <a name="remarks"></a>注解  
 此 setBytes 方法是由 java.sql.Blob 接口中的 setBytes 方法指定的。  
  
 从指定位置开始覆盖数据，并可以超过 BLOB 的初始长度。 指定“位置+1”值将追加字节。 传递“位置+2”或更大值（或零或更小值）会引发位置错误。 传递长度为零的 byte 数组会因未写入任何字节而返回零。  
  
## <a name="see-also"></a>另请参阅  
 [setBytes 方法 (SQLServerBlob)](../../../connect/jdbc/reference/setbytes-method-sqlserverblob.md)   
 [SQLServerBlob 方法](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob 成员](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob 类](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
