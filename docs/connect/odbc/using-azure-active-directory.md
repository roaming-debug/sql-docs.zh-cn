---
title: 结合使用 Azure Active Directory 和 ODBC 驱动程序
description: 借助 Microsoft ODBC Driver for SQL Server，ODBC 应用程序可以使用 Azure Active Directory 连接到 Azure SQL 数据库实例。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 52205f03-ff29-4254-bfa8-07cced155c86
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2a6b27c8c14a92ed9f17c56cab1cf2285b2cfec9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198914"
---
# <a name="using-azure-active-directory-with-the-odbc-driver"></a>结合使用 Azure Active Directory 和 ODBC 驱动程序
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="purpose"></a>目的

借助 Microsoft ODBC Driver for SQL Server 版本 13.1 或更高版本，ODBC 应用程序可以结合使用 Azure Active Directory 中的联合标识与用户名/密码、Azure Active Directory 访问令牌、Azure Active Directory 托管标识（17.3 以上版本）或 Windows 集成身份验证（Linux/macOS 上为 17.6 以上版本），连接到 Azure SQL 数据库实例。 对于 ODBC 驱动程序版本13.1，Azure Active Directory 访问令牌身份验证仅适用于 Windows  。 ODBC 驱动程序版本 17 和更高版本支持在所有平台（Windows、Linux 和 macOS）上进行此身份验证。 Windows 的 ODBC 驱动程序版本 17.1 中引入了新的使用登录 ID 的 Azure Active Directory 交互式身份验证。 在 ODBC 驱动程序版本 17.3.1.1 中，针对系统分配的标识和用户分配的标识，添加了一种新的 Azure Active Directory 托管标识身份验证方法。 所有这些都是通过使用新的 DSN 和连接字符串关键字以及连接属性来实现的。

> [!NOTE]
> Linux 和 macOS 上的 ODBC 驱动程序（早于 17.6 版本）仅支持直接对 Azure Active Directory 进行 Azure Active Directory 身份验证。 如果使用的是来自 Linux 或 macOS 客户端的 Azure Active Directory 用户名/密码身份验证，并且 Active Directory 配置要求客户端针对 Active Directory 联合身份验证服务终结点进行身份验证，则身份验证可能会失败。 从驱动程序 17.6 起，已消除这一限制。

## <a name="new-andor-modified-dsn-and-connection-string-keywords"></a>新的和/或修改的 DSN 和连接字符串关键字

使用 DSN 或连接字符串连接时，可以使用 `Authentication` 关键字来控制身份验证模式。 连接字符串中设置的值将重写 DSN 中的值（如提供）。 `Authentication` 设置的预属性值  是从连接字符串和 DSN 值计算得出的值。

|名称|值|默认|说明|
|-|-|-|-|
|`Authentication`|(未设置)、(空字符串)、`SqlPassword`、`ActiveDirectoryPassword`、`ActiveDirectoryIntegrated`、`ActiveDirectoryInteractive`、`ActiveDirectoryMsi`、`ActiveDirectoryServicePrincipal` |(未设置)|控制身份验证模式。<table><tr><th>值<th>说明<tr><td>(未设置)<td>由其他关键字确定的身份验证模式（现有旧连接选项。）<tr><td>(空字符串)<td>（仅限连接字符串。）重写和取消设置 DSN 中设置的 `Authentication` 值。<tr><td>`SqlPassword`<td>使用用户名和密码直接对 SQL Server 实例进行身份验证。<tr><td>`ActiveDirectoryPassword`<td>使用用户名和密码对 Azure Active Directory 标识进行身份验证。<tr><td>`ActiveDirectoryIntegrated`<td>仅限 Windows 和 Linux/Mac 17.6 以上版本的驱动程序。 使用集成身份验证对 Azure Active Directory 标识进行身份验证。<tr><td>`ActiveDirectoryInteractive`<td>_仅限 Windows 驱动程序_。 使用交互式身份验证对 Azure Active Directory 标识进行身份验证。<tr><td>`ActiveDirectoryMsi`<td>使用托管标识身份验证对 Azure Active Directory 标识进行身份验证。 对于用户分配的标识，UID 设置为用户标识的对象 ID。<tr><td>`ActiveDirectoryServicePrincipal`<td>(17.7+) 使用服务主体身份验证对 Azure Active Directory 标识进行身份验证。</table>|
|`Encrypt`|(未设置)、`Yes``No`|（请见说明）|控制连接的加密。 如果 `Authentication` 设置的预属性值在 DSN 或连接字符串中不是“无”  ，则默认值为 `Yes`。 否则默认值为 `No`。 如果属性 `SQL_COPT_SS_AUTHENTICATION` 重写 `Authentication` 的预属性值，请在 DSN 或连接字符串或连接属性中显式设置加密的值。 如果将值设置为 DSN 或连接字符串中的 `Yes`，则加密的预属性值为 `Yes`。|

