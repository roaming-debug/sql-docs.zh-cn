---
title: 数据警报设计器 | Microsoft Docs
description: 了解数据警报定义，以及如何在数据警报设计器中创建和编辑数据警报定义。
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- editing, data alerts
- updating, data alerts
- editing, alerts
- updating, alerts
- creating, data alerts
- creating, alerts
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: e83db2f4ce3a049a6c285b36a2d6a611369ca7bb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484349"
---
# <a name="data-alert-designer"></a>数据警报设计器

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE [ssrs-appliesto-not-2017](../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../includes/ssrs-previous-versions.md)]

使用数据警报设计器创建和编辑数据警报定义。 警报定义是元数据的一个集合，包括您感兴趣的报表数据、报表数据必须满足才能创建数据警报实例和发送数据警报消息的规则、警报消息的收件人等。  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

 若要创建警报定义，您需要执行许多相关任务：  
  
-   选择包含您要使用的数据的报表和报表数据馈送。  
  
-   定义导致发送警报的规则和子句。 通过使用由 AND 运算符组合的多个子句，这些规则可以简单或复杂。  
  
-   定义发送警报消息的频率以及警报开始和停止的日期和时间。 仅当结果更改时才发送警报消息。  
  
-   指定警报消息收件人的电子邮件地址。  
  
-   自定义警报消息的 **“主题”** 行。  
  
-   提供要在警报消息中包括的警报说明。  
  
> [!NOTE]  
>  因为只有在 SharePoint 模式下安装了 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 后， [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 数据警报功能才可用，所以，你想要对其创建警报的报表必须保存、部署或上传到 SharePoint 文档库中。  
>   
>  不能针对使用 Windows 集成身份验证或提示输入凭据的报表创建数据警报。 报表必须使用已存储的凭据。 有关详细信息，请参阅[为报表数据源指定凭据和连接信息](../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)  
  
 若要打开数据警报设计器，请单击报表工具栏上 **“操作”** 菜单中的 **“新建数据警报”** 选项。 如果您看不到 **“新建数据警报”** 选项，则此报表未配置为使用存储的凭据。 您可以通过从 SharePoint 库更新报表数据源来更新凭据类型。  
  
##  <a name="data-alert-designer-user-interface"></a><a name="AlertDesigner"></a> 数据警报设计器用户界面  
 数据警报设计器分为若干区域。 其中包括用于选择报表数据馈送的区域、通过向条件添加规则来创建简单或复杂条件的区域等区域。 下图显示数据警报设计器中的区域。  
  
 ![“警报设计器”用户界面中的区域](../reporting-services/media/rs-alertdesigner.gif "“警报设计器”用户界面中的区域")  
  
  
### <a name="alert-data"></a>警报数据  
 在打开数据警报设计器时，它将从报表生成所有数据馈送并使它们变得可用，并且“报表数据名称”下拉列表将包含各个馈送的名称。 在您创建警报定义时数据馈送将缓存在内存中，并且当您在数据馈送之间切换以便浏览报表数据时将快速填充显示数据馈送数据的表。  
  
 创建数据警报定义的第一步是选择包含您希望监视警报的数据的报表数据馈送。 报表可以有零个或多个数据馈送。 如果某个报表没有数据馈送，则无法对其创建警报。 数据馈送可由任何数据区域生成，包括所有类型的图表、仪表、指示器以及表、矩阵和列表。  
  
 如果报表已参数化，并且您在报表数据馈送中看不到预期的数据和列，则使用适当的参数值重新运行该报表。 这些列和值必须存在于要包括在数据馈送中的报表中。  
  
 根据报表的布局，您不见得能够直观地看到一个报表具有多少数据馈送以及数据馈送中包含哪些数据。 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]Atom 呈现扩展插件将生成你要用于警报的数据馈送。 该 Atom 呈现扩展插件将报表数据作为平展的行集提供，这是一种所有列都具有相同行数的表格格式。 这些行集是数据馈送的内容。 因为报表布局通常比较复杂并且包含多个对等或嵌套的数据区域，所以，需要多个数据馈送以便可供所有报表数据使用。 有关如何从报表生成数据馈送的详细信息，请参阅[基于报表生成数据馈送（报表生成器和 SSRS）](../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)和[从报表生成数据馈送（报表生成器和 SSRS）](../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md)。  
  
 在您选择某一数据馈送时，来自该数据馈送的数据将显示在一个具有行和列的表中（显示在数据警报设计器的警报数据窗格中）。 来自报表使用的数据源或报表本身的元数据指定列名，并且数据馈送填充您用于在数据条件中定义规则的字段列表。 该数据馈送还提供元数据，例如用于限制值的表列的数据类型以及在您创建规则时可用于字段的比较运算符。  
  
 某些报表具有数百万行数据。 该表仅显示数据馈送中的前 100 行数据。  
  
