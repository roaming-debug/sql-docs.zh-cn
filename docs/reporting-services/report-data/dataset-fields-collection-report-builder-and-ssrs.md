---
title: 数据集字段集合（报表生成器）| Microsoft Docs
description: 了解数据集字段集合。 数据集字段表示来自数据连接的数据。 字段可以表示数值数据，也可以表示非数值数据。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: b3884576-1f7e-4d40-bb7d-168312333bb3
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6ad6853715963c9a29ee4db2e73199fecd50801a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597326"
---
# <a name="dataset-fields-collection-report-builder-and-ssrs"></a>数据集字段集合（报表生成器和 SSRS）
  数据集字段表示来自数据连接的数据。 字段可以表示数值数据，也可以表示非数值数据。 示例包括销售额、总销售额、客户名称、数据库标识符、URL、图像、空间数据和电子邮件地址。 在设计图面上，字段显示为报表项（如文本框、表和图表）中的表达式。  
  
 报表有三种类型的字段，它们显示在“报表数据”窗格中：数据集字段、数据集计算字段和内置字段。  
  
-   **数据集字段。** 对数据源运行数据集查询时，表示要返回的字段集合的元数据。  
  
-   **数据集计算字段。** 为数据集创建的其他字段。 通过计算定义的表达式来创建各个计算字段。  
  
-   **内置字段。** 表示由报表生成器提供的字段集合的元数据，字段集合提供报表信息，如报表名称或处理报表的时间。 有关详细信息，请参阅[内置的全局和用户引用（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md)。  
  
 数据集字段名称保存为报表数据集定义的一部分。 有关详细信息，请参阅 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="dataset-fields-and-queries"></a><a name="Fields"></a> 数据集字段和查询  
 数据集字段由数据集查询命令和您定义的所有计算字段指定。 在报表中所看到的字段集合取决于您所使用的数据集类型：  
  
-   **共享数据集。** 在将共享数据集直接添加到报表后，或者当添加包含共享数据集的报表部件后，字段集合为用于共享数据集定义中的查询的字段列表。 当在报表服务器上更改共享数据集定义时，本地字段集合不会发生更改。 若要更新本地字段集合，必须刷新本地共享数据集的列表。  
  
-   **嵌入的数据集。** 字段集合是针对数据源运行当前查询所返回的字段列表。  
  
 有关详细信息，请参阅[在“报表数据”窗格中添加、编辑和刷新字段（报表生成器和 SSRS）](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md)  
  
  
### <a name="calculated-fields"></a>计算字段  
 通过创建表达式可以手动指定计算字段。 计算字段可用于创建数据源中不存在的新值。 例如，计算字段可以表示新值、一组字段值的自定义排序顺序，也可以表示转换为另一数据类型的现有字段。  
  
 计算字段对于报表为本地字段，不能保存为共享数据集的一部分。  
  
 有关详细信息，请参阅[在“报表数据”窗格中添加、编辑和刷新字段（报表生成器和 SSRS）](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md)。  
  
  
### <a name="entities-and-entity-fields"></a>实体和实体字段  
 如果您使用报表模型数据源，则指定实体和实体字段作为报表数据。 在报表模型的查询设计器中，可以通过交互方式浏览和选择相关实体，并选择要包含在报表数据集中的字段。 完成查询设计后，可以在“报表数据”窗格中查看实体标识符和实体字段的集合。 实体标识符由报表模型自动生成，通常不显示给最终用户。  
  
### <a name="using-extended-field-properties"></a>使用扩展字段属性  
 支持多维查询的数据源（例如，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]）支持字段的字段属性。 字段属性显示在查询的结果集中，但是在 **“报表数据”** 窗格中不可见。 它们仍可在报表中使用。 若要引用字段的属性，请将该字段拖到报表中，然后将默认属性 **Value** 更改为所需属性的字段名。 例如，在 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 多维数据集中，可以定义多维数据集单元格中的值格式。 通过使用字段属性 **FormattedValue** 可使用已格式化值。 若要直接使用该格式化值，而不是使用一个值，然后再设置文本框的格式属性，请将该字段拖到该文本框中，然后将默认表达式 `=Fields!FieldName.Value` 更改为 `=Fields!FieldName.FormattedValue`。  
  
> [!NOTE]
>  并非所有的 **Field** 属性均可用于所有数据源。 针对所有的数据源定义 **Value** 和 **IsMissing** 属性。 仅当数据源提供了其他预定义的属性（例如，多维数据源的 **Key**、 **UniqueName** 和 **ParentUniqueName** ）时，才支持这些属性。 某些数据访问接口支持自定义属性。 有关详细信息，请参阅 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)。 例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据源，请参阅 [Analysis Services 数据库的扩展字段属性 (SSRS)](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md)。  
  
  
##  <a name="understanding-default-expressions-for-fields"></a><a name="Defaults"></a> 了解字段的默认表达式  
 文本框可以是报表正文中的文本框报表项，也可以是 Tablix 数据区域的单元中的一个文本框。 将字段与文本框进行链接时，文本框的位置决定了字段引用的默认表达式。 在报表正文中，文本框值表达式必须指定聚合和数据集。 如果报表中只有一个数据集，系统将为您创建此默认表达式。 对于表示数值的字段，默认聚合函数为 SUM。 对于表示非数值的字段，默认聚合为 First。  
  
 在 Tablix 数据区域中，默认字段表达式取决于字段所添加到的文本框的行成员身份和组成员身份。 将“销售量”字段添加到表详细信息行的文本框中时，字段表达式为 `[Sales]`。 如果将同一字段添加到组头中的文本框，则默认表达式为 `(Sum[Sales])`，这是因为组头显示组的汇总值，而不是详细值。 报表运行时，报表处理器对每个表达式进行计算，然后替换报表中的结果。  
  
 有关表达式的详细信息，请参阅[表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)。  
  
  
