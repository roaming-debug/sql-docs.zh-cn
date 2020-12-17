---
title: 报表参数（报表生成器和报表设计器）| Microsoft Docs
description: 本主题介绍 Reporting Services 报表参数的常规用法、可设置的属性等内容。
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.custom: seodec18
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.date: 12/06/2018
ms.openlocfilehash: abbda73ac3aeda94ee8752dbbe638d93bc3d5573
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425108"
---
# <a name="report-parameters-report-builder-and-report-designer"></a>报表参数（报表生成器和报表设计器）

::: moniker range="=sql-server-2016"

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

::: moniker-end

::: moniker range=">=sql-server-2017"

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]

::: moniker-end

本主题介绍 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表参数的常规用法、可设置的属性等内容。 通过报表参数，您可以控制报表数据、将相关报表连接在一起以及更改报表显示。 可在 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 和报表设计器中所创建的分页报表中使用报表参数，也可在 [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)] 中所创建的移动报表中使用。 阅读有关 [报表参数概念](../../reporting-services/report-design/report-parameters-concepts-report-builder-and-ssrs.md)的详细信息。  

若要尝试自己向报表添加参数，请参阅 [教程：向报表添加参数（报表生成器）](../../reporting-services/tutorial-add-a-parameter-to-your-report-report-builder.md)中所创建的移动报表中使用。  

## <a name="common-uses-for-parameters"></a><a name="bkmk_Common_Uses_for_Parameters"></a> 参数的常规用法

 下面是一些最常用的使用参数的方法。  
  
**控制分页和移动报表数据**
  
- 通过编写包含变量的数据集查询，在数据源筛选分页报表数据。  
  
- 筛选共享数据集的数据。 当你向分页报表添加共享数据集时，无法更改查询。 在报表中，您可以添加一个数据集筛选器，在其中包含对您创建的报表参数的引用。  
  
- 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 移动报表中筛选共享数据集的数据。 有关详细信息，请参阅 [Create mobile reports with SQL Server Mobile Report Publisher](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) 。  
  
- 允许用户指定值以对分页报表中的数据进行自定义。 例如，为销售数据的开始日期和结束日期提供两个参数。  
  
**连接相关报表**
  
- 使用参数可将主报表关联到钻取报表、子报表和链接报表。 设计一组报表时，您可以将各个报表设计为回答某些特定的问题。 每个报表都可以为相关信息提供不同的视图或不同的详细程度。 若要提供一组相关报表，请为目标报表上的相关数据创建参数。  
  
    有关详细信息，请参阅[钻取报表（报表生成器和 SSRS）](../../reporting-services/report-design/drillthrough-reports-report-builder-and-ssrs.md)、[子报表（报表生成器和 SSRS）](../../reporting-services/report-design/subreports-report-builder-and-ssrs.md)和[创建链接报表](../../reporting-services/reports/create-a-linked-report.md)。  

- 为多个用户自定义参数集。 基于报表服务器上的销售报表创建两个链接报表。 一个链接报表使用销售人员的预定义参数值，而第二个链接报表使用销售经理的预定义参数值。 两个报表使用相同的报表定义。  
  
**更改报表显示**
  
- 通过 URL 请求向报表服务器发送命令，以自定义报表的呈现。 有关详细信息，请参阅 [URL 访问 (SSRS)](../../reporting-services/url-access-ssrs.md) 和[在 URL 内传递报表参数](../../reporting-services/pass-a-report-parameter-within-a-url.md)。  
  
- 允许用户指定值以帮助自定义报表的外观。 例如，提供一个布尔参数以指示是否展开或折叠表中所有的嵌套行组。  
  
- 通过在表达式中包含参数来使用户能自定义报表数据和外观。  
  
    有关详细信息，请参阅[集合引用（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-parameters-collection-references-report-builder.md)。  
  
## <a name="viewing-a-report-with-parameters"></a><a name="UserInterface"></a> 使用参数查看报表

查看具有参数的报表时，报表查看器工具栏将显示每个参数，以便你可以通过交互方式指定值。 下图显示了具有参数 @ReportMonth、@ReportYear、@EmployeeID、@ShowAll、@ExpandTableRows、@CategoryQuota 和 @SalesDate 的报表的参数区域。  

![查看包含参数的报表](../../reporting-services/report-design/media/ssrb-rptparamviewrpt.png "查看包含参数的报表")  
  
