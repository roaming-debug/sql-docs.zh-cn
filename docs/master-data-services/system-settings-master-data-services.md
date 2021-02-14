---
title: 系统设置
description: 了解如何为与 Master Data Services 数据库关联的所有 web 应用程序和 web 服务配置系统设置。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- Master Data Services, system settings
- system settings [Master Data Services]
ms.assetid: 83075cdf-f059-4646-8ba2-19be8202f130
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 75033975cf0f06f7f21c75edbacd40349ddc5ad1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343589"
---
# <a name="system-settings-master-data-services"></a>系统设置 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  对于与 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库相关联的所有 Web 应用程序和 Web 服务，您都可以配置系统设置。  
  
 其中的许多设置都可在 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 的 **“数据库”** 页上进行配置。 其他配置可以在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库的系统设置表 (mdm.tblSystemSetting) 中进行配置。  
  
 这些设置可分为以下几类：  
  
-   [常规设置](#General)  
  
-   [版本管理设置](#Versions)  
  
-   [临时设置](#Staging)  
  
-   [资源管理器设置](#Explorer)  
  
-   [Excel 外接程序设置](#xls)  
  
-   [业务规则设置](#BusinessRules)  
  
-   [通知设置](#Notifications)  
  
-   [安全设置](#Security)  
  
-   [未使用](#NotUsed)  
  
##  <a name="general-settings"></a><a name="General"></a> 常规设置  
  
|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
|**数据库连接超时**|**DatabaseConnectionTimeOut**|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库允许连接完成所花的秒数。 如果连接未在该时间内完成，则会取消连接并返回错误。 默认值为 **60** 秒（1 分钟）。|  
|**数据库命令超时**|**DatabaseCommandTimeOut**|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库允许命令完成所花的秒数。 如果命令未在该时间内完成，则会取消命令并返回错误。 默认值为 **3600** 秒 (60 分钟) 。|  
|**Web 服务超时**|**ServerTimeOut**|ASP.NET 允许用在完成 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 页请求上的秒数。 如果请求未在该时间内完成，则会取消请求并返回错误。 默认值为 **120000** 秒（2000 分钟）。|  
|**客户端超时**|**ClientTimeOut**|[!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 返回主页前处于非活动状态的秒数。 默认值为 **300** 秒（5 分钟）。|  
|**每批的行数**|**RowsPerBatch**|Web 服务要在每批中检索的记录数。 默认值为“50” 。|  
||ApplicationName|显示在事件日志中的文本。 默认值为 **MDM**。|  
||**SiteTitle**|显示在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 浏览器标题栏中的文本。 默认值为 **“主数据管理器”**。|  
|**日志保留期（天）**|**LogRentionDays**|在多少天后日志将被删除。 默认值为 -1，表示系统不会清除日志表。<br /><br /> 如果值为 0，则日志表只会保留当天的数据。 前几天的数据日志都将被截断。<br /><br /> 如果值大于 0，系统会保留值所指定的天数的日志数据。|  
  
##  <a name="version-management-settings"></a><a name="Versions"></a> 版本管理设置  
  
|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
|**仅复制提交的版本**|**CopyOnlyCommittedVersion**|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，确定用户是只能复制状态为 **“已提交”** 的模型版本，还是可以复制任何状态的版本。 默认值为 **“是”** 或 **“1”**，表示用户只能复制 **“已提交”** 版本。 更改为 **“否”** 或 **“2”** 将允许用户复制所有版本。|  
  
 有关详细信息，请参阅[版本 (Master Data Services)](../master-data-services/versions-master-data-services.md)。  
  
##  <a name="staging-settings"></a><a name="Staging"></a> 暂存设置  
  
|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
|**记录所有临时事务**|**StagingTransactionLogging**|仅适用于 SQL Server 2008 R2。 确定在临时记录加载到 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库时是否要将事务记入日志。 默认值为 **“关闭”** 或 **“2”**。 更改为 **“打开”** 或 **“1”** 可启用日志记录。|  
|**临时批处理间隔**|**StagingBatchInterval**|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **“集成管理”** 功能区域，从您选择 **“开始批处理”** 到处理您的批次之间相隔的秒数。 默认值为 **60** 秒（1 分钟）。|  
  
 有关详细信息，请参阅 [概述：从表导入数据 &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)。  
  
##  <a name="explorer-settings"></a><a name="Explorer"></a> 资源管理器设置  
  
|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
|**层次结构中的默认成员数**|**HierarchyChildNodeLimit**|在 “资源管理器”功能区域，显示“...更多...”之前每个层次结构中显示的最大成员数[!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 。 可以单击“...更多...”以显示下一组成员。 默认值为“50” 。|  
|**默认显示层次结构中的名称**|**ShowNamesInHierarchy**|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **“资源管理器”** 功能区域，确定查看层次结构时选择的默认设置。<br /><br /> 默认值为 **“是”** 或 **“1”**，表示显示每个成员的名称和代码。 更改为 **“否”** 或 **“2”** 可仅显示代码。|  
|**列表中基于域的属性的数目**|**DBAListRowLimit**|在  的“资源管理器”[!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 功能区域，双击网格中基于域的属性值时显示在列表中的属性数。 默认值为“50” 。 如果存在的成员超过 50，则会改为显示一个可搜索对话框。|  
||**GridFilterDefaultFuzzySimilarityLevel**|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **“资源管理器”** 功能区域，使用 **“匹配”** 筛选条件时使用的相似性级别。 默认值为“0.3” 。 设置的值越接近 **1** ，返回的匹配项就越接近搜索条件。 设置为 **1** 表示完全匹配。|  
  
##  <a name="add-in-for-excel-settings"></a><a name="xls"></a> 用于 Excel 的外接程序设置  
  
|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
|在网站主页上显示 Excel 外接程序文本|ShowAddInText|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 主页上，显示一个链接方便用户下载 [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]。|  
|网站主页上 Excel 外接程序的安装路径|AddInURL|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 主页上，如果显示指向 [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] 的链接，用户单击该链接时转到的位置。|  
  
##  <a name="business-rule-settings"></a><a name="BusinessRules"></a> 业务规则设置  
  
|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
|**将新业务规则递增的数字**|**BusinessRuleDefaultPriorityIncrement**|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **“系统管理”** 功能区域，将每个新业务规则的优先级递增的数字。 默认值为“10”。|  
|**要将业务规则应用到的成员数**|**BusinessRuleRealtimeMemberCount**|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **“资源管理器”** 功能区域，要将业务规则应用到的网格中最大成员数。 在 [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]中，要将业务规则应用到的活动工作表中最大成员数。 默认值为 **10000**。|  
|**首先执行业务规则用户脚本**|**BusinessRuleUserScriptExecuteFirst**|通常情况下，业务规则操作按以下顺序执行：“默认值”、“更改值”、“验证”、“外部操作”、“用户定义的操作脚本”。 如果将此设置更改为 1，则业务规则操作首先执行“用户定义的操作脚本”。 此设置为隐藏设置。 默认值为 **0**。|  
  
 有关详细信息，请参阅[业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)。  
  
##  <a name="notification-settings"></a><a name="Notifications"></a> 通知设置  
  
|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
|**用于通知的主数据管理器 URL**|**MDMRootURL**|[!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] web 应用程序的 URL，在电子邮件通知的链接中使用，例如 `https://constoso/mds`。|  
|**通知电子邮件间隔**|**NotificationInterval**|发送电子邮件通知的频率（以秒为单位）。 默认值为 **120** 秒（2 分钟）。|  
|**单个电子邮件中的通知数**|**NotificationsPerEmail**|将在单个通知电子邮件中列出的验证问题的最大数目。 如果存在其他问题，则这些问题将不包括在该电子邮件中，但可以在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中进行查看。|  
|**默认电子邮件格式**|**EmailFormat**|所有电子邮件通知的格式。 默认值为 **HTML** 或 **1**。 数据库设置 **2** 表示 **“文本”**。<br /><br /> 注意：可以在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中为单个用户替代此设置，只需在用户的“常规”  选项卡上更改和保存“电子邮件格式”  即可。|  
|**用于电子邮件地址的正则表达式**|**EmailRegExPattern**|在 " [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **用户和组权限** " 功能区域中，用于验证在用户的 " **常规** " 选项卡上输入的电子邮件地址的正则表达式。有关正则表达式的详细信息，请参阅 MSDN library 中的 [正则表达式语言元素](/dotnet/standard/base-types/regular-expression-language-quick-reference) 。|  
|**数据库邮件帐户**|**EmailProfilePrincipalAccount**|显示发送电子邮件通知时要使用的数据库邮件帐户。 默认配置文件为 **mds_email_user**。|  
|**数据库邮件配置文件**|**DatabaseMailProfile**|发送电子邮件通知时要使用的数据库邮件配置文件。 默认值为空。|  
||**ValidationIssueHTML**|业务规则验证失败时电子邮件用户收到的 HTML 格式的文本。|  
||**ValidationIssueText**|业务规则验证失败时电子邮件用户收到的纯文本格式的文本。|  
||**VersionStatusChangeText**|版本的状态更改时电子邮件用户收到的纯文本格式的文本。 只有对整个模型拥有 **“更新”** 权限的用户才会收到此电子邮件。|  
||**VersionStatusChangeHTML**|版本的状态更改时电子邮件用户收到的 HTML 格式的文本。 只有对整个模型拥有 **“更新”** 权限的用户才会收到此电子邮件。|  
  
 有关详细信息，请参阅[通知 (Master Data Services)](../master-data-services/notifications-master-data-services.md)。  
  
##  <a name="security-settings"></a><a name="Security"></a> 安全设置  
  
|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
||**SecurityMemberProcessInterval**|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **“用户和组权限”** 功能区域，应用 **“层次结构成员”** 选项卡上设置的用户和组权限的频率（以秒为单位）。 默认值为 **3600** 秒 (60 分钟) 。|  

##  <a name="performance-settings"></a><a name="Performance"></a> 性能设置  

|配置管理器设置|系统设置|描述|  
|-----------------------------------|--------------------|-----------------|  
|**启用性能改进设置**|**PerformanceImprovementEnable**|我们默认启用此设置（设置为 1），使加载权限相关页面具有良好的性能。 但在此情况下，创建/修改实体、属性、用户或组性能不佳。 为避免这种情况，可以禁用此设置（设置为 0）。 随后更改此设置。 必须运行命令“EXEC [mdm].[udpPerformanceToggleSwitch];”，以确保视图和数据正确无误。|  
  
 有关详细信息，请参阅[立即应用成员权限 (Master Data Services)](../master-data-services/immediately-apply-member-permissions-master-data-services.md)。  
  
##  <a name="not-used"></a><a name="NotUsed"></a> 未使用  
 不使用系统设置表中的以下设置。  
  
-   **SecurityMode**  
  
-   **MDSHubName**  
  
-   **ApplicationLogging**  
  
-   **ReportServer**  
  
-   **ReportDirectory**  
  
-   **BusinessRuleEngineIterationLimit**  
  
-   **BusinessRuleExtensibility**  
  
-   **AttributeExplorerMarkAllActionMemberCount**  
  
## <a name="see-also"></a>另请参阅  
 [数据库对象安全性 &#40;Master Data Services&#41;](../master-data-services/database-object-security-master-data-services.md)  
  
