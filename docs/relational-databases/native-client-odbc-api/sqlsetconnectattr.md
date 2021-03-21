---
title: SQLSetConnectAttr |Microsoft Docs
description: 了解 SQLSetConnectAttr 中的连接属性，包括在 SQL Server Native Client ODBC 驱动程序中设置这些属性和可能的值。
ms.custom: ''
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSetConnectAttr function
ms.assetid: d21b5cf1-3724-43f7-bc96-5097df0677b4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 30893a308be9cb7634f3c0ceb755572e0e5b274b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754827"
---
# <a name="sqlsetconnectattr"></a>SQLSetConnectAttr

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序忽略 SQL_ATTR_CONNECTION_TIMEOUT 的设置。  
  
 SQL_ATTR_TRANSLATE_LIB 也被忽略；并且指定不支持其他翻译库。 为了允许应用程序轻松地移植以便使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Microsoft ODBC 驱动程序，使用 SQL_ATTR_TRANSLATE_LIB 设置的任何值都将被复制到和复制出驱动程序管理器内的缓冲区中。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序将“可重复读”事务隔离作为可序列化实现。  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入了对新的事务隔离属性 SQL_COPT_SS_TXN_ISOLATION 的支持。 将 SQL_COPT_SS_TXN_ISOLATION 设置为 SQL_TXN_SS_SNAPSHOT 指示事务将在快照隔离级别下发生。  
  
> [!NOTE]  
> SQL_ATTR_TXN_ISOLATION 可以用于设置除 SQL_TXN_SS_SNAPSHOT 之外的所有其他隔离级别。 如果要使用快照隔离，必须通过 SQL_COPT_SS_TXN_ISOLATION 设置 SQL_TXN_SS_SNAPSHOT。 但是，可以使用 SQL_ATTR_TXN_ISOLATION 或 SQL_COPT_SS_TXN_ISOLATION 来检索该隔离级别。  
  
 将 ODBC 语句属性升级为连接属性可能导致意外结果。 可以将向服务器请求用于结果集处理的游标的语句属性升级为连接属性。 例如，将 ODBC 语句属性 SQL_ATTR_CONCURRENCY 设置为比默认 SQL_CONCUR_READ_ONLY 更受限制的值会导致驱动程序为对连接提交的所有语句使用动态游标。 对连接的语句执行 ODBC 目录函数将返回 SQL_SUCCESS_WITH_INFO 和一个诊断记录，该记录指示游标行为已更改为只读。 如果尝试对同一连接执行包含 COMPUTE 子句的 Transact-SQL SELECT 语句，将会失败。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序支持 sqlncli.h 中定义的针对 ODBC 连接属性的很多驱动程序特定扩展插件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序可能要求在连接前必须设置属性，如果属性已经设置它也可能忽略属性。 下表列出了这些限制。  
  
