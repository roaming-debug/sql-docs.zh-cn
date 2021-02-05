---
title: setAutoCommit 方法 (SQLServerConnection)
description: 详细了解 JDBC Driver for SQL Server 的 SQLServerConnection 类中 setAutoCommit 方法的公共 API。
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setAutoCommit
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: db1e22d2-e53f-474e-8c99-cb1fff7f491a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2560883b009aa1184d001851f0fc09a4fa575ac4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173946"
---
# <a name="setautocommit-method-sqlserverconnection"></a>setAutoCommit 方法 (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将此 [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) 对象的自动提交模式设置为给定状态。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setAutoCommit(boolean value)  
```  
  
#### <a name="parameters"></a>参数  
 *value*  
  
 true 表示启用连接的自动提交模式，false 表示禁用该模式。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setAutoCommit 方法是由 java.sql.Connection 接口中的 setAutoCommit 方法指定的。  
  
 如果连接处于自动提交模式下，则其所有 SQL 语句将作为单个事务运行并提交。 否则，其 SQL 语句将不断归入事务组，直到调用 [commit](../../../connect/jdbc/reference/commit-method-sqlserverconnection.md) 方法或 [rollback](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md) 方法为止。 默认情况下，新连接处于自动提交模式。  
  
 语句执行完毕或下次运行开始时（以二者中先发生的为准）都会执行提交操作。 当语句返回 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象时，该语句在检索完结果集中的最后一行或结果集关闭后即告完成。 对于更复杂的情况，单个语句可能会在输出参数值之外返回多个结果。 在这些情况下，检索完所有结果和输出参数值后将执行提交操作。  
  
 如果自动提交模式设置为 false，则 JDBC 驱动程序将在每次提交后隐式启动新事务。  
  
> [!NOTE]  
> 如果在事务处理过程中调用此方法，将提交此事务。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)  
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
