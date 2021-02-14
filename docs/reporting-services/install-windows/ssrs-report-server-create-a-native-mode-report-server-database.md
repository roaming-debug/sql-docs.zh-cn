---
title: 创建本机模式报表服务器数据库（配置管理器）| Microsoft Docs
description: 本机模式 Reporting Services 将 SQL Server 数据库用于内部存储。 该数据库是必需的，它用于存储已发布的报表、模型、共享数据源、会话数据、资源和服务器元数据。
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], databases
- databases [Reporting Services], creating
ms.assetid: 81b9f4ad-800b-4688-8b47-a5a83dc8ff10
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dab7b82e27ba57974a4bdb8e679164f6316ecd4a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067110"
---
# <a name="create-a-native-mode-report-server-database-report-server-configuration-manager"></a>创建本机模式报表服务器数据库（报表服务器配置管理器）

[!INCLUDE[ssrs-appliesto-sql2016-preview](../../includes/ssrs-appliesto-sql2016-preview.md)]

本机模式的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库用于内部存储。 该数据库是必需的，它用于存储已发布的报表、模型、共享数据源、会话数据、资源和服务器元数据。  

若要创建报表服务器数据库或更改连接字符串或凭据，请使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器的“数据库”页中的选项。  
  
## <a name="when-to-create-or-configure-the-report-server-databases"></a>何时创建或配置报表服务器数据库  
 如果在“仅文件”模式下安装报表服务器，则必须创建和配置报表服务器数据库。  
  
 如果在本机模式的默认配置下安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，则安装报表服务器实例时会自动创建和配置报表服务器数据库。 可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器查看或修改安装程序为您配置的设置。  
  
##  <a name="before-you-start"></a><a name="rsdbrequirements"></a> 开始之前  
 创建或配置报表服务器数据库是一个多步骤过程。 创建报表服务器数据库之前，请考虑要如何指定下列各项：  
  
 **选择数据库服务器**  
 查看支持的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 版本，以及查看[创建报表服务器数据库（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)主题中支持的版本。  
  
 **启用 TCP/IP 连接**  
 启用 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的 TCP/IP 连接。 默认情况下，某些 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 版本不启用 TCP/IP。 本主题中提供了相关说明。  
  
 **打开端口 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
 对于远程服务器，如果使用的是防火墙软件，则必须打开 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 侦听的端口。  
  
 **确定报表服务器凭据**  
 确定报表服务器与报表服务器数据库的连接方式。 凭据类型包括域用户帐户、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库用户帐户或报表服务器服务帐户。  
  
 这些凭据经过加密并存储在 RSReportServer.config 文件中。 报表服务器将这些凭据用于与报表服务器数据库进行的连接。 如果您要使用 Windows 用户帐户或数据库用户帐户，请确保指定已经存在的帐户。 尽管 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器将创建登录名并设置必要的权限，但不会为您创建帐户。 有关详细信息，请参阅[配置报表服务器数据库连接（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)。  
  
 **确定报表服务器语言**  
 选择要为报表服务器指定的语言。 当用户使用不同语言版本的浏览器连接到服务器时，预定义的角色名称、说明和“我的报表”文件夹不会以不同的语言显示。  
  
 **检查凭据以创建和设置数据库**  
 确保您拥有的帐户凭据具有在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例上创建数据库的权限。 这些凭据用于一次性连接以创建报表服务器数据库和 **RSExecRole**。 如果登录名尚不存在，将为报表服务器所用的帐户创建一个数据库用户登录名以连接到该数据库。 您可以用您登录时所用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户进行连接，也可以输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库登录名。  
  
### <a name="to-enable-access-to-a-remote-report-server-database"></a>启用对远程报表服务器数据库的访问  
  
1.  如果您使用的是远程 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，请登录到此数据库服务器以验证或启用 TCP/IP 连接。  
  
2.  依次指向 **“开始”** 、 **“所有程序”** 、 **Microsoft SQL Server**、 **“配置工具”** ，再单击 **“SQL Server 配置管理器”** 。  
  
3.  打开 **“SQL Server 网络配置”** 。  
  
4.  选择数据库实例。  
  
5.  右键单击“TCP/IP”，并选择“已启用”   。  
  
6.  重启服务。  
  
7.  打开防火墙软件并打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 侦听的端口。 对于默认实例，此端口通常为用于 TCP/IP 连接的 1433 端口。 有关详细信息，请参阅 [为数据库引擎访问配置 Windows 防火墙](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)。  
  
### <a name="to-create-a-local-report-server-database"></a>创建本地报表服务器数据库  
  
