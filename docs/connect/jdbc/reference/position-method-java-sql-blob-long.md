---
description: position 方法 (java.sql.Blob, long)
title: position 方法 (java.sql.Blob, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.position (java.sql.Blob.long)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ebd005e5-f6c5-4789-87f9-d2fdacd35060
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 247daf9f63d8f8967e6a2bc2d468100964b99b2c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176900"
---
# <a name="position-method-javasqlblob-long"></a>position 方法 (java.sql.Blob, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定的模式和起始索引返回 BLOB 中指定模式的位置。  
  
## <a name="syntax"></a>语法  
  
```  
  
public long position(java.sql.Blob pattern,  
                     long start)  
```  
  
#### <a name="parameters"></a>参数  
 *pattern*  
  
 要搜索的模式。  
  
 *start*  
  
 要搜索的起始索引。  
  
## <a name="return-value"></a>返回值  
 指示找到模式的位置的 long 值，如果未找到，则为 -1。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 position 方法是由 java.sql.Blob 接口中的 position 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [position 方法 (SQLServerBlob)](../../../connect/jdbc/reference/position-method-sqlserverblob.md)   
 [SQLServerBlob 方法](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob 成员](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob 类](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
