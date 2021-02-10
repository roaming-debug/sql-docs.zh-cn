---
description: Shape COMPUTE 子句
title: 形状计算子句 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- compute clause [ADO]
- data shaping [ADO], COMPUTE clause
ms.assetid: 3fdfead2-b5ab-4163-9b1d-3d2143a5db8c
author: rothja
ms.author: jroth
ms.openlocfilehash: b246b1e70f18836455008f8d9e3c1b972e71f183
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036997"
---
# <a name="shape-compute-clause"></a>Shape COMPUTE 子句
Shape 计算子句生成父 **记录集**，其列包含对子 **记录集** 的引用;其内容为章节、新列或计算列的可选列，或者对子 **记录** 集或之前形状的 **记录集** 执行聚合函数的结果。可选的 from 子句中列出的子 **记录集中** 的任何列。  
  
## <a name="syntax"></a>语法  
  
```  
SHAPE child-command [AS] child-alias  
   COMPUTE child-alias [[AS] name], [appended-column-list]  
   [BY grp-field-list]  
```  
  
## <a name="description"></a>说明  
 此子句的组成部分如下所示：  
  
 *子命令*  
 包含以下项之一：  
  
-   大括号中的查询命令 ( " {} " ) 返回子 **记录集** 对象。 将向基础数据提供程序发出命令，其语法取决于提供程序的要求。 这通常是 SQL 语言，尽管 ADO 不需要任何特定的查询语言。  
  
-   现有整形 **记录集** 的名称。  
  
-   另一个 shape 命令。  
  
-   TABLE 关键字，后跟数据提供程序中表的名称。  
  
 *子别名*  
 用于引用 *子命令* 返回的 **记录集** 的别名。 COMPUTE 子句的列列表中必须有 *子别名* ，并定义父记录集对象和子 **记录集** 对象之间的关系。  
  
 *追加的列列表*  
 一个列表，其中的每个元素在生成的父项中定义列。 每个元素都包含章节列、新列、计算列或由子 **记录集中** 的聚合函数生成的值。  
  
 *grp*  
 父记录集对象和子 **记录集** 对象中的列的列表，用于指定应如何将行分组到子级中。  
  
 对于 *grp 列表* 中的每个列，子记录集对象和父 **记录集** 对象中都有相应的列。 对于父 **记录集中** 的每一行， *grp* 列都具有唯一值，并且父行引用的子 **记录集** 只包含其 *grp* 列与父行具有相同值的子行。  
  
 如果包括 BY 子句，则将根据 COMPUTE 子句中的列对子 **记录集** 的行进行分组。 父 **记录集** 对于子 **记录集中** 的每个行组将占一行。  
  
 如果省略了 BY 子句，则整个子 **记录集** 将被视为单个组，并且父 **记录集** 将只包含一行。 该行将引用整个子 **记录集**。 省略 BY 子句可计算整个子 **记录集** 的 "总计" 聚合。  
  
 例如：  
  
```  
SHAPE {select * from Orders} AS orders             COMPUTE orders, SUM(orders.OrderAmount) as TotalSales         
```  
  
 不管使用计算或使用追加) 来构成父 **记录集** 的方式如何 (，它将包含一个用于将其与子 **记录集** 相关联的章节列。 如果需要，父 **记录集** 还可以包含列，这些列包含对子行的 SUM、MIN、MAX) 等聚合 (。 父 **记录集和子记录集** 都可能包含列，这些列包含 **记录集中** 行的表达式，还包含新的和初始为空的列。  
  
## <a name="operation"></a>操作  
 向提供程序发送 *子命令* ，后者返回子 **记录集**。  
  
 COMPUTE 子句指定父 **记录集** 的列，可能是对子 **记录集** 的引用、一个或多个聚合、一个计算表达式或新列。 如果有 BY 子句，则它定义的列也会附加到父 **记录集**。 BY 子句指定如何对子 **记录集** 的行进行分组。  
  
 例如，假设有一个名为 "人口统计" 的表，其中包含州、城市和人口字段。 表中的人口 (仅作为) 示例提供。  
  
|状态|City|人口数|  
|-----------|----------|----------------|  
|WA|西雅图|700000|  
|或|Medford|200,000|  
|或|Portland|400,000|  
|CA|Los Angeles|800000|  
|CA|San Diego|600000|  
|WA|Tacoma|500,000|  
|或|Corvallis|300,000|  
  
 现在，请发出以下 shape 命令：  
  
```  
rst.Open  "SHAPE {select * from demographics} AS rs "  & _  
          "COMPUTE rs, SUM(rs.population) BY state", _  
           objConnection  
```  
  
 此命令打开具有两个级别的形状 **记录集** 。 父级别是生成的 **记录集** ，其中聚合列 (`SUM(rs.population)`) 、引用子 **记录集** 的列 (`rs`) 以及用于对子 **记录集** 进行分组的列 (`state`) 。 子级别是查询命令 () 返回的 **记录集** `select * from demographics` 。  
  
 子 **记录集** 详细信息行将按状态分组，而不是以特定顺序分组。 也就是说，组不会按字母顺序或数字顺序排列。 如果希望对父 **记录集** 进行排序，则可以使用 **记录集排序** 方法对父 **记录集** 进行排序。  
  
 你现在可以导航打开的父 **记录集** 并访问子详细信息 **记录集** 对象。 有关详细信息，请参阅 [访问分层记录集中的行](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)。  
  
## <a name="resultant-parent-and-child-detail-recordsets"></a>最终的父和子详细信息记录集  
  
### <a name="parent"></a>Parent  
  
|SUM (rs。人口) |rs|状态|  
|---------------------------|--------|-----------|  
|1300000|对 child1 的引用|CA|  
|1,200,000|对 child2 的引用|WA|  
|1100000|对 child3 的引用|或|  
  
## <a name="child1"></a>Child1  
  
|状态|City|人口数|  
|-----------|----------|----------------|  
|CA|Los Angeles|800000|  
|CA|San Diego|600000|  
  
## <a name="child2"></a>Child2  
  
|状态|City|人口数|  
|-----------|----------|----------------|  
|WA|西雅图|700000|  
|WA|Tacoma|500,000|  
  
## <a name="child3"></a>Child3  
  
|状态|City|人口数|  
|-----------|----------|----------------|  
|或|Medford|200,000|  
|或|Portland|400,000|  
|或|Corvallis|300,000|  
  
## <a name="see-also"></a>另请参阅  
 [访问分层记录集中的行](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)   
 [数据定形概述](../../../ado/guide/data/data-shaping-overview.md)   
 [Field 对象](../../../ado/reference/ado-api/field-object.md)   
 [正式形状语法](../../../ado/guide/data/formal-shape-grammar.md)   
 [ADO)  (Recordset 对象 ](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [数据整理所需的提供程序](../../../ado/guide/data/required-providers-for-data-shaping.md)   
 [Shape APPEND 子句](../../../ado/guide/data/shape-append-clause.md)   
 [整体形状命令](../../../ado/guide/data/shape-commands-in-general.md)   
 [ADO)  (值属性 ](../../../ado/reference/ado-api/value-property-ado.md)   
 [Visual Basic for Applications 函数](../../../ado/guide/data/visual-basic-for-applications-functions.md)
