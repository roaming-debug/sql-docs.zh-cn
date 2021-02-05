---
description: SQLServerDataSource 成员
title: SQLServerDataSource 成员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 7e749bc5-d765-4864-be2b-7822d4c20c09
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2e16d07888c7a86ecd1b3296ee234794ae7aac00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178317"
---
# <a name="sqlserverdatasource-members"></a>SQLServerDataSource 成员
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  下表列出了由 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 类公开的成员。  
  
## <a name="constructors"></a>构造函数  
  
|名称|说明|  
|----------|-----------------|  
|[SQLServerDataSource ()](../../../connect/jdbc/reference/sqlserverdatasource-constructor.md)|初始化 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 类的新实例。|  
  
## <a name="fields"></a>字段  
 无。  
  
## <a name="inherited-fields"></a>继承的字段  
 无。  
  
## <a name="methods"></a>方法  
  
|名称|说明|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|返回 applicationIntent  连接属性的值。|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|返回应用程序名称。|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|尝试与此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象表示的数据源建立连接。|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|返回数据库名称。|  
|[getDisableStatementPooling](../../../connect/jdbc/reference/getdisablestatementpooling-method-sqlserverdatasource.md)|返回 disableStatementPooling  连接属性的值。 此设置控制是否为此连接启用语句池。|  
|[getEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/getenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource.md)|返回 enablePrepareOnFirstPreparedStatementCall  连接属性的值。|  
|[getEncrypt](../../../connect/jdbc/reference/getencrypt-method-sqlserverdatasource.md)|返回一个布尔值，此值指示是否启用了 encrypt 属性  。|  
|[getDescription](../../../connect/jdbc/reference/getdescription-method-sqlserverdatasource.md)|返回数据源的说明。|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|返回在数据库镜像配置中使用的故障转移服务器名称。|  
|[getHostNameInCertificate](../../../connect/jdbc/reference/gethostnameincertificate-method-sqlserverdatasource.md)|返回用于验证 SQL Server 传输层安全性 (TLS)（以前称为安全套接字层 (SSL)）证书的主机名。|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|返回 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例名。|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|返回一个布尔值，此值指示是否启用了 lastUpdateCount 属性  。|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|返回一个 int 值，此值指示数据库在报告锁定超时之前要等待的毫秒数  。|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|返回此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象在尝试进行连接时要等待的秒数。|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|返回用于所有日志记录和跟踪消息的字符输出流。|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|返回 multiSubnetFailover  连接属性的值。|  
|[getPacketSize](../../../connect/jdbc/reference/getpacketsize-method-sqlserverdatasource.md)|返回用于与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通信的当前网络数据包大小，以字节为单位指定。|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|返回用于与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通信的当前端口号。|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverdatasource.md)|返回对此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象的引用。|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverdatasource.md)|返回此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象的响应缓冲模式。|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|返回用于通过使用此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象创建的所有结果集的默认游标类型。|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|返回一个布尔值，此值指示是否启用了以 UNICODE 格式将字符串参数发送到服务器  。|  
|[getSendTimeAsDatetime](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md)|返回 SendTimeAsDatetime  连接属性的设置。|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|返回正在运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的计算机的名称。|  
|[getServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/getserverpreparedstatementdiscardthreshold-method-sqlserverdatasource.md)|返回 serverPreparedStatementDiscardThreshold  连接属性的值。|  
|[getStatementPoolingCacheSize](../../../connect/jdbc/reference/getstatementpoolingcachesize-method-sqlserverdatasource.md)|返回此连接的准备的语句缓存的大小。|  
|[getTrustManagerClass](../../../connect/jdbc/reference/gettrustmanagerclass-method-sqlserverdatasource.md)|返回 TrustManagerClass 连接属性的 String 值。|  
|[getTrustManagerConstructorArg](../../../connect/jdbc/reference/gettrustmanagerconstructorarg-method-sqlserverdatasource.md)|返回 TrustManagerConstructorArg 连接属性的 String 值。|  
|[getTrustServerCertificate](../../../connect/jdbc/reference/gettrustservercertificate-method-sqlserverdatasource.md)|返回一个布尔值，此值指示是否启用了 trustServerCertificate 属性  。|  
|[getTrustStore](../../../connect/jdbc/reference/gettruststore-method-sqlserverdatasource.md)|返回指向证书 trustStore 文件的路径（包括文件名）。|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|返回用于连接到数据源的 URL。|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|返回用于连接数据源的用户名。|  
|[getUseSQLServerBaseDate](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md)|返回 useSQLServerBaseDate 连接属性的设置。|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|返回用于连接数据源的客户端计算机名。|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|返回一个布尔值，此值指示是否启用了将 SQL 状态转换为兼容 XOPEN 的状态  。|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverdatasource.md)|指示此数据源对象是否为指定接口的包装。|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|设置 applicationIntent  连接属性的值。|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|设置应用程序名称。|  
|[setAuthenticationScheme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|指示您的应用程序要使用的集成安全性类型。|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|设置要连接到的数据库名称。|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|设置数据源的说明。|  
|[setDisableStatementPooling](../../../connect/jdbc/reference/setdisablestatementpooling-method-sqlserverdatasource.md)|将语句池设置为 true 或 false。|  
|[setEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/setenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource.md)|指定 enablePrepareOnFirstPreparedStatementCall  连接属性的新值。|  
|[setEncrypt](../../../connect/jdbc/reference/setencrypt-method-sqlserverdatasource.md)|设置一个布尔值，此值指示是否启用了 encrypt 属性  。|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|设置在数据库镜像配置中使用的故障转移服务器名称。|  
|[setHostNameInCertificate](../../../connect/jdbc/reference/sethostnameincertificate-method-sqlserverdatasource.md)|设置用于验证 SQL Server 传输层安全性 (TLS)（以前称为安全套接字层 (SSL)）证书的主机名。|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|设置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例名称。|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|设置一个布尔值，此值指示是否启用了 integratedSecurity 属性  。|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|设置一个布尔值，此值指示是否启用了 lastUpdateCount 属性  。|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|设置一个 int 值，此值指示在数据库报告锁定超时之前要等待的毫秒数  。|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|设置此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象在尝试进行连接时要等待的秒数。|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|设置用于所有日志记录和跟踪消息的字符输出流。|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|设置 multiSubnetFailover  连接属性的值。|  
|[setPacketSize](../../../connect/jdbc/reference/setpacketsize-method-sqlserverdatasource.md)|设置用于与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通信的当前网络数据包大小，以字节为单位指定。|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|设置用于连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的密码。|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|设置用于与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通信的端口号。|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverdatasource.md)|设置通过使用此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象创建的连接的响应缓冲模式。|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|设置用于通过使用此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象创建的所有结果集的默认游标类型。|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|设置一个布尔值，此值指示是否启用了以 UNICODE 格式将字符串参数发送到服务器  。|  
|[setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md)|指定如何将 java.sql.Time 值发送到服务器。|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|设置正在运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的计算机的名称。|  
|[setServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/setserverpreparedstatementdiscardthreshold-method-sqlserverdatasource.md)|设置 serverPreparedStatementDiscardThreshold  连接属性的新值。|  
|[setStatementPoolingCacheSize](../../../connect/jdbc/reference/setstatementpoolingcachesize-method-sqlserverdatasource.md)|设置此连接的准备的语句缓存的大小。|  
|[setTrustManagerClass](../../../connect/jdbc/reference/settrustmanagerclass-method-sqlserverdatasource.md)|设置 TrustManagerClass 连接属性的 String 值。|  
|[setTrustManagerConstructorArg](../../../connect/jdbc/reference/settrustmanagerconstructorarg-method-sqlserverdatasource.md)|设置 TrustManagerConstructorArg 连接属性的 String 值。|  
|[setTrustServerCertificate](../../../connect/jdbc/reference/settrustservercertificate-method-sqlserverdatasource.md)|设置一个布尔值，此值指示是否启用了 trustServerCertificate 属性  。|  
|[setTrustStore](../../../connect/jdbc/reference/settruststore-method-sqlserverdatasource.md)|设置指向证书 trustStore 文件的路径（包括文件名）。|  
|[setTrustStorePassword](../../../connect/jdbc/reference/settruststorepassword-method-sqlserverdatasource.md)|设置用于检查 trustStore 数据完整性的密码。|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|设置用于连接数据源的 URL。|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|设置用于连接数据源的用户名。|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|设置用于连接数据源的客户端计算机名。|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|设置一个布尔值，此值指示是否启用了将 SQL 状态转换为兼容 XOPEN 的状态  。|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverdatasource.md)|返回一个实现指定接口的对象，从而允许访问特定于 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 的方法。|  
  
## <a name="inherited-methods"></a>继承的方法  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|java.lang.Object|clone、equals、finalize、getClass、hashCode、notify、notifyAll、toString、wait|  
|java.sql.Wrapper|isWrapperFor、unwrap|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
