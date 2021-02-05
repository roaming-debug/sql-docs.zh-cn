---
description: 升级 Power Pivot for SharePoint
title: 升级 Power Pivot for SharePoint | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 80ba9e43-f3f0-4730-9fb1-2afd2dd3e6fc
author: Minewiskan
ms.author: owend
monikerRange: '>=sql-server-2016'
manager: erikre
ms.openlocfilehash: 2f034ac2223b15a3b66883a3ce3dbc434bcf07db
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813251"
---
# <a name="upgrade-power-pivot-for-sharepoint"></a>升级 Power Pivot for SharePoint

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  本文概述将 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 的部署升级到 [!INCLUDE[ssGeminiLong](../../includes/ssgeminilong-md.md)] 所需的步骤。 具体步骤取决于环境中当前运行的 SharePoint 版本，并包括 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 外接程序 (**spPowerPivot.msi**)。  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2010 | SharePoint 2013  
  
 有关发行说明，请参阅 [SQL Server 2016 发行说明](../../sql-server/sql-server-2016-release-notes.md)。  
  
 **本文内容：**  
  
 [先决条件](#bkmk_prereq)  
  
 [升级现有的 SharePoint 2013 场](#bkmk_uprgade_sharepoint2013)  
  
 [升级现有的 SharePoint 2010 场](#bkmk_uprgade_sharepoint2010)  
  
 [工作簿](#bkmk_workbooks)  
  
 [数据刷新](#bkmk_datarefresh)  
  
 [验证 Power Pivot 组件和服务的版本](#bkmk_verify_versions)  
  
 [升级 SharePoint 场中的多个 Power Pivot for SharePoint 服务器](#geminifarm)  
  
 [将 QFE 应用于场中的 Power Pivot 实例](#qfe)  
  
 [升级后的验证任务](#verify)  
  
## <a name="background"></a>背景  
  
-   如果要对具有两个或更多 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 实例的多服务器 SharePoint 2010 场进行升级，则必须在完全升级每个服务器 **后** ，才能继续对下一服务器进行升级。 完全升级包括运行 SQL Server 安装程序以升级 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 程序文件，然后执行 SharePoint 升级操作以便配置升级后的服务。 在适当的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具或 Windows PowerShell 中执行升级操作之前，服务器的可用性将受到限制。  
  
-   SharePoint 2010 场中 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 系统服务和 Analysis Services 的所有实例必须为相同版本。 有关如何验证版本的信息，请参阅本文中的[验证 Power Pivot 组件和服务的版本](#bkmk_verify_versions)部分。  
  
-   [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具是 SQL Server 共享功能之一，所有共享功能同时升级。 如果在升级过程中选择需要共享功能升级的其他 SQL Server 实例或功能，则将一同升级 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具。 如果该 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具已升级，但你的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 实例未升级，可能会导致问题。 有关 SQL Server 共享功能的详细信息，请参阅[使用安装向导（安装程序）升级到 SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)。  
  
-   [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 外接程序 (**spPowerPivot.msi**) 与先前版本并行安装。 例如，该外接程序安装到文件夹 `c:\Program Files\Microsoft SQL Server\nnn\Tools\PowerPivotTools`。 有关 SQL Server 安装文件的信息，请参阅[文件位置](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md#shared-files-for-all-instances-of-)。
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a>先决条件  
 **权限**  
  
-   你必须是场管理员才能对 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 安装进行升级。 您必须是本地管理员才能运行 SQL Server 安装程序。  
  
-   必须对场配置数据库具有 **db_owner** 权限。  
  
 **SQL Server：**  
  
-   如果现有 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 安装是 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]，则必须安装 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Service Pack 2 (SP2) 才能升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]。  
  
-   如果现有 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 安装是 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]，则必须安装 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Service Pack 1 (SP1) 才能升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]。  
  
 **SharePoint 2010：**  
  
-   如果现有安装运行的是 SharePoint 2010，则必须安装 SharePoint 2010 Service Pack 2 才能升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)][!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]。 有关详细信息，请参阅 [Service Pack 2 for Microsoft SharePoint 2010](https://www.microsoft.com/download/details.aspx?id=39672)。 使用 PowerShell 命令 `(Get-SPfarm).BuildVersion.ToString()` 验证版本。 若要引用发布日期之前的内部版本，请参阅 [SharePoint 2010 内部版本号](https://www.toddklindt.com/blog/Lists/Posts/Post.aspx?ID=224)。  
  
##  <a name="upgrade-an-existing-sharepoint-2013-farm"></a><a name="bkmk_uprgade_sharepoint2013"></a> 升级现有的 SharePoint 2013 场  
 若要升级在 SharePoint 2013 中部署的 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] ，请执行以下操作：  
  
 ![PowerPivot for SharePoint 2013 升级](../../database-engine/install-windows/media/as-powepivot-upgrade-flow-sharepoint2013.png "PowerPivot for SharePoint 2013 升级")  
  
1.  在以 SharePoint 模式运行 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的后端服务器上运行 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 安装程序。 如果服务器承载 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的多个实例，请至少升级 **POWERPIVOT** 实例。 以下列表概述了与 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 升级相关的安装向导步骤：  
  
    1.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导中单击 **“安装”** 。  
  
    2.  单击“从 SQL Server 升级...”。  
  
    3.  在 **“选择实例”** 页上，选择 **POWERPIVOT** 实例名，然后单击 **“下一步”** 。  
  
    4.  有关详细信息，请参阅[使用安装向导（安装程序）升级到 SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
  
2.  重新启动服务器。  
  
3.  在 SharePoint 2013 场中的每个服务器上运行 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 外接程序 (**spPowerPivot.msi**) 以安装数据访问接口。 您从中运行 SQL Server 安装向导的服务器除外，安装向导也将升级数据访问接口。 有关详细信息，请参阅[下载 Microsoft SQL Server 2014 Power Pivot for Microsoft SharePoint 2013](https://www.microsoft.com/download/details.aspx?id=42300) 和[安装或卸载 Power Pivot for SharePoint 外接程序 (SharePoint 2013)](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)。  
  
4.  **在 SharePoint 2013 场中的每个服务器上运行 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 2013 配置** 工具，以使用外接程序安装的更新解决方案文件配置 SharePoint 场。 不能使用 SharePoint 管理中心执行此步骤。 有关详细信息，请参阅以下主题：  
  
    1.  在 Windows“开始”页上，键入 **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** ，然后在搜索结果中单击 **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] SharePoint 2013 配置**。 请注意，搜索可能会返回配置工具的两个版本。  
  
         ![两个 PowerPivot 配置工具](/analysis-services/analysis-services/instances/install-windows/media/as-powerpivot-configtools-bothicons.gif "两个 PowerPivot 配置工具")  
  
         或  
  
         在“开始”菜单上，指向“所有程序”，然后依次单击 [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)] “配置工具”和 “SharePoint 2013 配置工具” **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** 。 请注意，只有在本地服务器上安装了 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 后，才会列出此工具。  
  
    2.  启动时，该配置工具会检查 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 场解决方案和 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] web 应用程序解决方案的升级状态。 如果检测到这些解决方案的较早版本，将看到消息“**已检测到 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案文件的更高版本。请选择升级选项以升级场**。” 单击“确定”  关闭系统验证消息。  
  
    3.  单击 **“升级功能、服务、应用程序和解决方案”** ，然后单击 **“确定”** 。  
  
    4.  查看左侧窗格任务列表中的操作，并排除您不希望该工具执行的所有操作。 默认情况下包括所有操作。 若要删除某个操作，请在左侧任务列表中选择此操作，然后在 **“参数”** 页上清除 **“在任务列表中包括此操作”** 复选框。  
  
    5.  或者，查看 **“脚本”** 或 **“输出”** 选项卡中的详细信息。  
  
         “输出”选项卡汇总了将由该工具执行的所有操作。 此信息保存在位于 `C:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\SPAddinConfiguration\Log`的日志文件中。  
  
         “脚本”选项卡显示 PowerShell cmdlet，或引用该工具将运行的 PowerShell 脚本文件。  
  
    6.  单击 **“验证”** 可检查每个操作是否有效。 如果 **“验证”** 不可用，这意味着所有操作都适用于您的系统。 如果“验证” 可用，你可能修改了某个输入值（例如 Excel 服务应用程序名称），或是该工具可能已确定无法执行某个操作。 如果无法执行某个操作，您必须排除它，或修复导致该操作被标记为无效的基本条件。  
  
        > [!IMPORTANT]  
        >  第一项操作 **“升级场解决方案”** 必须始终最先处理。 它注册用于配置服务器的 PowerShell cmdlet。 如果此操作出错，不要继续操作。 应该使用错误中提供的信息诊断并解决该问题，然后继续处理任务列表中的其他操作。  
  
    7.  单击 **“运行”** 执行对此任务有效的所有操作。 只有通过验证检查后， **“运行”** 才可用。 当你单击“运行”时，出现以下警告，提醒你将在批处理模式下执行操作：“该工具中所有标记为有效的配置设置将应用于 SharePoint 场 **。是否继续?** ”。  
  
    8.  单击 **“是”** 继续操作。  
  
    9. 升级场中的解决方案和功能可能要花几分钟才能完成。 在此期间，对 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据的连接请求“将失败”，并显示类似“无法刷新数据”或“试图执行请求操作期间出错  **。请重试**。” 升级完成后，服务器将变为可用，这些错误将不会再出现。  
  
     有关详细信息，请参阅以下主题：  
  
    -   [Power Pivot 配置工具](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools)  
  
    -   [配置或修复 Power Pivot for SharePoint 2013（Power Pivot 配置工具）](/analysis-services/power-pivot-sharepoint/configure-or-repair-power-pivot-for-sharepoint-2013)  
  
    -   [使用 Windows PowerShell 配置 Power Pivot](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-using-windows-powershell)  
  
    -   [针对 Power Pivot for SharePoint 的 PowerShell 参考](/analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint)  
  
5.  请通过执行升级后步骤以及通过检查场中 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务器的版本，确认升级成功。 有关详细信息，请参阅本文中的[升级后的验证任务](#verify)以及下面的部分。  
  
##  <a name="upgrade-an-existing-sharepoint-2010-farm"></a><a name="bkmk_uprgade_sharepoint2010"></a> 升级现有的 SharePoint 2010 场  
 若要升级在 SharePoint 2010 中部署的 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] ，请执行以下操作：  
  
 ![PowerPivot for SharePoint 2010 升级](../../database-engine/install-windows/media/as-powepivot-upgrade-flow-sharepoint2010.png "PowerPivot for SharePoint 2010 升级")  
  
1.  下载 [Service Pack 2 for Microsoft SharePoint 2010](https://www.microsoft.com/download/details.aspx?id=39672) 并将其应用于场中的所有服务器。 验证 SharePoint SP2 安装是否成功。 在管理中心中的“升级和迁移”页上，打开“检查产品和修补程序安装状态”页，以查看与 SP2 相关的状态消息。  
  
2.  验证“SharePoint 2010 管理”Windows 服务是否正在运行。  
  
    ```  
    Get-Service | where {$_.displayname -like "*SharePoint*"}  
    ```  
  
3.  验证 **SharePoint** 服务 **SQL Server Analysis Services** 和 **SQL Server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 系统服务** 是否已在 SharePoint 管理中心启动，或使用以下 PowerShell 命令：  
  
    ```  
    get-SPserviceinstance | where {$_.typename -like "*sql*"}  
    ```  
  
4.  验证 **Windows** 服务 **SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])** 是否正在运行。  
  
    ```  
    Get-Service | where {$_.displayname -like "*powerpivot*"}  
    ```  
  
5.  在运行 **SQL Server Analysis Services([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])** Windows 服务的第一台 SharePoint 应用程序服务器上 **运行[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]安装程序**，以升级 POWERPIVOT 实例。 在 SQL Server 安装向导的“安装”页上选择升级选项。 有关详细信息，请参阅 [使用安装向导（安装程序）升级到 SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)。  
  
6.  在运行配置工具前 **重新启动服务器** 。 此步骤可确保 SQL Server 安装程序安装的所有更新或必备组件在系统上得到完全配置。  
  
7.  **在 SharePoint 2013 场中的每个服务器上运行 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ) 服务的第一台 SharePoint 应用程序服务器上** 配置工具[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]，以在 SharePoint 中升级解决方案和 Web 服务。 不能使用管理中心执行此步骤。  
  
    1.  在“开始”菜单上，指向“所有程序”，依次单击 [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]、“配置工具”和“配置工具” **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** 。 请注意，只有在本地服务器上安装了 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 后，才会列出此工具。  
  
    2.  启动时，该配置工具会检查 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 场解决方案和 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] web 应用程序解决方案的升级状态。 如果检测到这些解决方案的较早版本，将看到消息“已检测到 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案文件的更高版本。 请选择升级选项以升级场。” 单击“确定”  关闭消息框。  
  
    3.  单击 **“升级功能、服务、应用程序和解决方案”** ，然后单击 **“确定”** 继续操作。  
  
    4.  将出现以下警告：“[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 管理面板中的工作簿将要升级到最新版本。 您对现有工作簿进行的所有定制都将丢失。 是否继续?”  
  
         此警告指的是报告数据刷新活动的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 管理面板中的工作簿。 如果您已经自定义了这些工作簿，当使用新版本替换现有文件时，对这些工作簿的所有修改都将丢失。  
  
         单击 **“是”** 可使用较新版本覆盖这些工作簿。 否则，单击 **“否”** 可返回主页。 将工作簿保存到不同位置，以便留有副本，然后在准备好继续操作后返回到此步骤。  
  
         有关自定义面板中使用的工作簿的详细信息，请参阅 [自定义 Power Pivot 管理面板](/previous-versions/sql/sql-server-2008-r2/ff718155(v=sql.105))。  
  
    5.  查看任务列表中的操作，并排除您不希望该工具来执行的所有操作。 默认情况下包括所有操作。 若要删除某个操作，请在任务列表中选择它，然后清除“参数”页上的 **“在任务列表中包括此操作”** 复选框。  
  
    6.  或者，检查 **“输出”** 选项卡或 **“脚本”** 选项卡中的详细信息。  
  
         “输出”选项卡汇总了将由该工具执行的所有操作。 此信息保存在位于 `c:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\ConfigurationTool\Log`的日志文件中。  
  
         “脚本”选项卡显示 PowerShell cmdlet，或引用该工具将运行的 PowerShell 脚本文件。  
  
    7.  单击 **“验证”** 可检查每个操作是否有效。 如果 **“验证”** 不可用，这意味着所有操作都适用于您的系统。 如果“验证” 可用，你可能修改了某个输入值（例如 Excel 服务应用程序名称），或是该工具可能已确定无法执行某个操作。 如果无法执行某个操作，您必须排除它，或修复导致该操作被标记为无效的基本条件。  
  
        > [!IMPORTANT]  
        >  第一项操作 **“升级场解决方案”** 必须始终最先处理。 它注册用于配置服务器的 PowerShell cmdlet。 如果此操作出错，不要继续操作。 应该使用错误中提供的信息诊断并解决该问题，然后继续处理任务列表中的其他操作。  
  
    8.  单击 **“运行”** 执行对此任务有效的所有操作。 只有通过验证检查后， **“运行”** 才可用。 当你单击“运行”时，出现以下警告，提醒你将在批处理模式下执行操作：“该工具中所有标记为有效的配置设置将应用于 SharePoint 场。 是否继续?”  
  
    9. 单击 **“是”** 继续操作。  
  
    10. 升级场中的解决方案和功能可能要花几分钟才能完成。 在此期间，针对 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据的连接请求将失败，并显示“无法刷新数据”或“试图执行请求操作期间出错。 请重试。” 升级完成后，服务器将变为可用，这些错误将不会再出现。  
  
8.  对场中的每个 SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) 服务重复以下过程：1) 运行 SQL Server 安装程序 2) 运行 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具。  
  
9. 请通过执行升级后步骤以及通过检查场中 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务器的版本，确认升级成功。 有关详细信息，请参阅本文中的[升级后的验证任务](#verify)以及下面的部分。  
  
10. **解决错误**  
  
     您可以在“参数”窗格中查看每个操作的错误信息。  
  
     对于与解决方案部署或收回相关的问题，请验证是否已启动 SharePoint 2010 管理服务。 此服务运行可触发场中配置更改的计时器作业。 如果该服务未运行，解决方案部署或收回将失败。 持续出现的错误表示现有的部署或收回作业已存在于队列中，阻止配置工具执行进一步的操作。  
  
    1.  以管理员身份启动 SharePoint 2010 Management Shell，然后运行以下命令查看队列中的作业：  
  
        ```  
        Stsadm -o enumdeployments  
        ```  
  
    2.  检查现有部署的以下信息：“类型”是收回或部署，“文件”为 powerpivotwebapp.wsp 或 powerpivotfarm.wsp 。  
  
    3.  对于与 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案有关的部署或收回，请复制“JobId”的 GUID 值，然后将其粘贴到以下命令中（使用 Shell 的“编辑”菜单上的“标记”、“复制”和“粘贴”命令复制该 GUID）：  
  
        ```  
        Stsadm -o canceldeployment -id "<GUID>"  
        ```  
  
    4.  通过依次单击 **“验证”** 和 **“运行”** ，在该配置工具中重试该任务。  
  
     对于其他所有错误，请查看 ULS 日志。 有关详细信息，请参阅[配置和查看 SharePoint 日志文件和诊断日志记录 (Power Pivot for SharePoint)](/analysis-services/power-pivot-sharepoint/configure-and-view-sharepoint-and-diagnostic-logging)。  
  
##  <a name="workbooks"></a><a name="bkmk_workbooks"></a> 工作簿  
 升级服务器不一定升级在其上运行的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿，但在 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for Excel 早期版本中创建的旧工作簿会使用在该版本中提供的功能继续像以前那样工作。 工作簿仍正常工作，因为已升级的服务器具有是以前安装一部分的 Analysis Services OLE DB 访问接口。  
  
##  <a name="data-refresh"></a><a name="bkmk_datarefresh"></a> 数据刷新  
 升级将影响数据刷新操作。 服务器上的预定数据刷新仅可用于与服务器版本匹配的工作簿。 如果承载的是先前版本的工作簿，对于这些工作簿，数据刷新可能不再工作。 要重新启用数据刷新，您必须升级工作簿。 可以在 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for Excel 中手动升级每个工作簿，或为 SharePoint 2010 中的数据刷新功能启用自动升级。 自动更新功能会在运行数据刷新前将工作簿升级到当前版本，而使数据刷新操作仍按计划执行。  
  
##  <a name="verify-the-versions-of-power-pivot-components-and-services"></a><a name="bkmk_verify_versions"></a> 验证 Power Pivot 组件和服务的版本  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 系统服务和 Analysis Services 的所有实例必须为相同版本。 若要验证所有服务器组件是否都属于同一版本，请检查以下各项的版本信息：  
  
### <a name="verify-the-version-of-power-pivot-solutions-and-the-power-pivot-system-service"></a>验证 Power Pivot 解决方案和 Power Pivot 系统服务的版本  
 运行以下 PowerShell 命令：  
  
```  
Get-PowerPivotSystemService  
```  
  
 验证 **CurrentSolutionVersion**。 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 为版本 13.0.\<major build>.\<minor build>  
  
### <a name="verify-the-version-of-the-analysis-services-windows-service"></a>验证 Analysis Services Windows 服务的版本  
 如果您只升级了 SharePoint 2010 场中的某些 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 服务器，则未升级服务器上的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例可能比场中预期的版本旧。 您需要将所有的服务器升级到相同的版本，以便它们可以使用。 使用以下方法之一验证每台计算机上 SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) Windows 服务的版本。  
  
 **Windows 文件资源管理器**：  
  
1.  导航到 **实例的** Bin [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 文件夹。 例如，`C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT\OLAP\bin`。  
  
2.  右键单击 `msmdsrv.exe` 并选择“属性”。  
  
3.  单击“详细信息”。  
  
4.  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 文件版本应为 13.00.\<major build>.\<minor build>。  
  
5.  验证此版本号是否与 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案和系统服务版本相同。  
  
 **服务启动信息：**  
  
 当 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务启动时，它会将版本信息写入到 Windows 事件日志。  
  
1.  运行 Windows `eventvwr`  
  
2.  对源 `MSOLAP$POWERPIVOT`创建筛选器。  
  
3.  查找如下信息级别事件  
  
     服务已启动。 Microsoft SQL Server Analysis Services 64 位 Evaluation (x64) RTM **13.0.2000.8**。  
  
 **使用 PowerShell 验证文件版本。**  
  
 可以使用 PowerShell 验证产品版本。 如果要对版本验证编写脚本或自动化版本验证，PowerShell 是个很好的选择。  
  
```  
(get-childitem "C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT2000\OLAP\bin\msmdsrv.exe").VersionInfo  
```  
  
 以上 PowerShell 命令将返回如下信息：  
  
 ProductVersion   FileVersion           FileName  
  
 **13.0.2000.8** 2016.0130.200    C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT2000\OLAP\bin\msmdsrv.exe  
  
### <a name="verify-the-msolap-data-provider-version-on-sharepoint"></a>验证 SharePoint 上的 MSOLAP 数据访问接口版本  
 使用以下指令来检查 Excel Services 信任的 Analysis Services OLE DB 访问接口版本。 您必须是场或服务应用程序管理员，才能检查 Excel Services 信任的数据访问接口设置。  
  
1.  在“管理中心”的“应用程序管理”中，单击 **“管理服务应用程序”** 。  
  
2.  单击 Excel Services 服务应用程序的名称，例如 **ExcelServiceApp1**。  
  
3.  单击 **“受信任的数据访问接口”** 。 您应看到 MSOLAP.5 (Microsoft OLE DB Provider for OLAP Services 11.0)。 如果升级了 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 安装，您还将看到先前版本的 MSOLAP.4。  
  
4.  有关详细信息，请参阅 [将 MSOLAP.5 添加为 Excel Services 中的受信任数据访问接口](/analysis-services/power-pivot-sharepoint/add-msolap-5-as-a-trusted-data-provider-in-excel-services)。  
  
 MSOLAP.4 被描述为“用于 OLAP Services 10.0 的 Microsoft OLE DB 数据访问接口”。 此版本可能是 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 中与 Excel Services 一起安装的默认版本，也可能是 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 版本。 SharePoint 安装的默认版本不支持 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据访问。 必须具有 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 版本或更高版本才能连接到 SharePoint 上的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿。 若要验证您是否具有 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 版本，请使用前一部分中介绍如何通过查看文件属性来验证版本的说明。  
  
### <a name="verify-the-adomdnet-data-provider-version"></a>验证 ADOMD.NET 数据访问接口版本  
 使用以下指令检查安装的 ADOMD.NET 版本。 您必须是场或服务应用程序管理员，才能检查 Excel Services 信任的数据访问接口设置。  
  
1.  在 SharePoint 应用程序服务器上，请浏览到 `c:\Windows\Assembly`。  
  
2.  按程序集名称排序并查找 **Microsoft.Analysis Services.Adomd.Client**。  
  
3.  验证版本号是否为 13.0.\<build number>。  
  
##  <a name="upgrading-multiple-power-pivot-for-sharepoint-servers-in-a-sharepoint-farm"></a><a name="geminifarm"></a> 升级 SharePoint 场中的多个 Power Pivot for SharePoint 服务器  
 在包含多个 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 服务器的多服务器拓扑中，所有服务器实例和组件都必须为同一版本。 运行最高版本软件的服务器为场中的所有服务器设置级别。 如果您只升级某些服务器，则运行旧版软件的服务器将变得不可用，直到它们也升级之后才可用。  
  
 升级了第一台服务器后，尚未升级的其他服务器将 **变为不可用**。 当所有服务器都运行在同一级别后将还原可用性。  
  
 SQL Server 安装程序可对物理计算机上已有的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 解决方案文件进行升级，但若要对场正在使用的解决方案进行升级，则必须使用本文先前部分中介绍的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具。  
  
##  <a name="applying-a-qfe-to-a-power-pivot-instance-in-the-farm"></a><a name="qfe"></a> 将 QFE 应用于场中的 Power Pivot 实例  
 为 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 服务器应用修补程序时，将使用包含特定问题的修补程序的较新版本来更新现有的程序文件。 当将 QFE 应用到多服务器拓扑时，并不存在您必须从其开始的主服务器。 你可以从任何服务器开始，只要您将同一个 QFE 应用到场中的其他 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 服务器。  
  
 当您应用 QFE 时，必须还执行一个配置步骤，该步骤将在场配置数据库中更新服务器版本信息。 已应用修补程序的服务器版本将成为场的新预期版本。 在所有计算机上都应用和配置 QFE 前，不具有 QFE 的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 实例将无法用于处理针对 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据的请求。  
  
 为确保正确应用和配置 QFE，请按照以下说明执行：  
  
1.  使用随 QFE 提供的说明安装修补程序。  
  
2.  启动 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具。  
  
3.  单击 **“升级功能、服务、应用程序和解决方案”** ，然后单击 **“确定”** 。  
  
4.  查看在升级任务中包含的操作，然后单击 **“验证”** 。  
  
5.  单击 **“运行”** 以便应用操作。  
  
6.  对场中的其他 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 实例重复上述步骤。  
  
    > [!IMPORTANT]  
    >  在多服务器部署中，请务必在继续下一台计算机之前修补和配置每个实例。 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具必须首先为当前实例完成升级任务，然后才能移到下一个实例。  
  
 若要检查场中服务的版本信息，请使用管理中心的“升级和修补程序管理”部分中的 **“查看产品和修补程序的安装状态”** 页。  
  
##  <a name="post-upgrade-verification-tasks"></a><a name="verify"></a> 升级后的验证任务  
 升级完成后，请使用以下步骤确认服务器可正常运行。  
  
|任务|链接|  
|----------|----------|  
|验证服务在运行 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 的所有计算机上正常运行。|[启动或停止 Power Pivot for SharePoint Server](/analysis-services/power-pivot-sharepoint/start-or-stop-a-power-pivot-for-sharepoint-server)|  
|在网站集级别验证功能激活。|[在管理中心中针对网站集激活 Power Pivot 功能集成](/analysis-services/power-pivot-sharepoint/activate-power-pivot-integration-for-site-collections-in-ca)|  
|通过打开工作簿并单击筛选器和切片器来启动查询，验证各个 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿是否正常加载。|检查硬盘上是否存在缓存的文件。 如果存在缓存文件，则确认已在该物理服务器上加载了数据文件。 在 c:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT\OLAP\Backup 文件夹中查找缓存文件。|  
|在为数据刷新配置的所选工作簿上测试数据刷新。|测试数据刷新的最简单方法是修改数据刷新计划，并且选中 **“也尽快刷新”** 复选框以便数据刷新立即运行。 此步骤将确定数据刷新对于当前工作簿是否成功。 对其他常用工作簿重复上述步骤，以便确保数据刷新正常执行。 有关计划数据刷新的详细信息，请参阅 [计划数据刷新 (Power Pivot for SharePoint)](/sharepoint/administration/data-refresh-using-the-unattended-data-refresh-account)。|  
|在一段时间后，监视 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 管理面板中的数据刷新报表，以确认没有发生数据刷新错误。|[Power Pivot 管理仪表板和使用情况数据](/analysis-services/power-pivot-sharepoint/power-pivot-management-dashboard-and-usage-data)|  
  
 有关如何配置 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 设置和功能的详细信息，请参阅 [在管理中心中管理和配置 Power Pivot 服务器](/analysis-services/power-pivot-sharepoint/power-pivot-server-administration-and-configuration-in-central-administration)。  
  
 关于指导你完成所有安装后配置任务的分步说明，请参阅 [初始配置 (Power Pivot for SharePoint)](/sharepoint/administration/configure-power-pivot-for-sharepoint-2013)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [Power Pivot for SharePoint 2010 安装](https://sharepointgeorge.com/2012/installing-sql-server-powerpivot-sharepointstep-step-guide/)  
  