## <a name="new-andor-modified-connection-attributes"></a>新的和/或修改的连接属性

已引入或修改以下连接预连接属性，以支持 Azure Active Directory 身份验证。 当连接属性具有相应的连接字符串或 DSN 关键字并且进行了设置时，将优先使用连接属性。

|Attribute|类型|值|默认|说明|
|-|-|-|-|-|
|`SQL_COPT_SS_AUTHENTICATION`|`SQL_IS_INTEGER`|`SQL_AU_NONE`, `SQL_AU_PASSWORD`, `SQL_AU_AD_INTEGRATED`, `SQL_AU_AD_PASSWORD`, `SQL_AU_AD_INTERACTIVE`, `SQL_AU_AD_MSI`, `SQL_AU_AD_SPA`, `SQL_AU_RESET`|(未设置)|请参阅上述 `Authentication` 关键字说明。 提供 `SQL_AU_NONE` 是为了显式重写 DSN 和/或连接字符串中设置的 `Authentication` 值，而如果设置了属性，`SQL_AU_RESET` 将取消设置，以便优先使用 DSN 或连接字符串值。|
|`SQL_COPT_SS_ACCESS_TOKEN`|`SQL_IS_POINTER`|指向 `ACCESSTOKEN` 或 NULL 的指针|Null|如果不是 null，则指定要使用的 AzureAD 访问令牌。 指定访问令牌并同时指定 `UID`、`PWD`、`Trusted_Connection` 或 `Authentication` 连接字符串关键字或其等效属性将发生错误。 <br> **注意：** ODBC 驱动程序版本 13.1 仅在 Windows  上支持此功能。|
|`SQL_COPT_SS_ENCRYPT`|`SQL_IS_INTEGER`|`SQL_EN_OFF`, `SQL_EN_ON`|（请见说明）|控制连接的加密。 `SQL_EN_OFF` 和 `SQL_EN_ON` 分别禁用和启用加密。 如果 `Authentication` 设置的预属性值不是“无”  或设置了 `SQL_COPT_SS_ACCESS_TOKEN`，并且在 DSN 或连接字符串中未指定 `Encrypt`，则默认值为 `SQL_EN_ON`。 否则默认值为 `SQL_EN_OFF`。 如果连接属性 `SQL_COPT_SS_AUTHENTICATION` 没有设置为“无”  ，则在 DSN 或连接字符串中未指定 `Encrypt` 的情况下，将 `SQL_COPT_SS_ENCRYPT` 显式设置为所需的值。 此属性的有效值控制[是否将加密用于连接。](../../relational-databases/native-client/features/using-encryption-without-validation.md)|
|`SQL_COPT_SS_OLDPWD`|\-|\-|\-|Azure Active Directory 不支持，因为无法通过 ODBC 连接来完成对 Azure AD 主体的密码更改。 <br><br>在 SQL Server 2005 中引入了 SQL Server 身份验证密码过期功能。 添加了 `SQL_COPT_SS_OLDPWD` 属性，以允许客户端同时为连接提供旧密码和新密码。 设置此属性时，访问接口对于第一次连接或后续连接将不使用连接池，因为连接字符串将包含现在已更改的“旧密码”。|
|`SQL_COPT_SS_INTEGRATED_SECURITY`|`SQL_IS_INTEGER`|`SQL_IS_OFF`,`SQL_IS_ON`|`SQL_IS_OFF`|已弃用  ；改为将 `SQL_COPT_SS_AUTHENTICATION` 设置为 `SQL_AU_AD_INTEGRATED`。 <br><br>强制将 Windows 身份验证（Linux 和 macOS 上的 Kerberos）用于服务器登录的访问验证。 使用 Windows 身份验证时，驱动程序忽略作为 `SQLConnect`、`SQLDriverConnect` 或 `SQLBrowseConnect` 处理的一部分提供的用户标识符和密码值。|

