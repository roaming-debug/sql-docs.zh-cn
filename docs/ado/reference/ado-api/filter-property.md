---
description: Filter 属性
title: Filter 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Filter
helpviewer_keywords:
- Filter property
ms.assetid: 80263a7a-5d21-45d1-84fc-34b7a9be4c22
author: rothja
ms.author: jroth
ms.openlocfilehash: 7366ba67d36049f1c5df701bc633d76f56bb9af5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167331"
---
# <a name="filter-property"></a>Filter 属性
指示 [记录集中](./recordset-object-ado.md)数据的筛选器。  
  
## <a name="settings-and-return-values"></a>设置和返回值

设置或返回一个 **变量** 值，该值可以包含以下项之一：  
  
-   **条件字符串：** 由一个或多个单独子句组成的字符串，这些子句与 **AND** 或 **or** 运算符相连接。  
  
-   **书签的数组：** 指向 **记录集** 对象中的记录的唯一书签值的数组。  
  
-   [FilterGroupEnum](./filtergroupenum.md)值。  
  
## <a name="remarks"></a>备注

使用 **Filter** 属性可以有选择地将记录 **集** 对象中的记录显示为屏幕。 筛选后的 **记录集** 将成为当前游标。 基于当前 **游标** 返回值的其他属性会受到影响，如 [ABSOLUTEPOSITION 属性 (ado)](./absoluteposition-property-ado.md)、 [AbsolutePage 属性 (ado)](./absolutepage-property-ado.md)、 [RecordCount 属性 (ado)](./recordcount-property-ado.md)和 [PageCount 属性 (ado)](./pagecount-property-ado.md)。 将 " **筛选器** " 属性设置为特定的新值会将当前记录移动到满足新值的第一条记录。
  
条件字符串由 " *FieldName-Operator-值* " 格式的子句组成 (例如， `"LastName = 'Smith'"`) 。 您可以通过将单个子句与 **AND** (（例如， `"LastName = 'Smith' AND FirstName = 'John'"`) 或 **或** (例如) ）来创建复合子句 `"LastName = 'Smith' OR LastName = 'Jones'"` 。 对于条件字符串，请使用以下准则：

-   *FieldName* 必须是 **记录集中** 的有效字段名称。 如果字段名称包含空格，则必须用方括号将该名称括起来。  
  
-   运算符必须是下列其中一项： \<, > 、 \<=, > =、 <>、= 或 **LIKE**。  
  