### <a name="alert-name"></a>警报名称  
 默认情况下，警报定义具有与报表相同的名称。 您可以将此警报名称更改为更有意义的名称。 这便于您管理警报，以及确定要用于更新、删除等操作的警报。  
  
 您可以对一个报表创建多个警报。 可以具有同名的多个警报定义，但建议您保持警报名称唯一。 这样可便于区分和管理警报定义。 您可以在数据警报管理器中查看所创建的所有警报的列表。 有关详细信息，请参阅 [向管理员提出警报的数据警报管理器](../reporting-services/data-alert-manager-for-alerting-administrators.md) 和 [在数据警报管理器中管理我的数据警报](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md)。  
  
### <a name="rules-and-clauses"></a>规则和子句  
 数据更改的作用域以及警报规则中的规则定义触发警报的数据更改。 数据更改的作用域如下：  
  
-   **任何数据具有** - 数据中至少一个值符合该条件指定的规则。  
  
-   **没有任何数据具有** - 数据中没有任何值符合该条件指定的规则。  
  
 一个规则可包含零个、一个或多个子句。 多个规则由 AND 逻辑运算符组合在一起。 如果该列具有字符串数据类型，则规则可包含使用 OR 运算符组合的多个子句。 下面的内容显示仅使用一个子句的基本规则、使用 AND 运算符组合的多个规则、使用一个或多个 OR 子句的多个规则。  
  
 **简单规则**  
  
-   净销售额 **“大于”** 100000  
  
-   销售日期“晚于”2010/6/1  
  
-   公司名称 **“不是”** Contoso  
  
 **AND 运算符组合的规则**  
  
-   销售额 **“大于”** 1500.00  
  
     **“并且”** 已销售的单位数 **“小于”** 500  
  
     返回日期“早于”2010/1/1  
  
-   销售额 **“大于或等于”** 1500.00  
  
     “并且”返回日期“晚于”2010/1/1   
  
     **“并且”** 已销售的单位数 **“大于”** 500  
  
-   促销名称 **“包含”** 春季  
  
     **“并且”** 已销售的单位数 **“大于”** 500  
  
     **“并且”** 返回 **“是”**  0  
  
 **使用 OR 子句的规则**  
  
-   最后名称 **“是”** Blythe  
  
     **“或者”**  Petulescu  
  
     **“或者”**  Martin  
  
-   返回日期“晚于”2010/1/1  
  
     **“并且”** 销售区域 **“是”**  中央  
  
     **“或者”**  南方  
  
     **“或者”**  北方  
  
 根据字段的数据类型，数据警报设计器提供了不同的比较。 数据警报设计器根据要进行比较值的字段的数据类型提供比较。 下表列出了适用于不同数据类型的比较。 规则中不支持 **Boolean** 数据类型。  
  
-   日期时间数据类型比较是： **“是”** 、 **“不是”** 、 **“早于”** 和 **“晚于”**  
  
