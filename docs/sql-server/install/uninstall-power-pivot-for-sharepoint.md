---
title: 卸载 Power Pivot for SharePoint | Microsoft Docs
description: 本文介绍如何卸载 Power Pivot for SharePoint 安装，这是由多个步骤构成的操作。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 3941a2f0-0d0c-4d1a-8618-7a6a7751beac
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 375f351c1d80e957fba29f81b9d2afc423c222ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336096"
---
# <a name="uninstall-power-pivot-for-sharepoint"></a>卸载 Power Pivot for SharePoint
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  卸载 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 安装是一个由多个步骤构成的操作，包括准备卸载、从场中删除功能和解决方案以及删除程序文件和注册表设置。  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2013 | SharePoint 2010  
  
 **本文内容：**  
  
-   [先决条件](#prereq)  
  
-   [步骤 1：卸载前一览表](#bkmk_before)  
  
-   [步骤 2：从 SharePoint 删除功能和解决方案](#bkmk_remove)  
  
-   [步骤 3：运行 SQL Server 安装程序以便从本地计算机中删除程序](#bkmk_uninstall)  
  
-   [步骤 4：卸载 Power Pivot for SharePoint 加载项](#bkmk_addin)  
  
-   [步骤 5：验证卸载情况](#verify)  
  
-   [步骤 6：卸载后一览表](#bkmk_post)  
  
##  <a name="prerequisites"></a><a name="prereq"></a>先决条件  
  
-   您必须是 SharePoint 场管理员或服务应用程序管理员，才能在场中卸载功能和解决方案。  
  
-   如果您还要卸载数据库引擎，必须是 SQL Server 系统管理员和本地 Administrators 组的成员。  
  
-   您必须是 Analysis Services 系统管理员和本地 Administrators 组的成员才能卸载 Analysis Services 和 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]。  
  
##  <a name="step-1-pre-uninstall-checklist"></a><a name="bkmk_before"></a> 步骤 1：卸载前一览表  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据访问。 作为第一步，您应该主动删除不再正常运行的文件和库。 这样，可以在卸载软件前解决与“缺少数据”有关的任何问题。  
  
1.  删除与 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 的安装相关联的所有 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿、文档和库。 一旦卸载软件后，库和文档都将失效。  
  
    -   [删除 Power Pivot 库](/analysis-services/power-pivot-sharepoint/delete-power-pivot-gallery)  
  
    -   [删除 Power Pivot 数据馈送库](/analysis-services/power-pivot-sharepoint/delete-a-power-pivot-data-feed-library)  
  
2.  删除包含或引用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据的其他库中的 Excel 工作簿或 Reporting Services 报表。  
  
3.  删除引用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据的 PerformancePoint 面板中的所有 Web 部件。  
  
4.  查看针对现有站点和库的 SharePoint 权限，以便确定是否将需要更改它们。 某些 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据访问方案（尤其是通过 URL 连接字符串访问其他工作簿中的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据的辅助数据访问）要求“读取”权限，此权限高于通常分配给只能访问站点的 SharePoint 用户的“查看”权限。 如果您不再要求“读取”权限，则可以相应降低权限。  
  
5.  或者，在停止服务后等上几天，然后再卸载软件。 此步骤并非卸载所必需的，但在您解决可能疏漏的任何数据迁移或技术替代问题时，将能够选择暂时恢复服务。  
  
##  <a name="step-2-remove-features-and-solutions-from-sharepoint"></a><a name="bkmk_remove"></a> 步骤 2：从 SharePoint 删除功能和解决方案  
 使用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具从 SharePoint 中删除 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务和应用程序。  
  
-   你必须是场管理员、Analysis Services 实例上的服务器管理员和场的配置数据库上的“db_owner”。  
  
-   使用适合 SharePoint 版本的配置工具版本。 不能对 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 安装使用这两个工具。  
  
-   验证“SharePoint 管理”服务是否正在运行。  
  
1.  **运行配置文件：** 注意，仅当 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 安装在本地服务器上时，才会列出这些配置工具。请在“开始”  菜单上，指向“所有程序” ，依次单击 [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]、“配置工具” ，然后单击以下项之一：  
  
    -   **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 2013 配置**  
  
    -   **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具**  
  
2.  选择 **“删除功能、服务、应用程序和解决方案”** ，然后单击 **“确定”** 。  
  
3.  也可以将窗口放大为实际大小。 您应该在该窗口的底部看到一个菜单栏，其中包含 **“验证”** 、 **“运行”** 和 **“退出”** 命令。  
  
4.  检查任务列表中的每个操作，了解每个操作的作用。  
  
     在“删除 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务应用程序”中，你可以选择删除与服务应用程序相关联的应用程序数据。 应用程序数据是随服务应用程序创建的 SQL Server 数据库，目的在于存储数据刷新计划、数据库实例信息、使用情况数据，以及 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 所使用的其他数据。 它并不存储用户文件，如 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿。 除非您有特定原因要保留应用程序数据（例如，您有与数据刷新或数据访问相关的数据保留策略），否则您可以删除应用程序数据库，而不删除 SharePoint 用户创建或保存的任何文件。  
  
     若要删除该数据库，请选择“删除 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务应用程序”，然后选择“删除与此服务应用程序相关联的应用程序数据”。  
  
5.  或者，检查 **“输出”** 选项卡或 **“脚本”** 选项卡中的详细信息。  
  
     “输出”选项卡汇总了将由该工具执行的所有操作。 此信息保存在位于以下位置的日志文件中：  
  
     `C:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\SPAddinConfiguration\Log`  
  
     “脚本”选项卡显示 PowerShell cmdlet，或引用该工具将运行的 PowerShell 脚本文件。  
  
6.  单击 **“验证”** 可检查每个操作是否有效。 如果 **“验证”** 不可用，这意味着所有操作都适用于您的系统。  
  
7.  单击 **“运行”** 执行对此任务有效的所有操作。 只有通过验证检查后， **“运行”** 才可用。 当你单击“运行”时，出现以下警告，提醒你将在批处理模式下执行操作：“该工具中所有标记为有效的配置设置将应用于 SharePoint 场。 是否继续?”  
  
8.  单击 **“是”** 继续操作。  
  
 **解决错误：**  
  
 在配置工具中，可以在“参数”窗格中查看每个操作的错误信息。 对于与解决方案部署或收回相关的问题，请验证是否已启动 SharePoint 管理服务。 此服务运行可触发场中配置更改的计时器作业。 如果该服务未运行，解决方案部署或收回将失败。 持续出现的错误表示现有的部署或收回作业已存在于队列中，阻止配置工具执行进一步的操作。 可以使用以下 PowerShell 命令来验证服务是否正在运行。  
  
```  
Get-Service | where {$_.displayname -like "*sharepoint* administration*"}  
```  
  
 若要查找并删除已存在于队列中的部署或收回作业，请执行以下操作：  
  
1.  对于其他所有错误，请查看 ULS 日志。 有关详细信息，请参阅[配置和查看 SharePoint 日志文件和诊断日志记录 (Power Pivot for SharePoint)](/analysis-services/power-pivot-sharepoint/configure-and-view-sharepoint-and-diagnostic-logging)。  
  
2.  以管理员身份启动 SharePoint Management Shell，然后运行以下命令查看队列中的作业：  
  
    ```  
    Stsadm -o enumdeployments  
    ```  
  
3.  检查现有部署的以下信息：“类型”是收回或部署，“文件”为 powerpivotwebapp.wsp 或 powerpivotfarm.wsp 。  
  
4.  对于与 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案有关的部署或收回，请复制“JobId”的 GUID 值，然后将其粘贴到以下命令中（使用 Shell 的“编辑”菜单上的“标记”、“复制”和“粘贴”命令复制该 GUID）：  
  
    ```  
    Stsadm -o canceldeployment -id "<GUID>"  
    ```  
  
5.  通过依次单击 **“验证”** 和 **“运行”** ，在该配置工具中重试该任务。  
  
 或者，您可以使用 PowerShell 从场中删除功能和解决方案。 有关详细信息，请参阅 [针对 Power Pivot for SharePoint 的 PowerShell 参考](/analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint)。  
  
##  <a name="step-3-run-sql-server-setup-to-remove-programs-from-the-local-computer"></a><a name="bkmk_uninstall"></a> 步骤 3：运行 SQL Server 安装程序以便从本地计算机中删除程序  
 删除程序文件时，需运行 SQL Server 安装程序来卸载软件。 卸载程序将删除安装程序已创建的文件和注册表项。 您可以使用“程序和功能”页来卸载软件。 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 的安装是 SQL Server 安装的一部分。  
  
 可以卸载部分安装而不影响已安装的其他 SQL Server 实例（或同一实例中的功能）。 例如，可以卸载 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 而保留安装的其他组件，例如 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 或数据库引擎。  
  
1.  从程序列表中选择“Microsoft SQL Server 2014 (64 位)”。  
  
2.  单击“卸载/更改”。  
  
3.  单击 **“删除”** 。 这将启动 SQL Server 安装程序。  
  
     你可以从安装程序中选择 **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** 实例，然后选择“Analysis Services”  和“Analysis Services SharePoint 集成”  ，这样就可以只删除该功能，而保留其他功能。  
  
##  <a name="step-4-uninstall-the-power-pivot-for-sharepoint-add-in"></a><a name="bkmk_addin"></a> 步骤 4：卸载 Power Pivot for SharePoint 加载项  
 如果您的 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 部署具有两个或更多服务器且您安装了 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 外接程序，则从安装它的每个服务器卸载 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 外接程序以完全卸载所有 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 文件。 有关详细信息，请参阅 [安装或卸载 Power Pivot for SharePoint 外接程序 (SharePoint 2013)](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)。  
  
##  <a name="step-5-verify-uninstall"></a><a name="verify"></a> 步骤 5：验证卸载情况  
  
1.  在管理中心的“管理服务器上的服务” 中，连接到卸载了 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 的服务器。  
  
2.  -   如果卸载了 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013，请验证“SQL Server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 系统服务”不再显示在列表中。  
  
    -   如果卸载了 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2010，请验证“SQL Server Analysis Services”和“SQL Server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 系统服务”不再显示在列表中。  
  
3.  卸载场中的最后一个 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 服务器后，请执行以下操作：  
  
    1.  在“应用程序管理”的“管理服务应用程序” 中，验证“ [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务应用程序”不再显示在列表中。  
  
    2.  在“系统设置”的“管理场功能” 中，验证“ [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 集成功能”不再出现在该页上。 在“管理场解决方案” 中，验证 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案不再出现在该页上。  
  
    3.  在“监视”的“配置诊断日志记录”  和“配置使用情况和运行状况数据收集” 中，验证 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 事件和事件类别不再出现。  
  
    4.  在“常规应用程序设置”中，验证“[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 管理面板”不再出现在该页上。  
  
##  <a name="step-6-post-uninstall-checklist"></a><a name="bkmk_post"></a> 步骤 6：卸载后一览表  
 使用下表删除在卸载过程中未删除的软件和文件。  
  
1.  从 `C:\Program Files\Microsoft SQL Server\MSAS13.PowerPivot`删除所有数据文件和子文件夹，然后删除该文件夹。 此步骤还将删除 DATA 目录中以前缓存的文件。  
  
2.  如果你尚未删除所有 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿、文档和库，请删除它们。  
  
    -   [删除 Power Pivot 库](/analysis-services/power-pivot-sharepoint/delete-power-pivot-gallery)  
  
    -   [删除 Power Pivot 数据馈送库](/analysis-services/power-pivot-sharepoint/delete-a-power-pivot-data-feed-library)  
  
3.  在 Secure Store Service 中，删除包含 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 使用的存储凭据的所有目标应用程序。 在你卸载 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 时，Secure Store Service 中的某些条目（但不是全部条目）将会被删除。 为 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 无人参与的数据刷新帐户创建的目标应用程序以及你为数据刷新创建的所有目标应用程序仍然存在，因此必须手动删除。  
  
     相反， [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 系统服务自动生成的单独目标应用程序将在卸载 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 时被自动删除。  
  
4.  在控制面板中，单击 **“程序”** ，然后单击 **“卸载程序”** 。卸载不再使用的任何 Analysis Services 客户端库。 卸载 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 时，将不删除 Analysis Services ADOMD.NET 和 Microsoft SQL Server 分析管理对象。 因为这些库可能由使用 Analysis Services 数据的其他程序使用，所以，SQL Server 安装程序将不会自动卸载它们。 如果不再需要，您必须单独卸载这些客户端库。  
  
     除非您在执行故障排除或者安装说明明确指示您进行卸载，否则，不要卸载 SQL Server Reporting Services SharePoint 2010 外接程序。 该 Reporting Services 外接程序由 Access Services 使用。 它是由 SharePoint 产品准备工具安装的，并且应保留在系统上以便支持 SharePoint 所需的功能。  
  
     不要卸载 Analysis Services OLE DB 访问接口。 SharePoint 将 OLE DB 访问接口作为连接到 Analysis Services 数据库的 Excel 工作簿的必备软件安装。 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 将安装更新的版本，但此版本是可以向后兼容的，因此，您应该在系统上保留此版本以免以后出现数据连接问题。  
  
## <a name="see-also"></a>另请参阅  
 [安装或卸载 Power Pivot for SharePoint 外接程序 (SharePoint 2013)](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)   
 [Power Pivot 配置工具](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools)  
  
