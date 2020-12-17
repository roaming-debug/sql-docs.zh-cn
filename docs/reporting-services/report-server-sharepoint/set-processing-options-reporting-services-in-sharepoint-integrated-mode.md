---
title: 设置处理选项（SharePoint 集成模式下的 Reporting Services）| Microsoft Docs
description: 在 SharePoint 集成模式下的 SQL Server Reporting Services 中，指定进行数据处理的时间、超时值和其他选项。
ms.date: 10/05/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 2f06c967c894092d8d89224948ed1772c9e70ed5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461468"
---
# <a name="set-processing-options-reporting-services-in-sharepoint-integrated-mode"></a>设置处理选项（SharePoint 集成模式下的 Reporting Services）

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  可以对 Reporting Services 报表设置处理选项，以决定何时进行数据处理。 还可以为报表处理设置超时值和用于决定是否为当前报表启用报表历史记录的选项。  
  
-   您可以按报表快照形式运行报表，以防止报表在任意时间运行（例如，在执行计划备份期间）。 报表快照通常按计划创建并在随后进行刷新，因此您可以精确地设定进行报表和数据处理的时间。 如果报表所基于的查询需要很长的运行时间，或查询使用的数据来自您需要在特定时间无人访问的数据源，则应以快照形式运行报表。  
  
-   报表服务器可以缓存已处理报表的副本，并在用户打开此报表时返回该副本。 缓存是一种性能增强技术。 在报表很大或需要频繁访问的情况下，缓存报表可以缩短检索该报表所需的时间。  
  
-   报表历史记录是以前运行的报表副本的集合。 您可以使用报表历史记录来维护一段时间以来的报表记录。 报表历史记录不适用于包含机密数据或个人数据的报表。 因此，报表历史记录只能包含使用单组特定凭据查询数据源的报表。此类凭据可以是已存储的凭据，也可以是在无人参与的情况下执行报表所用的凭据，并且对运行报表的所有用户都可用。  

    Reporting Services 与 SharePoint 的集成使用 SharePoint 的签出和签入内容管理功能将更新保存到 Reporting Services 内容类型。 这包括创建报表快照。 因此，如果您在文档库上启用了版本控制，则当创建新的报表历史记录快照时，您将看到更新后的报表版本。 这是更新快照的副作用。 当更新快照时，它导致报表的 LastExecution 属性发生变化，而这会导致报表的版本发生变化。  

-   可以通过指定超时值来限制使用系统资源的方式。  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

## <a name="set-data-refresh-options"></a>设置数据刷新选项
  
1.  指向库中的报表。  
  
2.  单击向下箭头，然后选择 **“管理处理选项”** 。  
  
3.  在 **“数据刷新选项”** 中，单击 **“使用快照数据”** 。 如果看到“此报表无法通过快照运行，因为一个或多个数据源凭据未存储”这一信息，则表明报表未配置为以无人参与的方式运行，在设置此选项之前，必须将数据源修改为使用已存储凭据。  
  
4.  在 **“数据快照选项”** 中，选择 **“计划数据处理”** 。  
  
5.  如果有要使用的现有共享计划，请选择 **“根据共享计划”** ，否则请单击 **“根据自定义计划”**，然后单击 **“配置”**。  
  
6.  选择频率、计划以及开始和结束日期，然后单击 **“确定”** 。  
  
7.  如果想要立刻创建用于报表的快照数据而不等待预定的数据处理开始执行，则应在 **“数据快照选项”** 中选择 **“保存此页时创建或更新快照”** 。  
  
## <a name="set-report-caching-options"></a>设置报表缓存选项
  
1.  指向库中的报表。  
  
2.  单击向下箭头，然后选择 **“管理处理选项”** 。  
  
3.  在 **“数据刷新选项”** 中，单击 **“使用缓存数据”** 。 如果看到“此报表无法缓存，因为未存储一个或多个数据源凭据”这一信息，则表明报表未配置为以无人参与的方式运行，在设置此选项之前，必须将数据源修改为使用已存储凭据。  
  
4.  在 **“缓存选项”** 中，指定缓存的过期方式：  
  
    -   输入缓存将在其后过期的分钟数。  
  
    -   使用共享计划，在计划中指定的时间清除缓存。  
  
    -   创建自定义计划，在指定的时间清除缓存。  
  
## <a name="set-processing-time-out-values"></a>设置处理超时值
  
1.  指向库中的报表。  
  
2.  单击向下箭头，然后选择 **“管理处理选项”** 。  
  
3.  如果要使用在报表服务器级指定的值，请在“处理超时”中选择“使用站点默认设置” 。 如果要用无超时或不同的超时值来替代该值，请选择“不对报表处理时间设置超时”或“限制报表处理时间(秒)”。  
  
## <a name="set-report-history-options-and-limits"></a>设置报表历史记录选项和限值
  
1.  指向库中的报表。  
  
2.  单击向下箭头，然后选择 **“管理处理选项”** 。  
  
3.  在 **“历史记录快照选项”** 中，指定数据处理的方式和时间，以及对数据处理进行保存的方式和时间。  
  
4.  在 **“历史记录快照限制”** 中，如果要使用在报表服务器级指定的值，请选择 **“使用站点默认设置”** 。 否则，请选择 **“不限制快照数”** 或 **“将快照数限制在”** 以指定自定义值。  
  
## <a name="set-database-timeout"></a>设置数据库超时
  
*  使用 Windows PowerShell 设置 SharePoint 报表服务器的数据库超时。 有关详细信息，请参阅[用于 Reporting Services SharePoint 模式的 PowerShell cmdlet](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md) 中的“获取并设置报表服务应用程序数据库的属性”一节。  
  
## <a name="next-steps"></a>后续步骤

 [设置报表处理属性](../../reporting-services/report-server/set-report-processing-properties.md)   
 [缓存报表](../../reporting-services/report-server/caching-reports-ssrs.md)   
 [为报表和共享数据集处理设置超时值](../../reporting-services/report-server/setting-time-out-values-for-report-and-shared-dataset-processing-ssrs.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
