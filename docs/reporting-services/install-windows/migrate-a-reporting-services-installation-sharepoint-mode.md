---
description: 迁移 Reporting Services 安装（SharePoint 模式）
title: 迁移 Reporting Services 安装（SharePoint 模式）| Microsoft Docs
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 61290949-690a-4e19-b078-57c99b6b30fa
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: 9281c38d56b85cda96b9a5f9888ff707bc4e139a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596208"
---
# <a name="migrate-a-reporting-services-installation-sharepoint-mode"></a>迁移 Reporting Services 安装（SharePoint 模式）

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE [ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  此主题概述将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式部署从一个 SharePoint 环境迁移到另一环境所需的步骤。 具体步骤可能会根据您正在从其迁移的版本而有所不同。 有关 SharePoint 模式的升级和迁移方案的详细信息，请参阅 [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)。 如果你只想要将报表项从一个服务器复制到另一个服务器，请参阅[用于在报表服务器之间复制内容的示例 Reporting Services rs.exe 脚本](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)。  
  
 有关迁移 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式部署的信息，请参阅 [迁移 Reporting Services 安装（本机模式）](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)。  
  
 完成迁移的常见原因是要将 SharePoint 2010 部署升级到 SharePoint 2013/2016。 SharePoint 2013/2016 不支持从 SharePoint 2010 就地升级，必须完成 **database-attach 升级** 过程或仅内容迁移。  
  
 有关升级 SharePoint 2013/2016 的详细信息，请参阅以下内容：  

-   [升级到 SharePoint 2016 的过程概述](https://technet.microsoft.com/library/cc262483\(v=office.16\))。

-   [升级到 SharePoint 2013 的过程概述](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process-from-sharepoint-2010-to-sharepoint-2013)。
  
-   [升级到 SharePoint 2013 之前的清除准备](/SharePoint/upgrade-and-update/clean-up-an-environment-before-an-upgrade-to-sharepoint-2013)。  
  
-   [将数据库从 SharePoint 2013 升级到 SharePoint 2016](https://technet.microsoft.com/library/cc303436\(v=office.16\))。

-   [将数据库从 SharePoint 2010 升级到 SharePoint 2013](/SharePoint/upgrade-and-update/upgrade-content-databases-from-sharepoint-2010-to-sharepoint-2013)。
  
-   [在 SharePoint 2016 中移动内容数据库](https://technet.microsoft.com/library/cc262792\(v=office.16\).aspx)。

-   [在 SharePoint 2013 中移动内容数据库](/SharePoint/administration/move-content-databases)。
  
##  <a name="migrate-from-reporting-services-sharepoint-mode-versions-prior-to-sql-server-2012"></a><a name="bkmk_prior_versions"></a> 从 SQL Server 2012 之前的 Reporting Services SharePoint 模式版本迁移  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式体系结构在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]中已更改，包括服务应用程序数据库架构。 如果要从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 之前的版本迁移到 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] SharePoint 模式，请先通过安装 SharePoint 和 SQL Server 2016 Reporting Services SharePoint 模式创建新的 SharePoint 环境。 有关详细信息，请参阅 [安装 Reporting Services SharePoint 模式](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)。  
  
 新 SharePoint 环境运行后，可以在包括内容数据库的数据库级别选择仅内容迁移或完全迁移。  
  
###  <a name="content-only-migration"></a><a name="bkmk_content_only_migration"></a> 仅限迁移的内容  
 **Reporting Services 仅内容迁移：** 如果要将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 内容复制到新场，则需要使用工具（如 **rs.exe** ）将内容复制到新 SharePoint 安装。 有关仅内容迁移的详细信息，请参阅以下内容：  
  
-   **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] RSS 脚本：** 此脚本可以在本机模式与 SharePoint 模式报表服务器之间迁移内容和资源。 有关详细信息，请参阅 [用于在报表服务器之间复制内容的示例 Reporting Services rs.exe 脚本](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md) 和 [从一个报表服务器迁移至另一报表服务器的 Reporting Services RS.exe 脚本](https://azuresql.codeplex.com/releases/view/115207)。  
  
-   **Reporting Services 迁移工具：** 该工具可以将报表项从本机模式服务器复制到 SharePoint 模式服务器。 有关详细信息，请参阅 [Reporting Services 迁移工具](https://www.microsoft.com/download/details.aspx?id=29560) (https://www.microsoft.com/download/details.aspx?id=29560) 。  
  
###  <a name="full-migration"></a><a name="bkmk_full_migration"></a> 完全迁移  
 **完全迁移：** 如果您将 SharePoint 内容数据库与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 目录数据库一起迁移到新的场中，则可以按照在本文中介绍的一系列备份和还原选项执行。 在某些情况下，您将需要使用与在备份阶段中使用的不同工具来用于还原阶段。 例如，你可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的先前版本备份加密密钥，但需要使用 SharePoint 管理中心或 PowerShell 将加密密钥还原到 SQL Server 2016 Reporting Services SharePoint 模式安装。  
  
####  <a name="databases-you-will-see-in-the-completed-migration"></a><a name="bkmk_databases"></a> 您将在完成的迁移中看到的数据库  
 下表描述了与在您成功迁移了 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 安装后将具有的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 相关的 SQL Server 数据库：  
  
|数据库|示例名称|说明|  
|--------------|------------------|-|  
|目录数据库|ReportingService_[service application GUID] (&#42;)|用户迁移。|  
|Temp 数据库|ReportingService_[service application GUID]TempDB (&#42;)|用户迁移。|  
|警报数据库|ReportingService_[服务应用程序 GUID]_Alerting|在创建 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序时创建。|  
  
 (&#42;) 该表中所示的示例名称遵循在你创建新的 SSRS 服务应用程序时 SSRS 使用的命名约定。 如果您在从不同的服务器进行迁移，则您的目录和 tempDB 将具有来自原始安装的名称。  
  
####  <a name="backup-operations"></a><a name="bkmk_backup_operations"></a> 备份操作  
 本节介绍迁移所需的信息的类型以及用于完成备份的工具或过程。  
  
 ![SSRS SharePoint 迁移的基本关系图](../../reporting-services/install-windows/media/rs-sharepoint-migration.gif "SSRS SharePoint 迁移的基本关系图")  
  
|Item|对象|方法|说明|  
|-|-------------|------------|-----------|  
|**1**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 加密密钥。|**Rskeymgmt.exe** 或 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器。 请参阅 [备份和还原 Reporting Services 加密密钥](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)。|指出的工具可用于备份操作；但对于还原操作，您将使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序管理页或 PowerShell。|  
|**2**|SharePoint 内容数据库。||备份数据库和分离数据库。<br /><br /> 请参阅[确定升级方法 (SharePoint Server 2010) (/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013)](/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013) 中的“数据库附加升级”部分。|  
|**3**|作为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]目录数据库的 SQL Server 数据库。|SQL Server 数据库备份和还原<br /><br /> 或<br /><br /> SQL Server 数据库分离和附加。||  
|**4**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置文件。|简单文件复制。|如果您对文件进行了自定义，则仅需复制 rsreportserver.config。 文件默认位置的示例：C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting\\*：<br /><br /> <br /><br /> RSReportServer.config<br /><br /> Rssvrpolicy.config<br /><br /> 针对报表服务器 ASP.NET 应用程序的 Web.config。<br /><br /> 针对 ASP.NET 的 Machine.config|  
  
####  <a name="restore-operations"></a><a name="bkmk_restore_operations"></a> 还原操作  
 本节介绍迁移所需的信息的类型以及用于完成还原的工具或过程。 您用于还原的工具可能会不同于用于备份的工具。  
  
 在完成还原步骤前，需要安装和配置新的 SharePoint 场和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式。 有关 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式的基本安装的详细信息，请参阅 [安装 Reporting Services SharePoint 模式](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)。  
  
|Item|对象|方法|说明|  
|-|-------------|------------|-----------|  
|**1**|将 SharePoint 内容数据库还原到新的场。|SharePoint“数据库附加升级”方法。|基本步骤：<br /><br /> 1) 在新的服务器上还原数据库。<br /><br /> 2) 通过指示 URL 将内容数据库附加到 Web 应用程序。<br /><br /> 3) Get-SPWebapplication 将列出所有 Web 应用程序和 URL。<br /><br /> <br /><br /> 请参阅[确定升级方法 (SharePoint Server 2010) (/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013)](/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013) 和[附加数据库和升级到 SharePoint Server 2010 (/SharePoint/upgrade-and-update/upgrade-content-databases)](/SharePoint/upgrade-and-update/upgrade-content-databases) 中的“数据库附加升级”部分。|  
|**2**|还原作为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 目录数据库 (ReportServer) 的 SQL Server 数据库。|SQL Database 库备份和还原。<br /><br /> **or**<br /><br /> SQL Server 数据库分离和附加。|首次使用数据库时，[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 将根据需要更新数据库架构，以便数据库可用于 SQL Server 2016 环境。|  
|**3**|创建新的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序。|创建新的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序。|在您创建新的服务应用程序时，将其配置为使用您复制到的报表服务器数据库。<br /><br /> 有关使用 SharePoint 管理中心的详细信息，请参阅[在 SharePoint 模式下安装第一个报表服务器](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md)中的“步骤 3：创建 Reporting Services 服务应用程序”。<br /><br /> 有关使用 PowerShell 的示例，请参阅 [Reporting Services SharePoint 服务和服务应用程序](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md)中的“使用 PowerShell 创建 Reporting Services 服务应用程序”一节。|  
|**4**|还原 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置文件。|简单文件复制。|文件默认位置的示例：C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting。|  
|**5**|还原 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]加密密钥。|使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序“SystemSettings”页还原密钥备份文件。<br /><br /> **or**<br /><br /> PowerShell。|请参阅[管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)主题中的“密钥管理”部分。|   
  
##  <a name="migrate-from-a-sql-server-2012-or-sql-server-2014-deployment"></a><a name="bkmk_migrate_from_ctp"></a> 从 SQL Server 2012 或 SQL Server 2014 部署迁移  
 在多服务器场中，用户可能会在不同计算机上分别具有内容数据库和目录数据库，在此情况下，您实际上仅需要将安装有 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的新服务器添加到 SharePoint 场中，然后从旧服务器上删除它。 应该不需要复制数据库。  
  
### <a name="backup-operations"></a>备份操作  
  
1.  备份 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 加密密钥。  
  
2.  在 SharePoint 管理中心中（或者使用 PowerShell）备份 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序。 这还将备份 SharePoint 中的服务应用程序数据库。 请参阅  [备份和还原 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md)主题  
  
3.  如果您具有无人参与的执行帐户 (UEA) 和 Windows 身份验证，请记下凭据，以便在还原过程使用这些凭据。  
  
4.  有关详细信息，请参阅 [在 SharePoint 2013 中备份服务应用程序](/SharePoint/administration/back-up-a-service-application)。  
  
### <a name="restore-operations"></a>还原操作  
  
1.  使用 SharePoint 管理中心还原 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序。 您也可以使用 PowerShell。  
  
2.  还原 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 加密密钥。  
  
     请参阅[管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)主题中的“密钥管理”部分  
  
3.  对服务应用程序配置 UEA 和 Windows 凭据。  
  
4.  有关详细信息，请参阅 [在 SharePoint 2013 中还原服务应用程序](/SharePoint/administration/restore-a-service-application)。  
  
##  <a name="additional-resources"></a><a name="bkmk_additional_resources"></a> 其他资源  
  
-   [开始升级到 SharePoint 2013 (/SharePoint/upgrade-and-update/get-started-with-upgrade)](/SharePoint/upgrade-and-update/get-started-with-upgrade)。  
  
-   [SharePoint 2013 升级过程概述 (/SharePoint/upgrade-and-update/overview-of-the-upgrade-process)](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process)。  

## <a name="next-steps"></a>后续步骤

[升级和迁移 Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[迁移 Reporting Services 安装](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)