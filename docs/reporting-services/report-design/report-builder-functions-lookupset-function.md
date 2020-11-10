---
title: LookupSet 函数（报表生成器）| Microsoft Docs
description: 了解 LookupSet 函数，该函数从包含名称或值对的数据集中返回指定名称的一组匹配值。
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 7685acfd-1c8d-420c-993c-903236fbe1ff
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 22d4a311d38d32fad4910960007223edbe2f9b70
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364452"
---
# <a name="report-builder-functions---lookupset-function"></a>报表生成器函数 - LookupSet 函数
  从包含名称/值对的数据集返回指定名称的一组匹配值。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>语法  
  
```  
  
LookupSet(source_expression, destination_expression, result_expression, dataset)  
```  
  
#### <a name="parameters"></a>参数  
 *source_expression*  
 ( **Variant** ) 在当前作用域中计算结果并指定要查找的名称或键的表达式。 例如，`=Fields!ID.Value`。  
  
 *destination_expression*  
 ( **Variant** ) 针对数据集中的每行计算结果并指定要匹配的名称或键的表达式。 例如，`=Fields!CustomerID.Value`。  
  
 *result_expression*  
 ( **Variant** ) 针对数据集中的行（其中， *source_expression* = *destination_expression* ）计算结果并指定要检索的值的表达式。 例如，`=Fields!PhoneNumber.Value`。  
  
 *数据集 (dataset)*  
 指定报表中数据集的名称的常量。 例如，“ContactInformation”。  
  
## <a name="return"></a>返回  
 返回 **VariantArray** ，如果没有匹配项，则返回 **Nothing** 。  
  
## <a name="remarks"></a>备注  
 使用 **LookupSet** 从名称/值对（每对具有 1 对多的关系）的指定数据集中检索一组值。 例如，对于表中的客户标识符，可以使用 **LookupSet** 从未绑定到该数据区域的数据集检索该客户的所有相关电话号码。  
  
 **LookupSet** 执行下列操作：  
  
-   计算当前作用域中源表达式的结果。  
  
-   根据指定数据集的排序规则，在应用筛选器后对指定数据集的每行计算目标表达式的结果。  
  
-   对于源表达式和目标表达式的每个匹配，计算数据集中该行的结果表达式。  
  
-   返回一组结果表达式值。  
  
 若要从具有指定名称的名称/值对（具有 1 对 1 的关系）的数据集中检索单个值，请使用 [Lookup 函数（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-lookup-function.md)。 若要为一组值调用 Lookup，请使用 [Multilookup 函数（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-multilookup-function.md)。  
  
 存在以下限制：  
  
-   在应用所有筛选表达式后计算 **LookupSet** 的结果。  
  
-   只支持一个级别的查找。 源、目标或结果表达式不能包含对查找函数的引用。  
  
-   源和目标表达式必须对同一数据类型计算结果。  
  
-   源、目标和结果表达式不能包含对报表或组变量的引用。  
  
-   **LookupSet** 不能作为以下报表项的表达式：  
  
    -   数据源的动态连接字符串。  
  
    -   数据集中的计算字段。  
  
    -   数据集中的查询参数。  
  
    -   数据集中的筛选器。  
  
    -   报表参数。  
  
    -   Report.Language 属性。  
  
 有关详细信息，请参阅[聚合函数引用（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md)和[总计、聚合和内置集合的表达式作用域（报表生成器和 SSRS）](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)。  
  
## <a name="examples"></a>示例

 在以下示例中，假定将表绑定到包含销售区域标识符 TerritoryGroupID 的数据集。 名为“Stores”的独立数据集包含区域中的所有商店列表以及区域标识符 ID 和商店名称 StoreName。  
  
### <a name="a-use-lookupset"></a>A. 使用 LookupSet  
 在以下表达式中， **LookupSet** 将值 TerritoryGroupID 与名为“Stores”的数据集中每行的 ID 进行比较。 对于每个匹配项，将该行的 StoreName 字段值添加到结果集。  
  
```  
=LookupSet(Fields!TerritoryGroupID.Value, Fields!ID.Value, Fields!StoreName.Value, "Stores")  
```  
  
### <a name="b-use-join-to-create-a-result-list"></a>B. 使用“Join”创建结果列表 
 由于 **LookupSet** 返回对象的集合，因此不能直接在文本框中显示结果表达式。 可以将集合中每个对象的值连接为一个字符串。  
  
 使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 函数 **Join** 可以创建来自一组对象的带分隔符的字符串。 使用逗号作为分隔符来合并一行中的对象。 在某些呈现器中，你可能使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 换行 (`vbCrLF`) 作为分隔符以在新行列出每个值。  
  
 以下表达式用作文本框的 Value 属性时，使用 **Join** 创建列表。  
  
```  
=Join(LookupSet(Fields!TerritoryGroupID.Value, Fields!ID.Value, Fields!StoreName.Value, "Stores"),",")  
```  
  
### <a name="c-add-code-to-generate-html"></a>C. 添加代码以生成 HTML
 对于仅呈现几次的文本框，您可以选择添加自定义代码来生成 HTML 以显示文本框中的值。 文本框中的 HTML 需要特别处理，因此对于呈现数千次的文本框来说不要这样做。  
  
 将以下 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 函数复制到报表定义中的代码块。 **MakeList** 提取在 *result_expression* 中返回的对象数组并使用 HTML 标记生成未排序的列表。 **Length** 返回对象数组中的项数。  
  
```  
Function MakeList(ByVal items As Object()) As String  
   If items Is Nothing Then  
      Return Nothing  
   End If  
  
   Dim builder As System.Text.StringBuilder =   
      New System.Text.StringBuilder()  
   builder.Append("<ul>")  
  
   For Each item As Object In items  
      builder.Append("<li>")  
      builder.Append(item)  
   Next  
   builder.Append("</ul>")  
  
   Return builder.ToString()  
End Function  
  
Function Length(ByVal items as Object()) as Integer  
   If items is Nothing Then  
      Return 0  
   End If  
   Return items.Length  
End Function  
```  
  
### <a name="d-call-the-function"></a>D. 调用该函数
 若要生成 HTML，必须调用该函数。 在文本框的 Value 属性中粘贴以下表达式并将文本的标记类型设置为 HTML。 有关详细信息，请参阅[向报表添加 HTML（报表生成器和 SSRS）](../../reporting-services/report-design/add-html-into-a-report-report-builder-and-ssrs.md)。  
  
```  
=Code.MakeList(LookupSet(Fields!TerritoryGroupID.Value, Fields!ID.Value, Fields!StoreName.Value, "Stores"))  
```  
  
## <a name="see-also"></a>另请参阅  
 [在报表中使用表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [表达式示例（报表生成器和 SSRS）](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [表达式中的数据类型（报表生成器和 SSRS）](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [总计、聚合和内置集合的表达式作用域（报表生成器和 SSRS）](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
  