## <a name="ui-additions-for-azure-active-directory-windows-driver-only"></a>Azure Active Directory（仅 Windows 驱动程序）的 UI 新增功能

驱动程序的 DSN 设置和连接 UI 已经通过在 Azure AD 中使用身份验证所需的附加选项得到了增强。

### <a name="creating-and-editing-dsns-in-the-ui"></a>在 UI 中创建和编辑 DSN

使用驱动程序的安装程序 UI 创建或编辑现有 DSN 时，可以使用新的 Azure AD 身份验证选项：

`Authentication=ActiveDirectoryIntegrated` 用于向 Azure SQL 数据库执行 Azure Active Directory 集成身份验证

![选择了 Azure Active Directory 集成身份验证的 DSN 创建和编辑屏幕。](windows/create-dsn-ad-integrated.png)

`Authentication=ActiveDirectoryPassword` 用于向 Azure SQL 数据库执行 Azure Active Directory 用户名/密码身份验证

![选择了 Azure Active Directory 密码身份验证的 DSN 创建和编辑屏幕。](windows/create-dsn-ad-password.png)

`Authentication=ActiveDirectoryInteractive` 用于向 Azure SQL 数据库执行 Azure Active Directory 交互式身份验证

![选择了 Azure Active Directory 交互式身份验证的 DSN 创建和编辑屏幕。](windows/create-dsn-ad-interactive.png)

`Authentication=SqlPassword` 用于向 SQL Server 执行用户名/密码身份验证（Azure 或其他）

![选择了 SQL Server 身份验证的 DSN 创建和编辑屏幕。](windows/create-dsn-ad-sql-server.png)

`Trusted_Connection=Yes` 用于 Windows 旧 SSPI 集成身份验证

![选择了集成 Windows 身份验证的 DSN 创建和编辑屏幕。](windows/create-dsn-win-sspi.png)

`Authentication=ActiveDirectoryMsi` 用于 Azure Active Directory 托管标识身份验证

![选择了托管服务标识身份验证的 DSN 创建和编辑屏幕。](windows/create-dsn-ad-msi.png)

Azure Active Directory 服务主体身份验证的 `Authentication=ActiveDirectoryServicePrincipal`

![选择了 Azure Active Directory 服务主体身份验证的 DSN 创建和编辑屏幕。](windows/create-dsn-ad-spa.png)

这七个选项分别对应于 `Trusted_Connection=Yes`（仅限现有的旧 Windows SSPI 集成身份验证）和 `Authentication=` `ActiveDirectoryIntegrated`、`SqlPassword`、`ActiveDirectoryPassword`、`ActiveDirectoryInteractive`、`ActiveDirectoryMsi` 以及 `ActiveDirectoryServicePrincipal`。

### <a name="sqldriverconnect-prompt-windows-driver-only"></a>SQLDriverConnect 提示（仅限 Windows 驱动程序）

SQLDriverConnect 在请求完成连接所需的信息时显示的提示对话框包四个新的 Azure AD 身份验证选项：

![SQLDriverConnect 显示的 SQL Server 登录对话框。](windows/server-login.png)

这些选项对应于上述 DSN 安装程序 UI 中提供的六个相同选项。

