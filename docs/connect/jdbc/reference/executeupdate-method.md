---
description: executeUpdate 方法 ()
title: executeUpdate 方法 () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.executeUpdate ()
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ca534c6b-ef4d-4ae8-8cc3-514728623cff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a15178a58be70d09a00bc0311f257de7a26c4c6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163375"
---
# <a name="executeupdate-method-"></a>executeUpdate 方法 ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  运行此 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) 对象中的 SQL 语句，这些语句必须是 SQL INSERT、UPDATE、MERGE 或 DELETE 语句；或是不返回任何内容的 SQL 语句，如 DDL 语句。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int executeUpdate()  
```  
  
## <a name="return-value"></a>返回值  
 一个指示受影响的行数的 int，如果使用 DDL 语句，则为 0。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 executeUpdate 方法是由 java.sql.PreparedStatement 接口中的 executeUpdate 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [executeUpdate 方法 &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [SQLServerPreparedStatement 类](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
