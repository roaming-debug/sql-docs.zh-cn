---
title: ODBC DSN 和连接字符串关键字
description: 此页面列出了 ODBC Driver for SQL Server 中提供的连接字符串和 DSN 的关键字以及 SQLSetConnectAttr 和 SQLGetConnectAttr 的连接属性。
ms.custom: ''
ms.date: 02/04/2019
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-chojas
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: d905ac9c7f2ea02bfac04b54e247d833ae337b96
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837382"
---
# <a name="dsn-and-connection-string-keywords-and-attributes"></a>DSN 和连接字符串关键字和属性

此页面列出了 ODBC Driver for SQL Server 中提供的连接字符串和 DSN 的关键字以及 SQLSetConnectAttr 和 SQLGetConnectAttr 的连接属性。

## <a name="supported-dsnconnection-string-keywords-and-connection-attributes"></a>受支持的 DSN/连接字符串关键字和连接属性

下表列出了每个平台的可用关键字和属性（L：Linux；M：macOS；W：Windows）。 单击关键字或属性获取更多详细信息。

| DSN / 连接字符串关键字 | 连接属性 | 平台 |
|-|-|-|
| [Addr](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Address](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [AnsiNPW](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ANSI_NPW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssansinpw) | LMW |
| [APP](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ApplicationIntent](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_APPLICATION_INTENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssapplicationintent) | LMW |
| [AttachDBFileName](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ATTACHDBFILENAME](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssattachdbfilename) | LMW |
| [身份验证](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | [SQL_COPT_SS_AUTHENTICATION](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | LMW |
| [AutoTranslate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRANSLATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstranslate) | LMW |
| [ColumnEncryption](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | [SQL_COPT_SS_COLUMN_ENCRYPTION](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | LMW |
| [ConnectRetryCount](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_COUNT](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [ConnectRetryInterval](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_INTERVAL](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [Database](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_ATTR_CURRENT_CATALOG](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| [说明](../../connect/odbc/dsn-connection-string-attribute.md#description) | | LMW |
| [驱动程序](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [DSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Encrypt](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ENCRYPT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssencrypt) | LMW |
| [Failover_Partner](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssfailoverpartner) | W |
| [FailoverPartnerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | W |
| [FileDSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [KeepAlive](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)（仅限 v17.4+，DSN）| | LMW |
| [KeepAliveInterval](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)（仅限 v17.4+，DSN） | | LMW |
| [KeystoreAuthentication](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystorePrincipalId](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystoreSecret](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [语言](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [MARS_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MARS_ENABLED](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmarsenabled) | LMW |
| [MultiSubnetFailover](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MULTISUBNET_FAILOVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmultisubnetfailover) | LMW |
| [Net](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Network](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [PWD](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [QueryLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquery) | W |
| [QueryLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquerylog) | W |
| [QueryLogTIme](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_INTERVAL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfqueryinterval) | W |
| [QuotedId](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_QUOTED_IDENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssquotedident) | LMW |
| [Regional](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [SaveFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Server](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ServerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_SERVER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| [StatsLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdata) | W |
| [StatsLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalog) | W |
| [TransparentNetworkIPResolution](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | [SQL_COPT_SS_TNIR](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | LMW |
| [Trusted_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_INTEGRATED_SECURITY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssintegratedsecurity) | LMW |
| [TrustServerCertificate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRUST_SERVER_CERTIFICATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstrustservercertificate) | LMW |
| [UID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [UseFMTONLY](../../connect/odbc/dsn-connection-string-attribute.md#usefmtonly) | | LMW |
| [WSID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| | [SQL_ATTR_ACCESS_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ACCESS_MODE) | LMW |
| | [SQL_ATTR_ASYNC_DBC_EVENT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCALLBACK](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCONTEXT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_AUTO_IPD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_AUTOCOMMIT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_AUTOCOMMIT) | LMW |
| | [SQL_ATTR_CONNECTION_DEAD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_CONNECTION_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_DBC_INFO_TOKEN](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_LOGIN_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_LOGIN_TIMEOUT) | LMW |
| | [SQL_ATTR_METADATA_ID](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_ODBC_CURSORS](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ODBC_CURSORS) | LMW |
| | [SQL_ATTR_PACKET_SIZE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_PACKET_SIZE) | LMW |
| | [SQL_ATTR_QUIET_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_QUIET_MODE) | LMW |
| | [SQL_ATTR_RESET_CONNECTION](../../odbc/reference/develop-driver/upgrading-a-3-5-driver-to-a-3-8-driver.md#connection-pooling) <br> (SQL_COPT_SS_RESET_CONNECTION) | LMW |  
| | [SQL_ATTR_TRACE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACE) | LMW |
| | [SQL_ATTR_TRACEFILE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACEFILE) | LMW |
| | [SQL_ATTR_TRANSLATE_LIB](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_DLL) | LMW |
| | [SQL_ATTR_TRANSLATE_OPTION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_OPTION) | LMW |
| | [SQL_ATTR_TXN_ISOLATION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TXN_ISOLATION) | LMW |
| | [SQL_COPT_SS_ACCESS_TOKEN](dsn-connection-string-attribute.md#sql_copt_ss_access_token) | LMW |
| | [SQL_COPT_SS_ANSI_OEM](dsn-connection-string-attribute.md#sql_copt_ss_ansi_oem)| W |
| | [SQL_COPT_SS_AUTOBEGINTXN](dsn-connection-string-attribute.md#sql_copt_ss_autobegintxn)| LMW |
| | [SQL_COPT_SS_BCP](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbcp) | LMW |
| | [SQL_COPT_SS_BROWSE_CACHE_DATA](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) | LMW |
| | [SQL_COPT_SS_BROWSE_CONNECT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseconnect) | LMW |
| | [SQL_COPT_SS_BROWSE_SERVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseserver) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREDATA](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoredata) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREPROVIDER](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoreprovider) | LMW |
| | [SQL_COPT_SS_CLIENT_CONNECTION_ID](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) | LMW |
| | [SQL_COPT_SS_CONCAT_NULL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconcatnull) | LMW |
| | [SQL_COPT_SS_CONNECTION_DEAD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconnectiondead) | LMW |
| | [SQL_COPT_SS_ENLIST_IN_DTC](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssenlistindtc) | W |
| | [SQL_COPT_SS_ENLIST_IN_XA](dsn-connection-string-attribute.md#sql_copt_ss_enlist_in_xa) | LMW |
| | [SQL_COPT_SS_FALLBACK_CONNECT](dsn-connection-string-attribute.md#sql_copt_ss_fallback_connect) | LMW |
| | [SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_MUTUALLY_AUTHENTICATED](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_OLDPWD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssoldpwd) | LMW |
| | [SQL_COPT_SS_PERF_DATA_LOG_NOW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalognow) | W |
| | [SQL_COPT_SS_PRESERVE_CURSORS](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsspreservecursors) | LMW |
| | [SQL_COPT_SS_SPID](../../connect/odbc/dsn-connection-string-attribute.md#sql_copt_ss_spid) (v17.5+) | LMW |
| | [SQL_COPT_SS_TXN_ISOLATION](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstxnisolation) | LMW |
| | [SQL_COPT_SS_USER_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssuserdata) | LMW |
| | [SQL_COPT_SS_WARN_ON_CP_ERROR](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsswarnoncperror) | LMW |
| [ClientCertificate](../../connect/odbc/dsn-connection-string-attribute.md#clientcertificate) | | LMW | 
| [ClientKey](../../connect/odbc/dsn-connection-string-attribute.md#clientkey) | | LMW | 


以下是[将连接字符串关键字用于 SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)、[SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 和 [SQLSetConnectAttr 函数](../../odbc/reference/syntax/sqlsetconnectattr-function.md)中未收录的一些连接字符串关键字和连接属性。

### <a name="description"></a>说明

用于描述数据源。

### <a name="sql_copt_ss_ansi_oem"></a>SQL_COPT_SS_ANSI_OEM

控制 ANSI 到 OEM 的数据转换。 

| 属性值 | 说明 |
|-|-|
| SQL_AO_OFF | （默认值）不执行转换。 |
| SQL_AO_ON | 执行转换。 |

### <a name="sql_copt_ss_autobegintxn"></a>SQL_COPT_SS_AUTOBEGINTXN

版本 17.6+ 自动提交关闭时，控制在回滚或提交后自动开始事务。

| 属性值 | 说明 |
|-|-|
| SQL_AUTOBEGINTXN_ON | （默认值）在回滚或提交后自动开始事务。 |
| SQL_AUTOBEGINTXN_OFF | 在回滚或提交后不自动开始事务。 |

### <a name="sql_copt_ss_fallback_connect"></a>SQL_COPT_SS_FALLBACK_CONNECT

控制 SQL Server 回退连接的使用。 此属性不再受支持。

| 属性值 | 说明 |
|-|-|
| SQL_FB_OFF | （默认值）禁用回退连接。 |
| SQL_FB_ON | 回退连接已启用。 |



## <a name="new-connection-string-keywords-and-connection-attributes"></a>新的连接字符串关键字和连接属性

###  <a name="authentication---sql_copt_ss_authentication"></a>身份验证 - SQL_COPT_SS_AUTHENTICATION

设置连接到 SQL Server 时要使用的身份验证模式。 有关详细信息，请参阅[使用 Azure Active Directory](using-azure-active-directory.md)。

| 关键字值 | 属性值 | 说明 |
|-|-|-|
| |SQL_AU_NONE|（默认值）未设置。 其他属性的组合可确定身份验证模式。|
|SqlPassword|SQL_AU_PASSWORD|SQL Server 身份验证（使用用户名和密码）。|
|ActiveDirectoryIntegrated|SQL_AU_AD_INTEGRATED|Azure Active Directory 集成身份验证。|
|ActiveDirectoryPassword|SQL_AU_AD_PASSWORD|Azure Active Directory 密码身份验证。|
|ActiveDirectoryInteractive|SQL_AU_AD_INTERACTIVE|Azure Active Directory 交互式身份验证。|
|ActiveDirectoryMsi|SQL_AU_AD_MSI|Azure Active Directory 托管标识身份验证。 对于用户分配的标识，UID 设置为用户标识的对象 ID。 |
| |SQL_AU_RESET|取消设置。 替代任何 DSN 或连接字符串设置。|

> [!NOTE]
> 使用 `Authentication` 关键字或属性时，将 `Encrypt` 显式指定为连接字符串/DSN/连接属性中所需的值。 有关详细信息，请参阅[将连接字符串关键字用于 SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。

### <a name="columnencryption---sql_copt_ss_column_encryption"></a>ColumnEncryption - SQL_COPT_SS_COLUMN_ENCRYPTION

控制透明列加密 (Always Encrypted)。 有关详细信息，请参阅[使用 Always Encrypted (ODBC)](using-always-encrypted-with-the-odbc-driver.md)。

| 关键字值 | 属性值 | 说明 |
|-|-|-|
|已启用|SQL_CE_ENABLED|启用 Always Encrypted。|
|已禁用|SQL_CE_DISABLED|（默认值）禁用 Always Encrypted。|
| |SQL_CE_RESULTSETONLY|仅启用解密（结果和返回值）。|

### <a name="transparentnetworkipresolution---sql_copt_ss_tnir"></a>TransparentNetworkIPResolution - SQL_COPT_SS_TNIR

控制透明网络 IP 解析功能，此功能与 MultiSubnetFailover 交互以允许更快的重新连接尝试。 有关详细信息，请参阅[使用透明网络 IP 解析](using-transparent-network-ip-resolution.md)。

| 关键字值 | 属性值| 说明 |
|-|-|-|
|已启用|SQL_IS_ON|（默认）启用透明网络 IP 解析。|
|已禁用|SQL_IS_OFF|禁用透明网络 IP 解析。|

### <a name="usefmtonly"></a>UseFMTONLY

连接到 SQL Server 2012 及更高版本时，控制对元数据的 SET FMTONLY 使用。

| 关键字值 | 说明 |
|-|-|
|否|（默认值）如果可用，请将 sp_describe_first_result_set 用于元数据。 |
|是| 对元数据使用 SET FMTONLY。 |


## <a name="clientcertificate"></a>ClientCertificate

指定用于身份验证的证书。 选项包括： 

| 选项值 | 说明 |
|-|-|
| sha1：`<hash_value>` | ODBC 驱动程序使用 SHA1 哈希在 Windows 证书存储中查找证书 |
| 主题：`<subject>` | ODBC 驱动程序使用主题在 Windows 证书存储中查找证书 |
| file:`<file_location>`[,password:`<password>`] | ODBC 驱动程序使用证书文件。 |

如果证书采用 PFX 格式，且 PFX 证书内的私钥受密码保护，则需要 password 关键字。 对于 PEM 和 DER 格式的证书，ClientKey 属性是必需的


## <a name="clientkey"></a>ClientKey

指定由 ClientCertificate 属性指定的 PEM 或 DER 证书的私钥的文件位置。 格式： 

| 选项值 | 说明 |
|-|-|
| file:`<file_location>`[,password:`<password>`] | 指定私钥文件的位置。 |

如果私钥文件受密码保护，则需要 password 关键字。 如果密码包含任何“,”字符，则会在每个字符后立即添加一个额外的“,”字符。 例如，如果密码为“a,b,c”，则连接字符串中存在的转义密码为“a,b,c”。 
    

### <a name="sql_copt_ss_access_token"></a>SQL_COPT_SS_ACCESS_TOKEN

允许使用 Azure Active Directory 访问令牌进行身份验证。 有关详细信息，请参阅[使用 Azure Active Directory](using-azure-active-directory.md)。

| 属性值 | 说明 |
|-|-|
| Null | （默认值）不提供任何访问令牌。 |
| ACCESSTOKEN* | 指向访问令牌的指针。 |

### <a name="sql_copt_ss_cekeystoredata"></a>SQL_COPT_SS_CEKEYSTOREDATA

与加载的密钥存储提供程序库进行通信。 请参阅控制透明列加密 (Always Encrypted)。 此属性没有默认值。 有关详细信息，请参阅[自定义密钥存储提供程序](custom-keystore-providers.md)。

| 属性值 | 说明 |
|-|-|
| CEKEYSTOREDATA * | 密钥存储提供程序库的通信数据结构 |

### <a name="sql_copt_ss_cekeystoreprovider"></a>SQL_COPT_SS_CEKEYSTOREPROVIDER

为 Always Encrypted 加载密钥存储提供程序库，或检索已加载的密钥存储提供程序库的名称。 有关详细信息，请参阅[自定义密钥存储提供程序](custom-keystore-providers.md)。 此属性没有默认值。

| 属性值 | 说明 |
|-|-|
| char * | 密钥存储提供程序库的路径 |

### <a name="sql_copt_ss_enlist_in_xa"></a>SQL_COPT_SS_ENLIST_IN_XA

要使用 XA 兼容事务处理器 (TP) 启用 XA 事务，应用程序需要使用 SQL_COPT_SS_ENLIST_IN_XA 和指向 `XACALLPARAM` 对象的指针调用 SQLSetConnectAttr  。 Windows（17.3 及更高版本）、Linux 和 macOS 支持此选项。
```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, param, SQL_IS_POINTER);  // XACALLPARAM *param
``` 
 要仅将 XA 事务与 ODBC 连接关联，请在调用 SQLSetConnectAttr 时使用 SQL_COPT_SS_ENLIST_IN_XA 而不是指针来提供 TRUE 或 FALSE  。 这仅在 Windows 上有效，不能用于通过客户端应用程序指定 XA 操作。 
 ```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, (SQLPOINTER)TRUE, 0);
``` 

|值|说明|平台|  
|-----------|-----------------|-----------------|  
|XACALLPARAM 对象*|指向 `XACALLPARAM` 对象的指针。|Windows、Linux 和 macOS|
|TRUE|将 XA 事务与 ODBC 连接关联。 将在 XA 事务的保护下执行所有相关的数据库活动。|Windows|  
|FALSE|将事务与 ODBC 连接解除关联。|Windows|

 若要详细了解 XA 事务，请参阅[使用 XA 事务](../../connect/odbc/use-xa-with-dtc.md)。

### <a name="sql_copt_ss_spid"></a>SQL_COPT_SS_SPID

检索连接的服务器进程 ID。 这相当于 T-SQL [@@SPID](../../t-sql/functions/spid-transact-sql.md) 变量，不同之处在于它不会导致额外往返到服务器。

| 属性值 | 说明 |
|-|-|
| DWORD | SPID |
