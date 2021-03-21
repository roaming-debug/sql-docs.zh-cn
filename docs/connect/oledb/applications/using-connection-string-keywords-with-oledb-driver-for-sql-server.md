---
title: 结合使用连接字符串关键字与 OLE DB 驱动程序
description: 某些 OLE DB Driver for SQL Server API 使用连接字符串，这些字符串是标识特定连接属性的关键字和值的列表。
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
f1_keywords:
- sql13.swb.connecttoserver.options.registeredservers.f1
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], connection string keywords
- MSOLEDBSQL, connection string keywords
- connection strings [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, connection string keywords
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c5ddba7e6b2ac0faef93ff544dbb5f23f3fb5d15
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751197"
---
# <a name="using-connection-string-keywords-with-ole-db-driver-for-sql-server"></a>将连接字符串关键字与适用于 SQL Server 的 OLE DB 驱动程序结合使用
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  某些 OLE DB Driver for SQL Server API 使用连接字符串来指定连接属性。 连接字符串是关键字和关联的值的列表；每个关键字标识特定的连接属性。  
  
> [!NOTE]
> 适用于 SQL Server 的 OLE DB 驱动程序允许连接字符串中存在多义性，以保持后向兼容性（例如，可以多次指定某些关键字，并且允许基于位置或优先级对发生冲突的关键字进行解析）。 OLE DB Driver for SQL Server 的未来版本可能不允许连接字符串中存在多义性。 在修改要使用适用于 SQL Server 的 OLE DB 驱动程序的应用程序时，最好消除连接字符串多义性上的任何依赖项。  
  
 以下各节描述了可以在使用 OLE DB Driver for SQL Server 作为数据提供程序时用于 OLE DB Driver for SQL Server 和 ActiveX 数据对象 (ADO) 的关键字。  

## <a name="ole-db-driver-connection-string-keywords"></a>OLE DB Driver 连接字符串关键字  

 OLE DB 应用程序可以用两种方式初始化数据源对象：  
  
-   **IDBInitialize::Initialize**  
  
-   **IDataInitialize::GetDataSource**  
  
 在第一种情况下，通过在 DBPROPSET_DBINIT 属性集中设置属性 DBPROP_INIT_PROVIDERSTRING，可以用访问接口字符串初始化连接属性。 在第二种情况下，可以将初始化字符串传递到 IDataInitialize::GetDataSource 方法以初始化连接属性  。 两种方法都初始化相同的 OLE DB 连接属性，但使用不同的关键字集合。 IDataInitialize::GetDataSource 所使用的关键字集合至少是初始化属性组内的属性的说明  。  
  
 将对应的 OLE DB 属性设置为某个默认值或显式设置为某个值的任何提供程序字符串设置，OLE DB 属性值将覆盖提供程序字符串中的设置。  
  
 通过 DBPROP_INIT_PROVIDERSTRING 值在提供程序字符串中所设置的布尔属性要使用值“`yes`”和“`no`”进行设置。 通过使用 IDataInitialize::GetDataSource 在初始化字符串中设置的布尔属性是使用值 `true` 和 `false` 进行设置的  。  
  
 使用 IDataInitialize::GetDataSource 的应用程序也可以使用由 IDBInitialize::Initialize 使用的关键字，但仅用于没有默认值的属性   。 如果应用程序在初始化字符串中同时使用 IDataInitialize::GetDataSource 关键字和 IDBInitialize::Initialize 关键字，则使用 IDataInitialize::GetDataSource 关键字设置    。 建议应用程序不要在 IDataInitialize:GetDataSource 连接字符串中使用 IDBInitialize::Initialize 关键字，因为在未来的版本中可能不保留该行为   。  
  
> [!NOTE]  
>  通过 IDataInitialize::GetDataSource 传递的连接字符串将转换为属性并且通过 IDBProperties::SetProperties 应用   。 如果组件服务在 IDBProperties::GetPropertyInfo 中找到了属性说明，则此属性将作为独立属性应用  。 否则，它将通过 DBPROP_PROVIDERSTRING 属性应用。 例如，如果指定连接字符串 Data Source=server1;Server=server2，数据源将设置为属性，但服务器将转换为提供程序字符串    。  
  
 如果指定访问接口特定的属性相同的多个实例，则使用第一个属性的第一个值。  

## <a name="using-idbinitializeinitialize"></a>Using IDBInitialize::Initialize

 将 IDBInitialize::Initialize 和 DBPROP_INIT_PROVIDERSTRING 配合使用的 OLE DB 应用程序所使用的连接字符串具有以下语法  ：  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=[{]attribute-value[}]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 属性值可以选择放在大括号中，并且最好这样做。 这样可以避免当属性值包含非字母数字字符时出现问题。 值中的第一个右大括号用于终止值，因此值不能包含右大括号。  
  
 连接字符串关键字的 `=` 符号之后的空白字符将被解释为文本，即使该值在引号内也是如此。  
  
 下表对可能与 DBPROP_INIT_PROVIDERSTRING 一起使用的关键字进行了说明。  
  
|关键字|初始化属性|说明|  
|-------------|-----------------------------|-----------------|  
|**Addr**|SSPROP_INIT_NETWORKADDRESS|“Address”的同义词  。|  
|**Address**|SSPROP_INIT_NETWORKADDRESS|运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的服务器的网络地址。 “Address”通常是服务器的网络名称，也可以是诸如管道、IP 地址或 TCP/IP 端口和套接字地址之类的其他名称  。<br /><br /> 如果指定了 IP 地址，请确保在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器中启用了 TCP/IP 或 named pipes 协议。<br /><br /> 使用 OLE DB Driver for SQL Server 时，“Address”  的值优先于传递给连接字符串中的“Server”  的值。 另外请注意，`Address=;` 将连接到在 Server 关键字中指定的服务器，而 `Address= ;, Address=.;`、 `Address=localhost;` 和 `Address=(local);` 都会产生本地服务器的连接  。<br /><br /> Address 关键字的完整语法如下所示  ：<br /><br /> [_protocol_ **:** ]_Address_[ **,** _port &#124;\pipe\pipename_]<br /><br /> _protocol_ 可以是 **tcp** (TCP/IP)、 **lpc** （共享内存）或 **np** （命名管道）。 有关协议的详细信息，请参阅[配置客户端协议](../../../database-engine/configure-windows/configure-client-protocols.md)。<br /><br /> 如果未指定协议  或“Network”  关键字，则 OLE DB Driver for SQL Server 将使用在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager 中指定的协议顺序。<br /><br /> “port”是指定服务器上所要连接到的端口  。 默认情况下，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用端口 1433。|   
|**APP**|SSPROP_INIT_APPNAME|用于标识应用程序的字符串。|  
|**ApplicationIntent**|SSPROP_INIT_APPLICATIONINTENT|连接到服务器时声明应用程序工作负荷类型。 可能值为 `ReadOnly` 和 `ReadWrite`。<br /><br /> 默认为 `ReadWrite`。 有关对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的 OLE DB Driver for SQL Server 支持的详细信息，请参阅 [OLE DB Driver for SQL Server 对高可用性和灾难恢复的支持](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|  
|**AttachDBFileName**|SSPROP_INIT_FILENAME|可附加数据库的主文件的名称（包括完整路径名）。 若要使用 AttachDBFileName，还必须使用访问接口字符串 Database 关键字来指定数据库名称  。 如果该数据库以前已经附加，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不会重新附加它（而是使用已附加的数据库作为连接的默认数据库）。|  
|**身份验证**<a href="#table1_1"><sup id="table1_authmode">**1**</sup></a>|SSPROP_AUTH_MODE|指定使用的 SQL 或 Active Directory 身份验证。 有效值是：<br/><ul><li>`(not set)`设置用户帐户 ：身份验证模式由其他关键字确定。</li><li>`ActiveDirectoryPassword:`使用 Azure Active Directory 标识进行用户 ID 和密码身份验证。</li><li>`ActiveDirectoryIntegrated:` 使用 Azure Active Directory 标识进行集成身份验证。</li><br/>**注意：** `ActiveDirectoryIntegrated` 关键字还可用于对 SQL Server 进行 Windows 身份验证。 它将替换 `Integrated Security`（或 `Trusted_Connection`）身份验证关键字。 建议  使用 `Integrated Security`（或 `Trusted_Connection`）关键字或其相应属性的应用程序将 `Authentication` 关键字（或其相应属性）的值设置为 `ActiveDirectoryIntegrated` 以启用新的加密和证书验证行为。<br/><br/><li>`ActiveDirectoryInteractive:` 使用 Azure Active Directory 标识进行交互式身份验证。 此方法支持 Azure 多重身份验证 (MFA)。 </li><li>`ActiveDirectoryMSI:` [托管标识 (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) 身份验证。 对于用户分配的标识，用户 ID 应设置为用户标识的对象 ID。</li><li>`ActiveDirectoryServicePrincipal:` Azure Active Directory 服务主体身份验证。 用户 ID 应设置为应用程序（客户端）ID。 密码应设置为应用程序（客户端）机密。</li><li>`SqlPassword:` 使用用户 ID 和密码的身份验证。</li><br/>**注意：** 建议  使用 `SQL Server` 身份验证的应用程序将 `Authentication` 关键字（或其相应属性）的值设置为 `SqlPassword` 以启用 [新的加密和证书验证行为](../features/using-azure-active-directory.md#encryption-and-certificate-validation)。</ul>|
|**自动翻译**|SSPROP_INIT_AUTOTRANSLATE|“AutoTranslate”的同义词  。|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|配置 OEM/ANSI 字符转换。 已识别的值为 `yes` 和 `no`。|  
|**Database**|DBPROP_INIT_CATALOG|数据库名称。|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|指定要使用的数据类型的处理模式。 对于提供程序数据类型，可识别的值为 `0`，对于 SQL Server 2000 数据类型则为 `80`。|  
|**加密**<a href="#table1_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|指定在通过网络发送数据之前是否应当将其加密。 可能值为 `yes` 和 `no`。 默认值是 `no`。|  
|**FailoverPartner**|SSPROP_INIT_FAILOVERPARTNER|用于数据库镜像的故障转移服务器的名称。|  
|**FailoverPartnerSPN**|SSPROP_INIT_FAILOVERPARTNERSPN|故障转移伙伴的 SPN。 默认值为空字符串。 空字符串导致 OLE DB Driver for SQL Server 使用提供程序生成的默认 SPN。|  
|**语言**|SSPROPT_INIT_CURRENTLANGUAGE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语言。|  
|**MarsConn**|SSPROP_INIT_MARSCONNECTION|如果服务器是 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本，则启用或禁用连接上的多个活动结果集 (MARS)。 可能值为 `yes` 和 `no`。 默认值是 `no`。|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|在连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可用性组或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的可用性组侦听程序时，应始终指定 MultiSubnetFailover=Yes  。 MultiSubnetFailover=Yes 将配置适用于 SQL Server 的 OLE DB 驱动程序以便更快地检测和连接到（当前）活动服务器。  可能值为 `Yes` 和 `No`。 默认为 `No`。 例如：<br /><br /> `MultiSubnetFailover=Yes`<br /><br /> 有关对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的 OLE DB Driver for SQL Server 支持的详细信息，请参阅 [OLE DB Driver for SQL Server 对高可用性和灾难恢复的支持](../../oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|  
|**Net**|SSPROP_INIT_NETWORKLIBRARY|“Network”的同义词  。|  
|**Network**|SSPROP_INIT_NETWORKLIBRARY|用于与组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例建立连接的网络库。|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|“Network”的同义词  。|  
|**PacketSize**|SSPROP_INIT_PACKETSIZE|表格格式数据流 (TDS) 包大小。 默认值为 0（实际值将由服务器决定）。|  
|**PersistSensitive**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|接受字符串 `yes` 和 `no` 作为值。 如果使用的是 `no`，则不允许数据源对象保留敏感的身份验证信息|  
|**PWD**|DBPROP_AUTH_PASSWORD|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录密码。|  
|**Server**|DBPROP_INIT_DATASOURCE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。 该值必须是服务器的网络名称、IP 地址或者 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器别名。<br /><br /> 如果不指定，则与本地计算机上的默认实例建立连接。<br /><br /> “Address”  关键字将替代“Server”  关键字。<br /><br /> 通过指定以下条件之一，可连接到本地服务器的默认实例：<br /><br /> **Server=;**<br /><br /> Server=.; <br /><br /> **Server=(local);**<br /><br /> **Server=(local);**<br /><br /> **Server=(localhost);**<br /><br /> **Server=(localdb)\\** *instancename* **;**<br /><br /> 有关 LocalDB 支持的详细信息，请参阅 [LocalDB 的 OLE DB Driver for SQL Server 支持](../../oledb/features/oledb-driver-for-sql-server-support-for-localdb.md)。<br /><br /> 若要指定的命名的实例[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，将追加 **\\** _InstanceName_。<br /><br /> 如果不指定服务器，则与本地计算机上的默认实例建立连接。<br /><br /> 如果指定了 IP 地址，请确保在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器中启用了 TCP/IP 或 named pipes 协议。<br /><br /> Server  关键字的完整语法如下：<br /><br /> **Server=** [_protocol_ **:** ]*Server*[ **,** _port_]<br /><br /> _protocol_ 可以是 **tcp** (TCP/IP)、 **lpc** （共享内存）或 **np** （命名管道）。<br /><br /> 以下示例将指定一个命名管道：<br /><br /> `np:\\.\pipe\MSSQL$MYINST01\sql\query`<br /><br /> 上述行指定命名管道协议 (`np`)、本地计算机上的一个命名管道 (`\\.\pipe`)、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称 (`MSSQL$MYINST01`)，以及命名管道的默认名称 (`sql/query`)。<br /><br /> 如果未指定协议  或“Network”  关键字，则 OLE DB Driver for SQL Server 将使用在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager 中指定的协议顺序。<br /><br /> “port”是指定服务器上所要连接到的端口  。 默认情况下，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用端口 1433。<br /><br /> 在使用 OLE DB Driver for SQL Server 时，对于传递到连接字符串中“Server”  的值，将忽略其前面的空格。|   
|**ServerSPN**|SSPROP_INIT_SERVERSPN|服务器的 SPN。 默认值为空字符串。 空字符串导致 OLE DB Driver for SQL Server 使用提供程序生成的默认 SPN。|  
|**超时**|DBPROP_INIT_TIMEOUT|等待数据源初始化完成的时间（秒）。|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|如果第一个解析的主机名 IP 未响应，且存在多个与主机名关联的 IP，就会影响连接序列。 TNIR 与 MultiSubnetFailover 交互，以提供其他连接序列。 可能值为 `Yes` 和 `No`。 默认值是 `Yes`。 有关详细信息，请参阅[使用透明网络 IP 解析](../../oledb/features/using-transparent-network-ip-resolution.md)。|  
|**Trusted_Connection**|DBPROP_AUTH_INTEGRATED|如果是 `yes`，这指示 OLE DB Driver for SQL Server 使用 Windows 身份验证模式进行登录验证。 否则，OLE DB Driver for SQL Server 将使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 用户名和密码进行登录验证，并且必须指定 UID 和 PWD 关键字。|  
|**TrustServerCertificate**<a href="#table1_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|接受字符串 `yes` 和 `no` 作为值。 默认值为 `no`，它意味着将验证服务器证书。|  
|**UID**|DBPROP_AUTH_USERID|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名。|  
|**UseFMTONLY**|SSPROP_INIT_USEFMTONLY|控制在连接到 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 及更高版本时的元数据检索方式。 可能值为 `yes` 和 `no`。 默认值是 `no`。<br /><br />默认情况下，OLE DB Driver for SQL Server 使用 [ sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) 和 [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) 存储过程来检索元数据。 这些存储过程有一些限制（例如，对临时表进行操作时存储过程将失败）。 将 UseFMTONLY  设置为 `yes` 指示驱动程序改用 [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) 进行元数据检索。|  
|**UseProcForPrepare**|SSPROP_INIT_USEPROCFORPREP|不推荐使用该关键字，并且 OLE DB Driver for SQL Server 将忽略其设置。|  
|**WSID**|SSPROP_INIT_WSID|工作站标识符。|  
  
<b id="table1_1">[1]:</b>为提高安全性，在使用身份验证或访问令牌初始化属性或其相应的连接字符串关键字时，将修改加密和证书验证行为。 有关详细信息，请参阅[加密和证书验证](../features/using-azure-active-directory.md#encryption-and-certificate-validation)。

## <a name="using-idatainitializegetdatasource"></a>Using IDataInitialize::GetDataSource

 使用 IDataInitialize::GetDataSource 的 OLE DB 应用程序所使用的连接字符串有以下语法  ：  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=[quote]attribute-value[quote]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 - `quote ::= " | '`  
  
 属性使用必须符合其作用域中允许的语法。 例如，WSID  使用大括号 ({}  ) 引号字符，而应用程序名称  则使用单引号字符 ('  ) 或双引号字符 ("  )。 只有字符串属性可以加引号。 尝试将整数或枚举属性用引号引起来将导致 `Connection String does not conform to OLE DB specification` 错误。  
  
 属性值可以选择放在单引号或双引号内，并且最好这样做。 这样可以避免当值包含非字母数字字符时出现问题。 也可以在值内使用引号字符，只要使用的是双引号。  
  
 连接字符串关键字的等号 (=) 之后的空白字符将被解释为文本，即使该值在引号内也是如此。  
  
 如果某一连接字符串具有下表所列的多个属性，将使用最后一个属性的值。  
  
 下表对可能与 IDataInitialize::GetDataSource 配合使用的关键字进行了说明  ：  
  
|关键字|初始化属性|说明|  
|-------------|-----------------------------|-----------------|  
|**访问令牌**<a href="#table2_1"><sup id="table2_accesstoken">**1**</sup></a>|SSPROP_AUTH_ACCESS_TOKEN|用于对 Azure Active Directory 进行身份验证的访问令牌。 <br/><br/>**注意：** 指定此关键字的同时还指定 `UID`、`PWD`、`Trusted_Connection` 或 `Authentication` 连接字符串关键字或其相应属性/关键字的话会出现错误。|
|**应用程序名称**|SSPROP_INIT_APPNAME|用于标识应用程序的字符串。|  
|**Application Intent**|SSPROP_INIT_APPLICATIONINTENT|连接到服务器时声明应用程序工作负荷类型。 可能值为 `ReadOnly` 和 `ReadWrite`。<br /><br /> 默认为 `ReadWrite`。 有关对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的 OLE DB Driver for SQL Server 支持的详细信息，请参阅 [OLE DB Driver for SQL Server 对高可用性和灾难恢复的支持](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|  
|**身份验证**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_AUTH_MODE|指定使用的 SQL 或 Active Directory 身份验证。 有效值是：<br/><ul><li>`(not set)`设置用户帐户 ：身份验证模式由其他关键字确定。</li><li>`ActiveDirectoryPassword:`使用 Azure Active Directory 标识进行用户 ID 和密码身份验证。</li><li>`ActiveDirectoryIntegrated:` 使用 Azure Active Directory 标识进行集成身份验证。</li><br/>**注意：** `ActiveDirectoryIntegrated` 关键字还可用于对 SQL Server 进行 Windows 身份验证。 它将替换 `Integrated Security`（或 `Trusted_Connection`）身份验证关键字。 建议  使用 `Integrated Security`（或 `Trusted_Connection`）关键字或其相应属性的应用程序将 `Authentication` 关键字（或其相应属性）的值设置为 `ActiveDirectoryIntegrated` 以启用新的加密和证书验证行为。<br/><br/><li>`ActiveDirectoryInteractive:` 使用 Azure Active Directory 标识进行交互式身份验证。 此方法支持 Azure 多重身份验证 (MFA)。 </li><li>`ActiveDirectoryMSI:` [托管标识 (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) 身份验证。 对于用户分配的标识，用户 ID 应设置为用户标识的对象 ID。</li><li>`ActiveDirectoryServicePrincipal:` Azure Active Directory 服务主体身份验证。 用户 ID 应设置为应用程序（客户端）ID。 密码应设置为应用程序（客户端）机密。</li><li>`SqlPassword:` 使用用户 ID 和密码的身份验证。</li><br/>**注意：** 建议  使用 `SQL Server` 身份验证的应用程序将 `Authentication` 关键字（或其相应属性）的值设置为 `SqlPassword` 以启用 [新的加密和证书验证行为](../features/using-azure-active-directory.md#encryption-and-certificate-validation)。</ul>|
|**自动翻译**|SSPROP_INIT_AUTOTRANSLATE|“AutoTranslate”的同义词  。|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|配置 OEM/ANSI 字符转换。 已识别的值为 `true` 和 `false`。|  
|**Connect Timeout**|DBPROP_INIT_TIMEOUT|等待数据源初始化完成的时间（秒）。|  
|**Current Language**|SSPROPT_INIT_CURRENTLANGUAGE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语言名称。|  
|**数据源**|DBPROP_INIT_DATASOURCE|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。<br /><br /> 如果不指定，则与本地计算机上的默认实例建立连接。<br /><br /> 若要详细了解有效的地址语法，请参阅本主题中的 Server 关键字说明  。|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|指定要使用的数据类型的处理模式。 可识别的值对提供程序数据类型为 `0`，对 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 数据类型为 `80`。|  
|**Failover Partner**|SSPROP_INIT_FAILOVERPARTNER|用于数据库镜像的故障转移服务器的名称。|  
|**Failover Partner SPN**|SSPROP_INIT_FAILOVERPARTNERSPN|故障转移伙伴的 SPN。 默认值为空字符串。 空字符串导致 OLE DB Driver for SQL Server 使用提供程序生成的默认 SPN。|  
|**初始目录**|DBPROP_INIT_CATALOG|数据库名称。|  
|**初始文件名**|SSPROP_INIT_FILENAME|可附加数据库的主文件的名称（包括完整路径名）。 若要使用 AttachDBFileName，还必须使用访问接口字符串 DATABASE 关键字来指定数据库名称  。 如果该数据库以前已经附加，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不会重新附加它（而是使用已附加的数据库作为连接的默认数据库）。|  
|**Integrated Security**|DBPROP_AUTH_INTEGRATED|接受 Windows 身份验证的值 `SSPI`。|  
|**MARS Connection**|SSPROP_INIT_MARSCONNECTION|启用或禁用连接上的多个活动结果集 (MARS)。 已识别的值为 `true` 和 `false`。 默认为 `false`。|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|在连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可用性组或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的可用性组侦听程序时，应始终指定 MultiSubnetFailover=True  。 MultiSubnetFailover=True 将配置适用于 SQL Server 的 OLE DB 驱动程序，以便更快地检测和连接到（当前）活动服务器。  可能值为 `True` 和 `False`。 默认为 `False`。 例如：<br /><br /> `MultiSubnetFailover=True`<br /><br /> 有关对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的 OLE DB Driver for SQL Server 支持的详细信息，请参阅 [OLE DB Driver for SQL Server 对高可用性和灾难恢复的支持](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|  
|**Network Address**|SSPROP_INIT_NETWORKADDRESS|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的网络地址。<br /><br /> 若要详细了解有效的地址语法，请参阅本主题中的 Address 关键字说明  。|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|用于与组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例建立连接的网络库。|  
|**Packet Size**|SSPROP_INIT_PACKETSIZE|表格格式数据流 (TDS) 包大小。 默认值为 0（实际值将由服务器决定）。|  
|**密码**|DBPROP_AUTH_PASSWORD|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录密码。|  
|**Persist Security Info**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|接受字符串 `true` 和 `false` 作为值。 如果是 `false`，则不允许数据源对象保留敏感的身份验证信息|  
|**提供程序**||对于 OLE DB Driver for SQL Server，这应为“MSOLEDBSQL”。|  
|**Server SPN**|SSPROP_INIT_SERVERSPN|服务器的 SPN。 默认值为空字符串。 空字符串导致 OLE DB Driver for SQL Server 使用提供程序生成的默认 SPN。|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|如果第一个解析的主机名 IP 未响应，且存在多个与主机名关联的 IP，就会影响连接序列。 TNIR 与 MultiSubnetFailover 交互，以提供其他连接序列。 可能值为 `True` 和 `False`。 默认值是 `True`。 有关详细信息，请参阅[使用透明网络 IP 解析](../../oledb/features/using-transparent-network-ip-resolution.md)。|  
|**信任服务器证书**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|接受字符串 `true` 和 `false` 作为值。 默认值为 `false`，它意味着将验证服务器证书。|  
|**对数据使用加密**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|指定在通过网络发送数据之前是否应当将其加密。 可能值为 `true` 和 `false`。 默认值是 `false`。|  
|**Use FMTONLY**|SSPROP_INIT_USEFMTONLY|控制在连接到 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 及更高版本时的元数据检索方式。 可能值为 `true` 和 `false`。 默认值是 `false`。<br /><br />默认情况下，OLE DB Driver for SQL Server 使用 [ sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) 和 [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) 存储过程来检索元数据。 这些存储过程有一些限制（例如，对临时表进行操作时存储过程将失败）。 将“Use FMTONLY”  设置为 `true` 指示驱动程序改用 [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) 进行元数据检索。|  
|**用户 ID**|DBPROP_AUTH_USERID|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名。|  
|**Workstation ID**|SSPROP_INIT_WSID|工作站标识符。|  
  
<b id="table2_1">[1]:</b>为提高安全性，在使用身份验证/访问令牌初始化属性或其相应的连接字符串关键字时，将修改加密和证书验证行为。 有关详细信息，请参阅[加密和证书验证](../features/using-azure-active-directory.md#encryption-and-certificate-validation)。

 > [!NOTE]
 > 在连接字符串中，`Old Password` 属性会设置 SSPROP_AUTH_OLD_PASSWORD，它是当前（有可能是过期的）密码，通过提供程序字符串属性无法得到该密码。  
  
## <a name="activex-data-objects-ado-connection-string-keywords"></a>ActiveX 数据对象 (ADO) 连接字符串关键字  

 ADO 应用程序设置 ADODBConnection 对象的 ConnectionString 属性，或提供连接字符串作为 ADODBConnection 对象的 Open 方法的参数     。  
  
 ADO 应用程序还可以使用由 OLE DB IDBInitialize::Initialize 方法使用的关键字，但仅针对没有默认值的属性  。 如果应用程序在初始化字符串中同时使用这些 ADO 关键字和 IDBInitialize::Initialize 关键字，则使用 ADO 关键字设置  。 建议应用程序仅使用 ADO 连接字符串关键字。  
  
 ADO 使用的连接字符串有以下语法：  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=["]attribute-value["]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 属性值可以选择放在双引号中，并且最好这样做。 这样可以避免当值包含非字母数字字符时出现问题。 属性值不能包含双引号。  
  
 下表对可能与 ADO 连接字符串一起使用的关键字进行了说明：  
  
|关键字|初始化属性|说明|  
|-------------|-----------------------------|-----------------|  
|**访问令牌**<a href="#table3_1"><sup id="table3_accesstoken">**1**</sup></a>|SSPROP_AUTH_ACCESS_TOKEN|用于对 Azure Active Directory 进行身份验证的访问令牌。<br/><br/>**注意：** 指定此关键字的同时还指定 `UID`、`PWD`、`Trusted_Connection` 或 `Authentication` 连接字符串关键字或其相应属性/关键字的话会出现错误。|
|**Application Intent**|SSPROP_INIT_APPLICATIONINTENT|连接到服务器时声明应用程序工作负荷类型。 可能值为 `ReadOnly` 和 `ReadWrite`。<br /><br /> 默认为 `ReadWrite`。 有关对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的 OLE DB Driver for SQL Server 支持的详细信息，请参阅 [OLE DB Driver for SQL Server 对高可用性和灾难恢复的支持](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|  
|**应用程序名称**|SSPROP_INIT_APPNAME|用于标识应用程序的字符串。|  
|**身份验证**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_AUTH_MODE|指定使用的 SQL 或 Active Directory 身份验证。 有效值是：<br/><ul><li>`(not set)`设置用户帐户 ：身份验证模式由其他关键字确定。</li><li>`ActiveDirectoryPassword:`使用 Azure Active Directory 标识进行用户 ID 和密码身份验证。</li><li>`ActiveDirectoryIntegrated:` 使用 Azure Active Directory 标识进行集成身份验证。</li><br/>**注意：** `ActiveDirectoryIntegrated` 关键字还可用于对 SQL Server 进行 Windows 身份验证。 它将替换 `Integrated Security`（或 `Trusted_Connection`）身份验证关键字。 建议  使用 `Integrated Security`（或 `Trusted_Connection`）关键字或其相应属性的应用程序将 `Authentication` 关键字（或其相应属性）的值设置为 `ActiveDirectoryIntegrated` 以启用新的加密和证书验证行为。<br/><br/><li>`ActiveDirectoryInteractive:` 使用 Azure Active Directory 标识进行交互式身份验证。 此方法支持 Azure 多重身份验证 (MFA)。 </li><li>`ActiveDirectoryMSI:` [托管标识 (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) 身份验证。 对于用户分配的标识，用户 ID 应设置为用户标识的对象 ID。</li><li>`ActiveDirectoryServicePrincipal:` Azure Active Directory 服务主体身份验证。 用户 ID 应设置为应用程序（客户端）ID。 密码应设置为应用程序（客户端）机密。</li><li>`SqlPassword:` 使用用户 ID 和密码的身份验证。</li><br/>**注意：** 建议  使用 `SQL Server` 身份验证的应用程序将 `Authentication` 关键字（或其相应属性）的值设置为 `SqlPassword` 以启用 [新的加密和证书验证行为](../features/using-azure-active-directory.md#encryption-and-certificate-validation)。</ul>|
|**自动翻译**|SSPROP_INIT_AUTOTRANSLATE|“AutoTranslate”的同义词  。|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|配置 OEM/ANSI 字符转换。 已识别的值为 `true` 和 `false`。|  
|**Connect Timeout**|DBPROP_INIT_TIMEOUT|等待数据源初始化完成的时间（秒）。|  
|**Current Language**|SSPROPT_INIT_CURRENTLANGUAGE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语言名称。|  
|**数据源**|DBPROP_INIT_DATASOURCE|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。<br /><br /> 如果不指定，则与本地计算机上的默认实例建立连接。<br /><br /> 若要详细了解有效的地址语法，请参阅本主题中的 Server 关键字说明  。|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|指定将使用的数据类型的处理模式。 对于提供程序数据类型，可识别的值为 `0`，对于 SQL Server 2000 数据类型则为 `80`。|  
|**Failover Partner**|SSPROP_INIT_FAILOVERPARTNER|用于数据库镜像的故障转移服务器的名称。|  
|**Failover Partner SPN**|SSPROP_INIT_FAILOVERPARTNERSPN|故障转移伙伴的 SPN。 默认值为空字符串。 空字符串导致 OLE DB Driver for SQL Server 使用提供程序生成的默认 SPN。|  
|**初始目录**|DBPROP_INIT_CATALOG|数据库名称。|  
|**初始文件名**|SSPROP_INIT_FILENAME|可附加数据库的主文件的名称（包括完整路径名）。 若要使用 AttachDBFileName，还必须使用提供程序字符串 DATABASE 关键字来指定数据库名称   。 如果该数据库以前已经附加，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不重新附加它；而是使用已附加的数据库作为连接的默认数据库。|  
|**Integrated Security**|DBPROP_AUTH_INTEGRATED|接受 Windows 身份验证的值 `SSPI`。|  
|**MARS Connection**|SSPROP_INIT_MARSCONNECTION|如果服务器是 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本，则启用或禁用连接上的多个活动结果集 (MARS)。 可能的值为 `true` 和 `false`。默认值为 `false`。|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|在连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可用性组或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的可用性组侦听程序时，应始终指定 MultiSubnetFailover=True  。 MultiSubnetFailover=True 将配置适用于 SQL Server 的 OLE DB 驱动程序，以便更快地检测和连接到（当前）活动服务器。  可能值为 `True` 和 `False`。 默认为 `False`。 例如：<br /><br /> `MultiSubnetFailover=True`<br /><br /> 有关对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的 OLE DB Driver for SQL Server 支持的详细信息，请参阅 [OLE DB Driver for SQL Server 对高可用性和灾难恢复的支持](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|  
|**Network Address**|SSPROP_INIT_NETWORKADDRESS|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的网络地址。<br /><br /> 若要详细了解有效的地址语法，请参阅本主题中的 Address 关键字说明  。|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|用于与组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例建立连接的网络库。|  
|**Packet Size**|SSPROP_INIT_PACKETSIZE|表格格式数据流 (TDS) 包大小。 默认值为 0（实际值将由服务器决定）。|  
|**密码**|DBPROP_AUTH_PASSWORD|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录密码。|  
|**Persist Security Info**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|接受字符串 `true` 和 `false` 作为值。 如果是 `false`，则不允许数据源对象保留敏感的身份验证信息。|  
|**提供程序**||对于 OLE DB Driver for SQL Server，值为 `MSOLEDBSQL`。|  
|**Server SPN**|SSPROP_INIT_SERVERSPN|服务器的 SPN。 默认值为空字符串。 空字符串导致 OLE DB Driver for SQL Server 使用提供程序生成的默认 SPN。|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|如果第一个解析的主机名 IP 未响应，且存在多个与主机名关联的 IP，就会影响连接序列。 TNIR 与 MultiSubnetFailover 交互，以提供其他连接序列。 可能值为 `True` 和 `False`。 默认值是 `True`。 有关详细信息，请参阅[使用透明网络 IP 解析](../../oledb/features/using-transparent-network-ip-resolution.md)。|  
|**信任服务器证书**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|接受字符串 `true` 和 `false` 作为值。 默认值为 `false`，它意味着将验证服务器证书。|  
|**对数据使用加密**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|指定在通过网络发送数据之前是否应当将其加密。 可能值为 `true` 和 `false`。 默认值是 `false`。|  
|**Use FMTONLY**|SSPROP_INIT_USEFMTONLY|控制在连接到 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 及更高版本时的元数据检索方式。 可能值为 `true` 和 `false`。 默认值是 `false`。<br /><br />默认情况下，OLE DB Driver for SQL Server 使用 [ sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) 和 [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) 存储过程来检索元数据。 这些存储过程有一些限制（例如，对临时表进行操作时存储过程将失败）。 将“Use FMTONLY”  设置为 `true` 指示驱动程序改用 [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) 进行元数据检索。|  
|**用户 ID**|DBPROP_AUTH_USERID|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名。|  
|**Workstation ID**|SSPROP_INIT_WSID|工作站标识符。|  
  
<b id="table3_1">[1]:</b>为提高安全性，在使用身份验证/访问令牌初始化属性或其相应的连接字符串关键字时，将修改加密和证书验证行为。 有关详细信息，请参阅[加密和证书验证](../features/using-azure-active-directory.md#encryption-and-certificate-validation)。

 > [!NOTE] 
 > 在连接字符串中，“旧密码”属性会设置 SSPROP_AUTH_OLD_PASSWORD，它是当前（有可能是过期的）密码，通过提供程序字符串属性无法得到该密码。  
  
## <a name="see-also"></a>另请参阅  

 [使用适用于 SQL Server 的 OLE DB 驱动程序生成应用程序](building-applications-with-oledb-driver-for-sql-server.md)