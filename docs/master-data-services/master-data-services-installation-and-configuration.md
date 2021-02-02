---
title: 安装和配置
description: 了解如何在 Windows Server 2012 R2 计算机上安装 Master Data Services，配置 MDS 数据库和网站，以及部署示例模型和数据。
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: quickstart
ms.assetid: f6cd850f-b01b-491f-972c-f966b9fe4190
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ca05529c9b4ce1d9ec603989af81f0d2aeebf76b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236401"
---
# <a name="master-data-services-installation-and-configuration"></a>Master Data Services 的安装和配置

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  本文介绍了如何在 Windows Server 2012 R2 计算机上安装 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 、设置 MDS 数据库和网站，以及部署示例模型和数据。 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) 使你的组织能够管理数据的受信任版本。   
  
> [!NOTE] 
> 使用支持 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 的开发者版本时，可以在 Windows 10 系统的计算机安装 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]。 
>>有关支持不同 [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] 版本的操作系统的详细信息，请参阅[安装 SQL Server 2016 的硬件和软件要求](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。 

有关如何在 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]中组织数据的概述，请参阅 [Master Data Services 概述 (MDS)](../master-data-services/master-data-services-overview-mds.md)。     
  
 有关 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 中新增功能的信息，请参阅 [Master Data Services (MDS) 中的新增功能](../master-data-services/what-s-new-in-master-data-services-mds.md)。  
 
有关可帮助你了解 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]的视频以及其他培训资源的链接，请参阅 [了解 Master Data Services](../master-data-services/learn-sql-server-master-data-services.md)。 
  
> **下载**  
> -   若要下载 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]，请转到  **[评估中心](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-ctp/)** 。  
> -   已经拥有 Azure 帐户？  然后转到[此处](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)，启动已安装 SQL Server 的虚拟机。  
> 
> **无法创建 MDS 网站？**
> >请查看此 Microsoft 支持文章以获取有关如何解决此问题的说明。
> [无法在 SQL Server 2016 中通过使用低特权帐户创建 MDS 网站](https://aka.ms/mdssupport) 

## <a name="internet-explorer-and-silverlight"></a>Internet Explorer 和 Silverlight
- 在 Windows Server 2012 计算机上安装 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 时，可能必须配置 Internet Explorer 增强安全性以允许为 Web 应用程序站点编写脚本。 否则，浏览到服务器计算机上的站点将失败。
- 在使用 Web 应用程序时，Silverlight 5 必须安装在客户端计算机上。 如果不具有所需版本的 Silverlight，则在导航到需要 Silverlight 的 Web 应用程序区域时，系统将提示安装 Silverlight。 可以从[此处](https://www.microsoft.com/silverlight/)安装 Silverlight 5。

## <a name="ssmdsshort_md-on-an-azure-virtual-machine"></a>Azure 虚拟机上的 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]
默认情况下，当你使用已安装的创建 Azure 虚拟机时 [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] ， [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 还将安装。 

下一步是安装 Internet 信息服务 (IIS)。 请参阅[安装和配置 IIS](#InstallIIS) 部分。 

如果对更改 [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] 的安装感兴趣，可以在默认位置 `<drive>`:\SQLServer_13.0_Full 找到 setup.exe 文件。
  
## <a name="installing-master-data-services"></a><a name="InstallMDS"></a> 安装 Master Data Services  
 可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 安装程序安装向导或命令提示符来安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]。  
  
 **在 Windows Server 2012 R2 计算机上使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 安装程序安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]**  
  
1.  双击 Setup.exe，然后按照安装向导中的步骤进行操作。  
  
2.  在“功能选择”页的“共享功能”下，选择 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]。  
  
     将安装 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]、程序集、Windows PowerShell 管理单元以及 Web 应用程序和服务的文件夹和文件。  
  
     ![mds_SQLServer2016Setup_FeatureSelection](../master-data-services/media/mds-sqlserver2016setup-featureselection.png "mds_SQLServer2016Setup_FeatureSelection")  
  
