---
title: 通过 SqlClient 使用 Azure Active Directory 身份验证
description: 介绍如何通过 SqlClient 使用受支持的 Azure Active Directory 身份验证模式连接到 Azure SQL 数据源
ms.date: 11/20/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: karinazhou
ms.author: v-jizho2
ms.reviewer: v-daenge
ms.openlocfilehash: c57c2d10854ed902a6230eafc3a912cd0508c989
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835984"
---
# <a name="using-azure-active-directory-authentication-with-sqlclient"></a>通过 SqlClient 使用 Azure Active Directory 身份验证

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

本文介绍如何通过 SqlClient 从 .NET 应用程序使用 Azure Active Directory (Azure AD) 身份验证连接到 Azure SQL 数据源。

Azure AD 身份验证使用 Azure AD 中的标识访问 Azure SQL 数据源（例如 Azure SQL 数据库、Azure SQL 托管实例和 Azure Synapse Analytics）。 利用 Microsoft.Data.SqlClient 命名空间，客户端应用程序可以在连接到 Azure SQL 数据库时在不同的身份验证模式下指定 Azure AD 凭据。

设置连接字符串中的 `Authentication` 连接属性时，客户端可以根据提供的值选择首选 Azure AD 身份验证模式：

- 最早的 Microsoft.Data.SqlClient 版本支持在 .NET Framework、.NET Core 和 .NET Standard 上使用 `Active Directory Password`。 它还支持在 .NET Framework 上使用 `Active Directory Integrated` 身份验证和 `Active Directory Interactive` 身份验证。 

- 从 Microsoft.Data.SqlClient 2.0.0 开始，对 `Active Directory Integrated` 身份验证和 `Active Directory Interactive` 身份验证的支持已扩展到 .NET Framework、.NET Core 和 .NET Standard。 

  在 SqlClient 2.0.0 中还添加了新的 `Active Directory Service Principal` 身份验证模式。 它利用客户端 ID 和服务主体标识的机密来完成身份验证。 

- Microsoft.Data.SqlClient 2.1.0 中添加了更多身份验证模式，包括 `Active Directory Device Code Flow` 和 `Active Directory Managed Identity`（也称为 `Active Directory MSI`）。 这些新模式允许应用程序获取连接到服务器的访问令牌。 

有关 Azure AD 身份验证的更多信息（除以下各节介绍的信息外），请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/azure-sql/database/authentication-aad-overview)。

## <a name="setting-azure-active-directory-authentication"></a>设置 Azure Active Directory 身份验证

应用程序使用 Azure AD 身份验证连接到 Azure SQL 数据源时需要提供有效的身份验证模式。 下表列出了支持的身份验证模式。 应用程序通过使用连接字符串中的 `Authentication` 连接属性指定一种模式。

| 值 | 描述  | 框架    | Microsoft.Data.SqlClient 版本 |
|:--|:--|:--|:--:|
| Active Directory 密码 | 使用用户名和密码对 Azure AD 标识进行身份验证 | .NET Framework 4.6+、.NET Core 2.1+、.NET Standard 2.0+  | 1.0+|
| Active Directory 已集成 |使用集成身份验证对 Azure AD 标识进行身份验证 | .NET Framework 4.6+、.NET Core 2.1+、.NET Standard 2.0+ | 2.0.0+<sup>1</sup> |
| Azure Active 交互式 | 使用交互式身份验证对 Azure AD 标识进行身份验证 | .NET Framework 4.6+、.NET Core 2.1+、.NET Standard 2.0+ | 2.0.0+<sup>1</sup> |
| Active Directory 服务主体 | 使用客户端 ID 和服务主体标识的机密对 Azure AD 标识进行身份验证 | .NET Framework 4.6+、.NET Core 2.1+、.NET Standard 2.0+ | 2.0.0+ |
| Active Directory 设备代码流 | 使用设备代码流模式对 Azure AD 标识进行身份验证 | .NET Framework 4.6+、.NET Core 2.1+、.NET Standard 2.0+ | 2.1.0+ |
| Active Directory 托管标识、 <br>Active Directory MSI | 使用系统分配或用户分配的托管标识对 Azure AD 标识进行身份验证 | .NET Framework 4.6+、.NET Core 2.1+、.NET Standard 2.0+ | 2.1.0+ |

<sup>1</sup> 在 Microsoft.Data.SqlClient 2.0.0 之前，`Active Directory Integrated` 和 `Active Directory Interactive` 身份验证模式仅在 .NET Framework 4.6 或更高版本上受支持。

## <a name="using-active-directory-password-authentication"></a>使用 Active Directory 密码身份验证