### <a name="example-connection-strings"></a>连接字符串示例
1. SQL Server 身份验证 - 旧语法。 服务器证书未经验证，并且仅当服务器强制执行时才使用加密。 用户名/密码在连接字符串中传递。
`server=Server;database=Database;UID=UserName;PWD=Password;`
2. SQL 身份验证 -新语法。 客户端请求加密（`Encrypt` 的默认值为 `true`），并验证服务器证书，而不考虑加密设置（除非 `TrustServerCertificate` 设置为 `true`）。 用户名/密码在连接字符串中传递。
 `server=Server;database=Database;UID=UserName;PWD=Password;Authentication=SqlPassword;`
3. 使用 SSPI（面向 SQL Server 或 SQL IaaS）进行 Windows 集成身份验证（Linux 和 macOS 上的 Kerberos）- 当前语法。 除非使用加密，否则不会验证服务器证书。 
`server=Server;database=Database;Trusted_Connection=yes;`
4. （仅限 Windows 驱动程序  。）使用 SSPI 进行 Windows 集成身份验证（如果目标数据库位于 SQL Server 或 SQL IaaS 中）- 新语法。 客户端请求加密（`Encrypt` 的默认值为 `true`），并验证服务器证书，而不考虑加密设置（除非 `TrustServerCertificate` 设置为 `true`）。 
`server=Server;database=Database;Authentication=ActiveDirectoryIntegrated;`
5. Azure Active Directory 用户名/密码身份验证（如果目标数据库位于 Azure SQL 数据库中）。 无论如何设置加密，都会验证服务器证书（除非 `TrustServerCertificate` 设置为 `true`）。 用户名/密码在连接字符串中传递。 
`server=Server;database=Database;UID=UserName;PWD=Password;Authentication=ActiveDirectoryPassword;`
6. （仅限 Windows 和 Linux/macOS 17.6 以上版本的驱动程序。）使用 ADAL 或 Kerberos 进行 Windows 集成身份验证，它涉及到为 Azure AD 颁发的访问令牌兑换的 Windows 帐户凭据，假定目标数据库在 Azure SQL 数据库中。 无论如何设置加密，都会验证服务器证书（除非 `TrustServerCertificate` 设置为 `true`）。 在 Linux/macOS 上，需要提供合适的 Kerberos 票证；有关详细信息，请参阅下面的联合帐户和[使用集成身份验证](linux-mac/using-integrated-authentication.md)部分。
`server=Server;database=Database;Authentication=ActiveDirectoryIntegrated;`
7. （仅限 Windows 驱动程序。）Azure AD 交互身份验证使用 Azure Active Directory 多重身份验证技术来设置连接。 在此模式下，通过提供登录 ID，将触发 Azure 身份验证对话框，并允许用户输入密码来完成连接。 用户名在连接字符串中传递。
`server=Server;database=Database;UID=UserName;Authentication=ActiveDirectoryInteractive;`

![使用 Active Directory 交互式身份验证时的 Windows Azure 身份验证 UI。](windows/WindowsAzureAuth.png)

8. Azure Active Directory 托管标识身份验证使用系统分配的标识或用户分配的标识进行身份验证，以设置连接。 对于用户分配的标识，UID 设置为用户标识的对象 ID。<br>
对于系统分配的标识，<br>
`server=Server;database=Database;Authentication=ActiveDirectoryMsi;`<br>
对于对象 ID 为 myObjectId 的用户分配的标识，<br>
`server=Server;database=Database;UID=myObjectId;Authentication=ActiveDirectoryMsi;`

9. Azure Active Directory 服务主体身份验证 `server=Server;databse=Database;UID=clientId;PWD=clientSecret;Authentication=ActiveDirectoryServicePrincipal;`

