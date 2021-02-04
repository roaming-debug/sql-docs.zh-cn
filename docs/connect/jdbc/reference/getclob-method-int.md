---
description: getClob 方法 (int)
title: getClob 方法 (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getClob (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 34858e03-5b93-40b1-bf21-13ad7cc7a55e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d73a1ddd65f00b9280a02691513d8f7675b60a51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163330"
---
# <a name="getclob-method-int"></a>getClob 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定参数索引，检索指定 JDBC BLOB 参数作为 Java 编程语言中的 Clob 对象的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.Clob getClob(int index)  
```  
  
#### <a name="parameters"></a>参数  
 *index*  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 Clob 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getClob 方法是由 java.sql.CallableStatement 接口中的 getClob 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [getClob 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getclob-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
