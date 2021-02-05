---
description: getPooledConnection 方法 (java.lang.String, java.lang.String)
title: getPooledConnection 方法 (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnectionPoolDataSource.getPooledConnection (java.lang.String, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f2e6391d-9aaf-4b09-ae1c-a27c1ada6301
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5da68c20ed56b97db07b0e5d97a9f6e654655ad8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175257"
---
# <a name="getpooledconnection-method-javalangstring-javalangstring"></a>getPooledConnection 方法 (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  基于给定的用户名和密码尝试建立可用作池连接的物理数据库连接。  
  
## <a name="syntax"></a>语法  
  
```  
  
public javax.sql.PooledConnection getPooledConnection(java.lang.String user,  
                                                      java.lang.String password)  
```  
  
#### <a name="parameters"></a>参数  
 *user*  
  
 一个包含用户名的字符串。  
  
 passwword  
  
 一个包含密码的字符串。  
  
## <a name="return-value"></a>返回值  
 [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md) 对象。  
  
## <a name="exceptions"></a>例外  
 java.sql.SQLException  
  
## <a name="remarks"></a>注解  
 此 getPooledConnection 方法是由 javax.sql.ConnectionPoolDataSource 接口中的 getPooledConnection 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)   
 [SQLServerConnectionPoolDataSource 方法](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-methods.md)   
 [SQLServerConnectionPoolDataSource 成员](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-members.md)   
 [SQLServerConnectionPoolDataSource 类](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
  