> [!NOTE]
>- 将 Active Directory 选项与 Windows ODBC 驱动程序版本 17.4.2之前的版本一起使用时，请确保已安装[适用于 SQL Server 的 Active Directory 身份验证库](https://go.microsoft.com/fwlink/?LinkID=513072)。 使用 Linux 和 macOS 驱动程序时，请确保已安装 `libcurl`。 对于驱动程序版本17.2 及更高版本，这不是显式依赖项，因为其他身份验证方法或 ODBC 操作不需要它。
>- 如果 Azure Active Directory 配置包括条件访问策略，且客户端为 Windows 10 或 Server 2016 或更高版本，则通过集成或用户名/密码进行身份验证可能会失败。 条件访问策略需要使用 Windows 帐户管理器 (WAM)，它在 Windows 的驱动程序版本 17.6 或更高版本中受支持。 若要使用 WAM，请分别在全局、用户 DSN 或系统 DSN 范围内配置的 `HKLM\Software\ODBC\ODBCINST.INI\ODBC Driver 17 for SQL Server`、`HKCU\Software\ODBC\ODBC.INI\<your-user-DSN-name>` 或 `HKLM\Software\ODBC\ODBC.INI\<your-system-DSN-name>` 中创建名为 `ADALuseWAM` 的新字符串或 DWORD 值，并将其设置为值 1。 请注意，使用 WAM 进行身份验证不支持以具有 `runas` 的其他用户身份运行应用程序。 对于 Linux 或 macOS，不支持需要条件访问策略的情况。
>- 若要使用 SQL Server 帐户的用户名和密码进行连接，现在可以使用新的 `SqlPassword` 选项，特别建议对 Azure SQL 使用此选项，因为此选项启用了更安全的连接默认值。
>- 若要使用 Azure Active Directory 帐户的用户名和密码进行连接，请在连接字符串中指定 `Authentication=ActiveDirectoryPassword`，并分别使用用户名和密码指定 `UID` 和 `PWD` 关键字。
>- 若要使用 Windows 集成身份验证或 Active Directory 集成（仅限 Windows 和 Linux/macOS 17.6 以上版本的驱动程序）身份验证进行连接，请在连接字符串中指定 `Authentication=ActiveDirectoryIntegrated`。 驱动程序将自动选择正确的身份验证模式。 不得指定 `UID` 和 `PWD`。
>- 若要使用 Active Directory 交互式（仅限 Windows 驱动程序）身份验证进行连接，必须指定 `UID`。

## <a name="authenticating-with-an-access-token"></a>使用访问令牌进行身份验证

`SQL_COPT_SS_ACCESS_TOKEN` 预连接属性允许使用从 Azure AD 获取的访问令牌进行身份验证，而不是使用用户名和密码，还可以绕过驱动程序对访问令牌的协商和获取。 若要使用访问令牌，请将 `SQL_COPT_SS_ACCESS_TOKEN` 连接属性设置为指向 `ACCESSTOKEN` 结构的指针：

~~~
typedef struct AccessToken
{
    DWORD dataSize;
    BYTE data[];
} ACCESSTOKEN;
~~~

`ACCESSTOKEN` 是长度可变的结构，它由 4 个字节的长度  组成，后跟构成访问令牌的不透明数据的长度  字节。 由于 SQL Server 处理访问令牌的方式，通过 [OAuth 2.0](/azure/active-directory/develop/active-directory-authentication-scenarios) JSON 响应获得的令牌必须进行扩展，以便每个字节后面都有一个 0 填充字节，类似于只包含 ASCII 字符的 UCS-2 字符串；但是，令牌是一个不透明的值，并且指定的长度（以字节为单位）不能包含任何 null 终止符。 由于相当多的长度和格式限制，此身份验证方法仅以编程方式通过 `SQL_COPT_SS_ACCESS_TOKEN` 连接属性提供；没有相应的 DSN 或连接字符串关键字。 连接字符串不能包含 `UID`、`PWD`、`Authentication` 或 `Trusted_Connection` 关键字。

> [!NOTE]
> ODBC 驱动程序版本 13.1 仅在 Windows  上支持此身份验证。

## <a name="azure-active-directory-authentication-sample-code"></a>Azure Active Directory 身份验证示例代码

以下示例显示了使用带有连接关键字的 Azure Active Directory 连接到 SQL Server 所需的代码。 请注意，不需要更改应用程序代码本身；使用 Azure AD 进行身份验证只需修改连接字符串或 DSN（如果使用）：
~~~
    ...
    SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myuser;PWD=myPass;Authentication=ActiveDirectoryPassword"
    ...
    SQLDriverConnect(hDbc, NULL, connString, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);    
    ...
~~~
以下示例显示了使用带有访问令牌身份验证的 Azure Active Directory 连接到 SQL Server 所需的代码。 在这种情况下，需要修改应用程序代码以处理访问令牌并设置关联的连接属性。
~~~
    SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server}"
    SQLCHAR accessToken[] = "eyJ0eXAiOi..."; // In the format extracted from an OAuth JSON response
    ...
    DWORD dataSize = 2 _ strlen(accessToken);
    ACCESSTOKEN *pAccToken = malloc(sizeof(ACCESSTOKEN) + dataSize);
    pAccToken->dataSize = dataSize;
    // Expand access token with padding bytes
    for(int i = 0, j = 0; i < dataSize; i += 2, j++) {
        pAccToken->data[i] = accessToken[j];
        pAccToken->data[i+1] = 0;
    }
    ...
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_ACCESS_TOKEN, (SQLPOINTER)pAccToken, SQL_IS_POINTER);
    SQLDriverConnect(hDbc, NULL, connString, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);        
    ...
    free(pAccToken);
