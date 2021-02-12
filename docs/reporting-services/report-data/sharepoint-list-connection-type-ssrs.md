---
title: SharePoint 列表连接类型 | Microsoft Docs
description: 利用本文中有关 SharePoint 列表连接类型的信息了解如何生成数据源。
ms.date: 05/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 2c4adf2f-e9c4-4fae-bd3c-97fe64436caf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7bb6380e168c1e4bddefab539354d80bf14805c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033457"
---
# <a name="sharepoint-list-connection-type-ssrs"></a>SharePoint 列表连接类型 (SSRS)

[!INCLUDE[ssrs-appliesto-sql2016-preview](../../includes/ssrs-appliesto-sql2016-preview.md)] [!INCLUDE [ssrs-appliesto-sharepoint-2013-2016](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

若要在报表中包含来自 Microsoft SharePoint 列表的数据，您必须添加或创建一个基于 Microsoft SharePoint 列表类型的报表数据源的数据集。 此内置数据源类型是基于 Microsoft SQL Server Reporting Services SharePoint 列表数据扩展插件。 使用此数据源类型连接到 SharePoint 2013 及更高版本，并从中检索列表数据。

使用本主题中的信息来生成一个数据源。 有关分步说明，请参阅 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)。  

##  <a name="connection-string"></a><a name="Connection"></a> 连接字符串  
 SharePoint 列表的连接字符串是指向 SharePoint 站点或子站点（例如， `https://MySharePointWeb/MySharePointSite` 或 `https://MySharePointWeb/MySharePointSite/Subsite`）的 URL。  
  
 查询设计器会自动显示您拥有足够访问权限的 SharePoint 列表。  
  
 有关更多连接字符串的示例，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)。  
  
##  <a name="credentials"></a><a name="Credentials"></a> 凭据  
 执行以下操作时需要提供凭据：运行查询、本地预览报表以及从报表服务器预览报表。 报表发布后，您可能需要更改数据源的凭据，以使报表在报表服务器上运行时，用于检索数据的权限有效。 可与此数据扩展插件一起使用的凭据类型取决于用作数据源的 SharePoint 列表的 SharePoint 技术配置。  
  
 下表概括了在连接到本地场 SharePoint 列表和远程 SharePoint 列表时，SharePoint 列表扩展的凭据检索行为。  
  
 **表 1** 适用于部署到旧的 Windows SharePoint 站点的报表。 旧的 Windows 站点仅支持 Kerberos、NTLM 和基于窗体的身份验证 (FBA)。 **表 2** 适用于部署到基于声明的 SharePoint 站点的报表。  
  
 **表 1**  
  
|检索类型|受支持的凭据|经典模式 Windows 身份验证|*声明身份验证|  
|-|---------------------------|-----------------------------------------|-----------------------------|  
|本地场 SharePoint 列表|Windows 身份验证（集成）或 SharePoint 用户标记|是|是|  
||存储、提示、无（带有 Windows 凭据）<br /><br /> 不支持带有非 Windows 凭据的存储和提示凭据。|是|否|  
|远程 SharePoint 列表|Windows 身份验证（集成）或 SharePoint 用户标记|是|否<br /><br /> 远程 SharePoint 列表不支持基于窗体的身份验证和声明身份验证。|  
||存储、提示、无（带有 Windows 凭据）<br /><br /> 不支持带有非 Windows 凭据的存储和提示凭据。|是|否<br /><br /> 远程 SharePoint 列表不支持基于窗体的身份验证和声明身份验证。|  
  
 *Windows 身份验证、基于窗体的身份验证 (FBA)、安全应用程序标记语言 (SAML) 标记、其他标识提供程序或以上提到的多个身份验证提供程序的组合。  
  
 **表 2**  
  
