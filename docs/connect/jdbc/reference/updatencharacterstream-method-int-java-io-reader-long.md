---
description: updateNCharacterStream 方法 (int, java.io.Reader, long)
title: updateNCharacterStream 方法 (int, java.io.Reader, long) | Microsoft 文档
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: aeec0a56-038e-45b1-98c8-b1046ebd25db
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d0b4158340c3d7977ee5e1834cd9fff780a17ac6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176552"
---
# <a name="updatencharacterstream-method-int-javaioreader-long"></a>updateNCharacterStream 方法 (int, java.io.Reader, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  使用将具有指定字节数的字符流值更新指定列。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void updateNCharacterStream(int columnIndex,  
                                    java.io.Reader x,  
                                    long length)  
```  
  
#### <a name="parameters"></a>parameters  
 columnIndex   
  
 指示列索引的 int  。  
  
 *x*  
  
 Reader 对象。  
  
 *length*  
  
 流的长度。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 updateNCharacterStream 方法是由 java.sql.ResultSet 接口中的 updateNCharacterStream 方法指定的。  
  
 此方法将来自 Reader 对象的 Unicode 字符传递给所选 nchar、nvarchar(max)、ntext 和 xml 列。 在其他数据类型列上使用此方法会引发异常。  
  
 如果流长度与 length 参数指定的长度不同，则 JDBC 驱动程序将在更新或插入行时引发异常。  
  
 如果流长度未知，则可将 length 参数设置为 -1 以指示驱动程序应接受流而不考虑其长度。 使用 sqljdbc4.jar，当应用程序希望使用长度未知的流来更新列时，建议使用 JDBC 4.0 方法 [updateNCharacterStream 方法 &#40;int, java.io.Reader&#41;](../../../connect/jdbc/reference/updatencharacterstream-method-int-java-io-reader.md)。  
  
## <a name="see-also"></a>另请参阅  
 [updateNCharacterStream 方法 (SQLServerResultSet)](../../../connect/jdbc/reference/updatencharacterstream-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
