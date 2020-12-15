---
title: 在托管实例上托管数据库
description: 了解如何创建和配置 Master Data Services (MDS) 数据库，并将其托管在 Azure SQL 托管实例上。
ms.custom: ''
ms.date: 07/01/2019
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 19519697-c219-44a8-9339-ee1b02545445
author: v-redu
ms.author: lle
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: ef24acb23a346b59b747d876d60d9a58374188bd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469968"
---
# <a name="host-an-mds-database-on-a-managed-instance"></a>在托管实例上托管 MDS 数据库

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  本文介绍如何为托管实例上的 MDS) 数据库配置 Master Data Services (。
  
## <a name="preparation"></a>准备工作

若要准备，需要创建并配置 Azure SQL 托管实例并配置 web 应用程序计算机。

### <a name="create-and-configure-the-database"></a>创建并配置数据库

1. 使用虚拟网络创建托管实例。 有关详细信息，请参阅 [快速入门：创建 SQL 托管实例](/azure/sql-database/sql-database-managed-instance-get-started) 。

1. 配置点到站点连接。 有关说明，请参阅 [使用本机 Azure 证书身份验证配置与 VNet 的点到站点连接 Azure 门户](/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) 。

1. 配置 SQL 托管实例 Azure Active Directory 身份验证。 有关详细信息，请参阅 [通过 SQL 配置和管理 Azure Active Directory authentication](/azure/sql-database/sql-database-aad-authentication-configure) 。

### <a name="configure-web-application-machine"></a>配置 web 应用程序计算机

1. 安装点到站点连接证书和 VPN 以确保计算机可以访问托管实例。 有关说明，请参阅 [使用本机 Azure 证书身份验证配置与 VNet 的点到站点连接 Azure 门户](/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) 。

1. 安装以下角色和功能：
   - 角色：
     - Internet Information Services
     - Web 管理工具
     - IIS 管理控制台
     - 万维网服务
     - 应用程序开发
     - .NET Extensibility 3.5
     - .NET Extensibility 4.5
     - ASP.NET 3.5
     - ASP.NET 4.5
     - ISAPI 扩展
     - ISAPI 筛选器
     - 常见的 HTTP 功能
     - 默认文档
     - 目录浏览
     - HTTP 错误
     - 静态内容
     - 运行状况和诊断
     - HTTP 日志记录
     - 请求监视器
     - 性能
     - 静态内容压缩
     - 安全性
     - 请求筛选
     - Windows 身份验证
       > [!NOTE]
       > 不安装 WebDAV 发布

   - 功能：
     - .NET Framework 3.5（包括 .NET 2.0 和 3.0）
     - .NET Framework 4.5 高级服务
     - ASP.NET 4.5
     - WCF Services
     - HTTP 激活 (必需) 
     - TCP 端口共享
     - Windows Process Activation Service
     - 进程模型
     - .NET 环境
     - 配置 API
     - 动态内容压缩

## <a name="install-and-configure-an-mds-web-application"></a>安装和配置 MDS web 应用程序

接下来，安装和配置 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 。

### <a name="install-sql-server-2019"></a>安装 SQL Server 2019

使用 SQL Server 安装程序安装向导或命令提示符安装 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 。

1. 打开 `Setup.exe` ，然后按照安装向导中的步骤进行操作。

2. 在“功能选择”页的“共享功能”下，选择 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]。
此操作安装：
   - [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]
   - 程序集
   - Windows PowerShell 管理单元
   - Web 应用程序和服务的文件夹和文件。

   ![显示 "功能选择" 页的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-sqlfeatureselection.png "mds-SQLServer2019-MI_SQLFeatureSelection")  

### <a name="set-up-the-database-and-website"></a>设置数据库和网站

1. 连接 Azure 虚拟网络，以确保可以连接到托管实例。

   ![用于连接到 Azure 虚拟网络的测试 MI VPN 的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-p2svpnconnect.png "mds-SQLServer2019-MI_P2SVPNConnect")

