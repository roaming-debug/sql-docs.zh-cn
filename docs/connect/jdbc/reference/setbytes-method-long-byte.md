---
description: setBytes 方法 (long, byte)
title: setBytes 方法 (long, byte) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.setBytes (long.byte[])
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ffb8f107-0f9d-4410-957f-62b718e1e872
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2329bb9598c56ecabb7282faaf6b7b7f5565b068
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173766"
---
# <a name="setbytes-method-long-byte"></a>setBytes 方法 (long, byte)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将给定的字节数组写入 BLOB（从给定的位置开始），然后返回写入的字节数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int setBytes(long pos,  
                    byte[] bytes)  
```  
  
#### <a name="parameters"></a>参数  
 pos  
  
 BLOB 中开始写入数据的位置（从 1 开始）。  
  
 *bytes*  
  
 要写入 BLOB 的字节的数组。  
  
## <a name="return-value"></a>返回值  
 指定写入的字节数的 long 值。  
  
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
  
  
