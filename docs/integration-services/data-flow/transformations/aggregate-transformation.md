---
description: 聚合转换
title: 聚合转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.aggregatetrans.f1
- sql13.dts.designer.aggregationtransformation.aggregations.f1
- sql13.dts.designer.aggregationtransformation.advanced.f1
helpviewer_keywords:
- IsBig property
- aggregate functions [Integration Services]
- Aggregate transformation [Integration Services]
- large data, SSIS transformations
ms.assetid: 2871cf2a-fbd3-41ba-807d-26ffff960e81
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c99a1d737e2b3d5c928f1b555915a14443b30584
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123451"
---
# <a name="aggregate-transformation"></a>聚合转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  聚合转换将聚合函数（如 Average）应用于列值，并将结果复制到转换输出。 除聚合函数以外，转换还提供 GROUP BY 子句，用于指定所要聚合的组。  
  
## <a name="operations"></a>Operations  
 聚合转换支持下列运算。  
  
|Operation|说明|  
|---------------|-----------------|  
|Group by|将数据集划分为组。 任何数据类型的列都可用于分组。 有关详细信息，请参阅 [GROUP BY (Transact-SQL)](../../../t-sql/queries/select-group-by-transact-sql.md)。|  
|Sum|对列中的值求和。 只能对数值数据类型的列求和。 有关详细信息，请参阅 [SUM (Transact-SQL)](../../../t-sql/functions/sum-transact-sql.md)。|  
|平均值|返回列中值的平均值。 只能对数值数据类型的列求平均值。 有关详细信息，请参阅 [AVG (Transact-SQL)](../../../t-sql/functions/avg-transact-sql.md)。|  
|计数|返回组中的项数。 有关详细信息，请参阅 [COUNT (Transact-SQL)](../../../t-sql/functions/count-transact-sql.md)。|  
|Count distinct|返回组中的唯一非空值的数量。|  
|最小值|返回组中的最小值。 有关详细信息，请参阅 [MIN (Transact-SQL)](../../../t-sql/functions/min-transact-sql.md)。 与 Transact-SQL MIN 函数不同，此运算只能用于数值、日期和时间数据类型。|  
|最大值|返回组中的最大值。 有关详细信息，请参阅 [MAX (Transact-SQL)](../../../t-sql/functions/max-transact-sql.md)。 与 Transact-SQL MAX 函数不同，此运算只能用于数值、日期和时间数据类型。|  
  
 聚合转换处理空值的方式和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 关系数据库引擎相同。 此行为在 SQL-92 标准中定义。 下列规则适用：  
  
-   在 GROUP BY 子句中，空值和其他列值处理方式类似。 如果分组列包含多个空值，那么这些空值将放入单独的一组中。  
  
-   在 COUNT（列名称）和 COUNT（DISTINCT 列名称）函数中，空值将被忽略，并且结果将排除包含指定列中的空值的行。  
  
-   在 COUNT (*) 函数中，对所有行计数，包括含空值的行。  
  
## <a name="big-numbers-in-aggregates"></a>聚合中的大数  
 列可能包含因数值庞大或精度要求高而需要特别考虑的数值。 聚合转换包含 IsBig 属性，你可以针对输出列设置它，以便调用对大数或高精度数字的特别处理。 如果列值可能超过 40 亿，或者需要超过 float 数据类型的精度，则应将 IsBig 设置为 1。  
  
 将 IsBig 属性设置为 1 将以下列方式影响聚合转换的输出：  
  
-   使用 DT_R8 数据类型，而不用 DT_R4 数据类型。  
  
-   计数结果以 DT_UI8 数据类型存储。  
  
-   非重复计数结果以 DT_UI4 数据类型存储。  
  
> [!NOTE]  
>  在用于 GROUP BY、Maximum 或 Minimum 运算的列中，不能将 IsBig 设置为 1。  
  
## <a name="performance-considerations"></a>性能注意事项  
 聚合转换包含一组属性，对其进行设置可增强转换的性能。  
  
-   执行 **Group by** 运算时，请设置组件的 Keys 或 KeysScale 属性和组件输出。 通过使用 Keys，可指定要求转换处理的精确键数。 （在此上下文中，Keys 指的是要求 **Group by** 运算产生的组数。）使用 KeysScale，可以指定大致的键数。 为 Keys 或 KeyScale 指定适当的值将会提高性能，原因是转换能够为转换缓存的数据分配足量的内存。  
  
-   执行 **Distinct count** 运算时，请设置组件的 CountDistinctKeys 或 CountDistinctScale 属性。 使用 CountDistinctKeys 可指定要求转换处理的非重复计数运算的精确键数。 （在此上下文中，CountDistinctKeys 指的是要求 **Distinct count** 运算产生的非重复值数。）使用 CountDistinctScale，可指定非重复计数运算的大致键数。 为 CountDistinctKeys 或 CountDistinctScale 指定适当的值将会提高性能，原因是转换能够为转换缓存的数据分配足量的内存。  
  
