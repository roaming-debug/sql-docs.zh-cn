---
title: 连接选项
description: 本主题列出了SQLSRV 驱动程序中 sqlsrv_connect 的关联阵列中允许的选项，或 PDO_SQLSRV 驱动程序的数据源名称中允许的关键字。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 6d1ea295-8e34-438e-8468-4bbc0f76192c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a8718cc04972e74ffc39b1528ed73eb00664eda8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076249"
---
# <a name="connection-options"></a>连接选项
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

本主题列出了关联阵列中允许的选项（当在 SQLSRV 驱动程序中使用 [sqlsrv_connect](sqlsrv-connect.md) 时）或数据源名称 (dsn) 中允许的关键字（当在PDO_SQLSRV 驱动程序中使用 [PDO::__construct](pdo-construct.md) 时）。  

## <a name="table-of-connection-options"></a>连接选项表

|密钥|值|说明|默认|  
|-------|---------|---------------|-----------|  
|AccessToken|字符串|从 OAuth JSON 响应中提取的 Azure AD 访问令牌的字节字符串。<br /><br />连接字符串不得包含用户 ID、密码或 `Authentication` 关键字。 有关详细信息，请参阅[使用 Azure Active Directory 身份验证进行连接](azure-active-directory.md)。|未设置。|
|APP|字符串|指定跟踪中所使用的应用程序名称。|未设置。|  
|ApplicationIntent|字符串|连接到服务器时声明应用程序工作负荷类型。 可能的值为 ReadOnly 和 ReadWrite   。<br /><br />若要详细了解 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 对 [!INCLUDE[ssHADR](../../includes/sshadr_md.md)] 的支持，请参阅[高可用性和灾难恢复支持](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|ReadWrite|
|AttachDBFileName|字符串|指定服务器应附加的数据库文件。|未设置。|
|身份验证|以下字符串之一：<br /><br />**SqlPassword**<br /><br />**ActiveDirectoryPassword**<br /><br />**ActiveDirectoryMsi**<br /><br />**ActiveDirectoryServicePrincipal**|指定身份验证模式。<br /><br />有关详细信息，请参阅[使用 Azure Active Directory 身份验证进行连接](azure-active-directory.md)。|未设置。|
|CharacterSet<br /><br />（在 PDO_SQLSRV 驱动程序中不受支持）|字符串|指定用于将数据发送到服务器的字符集。<br /><br />可能的值为 SQLSRV_ENC_CHAR 和 UTF-8。 有关详细信息，请参阅[操作说明：使用内置 UTF-8 支持发送和检索 UTF-8 数据](how-to-send-and-retrieve-utf-8-data-using-built-in-utf-8-support.md)。|SQLSRV_ENC_CHAR|  
|ColumnEncryption|以下字符串之一：<br /><br />**已启用**<br /><br />**已禁用**<br /><br />用于标识证明协议和 URL 来证明 enclave 的字符串|指定 Always Encrypted 功能是否已启用。 如果证明协议和 URL 已指定，表明具有安全 Enclave 的 Always Encrypted 已启用，前提是满足[此处](always-encrypted-secure-enclaves.md)所述的其他要求。|已禁用|  
|ConnectionPooling|1 或 **true** ，表示启用连接池。<br /><br />0 或 **false** ，表示禁用连接池。|指定是否从连接池分配连接（1 或 true 表示是，0 或 false 表示否）   <sup>1</sup>。| true (1)|  
|ConnectRetryCount|介于 0 和 255 之间（含限值）的整数|在连接断开时最多尝试重新建立多少次连接后放弃。 在连接断开时，默认尝试一次重新建立连接。 值 0 表示不会尝试重新建立连接。|1|  
|ConnectRetryInterval|介于 1 和 60 之间（含限值）的整数|尝试重新建立连接的时间间隔（以秒为单位）。 应用程序在检测到连接断开后立即尝试重新连接，然后在等待 `ConnectRetryInterval` 秒后重试。 如果 `ConnectRetryCount` 等于0，则忽略此关键字。|1|  
|数据库|字符串|指定用于建立连接的数据库名称<sup>2</sup>。|供登录时使用的默认数据库。|  
|DecimalPlaces<br /><br />（在 PDO_SQLSRV 驱动程序中不受支持）|介于 0 和 4 之间（含 0 和 4）的整数|指定设置提取的 Money 值格式时的小数位数。<br /><br />仅当 `FormatDecimals` 为 true 时此选项才起作用。 将忽略任何负整数或大于 4 的值。|默认精度和确定位数|
|驱动程序|字符串|指定用于与 SQL Server 进通信的 Microsoft ODBC 驱动程序。<br /><br />可能的值包括：<br />适用于 SQL Server 的 ODBC 驱动程序 17<br />ODBC Driver 13 for SQL Server<br />ODBC Driver 11 for SQL Server（仅限 Windows）。|如果 Driver 关键字未指定，Microsoft Drivers for PHP for SQL Server 会尝试在系统中查找受支持的一个或多个 Microsoft ODBC 驱动程序，从最新版 ODBC 开始查找。| 
|加密|1 或 **true** ，表示启用加密。<br /><br />0 或 **false** ，表示禁用加密。|指定是加密（1 或 true）还是解密（0 或 false）与 SQL Server 的通信<sup>3</sup>   。| false (0)|  
|Failover_Partner|字符串|指定要在主服务器不可用时使用的服务器和数据库镜像的实例（如果已启用且已配置）。<br /><br />结合使用 `Failover_Partner` 与 `MultiSubnetFailover` 存在一些限制。 有关详细信息，请参阅 [对高可用性和灾难恢复的支持](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。<br /><br />Linux 或 macOS 不支持此选项，因为 Linux 或 macOS 上的 ODBC 驱动程序不支持数据库镜像。 请改用 AlwaysOn 可用性组，并设置 `MultiSubnetFailover` 和 `TransparentNetworkIPResolution` 选项。|未设置。|
|FormatDecimals<br /><br />（在 PDO_SQLSRV 驱动程序中不受支持）|1 或 true  （为提取的十进制字符串设置格式）。<br /><br />0 或 false  （默认十进制格式设置行为）。|指定是否在合适时向十进制字符串添加前导零，并启用用于设置 Money 类型格式的 `DecimalPlaces` 选项。 如果保留 false，使用的默认行为是返回精确的精度，并为小于 1 的值省略前导零。<br /><br />有关详细信息，请参阅[设置十进制字符串和 Money 值格式](formatting-decimals-sqlsrv-driver.md)。| false (0)|
|KeyStoreAuthentication|**KeyVaultPassword**<br /><br />**KeyVaultClientSecret**|用于访问 Azure Key Vault 的身份验证方法。 控制与 `KeyStorePrincipalId` 和 `KeyStoreSecret` 一起使用的凭据类型。 有关详细信息，请参阅[使用 Azure Key Vault](using-always-encrypted-php-drivers.md#using-azure-key-vault)。|未设置。|
|KeyStorePrincipalId|字符串|试图访问 Azure Key Vault 的帐户的标识符。 <br /><br />如果 `KeyStoreAuthentication` 是 KeyVaultPassword  ，此必须为 Azure Active Directory 用户名。 <br /><br />如果 `KeyStoreAuthentication` 是 KeyVaultClientSecret  ，此必须为应用程序客户端 ID。|未设置。|
|KeyStoreSecret|字符串|试图访问 Azure Key Vault 的帐户的凭据密码。 <br /><br />如果 `KeyStoreAuthentication` 是 KeyVaultPassword  ，此必须为 Azure Active Directory 密码。 <br /><br />如果 `KeyStoreAuthentication` 是 KeyVaultClientSecret  ，此必须为应用程序客户端密码。|未设置。|
|语言|字符串|指定服务器返回的消息的语言。 `sys.syslanguages` 表中列出了可用语言。 <br /><br />此选项既不会影响驱动程序本身使用的语言，因为驱动程序目前只有英语版，也不会影响基础 ODBC 驱动程序的语言，因为驱动程序的语言是由客户端系统上安装的本地化版本确定。 因此，更改此设置可能会导致消息以不同的语言返回，具体视来自 PHP 驱动程序、ODBC 驱动程序还是 SQL Server 而定。|默认为 SQL Server 中设置的语言。|
|LoginTimeout|整数（SQLSRV 驱动程序）<br /><br />字符串（PDO_SQLSRV 驱动程序）|指定在连接尝试失败前等待的秒数。|无超时。|  
|MultipleActiveResultSets|1 或 **true** ，表示使用多个活动的结果集。<br /><br />0 或 **false** ，表示禁用多个活动的结果集。|禁用或显式启用支持多重活动结果集 (MARS)。<br /><br />有关详细信息，请参阅[操作说明：禁用多重活动结果集 &#40;MARS&#41;](how-to-disable-multiple-active-resultsets-mars.md)。|true (1)|  
|MultiSubnetFailover|字符串|在连接到 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 可用性组侦听器或 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 故障转移群集实例的可用性组侦听器时，应始终指定 `multiSubnetFailover=yes`。 `multiSubnetFailover=yes` 配置 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]，以提供对（当前）活动服务器的更快检测和连接。 可能的值为 Yes 和 No。<br /><br />若要详细了解 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 对 [!INCLUDE[ssHADR](../../includes/sshadr_md.md)] 的支持，请参阅[高可用性和灾难恢复支持](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|否|  
|PWD<br /><br />（在 PDO_SQLSRV 驱动程序中不受支持）|字符串|指定与使用 SQL Server 身份验证进行连接时使用的用户 ID 关联的密码<sup>4</sup>。|未设置。|  
|QuotedId|1 或 true，表示使用 SQL-92 规则  。<br /><br />0 或 **false** ，表示使用旧规则。|指定对带引号的标识符是使用 SQL-92 规则（1 或 true）还是使用旧的 Transact-SQL 规则（0 或 false）   。| true (1)|  
|ReturnDatesAsStrings<br /><br />（在 PDO_SQLSRV 驱动程序中不受支持）|1 或 **true** ，表示以字符串的形式返回日期和时间类型。<br /><br />0 或 **false** ，表示以 PHP **DateTime** 的形式返回日期和时间类型。|以字符串或 PHP 类型的形式检索日期和时间类型（datetime、smalldatetime、date、time、datetime2 和 datetimeoffset）。 有关详细信息，请参阅[操作说明：使用 SQLSRV 驱动程序检索日期和时间类型作为字符串](how-to-retrieve-date-and-time-type-as-strings-using-the-sqlsrv-driver.md)。 <br /><br />如果使用的是 PDO_SQLSRV 驱动程序，除非另行指定，否则日期以字符串的形式返回。 有关详细信息，请参阅[操作说明：使用 PDO_SQLSRV 驱动程序检索日期和时间类型作为 PHP Datetime 对象](how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)。|**false**|
|可滚动|字符串|“缓冲”表示你希望使用客户端（缓冲）游标，这将允许你将整个结果集缓存到内存。 有关详细信息，请参阅[游标类型（SQLSRV 驱动程序）](cursor-types-sqlsrv-driver.md)。|只进游标|  
|服务器<br /><br />（在 SQLSRV 驱动程序中不受支持）|字符串|要连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。<br /><br />你还可以指定要连接到 AlwaysOn 可用性组的虚拟网络名称。 若要详细了解 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 对 [!INCLUDE[ssHADR](../../includes/sshadr_md.md)] 的支持，请参阅[高可用性和灾难恢复支持](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。|Server 是必需的关键字（尽管它不必是连接字符串中的第一个关键字）。 如果服务器名称未传递给该关键字，将尝试连接到本地实例。<br /><br />传递给 Server 的值可以是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称，也可以是该实例的 IP 地址。 你可以选择指定一个端口号（例如 `sqlsrv:server=(local),1033`）。<br /><br />从 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 的版本 3.0 开始，你还可以指定带有 `server=(localdb)\instancename`的 LocalDB 实例。 有关详细信息，请参阅 [LocalDB 支持](php-driver-for-sql-server-support-for-localdb.md)。|  
|TraceFile|字符串|指定用于跟踪数据的文件的路径。|未设置。|  
|TraceOn|1 或 **true** ，表示启用跟踪。<br /><br />0 或 **false** ，表示禁用跟踪。|指定对正在建立的连接是启用（1 或 true）还是禁用（0 或 false）ODBC 跟踪   。| false (0)|  
|TransactionIsolation|SQLSRV 驱动程序使用以下值：<br /><br />SQLSRV_TXN_READ_UNCOMMITTED<br /><br />SQLSRV_TXN_READ_COMMITTED<br /><br />SQLSRV_TXN_REPEATABLE_READ<br /><br />SQLSRV_TXN_SNAPSHOT<br /><br />SQLSRV_TXN_SERIALIZABLE<br /><br />PDO_SQLSRV 驱动程序使用以下值：<br /><br />PDO::SQLSRV_TXN_READ_UNCOMMITTED<br /><br />PDO::SQLSRV_TXN_READ_COMMITTED<br /><br />PDO::SQLSRV_TXN_REPEATABLE_READ<br /><br />PDO::SQLSRV_TXN_SNAPSHOT<br /><br />PDO::SQLSRV_TXN_SERIALIZABLE|指定事务隔离级别。<br /><br />有关事务隔离的详细信息，请参阅 SQL Server 文档中的 [设置事务隔离级别](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。|SQLSRV_TXN_READ_COMMITTED<br /><br />或<br /><br />PDO::SQLSRV_TXN_READ_COMMITTED|  
|TransparentNetworkIPResolution|  启用或禁用|如果第一个解析的主机名 IP 未响应，且存在多个与主机名关联的 IP，就会影响连接序列。<br /><br />它与 `MultiSubnetFailover` 交互，以提供不同的连接序列。 有关详细信息，请参阅[透明网络 IP 解析](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)或[使用透明网络 IP 解析](../odbc/using-transparent-network-ip-resolution.md)。|已启用|
|TrustServerCertificate|1 或 **true** ，表示信任证书。<br /><br />0 或 **false** ，表示不信任证书。|指定客户端是应信任（1 或 true）还是应拒绝（0 或 false）自签名的服务器证书   。| false (0)|  
|UID<br /><br />（在 PDO_SQLSRV 驱动程序中不受支持）|字符串|指定在使用 SQL Server 身份验证进行连接时要使用的用户 ID<sup>4</sup>。|未设置。|  
|WSID|字符串|指定用于跟踪的计算机的名称。|未设置。|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |

1. 在 Linux 和 macOS 中，不能使用 `ConnectionPooling` 属性来启用/禁用连接池。 请参阅[连接池 (Microsoft Drivers for PHP for SQL Server)](connection-pooling-microsoft-drivers-for-php-for-sql-server.md)。

2. 所有在已建立的连接上执行的查询都是针对 `Database` 属性指定的数据库进行。 不过，如果用户具有相应权限，则可以使用完全限定的名称访问其他数据库中的数据。 例如，如果 master  数据库使用 `Database` 连接属性进行设置，仍可以使用完全限定的名称执行可访问 AdventureWorks.HumanResources.Employee  表的 Transact-SQL 查询。  

3. 因为加密数据需要计算开销，所以启用 `Encryption` 可能会影响某些应用程序的性能。  

4. 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接时，必须同时设置 `UID` 和 `PWD` 属性。  

许多支持的键都是 ODBC 连接字符串属性。 有关 ODBC 连接字符串的信息，请参阅 [将连接字符串关键字用于 SQL Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。

## <a name="see-also"></a>另请参阅  
[连接到服务器](connecting-to-the-server.md)  