1. **“参数”窗格** 报表查看器工具栏显示各个参数的提示和默认值。 可以在参数窗格中自定义参数布局。 有关详细信息，请参阅 [自定义报表中的参数窗格（报表生成器）](../../reporting-services/report-design/customize-the-parameters-pane-in-a-report-report-builder.md)中所创建的移动报表中使用。  
  
2. **SalesDate 参数\@** 参数 @SalesDate 的数据类型为 DateTime  。 文本框旁边会显示“选择日期”提示。 若要修改日期，请在文本框中键入新日期或使用日历控件。  
  
3. **ShowAll 参数\@** 参数 @ShowAll 的数据类型为 Boolean  。 使用单选按钮指定 **True** 或 **False**。  
  
4. **“显示或隐藏参数区域”控点** 在报表查看器工具栏上，单击此箭头可显示或隐藏参数窗格。  
  
5. **CategoryQuota 参数\@** 参数 @CategoryQuota 的数据类型为 Float，因此它采用数值  。  @CategoryQuota 设置为允许多个值。  
  
6. **查看报表** 输入参数值之后，单击“查看报表”  可运行报表。 如果所有参数都具有默认值，则报表会在第一次查看时自动运行。  
  
## <a name="creating-parameters"></a><a name="bkmk_Create_Parameters"></a> 创建参数

可以通过几种不同方式创建报表参数。
  
> [!NOTE]
>  并非所有的数据源都支持参数。
  
**具有参数的数据集查询或存储过程**
  
 添加一个包含变量的数据集查询或包含输入参数的数据集存储过程。 为每个变量或输入参数创建一个数据集参数，并为每个数据集参数创建一个报表参数。  
  
 ![报表生成器参数数据集属性](../../reporting-services/report-design/media/ssrb-paramdatasetprops.png "报表生成器参数数据集属性")  
  
 来自报表生成器的此图像显示：  
  
1.  “报表数据”窗格中的报表参数。  
  
2.  具有参数的数据集。  
  
3.  “参数”窗格。  
  
