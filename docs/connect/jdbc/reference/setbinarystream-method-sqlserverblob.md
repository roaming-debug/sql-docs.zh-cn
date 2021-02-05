---
description: setBinaryStream 方法 (SQLServerBlob)
title: setBinaryStream 方法 (SQLServerBlob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.setBinaryStream
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: abcec31f-1a60-4765-9725-8cf7e9f1f8ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0f72c246530b4f2298af9bf846bad15fd8380d5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173853"
---
# <a name="setbinarystream-method-sqlserverblob"></a>setBinaryStream 方法 (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索可用于写入 BLOB 值的流。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.io.OutputStream setBinaryStream(long pos)  
```  
  
#### <a name="parameters"></a>参数  
 Pos  
  
 BLOB 值中开始写入的位置。  
  
## <a name="return-value"></a>返回值  
 输出流。  
  
## <a name="exceptions"></a>例外  
 java.sql.SQLException  
  
## <a name="remarks"></a>注解  
 此 setBinaryStream 方法是由 java.sql.Blob 接口中的 setBinaryStream 方法指定的。  
  
 输出流从指定位置开始覆盖 BLOB 中的数据，并可以超过 BLOB 的初始长度。 指定“位置+1”值将追加字节。 传递“位置+2”或更大值（或零或更小值）会引发位置错误。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerBlob 方法](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob 成员](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob 类](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