~~~
下面是用于 Azure Active Directory 交互式身份验证的连接字符串示例。 请注意，它不包含 PWD 字段，因为将使用 Azure 身份验证屏幕输入密码。
~~~
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myuser;Authentication=ActiveDirectoryInteractive"
~~~
下面是用于 Azure Active Directory 托管标识身份验证的连接字符串示例。 请注意，对于用户分配的标识，UID 设置为用户标识的对象 ID。
~~~
// For system-assigned identity,
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};Authentication=ActiveDirectoryMsi"
...
// For user-assigned identity with object ID equals to myObjectId
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myObjectId;Authentication=ActiveDirectoryMsi"
~~~

## <a name="considerations-for-using-adfs-federated-accounts-on-linuxmacos"></a>在 Linux/macOS 上使用 ADFS 联合帐户的注意事项

从 17.6 版开始，适用于 Linux 和 macOS 的驱动程序支持使用用户名/密码 (`ActiveDirectoryPassword`) 或 Kerberos (`ActiveDirectoryIntegrated`) 通过 Azure Active Directory ADFS 联合帐户进行身份验证。 使用集成模式时，有一些依赖于平台的限制。

当对 UPN 后缀不同于 Kerberos 领域的用户进行身份验证时（即使用备用 UPN 后缀），需要在获取 Kerberos 票证时使用“企业主体”选项（将 `-E` 选项与 `kinit` 结合使用，并提供 `user@federated-domain` 格式的主体名称）。 这允许驱动程序正确确定联合域和 Kerberos 领域。

可以通过检查 `klist` 命令的输出来验证是否有适用的 Kerberos 票证。 如果联合域与 Kerberos 领域和 UPN 后缀相同，则主体名称将采用格式 `user@realm`。 如果不同，则主体名称应采用格式 `user@federated-domain@realm`。

### <a name="linux"></a>Linux

在 SuSE 11 上，1.6.x 的默认 Kerberos 库版本不支持使用备用 UPN 后缀所需的“企业主体”选项。 若要将备用 UPN 后缀与 Azure AD 集成身份验证配合使用，请将 Kerberos 库升级到 1.7 或更高版本。

在 Alpine Linux 上，默认 `libcurl` 不支持 Azure AD 集成身份验证所需的 SPNEGO/Kerberos 身份验证。

### <a name="macos"></a>macOS

系统 Kerberos 库 `kinit` 支持带有 `--enterprise` 选项的企业主体，但也会隐式执行名称规范化，这会阻止使用备用 UPN 后缀。 若要将备用 UPN 后缀与 Azure AD 集成身份验证配合使用，请通过 `brew install krb5` 安装较新的 Kerberos 库，并将其 `kinit` 与上文所述的 `-E` 选项一起使用。

## <a name="see-also"></a>另请参阅

[使用 Azure AD 身份验证的 Azure SQL 数据库对基于令牌的身份验证的支持](/archive/blogs/sqlsecurity/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth)

[使用集成身份验证](linux-mac/using-integrated-authentication.md)