## <a name="aggregate-transformation-configuration"></a>聚合转换配置  
 在转换级、输出级和列级配置聚合转换。  
  
-   在转换级，可以通过指定以下值配置聚合转换，以便提高性能：  
  
    -   要求 **Group by** 运算产生的组数。  
  
    -   要求 **Count distinct** 运算产生的非重复值数。  
  
    -   聚合过程中内存可扩展的百分比。  
  
     还可以将聚合转换配置为在除数为零时生成警告而不是失败。  
  
-   在输出级，可以通过指定要求 **Group by** 运算产生的组数配置聚合转换，以提高性能。 聚合转换支持多个输出，每个输出可采用不同配置。  
  
-   在列级，可以指定以下值：  
  
    -   列执行的聚合。  
  
    -   聚合的比较选项。  
  
 还可以通过指定以下值配置聚合转换，以便提高性能：  
  
-   要求对列执行的 **Group by** 运算产生的组数。  
  
-   要求对列执行的 **Count distinct** 运算产生的非重复值数。  
  
 如果列包含很大的数值或精度很高的数值，还可以将这样的列标识为 IsBig。  
  
 聚合转换是异步过程，也就是说它并非逐行地占用和发布数据， 而是占用整个行集，执行其分组和聚合操作，然后发布结果。  
  
 此转换不传递任何列，而是在数据流中为发布的数据创建新列。 只有应用聚合函数的输入列或转换用于分组的输入列才复制到转换输出。 例如，聚合转换输入可能有三列： **CountryRegion**、 **City** 和 **Population**。 转换按 **CountryRegion** 列分组，并对 **Population** 列应用 Sum 函数。 因此，输出不包含 **City** 列。  
  
 也可将多个输出添加到聚合转换，并将每个聚合定向到不同输出。 例如，如果聚合转换应用 Sum 和 Average 函数，则可以将每个聚合定向到不同输出。  
  
 可以对单个输入列应用多个聚合。 例如，如果需要名为 **Sales** 的输入列的和值与平均值，则可以配置转换，使其将 Sum 和 Average 函数都应用于 **Sales** 列。  
  
 聚合转换具有一个输入和一个或多个输出。 它不支持错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](../set-the-properties-of-a-data-flow-component.md)  
  