3.  完成安装向导。  

## <a name="installing-and-configuring-iis"></a><a name="InstallIIS"></a> 安装和配置 IIS
  
1.  在 [!INCLUDE[winblue_server_2](../includes/winblue-server-2-md.md)] 中，单击“桌面”上任务栏上的“服务器管理器”图标。  
  
     ![Windows Server 2012 任务栏中服务器管理器的图标](../master-data-services/media/mds-windowsservertaskbar-servermanagericon.png "Windows Server 2012 任务栏中服务器管理器的图标")  
  
5.  在“服务器管理器”中，单击“管理”菜单中的“添加角色和功能”。  
   
     ![在服务器管理中，"添加角色和功能" 菜单命令](../master-data-services/media/mds-servermanagerdashboard-addrolesfeaturesmenu.png "在服务器管理中，"添加角色和功能" 菜单命令")  
  
6.  在“添加角色和功能向导”的“安装类型”页上，接受默认值（**基于角色或基于功能的安装**），然后单击“下一步”。  
  
7.  单击“从服务器池中选择服务器”，然后单击安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 的服务器。  
  
     ![mds_AddRolesFeaturesWizard_ServerSelectionPage](../master-data-services/media/mds-addrolesfeatureswizard-serverselectionpage.png) 
  
8. 在“服务器角色”页, 单击“Web 服务器” ，再单击“下一步”。 

   ![mds_AddRolesFeaturesWizard_ServerRolesPage](../master-data-services/media/mds-addrolesfeatureswizard-serverrolespage.png)
   
9. 在“功能”页，确认选中以下功能，然后单击“下一步”。 [!INCLUDE[winblue_server_2_md](../includes/winblue-server-2-md.md)] 上的 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 需要这些功能。
  
    |功能|功能|  
    |--------------|--------------|  
    |![mds_AddRolesFeaturesWizard_FeaturesPage](../master-data-services/media/mds-addrolesfeatureswizard-featurespage.png)|![mds_AddRolesFeaturesWizard_FeaturesPage_WindowsProcActive](../master-data-services/media/mds-addrolesfeatureswizard-featurespage-windowsprocactive.png)|  