-   值是将字段值与之进行比较的值 (例如，"Smith"、#8/24/95 #、12.345 或 $50.00) 。 使用带有字符串的单引号和井号 ( # ) 日期。 对于数字，可以使用小数点、美元符号和科学记数法。 如果运算符 **类似**，则值可以使用通配符。 只允许使用星号 ( * ) 和百分号 (% ) 通配符，它们必须是字符串中的最后一个字符。 值不能为空。  
  
> [!NOTE]
>  若要在筛选器值中包含单引号 ( ") ，请使用两个单引号表示一个引号。 例如，若要筛选 O'Malley，则条件字符串应为 `"col1 = 'O''Malley'"` 。 若要在筛选值的开头和末尾包含单引号，请用井号将该字符串括 ( # ) 。 例如，若要筛选 "1"，则条件字符串应为 `"col1 = #'1'#"` 。  
  
-   AND 和 OR 之间没有优先级。 子句可以在括号内分组。 但是，不能对通过或联接的子句进行分组，然后使用和将该组联接到另一个子句，如以下代码片段所示：  
 `(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'`  
  
-   而是将此筛选器构造为  
 `(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')`  
  
-   在 **LIKE** 子句中，可以在模式的开头和结尾使用通配符。 例如，你可以使用 `LastName Like '*mit*'`。 或者使用 **LIKE** ，只能在模式的末尾使用通配符。 例如，`LastName Like 'Smit*'`。  
  
 例如，使用筛选器常量可以在批处理更新模式下更轻松地解决单个记录冲突，只允许查看在上一次 [UpdateBatch 方法](./updatebatch-method.md) 方法调用期间受影响的记录。  
  
由于与基础数据发生冲突，设置 **Filter** 属性本身可能会失败。 例如，如果另一个用户已删除记录，则可能发生此失败。 在这种情况下，提供程序会将警告 [ (ADO) ](./errors-collection-ado.md) 收集返回到错误集合，但不会暂停程序执行。 仅当所有请求的记录存在冲突时，才会发生运行时错误。 使用 [Status 属性 (ADO Recordset) ](./status-property-ado-recordset.md) 属性可查找存在冲突的记录。  
  
将 **Filter** 属性设置为长度为零的字符串 ( "" ) 与使用 **adFilterNone** 常量的效果相同。
  
只要设置了 **Filter** 属性，当前记录位置就会移动到记录 **集** 的筛选部分记录中的第一条记录。 同样，在清除 " **筛选器** " 属性时，当前记录位置会移动到 **记录集中** 的第一条记录。

假设根据某种类型的变量（如 sql_variant 类型）筛选 **记录集** 。 如果在条件字符串中使用的字段和筛选器值的子类型不匹配，则会出现 (DISP_E_TYPEMISMATCH 或 80020005) 错误。 例如，假设：

-  (rs) 的 **记录集** 对象包含 sql_variant 类型 (C) 的列。
- 并且已为此列的一个字段分配了 I4 类型的值1。 条件字符串在字段中设置为 `rs.Filter = "C='A'"` 。

此配置在运行时产生错误。 但是， `rs.Filter = "C=2"` 对同一字段应用时将不会产生任何错误。 并筛选出当前记录集的字段。

有关书签值的说明，请参阅 [Bookmark 属性 (ADO) ](./bookmark-property-ado.md) 属性，可在其中生成用于筛选器属性的数组。

只有条件字符串形式的筛选器会影响持久 **记录集** 的内容。 条件字符串的一个示例是 `OrderDate > '12/31/1999'` 。 使用书签数组或 **FilterGroupEnum** 中的值创建的筛选器不会影响持久 **记录集** 的内容。 这些规则适用于使用客户端或服务器端游标创建的记录集。
  
> [!NOTE]
>  如果将 adFilterPendingRecords 标志应用于批处理更新模式中已筛选和已修改的 **记录集** ，则结果 **集** 将为空（如果筛选基于单键控表的键字段，并且已对键字段值进行修改）。 如果以下语句之一为 true，则生成的 **记录集** 将为非空：  
  
-   筛选基于单键控表中的非键字段。  
  
-   筛选基于多密钥表中的任何字段。  
  
-   对单键控表中的非键字段进行了修改。  
  
-   对多键控表中的任何字段进行了修改。  
  
下表总结了 **adFilterPendingRecords** 对不同筛选和修改组合的影响。 左侧列显示可能的修改。 可以对任何非键控字段、单键控表中的键字段或多键表中的任何键字段进行修改。 顶部行显示了筛选条件。 筛选可以基于任何非键控字段、单键控表中的键字段或多键表中的任何键字段。 交叉单元显示结果。 **+** 加号表示应用 **adFilterPendingRecords** 会导致非空 **记录集**。 **-** 负号表示空 **记录集**。  
  
|各种|非键|单个键|多个键|
|-|--------------|----------------|-------------------|
|**非键**|+|+|+|
|**单个键**|+|-|不适用|
|**多个键**|+|不适用|+|
|||||
  
## <a name="applies-to"></a>应用于

[记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅

[ (VB) ](./filter-and-recordcount-properties-example-vb.md) 
 的 Filter 和 RecordCount 属性示例[ (VC + +) 的 Filter 和 RecordCount 属性示例](./filter-and-recordcount-properties-example-vc.md) 
[ADO) ](./clear-method-ado.md) 
 (Clear 方法[优化 Property-Dynamic (ADO) ](./optimize-property-dynamic-ado.md)