1. 打开 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ，然后在左窗格中选择 " **数据库配置** "。

1. 选择 " **创建数据库** " 以打开 " **创建数据库向导**"。 选择“下一步”  。

1. 在 " **数据库服务器** " 页上，完成 " **SQL Server 实例** " 字段，然后选择 " **身份验证类型**"。 选择 " **测试连接** " 以确认你可以通过所选的身份验证类型使用凭据连接到数据库。 选择“下一步”  。

   > [!NOTE]
   > - SQL Server 实例如下所示 `xxxxxxx.xxxxxxx.database.windows.net` 。
   > - 对于托管实例，请选择 **"SQL Server 帐户"** 和 **"当前用户– Active Directory 集成"** 身份验证类型。
   > - 如果选择 " **当前用户– Active Directory 集成** 为身份验证类型"，则 " **用户名** " 字段为只读，并显示当前登录的 Windows 用户帐户。 如果在 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Azure 虚拟机上运行 SQL Server 2019 (vm) ，则 " **用户名** " 字段将显示 vm 上的本地管理员帐户的 vm 名称和用户名。

   身份验证必须包含托管实例的 **"sysadmin"** 规则。

   !["创建数据库" 向导的 "数据库服务器" 页的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-createdbconnect.png "mds-SQLServer2019-MI_CreateDBConnect")  

1. 在“数据库名称”字段中键入名称。 （可选）若要选择 Windows 排序规则，请清除 " **SQL Server 默认排序规则** " 复选框，然后选择一个或多个可用选项。 例如， **区分大小写**。 选择“下一步”  。

   !["创建数据库" 向导的 "数据库" 页的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-createddbname.png "mds-SQLServer2019-MI_CreatedDBName")

1. 在 " **用户名** " 字段中，指定的默认超级用户的 Windows 帐户 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 。 超级用户有权访问所有功能区域，并且可以添加、删除和更新所有模型。

   !["创建数据库" 向导的 "管理员帐户" 页的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-createdbusername.png "mds-SQLServer2019-MI_createDBUserName")

1. 选择 " **下一步** " 以查看数据库的设置摘要 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 。 再次选择 "  **下一步** " 以创建数据库。 你将看到 " **进度" 和 "完成** " 页。

