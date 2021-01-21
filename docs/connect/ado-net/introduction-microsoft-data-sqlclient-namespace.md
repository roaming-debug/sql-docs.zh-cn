---
title: Microsoft.Data.SqlClient 命名空间简介
description: 了解 Microsoft.Data.SqlClient 命名空间，以及为何将其作为首选方式连接到 SQL for .NET 应用程序。
ms.date: 11/19/2020
ms.assetid: c18b1fb1-2af1-4de7-80a4-95e56fd976cb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: e966e4f2f43ebe546d6baa0b757f682f3eca205b
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596366"
---
# <a name="introduction-to-microsoftdatasqlclient-namespace"></a>Microsoft.Data.SqlClient 命名空间简介

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="release-notes-for-microsoftdatasqlclient-21"></a>Microsoft.Data.SqlClient 2.1 发行说明

GitHub 存储库中也提供了发行说明：[2.1 发行说明](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.1)。

### <a name="new-features"></a>新增功能

### <a name="cross-platform-support-for-always-encrypted"></a>对 Always Encrypted 的跨平台支持
Microsoft.Data.SqlClient v2.1 扩展了对以下平台的 Always Encrypted 支持：

| 支持 Always Encrypted | 支持具有安全 Enclave 的 Always Encrypted  | 目标 Framework | Microsoft.Data.SqlClient 版本 | 操作系统 |
|:--|:--|:--|:--:|:--:|
| 是 | 是 | .NET Framework 4.6 及更高版本 | 1.1.0+ | Windows |
| 是 | 是 | .NET Core 2.1+ | 2.1.0+<sup>1</sup> | Windows、Linux、macOS |
| 是 | 否<sup>2</sup> | .NET Standard 2.0 | 2.1.0+ | Windows、Linux、macOS |
| 是 | 是 | .NET Standard 2.1+ | 2.1.0+ | Windows、Linux、macOS |

> [!NOTE]
> <sup>1</sup> 在 Microsoft.Data.SqlClient 版本 v2.1 之前，Always Encrypted 仅在 Windows 上受支持。
> <sup>2</sup> 具有安全 Enclave 的 Always Encrypted 在 .NET Standard 2.0 上不受支持。

### <a name="azure-active-directory-device-code-flow-authentication"></a>Azure Active Directory 设备代码流身份验证
Microsoft.Data.SqlClient v2.1 支持使用 MSAL.NET 进行“设备代码流”身份验证。
参考文档：[OAuth2.0 设备授权授权流](/azure/active-directory/develop/v2-oauth2-device-code)

连接字符串示例：

`Server=<server>.database.windows.net; Authentication=Active Directory Device Code Flow; Database=Northwind;`

以下 API 支持自定义设备代码流回调机制：

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetDeviceCodeFlowCallback(Func<DeviceCodeResult, Task> deviceCodeFlowCallbackMethod)
}
```

### <a name="azure-active-directory-managed-identity-authentication"></a>Azure Active Directory 托管标识身份验证
Microsoft.Data.SqlClient v2.1 支持使用[托管标识](/azure/active-directory/managed-identities-azure-resources/overview)进行 Azure Active Directory 身份验证。

支持以下身份验证模式关键字：
- Active Directory 托管标识
- Active Directory MSI（用于跨 MS SQL 驱动程序兼容性）

连接字符串示例：

```cs
// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; Initial Catalog={db};"

// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"
```

### <a name="azure-active-directory-interactive-authentication-enhancements"></a>Azure Active Directory 交互式身份验证增强功能
Microsoft.Data.SqlClient v2.1 添加了以下 API，用于自定义“Active Directory 交互式”身份验证体验：

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void ClearUserTokenCache();
}
```

### <a name="sqlclientauthenticationproviders-configuration-section"></a>`SqlClientAuthenticationProviders` 配置节
Microsoft.Data.SqlClient v2.1 引入了新的配置节 `SqlClientAuthenticationProviders`（现有 `SqlAuthenticationProviders` 的克隆）。 定义了适当的类型时，仍支持现有的配置节 `SqlAuthenticationProviders`，以实现向后兼容性。

