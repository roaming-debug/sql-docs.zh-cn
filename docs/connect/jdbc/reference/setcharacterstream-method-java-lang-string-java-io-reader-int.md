---
description: setCharacterStream 方法 (java.lang.String, java.io.Reader, int)
title: setCharacterStream 方法 (java.lang.String, java.io.Reader, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setCharacterStream
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 88a8e89e-8817-4161-85b1-9a9a2fd01cdb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d13459bed46cfc34ca053947f3f3772bc7241847
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173698"
---
# <a name="setcharacterstream-method-javalangstring-javaioreader-int"></a>setCharacterStream 方法 (java.lang.String, java.io.Reader, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为具有指定字符数的指定的 Reader 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setCharacterStream(java.lang.String parameterName,  
                              java.io.Reader value,  
                              int length)  
```  
  
#### <a name="parameters"></a>参数  
 parameterName  
  
 一个字符串，该字符串包含参数的名称。  
  
 *value*  
  
 包含 Unicode 数据的 Reader 对象。  
  
 *length*  
  
 以字符数指示长度的 int。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setCharacterStream 方法是由 java.sql.CallableStatement 接口中的 setCharacterStream 方法指定的。  
  
 如果流长度与 length 参数指定的长度不同，则 JDBC 驱动程序将在更新或插入行时引发异常。  
  
 如果流长度未知，则可将 length 参数设置为 -1 以指示驱动程序应接受流而不考虑其长度。 使用 sqljdbc4.jar，当应用程序希望使用长度未知的流来更新列时，我们建议使用 JDBC 4.0 方法 [setCharacterStream 方法 (java.lang.String, java.io.Reader)](../../../connect/jdbc/reference/setcharacterstream-method-java-lang-string-java-io-reader.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
