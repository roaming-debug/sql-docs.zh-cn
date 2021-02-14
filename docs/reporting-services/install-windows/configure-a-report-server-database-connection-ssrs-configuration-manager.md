---
title: 配置报表服务器数据库连接（配置管理器）| Microsoft Docs
description: 每个报表服务器实例都需要连接到存储由服务器管理的报表、共享数据源、资源和元数据的报表服务器数据库。
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: seo-lt-2019, seo-mmd-2019
ms.date: 01/04/2020
ms.openlocfilehash: fdbf12310fb46add3ce9e3240d7c249028eda606
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057292"
---
# <a name="configure-a-report-server-database-connection-report-server-configuration-manager"></a>配置报表服务器数据库连接（报表服务器配置管理器）

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

每个报表服务器实例都需要连接到存储由服务器管理的报表、共享数据源、资源和元数据的报表服务器数据库。 如果要安装默认配置，则可以在报表服务器安装过程中创建初始连接。 多数情况下，可以在安装程序完成之后使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具配置连接。 您可以随时修改连接，以更改帐户类型或重置凭据。 有关如何创建数据库并配置连接的分步说明，请参阅[创建本机模式报表服务器数据库（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md)。

如果出现下列情况，则必须配置报表服务器数据库连接：  

- 为首次使用配置报表服务器。  

- 配置报表服务器以使用不同的报表服务器数据库。  

- 更改数据库连接所使用的用户帐户或密码。 当帐户信息存储在 RSReportServer.config 文件中时，您只需要更新数据库连接。 如果使用服务帐户进行连接（该帐户使用 Windows 集成安全性作为凭据类型），则不会存储密码，从而无需更新连接信息。 有关更改帐户的详细信息，请参阅[配置报表服务器服务帐户（报表服务器配置管理器）](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)。  

- 配置报表服务器扩展部署。 配置扩展部署时，您需要创建多个到报表服务器数据库的连接。 有关如何执行此多步操作的详细信息，请参阅[配置本机模式报表服务器横向扩展部署（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)。  

## <a name="how-reporting-services-connects-to-the-database-engine"></a>Reporting Services 如何连接到数据库引擎

报表服务器根据凭据和连接信息以及对使用该数据库的报表服务器实例有效的加密密钥来访问报表服务器数据库。 拥有有效的加密密钥对于存储和检索敏感数据是必要的。 首次配置数据库时，会自动创建加密密钥。 创建密钥之后，如果更改报表服务器服务标识，则必须更新这些密钥。 有关使用加密密钥的详细信息，请参阅[配置和管理加密密钥（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)。  

报表服务器数据库为内部组件，只有报表服务器可以访问。 为报表服务器数据库指定的凭据和连接信息专门由报表服务器使用。 请求报表的用户不需要拥有报表服务器数据库的数据库权限或数据库登录名。  

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 使用 **System.Data.SqlClient** 连接到承载报表服务器数据库的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。 如果使用的是 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的本地实例，报表服务器将使用共享内存建立连接。 如果使用的是报表服务器数据库的远程数据库服务器，则可能必须根据所使用的版本启用远程连接。 如果使用的是 Enterprise Edition，则默认情况下会启用 TCP/IP 远程连接。  

若要验证实例是否接受远程连接，请依次单击“开始”、“所有程序”、[!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]、“配置工具”、“SQL Server 配置管理器”，然后确认为每个服务启用了 TCP/IP 协议     。  

启用远程连接时，也会启用客户端协议和服务器协议。 若要确认协议已启用，请依次单击 **“开始”** 、 **“所有程序”** 、 [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]、 **“配置工具”** 、 **“SQL Server 配置管理器”** 、 **“SQL Server 网络配置”** ，再单击 **“MSSQLSERVER 协议”** 。 有关详细信息，请参阅在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中[启用或禁用服务器网络协议](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)。  

## <a name="defining-a-report-server-database-connection"></a>定义报表服务器数据库连接

若要配置连接，您必须使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器工具或 **rsconfig** 命令行实用工具。 报表服务器需要以下连接信息：  

- 承载报表服务器数据库的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的名称。  

- 报表服务器数据库的名称。 首次创建连接时，可以创建新的报表服务器数据库，或选择现有数据库。 有关详细信息，请参阅[创建报表服务器数据库（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)。  

- 凭据类型。 可以使用服务帐户、Windows 域帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库登录名。  

- 用户名和密码（仅在使用 Windows 域帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名时需要）。

必须为所提供的凭据授予访问报表服务器数据库的权限。 如果使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具，则此步骤会自动执行。 有关访问数据库所需权限的详细信息，请参阅本主题中的“数据库权限”一节。  

### <a name="storing-database-connection-information"></a>存储数据库连接信息

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 在下列 RSreportserver.config 设置中存储和加密连接信息。 必须使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具或 rsconfig 实用工具为这些设置创建加密值。  

并非所有的值都针对每一种连接类型进行了设置。 如果使用默认值配置连接（即使用服务帐户建立连接），则 \<**LogonUser**>、\<**LogonDomain**> 和 \<**LogonCred**> 将为空，如下所示：  

```
<Dsn></Dsn>  
<ConnectionType></ConnectionType>  
<LogonUser></LogonUser>  
<LogonDomain></LogonDomain>  
<LogonCred></LogonCred>  
```