新节允许应用程序配置文件同时包含 System.Data.SqlClient 的 SqlAuthenticationProviders 节和 Microsoft.Data.SqlClient 的 SqlClientAuthenticationProviders 节。


### <a name="azure-active-directory-authentication-using-an-application-client-id"></a>使用应用程序客户端 ID 进行 Azure Active Directory 身份验证
Microsoft.Data.SqlClient v2.1 支持将用户定义的应用程序客户端 ID 传递到 Microsoft 身份验证库。 使用 Azure Active Directory 进行身份验证时，使用应用程序客户端 ID。

引入了以下新 API：

1. ActiveDirectoryAuthenticationProvider 中引入了新的构造函数：\
[适用于所有 .NET 平台（.NET Framework、.NET Core 和 .NET Standard）]

```csharp
public ActiveDirectoryAuthenticationProvider(string applicationClientId)
```

用法：
```csharp
string APP_CLIENT_ID = "<GUID>";
SqlAuthenticationProvider customAuthProvider = new ActiveDirectoryAuthenticationProvider(APP_CLIENT_ID);
SqlAuthenticationProvider.SetProvider(SqlAuthenticationMethod.ActiveDirectoryInteractive, customAuthProvider);

using (SqlConnection sqlConnection = new SqlConnection("<connection_string>")
{
    sqlConnection.Open();
}
```

2. 在 `SqlAuthenticationProviderConfigurationSection` 和 `SqlClientAuthenticationProviderConfigurationSection` 下引入了新的配置属性：\
[适用于 .NET Framework 和 .NET Core]

```csharp
internal class SqlAuthenticationProviderConfigurationSection : ConfigurationSection
{
    ...
    [ConfigurationProperty("applicationClientId", IsRequired = false)]
    public string ApplicationClientId => this["applicationClientId"] as string;
}

// Inheritance
internal class SqlClientAuthenticationProviderConfigurationSection : SqlAuthenticationProviderConfigurationSection
{ ... }
```

用法：
```xml
<configuration>
    <configSections>
        <section name="SqlClientAuthenticationProviders"
                         type="Microsoft.Data.SqlClient.SqlClientAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
    </configSections>
    <SqlClientAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>

<!--or-->

<configuration>
    <configSections>
        <section name="SqlAuthenticationProviders"
                         type="Microsoft.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
    </configSections>
    <SqlAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>
```

### <a name="data-classification-v2-support"></a>数据分类 v2 支持
Microsoft.Data.SqlClient v2.1 引入了对数据分类的“敏感度级别”信息的支持。 现推出以下新 API：

```csharp
public class SensitivityClassification
{
    public SensitivityRank SensitivityRank;
}

public class SensitivityProperty
{
    public SensitivityRank SensitivityRank;
}

public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}
```

### <a name="server-process-id-for-an-active-sqlconnection"></a>活动 `SqlConnection` 的服务器进程 ID
Microsoft.Data.SqlClient v2.1 为活动连接引入了新的 `SqlConnection` 属性 `ServerProcessId`。

```csharp
public class SqlConnection
{
    // Returns the server process Id (SPID) of the active connection.
    public int ServerProcessId;
}
```

### <a name="trace-logging-support-in-native-sni"></a>本机 SNI 中的跟踪日志记录支持
Microsoft.Data.SqlClient v2.1 扩展了现有 `SqlClientEventSource` 实现，以支持 SNI.dll 中的事件跟踪。 必须使用类似于 Xperf 的工具捕获事件。

可以通过将命令发送到 `SqlClientEventSource` 来启用跟踪，如下图所示：

```csharp
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```


### <a name="command-timeout-connection-string-property"></a>“命令超时”连接字符串属性
Microsoft.Data.SqlClient v2.1 引入了“命令超时”连接字符串属性来替代默认值 30 秒。 可以使用 SqlCommand 上的 `CommandTimeout` 属性来替代单个命令的超时。

连接字符串示例：

