---
description: 升级报表服务器数据库
title: 升级报表服务器数据库 | Microsoft Docs
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- upgrading databases
- report server database
- upgrading Reporting Services
ms.assetid: 4091cf87-9d97-4048-a393-67f1f9207401
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cf6cc7dc672e14a0fd988354ba4900fb49c206e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075888"
---
# <a name="upgrade-a-report-server-database"></a>升级报表服务器数据库

报表服务器数据库可为一个或多个报表服务器实例提供存储。 因为报表服务器数据库架构可能会因为推出新的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]版本而有所变化，所以要求数据库版本与使用的报表服务器实例的版本相匹配。 大多数情况下，报表服务器数据库可以自动升级，您不需要执行任何具体操作。  
  
 **本机模式：** 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式中，报表服务器数据库实际包含两个数据库，其默认名称分别为 ReportServer 和 ReportServerTempDB。  

::: moniker range="=sql-server-2016"
  
 **SharePoint 模式：** 在 SQL Server 2016 Reporting Services SharePoint 模式中，报表服务器数据库实际上是为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序的每个实例创建的数据库集合。  

::: moniker-end

## <a name="ways-to-upgrade-a-native-mode-report-server-database"></a>如何升级本机模式报表服务器数据库

 以下列表指出了升级报表服务器数据库的各种情况：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序升级报表服务器的单个实例。 在服务启动并且报表服务器确定数据库架构版本与服务器版本不匹配之后，将自动升级报表服务器数据库架构。  
  
     服务启动时，报表服务器会检查数据库架构版本以验证它是否与服务器版本相匹配。 如果数据库架构版本较低，该架构将自动升级到报表服务器所需的架构版本。 如果还原或附加一个较低的报表服务器数据库，则自动升级功能特别有用。 将在报表服务器跟踪日志文件中输入一条消息，指示已升级数据库架构版本。  
  
-   当选择旧版本与新报表服务器实例一起使用时， [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器将升级本地或远程报表服务器数据库。 在这种情况下，必须在发生此操作之前确认升级操作。  
  
     [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器不再提供单独的“升级”按钮或升级脚本。 鉴于报表服务器服务的自动升级功能，这些功能从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始已过时。  
  
 架构更新后，无法再将升级回滚到以前的版本。 请务必备份报表服务器数据库，以备需要重新创建先前安装。  
  
## <a name="how-the-schema-metadata-and-report-server-content-is-updated"></a>如何更新架构、元数据和报表服务器内容  
 升级报表服务器数据库需分三个步骤：  
  
1.  在安装并启动服务后，或者您在较旧版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器中选择 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式报表服务器数据库后，架构将自动升级。 此外，报表服务器服务会在启动时检查数据库版本。 如果报表服务器连接到早期版本的数据库，则报表服务器将在启动过程中更新该数据库。  
  
2.  在更新架构后首次使用报表服务器数据库时升级安全描述符。  
  
3.  首次使用时，升级已发布报表和已编译报表快照。 有关更多信息，请参见 [Upgrade Reports](../../reporting-services/install-windows/upgrade-reports.md)。  
  
 除了报表服务器数据库外，报表服务器还会使用临时数据库。 升级报表服务器数据库时，会自动升级临时数据库。  
  
## <a name="permissions-required-to-upgrade-a-report-server-database"></a>升级报表服务器数据库所需的权限  
 如果要升级的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装包含报表服务器数据库，而没有足够的权限执行数据库升级，您可能会看到一条错误消息。 默认情况下，安装程序会使用运行安装程序的用户的安全令牌连接到远程 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例并更新架构。 如果你拥有对承载报表服务器数据库的数据库服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sysadmin** 权限，将会成功升级数据库。 同样，如果从命令提示符运行安装程序，并为拥有 **sysadmin** 权限来修改远程计算机上的架构的帐户指定 RSUPGRADEDATABASEACCOUNT 和 RSUPGRADEPASSWORD 参数，也将成功升级数据库。  
  
 但是，如果您没有对远程计算机上的数据库的 **sysadmin** 权限，则系统将拒绝连接，并出现以下错误：  
  
 `"Setup was not able to upgrade the report server database schema. You must update the database schema manually after setup is finished. To update the schema, run the Report Server Configuration Manager, open the Database Setup page, re-select the database, and click Apply. The database will be upgraded automatically."`  
  
 此时，报表服务器程序文件将被升级，但报表服务器数据库将为早期版本的格式。 报表服务器将不可用，直到通过手动升级数据库来完成升级过程为止。  
  
#### <a name="to-upgrade-a-native-mode-database-with-scripts"></a>使用脚本升级本机模式数据库  
 您可以生成 WMI 脚本来升级报表服务器数据库。 有关详细信息，请参阅 [GenerateDatabaseUpgradeScript 方法 (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md)  
  
## <a name="next-steps"></a>后续步骤

[报表服务器配置管理器](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
[创建报表服务器数据库](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
[升级和迁移 Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[迁移 Reporting Services 安装](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
