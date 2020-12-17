---
description: 用于 SSRS 服务应用程序的设置订阅和警报
title: 用于 SSRS 服务应用程序的设置订阅和警报 | Microsoft Docs
ms.date: 06/03/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services Shared Service
- SharePoint Mode [Reporting Services]
- SharePoint Mode
- Reporting Services Service Application
- SSRS service application
ms.assetid: d0de3f1f-4887-47fb-bacf-46aaad74c4be
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 04b8fa62adfc5cb78ec52f2d87617f9136042aa7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97402556"
---
# <a name="provision-subscriptions-and-alerts-for-ssrs-service-applications"></a>用于 SSRS 服务应用程序的设置订阅和警报
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅和数据警报需要 SQL Server 代理，还需配置 SQL Server 代理权限。 如果您看到指示“需要 SQL Server 代理”的错误消息，而您已验证 SQL Server 代理正在运行，则您需要更新或验证权限。 本主题限于 SharePoint 模式中的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，并说明使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅来更新 SQL Server 代理权限的三种方式。 对于服务应用程序、msdb 和 master 数据库中的对象，在本主题的步骤中使用的凭据必须具有足够的权限来将执行权限授予 RSExecRole。  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** SharePoint 2016 | SharePoint 2013|  
  
 ![对服务应用程序数据库的 SQL 代理权限](../../reporting-services/install-windows/media/rs-provisionsqlagent.gif "对服务应用程序数据库的 SQL 代理权限")  
  
||说明|  
|------|-----------------|  
|**1**|承载 Reporting Services 服务应用程序数据库的 SQL Server 数据库引擎实例。|  
|**2**|SQL 数据库引擎实例的 SQL Server 代理实例。|  
|**3**|Reporting Services 服务应用程序数据库。 基于用于创建服务应用程序的信息的名称。 下面是示例数据库的名称：<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0_Alerting<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0TempDB|  
|**4**|SQL Server 数据库引擎实例的 master 和 MSDB 数据库。|  
  
 使用以下三种方法之一更新权限：  
  
1.  在 **“设置、订阅和警报”** 页上，键入凭据并单击 **“确定”**。  
  
2.  在“设置、订阅和警报”页上单击 **“下载脚本”** 按钮，以便下载可用于配置权限的 transact SQL 脚本。  
  
3.  运行 PowerShell cmdlet，以便生成可用于配置权限的 transact SQL 脚本。  
  
### <a name="to-update-permissions-using-the-provision-page"></a>使用“设置”页更新权限  
  
1.  在 SharePoint 管理中心内，在 **“应用程序管理”** 组中单击 **“管理服务应用程序”**  
  
2.  在列表中找到您的服务应用程序，然后单击该应用程序的名称或者单击 **“类型”** 列以选择该服务应用程序，接着单击 SharePoint 功能区中的 **“管理”** 按钮。  
  
3.  在 **“管理 Reporting Services 应用程序”** 页上，单击 **“设置订阅和警报”**。  
  
4.  如果 SharePoint 管理员具有针对 Master 数据库和服务应用程序数据库的足够权限，请键入那些凭据。  
  
5.  单击“确定”按钮。  
  
##  <a name="to-download-the-transact-sql-script"></a><a name="bkmk_download"></a> 下载 Transact-SQL 脚本  
  
1.  在 SharePoint 管理中心内，在 **“应用程序管理”** 组中单击 **“管理服务应用程序”**  
  
2.  在列表中找到您的服务应用程序，然后单击该应用程序的名称或者单击 **“类型”** 列以选择该服务应用程序，接着单击 SharePoint 功能区中的 **“管理”** 按钮。  
  
3.  在 **“管理 Reporting Services 应用程序”** 页上，单击 **“设置订阅和警报”**。  
  
4.  在 **“查看状态”** 区域中，验证 SQL Server 代理是否正在运行。  
  
5.  单击 **“下载脚本”** ，以便下载可以在 SQL Server Management Studio 中运行的 transact SQL 脚本，从而授予权限。 创建的脚本文件的名称将包含你的 Reporting Services 服务应用程序的名称，例如 **[服务应用程序的名称]-GrantRights.sql**。  
  
### <a name="to-generate-the-transact-sql-statement-with-powershell"></a>利用 PowerShell 生成 Transact-SQL 语句  
  
1.  你也可以在 SharePoint 2016（或 SharePoint 2013）Management Shell 中使用 Windows PowerShell cmdlet 创建 Transact-SQL 脚本。  
  
2.  在 **“开始”** 菜单上，单击 **“所有程序”**。  
  
3.  展开“Microsoft SharePoint 2016 产品”，然后单击“SharePoint 2016 Management Shell”。
  
4.  通过替换报表服务器数据库的名称、应用程序池帐户和语句路径来更新下面的 PowerShell cmdlet。  
  
     **cmdlet 的语法：** `Get-SPRSDatabaseRightsScript -DatabaseName <ReportingServices database name> -UserName <app pool account> -IsWindowsUser | Out-File <path of statement>`  
  
     **示例 cmdlet：** `Get-SPRSDatabaseRightsScript -DatabaseName ReportingService_46fd00359f894b828907b254e3f6257c -UserName "NT AUTHORITY\NETWORK SERVICE" -IsWindowsUser | Out-File c:\SQLServerAgentrights.sql`  
  
## <a name="using-the-transact-sql-script"></a>使用 Transact-SQL 脚本  
 以下过程可用于通过“设置”页下载的脚本或使用 PowerShell 创建的脚本。  
  
#### <a name="to-load-the-transact-sql-script-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中加载 Transact-SQL 脚本  
  
1.  要打开 SQL Server Management Studio，请在“开始”菜单上单击“[!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]”，然后单击“SQL Server Management Studio”。  
  
2.  在 **“连接到服务器”** 对话框中，设置以下选项：  
  
    -   在 **“服务器类型”** 列表中，选择 **“数据库引擎”**  
  
    -   在 **“服务器名称”** 中键入要在其上配置 SQL Server 代理的 SQL Server 实例的名称。  
  
    -   选择身份验证模式。  
  
    -   如果使用 SQL Server 身份验证进行连接，请提供登录名和密码。  
  
3.  单击“连接”。  
  
#### <a name="to-run-the-transact-sql-statement"></a>运行 Transact-SQL 语句  
  
1.  在 SQL Server Management Studio 的工具栏上，单击 **“新建查询”**。  
  
2.  在 **“文件”** 菜单上，单击 **“打开”**，然后单击 **“文件”**。  
  
3.  定位到保存 SharePoint 2016（或 SharePoint 2013）Management Shell 中生成的 Transact-SQL 语句的文件夹。  
  
4.  单击该文件，然后单击 **“打开”**。  
  
     该语句将添加到查询窗口。  
  
5.  单击“执行” 。  
  
  