10. 在左侧窗格中，单击“Web 服务器角色 (IIS)”，然后单击“角色服务”。
11. 在“角色服务”页，确认选中以下服务，然后单击“下一步”。 [!INCLUDE[winblue_server_2](../includes/winblue-server-2-md.md)] 上的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 需要这些服务。

    > [!WARNING]  
    >  不要安装 WebDAV 发布角色服务。 WebDAV 发布与 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]不兼容。  
  
     |角色服务|角色服务|  
    |-----------------------------|-----------------------------|  
    |![mds_AddRolesFeaturesWizard_RoleServicesPage](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage.png)|![mds_AddRolesFeaturesWizard_RoleServicesPage_PerformSecurity](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-performsecurity.png)|  
    |![mds_AddRolesFeaturesWizard_RoleServicesPage_AppDevsection](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-appdevsection.png)|![mds_AddRolesFeaturesWizard_RoleServicesPage_ManageToolssection](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-managetoolssection.png)|  
    |||  
  
     有关在其他操作系统上所需的功能和角色服务的列表，请参阅 [Web 应用程序要求 &#40;Master Data Services&#41;](../master-data-services/install-windows/web-application-requirements-master-data-services.md)。   
  
 有关使用安装程序安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的详细信息，请参阅[使用安装向导安装 SQL Server 2016（安装程序）](../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)。  
  
 有关使用命令提示符安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的详细信息，请参阅 [从命令提示符安装 SQL Server 2016](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)。 使用命令提示符时， [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 作为功能参数提供。  
  
 有关链接到与预安装任务相关的其他信息的简要说明，请参阅 [安装 Master Data Services](../master-data-services/install-windows/install-master-data-services.md)。  
  
##  <a name="setting-up-the-database-and-website"></a><a name="SetUpWeb"></a>设置数据库和网站  
 **使用 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 设置数据库和网站**  

 
> [!WARNING]
>  启动 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 配置管理器之前，必须[安装 IIS](#InstallIIS)。 否则，Configuration Manager 将显示“Internet Information Services 错误”消息，不能创建 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Web 应用程序。  
> 
> 浏览器要求
> >[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Web 应用程序仅适用于 Internet Explorer (IE) 9 或更高版本。 IE 8 及早期版本、Microsoft Edge 和 Chrome 均不受支持。    
  
1.  启动 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]，然后单击左窗格中的“数据库配置”。  
  
2.  单击“创建数据库”，然后在“创建数据库向导”中单击“下一步”。  
  
3.  在 " **数据库服务器** " 页上，指定 SQL Server 实例。 

    >  [!INCLUDE[sqlv15](../includes/sssql19-md.md)] 添加对 SQL Server 托管实例的支持。 将 **SQL Server 实例** 的值设置为托管实例的主机。 例如，`xxxxxx.xxxxxx.database.windows.net`。

4. 选择 **身份验证类型** ，然后单击 " **测试连接** " 以确认你可以使用所选身份验证类型的凭据连接到数据库。 单击“下一步”。

    >对于 [!INCLUDE[sqlv15](../includes/sssql19-md.md)] ，若要连接到托管实例，请使用以下身份验证类型之一：
    >
    >- Azure Active Directory 集成身份验证： **当前用户– Active Directory 集成**
    >- SQL Server 身份验证： **SQL Server 帐户**。
    >
    >在 SQL 托管实例中，用户必须是 `sysadmin` 固定服务器角色的成员。

    > [!NOTE]  
    >  选择“当前用户 - 集成安全性”作为身份验证类型时，“用户名”框为只读，并且显示登录到计算机的 Windows 用户帐户的名称。 如果在 Azure 虚拟计算机 (VM) 上运行 [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]，“用户名”框显示 VM 名称以及该 VM 上本地管理员帐户的用户名。 

    ![mds_2016ConfigManager_CreateDatabaseWizard_ServerPage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-serverpage.png)  
  
4.  在“数据库名称”字段中键入名称。 （可选）若要选择 Windows 排序规则，请清除“SQL Server 默认排序规则”复选框，单击一个或多个可用选项，如“区分大小写”。 单击“下一步”。

    ![mds_2016ConfigManager_CreateDatabaseWizard_DatabasePage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-databasepage.png)  
  
     有关 Windows 排序规则的详细信息，请参阅 [Windows 排序规则名称 (Transact-SQL)](../t-sql/statements/windows-collation-name-transact-sql.md)。  
  
5.  在“用户名”字段中，指定将成为 Master Data Services 默认超级用户的用户 Windows 帐户。 超级用户有权访问所有功能区域，并且可以添加、删除和更新所有模型。  

    ![mds_2016ConfigManager_CreateDatabaseWizard_AdminPage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-adminpage.png)  
  
6.  单击 " **下一步** " 查看数据库的设置摘要 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ，然后再次单击 " **下一步** " 以创建数据库。 出现“进度和完成”页。

7. 创建和配置数据库后，单击“完成”。  
  
     有关“创建数据库向导”中的设置的详细信息，请参阅[创建数据库向导（Master Data Services 配置管理器）](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md)。  
  
7.  在 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 中的“数据库配置”页上，单击“选择数据库”。  
  
8.  单击“连接”，选择在步骤 7 中创建的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库，然后单击“确定”。 

    ![mds_2016ConfigManager_SelectDatabaseButton_ConnectToDatabaseDialog](../master-data-services/media/mds-2016configmanager-selectdatabasebutton-connecttodatabasedialog.png)  
  
     你已经完成了数据库的设置。 “数据库配置”页现在将显示为 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]（创建的数据库和当前的数据库版本）连接的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例。  

    ![mds_2016ConfigManager_DatabaseConfig_Completed](../master-data-services/media/mds-2016configmanager-databaseconfig-completed.png)   
  
9. 在 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 中，单击左窗格中的“Web 配置”。  
  
10. 在“网站”列表框中，单击“默认网站”，然后单击“创建”以创建 Web 应用程序。  
  
    > [!NOTE]  
    >  选择“默认网站”时，必须创建一个 Web 应用程序。 如果在列表框中选择“创建新网站”，将自动创建应用程序。  

     ![mds_2016ConfigManager_WebConfig](../master-data-services/media/mds-2016configmanager-webconfig.png)  
  
11. 在“应用程序池”部分中，执行下列操作之一。  
  
    -   为数据库“管理员帐户”输入在第 5 步中输入的同一用户名称，输入密码，然后单击“确定”。  
  
         **或**  
  
    -   输入不同的用户名称，输入密码，然后单击“确定”。  
  
         创建数据库和 Web 应用程序时，不必使用相同的帐户。  

        ![mds_2016ConfigManager_WebConfig_CreateWebApplication](../master-data-services/media/mds-2016configmanager-webconfig-createwebapplication.png)   
  
     有关“创建 Web 应用程序”对话框的详细信息，请参阅[创建 Web 应用程序对话框（Master Data Services 配置管理器）](../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md)。  

    > [!NOTE] 
    >  如果域实现 [Windows 的 2020 ldap 通道绑定和 ldap 签名要求](https://support.microsoft.com/en-us/help/4520412/2020-ldap-channel-binding-and-ldap-signing-requirements-for-windows)。 你将看到 "无法在 Active Directory 中验证凭据" 问题。 使用域帐户创建应用程序池时。 要解决此问题，请使用 **本地计算机用户**，而不是域用户。 这可以绕过凭据检查 Active Directory。 创建 web 应用程序后，可以在 **Internet Information Services (IIS) 管理器** 中将标识更改为域用户。
  
12. 在“Web 配置”页的“Web 应用程序”框中，单击已创建的应用程序，然后单击“将应用程序与数据库相关联”部分中的“选择”。  
  
13. 单击“连接”，选择想要与 Web 应用程序相关联的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库，然后单击“确定”。  
  
     你已经完成了网站的设置。 “Web 配置”页现在将显示你所选的网站、创建的 Web 应用程序以及与应用程序相关联的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库。  

     ![mds_2016ConfigManager_WebConfig_Completed](../master-data-services/media/mds-2016configmanager-webconfig-completed.png)  
 
     
15. 单击“应用”。 显示“配置完成”消息框。 在消息框中单击“确定”，启动 Web 应用程序。 网站地址为 "https://*server 名称* / *web 应用程序*/"。 


![mds_2016ConfigurationComplete_MessageBox](../master-data-services/media/mds-2016configurationcomplete-messagebox.png) 
  
有关“Web 配置”页上的设置的详细信息，请参阅[“Web 配置”页（Master Data Services 配置管理器）](../master-data-services/web-configuration-page-master-data-services-configuration-manager.md)  
  
 你还可以使用 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 指定与 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库相关联的 Web 应用程序和服务的其他设置。 例如，你可以指定加载数据的频率或发送验证电子邮件的频率。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md)。  
  
##  <a name="deploying-sample-models-and-data"></a><a name="deploySample"></a>部署示例模型和数据  
 以下三个示例模型包都包含在  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中。   这些示例模型包括数据。 示例模型包的默认位置为 %programfiles%\Microsoft SQL Server\140\Master Data Services\Samples\Packages
  
-   chartofaccounts_en.pkg  
-   customer_en.pkg  
-   product_en.pkg  
  
 可以使用 MDSModelDeploy 工具部署这些包。 MDSModelDeploy 工具的默认位置为 驱动器\Program Files\Microsoft SQL Server\ 140\Master Data Services\Configuration。  
  
 有关运行此工具的先决条件的信息，请参阅 [使用 MDSModelDeploy 部署模型部署包](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)。  
  
 有关在 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 中更新数据以支持新功能的信息，请参阅 [SQL Server 示例：模型部署包 (MDS)](../master-data-services/sql-server-samples-model-deployment-packages-mds.md)。  
  
 **部署示例模型**  
  
1.  将示例模型包复制到驱动器\Program Files\Microsoft SQL Server\140\Master Data Services\Configuration。  
  
2.  通过运行以下命令打开管理员命令提示符，然后导航到 MDSModelDeploy.exe。  
  
    ```  
    cd c:\Program Files\Microsoft SQL Server\140\Master Data Services\Configuration  
    ```  
  
3.  通过运行以下每个命令将每个示例模型部署到 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 。  
  
    > [!IMPORTANT]  
    >  在下面的示例中， `MDS1` 服务值是指定的。 设置 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 网站时，如果选择了“默认网站”，则使用此值。  请参阅 [设置数据库和网站](#SetUpWeb) 部分。  
    >   
    >  如果创建了一个新的网站，或选择了另一个现有网站，首先运行以下命令以确定正确的服务值。  
    >   
    >  `MDSModelDeploy listservices`  
    >   
    >  返回的值列表中，第一个服务值为指定用于部署模型的值。  

    > [!NOTE]
    > 为了解更多有关示例模型的元数据信息，请参考“c:\Program Files\Microsoft SQL Server\140\Master Data Services\Configuration”中提供的自述文件
   
     **部署 chartofaccounts_en.pkg 示例模型**  
  
    ```console
    MDSModelDeploy deploynew -package chartofaccounts_en.pkg -model ChartofAccounts -service MDS1  
    ```  
  
     **部署 customer_en.pkg 示例模型**  
  
    ```console
    MDSModelDeploy deploynew -package customer_en.pkg -model Customer -service MDS1  
    ```  
  
     **部署 product_en.pkg 示例模型**  
  
    ```console
    MDSModelDeploy deploynew -package product_en.pkg -model Product -service MDS1  
    ```  
  
     成功部署某一模型后，将显示“MDSModelDeploy 操作已完成”消息。  
  
     下图显示部署 product_en.pkg 示例模型的命令。  
  
     ![用于部署产品示例模型的命令行](../master-data-services/media/mds-commandprompt-deployingsamplemodel-product.png "用于部署产品示例模型的命令行")  
  
4.  若要查看示例模型，执行以下操作。  
  
    1.  导航到你设置的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 网站。 请参阅 [设置数据库和网站](#SetUpWeb) 部分。  
  
         网站地址为 "https://*server 名称* / *web 应用程序*/"。  
  
    2.  从“模型”列表框中选择模型，然后单击“资源管理器”。  
  
         ![MDS 网站、主页。](../master-data-services/media/mds-mdswebsite-homepage-selectsamplemodel.png "MDS 网站、主页。")  
  
## <a name="next-step"></a>下一步  
 为你的数据创建一个新的模型和实体。 请参阅[创建模型 (Master Data Services)](../master-data-services/create-a-model-master-data-services.md) 和[创建实体 (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md)。  
  
 有关如何使用模型和实体为 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 中的数据生成结构的概述，请参阅 [Master Data Services 概述 (MDS)](../master-data-services/master-data-services-overview-mds.md)  
    
## <a name="see-also"></a>另请参阅  
 [Master Data Services 数据库](../master-data-services/master-data-services-database.md)   
 [主数据管理器 Web 应用程序](../master-data-services/master-data-manager-web-application.md)   
 ["数据库配置" 页 &#40;Master Data Services 配置管理器&#41;](../master-data-services/database-configuration-page-master-data-services-configuration-manager.md)   
 [Master Data Services (MDS) 中的新增功能](../master-data-services/what-s-new-in-master-data-services-mds.md)  
  
