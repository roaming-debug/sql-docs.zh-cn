---
description: getBinaryStream 方法 (long, long)
title: getBinaryStream 方法 (long, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 30bc8882-04b4-4efd-95e4-7d3a2a8c1d47
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: da3fcf0ab4e97c19458981feea8f7e2af4c5a133
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168231"
---
# <a name="getbinarystream-method-long-long"></a>getBinaryStream 方法 (long, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  通过使用指定的起始位置和长度来返回包含部分 BLOB 值的输入流对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.io.InputStream getBinaryStream(long pos, long length)  
```  
  
#### <a name="parameters"></a>参数  
 pos  
  
 要检索的部分值的第一个字节的偏移量。  
  
 *length*  
  
 要检索的部分值的字节长度。  
  
## <a name="return-value"></a>返回值  
 包含 BLOB 数据的输入流。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getBinaryStream 方法是由 java.sql.Blob 接口中的 getBinaryStream 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerBlob 方法](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob 成员](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob 类](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
