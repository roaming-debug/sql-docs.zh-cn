---
description: getBlob 方法 (int)
title: getBlob 方法 (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getBlob (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bef3ef12-cdda-4a18-90d6-4a501b8e30f0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 54e9c032fa43e2f0ea18494a3f39afedb60fc86b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168163"
---
# <a name="getblob-method-int"></a>getBlob 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定的参数索引，检索指定 JDBC BLOB 参数作为 Java 编程语言中的 BLOB 对象的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.Blob getBlob(int index)  
```  
  
#### <a name="parameters"></a>参数  
 *index*  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 Blob 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getBlob 方法是由 java.sql.CallableStatement 接口中的 getBlob 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [getBlob 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getblob-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