##  <a name="field-data-types"></a><a name="DataTypes"></a> 字段数据类型  
 创建数据集后，数据源中字段的数据类型可能不与报表中使用的数据类型完全相同。 数据类型可能经过一到两层映射。 数据处理扩展插件或数据访问接口可能将数据源中的数据类型映射到公共语言运行时 (CLR) 数据类型。 数据处理扩展插件返回的数据类型将映射到 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]中的公共语言运行时 (CLR) 数据类型的子集。  
  
 在数据源中，数据以该数据源所支持的数据类型存储。 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的数据必须是受支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型之一，例如 **nvarchar** 或 **datetime**。 从数据源检索数据时，数据通过与数据源类型相关联的数据处理扩展插件或数据访问接口进行传递。 数据可能从数据源所使用的数据类型转换为数据处理扩展插件所支持的数据类型，具体情况取决于数据处理扩展插件。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 使用随 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]一起安装的公共语言运行时 (CLR) 所支持的数据类型。 数据访问接口将结果集中的每一列从本机数据类型映射到 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 数据类型。  
  
 在每个阶段，数据分别由不同的数据类型表示，如以下列表所描述：  
  
-   **数据源** ：所连接的数据源类型版本支持的数据类型。  
  
     例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据源的典型数据类型包括 **int**、 **datetime** 和 **varchar**。 由 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 引入的数据类型增加了对 **date**、 **time**、 **datetimetz** 和 **datetime2** 的支持。 有关详细信息，请参阅 [数据类型 (Transact-SQL)](/previous-versions/sql/sql-server-2008/ms187752(v=sql.100))。  
  
-   **数据访问接口或数据处理扩展插件** ：连接到数据源时选择的数据处理扩展插件的数据访问接口版本所支持的数据类型。 基于 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 的数据访问接口使用 CLR 支持的数据类型。 有关 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序数据类型的详细信息，请参阅 MSDN 上的 [数据类型映射 (ADO.NET)](/dotnet/framework/data/adonet/data-type-mappings-in-ado-net) 和 [使用基本类型](/dotnet/standard/base-types/common-type-system) 。  
  
     例如， [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 支持的典型数据类型包括 **Int32** 和 **String**。 **DateTime** 结构支持日历日期和时间。 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 2.0 Service Pack 1 引入了对 **DateTimeOffset** 结构的支持，用于表示带时区偏移量的日期。  
  
    > [!NOTE]  
    >  报表服务器使用在报表服务器上安装和配置的数据访问接口。 处于预览模式的报表创作客户端使用客户端计算机上安装并配置的数据处理扩展插件。 您必须在报表客户端环境和报表服务器环境中测试报表。  
  
-   **报表处理器** 数据类型基于在安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]时所安装的 CLR 版本。  
  
     例如，下表显示了报表处理器用于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 中引入的新日期和时间类型的数据类型：  
  
    |SQL 数据类型|CLR 数据类型|说明|  
    |-------------------|-------------------|-----------------|  
    |**Date**|**DateTime**|仅日期|  
    |**时间**|**TimeSpan**|仅时间|  
    |**DateTimeTZ**|**DateTimeOffset**|带有时区偏移量的日期和时间|  
    |**DateTime2**|**DateTime**|带有毫秒小数部分的日期和时间|  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库类型的详细信息，请参阅 [数据类型（数据库引擎）](/previous-versions/sql/sql-server-2008/ms187752(v=sql.100)) 和 [日期和时间数据类型及函数 (Transact-SQL)](/previous-versions/sql/sql-server-2008/ms186724(v=sql.100))。  
  
 有关在表达式中包括对数据集字段的引用的详细信息，请参阅[表达式中的数据类型（报表生成器和 SSRS）](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)。  
  
  
##  <a name="detecting-missing-fields-at-run-time"></a><a name="MissingFields"></a> 检测运行时缺失的字段  
 处理报表时，数据集的结果集可能会由于数据源中不再存在指定的所有列而不包含这些列的值。 可以使用字段属性 IsMissing 检测在运行时是否返回了某个字段的值。 有关详细信息，请参阅[数据集字段集合引用（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-dataset-fields-collection-references-report-builder.md)。  
  
  
## <a name="see-also"></a>另请参阅  
 [“数据集属性”对话框 ->“字段”（报表生成器）]()   
 [报表生成器中的报表部件和数据集](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
