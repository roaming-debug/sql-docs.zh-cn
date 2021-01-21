---
title: XML 连接类型 | Microsoft Docs
description: 了解用于连接到 XML 文档、Web 服务或查询中嵌入的 XML 并从中检索数据的 XML 连接类型。
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 5b55fff2-1b15-4156-83ef-15ad9cf9f509
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b6ca1df39a5aeed8b57de5c6eff7ab66e725062d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596530"
---
# <a name="xml-connection-type-ssrs"></a>XML 连接类型 (SSRS)
  若要在报表中包含来自 XML 数据源的数据，则必须拥有一个基于 XML 类型的报表数据源的数据集。 此内置数据源类型基于 XML 数据扩展插件。 使用此数据源类型可连接到 XML 文档、Web 服务、或查询中嵌入的 XML 并从中检索数据。  
  
 此数据扩展插件支持参数和与连接字符串分开管理的凭据。  
  
 使用本主题中的信息来生成一个数据源。 有关分步说明，请参阅 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)。  
  
##  <a name="connection-string"></a><a name="Connection"></a> 连接字符串  
 连接字符串必须为指向 Web 服务、基于 Web 的应用程序或可通过 HTTP 使用的 XML 文档的 URL。 XML 文档必须具有 XML 扩展名。 还可以对数据集查询中嵌入的 XML 数据使用空连接字符串。  
  
 下面的示例对 Web 服务器和 XML 文档的连接字符串语法分别进行了说明。 不支持 `file://` 协议。  
  
|XML 文档类型|连接字符串示例|  
|-----------------------|-------------------------------|  
|Web 服务|`https://adventure-works.com/results.aspx`|  
|XML 文档|`https://localhost/XML/Customers.xml`|  
|嵌入的 XML 文档|*Empty*|  
  
 有关更多连接字符串的示例，请参阅 [报表生成器中的数据连接、数据源和连接字符串](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)。  
  
##  <a name="credentials"></a><a name="Credentials"></a> 凭据  
 执行以下操作时需要提供凭据：运行查询、本地预览报表以及从报表服务器预览报表。  
  
 报表发布后，您可能需要更改数据源的凭据，以使报表在报表服务器上运行时，用于检索数据的权限有效。  
  
 在报表创作客户端，可以使用下列选项指定凭据：  
  
-   当前 Windows 用户（也称为集成安全性）。  
  
-   不需要提供任何凭据。 如果选择不使用任何凭据，则将使用匿名访问。 请确保您已为报表服务器定义了无人参与的执行帐户以连接到外部数据源。 XML 数据处理扩展插件不会将凭据传递到目标 URL 或 Web 服务；只有在定义了无人参与的执行帐户之后，连接才会成功。 有关详细信息，请参阅[配置无人参与的执行帐户（报表服务器配置管理器）](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)。  
  
 不支持存储的凭据和提示的凭据。 注意，如果禁用 Windows 集成安全性，则无法使用它来检索数据。 如果指定了存储凭据或提示凭据，则会在执行时发生错误。  
  
 有关详细信息，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) 或[指定报表数据源的凭据和连接信息](specify-credential-and-connection-information-for-report-data-sources.md)。  
  
##  <a name="queries"></a><a name="Query"></a> 查询  
 查询指定了要为报表数据集检索哪些数据。 查询的结果集中的列填充数据集的字段集合。 报表仅处理查询检索的第一个结果集。  
  
 必须使用基于文本的查询设计器创建查询。 查询必须返回 XML 数据。  
  
 有关基于文本的查询设计器的详细信息，请参阅[基于文本的查询设计器用户界面（报表生成器）](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md)。  
  
 下面显示当数据源为 XML 类型时数据集查询的可能值。  
  
-   *Empty*  
  
     使用空查询创建默认结果集。 默认查询是通过读取数据源并遍历到 XML 节点层次结构的第一个叶集合来创建的。 结果集包括具有文本值的所有节点以及沿该路径的所有节点属性。 结果集中的列将映射到数据集的字段。  
  
