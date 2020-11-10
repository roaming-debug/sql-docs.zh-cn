---
title: Multilookup 函数（报表生成器）| Microsoft Docs
description: Multilookup 函数从报表生成器中包含名称/值对的数据集中返回指定名称集的一组第一个匹配值。
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 1fec079e-33b3-4e4d-92b3-6b4d06a49a77
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b8db1d2a7fe18264c81d7585e02babef65b3346d
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364569"
---
# <a name="report-builder-functions---multilookup-function"></a>报表生成器函数 - Multilookup 函数
  从包含名称/值对的数据集返回指定名称集的一组第一个匹配值。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>语法  
  
```  
  
Multilookup(source_expression, destination_expression, result_expression, dataset)  
```  
  
#### <a name="parameters"></a>参数  
 *source_expression*  
 ( **VariantArray** ) 在当前作用域中计算结果并指定要查找的名称或键的集合的表达式。 例如，对于多值参数， `=Parameters!IDs.value`。  
  
 *destination_expression*  
 ( **Variant** ) 针对数据集中的每行计算结果并指定要匹配的名称或键的表达式。 例如，`=Fields!ID.Value`。  
  
 *result_expression*  
 ( **Variant** ) 针对数据集中的行（其中， *source_expression* = *destination_expression* ）计算结果并指定要检索的值的表达式。 例如，`=Fields!Name.Value`。  
  
 *数据集 (dataset)*  
 指定报表中数据集的名称的常量。 例如，“Colors”。  
  
## <a name="return"></a>返回  
 返回 **VariantArray** ，如果没有匹配项，则返回 **Nothing** 。  
  
## <a name="remarks"></a>备注  
 使用 **Multilookup** 从名称-值对（每对具有 1 对 1 的关系）的数据集中检索一组值。 **MultiLookup** 等同于对一组名称或键调用 **Lookup** 。 例如，对于基于主键标识符的多值参数，可以在表中使用文本框表达式中的 **Multilookup** 来检索未绑定到该参数或该表的数据集中的相关值。  
  
 **Multilookup** 执行下列操作：  
  
-   计算当前作用域中源表达式的结果并生成变体对象的数组。  
  
-   对于数组中的每个对象，调用 [Lookup 函数（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-lookup-function.md)并向返回数组添加结果。  
  
-   返回结果集。  
  
 若要从具有指定名称的名称/值对（具有 1 对 1 的关系）的数据集中检索单个值，请使用 [Lookup 函数（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-lookup-function.md)。 若要从具有某名称的名称-值对（具有 1 对多的关系）的数据集中检索多个值，请使用 [LookupSet 函数（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-lookupset-function.md)。  
  
 存在以下限制：  
  
-   在应用所有筛选表达式后计算 **Multilookup** 的结果  
  
-   只支持一个级别的查找。 源、目标或结果表达式不能包含对查找函数的引用。  
  
-   源和目标表达式必须对同一数据类型计算结果。  
  
-   源、目标和结果表达式不能包含对报表或组变量的引用。  
  
-   **Multilookup** 不能作为以下报表项的表达式：  
  
    -   数据源的动态连接字符串。  
  
    -   数据集中的计算字段。  
  
    -   数据集中的查询参数。  
  
    -   数据集中的筛选器。  
  
    -   报表参数。  
  
    -   Report.Language 属性。  
  
 有关详细信息，请参阅[聚合函数引用（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md)和[总计、聚合和内置集合的表达式作用域（报表生成器和 SSRS）](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)。  
  
## <a name="examples"></a>示例

### <a name="a-use-multilookup-function"></a>A. 使用 MultiLookup 函数
 假定名为“Category”的数据集包含字段 CategoryList，该字段包含类别标识符的逗号分隔列表，例如“2, 4, 2, 1”。  
  
 数据集 CategoryNames 包含类别标识符和类别名称，如下表中所示：  
  
|ID|名称|  
|--------|----------|  
|1|Accessories|  
|2|Bikes|  
|3|Clothing|  
|4|组件|  
  
 若要查找与标识符列表对应的名称，请使用 **Multilookup** 。 必须首先将该列表拆分为字符串数组，调用 **Multilookup** 以检索类别名称，然后将结果连接成字符串。  
  
 将以下表达式放入绑定到 Category 数据集的数据区域中的文本框时，显示“自行车, 组件, 自行车, 附件”：  
  
```  
=Join(MultiLookup(Split(Fields!CategoryList.Value,","),  
   Fields!CategoryID.Value,Fields!CategoryName.Value,"Category")),  
   ", ")  
```  
  
### <a name="b-use-multilookup-with-multivalue-parameter"></a>B. 将 MultiLookup 与多值参数一起使用  
 假定数据集 ProductColors 包含颜色标识符字段 ColorID 和颜色值字段 Color，如下表中所示：  
  
|ColorID|Color|  
|-------------|-----------|  
|1|Red|  
|2|蓝色|  
|3|绿色|  
  
 假定多值参数 *MyColors* 未绑定到数据集以获取可用值。 该参数的默认值设置为 2 和 3。 将以下表达式放入表中的文本框时，将该参数的多个所选值连接为逗号分隔的列表并显示“蓝色, 绿色”。  
  
```  
=Join(MultiLookup(Parameters!MyColors.Value,Fields!ColorID.Value,Fields!Color.Value,"ProductColors"),", ")  
```  
  
## <a name="see-also"></a>另请参阅  
 [在报表中使用表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [表达式示例（报表生成器和 SSRS）](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [表达式中的数据类型（报表生成器和 SSRS）](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [总计、聚合和内置集合的表达式作用域（报表生成器和 SSRS）](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
  
