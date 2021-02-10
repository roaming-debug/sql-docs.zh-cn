---
description: 使用记录集
title: 使用记录集 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset object [ADO]
ms.assetid: bdf9a56a-de4a-44de-9111-2f11ab7b16ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e64608adc461534a30630a6b8b8d29d0478544a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032249"
---
# <a name="working-with-recordsets"></a>使用记录集
**Recordset** 对象提供内置功能，使你可以重新排列结果集中数据的顺序，以根据你提供的条件搜索特定记录，甚至还可以使用索引来优化这些搜索操作。 这些功能是否可用于使用取决于提供程序和在某些情况下（例如， [索引](../../../ado/reference/ado-api/index-property.md) 属性的结构）的数据源本身的结构。  
  
## <a name="arranging-data"></a>排列数据  
 通常，对 **记录集中** 的数据进行排序的最有效方法是在用于向其返回结果的 SQL 命令中指定 order by 子句。 但是，您可能需要更改已创建的 **记录集中** 数据的顺序。 您可以使用 **Sort** 属性来确定 **记录集** 的行的遍历顺序。 此外，" **筛选器** " 属性确定遍历行时可以访问的行。  
  
 **Sort** 属性设置或返回一个 **字符串** 值，该值指示作为排序依据的 **记录集** 中的字段名称。 每个名称都由逗号分隔，后面可以跟一个空格，关键字 **ASC** (按升序对字段进行排序) 或 **DESC** (按降序对字段进行排序) 。 默认情况下，如果未指定关键字，则字段将按升序排序。  
  
 排序操作非常高效，因为数据不会进行物理重新排列，而是按索引指定的顺序进行访问。  
  
 **Sort** 属性需要将 [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md)属性设置为 **adUseClient**。 如果索引不存在，则将为 **Sort** 属性中指定的每个字段创建临时索引。  
  
 如果将 **Sort** 属性设置为空字符串，则会将行重置为其原始顺序并删除临时索引。 不会删除现有索引。  
  
 假设 **记录集** 包含三个名为 " *firstName*"、" *middleInitial*" 和 " *lastName*" 的字段。 将 **Sort** 属性设置为字符串 " `lastName DESC, firstName ASC` "，这会按姓氏以降序对 **记录集** 进行排序，然后按名字对其进行升序排序。 忽略中间的初始。  
  
 排序条件字符串中引用的任何字段都不能命名为 "ASC" 或 "DESC"，因为这些名称与关键字 **ASC** 和 **DESC** 冲突。 通过在返回 **记录集** 的查询中使用 **AS** 关键字，为具有冲突名称的字段提供别名。  
  
 有关 **记录集** 筛选的详细信息，请参阅本主题后面的 "筛选结果"。  
  
## <a name="finding-a-specific-record"></a>查找特定记录  
 ADO 提供 [查找](../../../ado/reference/ado-api/find-method-ado.md)[和查找](../../../ado/reference/ado-api/seek-method.md)方法来查找 **记录集中** 的特定记录。 **Find** 方法受各种提供程序支持，但仅限于单个搜索条件。 **Seek** 方法支持对多个条件进行搜索，但许多提供程序都不支持。  
  
 字段的索引可以极大地提高 **Find** 方法的性能，并对 **记录集** 对象的属性 **进行排序** 和 **筛选**。 可以通过设置字段对象的动态 [Optimize](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md)属性来为该 **字段** 创建内部索引。 当你将 [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md)属性设置为 **adUseClient** 时，此动态属性将添加到 **Field** 对象的 **Properties** 集合。 请记住，此索引是 ADO 内部的，不能对其进行访问，也不能将它用于任何其他目的。 此外，此索引与 **Recordset** 对象的 [index](../../../ado/reference/ado-api/index-property.md)属性不同。  
  
 **Find** 方法快速查找 (字段) **记录集** 的列中的值。 您可以通过使用 **Optimize** 属性在列上创建索引，从而提高 **Find** 方法的速度。  
  
 **Find** 方法将搜索限制为一个字段的内容。 **Seek** 方法要求您具有索引，并且还具有其他限制。 如果必须对不是索引基础的多个字段进行搜索，或者，如果您的提供程序不支持索引，则可以使用 **Recordset** 对象的 **Filter** 属性来限制结果。  
  