|检索类型|受支持的凭据|经典模式 Windows 身份验证|*声明身份验证|  
|-|---------------------------|-----------------------------------------|-----------------------------|  
|本地场 SharePoint 列表|Windows 身份验证（集成）或 SharePoint 用户标记|是|是|  
||存储、提示、无（带有 Windows 凭据）<br /><br /> 不支持带有非 Windows 凭据的存储和提示凭据。|否|否|  
|远程 SharePoint 列表|Windows 身份验证（集成）或 SharePoint 用户标记|是|否<br /><br /> 远程 SharePoint 列表不支持基于窗体的身份验证和声明身份验证。|  
||存储、提示、无（带有 Windows 凭据）<br /><br /> 不支持带有非 Windows 凭据的存储和提示凭据。|否|否<br /><br /> 远程 SharePoint 列表不支持基于窗体的身份验证和声明身份验证。|  
  
 *Windows 身份验证、基于窗体的身份验证 (FBA)、安全应用程序标记语言 (SAML) 标记、其他标识提供程序或以上提到的多个身份验证提供程序的组合。  
  
 **Windows 身份验证**  
 对于配置为在“可信帐户”模式下与报表服务器一起使用的 SharePoint 技术，不支持此选项。 仅适用于 SQL Server 2012 Reporting Services 之前的版本。

 对于配置为在“Windows 集成”模式下与报表服务器一起使用的 SharePoint 技术，此选项既适用于当前 Windows 用户，也适用于当前 SharePoint 用户。
 
 对于配置为不使用报表服务器的 SharePoint 技术（本地模式），不支持此选项。 有关本地模式的详细信息，请参阅[报表查看器中的本地模式和连接模式报表对比（SharePoint 模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
 **不需要凭据（不使用凭据）：**  
 若要使用此选项，必须为报表服务器配置无人参与的执行帐户。 有关详细信息，请参阅[配置无人参与的执行帐户（报表服务器配置管理器）](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)。  
  
 有关 Microsoft BI 堆栈中声明身份验证支持的信息，请参阅 [在 Microsoft BI 堆栈中使用声明身份验证](https://social.technet.microsoft.com/wiki/contents/articles/15274.using-claims-authentication-across-the-microsoft-bi-stack.aspx)。  
  
 有关详细信息，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)、[指定报表数据源的凭据和连接信息](specify-credential-and-connection-information-for-report-data-sources.md)和 [Reporting Services 支持的数据源 (SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)。  
  
##  <a name="queries"></a><a name="Query"></a> 查询  
 若要设计一个查询，请基于数据源创建新数据集，然后打开关联的查询设计器。 有关详细信息，请参阅 [创建共享数据集或嵌入数据集（报表生成器和 SSRS）](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)。  
  
 SharePoint 列表图形查询设计器中显示四个窗格：  
  
 **SharePoint 列表**  ：显示该数据源在站点上的所有 SharePoint 列表的列表。 选择某一列表，然后选择要位于查询中的字段。 此窗格中字段的名称是 SharePoint 友好名称，也称为显示名称。 将鼠标指针悬停在某一项上可在工具提示中显示以下属性：  
  
-   **名称** ：字段的唯一名称。  
  
-   **标识符** ：字段的唯一标识符。  
  
-   **字段类型** ：字段的数据类型。  
  
-   **隐藏** ：字段是否显示在 SharePoint 列表视图中。  
  
 不支持从多个列表中选择字段。 可以为每个列表创建一个数据集并从每个数据集中选择字段。 如果这些列表具有通用字段，则可以使用绑定到一个数据集的 Tablix 数据区域中的 Lookup 函数从另一个未绑定到数据区域的数据集中检索值。 有关详细信息，请参阅 [Lookup 函数（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-lookup-function.md)。  
  
-   **所选字段**  ：显示所选的字段。 此窗格中的字段名称是 SharePoint 用户指定的友好名称。 在您关闭查询设计器后，将会在“报表数据”窗格的数据集字段集合中看到这些名称。 [“数据集属性”对话框 ->“字段”（报表生成器）](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)页介绍了唯一名称和友好名称的关系。  
  
-   **应用的筛选器**  ：在数据返回到报表中之前，限制从 SharePoint 列表返回的数据。 选择用于限制在列表中检索的数据的字段名称、运算符和值。 根据所选值的数据类型的不同，运算符也会有所不同。  
  
     您不能在图形查询设计器中更改排序顺序或指定组。 为了更改排序顺序或指定组，请对报表数据集设置排序表达式，并且对报表中数据区域上的表达式进行分组。 不支持查询参数。 若要筛选报表中的数据，请使用您创建的报表筛选器或报表参数。 有关详细信息，请参阅 [对数据进行筛选、分组和排序（报表生成器和 SSRS）](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md) 和 [报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)。  
  
-   **查询结果**  ：显示查询运行时返回的示例行。 如果 SharePoint 列表值在 SharePoint 站点上频繁更改，则您在查询结果窗格中看到的值可能不同于在报表中看到的值。  
  
-   **所选字段**  ：显示所选的字段。 此窗格中的字段名称是 SharePoint 用户指定的友好名称。 在您关闭查询设计器后，将会在“报表数据”窗格的数据集字段集合中看到这些名称。 [“数据集属性”对话框 ->“字段”（报表生成器）](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)页介绍了唯一名称和友好名称的关系。  
  
-   **应用的筛选器**  ：在数据返回到报表中之前，限制从 SharePoint 列表返回的数据。 选择用于限制在列表中检索的数据的字段名称、运算符和值。 根据所选值的数据类型的不同，运算符也会有所不同。  
  
     您不能在图形查询设计器中更改排序顺序或指定组。 为了更改排序顺序或指定组，请对报表数据集设置排序表达式，并且对报表中数据区域上的表达式进行分组。 不支持查询参数。 若要筛选报表中的数据，请使用您创建的报表筛选器或报表参数。 有关详细信息，请参阅 [对数据进行筛选、分组和排序（报表生成器和 SSRS）](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md) 和 [报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)。  
  
-   **查询结果**  ：显示查询运行时返回的示例行。 如果 SharePoint 列表值在 SharePoint 站点上频繁更改，则您在查询结果窗格中看到的值可能不同于在报表中看到的值。  
  
 有关详细信息，请参阅 [SharePoint 列表查询设计器（报表生成器）](../../reporting-services/report-data/sharepoint-list-query-designer-report-builder.md)。  
  
### <a name="query-text"></a>查询文本  
 若要查看图形查询设计器生成的查询，请切换到基于文本的查询设计器。 在此视图中，您可以看到由图形查询设计器创建的 XML。 该 XML 包括用于列表名称、字段集合和筛选器的元素。  
  
#### <a name="example-1-specified-fields-for-a-list"></a>示例 1。 列表的指定字段  
 下面的示例演示一个格式正确的 SharePoint 查询：  
  
```  
<RSSharePointList>  
<listName>MyList</listName>  
<viewFields>  
  <FieldRef Name="Field1"/>  
  <FieldRef Name="Field4"/>  
</viewFields>  
<Query>  
  <Where>  
    <And>  
      <Gt>  
        <FieldRef Name="Field1"/>  
        <Value Type="Integer">1</Value>  
      </Gt>  
      <IsNotNull>  
        <FieldRef Name="Field2"/>  
        <Value Type="string"/>  
      </IsNotNull>   
    </And>  
  </Where>  
</Query>  
</RSSharePointList>  
```  
  
 您可以编辑此查询的视图，只要它保持格式正确的 XML 文本。  
  
#### <a name="example-2-all-fields-for-a-list"></a>示例 2。 列表的所有字段  
 您还可以仅指定列表的名称，包括隐藏字段在内的所有字段都将返回。 下面的示例从名为 Tasks 的列表中检索所有字段：  
  
```  
<RSSharePointList>  
<listName>Tasks</listName>  
</RSSharePointList>  
```  
  
 在查询结果中返回 Tasks 列表的所有字段。  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameters  
 此数据扩展插件不支持参数。  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> 操作指南主题  
 本节包含使用数据连接、数据源和数据集的分步说明。  
  
 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [创建共享数据集或嵌入数据集（报表生成器和 SSRS）](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [向数据集添加筛选器（报表生成器和 SSRS）](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
##  <a name="related-sections"></a><a name="Related"></a> 相关章节  
 文档中的这些章节提供有关报表数据的深入概念性信息，以及有关如何定义、自定义和使用与数据相关的报表部件的步骤信息。  
  
 [报表数据集 (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md)  
 提供访问报表数据的概述。  
  
 [创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 提供有关数据连接和数据源的信息。  
  
 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 提供有关嵌入数据集和共享数据集的信息。  
  
 [数据集字段集合（报表生成器和 SSRS）](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 提供有关查询生成的数据集字段集合的信息。  
  
 [Reporting Services 支持的数据源 &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)。  
 提供有关每个数据扩展插件的平台和版本支持的详细信息。  

## <a name="see-also"></a>另请参阅

[报表参数](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[对数据进行筛选、分组和排序](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