`"Server={serverURL}; Initial Catalog={db}; Integrated Security=true; Command Timeout=60"`

### <a name="removal-of-symbols-from-native-sni"></a>从本机 SNI 中删除符号
在 Microsoft.Data.SqlClient v2.1 中，我们从 [Microsoft.Data.SqlClient.SNI.runtime](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime) NuGet（从 [v2.1.1](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime/2.1.1) 开始）删除了 [v2.0.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI/2.0.0) 中引入的符号。 公共符号现已发布到 Microsoft symbols Server，以供 BinSkim 等需要访问公共符号的工具使用。

### <a name="source-linking-of-microsoftdatasqlclient-symbols"></a>Microsoft.Data.SqlClient 符号的源链接
从 Microsoft.Data.SqlClient v2.1 开始，Microsoft.Data.SqlClient 符号可源链接并发布到 Microsoft Symbols Server，以获得增强的调试体验，而无需下载源代码。


## <a name="release-notes-for-microsoftdatasqlclient-20"></a>Microsoft.Data.SqlClient 2.0 发行说明

GitHub 存储库中也提供了发行说明：[2.0 发行说明](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.0)。

### <a name="breaking-changes"></a>中断性变更

- Enclave 提供程序接口 `SqlColumnEncryptionEnclaveProvider` 的访问修饰符已从 `public` 更改为 `internal`。

- `SqlClientMetaDataCollectionNames` 类中的常量已更新，以反映 SQL Server 中的更改。

- 驱动程序现在将在目标 SQL Server 强制执行 TLS 加密时执行服务器证书验证，这是 Azure 连接的默认设置。

- `SqlDataReader.GetSchemaTable()` 现在返回空 `DataTable`，而不是 `null`。

- 驱动程序现执行十进制小数位舍入以匹配 SQL Server 行为。 为实现后向兼容性，可使用 AppContext 开关启用之前的截断行为。

- 对于使用 Microsoft.Data.SqlClient 的 .NET Framework 应用程序，之前下载到 `bin\x64` 和 `bin\x86` 文件夹的 SNI.dll 文件现命名为 `Microsoft.Data.SqlClient.SNI.x64.dll` 和 ` Microsoft.Data.SqlClient.SNI.x86.dll` 并将下载到 `bin` 目录。