-   数值数据类型比较是： **“是”**、 **“不是”**、 **“小于”**、 **“小于或等于”** 和 **“大于”** 以及 **“大于或等于”**  
  
-   字符串数据类型比较是： **“是”** 、 **“不是”** 和 **“包含”**  
  
 在创建规则时，通过选中 **“值输入模式”** 或者 **“字段选择模式”** 来确定在比较中使用值还是字段。 如果您选中 **“值输入模式”** ，则提供要比较的值列表。 包含多个 OR 子句的比较与 [!INCLUDE[tsql](../includes/tsql-md.md)]中的 IN 逻辑比较非常类似，后者是用来测试匹配的值列表。 有关详细信息，请参阅 [IN (Transact-SQL)](../t-sql/language-elements/in-transact-sql.md)。  
  
 如果选中 **“字段选择模式”** ，则在两个字段之间逐行进行比较。 这两个字段必须具有兼容的数据类型（例如，两个数字字段），否则比较无效。 当您选中 **“字段选择模式”** 时，会自动显示一个字段列表。  
  
 没有规则的数据警报也是有效的。 这种类型的警报可能非常有用。 假设一种应用场景，只有在报表数据馈送具有数据时才会通知您。 该数据馈送包含参加者信息并且在参加者取消前该数据馈送为空。 在这个应用场景中，您将收到一个警报，从第一个取消开始。  
  
 您可以删除各规则和子句。  
  
 数据警报消息中包含规则和子句。  
  
### <a name="schedule-settings"></a>计划设置  
 您为数据警报定义的计划将定义用于发送数据警报消息的重复执行模式以及何时开始和停止发送警报消息。 该模式为：一次、每分钟、每天和每周。 尽管一个警报只有一个计划，但您可以通过使用这些时间间隔创建满足大多数业务需要的复杂的重复执行模式。 下面是在计划中常用的重复执行模式的示例：  
  
-   **每天，每隔 10 天** - 将警报一天发送一次，每隔 10 天。  
  
-   **每周，每 2 周的星期一** - 仅在每两周的星期一发送警报。  
  
-   **每小时，每隔 12 小时** - 每隔 12 小时发送警报。  
  
-   **分钟，每隔 30 分钟** - 每隔 30 分钟发送警报。  
  
 重复执行模式指定何时发送警报。 如果在该模式指定的间隔期间满足规则，则在该间隔结束前将不发送警报。  
  
 如果想在报表数据遵循指定的规则时尽快接收数据警报消息，则可计划经常运行警报。 如果报表数据没有更改，则您和其他收件人可能收到许多冗余消息。 如果想在来自应用该规则的结果更改时才接收消息，则选择 **“仅当结果更改时才发送消息”** 选项。  
  
> [!IMPORTANT]  
>  建议您不要以高于每天一次的频率使用重复执行模式，除非有重要的业务原因需要这么做。 不支持实时处理数据警报定义。 过于频繁地处理数据警报定义会影响报表服务器和总体 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 部署的性能。  
  
### <a name="email-settings"></a>电子邮件设置  
 在“收件人”选项中，指定通过电子邮件接收数据警报消息的收件人的电子邮件地址。 多个电子邮件地址由分号分隔，与在 Microsoft Office Outlook 电子邮件中的操作方式相同。 还可以指定分发组作为收件人，以便更容易和更有效地管理收件人列表。 如果在您创建警报定义时 SharePoint 可以确定您的电子邮件地址，则您的电子邮件地址将自动添加到收件人列表中；否则，您需要显式将您自己作为收件人添加。  
  
 电子邮件的默认主题为“\<alert name> 的数据警报”。 您可以更改主题以适合您的需要。  
  
 还可以提供一个说明以包含在 **“说明”** 选项的数据警报消息中。 包含说明（尤其如果您具有类似的数据警报）将帮助您快速区分和了解您的警报消息。 除了在报表服务器满足指定规则时发送的警报消息之外，在发生错误时还向所有收件人发送一条警报消息。 有关详细信息，请参阅 [Data Alert Messages](../reporting-services/data-alert-messages.md)。  
  
 有关如何生成电子邮件的详细信息，请参阅 [Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)。  
  