-   *元素路径*  
  
     指定在从数据源中检索 XML 数据时要使用的节点序列。  
  
-   *XML 查询元素*  
  
     具有以下可选元素的 XML 查询规范：  
  
    -   **XML 数据源为 Web 服务**  
  
         必需的 XML 元素：  
  
         `<Method Namespace=` *"namespace"*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *soap action* `</SoapAction>`  
  
         可选的 XML 元素：  
  
         `<ElementPath>`  *element path*  `</ElementPath>`  
  
         `<Method Namespace=` *"namespace"*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *soap action* `</SoapAction>`  
  
    -   **XML 数据源为 XML 文档**  
  
         必需的 XML 元素：无  
  
         可选的 XML 元素：  
  
         `<ElementPath>`  *element path*  `</ElementPath>`  
  
    -   **XML 数据源为嵌入式 XML 文档**  
  
         必需的 XML 元素：  
  
         `<XmlData>` 内部 XML `</XmlData>`  
  
         可选的 XML 元素：  
  
         `<ElementPath>`  *element path*  `</ElementPath>`  
  
         `-- or --`  
  
         `<ElementPath IgnoreNamespaces="true">`  *element path*  `</ElementPath>`  
  
 有关查询语法的详细信息，请参阅[用于 XML 报表数据的 XML 查询语法 (SSRS)](../../reporting-services/report-data/xml-query-syntax-for-xml-report-data-ssrs.md)。  
  
 有关示例，请参阅 [Reporting Services：使用 XML 和 Web 服务数据源](/previous-versions/sql/sql-server-2005/administrator/aa964129(v=sql.90))。  
  
### <a name="requirements-for-retrieving-xml-web-service-data"></a>检索 XML Web 服务数据的要求  
 XML 数据处理扩展插件不能检测架构。 因此，您必须通过某种方式来发现哪些 SOAP 方法将检索所需数据。 您还必须了解 Web 服务用于其数据的寻址方案或命名空间。  
  
 对于 Web 服务，可以提供用于指定调用方法或 SOAP 操作的 \<**Query**> 元素。 如果 XML 数据源具有可产生要用于报表的数据的层次结构，则可以将查询留空并使用默认查询。 查询运行时检索的 XML 元素节点值和属性将映射到在报表中使用的数据集字段。  
  
### <a name="requirements-for-retrieving-xml-document-data"></a>检索 XML 文档数据的要求  
 使用 http 协议时，服务器必须返回 XML 数据，或者 XML 数据必须嵌入 XML **Query** 元素中。 如果您使用 http 协议直接引用 XML 文档，则文档的扩展名必须为 .xml。  
  
 您必须了解如何创建 XML 查询来检索所需的所有数据。 如果不指定元素路径，则默认的 XML 文档分析行为是选择 XML 文档中指向叶节点集合的第一个可用路径。 如果 XML 文档包括指向其他同级叶节点集合的其他路径，则除非在查询中指定一个路径，否则将忽略这些节点。  
  
 可以使用与 XQuery 类似的 XML 语法提供元素路径。  
  
 有关详细信息，请参阅[用于 XML 报表数据的元素路径语法 &#40;SSRS&#41;](../../reporting-services/report-data/element-path-syntax-for-xml-report-data-ssrs.md)。  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameters  
 系统不会对查询进行分析以标识参数。  
  
 若要添加参数，必须通过“ **数据集属性** ”对话框中的 [“参数”](/previous-versions/sql/) 页手动创建参数。  
  
##  <a name="remarks"></a><a name="Remarks"></a> 注释  
 XML 数据扩展插件支持基于 XML 数据（表格格式且不分层）生成报表。 有关详细信息，请参阅[从外部数据源中添加数据 (SSRS)](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md)。  
  
 不提供从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库检索 XML 文档的内置支持。  
  
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
 [报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [对数据进行筛选、分组和排序（报表生成器和 SSRS）](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
