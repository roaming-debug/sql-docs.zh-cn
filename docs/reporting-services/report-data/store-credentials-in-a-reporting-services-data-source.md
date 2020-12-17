---
title: 在 Reporting Services 数据源中存储凭据 | Microsoft Docs
description: 了解如何为本机模式下和 SharePoint 模式下的报表服务器配置存储凭据。
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 05/24/2018
ms.openlocfilehash: 18d9a5ad8c4df17525bad04f0b056e7f9c4f05ae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478848"
---
# <a name="store-credentials-in-a-reporting-services-data-source"></a>在 Reporting Services 数据源中存储凭据

::: moniker range="=sql-server-2016"

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

::: moniker-end

::: moniker range=">=sql-server-2017"

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]

::: moniker-end

可以配置存储的凭据， [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器可使用这些凭据来访问报表的外部数据。 如果报表在无人参与的状态下运行，则使用存储凭据，例如将报表作为电子邮件发布的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅。 计划或触发报表处理时，报表服务器将检索和使用这些凭据。 本主题向你说明了为本机模式下和 SharePoint 模式下的报表服务器配置存储凭据的过程。  
  
##  <a name="security-policy-requirements-for-stored-credentials"></a><a name="bkmk_top"></a> 存储凭据的安全策略要求  
 ![as_powerpivot_refresh_sss_set_key](/analysis-services/analysis-services/power-pivot-sharepoint/media/as-powerpivot-refresh-sss-set-key.gif "as_powerpivot_refresh_sss_set_key") 要求为报表服务器上的以下其中一项安全策略配置用于存储凭据的帐户。 建议使用环境要求的最低级别权限来选择策略。  
  
1.  **允许在本地登录**。 有关详细信息，请参阅 [允许在本地登录](https://technet.microsoft.com/library/cc756809\(v=WS.10\).aspx)。  
  
2.  **作为批处理作业登录**。 有关详细信息，请参阅 [作为批处理作业登录](https://technet.microsoft.com/library/cc755659\(v=ws.10\).aspx)。  
  
3.  有关策略的一般信息，请参阅 [在组策略对象上编辑安全设置](https://technet.microsoft.com/library/cc736516\(v=ws.10\).aspx)。  
  
##  <a name="configure-stored-credentials-for-a-report-specific-data-source-native-mode"></a><a name="bkmk_stored_credentials_data_source_native"></a> 为特定于报表的数据源配置存储的凭据（本机模式）  
  
1.  在 Web 门户中，浏览到包含该报表的文件夹。 单击报表磁贴右上角的省略号 (...)。  
  
2.  单击“管理”  ，然后单击“数据源” 。  
  
3.  选择 **“自定义数据源”** 。  
  
4.  在“数据源类型”  列表中，选择处理数据源的数据所用的数据处理扩展。  
  
5.  对于 **“连接字符串”** ，指定报表服务器用于连接数据源的连接字符串。 下面的示例说明了用于连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的连接字符串：  
  
    ```  
    data source=<servername>;initial catalog=AdventureWorks2012  
    ```  
  
6.  对于“连接方法” ，选择“安全存储在报表服务器中的凭据” 。  
  
7.  键入用户名和密码。  
  
    -   如果帐户是 Windows 域用户帐户，请按照以下格式指定该帐户：\<domain>\\<account\>，然后选择“在与数据源建立连接时用作 Windows 凭据”。  
  
    -   如果用户名和密码是数据库凭据，请不要选择 **“在与数据源建立连接时用作 Windows 凭据”** 。 如果数据库服务器支持模拟或委托，则可以选择 **“与数据源建立连接之后模拟经过身份验证的用户”**。  
  
8.  单击“应用”。  
  
     ![用于“返回页首”链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [适用于存储凭据的安全策略要求](#bkmk_top)  
  
##  <a name="configure-stored-credentials-for-a-report-specific-data-source-sharepoint-mode"></a><a name="bkmk_stored_credentials_data_source_sharepoint"></a> 为特定于报表的数据源配置存储的凭据（SharePoint 模式）  
  
1.  浏览到包含该报表的文档库，然后单击打开菜单 ![ssrs 项的文档库上下文菜单](../../reporting-services/report-data/media/ssrs-sharepoint-item-context-menu.png "ssrs 项的文档库上下文菜单")。  
  
2.  单击第二个打开菜单 ![ssrs 项的文档库上下文菜单](../../reporting-services/report-data/media/ssrs-sharepoint-item-context-menu.png "ssrs 项的文档库上下文菜单")，然后单击“管理数据源”。  
  
3.  单击想要使用存储的凭据进行配置的“自定义”  数据源的名称。  
  
4.  在“数据源类型”  列表中，选择处理数据源的数据所用的数据处理扩展。  
  
5.  对于 **“连接字符串”** ，指定报表服务器用于连接数据源的连接字符串。 下面的示例说明了用于连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的连接字符串：  
  
    ```  
    data source=<servername>;initial catalog=AdventureWorks2012  
    ```  
  
6.  对于“凭据” ，选择“存储的凭据” 。  
  
7.  键入“用户名”  和“密码” 。  
  
    -   如果帐户是 Windows 域用户帐户，请按照以下格式指定该帐户：\<domain>\\<account\>，然后选择“在与数据源建立连接时用作 Windows 凭据”。  
  
    -   如果用户名和密码是数据库凭据，请勿选择“用作 Windows 凭据” 。 如果数据库服务器支持模拟或委托，则可选择 **“设置此帐户的执行上下文”** 。  
  
8.  单击“确定”。  
  
     ![用于“返回页首”链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [适用于存储凭据的安全策略要求](#bkmk_top)  
  
##  <a name="configure-stored-credentials-for-a-shared-data-source-native-mode"></a><a name="bkmk_stored_credentials_shared_data_source_native"></a> 为共享数据源配置存储凭据（本机模式）  
  
1.  在 Web 门户中，浏览到共享数据源项。 
  
2.  单击报表磁贴右上角的省略号 (...) >“管理”。 
  
3.  在“类型”列表中，指定用于处理来自数据源的数据的数据处理扩展插件。  
  
4.  对于 **“连接字符串”** ，指定报表服务器用于连接数据源的连接字符串。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议您不要在连接字符串中指定凭据。  
  
     下面的示例演示用于连接到本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的连接字符串：  
  
    ```  
    data source=<localservername>; initial catalog=AdventureWorks2012  
    ```  
  
5.  键入用户名和密码。  
  
    -   如果帐户是 Windows 域用户帐户，请按照以下格式指定该帐户：\<domain>\\<account\>，然后选择“在与数据源建立连接时用作 Windows 凭据”。  
  
    -   如果用户名和密码是数据库凭据，请不要选择 **“在与数据源建立连接时用作 Windows 凭据”** 。 如果数据库服务器支持模拟或委托，则可以选择 **“与数据源建立连接之后模拟经过身份验证的用户”**。  
  
6.  单击“应用”。  
  
     ![用于“返回页首”链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [适用于存储凭据的安全策略要求](#bkmk_top)  
  
##  <a name="configure-stored-credentials-for-a-shared-data-source-sharepoint-mode"></a><a name="bkmk_stored_credentials_shared_data_source_sharepoint"></a> 为共享数据源配置存储凭据（SharePoint 模式）  
  
1.  在文档库中，浏览到共享数据源项。![共享数据源图标](../../reporting-services/report-data/media/hlp-16datasource.png "共享数据源图标")  
  
2.  单击上下文菜单 ![ssrs 项的文档库上下文菜单](../../reporting-services/report-data/media/ssrs-sharepoint-item-context-menu.png "ssrs 项的文档库上下文菜单")，然后单击第二个上下文菜单 ![ssrs 项的文档库上下文菜单](../../reporting-services/report-data/media/ssrs-sharepoint-item-context-menu.png "ssrs 项的文档库上下文菜单")。  
  
3.  单击“编辑数据源定义” 。  
  
4.  在“数据源类型”  列表中，指定处理数据源的数据所用的数据处理扩展。  
  
5.  对于 **“连接字符串”** ，指定报表服务器用于连接数据源的连接字符串。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议您不要在连接字符串中指定凭据。  
  
     下面的示例演示用于连接到本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的连接字符串：  
  
    ```  
    data source=<localservername>; initial catalog=AdventureWorks2012  
    ```  
  
6.  键入用户名和密码。  
  
    -   如果帐户是 Windows 域用户帐户，请按照以下格式指定该帐户：\<domain>\\<account\>，然后选择“用作 Windows 凭据”。  
  
    -   如果用户名和密码是数据库凭据，请勿选择“用作 Windows 凭据” 。 如果数据库服务器支持模拟或委托，则可选择 **Set Execution context to this account**。  
  
7.  单击“确定” 。  
  
     ![用于“返回页首”链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [适用于存储凭据的安全策略要求](#bkmk_top)  
  
## <a name="see-also"></a>另请参阅  
 [为报表数据源指定凭据和连接信息](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
