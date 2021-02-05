---
description: updateSQLXML 方法 (int, java.sql.SQLXML)
title: updateSQLXML 方法 (int, java.sql.SQLXML) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b5170751-fbe1-433b-96f5-4f237ba55f60
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4f91be12042616311ec1da91bc410989b0cf82b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193060"
---
# <a name="updatesqlxml-method-int-javasqlsqlxml"></a>updateSQLXML 方法 (int, java.sql.SQLXML)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  使用 java.sql.SQLXML 值更新指定列  
  
## <a name="syntax"></a>语法  
  
```  
  
public void updateSQLXML(int columnIndex,  
                         java.sql.SQLXML xmlObject)  
```  
  
#### <a name="parameters"></a>parameters  
 columnIndex   
  
 指示列索引的 int  。  
  
 xmlObject  
  
 SQLXML 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 updateSQLXML 方法是由 java.sql.ResultSet 接口中的 updateSQLXML 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [updateSQLXML Method (SQLServerResultSet)](../../../connect/jdbc/reference/updatesqlxml-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