`Active Directory Password` 身份验证模式支持本机或联合 Azure AD 用户使用 Azure AD 对访问 Azure 数据源进行身份验证。 使用此模式时，必须在连接字符串中提供用户凭据。 下面的示例演示如何使用 `Active Directory Password` 身份验证。

```c#
// Use your own server, database, user ID, and password.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Password; Database=testdb; User Id=user@domain.com; Password=***";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-integrated-authentication"></a>使用 Active Directory 集成身份验证

要使用 `Active Directory Integrated` 身份验证模式，需要将本地 Active Directory 实例与云中的 Azure AD 进行联合。 例如，可以使用 Active Directory 联合身份验证服务 (AD FS) 进行联合。

在此模式下登录到加入域的计算机时，可以访问 Azure SQL 数据源，而无需提供凭据。 不能在 .NET Framework 应用程序的连接字符串中指定用户名和密码。 在 .NET Core 和 .NET Standard 应用程序的连接字符串中，用户名是可选的。 在此模式下，不能设置 SqlConnection 的 `Credential` 属性。 

以下代码片段是使用 `Active Directory Integrated` 身份验证时的一个示例。

```c#
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is optional for .NET Core and .NET Standard.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-interactive-authentication"></a>使用 Active Directory 交互式身份验证

`Active Directory Interactive` 身份验证支持用于连接到 Azure SQL 数据源的多重身份验证技术。 如果在连接字符串中提供此身份验证模式，则将显示 Azure 身份验证屏幕，并要求用户输入有效凭据。 不能在连接字符串中指定密码。 

在此模式下，不能设置 SqlConnection 的 `Credential` 属性。 在 Microsoft.Data.SqlClient 2.0.0 和更高版本中，在交互模式下，允许在连接字符串中使用用户名。 

下面的示例演示如何使用 `Active Directory Interactive` 身份验证。

```c#
// Use your own server, database, and user ID.
// User ID is optional.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is not provided.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-service-principal-authentication"></a>使用 Active Directory 服务主体身份验证

在 `Active Directory Service Principal` 身份验证模式下，客户端应用程序可通过提供客户端 ID 和服务主体标识的机密来连接到 Azure SQL 数据源。 服务主体身份验证涉及：
1. 使用机密设置应用注册。
1. 向 Azure SQL 数据库实例中的应用授予权限。
1. 使用正确的凭据连接。 

下面的示例演示如何使用 `Active Directory Service Principal` 身份验证。

```c#
// Use your own server, database, app ID, and secret.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Service Principal; Database=testdb; User Id=AppId; Password=secret";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-device-code-flow-authentication"></a>使用 Active Directory 设备代码流身份验证

在适用于 .NET 的 [Microsoft 身份验证库](/azure/active-directory/develop/msal-overview) (MSAL.NET) 中，`Active Directory Device Code Flow` 身份验证使客户端应用程序可以从不具备交互式 Web 浏览器的设备和操作系统连接到 Azure SQL 数据源。 将在另一台设备上执行交互式身份验证。 有关设备代码流身份验证的详细信息，请参阅 [OAuth 2.0 设备代码流](/azure/active-directory/develop/v2-oauth2-device-code)。 

在此模式下，不能设置 `SqlConnection` 的 `Credential` 属性。 此外，不能在连接字符串中指定用户名和密码。 

以下代码片段是使用 `Active Directory Device Code Flow` 身份验证的一个示例。

```c#
// Use your own server and database.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Device Code Flow; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-managed-identity-authentication"></a>使用 Active Directory 托管标识身份验证

Azure 资源的托管标识是以前称为托管服务标识 (MSI) 服务的新名称。 当客户端应用程序使用 Azure 资源访问支持 Azure AD 身份验证的 Azure 服务时，可以使用托管标识进行身份验证，方法是为 Azure AD 中的 Azure 资源提供标识。 然后，可以使用该标识来获取访问令牌。 此身份验证方法不需管理凭据和密码。

托管标识分为两种类型：

- 系统分配的托管标识是在 Azure AD 中的服务实例上创建的。 它与该服务实例的生命周期相关联。 
- 用户分配的托管标识是作为独立的 Azure 资源创建的。 可以将其分配给 Azure 服务的一个或多个实例。 

有关托管标识的详细信息，请参阅[关于 Azure 资源的托管标识](/azure/active-directory/managed-identities-azure-resources/overview)。

自 Microsoft.Data.SqlClient 2.1.0 起，驱动程序支持通过托管标识获取访问令牌，实现对 Azure SQL 数据库、Azure Synapse Analytics 和 Azure SQL 托管实例进行身份验证。 若要使用此身份验证，请在连接字符串中指定 `Active Directory Managed Identity`或 `Active Directory MSI`，无需提供密码。 

在此模式下，也不能设置 `SqlConnection` 的 `Credential` 属性。 对于用户分配的托管标识，必须提供用户名。 

下面的示例演示如何使用系统分配的托管标识进行 `Active Directory Managed Identity` 身份验证。

```c#
// For system-assigned managed identity
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```

下面的示例演示使用系统分配的托管标识的 `Active Directory Managed Identity`身份验证。

```c#
// For user-assigned managed identity
// Use your own server, database, and user ID.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="customizing-active-directory-authentication"></a>自定义 Active Directory 身份验证

