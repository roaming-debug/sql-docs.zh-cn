---
description: setCharacterStream 方法 (java.lang.String, java.io.Reader, long)
title: setCharacterStream 方法 (java.lang.String, java.io.Reader, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 54fb2f13-f8d8-47b5-bec1-4a5af3e86a84
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 29922cb50d3c76b8b2306be3ae9601caf5b97b7a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173685"
---
# <a name="setcharacterstream-method-javalangstring-javaioreader-long"></a>setCharacterStream 方法 (java.lang.String, java.io.Reader, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为具有指定字符数长度的指定 java.io.Reader 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setCharacterStream(java.lang.String parameterName  
                        java.io.Reader reader,  
                        long length)  
```  
  
#### <a name="parameters"></a>参数  
 parameterName  
  
 包含参数名称的字符串。  
  
 reader  
  
 包含 Unicode 数据的 Reader 对象。  
  
 *length*  
  
 指示流中字符数的 long。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setCharacterStream 方法是由 java.sql.CallableStatement 接口中的 setCharacterStream 方法指定的。  
  
 如果流长度与 length 参数指定的长度不同，则 JDBC 驱动程序将在更新或插入行时引发异常。  
  
 如果流长度未知，则可将 length 参数设置为 -1 以指示驱动程序应接受流而不考虑其长度。 使用 sqljdbc4.jar，当应用程序希望使用长度未知的流来更新列时，我们建议使用 JDBC 4.0 方法 [setCharacterStream 方法 (java.lang.String, java.io.Reader)](../../../connect/jdbc/reference/setcharacterstream-method-java-lang-string-java-io-reader.md)。  
  
## <a name="see-also"></a>另请参阅  
 [setCharacterStream 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setcharacterstream-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