##  <a name="create-a-data-alert-definition"></a><a name="CreateAlert"></a> 创建数据警报定义  
 如果您被授予了 SharePoint 的“查看项”和“创建警报”权限，只要报表使用存储的凭据或没有凭据，您就可以为您有权查看的任何报表创建数据警报定义。 您从 SharePoint 库运行该报表。 您可在数据警报设计器中使用的数据来自该报表。 如果对此报表进行参数化，你可能需要使用不同的参数值来运行此报表，从而确保你感兴趣的数据出现在此报表中。 在该报表打开后，单击报表工具栏上 **“操作”** 菜单中的 **“新建数据警报”** 选项，以便打开数据警报设计器。 下图显示如何打开数据警报设计器。  
  
 ![从 SharePoint 库打开警报设计器](../reporting-services/media/rs-openalertdesigneriw.gif "从 SharePoint 库打开警报设计器")  
  
 有关详细信息，请参阅 [在数据警报设计器中创建数据警报](../reporting-services/create-a-data-alert-in-data-alert-designer.md)。  
  
  
##  <a name="save-a-data-alert-definition"></a><a name="SaveAlert"></a> 保存数据警报定义  
 数据警报设计器显示将保存数据警报定义的站点的 URL。 数据警报定义始终作为报表保存到相同的站点。  
  
> [!NOTE]  
>  您选择运行报表的参数值保存在警报定义中，并在作为处理警报定义的一个步骤重新运行报表时使用。 若要使用不同的参数值，您必须创建一个新的警报定义。  
  
 在保存警报定义前，要对该定义进行验证。 您必须首先纠正所有错误，然后才能成功保存警报定义。 有关详细信息，请参阅 [在数据警报设计器中创建数据警报](../reporting-services/create-a-data-alert-in-data-alert-designer.md)。  
  
  
##  <a name="edit-a-data-alert-definition"></a><a name="EditAlert"></a> 编辑数据警报定义  
 在保存数据警报定义后，可以在数据警报设计器中重新打开和编辑该定义。 您可以添加、更改或删除规则和子句以及更改计划和电子邮件设置。 如果警报使用的报表数据馈送已更改，并且不再提供警报规则引用的字段，或者这些字段的数据类型或其他元数据已更改，则该警报定义将不再有效，您必须纠正错误后才能重新保存它。 如果您要使用不同的数据馈送，则必须创建一个新的警报定义。  
  
 若要编辑数据警报定义，请在警报管理器中右键单击，然后单击 **“编辑”** 。 下图显示了数据警报管理器中数据警报上的上下文菜单。  
  
 ![通过单击“编辑”打开数据警报设计器](../reporting-services/media/rs-alertmanageriwopendesigner.gif "通过单击“编辑”打开数据警报设计器")  
  
 有关详细信息，请参阅 [在警报设计器中编辑数据警报](../reporting-services/edit-a-data-alert-in-alert-designer.md)。  
  
  
##  <a name="related-tasks"></a><a name="HowTo"></a> 相关任务  
 本节列出的过程介绍如何创建和编辑警报。  
  
-   [在警报设计器中编辑数据警报](../reporting-services/edit-a-data-alert-in-alert-designer.md)  
  
-   [在数据警报设计器中创建数据警报](../reporting-services/create-a-data-alert-in-data-alert-designer.md)  

## <a name="see-also"></a>另请参阅

[Reporting Services 数据警报](../reporting-services/reporting-services-data-alerts.md)   
[适用于警报管理员的数据警报管理器](../reporting-services/data-alert-manager-for-alerting-administrators.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