|SQL Server 属性|在连接到服务器之前或之后设置|  
|--------------------------|----------------------------------------------|  
|SQL_COPT_SS_ANSI_NPW|以前|  
|SQL_COPT_SS_APPLICATION_INTENT|以前|  
|SQL_COPT_SS_ATTACHDBFILENAME|以前|  
|SQL_COPT_SS_BCP|以前|  
|SQL_COPT_SS_BROWSE_CONNECT|以前|  
|SQL_COPT_SS_BROWSE_SERVER|以前|  
|SQL_COPT_SS_CONCAT_NULL|以前|  
|SQL_COPT_SS_CONNECTION_DEAD|之后|  
|SQL_COPT_SS_ENCRYPT|以前|  
|SQL_COPT_SS_ENLIST_IN_DTC|之后|  
|SQL_COPT_SS_ENLIST_IN_XA|之后|  
|SQL_COPT_SS_FALLBACK_CONNECT|以前|  
|SQL_COPT_SS_FAILOVER_PARTNER|以前|  
|SQL_COPT_SS_INTEGRATED_SECURITY|以前|  
|SQL_COPT_SS_MARS_ENABLED|以前|  
|SQL_COPT_SS_MULTISUBNET_FAILOVER|以前|  
|SQL_COPT_SS_OLDPWD|以前|  
|SQL_COPT_SS_PERF_DATA|之后|  
|SQL_COPT_SS_PERF_DATA_LOG|之后|  
|SQL_COPT_SS_PERF_DATA_LOG_NOW|之后|  
|SQL_COPT_SS_PERF_QUERY|之后|  
|SQL_COPT_SS_PERF_QUERY_INTERVAL|之后|  
|SQL_COPT_SS_PERF_QUERY_LOG|之后|  
|SQL_COPT_SS_PRESERVE_CURSORS|以前|  
|SQL_COPT_SS_QUOTED_IDENT|任一个|  
|SQL_COPT_SS_TRANSLATE|任一个|  
|SQL_COPT_SS_TRUST_SERVER_CERTIFICATE|以前|  
|SQL_COPT_SS_TXN_ISOLATION|任一个|  
|SQL_COPT_SS_USE_PROC_FOR_PREP|任一个|  
|SQL_COPT_SS_USER_DATA|任一个|  
|SQL_COPT_SS_WARN_ON_CP_ERROR|以前|  
  
 对同一个会话、数据库或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 状态使用一个预连接属性和等同的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 命令可能会产生意外行为。 例如，  
  
```  
SQLSetConnectAttr(SQL_COPT_SS_QUOTED_IDENT, SQL_QI_ON) // turn ON via attribute  
SQLDriverConnect(...);  
SQLExecDirect("SET QUOTED_IDENTIFIER OFF") // turn OFF via Transact-SQL  
SQLSetConnectAttr(SQL_ATTR_CURRENT_CATALOG, ...) // restores to pre-connect attribute value  
```  

<a name="sqlcoptssansinpw"></a>
## <a name="sql_copt_ss_ansi_npw"></a>SQL_COPT_SS_ANSI_NPW  
 SQL_COPT_SS_ANSI_NPW 在比较和连接、字符数据类型填充以及警告中允许或禁止使用 ISO 对 NULL 的处理方式。 有关详细信息，请参阅 SET ANSI_NULLS、SET ANSI_PADDING、SET ANSI_WARNINGS 和 SET CONCAT_NULL_YIELDS_NULL。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_AD_ON|默认。 连接使用 ANSI 默认行为处理 NULL 比较、填充、警告和 NULL 连接。|  
|SQL_AD_OFF|连接使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 定义的方式来处理 NULL、字符数据类型填充和警告。|  
  
 如果使用连接池，则应在连接字符串中（而不是 SQLSetConnectAttr）设置 SQL_COPT_SS_ANSI_NPW。 建立连接后，当使用连接池时，任何尝试更改此属性的操作都将失败且无提示。  

<a name="sqlcoptssapplicationintent"></a>
## <a name="sql_copt_ss_application_intent"></a>SQL_COPT_SS_APPLICATION_INTENT  
 连接到服务器时声明应用程序工作负荷类型。 可能的值为 **Readonly** 和 **ReadWrite**。 例如：  
  