### <a name="find"></a>查找  
 **Find** 方法在 **记录集中** 搜索满足指定条件的行。 或者，可以指定搜索方向、起始行和起始行的偏移量。 如果满足条件，则在找到的记录上设置当前行位置;否则，根据搜索方向，将位置设置为 (或) **记录集** 的结束时间。  
  
 只能为条件指定单个列名。 换言之，此方法不支持多列搜索。  
  
 条件的比较运算符可以是 " **>** " (大于) ，"* * \<**" (less than), "=" (equal), "> =" (大于或等于) ，"<=" (小于或等于) ，"<>" (不等于) ，或 "LIKE" (模式匹配) 。  
  
 条件值可以是字符串、浮点数字或日期。 字符串值用单引号或 "#" 分隔 (数字符号) 标记， (例如，"state =" WA "" 或 "state = #WA #" ) 。 日期值用 "#" 分隔 (数字符号) 标记， (例如，"start_date > #7/22/97 #" ) 。  
  
 如果比较运算符为 "like"，则字符串值可以包含星号 ( * ) 查找任意字符或子字符串的一个或多个匹配项。 例如，"Maine" 与 " \* " 匹配。 你还可以使用前导和尾随星号查找值中包含的子字符串。 例如，"状态" 与 " \* as \* " "匹配阿拉斯加，阿肯色，和马萨诸塞州。  
  
 星号只能在条件字符串的末尾或同时用于条件字符串的开头和结尾，如前文所述。 不能将星号用作前导通配符 ( "* str" ) 或嵌入的通配符 ( 的 \* r ") 。 这将导致错误。  
  
### <a name="seek-and-index"></a>查找和索引  
 如果基础提供程序支持 **Recordset** 对象上的索引，则将 **Seek** 方法与 **Index** 属性一起使用。 使用 [支持](../../../ado/reference/ado-api/supports-method.md)**(adSeek)** 方法来确定基础提供程序是否支持 **查找**，并 **支持 (a)** 方法来确定提供程序是否支持索引。  (例如， [Microsoft Jet 的 OLE DB 提供程序](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-jet.md) 支持 **查找** 和 **索引**。 )   
  
 如果查找找不到所需的行 **，则不** 会出现错误，并且行位于 **记录集** 的末尾。 执行此方法之前，请将 **index** 属性设置为所需的索引。  
  
 只有服务器端游标才支持此方法。 当 **记录集** 对象的 [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md)属性值为 **adUseClient** 时，不支持查找。  
  
 仅当使用 [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md)值 **adCmdTableDirect** 打开 **Recordset** 对象时，才能使用此方法。  
  
## <a name="filtering-the-results"></a>筛选结果  
 **Find** 方法将搜索限制为一个字段的内容。 **Seek** 方法要求您具有索引，并且还具有其他限制。 如果您必须在多个字段中搜索非索引，或者如果您的提供程序不支持索引，则可以使用 **Recordset** 对象的 **Filter** 属性来限制结果。  
  
 使用 **Filter** 属性可以有选择地将记录 **集** 对象中的记录显示为屏幕。 筛选后的 **记录集** 将成为当前游标，这意味着在删除 **筛选器** 之前，不满足 **筛选** 条件的记录将不会出现在 **记录集中**。 基于当前游标返回值的其他属性会受到影响，如 **AbsolutePosition**、 **AbsolutePage**、 **RecordCount** 和 **PageCount**。 这是因为，将 " **筛选器** " 属性设置为特定值会将当前记录移动到满足新值的第一个记录。  
  
 **Filter** 属性采用变量参数。 此值表示以下三种方法之一：使用 **筛选器** 属性：条件字符串、 **FilterGroupEnum** 常量或书签数组。 有关详细信息，请参阅本主题后面的使用条件字符串进行筛选、使用常量进行筛选和使用书签进行筛选。  
  
> [!NOTE]
>  如果知道要选择的数据，则打开包含 SQL 语句的 **记录集** 通常会有效地对结果集进行筛选，而不是依赖于 **筛选器** 属性。  
  
 若要从 **记录集中** 删除筛选器，请使用 **adFilterNone** 常量。 将 **Filter** 属性设置为长度为零的字符串 ( "" ) 与使用 **adFilterNone** 常量的效果相同。  
  
### <a name="filtering-with-a-criteria-string"></a>使用条件字符串进行筛选  
 条件字符串由 *FieldName 运算符值* 格式的子句组成 (例如 `"LastName = 'Smith'"`) 。 可以通过将单个子句与 **and** (（例如， `"LastName = 'Smith' AND FirstName = 'John'"`) 和 **或** (例如) ）来创建复合子句 `"LastName = 'Smith' OR LastName = 'Jones'"` 。 对于条件字符串，请使用以下准则：  
  
-   *FieldName* 必须是 **记录集中** 的有效字段名称。 如果字段名称包含空格，则必须用方括号将该名称括起来。  
  
-   *运算符* 必须是下列其中一项： **\<**, **>** 、 **\<=**, **>=** 、 **<>** 、 **=** 或。   
  
-   *值* 是用于比较字段值 (例如，、、 `'Smith'` `#8/24/95#` `12.345` 或) 的值 `$50.00` 。 使用单引号将 ( ") 字符串和井号， (`#`) 包含日期。 对于数字，可以使用小数点、美元符号和科学记数法。 如果 *运算符***类似**，则 *值* 可以使用通配符。 只 \* 允许使用星号 () 和百分号 (% ) 通配符，并且它们必须是字符串中的最后一个字符。 *值* 不能为 null。  
  
    > [!NOTE]
    >  若要在筛选器 *值* 中包含单引号 ( ") ，请使用两个单引号表示一个引号。 例如，若要筛选 *O'Malley*，则条件字符串应为 `"col1 = 'O''Malley'"` 。 若要在筛选器值的开头和末尾包含单引号，请将字符串用井号括起来 ( # ) 。 例如，若要筛选 *"1"*，则条件字符串应为 `"col1 = #'1'#"` 。  
  
 **And** 和 **OR** 之间没有优先级。 子句可以在括号内分组。 但是，不能对通过 **或** 联接的子句进行分组，然后将该组与 and 联接到另一个子句，如下所示。  
  
```  
(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'  
```  
  
 相反，可以按如下所示构造此筛选器。  
  
```  
(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')  
```  
  
 在 **LIKE** 子句中，可以在模式的开头和结尾使用通配符 (例如， `LastName Like '*mit*'`) ，或仅在模式末尾使用通配符 (例如 `LastName Like 'Smit*'`) 。  
  
### <a name="filtering-with-a-constant"></a>使用常量进行筛选  
 以下常量可用于筛选 **记录集**。  
  
|返回的常量|说明|  
|--------------|-----------------|  
|**adFilterAffectedRecords**|用于仅查看受上次 **删除**、重新 **同步**、 **UpdateBatch** 或 **CancelBatch** 调用影响的记录的筛选器。|  
|**adFilterConflictingRecords**|用于查看上次批处理更新失败的记录的筛选器。|  
|**adFilterFetchedRecords**|用于查看当前缓存中的记录的筛选器，即，最后一次调用的结果是从数据库中检索记录。|  
|**adFilterNone**|删除当前筛选器并还原所有记录以供查看。|  
|**adFilterPendingRecords**|用于仅查看已更改但尚未发送到服务器的记录的筛选器。 仅适用于批处理更新模式。|  
  
 例如，使用筛选器常量可以在批处理更新模式下更轻松地解决单个记录冲突，只允许查看在上一次 **UpdateBatch** 方法调用期间受影响的记录，如下面的示例中所示。  
  
 `Attribute VB_Name = "modExaminingData"`  
  
### <a name="filtering-with-bookmarks"></a>用书签进行筛选  
 最后，您可以将一个书签的可变数组传递到 " **筛选器** " 属性。 生成的游标将只包含书签传递到属性的那些记录。 下面的代码示例从 " *ProductName* " 字段中包含 "B" 的记录 **集中** 的记录创建书签的数组。 然后，它将数组传递给 **Filter** 属性，并显示有关生成的筛选 **记录集** 的信息。  
  
```  
'BeginFilterBkmk  
Dim vBkmkArray() As Variant  
Dim i As Integer  
  
'Recordset created using "SELECT * FROM Products" as command.  
'So, we will check to see if ProductName has a capital B, and  
'if so, add to the array.  
i = 0  
Do While Not objRs.EOF  
    If InStr(1, objRs("ProductName"), "B") Then  
        ReDim Preserve vBkmkArray(i)  
        vBkmkArray(i) = objRs.Bookmark  
        i = i + 1  
        Debug.Print objRs("ProductName")  
    End If  
    objRs.MoveNext  
Loop  
  
'Filter using the array of bookmarks.  
objRs.Filter = vBkmkArray  
  
objRs.MoveFirst  
Do While Not objRs.EOF  
    Debug.Print objRs("ProductName")  
    objRs.MoveNext  
Loop  
'EndFilterBkmk  
```  
  
## <a name="creating-a-clone-of-a-recordset"></a>创建记录集的克隆  
 使用 **Clone** 方法可创建多个重复的 **记录集** 对象，尤其是在您想要在一组给定的记录中维护多个当前记录时。 使用 **Clone** 方法比使用与原始方法相同的定义创建和打开新的 **记录集** 对象更有效。  
  
 新创建的克隆的当前记录最初设置为第一条记录。 克隆的 **记录集中** 的当前记录指针不会与原始记录集同步，反之亦然。 您可以在每个 **记录集中** 单独导航。  
  
 对一个 **记录集** 对象所做的更改在其所有克隆中都可见，而不考虑游标类型。 但是，在对原始记录集执行 [Requery](../../../ado/reference/ado-api/requery-method.md) 后，克隆将不再同步到原始 **记录集**。  
  
 关闭原始 **记录集** 不会关闭其副本，也不会关闭副本来关闭原始或任何其他副本。  
  
 仅当 **记录集** 对象支持书签时，才能对其进行克隆。 书签值可互换;也就是说，一个 **记录集** 对象的书签引用会引用其任何克隆中的同一记录。
