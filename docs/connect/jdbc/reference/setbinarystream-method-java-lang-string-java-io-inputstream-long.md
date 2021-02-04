---
description: setBinaryStream 方法 (java.lang.String, java.io.InputStream, long)
title: 设置输入流的 setBinaryStream 方法 - long | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: d59c7327-c9dc-4e4f-9dff-19e1a3c62eb2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8669f362907fcfd9c83bd53e4f0873f3fe84a690
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173870"
---
# <a name="setbinarystream-method-javalangstring-javaioinputstream-long"></a>setBinaryStream 方法 (java.lang.String, java.io.InputStream, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为指定的输入流，而指定输入流将含有指定字节数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setBinaryStream(java.lang.String parameterName,  
                            java.io.InputStream x,  
                            long length)  
```  
  
#### <a name="parameters"></a>参数  
 parameterName  
  
 一个字符串，该字符串包含参数的名称。  
  
 *x*  
  
 InputStream 对象。  
  
 *length*  
  
 以字节数指示长度的 long。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setBinaryStream 方法是由 java.sql.CallableStatement 接口中的 setBinaryStream 方法指定的。  
  
 如果流长度与 length 参数指定的长度不同，则 JDBC 驱动程序将在更新或插入行时引发异常。  
  
 如果流长度未知，则可将 length 参数设置为 -1 以指示驱动程序应接受流而不考虑其长度。 使用 sqljdbc4.jar，当应用程序希望使用长度未知的流来更新列时，我们建议使用 JDBC 4.0 方法 [setBinaryStream 方法 (java.lang.String, java.io.InputStream)](../../../connect/jdbc/reference/setbinarystream-method-java-lang-string-java-io-inputstream.md)。  
  
## <a name="see-also"></a>另请参阅  
 [setBinaryStream (SQLServerCallableStatement)](../../../connect/jdbc/reference/setbinarystream-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