如果将连接配置为使用特定的 Windows 帐户或数据库登录名，则在随后更改帐户或登录名时必须更新已存储的值。  

### <a name="choosing-a-credential-type"></a>选择凭据类型

可以在报表服务器数据库连接中使用三种类型的凭据：  

- 使用报表服务器服务帐户的 Windows 集成安全性。 由于报表服务器作为单个服务实现，因此只有运行服务的帐户需要数据库访问。  
  
- Windows 用户帐户。 如果报表服务器和报表服务器数据库安装在同一台计算机上，则可以使用本地帐户。 否则，必须使用域帐户。  
  
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。
  
> [!NOTE]
> 不能使用自定义身份验证扩展插件来连接到报表服务器数据库。 自定义身份验证扩展插件只能用来对报表服务器的主体进行身份验证。 它们不会影响报表服务器数据库的连接，也不会影响为报表提供内容的外部数据源的连接。
  
如果 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例配置为使用 Windows 身份验证且与报表服务器计算机位于同一个域或可信域中，则可将连接配置为使用通过 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具作为连接属性进行管理的服务帐户或域用户帐户。 如果数据库服务器位于另一个域中，或者您使用的是工作组安全性，则必须将连接配置为使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库登录名。 在这种情况下，一定要对连接进行加密。  

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> 使用 Azure SQL 托管实例承载报表服务器数据库时，SQL Server 身份验证是唯一受支持的凭据类型。 此外，请注意，托管实例无法承载报表服务器实例。

::: moniker-end

#### <a name="using-service-accounts-and-integrated-security"></a>使用服务帐户和集成安全性

可以使用 Windows 集成安全性通过报表服务器服务帐户进行连接。 已为此帐户授予了登录报表服务器数据库的权限。 如果以默认配置安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，这将是安装程序选择的默认凭据类型。  

此服务帐户为可信帐户，此帐户提供一种低维护方法来管理报表服务器数据库连接。 由于此服务帐户使用 Windows 集成安全性来建立连接，因此无需存储凭据。 但是，如果以后要更改服务帐户密码或标识（例如从内置帐户切换到域帐户），请确保使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具进行更改。 该工具会将数据库权限自动更新为使用修改后的帐户信息。 有关详细信息，请参阅[配置报表服务器服务帐户（报表服务器配置管理器）](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)。  

将数据库连接配置为使用服务帐户时，如果报表服务器数据库位于远程计算机上，则帐户必须拥有网络权限。 如果报表服务器数据库位于不同的域中，并且位于防火墙之后，或者您使用的是工作组安全性而非域安全性，则不要使用服务帐户。 请改用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库用户帐户。  

#### <a name="using-a-domain-user-account"></a>使用域用户帐户

可以为报表服务器到报表服务器数据库的连接指定一个 Windows 用户帐户。 如果使用本地帐户或域帐户，则每次更改密码或帐户时，必须更新报表服务器数据库连接。 请始终使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具来更新连接。  

#### <a name="using-a-sql-server-login"></a>使用 SQL Server 登录名

可以指定使用一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名连接到报表服务器数据库。 如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证并且报表服务器数据库位于远程计算机上，则可以使用 IPSec 来协助保护服务器之间的数据传输。 如果使用数据库登录名，则每次更改密码或帐户时，必须更新报表服务器数据库连接。

### <a name="database-permissions"></a>数据库权限

用来连接到报表服务器数据库的帐户被授予了以下角色：  

- **ReportServer** 数据库的 **public** 和 **RSExecRole** 角色。  

- **master** 、 **msdb** 和 **ReportServerTempDB** 数据库的 **RSExecRole** 角色。  

使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具创建或修改连接时，将自动授予这些权限。 如果使用 rsconfig 实用工具并且要为该连接指定不同的帐户，则必须为该新帐户更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 可以在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具中创建用来更新报表服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的脚本文件。  

### <a name="verifying-the-database-name"></a>验证数据库名称

使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具可以确定特定报表服务器实例所使用的报表服务器数据库。 若要查找该名称，请连接到该报表服务器实例并打开“数据库安装”页。  

## <a name="using-a-different-report-server-database-or-moving-a-report-server-database"></a>使用不同的报表服务器数据库或移动报表服务器数据库

可以通过更改连接信息将报表服务器实例配置为使用不同的报表服务器数据库。 切换数据库的一个常见示例是部署生产报表服务器。 通常，生产服务器是通过从测试报表服务器数据库切换到生产报表服务器数据库来实现的。您还可以将报表服务器数据库移动到另一台计算机上。 有关详细信息，请参阅在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中[升级和迁移 Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)。  

## <a name="configuring-multiple-reports-servers-to-use-the-same-report-server-database"></a>将多个报表服务器配置为使用同一个报表服务器数据库

可以将多个报表服务器配置为使用同一个报表服务器数据库。 此部署配置称为扩展部署。 如果要在服务器群集中运行多个报表服务器，则此配置为必备条件。 但是，如果要将服务应用程序分段，或者要测试新的报表服务器实例的安装和设置以将其与现有报表服务器安装进行比较，则也以可使用此配置。 有关详细信息，请参阅[配置本机模式报表服务器横向扩展部署（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)。  

## <a name="next-steps"></a>后续步骤

- [创建报表服务器数据库](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
- [管理 Reporting Services 本机模式报表服务器](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
- [配置报表服务器服务帐户](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