- 从 `SqlConnectionStringBuilder` 获取连接字符串时，新的连接字符串属性同义词将替换旧的属性，以确保一致性。 [阅读详细信息](#new-connection-string-property-synonyms)

### <a name="new-features"></a>新增功能

#### <a name="dns-failure-resiliency"></a>DNS 故障复原

驱动程序现会将 IP 地址从每个成功连接缓存到支持该功能的 SQL Server 终结点。 如果在连接尝试期间发生 DNS 解析失败，驱动程序将尝试使用该服务器的已缓存 IP 地址（如存在）建立连接。

#### <a name="eventsource-tracing"></a>EventSource 跟踪

此版本引入了对捕获用于调试应用程序的事件跟踪日志的支持。 若要捕获这些事件，客户端应用程序必须侦听来自 SqlClient 的 EventSource 实现的事件：

```
Microsoft.Data.SqlClient.EventSource
```

有关详细信息，请参阅如何[在 SqlClient 中启用事件跟踪](enable-eventsource-tracing.md)。

#### <a name="enabling-managed-networking-on-windows"></a>在 Windows 上启用托管网络

新的 AppContext 开关“Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows”支持在 Windows 上使用托管的 SNI 实现进行测试和调试。 此开关将切换驱动程序的行为，以便在 Windows 上的 .NET Core 2.1+ 和 .NET Standard 2.0+ 项目中使用托管的 SNI，从而消除对 Microsoft.Data.SqlClient 库的本机库的所有依赖项。

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

有关驱动程序中可用开关的完整列表，请参阅 [SqlClient 中的 AppContext 开关](appcontext-switches.md)。

#### <a name="enabling-decimal-truncation-behavior"></a>启用小数截断行为

默认情况下，驱动程序将舍入十进制数据小数位，这与 SQL Server 一样。 为实现后向兼容性，可将 AppContext 开关“Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal”设置为“true” 。

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

#### <a name="new-connection-string-property-synonyms"></a>新的连接字符串属性同义词

已为以下现有的连接字符串属性添加了新的同义词，避免在包含多个字词的属性周围出现间距混淆。 旧属性名称将继续得到支持以实现后向兼容性，但在从 [SqlConnectionStringBuilder](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder) 中提取连接字符串时，新的连接字符串属性现在将包括在内。

|现有的连接字符串属性|新的同义词|
|-----------------------------------|-----------|
| ApplicationIntent | 应用程序意向 |
| ConnectRetryCount | 连接重试计数 |
| ConnectRetryInterval | 连接重试间隔 |
| PoolBlockingPeriod | 池阻塞期 |
| MultipleActiveResultSets | 多重活动结果集 |
| MultiSubnetFailover | 多重子网故障转移 |
| TransparentNetworkIPResolution | 透明网络 IP 解析 |
| TrustServerCertificate | 信任服务器证书 |

#### <a name="sqlbulkcopy-rowscopied-property"></a>SqlBulkCopy RowsCopied 属性

RowsCopied 属性提供对已在正在进行的大容量复制操作中处理的行数的只读访问。 此值不一定等于添加到目标表中的最终行数。

#### <a name="connection-open-overrides"></a>连接打开替代

可替代 SqlConnection.Open() 的默认行为，以禁用由暂时性错误触发的 10 秒延迟和自动连接重试。

```csharp
using SqlConnection sqlConnection = new SqlConnection("Data Source=(local);Integrated Security=true;Initial Catalog=AdventureWorks;");
sqlConnection.Open(SqlConnectionOverrides.OpenWithoutRetry);
```

> [!NOTE]
> 请注意，此替代只能应用于 SqlConnection.Open()，而不能应用于 SqlConnection.OpenAsync()。

#### <a name="username-support-for-active-directory-interactive-mode"></a>对 Active Directory 交互模式的用户名支持

对 .NET Framework 和 .NET Core 使用 Azure Active Directory 交互身份验证模式时，可在连接字符串中指定用户名

使用“用户 ID”或“UID”连接字符串属性设置用户名 ：

```
"Server=<server name>; Database=<db name>; Authentication=Active Directory Interactive; User Id=<username>;"
```

#### <a name="order-hints-for-sqlbulkcopy"></a>SqlBulkCopy 的排序提示

可提供排序提示，提高对具有聚集索引的表进行大容量复制操作的性能。 有关详细信息，请参阅[大容量复制操作](sql/bulk-copy-order-hints.md)一节。

#### <a name="sni-dependency-changes"></a>SNI 依赖项更改

Windows 上的 Microsoft.Data.SqlClient（.NET Core 和 .NET Standard）现依赖于 Microsoft.Data.SqlClient.SNI.runtime，取代了先前对 runtime.native.System.Data.SqlClient.SNI 的依赖 。 新的依赖项增加了对 ARM 平台以及 Windows 上已受支持的 ARM64、x64 和 x86 平台的支持。

## <a name="release-notes-for-microsoftdatasqlclient-110"></a>Microsoft.Data.SqlClient 1.1.0 的发行说明

GitHub 存储库中也提供了发行说明：[1.1 发行说明](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1)。

### <a name="new-features"></a>新增功能

#### <a name="always-encrypted-with-secure-enclaves"></a>具有安全 Enclave 的 Always Encrypted

从 Microsoft SQL Server 2016 开始，提供 Always Encrypted。 从 Microsoft SQL Server 2019 开始，提供安全 enclave。 为使用 Enclave 功能，连接字符串应包括所需的证明协议和证明 URL。 例如：

```
Attestation Protocol=HGS;Enclave Attestation Url=<attestation_url_for_HGS>
```

有关详细信息，请参阅：

- [对 Always Encrypted 的 SqlClient 支持](sql/sqlclient-support-always-encrypted.md)
- [教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)

## <a name="release-notes-for-microsoftdatasqlclient-10"></a>Microsoft.Data.SqlClient 1.0 的发行说明

Microsoft.Data.SqlClient 命名空间的初始版本提供了现有 System.Data.SqlClient 命名空间之外的其他功能。
GitHub 存储库中也提供了发行说明：[1.0 发行说明](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.0)。

### <a name="new-features"></a>新增功能

#### <a name="new-features-over-net-framework-472-systemdatasqlclient"></a>通过 .NET Framework 4.7.2 System.Data.SqlClient 提供的新增功能

- **数据分类** - 在 Azure SQL 数据库和 Microsoft SQL Server 2019 中提供。

- **UTF-8 支持** - 在 Microsoft SQL Server 2019 中提供。

#### <a name="new-features-over-net-core-22-systemdatasqlclient"></a>通过 .NET Core 2.2 System.Data.SqlClient 提供的新增功能

- **数据分类** - 在 Azure SQL 数据库和 Microsoft SQL Server 2019 中提供。

- **UTF-8 支持** - 在 Microsoft SQL Server 2019 中提供。

- **身份验证** - Active Directory 密码身份验证模式。

### <a name="data-classification"></a>数据分类

当基础源支持该功能并包含有关[数据敏感度和分类](../../relational-databases/security/sql-data-discovery-and-classification.md)的元数据时，数据分类会引入一组新的 API，公开有关通过 SqlDataReader 检索到的对象的只读数据敏感度和分类信息。 有关示例应用程序，请参阅 [SqlClient 中的数据发现和分类](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1)。

```csharp
public class SqlDataReader
{
    public Microsoft.Data.SqlClient.DataClassification.SensitivityClassification SensitivityClassification
}

namespace Microsoft.Data.SqlClient.DataClassification
{
    public class ColumnSensitivity
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.SensitivityProperty> SensitivityProperties
    }
    public class InformationType
    {
        public string Id
        public string Name
    }
    public class Label
    {
        public string Id
        public string Name
    }
    public class SensitivityClassification
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.ColumnSensitivity> ColumnSensitivities
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.InformationType> InformationTypes
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.Label> Labels
    }
    public class SensitivityProperty
    {
        public Microsoft.Data.SqlClient.DataClassification.InformationType InformationType
        public Microsoft.Data.SqlClient.DataClassification.Label Label
    }
}
```

### <a name="utf-8-support"></a>UTF-8 支持

UTF-8 支持不需要更改任何应用程序代码。 当服务器支持 UTF-8 且基础列排序规则为 UTF-8 时，这些 SqlClient 更改会优化客户端与服务器之间的通信。 请参阅 [SQL Server 2019 的新增功能](../../sql-server/what-s-new-in-sql-server-ver15.md)中的 UTF-8 部分。

### <a name="always-encrypted-with-enclaves"></a>具有 enclave 的 Always Encrypted

通常，使用 .NET Framework 上的 System.Data.SqlClient 和内置列主密钥存储提供程序的现有文档现在还应与 .NET Core 一起使用。

 [配合使用 Always Encrypted 和 .NET Framework 数据提供程序进行开发](../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)

 [Always Encrypted：保护敏感数据并将加密密钥存储在 Windows 证书存储中](/azure/sql-database/sql-database-always-encrypted)

### <a name="authentication"></a>身份验证

可以使用“身份验证”连接字符串选项来指定不同的身份验证模式。 有关详细信息，请参阅 [SqlAuthenticationMethod 的文档](/dotnet/api/system.data.sqlclient.sqlauthenticationmethod?view=netframework-4.7.2&preserve-view=true)。

> [!NOTE]
> 自定义密钥存储提供程序（如 Azure 密钥保管库提供程序）将需要更新以支持 Microsoft.Data.SqlClient。 同样，还需要更新 enclave 提供程序以支持 Microsoft.Data.SqlClient。
> 仅 .NET Framework 和 .NET Core 目标支持 Always Encrypted。 .NET Standard 不支持 Always Encrypted，因为 .NET Standard 缺少某些加密依赖项。