```  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_APPLICATION_INTENT, TEXT("Readonly"), SQL_NTS)  
```  
  
 默认值为 ReadWrite  。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 对 ag 的支持的详细信息 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ，请参阅 [高可用性和灾难恢复的 SQL Server Native Client 支持](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。  

<a name="sqlcoptssattachdbfilename"></a>
## <a name="sql_copt_ss_attachdbfilename"></a>SQL_COPT_SS_ATTACHDBFILENAME  
 SQL_COPT_SS_ATTACHDBFILENAME 指定可附加的数据库的主文件名称。 附加此数据库并使其成为连接的默认数据库。 若要使用 SQL_COPT_SS_ATTACHDBFILENAME 必须将数据库名称指定为连接属性 SQL_ATTR_CURRENT_CATALOG 或 [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md)的 database = 参数的值。 如果数据库以前附加过，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将不会重新附加它。  
  
|值|说明|  
|-----------|-----------------|  
|指向字符串的 SQLPOINTER|该字符串包含要附加的数据库的主文件名称。 它包括该文件的完整路径名称。|  

<a name="sqlcoptssbcp"></a>
## <a name="sql_copt_ss_bcp"></a>SQL_COPT_SS_BCP  
 SQL_COPT_SS_BCP 支持针对连接的大容量复制函数。 有关详细信息，请参阅 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_BCP_OFF|默认。 无法对连接使用大容量复制函数。|  
|SQL_BCP_ON|可以对连接使用大容量复制函数。|  

<a name="sqlcoptssbrowseconnect"></a>
## <a name="sql_copt_ss_browse_connect"></a>SQL_COPT_SS_BROWSE_CONNECT  
 此属性用于自定义 [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)返回的结果集。 SQL_COPT_SS_BROWSE_CONNECT 允许或禁止从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的枚举实例返回其他信息。 这可以包含服务器是否是群集、不同实例的名称以及版本号等信息。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_MORE_INFO_NO|默认。 返回服务器列表。|  
|SQL_MORE_INFO_YES|**SQLBrowseConnect** 返回服务器属性的扩展字符串。|  

<a name="sqlcoptssbrowseserver"></a>
## <a name="sql_copt_ss_browse_server"></a>SQL_COPT_SS_BROWSE_SERVER  
 此属性用于自定义 **SQLBrowseConnect** 返回的结果集。 SQL_COPT_SS_BROWSE_SERVER 指定 **SQLBrowseConnect** 为其返回信息的服务器名称。  
  
|值|说明|  
|-----------|-----------------|  
|computername|**SQLBrowseConnect** 返回指定计算机上的实例的列表 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 双反斜杠 (\\ \\) 不应用于服务器名称 (例如，而不是 \\ \MyServer，则应) 使用 MyServer。|  
|Null|默认。 **SQLBrowseConnect** 返回域中所有服务器的信息。|  

<a name="sqlcoptssconcatnull"></a>
## <a name="sql_copt_ss_concat_null"></a>SQL_COPT_SS_CONCAT_NULL  
 在连接字符串时，SQL_COPT_SS_CONCAT_NULL 允许或禁止使用 ISO 对 NULL 的处理方式。 有关详细信息，请参阅 SET CONCAT_NULL_YIELDS_NULL。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_CN_ON|默认。 在连接字符串时，连接使用 ISO 默认行为来处理 NULL 值。|  
|SQL_CN_OFF|在连接字符串时，连接使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 定义的行为来处理 NULL 值。|  

<a name="sqlcoptssencrypt"></a>
## <a name="sql_copt_ss_encrypt"></a>SQL_COPT_SS_ENCRYPT  
 控制连接的加密。  
  
 加密使用服务器上的证书。 除非将连接属性 SQL_COPT_SS_TRUST_SERVER_CERTIFICATE 设置为 SQL_TRUST_SERVER_CERTIFICATE_YES 或连接字符串包含 "TrustServerCertificate=yes"，否则证书必须要由证书颁发机构验证。 如果满足上述条件之一且服务器上没有证书，则由服务器生成和签名的证书可以用于加密连接。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_EN_ON|将加密连接。|  
|SQL_EN_OFF|将不加密连接。 这是默认值。|  

<a name="sqlcoptssenlistindtc"></a>
## <a name="sql_copt_ss_enlist_in_dtc"></a>SQL_COPT_SS_ENLIST_IN_DTC  
 客户端调用 Microsoft 分布式事务处理协调器 (MS DTC) OLE DB **ITransactionDispenser：： BeginTransaction** 方法来开始 ms dtc 事务，并创建表示该事务的 ms dtc 事务对象。 然后，应用程序调用带有 SQL_COPT_SS_ENLIST_IN_DTC 选项的 **SQLSetConnectAttr** ，将 transaction 对象与 ODBC 连接相关联。 将在 MS DTC 事务的保护下执行所有相关的数据库活动。 应用程序调用 **SQLSetConnectAttr** 与 SQL_DTC_DONE 结束连接的 DTC 关联。  
  
|值|说明|  
|-----------|-----------------|  
|DTC 对象*|指定要导出到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的事务的 MS DTC OLE 事务对象。|  
|SQL_DTC_DONE|限定 DTC 事务的结束。|  

<a name="sqlcoptssenlistinxa"></a>
## <a name="sql_copt_ss_enlist_in_xa"></a>SQL_COPT_SS_ENLIST_IN_XA  
 若要开始使用 XA 兼容事务处理器的 XA 事务 (TP) ，客户端将调用 Open Group **tx_begin** 函数。 然后，应用程序使用 SQL_COPT_SS_ENLIST_IN_XA 参数 TRUE 调用 **SQLSetConnectAttr** ，将 XA 事务与 ODBC 连接关联。 将在 XA 事务的保护下执行所有相关的数据库活动。 若要结束 XA 与 ODBC 连接的关联，客户端必须使用 SQL_COPT_SS_ENLIST_IN_XA 参数 FALSE 调用 **SQLSetConnectAttr** 。 有关详细信息，请参阅 Microsoft 分布式事务处理协调器文档。  
  
## <a name="sql_copt_ss_fallback_connect"></a>SQL_COPT_SS_FALLBACK_CONNECT  
 不再支持该属性。  

<a name="sqlcoptssfailoverpartner"></a>
## <a name="sql_copt_ss_failover_partner"></a>SQL_COPT_SS_FAILOVER_PARTNER  
 用于指定或检索在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中进行数据库镜像的故障转移伙伴的名称，它是用 Null 值结束的字符串，必须在首次连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 前设置。  
  
 建立连接后，应用程序可以使用 [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) 查询此属性，以确定故障转移伙伴的标识。 如果主服务器没有故障转移伙伴，则此属性将返回空字符串。 这允许智能应用程序缓存最近确定的备份服务器，但是这类应用程序应注意此信息仅在首次建立连接时才更新或重置（如果缓存），对于长期连接可能过时。  
  
 有关详细信息，请参阅[使用数据库镜像](../../relational-databases/native-client/features/using-database-mirroring.md)。  

<a name="sqlcoptssintegratedsecurity"></a>
## <a name="sql_copt_ss_integrated_security"></a>SQL_COPT_SS_INTEGRATED_SECURITY  
 SQL_COPT_SS_INTEGRATED_SECURITY 强制将 Windows 身份验证用于服务器登录的访问验证。 使用 Windows 身份验证时，驱动程序将忽略作为 **SQLConnect**、 [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md)或 [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) 处理的一部分提供的用户标识符和密码值。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_IS_OFF|默认。 登录时将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证用于验证用户标识符和密码。|  
|SQL_IS_ON|使用 Windows 身份验证模式来验证用户对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的访问权限。|  

<a name="sqlcoptssmarsenabled"></a>
## <a name="sql_copt_ss_mars_enabled"></a>SQL_COPT_SS_MARS_ENABLED  
 此属性启用或禁用多个活动结果集 (MARS)。 默认情况下，禁用 MARS。 应在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前设置此属性。 打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接后，MARS 在连接的生存期内将保持启用或禁用状态。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_MARS_ENABLED_NO|默认。 禁用多个活动结果集 (MARS)。|  
|SQL_MARS_ENABLED_YES|启用 MARS。|  
  
 有关 MARS 的详细信息，请参阅 [使用多个活动的结果集 &#40;MARS&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)。  

<a name="sqlcoptssmultisubnetfailover"></a>
## <a name="sql_copt_ss_multisubnet_failover"></a>SQL_COPT_SS_MULTISUBNET_FAILOVER  
 如果您的应用程序要连接到不同子网上的 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 可用性组 (AG)，则此连接属性将配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 以便更快检测和连接到（当前）活动服务器。 例如：  
  
```  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_MULTISUBNET_FAILOVER, SQL_IS_ON, SQL_IS_INTEGER)  
```  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 对 ag 的支持的详细信息 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ，请参阅 [高可用性和灾难恢复的 SQL Server Native Client 支持](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_IS_ON|如果有故障转移，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 将更快地进行重新连接。|  
|SQL_IS_OFF|如果有故障转移，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 不会更快地进行重新连接。|  

<a name="sqlcoptssoldpwd"></a>
## <a name="sql_copt_ss_oldpwd"></a>SQL_COPT_SS_OLDPWD  
 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入了 SQL Server 身份验证密码过期功能。 已新增 SQL_COPT_SS_OLDPWD 属性以允许客户端同时提供连接的旧密码和新密码。 设置此属性时，访问接口对于第一次连接或后续连接将不使用连接池，因为连接字符串将包含现在已更改的“旧密码”。  
  
 有关详细信息，请参阅[以编程方式更改密码](../../relational-databases/native-client/features/changing-passwords-programmatically.md)。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_COPT_SS_OLD_PASSWORD|指向包含旧密码的字符串的 SQLPOINTER。 此值是只写的，必须在连接到服务器之前设置。|  

<a name="sqlcoptssperfdata"></a>
## <a name="sql_copt_ss_perf_data"></a>SQL_COPT_SS_PERF_DATA  
 SQL_COPT_SS_PERF_DATA 启动或停止性能数据日志记录。 在启动数据日志记录之前必须设置数据日志文件名。 请参阅下面的 SQL_COPT_SS_PERF_DATA_LOG。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_PERF_START|启动驱动程序性能数据抽样。|  
|SQL_PERF_STOP|停止性能数据抽样的计数器。|  
  
 有关详细信息，请参阅 [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)。  

<a name="sqlcoptssperfdatalog"></a>
## <a name="sql_copt_ss_perf_data_log"></a>SQL_COPT_SS_PERF_DATA_LOG  
 SQL_COPT_SS_PERF_DATA_LOG 分配用于记录性能数据的日志文件的名称。 该日志文件名是 ANSI 或 Unicode 以 Null 值结束的字符串（取决于应用程序编译）。 应 SQL_NTS *StringLength* 参数。  

<a name="sqlcoptssperfdatalognow"></a>
## <a name="sql_copt_ss_perf_data_log_now"></a>SQL_COPT_SS_PERF_DATA_LOG_NOW  
 SQL_COPT_SS_PERF_DATA_LOG_NOW 指示驱动程序将统计信息日志条目写入磁盘。 应 SQL_NTS *StringLength* 参数。  

<a name="sqlcoptssperfquery"></a>
## <a name="sql_copt_ss_perf_query"></a>SQL_COPT_SS_PERF_QUERY  
 SQL_COPT_SS_PERF_QUERY 启动或停止对长时间运行的查询的日志记录。 在启动日志记录之前必须提供查询日志文件名。 应用程序可以通过设置日志记录的时间间隔来定义“长时间运行”。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_PERF_START|启动长时间运行的查询的日志记录。|  
|SQL_PERF_STOP|停止长时间运行的查询的日志记录。|  
  
 有关详细信息，请参阅 [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)。  

<a name="sqlcoptssperfqueryinterval"></a>
## <a name="sql_copt_ss_perf_query_interval"></a>SQL_COPT_SS_PERF_QUERY_INTERVAL  
 SQL_COPT_SS_PERF_QUERY_INTERVAL 设置以毫秒为单位的查询日志记录阈值。 将未在该阈值内解决的查询记录到长时间运行的查询日志文件。 查询阈值没有上限。 查询阈值为零将导致对所有查询进行日志记录。  

<a name="sqlcoptssperfquerylog"></a>
## <a name="sql_copt_ss_perf_query_log"></a>SQL_COPT_SS_PERF_QUERY_LOG  
 SQL_COPT_SS_PERF_QUERY_LOG 分配用于记录长时间运行的查询数据的日志文件名称。 该日志文件名是 ANSI 或 Unicode 以 Null 值结束的字符串（取决于应用程序编译）。 *StringLength* 参数应为 SQL_NTS 或字符串的长度（以字节为单位）。  

<a name="sqlcoptsspreservecursors"></a>
## <a name="sql_copt_ss_preserve_cursors"></a>SQL_COPT_SS_PRESERVE_CURSORS  
 在提交/回滚事务时，此属性允许您查询和设置连接是否保留游标。 设置为 SQL_PC_ON 或 SQL_PC_OFF。 默认值为 SQL_PC_OFF。 此设置控制在调用 [SQLEndTran](../../relational-databases/native-client-odbc-api/sqlendtran.md) (或 SQLTransact) 时，驱动程序是否将 (s) 关闭光标。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_PC_OFF|默认。 使用 **SQLEndTran** 提交或回滚事务时将关闭游标。|  
|SQL_PC_ON|当使用 **SQLEndTran** 提交或回滚事务时，游标不会关闭，除非在异步模式下使用静态或键集游标。 如果在发出回滚命令时未完成游标的填充，则关闭游标。|  

<a name="sqlcoptssquotedident"></a>
## <a name="sql_copt_ss_quoted_ident"></a>SQL_COPT_SS_QUOTED_IDENT  
 SQL_COPT_SS_QUOTED_IDENT 允许在对连接提交的 ODBC 和 Transact-SQL 语句中包含带引号的标识符。 通过提供带引号的标识符，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序允许使用否则无效的对象名称，如 "My Table"，它在标识符中包含空格字符。 有关详细信息，请参阅 SET QUOTED_IDENTIFIER。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_QI_OFF|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接不允许在提交的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中使用带引号的标识符。|  
|SQL_QI_ON|默认。 连接允许在提交的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中使用带引号的标识符。|  

<a name="sqlcoptsstranslate"></a>
## <a name="sql_copt_ss_translate"></a>SQL_COPT_SS_TRANSLATE  
 SQL_COPT_SS_TRANSLATE 导致在交换 MBCS 数据时驱动程序在客户端和服务器代码页之间进行字符转换。 此属性仅影响存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **char**、 **varchar** 和 **text** 列中的数据。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_XL_OFF|在客户端和服务器之间交换字符数据时，驱动程序不将一种代码页的字符转换为另一种代码页的字符。|  
|SQL_XL_ON|默认。 在客户端和服务器之间交换字符数据时，驱动程序将一种代码页的字符转换为另一种代码页的字符。 驱动程序自动配置字符转换，确定服务器上安装的代码页和客户端使用的代码页。|  

<a name="sqlcoptsstrustservercertificate"></a>
## <a name="sql_copt_ss_trust_server_certificate"></a>SQL_COPT_SS_TRUST_SERVER_CERTIFICATE  
 SQL_COPT_SS_TRUST_SERVER_CERTIFICATE 在使用加密时导致驱动程序启用或禁用证书验证。 此属性的值是可读/写的，但是在建立连接后设置它没有影响。  
  
 客户端应用程序可以在打开连接后查询此属性，以确定使用的实际加密和验证设置。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_TRUST_SERVER_CERTIFICATE_NO|默认。 不启用不带证书验证的加密。|  
|SQL_TRUST_SERVER_CERTIFICATE_YES|启用不带证书验证的加密。|  

<a name="sqlcoptsstxnisolation"></a>
## <a name="sql_copt_ss_txn_isolation"></a>SQL_COPT_SS_TXN_ISOLATION  
 SQL_COPT_SS_TXN_ISOLATION 设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特有的快照隔离属性。 不能使用 SQL_ATTR_TXN_ISOLATION 设置快照隔离，因为该值是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特有的。 不过，可以使用 SQL_ATTR_TXN_ISOLATION 或 SQL_COPT_SS_TXN_ISOLATION 来检索它。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_TXN_SS_SNAPSHOT|指示您无法从一个事务中看到在其他事务中进行的更改，即便重新查询也是如此。|  
  
 有关快照隔离的详细信息，请参阅 [使用快照隔离](../../relational-databases/native-client/features/working-with-snapshot-isolation.md)。  
  
## <a name="sql_copt_ss_use_proc_for_prep"></a>SQL_COPT_SS_USE_PROC_FOR_PREP

 不再支持该属性。  

<a name="sqlcoptssuserdata"></a>
## <a name="sql_copt_ss_user_data"></a>SQL_COPT_SS_USER_DATA  
 SQL_COPT_SS_USER_DATA 设置用户数据指针。 用户数据是基于每个连接记录的客户端拥有的内存。  
  
 有关详细信息，请参阅 [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)。  

<a name="sqlcoptsswarnoncperror"></a>
## <a name="sql_copt_ss_warn_on_cp_error"></a>SQL_COPT_SS_WARN_ON_CP_ERROR  
 此属性确定在代码页转换期间丢失数据时系统是否会向您发出警告。 这仅适用于来自服务器的数据。  
  
|值|说明|  
|-----------|-----------------|  
|SQL_WARN_YES|在代码页转换过程中遇到数据丢失时生成警告。|  
|SQL_WARN_NO|（默认）在代码页转换过程中遇到数据丢失时不生成警告。|  
  
## <a name="sqlsetconnectattr-support-for-service-principal-names-spns"></a>SQLSetConnectAttr 对服务主体名称 (SPN) 的支持

 SQLSetConnectAttr 可用于设置新连接属性的值 SQL_COPT_SS_SERVER_SPN 和 SQL_COPT_SS_FAILOVER_PARTNER_SPN。 打开连接时不能设置这些属性；如果在打开连接时尝试设置这些属性，将返回错误 HY011 并显示消息“此时操作无效”。  (SQLSetConnectOption 还可用于设置这些值。 )   
  
 有关 Spn 的详细信息，请参阅 [&#40;ODBC&#41;的客户端连接中的服务主体名称 &#40;spn&#41; ](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md)。  

<a name="sqlcoptssconnectiondead"></a>
## <a name="sql_copt_ss_connection_dead"></a>SQL_COPT_SS_CONNECTION_DEAD  
 这是一个只读属性。  
  
 有关 SQL_COPT_SS_CONNECTION_DEAD 的详细信息，请参阅 [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) 和 [连接到数据源 &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/connecting-to-a-data-source-odbc.md)。  
  
## <a name="example"></a>示例

 此示例记录性能数据。  
  
```  
SQLPERF*     pSQLPERF;  
SQLINTEGER   nValue;  
  
// See if you are already logging. SQLPERF* will be NULL if not.  
SQLGetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA, &pSQLPERF,  
    sizeof(SQLPERF*), &nValue);  
  
if (pSQLPERF == NULL)  
    {  
    // Set the performance log file name.  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG,  
        (SQLPOINTER) "\\My LogDirectory\\MyServerLog.txt", SQL_NTS);  
  
    // Start logging...  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA,  
        (SQLPOINTER) SQL_PERF_START, SQL_IS_INTEGER);  
    }  
else  
    {  
    // Take a snapshot now so that your performance statistics are discernible.  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
    }  
  
    // ...perform some action...  
  
// ...take a performance data snapshot...  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
  
    // ...perform more actions...  
  
// ...take another snapshot...  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
  
// ...and disable logging.  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA,  
    (SQLPOINTER) SQL_PERF_STOP, SQL_IS_INTEGER);  
  
// Continue on...  
```  
  
## <a name="see-also"></a>另请参阅

 [SQLSetConnectAttr 函数](../../odbc/reference/syntax/sqlsetconnectattr-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)   
 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS (Transact-SQL)](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)   
 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)   
 [SQLPrepare 函数](../../odbc/reference/syntax/sqlprepare-function.md)   
 [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md)  
  