4.  在“数据集属性”对话框中列出的参数。  
  
 可嵌入或共享数据集。 在向报表中添加共享数据集时，不能在报表中覆盖标记为内部参数的数据集参数。 可以覆盖未被标记为内部参数的数据集参数。  
  
 有关详细信息，请参阅本主题中的 [数据集查询](#bkmk_Dataset_Parameters) 。  
  
**手动创建参数**
  
从“报表数据”窗格中手动创建参数。 您可以配置报表参数，以便用户可以通过交互方式输入值以帮助自定义报表的内容或外观。 也可以对报表参数进行配置，以便用户无法更改预配置值。  
  
> [!NOTE]  
>  因为在服务器上对参数实行单独管理，所以重新发布带有新的参数设置的主报表不会覆盖报表上的现有参数设置。  
  
 **具有参数的报表部件**
  
 添加一个报表部件，其中包含对参数的引用或对包含变量的共享数据集的引用。  
  
 报表部件存储于报表服务器上，并且可供他人用于其报表中。 不能通过报表服务器对参数的报表部件进行管理。 可以在报表部件库中搜索参数，并在添加参数后，在报表中配置这些参数。 有关详细信息，请参阅 [报表部件（报表生成器和 SSRS）](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)。  
  
> [!NOTE]  
>  对于具有相关数据集以及参数的数据区域，参数可以发布为单独的报表部件。 尽管参数作为报表部件列出，但您不能直接向报表添加报表部件参数。 而是应添加报表部件，此时，将从报表部件包含或引用的数据集查询中自动生成任何所需的报表参数。 有关报表部件的详细信息，请参阅[报表部件（报表生成器和 SSRS）](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)和[报表设计器中的报表部件 (SSRS)](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md)。  
  
### <a name="parameter-values"></a>参数值

 以下是用于在报表中选择参数值的选项。  
  
- 从下拉列表中选择一个单一参数值。  
  
- 从下拉列表中选择多个参数值。  
  
- 从下拉列表中为参数选择一个值，该值决定可在下拉列表中为其他参数选择的值。 这些是级联参数。 级联参数使你能够持续筛选数以千计的值，最终将值的数量限定在易于管理的范围内。  
  
     有关详细信息，请参阅 [向报表添加级联参数（报表生成器和 SSRS）](../../reporting-services/report-design/add-cascading-parameters-to-a-report-report-builder-and-ssrs.md)中所创建的移动报表中使用。  
  
- 无需先选择参数值即可运行报表，因为已经为该参数设置了默认值。  
  
## <a name="report-parameter-properties"></a><a name="bkmk_Report_Parameters"></a> 报表参数属性

 可用使用“报表属性”对话框来更改报表属性参数。 下表总结了可以为各个参数设置的属性：  
  
|properties|说明|  
|--------------|-----------------|  
|名称|键入区分大小写的参数名称。 名称必须以字母开头，可以包含字母、数字、下划线 (_)。 名称中不能包含空格。 对于自动生成的参数，其名称会与数据集查询中的参数相匹配。 默认情况下，手动创建的参数与 ReportParameter1 相类似。|  
|Prompt|在报表查看器工具栏上的参数旁边显示的文本。|  
|数据类型|报表参数必须为以下数据类型之一：<br /><br /> **Boolean** 中所创建的移动报表中使用。 用户通过单选按钮选择 True 或 False。<br /><br /> **DateTime**。 用户从日历控件中选择日期。<br /><br /> **Integer**。 用户在文本框中键入值。<br /><br /> **Float**。 用户在文本框中键入值。<br /><br /> **文本**。 用户在文本框中键入值。<br /><br /> 为参数定义可用值时，用户可从下拉列表中选择值，即使数据类型为 **DateTime** 也是如此。<br /><br /> 有关报表数据类型的详细信息，请参阅 [RDL Data Types](../../reporting-services/reports/report-definition-language-ssrs.md#bkmk_RDL_Data_Types)。|  
|允许空值|如果参数的值可为空字符串或为空白，请选择此选项。<br /><br /> 如果为参数指定有效值，并希望将空白值作为有效值之一，则必须在指定的值中包含空白值。 选择此选项并不会自动在可用值中包含空白值。|  
|允许 Null 值|如果参数的值可为 Null，请选择此选项。<br /><br /> 如果为参数指定有效值，并希望将 Null 作为有效值之一，则必须在指定的值中包含 Null。 选择此选项并不会自动在可用值中包含 Null。|  
|允许多个值|提供可用值以创建下拉列表，供用户从中选择。 这是确保只在数据集查询中提交有效值的好方法。<br /><br /> 如果参数值可为下拉列表中显示的多个值，请选择此选项。 不允许为 Null 值。 选择此选项后，将向参数下拉列表中的可用值列表添加复选框。 列表的顶部包括 **“全选”** 复选框。 用户可以选中所需的值。<br /><br /> 如果用于提供值的数据快速更改，则用户看到的列表可能不是最新列表。|  
|Visible|选择此选项可在报表运行时在其顶部显示报表参数。 此选项允许用户在运行时选择参数值。|  
|Hidden|选择此选项可隐藏已发布报表中的报表参数。 报表参数值仍可在报表 URL、订阅定义或报表服务器中进行设置。|  
|内部|选择此选项可以隐藏报表参数。 在已发布报表中，只能在报表定义中查看报表参数。|  
|可用值|如果已为参数指定可用值，则有效值将始终作为下拉列表显示。 例如，如果为 **DateTime** 参数提供了可用值，则在参数窗格中将显示日期下拉列表而不是显示日历控件。<br /><br /> 为了确保值列表在报表与子报表中是一致的，可以在数据源中设置一个选项，使用单个事务处理与数据源关联的数据集中的所有查询。<br /><br /> **安全说明** 在任何包含数据类型为 Text 的参数的报表中，请务必使用可用值列表（也称为有效值列表），并确保任何运行该报表的用户仅具有查看该报表中数据所必需的权限  。 有关详细信息，请参阅 [安全性（报表生成器）](../../reporting-services/report-builder/security-report-builder.md)中所创建的移动报表中使用。|  
|默认值|设置来自查询或静态列表的默认值。<br /><br /> 如果每个参数均具有默认值，则报表将在第一次查看时自动运行。|  
|高级|设置报表定义属性 **UsedInQuery**，该值指示此参数是直接还是间接影响报表中的数据。<br /><br /> **自动确定何时刷新**<br /> 当您希望报表处理器来确定该值的设置时选择此选项。 如果报表处理器发现数据集查询具有对此参数的直接或间接引用，或者报表具有子报表，则该值为 **True** 。<br /><br /> **始终刷新**<br /> 当报表参数直接或间接用于数据集查询或参数表达式时，请选择此选项。 此选项将 **UsedInQuery** 设置为 True。<br /><br /> **从不刷新**<br /> 当报表参数未直接或间接用于数据集查询或参数表达式时，请选择此选项。 此选项将 **UsedInQuery** 设置为 False。<br /><br /> **注意** 应慎重使用“从不刷新”  。 在报表服务器上， **UsedInQuery** 用于帮助控制报表数据和所呈现报表的高速缓存选项，以及控制快照报表的参数选项。 如果您未正确设置 **“从不刷新”** ，可能导致对不正确的报表数据或报表进行高速缓存，或者导致快照报表具有不一致的数据。 有关详细信息，请参阅[报表定义语言 (SSRS)](../../reporting-services/reports/report-definition-language-ssrs.md)。|  
  
##  <a name="dataset-query"></a><a name="bkmk_Dataset_Parameters"></a> 数据集查询  
 若要筛选数据集查询中的数据，可以通过指定结果集中要包含或排除的值，来包含一个限制检索到的数据的限制子句。  
  
 使用数据源的查询设计器来帮助生成参数化查询。  
  
-   对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询，不同的数据源支持不同的参数语法。 支持范围包括在查询中按位置或名称标识的参数。 有关详细信息，请参阅[报表数据集 (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md) 中针对特定外部数据源类型的相关主题。 在关系查询设计器中，必须为筛选器选择参数选项，才能创建参数化查询。 有关详细信息，请参阅[关系查询设计器用户界面（报表生成器）](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md)。  
  
-   对于基于多维数据源（例如 Microsoft SQL Server Analysis Services、SAP NetWeaver BI 或 Hyperion Essbase）的查询，可以指定是否创建基于查询设计器中您指定的筛选器的参数。 有关详细信息，请参阅[查询设计工具 &#40;SSRS&#41;](../report-data/query-design-tools-ssrs.md) 中与数据扩展插件对应的查询设计器主题。  
  
##  <a name="parameter-management-for-a-published-report"></a><a name="bkmk_Manage_Parameters"></a> 已发布报表的参数管理  
 当您设计报表时，报表参数保存在报表定义中。 当您发布报表时，报表参数与报表定义分开保存和管理。  
  
 对于已发布报表，可以使用：  
  
-   **报表参数属性。** 直接在报表服务器上独立于报表定义更改报表参数值。  
  
-   **缓存的报表。** 若要为报表创建缓存计划，每个参数都必须具有默认值。 有关详细信息，请参阅 [缓存报表 (SSRS)](../../reporting-services/report-server/caching-reports-ssrs.md)版本中预加载缓存的唯一方法。  
  
-   **缓存共享数据集。** 若要为共享数据集创建缓存计划，每个参数都必须具有默认值。 有关详细信息，请参阅 [缓存报表 (SSRS)](../../reporting-services/report-server/caching-reports-ssrs.md)版本中预加载缓存的唯一方法。  
  
-   **链接报表。** 可以使用预设参数值为不同受众创建链接报表以筛选数据。 有关详细信息，请参阅 [创建链接报表](../../reporting-services/reports/create-a-linked-report.md)。  
  
-   **报表订阅。** 可以指定参数值以筛选数据并通过订阅传递报表。 有关详细信息，请参阅[订阅和传递 (Reporting Services)](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)。  
  
-   **URL 访问。** 可以在指向报表的 URL 中指定参数值。 还可以使用 URL 访问运行报表并指定参数值。 有关详细信息，请参阅 [URL 访问 (SSRS)](../../reporting-services/url-access-ssrs.md)。  
  
 如果重新发布报表定义，则会保留已发布报表的参数属性。 如果报表定义重新发布为同一报表，并且参数名和数据类型不变，则属性设置将保留不变。 如果添加或删除了报表定义中的参数，或是更改了现有参数的数据类型或名称，则您最好更改已发布报表中的参数属性。  
  
 并非在所有情况下都可以修改所有参数。 如果报表参数从查询中获取默认值，则无法为已发布报表修改该值，并且无法在报表服务器上修改该值。 运行时使用的值将在运行查询时确定，如果是基于表达式的参数，则在对表达式求值时确定。  
  
 报表执行选项可以影响参数的处理方式。 作为快照运行的报表不能使用来自查询的参数，除非该查询包含这些参数的默认值。  
  
##  <a name="parameters-for-a-subscription"></a><a name="bkmk_Parameters_Subscription"></a> 订阅参数  
 您可以定义按需订阅或快照订阅，可以指定在订阅处理过程中所用的参数值。  
  
-   按需运行报表  。  对于按需报表，你可以指定不同于每个参数（该报表所列的参数）的已发布值的参数值。 例如，假设有一个 Call Service 报表使用 *Time Period* 参数返回当前日、周或月的客户服务请求。 如果报表的默认参数值设置为“今天”  ，则订阅可以使用不同的参数值（例如，“周”  或“月”  ）以生成包含每周或每月数字的报表。  
  
-   **快照。**  对于快照，订阅必须使用为快照定义的参数值。 您的订阅不能覆盖为快照定义的参数值。 例如，假设您要订阅作为报表快照运行的西部地区销售报表，并且该快照指定 **Western** 作为区域参数值。 在这种情况下，如果创建对此报表的订阅，则必须在订阅中使用参数值 **Western** 。 若要提供忽略参数的可见说明，则应将订阅页上的参数字段设置为只读字段。  
  
     报表执行选项可以影响参数的处理方式。 作为报表快照运行的参数化报表使用为报表快照定义的参数值。 参数值在报表的参数属性页中定义。 作为快照运行的报表不能使用来自查询的参数，除非该查询包含这些参数的默认值。  
  
     如果在定义订阅之后更改了报表快照中的参数值，则报表服务器将停用该订阅。 停用订阅表示该报表已经修改。 若要激活该订阅，请先打开订阅，然后保存即可。  
  
> [!NOTE]  
>  数据驱动订阅可以使用从订阅服务器数据源中获取的参数值。 有关详细信息，请参阅[使用外部数据源提供订阅方数据（数据驱动订阅）](../../reporting-services/subscriptions/use-an-external-data-source-for-subscriber-data-data-driven-subscription.md)。  
  
 有关详细信息，请参阅[订阅和传递 (Reporting Services)](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)。  
  
##  <a name="parameters-and-securing-data"></a><a name="bkmk_Parameters_Security"></a> 参数和数据保护  
 在分发包含保密信息或敏感信息的参数化报表时要谨慎。 用户可能会很容易地将报表参数替换为其他值，从而导致您不希望发生的信息泄露。  
  
 另一种将参数用于雇员或个人数据的安全方法是：基于包含 Users 集合中的 **UserID** 字段的表达式选择数据。 Users 集合提供了获取报表运行用户的标识的方法，并使用该标识检索用户特定的数据。  
  
> [!IMPORTANT]  
>  在任何包括 **String** 类型参数的报表中，请务必使用可用值列表（也称为有效值列表），并确保任何运行该报表的用户仅具有查看该报表中数据所必需的权限。 定义 **String** 类型的参数时，系统将向用户显示一个可以使用任何值的文本框。 可用值列表限制可以输入的值。 如果报表参数与数据集参数关联，并但您没有使用可用值列表，则报表用户可能会在文本框中键入 SQL 语法，从而导致报表和服务器容易受到 SQL 注入攻击。 如果用户有足够的权限执行新的 SQL 语句，则可能在服务器上产生意外结果。  
>   
>  如果报表参数与数据集参数无关联，并且参数值包含在报表中，则报表用户可能会在参数值中键入表达式语法或 URL，并将报表呈现为 Excel 或 HTML 格式。 如果其他用户查看报表并单击呈现的参数内容，则用户可能会无意中执行恶意脚本或链接。  
>   
>  若要降低无意中运行恶意脚本的风险，请仅从可信来源打开呈现的报表。 有关保护报表的详细信息，请参阅 [保护报表和资源](../../reporting-services/security/secure-reports-and-resources.md)。  

##  <a name="related-sections"></a><a name="bkmk_Related_Topics"></a> 相关章节  

 [教程：向报表添加参数（报表生成器）](../../reporting-services/tutorial-add-a-parameter-to-your-report-report-builder.md)  
  
[报表参数概念](../../reporting-services/report-design/report-parameters-concepts-report-builder-and-ssrs.md)  
  
 [报表示例（报表生成器和 SSRS）](https://go.microsoft.com/fwlink/?LinkId=198283)  
  
 [在报表中使用表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)  
  
 [表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
 [对数据进行筛选、分组和排序（报表生成器和 SSRS）](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)  
  
 [安全性（报表生成器）](../../reporting-services/report-builder/security-report-builder.md)  
  
 [交互式排序、文档结构图和链接（报表生成器和 SSRS）](../../reporting-services/report-design/interactive-sort-document-maps-and-links-report-builder-and-ssrs.md)  
  
 [钻取、深化、子报表和嵌套数据区域（报表生成器和 SSRS）](../../reporting-services/report-design/drillthrough-drilldown-subreports-and-nested-data-regions.md)  
  
  
