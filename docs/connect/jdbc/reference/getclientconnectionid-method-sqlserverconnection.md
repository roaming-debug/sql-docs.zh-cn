---
description: getClientConnectionID 方法 (SQLServerConnection)
title: getClientConnectionID 方法 (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: bee39c11-733a-461f-92cc-33efcb2af87d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d216f6a4a5cbe24b2b73f7669d54b38faf639053
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176140"
---
# <a name="getclientconnectionid-method-sqlserverconnection"></a>getClientConnectionID 方法 (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  无论最新连接尝试成功还是失败，都获取该尝试的连接 ID。  
  
## <a name="syntax"></a>语法  
  
``` 
public Java.util.UUID SQLServerConnection.getClientConnectionID();  
```  
  
## <a name="return-value"></a>返回值  
 表示最新连接尝试的连接 ID 的 16 字节的 GUID。 或者为 NULL（如果在启动连接请求和预登录握手后失败）。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 若要详细了解如何访问扩展事件日志中的诊断信息，请参阅[访问扩展事件日志中的诊断信息](../../../connect/jdbc/accessing-diagnostic-information-in-the-extended-events-log.md)。  
  
 以下示例显示如何获取连接 ID：  
  
```  
Connection con = DriverManager.getConnection(connectionUrl);  
UUID id = ((ISQLServerConnection)con).getClientConnectionId();  
```  
  
 以下示例显示获取连接 ID 的另一种方法：  
  
```  
SQLServerConnectionPoolDataSource ds = new SQLServerConnectionPoolDataSource();  
ds.setUser("...");  
ds.setPassword("...");  
ds.setServerName("...");  
PooledConnection pcon= ds.getPooledConnection();  
Connection cn = pcon.getConnection();  
UUID conid = ((ISQLServerConnection)cn).getClientConnectionId();  
```  
  
 无论连接到什么版本的服务器，getClientConnectionID 都有效，但是在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2008 R2 及更早版本中将不会提供扩展事件日志和连接环形缓冲区上的条目。  
  
 如果启用记录连接 ID 的扩展事件，您可以在扩展事件日志中查找连接 ID，以查看失败是否源自服务器。 还可以针对某些连接错误在连接环形缓冲区（[在 SQL Server 2008 中使用连接环形缓冲区解决连接问题](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)）中查找连接 ID。 如果在连接环形缓冲区中找不到连接 ID，可以认为是网络错误。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