1.  启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器并连接到要为其创建数据库的报表服务器实例。 有关详细信息，请参阅[报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)。  
  
2.  在“数据库”页上，选择“更改数据库”  。  
  
3.  选择“创建新的报表服务器数据库”，然后选择“下一步”   。  
  
4.  连接到您将用于创建和承载报表服务器数据库的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例：  
  
    1.  键入要使用的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例。 向导将显示作为默认实例（如果可用）运行的本地 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。 否则，您必须键入要使用的服务器和实例。 命名实例采用以下格式：\<servername>\\<instancename\>。  
  
    2.  输入用于一次性连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的凭据以创建报表服务器数据库。 有关如何使用这些凭据的详细信息，请参阅本主题中的 [开始之前](#rsdbrequirements) 。  
  
    3.  选择“测试连接”，验证与服务器的连接  。  
  
    4.  选择“**下一页**”。  
  
5.  指定用于创建数据库的属性。 有关如何使用这些属性的详细信息，请参阅本主题中的 [开始之前](#rsdbrequirements) ：  
  
    1.  键入报表服务器数据库的名称。 创建主数据库时，会同时为其创建一个临时数据库。 请考虑使用一个说明性名称来帮助记忆数据库的使用方式。 请注意，您指定的名称将在数据库的生存期内使用。 在创建报表服务器数据库之后，不能对其进行重命名。  
  
    2.  选择要显示角色定义和“我的报表”所用的语言。  
  
    3.  报表服务器模式始终设置为 **“本机”** 。  
  
    4.  选择“**下一页**”。  
  
6.  指定报表服务器用来连接到报表服务器数据库的凭据。  
  
    1.  指定身份验证类型：  
  
         选择 **“数据库凭据”** 以使用已定义的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库登录名进行连接。 如果报表服务器位于不同域、不可信域或装有防火墙的计算机中，则建议使用数据库凭据。  
  
         如果你拥有的最低特权域用户帐户具有登录到该计算机和数据库服务器的权限，请选择“Windows 凭据”  。  
  
         如果希望报表服务器使用其自身的服务帐户进行连接，则选择 **“服务凭据”** 。 使用此选项，该服务器将使用集成安全性进行连接；凭据不进行加密或存储。  
  
    2.  选择“**下一页**”。  
  
7.  检查“摘要”页上的信息以确保设置正确，然后选择“下一步”  。  
  
8.  选择“报表服务器 URL”页上的 URL，验证连接。 必须定义这些 URL 才能进行此测试。 如果报表服务器数据库连接有效，将看到报表服务器文件夹层次结构。 有关详细信息，请参阅 [验证 Reporting Services 安装](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)  

## <a name="change-database-credentials"></a>更改数据库凭据

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器提供了更改凭据向导，可引导您完成重新配置报表服务器用于连接到报表服务器数据库的帐户的步骤。 更改凭据时，配置管理器将为报表服务器当前使用的报表服务器数据库更新数据库服务器的所有权限和数据库登录信息。 

1.  启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器并连接到要为其创建数据库的报表服务器实例。 有关详细信息，请参阅[报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)。  
  
2.  在“数据库”页上，选择“更改凭据”  。 

3.  连接到您将用于创建和承载报表服务器数据库的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例：  
  
    1.  输入用于一次性连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的凭据以创建报表服务器数据库。 有关如何使用这些凭据的详细信息，请参阅本主题中的 [开始之前](#rsdbrequirements) 。  
  
    2.  选择“测试连接”，验证与服务器的连接  。  
  
    3.  选择“**下一页**”。  

4.  指定报表服务器用来连接到报表服务器数据库的凭据。  
  
    1.  指定身份验证类型：  
  
         选择 **“数据库凭据”** 以使用已定义的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库登录名进行连接。 如果报表服务器位于不同域、不可信域或装有防火墙的计算机中，则建议使用数据库凭据。  
  
         如果你拥有的最低特权域用户帐户具有登录到该计算机和数据库服务器的权限，请选择“Windows 凭据”  。  
  
         如果希望报表服务器使用其自身的服务帐户进行连接，则选择 **“服务凭据”** 。 使用此选项，该服务器将使用集成安全性进行连接；凭据不进行加密或存储。  
  
    2.  选择“**下一页**”。 

5. 检查设置，然后选择“下一步”  。

6. 更改完成后，选择“完成”  。

## <a name="next-steps"></a>后续步骤

[配置报表服务器数据库连接](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
[管理 Reporting Services 本机模式报表服务器](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
[报表服务器配置管理器](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