除了使用驱动程序中内置的 Active Directory 身份验证外，Microsoft.Data.SqlClient 2.1.0 和更高版本还为应用程序提供了自定义 Active Directory 身份验证的选项。 自定义基于 `ActiveDirectoryAuthenticationProvider` 类，该类派生自 [`SqlAuthenticationProvider`](/dotnet/api/system.data.sqlclient.sqlauthenticationprovider) 抽象类。 

在 Active Directory 身份验证期间，客户端应用程序可以通过以下任一方法定义其自己的 `ActiveDirectoryAuthencationProvider` 类：

- 使用自定义的回调方法。
- 通过 SqlClient 驱动程序将应用程序客户端 ID 传递到 MSAL 库，用于获取访问令牌。

下面的示例演示如何在使用 `Active Directory Device Code Flow` 身份验证时使用自定义回调。 

[!code-csharp [AADAuthenticationCustomDeviceFlowCallback#1](~/../sqlclient/doc/samples/AADAuthenticationCustomDeviceFlowCallback.cs#1)]

使用自定义的 `ActiveDirectoryAuthenticationProvider` 类时，如果正在使用支持的 Active Directory 身份验证模式，则可以将用户定义的应用程序客户端 ID 传递到 SqlClient。 支持的 Active Directory 身份验证模型包括 `Active Directory Password`、`Active Directory Integrated`、`Active Directory Interactive`、`Active Directory Service Principal` 和 `Active Directory Device Code Flow`。

应用程序客户端 ID 还可通过 `SqlAuthenticationProviderConfigurationSection` 或 `SqlClientAuthenticationProviderConfigurationSection` 进行配置。 配置属性 `applicationClientId` 适用于 .NET Framework 4.6 + 和 .NET Core 2.1 +。

以下代码片段是在使用 `Active Directory Interactive` 身份验证时使用自定义的 `ActiveDirectoryAuthenticationProvider` 类和用户定义的应用程序客户端 ID 的示例。

[!code-csharp [ApplicationClientIdAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/ApplicationClientIdAzureAuthenticationProvider.cs#1)]

以下示例演示如何通过配置节设置应用程序客户端 ID。

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

## <a name="support-for-a-custom-sql-authentication-provider"></a>支持自定义 SQL 身份验证提供程序

获得更多灵活性后，客户端应用程序还可以使用自己的提供程序进行 Active Directory 身份验证，而不是使用 `ActiveDirectoryAuthenticationProvider` 类。 自定义身份验证提供程序需要是具有重写方法的 `SqlAuthenticationProvider` 的子类。 

下面的示例演示如何使用新的身份验证提供程序进行 `Active Directory Device Code Flow` 身份验证。

[!code-csharp [CustomDeviceCodeFlowAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/CustomDeviceCodeFlowAzureAuthenticationProvider.cs#1)]

除了改进 `Active Directory Interactive` 身份验证体验之外，Microsoft.Data.SqlClient 2.1.0 和更高版本还为客户端应用程序提供了以下 API，用于自定义交互式身份验证和设备代码流身份验证。

```c#
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    // Sets a reference to the current System.Windows.Forms.IWin32Window that triggers the browser to be shown. 
    // Used to center the browser pop-up onto this window.
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    // Sets a reference to the ViewController (if using Xamarin.iOS), Activity (if using Xamarin.Android) IWin32Window, or IntPtr (if using .NET Framework). 
    // Used for invoking the browser for Active Directory Interactive authentication.
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Sets a callback method that's invoked with a custom web UI instance that will let the user sign in with Azure AD, present consent if needed, and get back the authorization code. 
    // Applicable when working with Active Directory Interactive authentication.
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Clears cached user tokens from the token provider.
    public static void ClearUserTokenCache();
}
```


## <a name="see-also"></a>另请参阅
- [Azure Active Directory 中的应用程序对象和服务主体对象](/azure/active-directory/develop/app-objects-and-service-principals)
- [身份验证流](/azure/active-directory/develop/msal-authentication-flows)
