---
description: executeUpdate 方法 (java.lang.String) (SQLServerStatement)
title: executeUpdate 方法 (java.lang.String) (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.executeUpdate (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 85e7c3a2-f2da-49bf-9d90-5fd246fd60e1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8d518ab7d043a5b54cf363a4be96523fed473a79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168506"
---
# <a name="executeupdate-method-javalangstring-sqlserverstatement"></a>executeUpdate 方法 (java.lang.String) (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  运行给定的 SQL 语句，可以是 INSERT、UPDATE 或 DELETE 语句；或不返回任何内容的 SQL 语句，例如 SQL DDL 语句。 从 [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0 开始，executeUpdate 将返回在 MERGE 操作中更新的正确行数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int executeUpdate(java.lang.String sql)  
```  
  
#### <a name="parameters"></a>参数  
 *sql*  
  
 包含 SQL 语句的 String。  
  
## <a name="return-value"></a>返回值  
 一个指示受影响的行数的 int，如果使用 DDL 语句，则为 0。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 executeUpdate 方法是由 java.sql.Statement 接口中的 executeUpdate 方法指定的。  
  
 如果执行存储过程将产生大于 1 的更新计数，或生成多个结果集，则请使用 [execute](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md) 方法执行存储过程。  
  
## <a name="see-also"></a>另请参阅  
 [executeUpdate 方法 (SQLServerStatement)](../../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md)   
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