1. 创建并配置数据库后，选择 " **完成**"。

   有关 " **创建数据库向导**" 中的设置的详细信息，请参阅 [创建数据库向导 &#40;[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Configuration Manager&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md)。

1. 在的 " **数据库配置** " 页上 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ，选择 " **选择数据库**"。

1. 选择 " **连接**"，选择 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 数据库，然后选择 **"确定"**。

   !["连接到数据库" 对话框的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-connectdbname.png "mds-SQLServer2019-MI_connectDBName")

1. 在中 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ，选择左窗格中的 " **Web 配置** "。

1. 在 " **网站** " 列表框中，选择 " **默认** 网站"，然后选择 " **创建** " 创建 Web 应用程序。

   !["Master Data Services 配置管理器" 对话框的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-webconfiguration.png "mds-SQLServer2019-MI_WebConfiguration")

   > [!NOTE]
   > 如果选择 " **默认** 网站"，则需要单独创建一个 Web 应用程序。 如果在列表框中选择 " **新建网站** "，则会自动创建该应用程序。

1. 在 " **应用程序池** " 部分中，输入其他用户名，输入密码，然后选择 **"确定"**。

   !["应用程序管理" 对话框的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-createwebapplication.png "mds-SQLServer2019-MI_CreateWebApplication")

   > [!NOTE]
   > 请确保用户可以使用最近创建的 Active Directory 集成身份验证来访问数据库。 另外，还可以在以后更改连接 `web.config` 。

   有关 " **创建 Web 应用程序** " 对话框的详细信息，请参阅 " [创建 web 应用程序" 对话框 &#40;[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Configuration Manager&#41;](../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md)。

1. 在 "web **配置**" 窗格的 "web **应用程序**" 窗口中，选择您创建的应用程序，然后在 "**将应用程序与数据库关联**" 部分中选择 "**选择**"。

1. 选择 " **连接** "，然后选择 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 要与 web 应用程序关联的数据库。 选择“确定”。

   已完成网站设置。 " **Web 配置** " 页现在会显示所选网站、所创建的 Web 应用程序以及 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 与该应用程序关联的数据库。

   ![Web 配置部分的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-webconfigselectdb.png "mds-SQLServer2019-MI_WebConfigSelectDB")

1. 选择“应用”。 你将看到 " **配置完成** " 消息。 在消息框中选择 **"确定"** 以启动 web 应用程序。 网站地址为 `http://server name/web application/` 。

## <a name="configure-authentication"></a>配置身份验证

若要将托管实例数据库连接到 web 应用程序，需要更改其他身份验证类型。

`web.config`在下找到该文件 `C:\Program Files\Microsoft SQL Server\150\Master Data Services\WebApplication` 。 修改 connectionString 以更改其他身份验证类型以连接到托管实例数据库。

默认的身份验证类型 `Active Directory Integrated` 如下面的连接字符串示例所示：

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;Authentication=&quot;Active Directory Integrated&quot;" />
   ```

MDS 还支持 Active Directory 密码身份验证和 SQL Server 身份验证，如下面的连接字符串示例中所示：

- Active Directory 密码身份验证

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;Authentication=&quot;Active Directory Password&quot; ; UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!" />
   ```

- SQL Server 身份验证

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;User ID=UserName;Password=MyPassword!;" />
   ```

## <a name="upgrade-ssmdsshort_md-and-sql-database-version"></a>升级 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 和 SQL 数据库版本

### <a name="upgrade-ssmdsshort_md"></a>升级 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]

安装 **SQL Server 2019 累积更新**。 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 将自动更新。

### <a name="upgrade-sql-server"></a>升级 SQL Server

你可能会收到错误： `The client version is incompatible with the database version` 安装 **SQL Server 2019 累积更新**。
![Master Data Services 错误的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-upgradedbpage.png "mds-SQLServer2019-MI_UpgradeDBPage")

若要解决此问题，需要升级数据库版本：

1. 打开 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ，然后在左窗格中选择 "  **数据库配置** "。

1. 在的 " **数据库配置** " 页上 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] ，选择 " **选择数据库**"。

1. 选择 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 与 web 应用程序关联的数据库。 选择 " **连接**"，然后选择 **"确定"**。

   !["连接到 Master Data Services 数据库" 对话框的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-connectdbname.png "mds-SQLServer2019-MI_ConnectDBName")

1. 选择 "**升级数据库 ...** " .

   ![升级数据库选项的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-selectupgradedb.png "mds-SQLServer2019-MI_SelectUpgradeDB")

1. 在升级数据库向导的 "**欢迎**" 页上，选择 "**下一步**"，然后在 "**升级评审**" 页上选择。

   ![升级数据库向导的 "升级评审" 页的屏幕截图。](../master-data-services/media/mds-sqlserver2019-config-mi-upgradedbwizard.png "mds-SQLServer2019-MI_UpgradeDBWizard")

1. 完成所有任务后，选择 " **完成** "。

## <a name="see-also"></a>请参阅

- [Master Data Services 数据库](../master-data-services/master-data-services-database.md)
- [主数据管理器 Web 应用程序](../master-data-services/master-data-manager-web-application.md)
- [“数据库配置”页（Master Data Services 配置管理器）](../master-data-services/database-configuration-page-master-data-services-configuration-manager.md)
- [Master Data Services (MDS) 中的新增功能](../master-data-services/what-s-new-in-master-data-services-mds.md)