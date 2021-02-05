---
description: getBytes 方法 (SQLServerBlob)
title: getBytes 方法 (SQLServerBlob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.getBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bea1b810-b5c1-466d-bdc4-561468214632
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 32fd57b6be09a09036d3bd29c5e22ee9934e7ab8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168106"
---
# <a name="getbytes-method-sqlserverblob"></a>getBytes 方法 (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  获取 BLOB 数据作为字节数组。  
  
## <a name="syntax"></a>语法  
  
```  
  
public byte[] getBytes(long pos,  
                       int length)  
```  
  
#### <a name="parameters"></a>参数  
 pos  
  
 开始位置，从 1（而不是从 0）开始。  
  
 *length*  
  
 要获取的数据的长度。  
  
## <a name="return-value"></a>返回值  
 包含请求数据的 byte 数组。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getBytes 方法是由 java.sql.Blob 接口中的 getBytes 方法指定的。  
  
 如果具有长度为 Null 或为零的 BLOB，并试图在位置 1 处获取恰好零字节，此时将返回空的 byte 数组（长度为 0 的字节数组）。  
  
 如果具有长度为 Null 或为零的 BLOB，并试图从位置 1 之外的其他位置获取任意长度的字节，此时将引发位置异常。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerBlob 方法](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob 成员](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob 类](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