-   [转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 有关如何设置属性的详细信息，请单击下列主题之一：  
  
-   [使用聚合转换来聚合数据集中的值](../../../integration-services/data-flow/transformations/aggregate-values-in-a-dataset-by-using-the-aggregate-transformation.md)  
  
-   [设置数据流组件的属性](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
-   [为合并转换和合并联接转换排序数据](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 [使用聚合转换来聚合数据集中的值](../../../integration-services/data-flow/transformations/aggregate-values-in-a-dataset-by-using-the-aggregate-transformation.md)  
  
## <a name="aggregate-transformation-editor-aggregations-tab"></a>聚合转换编辑器（“聚合”选项卡）
  可以使用 **“聚合转换编辑器”** 对话框的 **“聚合”** 选项卡指定聚合的列以及聚合属性。 可以应用多个聚合。 此转换不生成错误输出。  
  
> [!NOTE]  
>  对于键计数、键范围、非重复键计数和非重复键范围的选项来说，如果是在“高级”选项卡中指定的，则会应用于组件级；如果是在“聚合”选项卡的高级显示部分中指定的，则会应用于输出级；而如果是在“聚合”选项卡底部的列列表中指定的，则会应用于列级。  
>   
>  在聚合转换中， **“键”** 和 **“键范围”** 是指期望 **“分组依据”** 操作产生的组数。 **“非重复键计数”** 和 **“非重复键数范围”** 是指期望 **“非重复计数”** 操作产生的非重复值的数量。  
  
### <a name="options"></a>选项  
 **高级/基本**  
 显示或隐藏为多个输出配置多个聚合的选项。 默认情况下，隐藏“高级”选项。  
  
 **聚合名**  
 在“高级”显示中，键入聚合的友好名称。  
  
 **按列分组**  
 在“高级”显示中，通过使用下面描述的“可用输入列”列表，选择用于分组的列。  
  
 **键范围**  
 在“高级”显示中，根据需要指定聚合可写入的键的大致数目。 默认情况下，此选项的值为 **“未指定”**。 如果同时设置了 **“键范围”** 和 **“键”** 属性，则 **“键”** 的值优先。  
  
|值|说明|  
|-----------|-----------------|  
|未指定|不使用“键范围”属性。|  
|低|聚合可以写入大约 500,000 个键。|  
|中型|聚合可以写入大约 5,000,000 个键。|  
|高|聚合可以写入 25,000,000 个以上的键。|  
  
 **“键”**  
 在“高级”显示中，根据需要指定聚合可写入的键的精确数目。 如果同时指定了 **“键范围”** 和 **“键”** ，则 **“键”** 优先。  
  
 **可用输入列**  
 通过使用此表中的复选框，从可用输入列列表中选择。  
  
 **输入列**  
 从可用输入列的列表中进行选择。  
  
 **输出别名**  
 为每一列键入一个别名。 默认值为输入列的名称；不过，您也可以任选一个唯一的描述性名称。  
  
 **操作**  
 参照下表，从可用操作列表中选择。  
  
|Operation|说明|  
|---------------|-----------------|  
|**GroupBy**|将数据集划分为组。 可以将任何数据类型的列用于分组。 有关详细信息，请参阅 GROUP BY。|  
|**Sum**|对列中的值求和。 只能对数值数据类型的列求和。 有关详细信息，请参阅 SUM。|  
|**平均值**|返回列中值的平均值。 只能对数值数据类型的列求平均值。 有关详细信息，请参阅 AVG。|  
|**Count**|返回组中的项数。 有关详细信息，请参阅 COUNT。|  
|**CountDistinct**|返回组中的唯一非空值的数量。 有关详细信息，请参阅 COUNT 和 DISTINCT。|  
|**最低**|返回组中的最小值。 只限于数值数据类型。|  
|最大值|返回组中的最大值。 只限于数值数据类型。|  
  
 **比较标志**  
 如果选择“分组依据”，请使用复选框来控制转换如何执行比较。 有关字符串比较选项的信息，请参阅 [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md)。  
  
 **Count Distinct Scale**  
 根据需要，可以指定聚合能够写入的非重复值的大致数目。 默认情况下，此选项的值为 **“未指定”**。 如果同时指定 **CountDistinctScale** 和 **CountDistinctKeys** ，则 **CountDistinctKeys** 优先。  
  
|值|说明|  
|-----------|-----------------|  
|未指定|不使用 **CountDistinctScale** 属性。|  
|低|聚合可以写入大约 500,000 个非重复值。|  
|中型|聚合可以写入大约 5,000,000 个非重复值。|  
|高|聚合可以写入 25,000,000 个以上的非重复值。|  
  
 **Count Distinct Keys**  
 根据需要，可以指定聚合能够写入的非重复值的精确数目。 如果同时指定 **CountDistinctScale** 和 **CountDistinctKeys** ，则 **CountDistinctKeys** 优先。  
  
## <a name="aggregate-transformation-editor-advanced-tab"></a>聚合转换编辑器（“高级”选项卡）
  可以使用 **“聚合转换编辑器”** 对话框的 **“高级”** 选项卡设置组件属性，指定聚合以及设置输入和输出列的属性。  
  
> [!NOTE]  
>  对于键计数、键范围、非重复键计数和非重复键范围的选项来说，如果是在“高级”选项卡中指定的，则会应用于组件级；如果是在“聚合”选项卡的高级显示部分中指定的，则会应用于输出级；而如果是在“聚合”选项卡底部的列列表中指定的，则会应用于列级。  
>   
>  在聚合转换中， **“键”** 和 **“键范围”** 是指期望 **“分组依据”** 操作产生的组数。 **“非重复键计数”** 和 **“非重复键数范围”** 是指期望 **“非重复计数”** 操作产生的非重复值的数量。  
  
### <a name="options"></a>选项  
 **“键范围”**  
 根据需要，可以指定聚合所需的键的大致数目。 转换将使用此信息优化其初始缓存大小。 默认情况下，此选项的值为 **“未指定”**。 如果同时指定了 **“键范围”** 和 **“键数”** ，则 **“键数”** 优先。  
  
|值|说明|  
|-----------|-----------------|  
|未指定|不使用 **“键范围”** 属性。|  
|低|聚合可以写入大约 500,000 个键。|  
|中型|聚合可以写入大约 5,000,000 个键。|  
|高|聚合可以写入 25,000,000 个以上的键。|  
  
 **“键数”**  
 根据需要，可以指定聚合所需的键的精确数目。 转换将使用此信息优化其初始缓存大小。 如果同时指定了 **“键范围”** 和 **“键数”** ，则 **“键数”** 优先。  
  
 **“非重复键数范围”**  
 根据需要，可以指定聚合能够写入的非重复值的大致数目。 默认情况下，此选项的值为 **“未指定”**。 如果同时指定了 **“非重复键数范围”** 和 **“非重复键计数”** ，则 **“非重复键计数”** 优先。  
  
|值|说明|  
|-----------|-----------------|  
|未指定|不使用 CountDistinctScale 属性。|  
|低|聚合可以写入大约 500,000 个非重复值。|  
|中型|聚合可以写入大约 5,000,000 个非重复值。|  
|高|聚合可以写入 25,000,000 个以上的非重复值。|  
  
 **“非重复键计数”**  
 根据需要，可以指定聚合能够写入的非重复值的精确数目。 如果同时指定了 **“非重复键数范围”** 和 **“非重复键计数”** ，则 **“非重复键计数”** 优先。  
  
 **自动扩展系数**  
 使用一个 1 到 100 之间的值指定在聚合过程中内存可扩展的百分比。 默认情况下，此选项的值为 **25%**。  
  
## <a name="see-also"></a>另请参阅  
 [数据流](../../../integration-services/data-flow/data-flow.md)   
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
