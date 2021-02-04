---
description: SQLServerXADataSource 成员
title: SQLServerXADataSource 成员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 04178645-915f-4569-8907-d45e299bbe7d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e93f7fe568f66686ff6d71b569c040cdea45cad2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181179"
---
# <a name="sqlserverxadatasource-members"></a>SQLServerXADataSource 成员
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  下表列出了由 [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md) 类公开的成员。  
  
## <a name="constructors"></a>构造函数  
  
|名称|说明|  
|----------|-----------------|  
|[SQLServerXADataSource ()](../../../connect/jdbc/reference/sqlserverxadatasource-constructor.md)|初始化 [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md) 类的新实例。|  
  
## <a name="fields"></a>字段  
 无。  
  
## <a name="inherited-fields"></a>继承的字段  
 无。  
  
## <a name="methods"></a>方法  
  
|名称|说明|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回 applicationIntent 连接属性的值。|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回应用程序名称。|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）尝试建立与此 DataSource 对象表示的数据源之间的连接。|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回数据库名称。|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回在数据库镜像配置中使用的故障转移服务器的名称。|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例名称。|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回一个布尔值，此值指示是否启用了 lastUpdateCount 属性。|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回一个 int 值，此值指示数据库在报告锁定超时前将等待的毫秒数。|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回此 DataSource 对象在尝试连进行接时将等待的秒数。|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回用于所有日志记录和跟踪消息的字符输出流。|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）检索 multiSubnetFailover 连接属性的值。|  
|[getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)|（继承自 [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)）尝试建立可用作池连接的物理数据库连接。|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回用于与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通信的当前端口号。|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverxadatasource.md)|返回对此 [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md) 对象的引用。|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回用于通过使用此 DataSource 对象创建的所有结果集的默认游标类型。|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回一个布尔值，此值指示是否启用了以 UNICODE 格式将字符串参数发送到服务器。|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回正在运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的计算机的名称。|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回用于连接到数据源的 URL。|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回用于连接数据源的用户名。|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回用于连接到数据源的客户端计算机的名称。|  
|[getXAConnection](../../../connect/jdbc/reference/getxaconnection-method-sqlserverxadatasource.md)|尝试建立可在分布式事务中使用的物理数据库连接。|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）返回一个布尔值，此值指示是否启用了将 SQL 状态转换为兼容 XOPEN 的状态。|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverxadatasource.md)|指示此对象是否为指定接口的包装。|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置 applicationIntent 连接属性的值。|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置应用程序名称。|  
|[setAuthenticationSceme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）指示你的应用程序要使用的集成安全性的类型。|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置要连接到的数据库名称。|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置数据源的说明。|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置在数据库镜像配置中使用的故障转移服务器的名称。|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例名称。|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置一个布尔值，此值指示是否启用了 integratedSecurity 属性。|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置一个布尔值，此值指示是否启用了 lastUpdateCount 属性。|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置一个 int 值，此值指示在数据库报告锁定超时前要等待的毫秒数。|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置此 DataSource 对象在尝试进行连接时将等待的秒数。|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置用于所有日志记录和跟踪消息的字符输出流。|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置 multiSubnetFailover 连接属性的值。|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置将用于连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的密码。|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置用于与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通信的端口号。|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置用于通过使用此 DataSource 对象创建的所有结果集的默认游标类型。|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置一个布尔值，此值指示是否启用了以 UNICODE 格式将字符串参数发送到服务器 。|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置正在运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的计算机的名称。|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置用于连接到数据源的 URL。|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置用于连接数据源的用户名。|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置用于连接到数据源的客户端计算机名称。|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|（继承自 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)）设置一个布尔值，此值指示是否启用了将 SQL 状态转换为兼容 XOPEN 的状态。|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverxadatasource.md)|返回一个实现指定接口的对象，从而允许访问特定于 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 的方法。|  
  
## <a name="inherited-methods"></a>继承的方法  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource|getPooledConnection|  
|com.microsoft.sqlserver.jdbc.SQLServerDataSource|getApplicationName、getConnection、getDatabaseName、getDescription、getFailoverPartner、getInstanceName、getLastUpdateCount、getLockTimeout、getLoginTimeout、getLogWriter、getPortNumber、getSelectMethod、getSendStringParametersAsUnicode、getServerName、getURL、getUser、getWorkstationID、getXopenStates、setApplicationName、setDatabaseName、setDescription、setFailoverPartner、setInstanceName、setIntegratedSecurity、setLastUpdateCount、setLockTimeout、setLoginTimeout、setLogWriter、setPassword、setPortNumber、setSelectMethod、setSendStringParametersAsUnicode、setServerName、setURL、setUser、setWorkstationID、setXopenStates|  
|java.lang.Object|clone、equals、finalize、getClass、hashCode、notify、notifyAll、toString、wait|  
|java.sql.Wrapper|isWrapperFor、unwrap|  
|javax.sql.XADataSource|getLoginTimeout、getLogWriter、setLoginTimeout、setLogWriter|  
|javax.sql.ConnectionPoolDataSource|getLoginTimeout、getLogWriter、setLoginTimeout、setLogWriter|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerXADataSource 类](../../../connect/jdbc/reference/sqlserverxadatasource-class.md)  
  
  
