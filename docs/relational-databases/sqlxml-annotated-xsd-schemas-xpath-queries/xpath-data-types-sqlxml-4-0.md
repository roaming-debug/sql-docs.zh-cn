---
title: SQLXML) 的 XPath 数据类型 (
description: 了解 SQLXML 4.0 中的 XPath 数据类型，以及它们如何与 Microsoft SQL Server 和 XML 架构 (XSD) 数据类型进行比较。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping XDR types to XPath types [SQLXML]
- data types [XPath]
- arithmetic operators
- mapping data types [SQLXML]
- relational operators [SQLXML]
- node-set [SQLXML]
- data types [SQLXML], XPath
- XPath operators [SQLXML]
- XDR data type [SQLXML]
- equality operators [SQLXML]
- XPath conversions [SQLXML]
- converting data types [SQLXML]
- Boolean operators
- XPath queries [SQLXML], data types
- XPath data types [SQLXML]
- operators [SQLXML]
ms.assetid: a90374bf-406f-4384-ba81-59478017db68
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 15b308481b6622284d6f8bdd36d474b3886570d6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460034"
---
# <a name="xpath-data-types-sqlxml-40"></a>XPath 数据类型 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、XPath 和 XML 架构 (XSD) 的数据类型非常不同。 例如，XPath 没有整数或日期数据类型，但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 XSD 则具有许多此类数据类型。 XSD 可将纳秒精度用于时间值，而 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 最高只能使用 1/300 秒的精度。 因此，将一种数据类型映射到另一种数据类型并不是始终可行的。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将数据类型映射到 XSD 数据类型的详细信息，请参阅 [数据类型强制转换和 sql： datatype 批注 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md)。  
  
 XPath 包含三种数据类型： **字符串**、 **数字** 和 **布尔值**。 **Number** 数据类型始终为 IEEE 754 双精度浮点。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Float (53)** 数据类型是最接近的 XPath。  但是， **float (53)** 不完全与 IEEE 754 完全相同。 例如，NaN（非数字）和 infinity 均未使用。 尝试将非数字字符串转换为 **数字** ，并尝试除以零会导致错误。  
  
## <a name="xpath-conversions"></a>XPath 转换  
 在您使用 `OrderDetail[@UnitPrice > "10.0"]` 之类的 XPath 查询时，隐式和显式数据类型转换可能会对查询的意义产生细微的变化。 因此，理解 XPath 数据类型的实现方式十分重要。 有关 XPath 语言规范，XML 路径语言 (XPath) 版本 1.0 W3C 建议的建议8年10月1999，可在 W3C 网站上找到 http://www.w3.org/TR/1999/PR-xpath-19991008.html 。  
  
 XPath 运算符分为四个类别：  
  
-   布尔运算符（and、or）  
  
-   关系运算符 (\<, > ， \<=, > =)   
  
-   相等运算符（=、!=）  
  
-   算术运算符（+、-、*、div、mod）  
  
 每个运算符类别都以不同方式转换其操作数。 XPath 运算符根据需要隐式转换其操作数。 算术运算符将其操作数转换为 **数字**，并生成一个数值。 布尔运算符将其操作数转换为 **布尔** 值，并产生布尔值。 关系运算符和相等运算符产生布尔值。 但是，根据其操作数的原始数据类型，这两种运算符具有不同的转换规则，如下表所示。  
  
|操作数|关系运算符|相等运算符|  
|-------------|-------------------------|-----------------------|  
|这两种操作数均为节点集。|当且仅当一个集中有一个节点，并且第二个集中有一个节点时，使其 **字符串** 值的比较结果为 true 时，为 true。|相同.|  
|一个是节点集，另一个是 **字符串**。|当且仅当节点集中存在某一节点时为 TRUE，这样在转换为 **数字** 时，它与转换为 **数字** 的 **字符串** 的比较结果为 true。|当且仅当节点集中存在某一节点时为 TRUE，这样在转换为 **字符串** 时，它与 **字符串** 的比较结果为 true。|  
|一个是节点集，另一个是 **数字**。|当且仅当节点集中存在某一节点时为 TRUE，这样在转换为 **数字** 时，它与 **数字** 的比较结果为 true。|相同.|  
|一个是节点集，另一个是 **布尔值**。|当且仅当节点集中存在某一节点时为 TRUE，这样在转换为 **布尔值** 后再转换 **为数字时，它** 与转换为 **数字** 的 **布尔值** 的比较结果为 true。|当且仅当节点集中有一个节点时为 TRUE，这样在转换为 **布尔值** 时，它与 **布尔** 值的比较结果为 true。|  
|两者均不是节点集。|将两个操作数都转换为 **数字** ，然后进行比较。|将两个操作数均转换为常见类型，然后进行比较。 如果为 **布尔值**，则转换为 **布尔** 值; 如果为 **number**，则转换为 **数值**;否则，转换为 **字符串**。|  
  
> [!NOTE]  
>  由于 XPath 关系运算符始终将其操作数转换为 **数字**，因此不可能进行 **字符串** 比较。 为了包含日期比较，SQL Server 2000 为 XPath 规范提供此变体：当关系运算符将 **字符串** 与 **字符串**、节点设置 **为字符串或** 将字符串值节点设置为字符串值的节点集时，将对 **字符串** 进行比较， (不会执行 **数字** 比较) 。  
  
## <a name="node-set-conversions"></a>节点集转换  
 节点集转换并非始终都是直观的。 通过只采用集内第一个节点的字符串值，将节点集转换为 **字符串** 。 通过将节点集转换为 **字符串**，然后将 **字符串** 转换为 **数字**，将该节点集转换为 **数字**。 节点集通过测试是否存在将其转换为 **布尔值** 。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不执行针对节点集的位置选择：例如，XPath 查询 `Customer[3]` 意味着第三个客户；但在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中不支持此类型的位置选择。 因此，不会实现 XPath 规范所述的节点集到 **字符串** 或节点集到 **数字** 的转换。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用“任何”语义，而 XPath 规范指定“第一个”语义。 例如，根据 W3C XPath 规范，XPath 查询 `Order[OrderDetail/@UnitPrice > 10.0]` 会选择包含 **单价** 大于10.0 的第一个 **OrderDetail** 的订单。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此 XPath 查询选择具有 **单价** 大于10.0 的所有 **OrderDetail** 的订单。  
  
 转换为 **布尔值** 将生成一个存在测试;因此，XPath 查询 `Products[@Discontinued=true()]` 等效于 sql 表达式 "products. 废止 is not null"，而不是 sql 表达式 "products. 废止 = 1"。 若要使查询等效于后面的 SQL 表达式，请首先将节点集转换为非 **布尔** 类型，如 **number**。 例如，`Products[number(@Discontinued) = true()]`。  
  
 因为如果运算符对于节点集中任一节点为 TRUE，则大多数运算符均定义为 TRUE；所以，在节点集为空时，这些运算的计算结果始终为 FALSE。 因此，如果 A 为空，则 `A = B` 和 `A != B` 均为 FALSE，并且 `not(A=B)` 和 `not(A!=B)` 均为 TRUE。  
  
 通常，如果数据库中的列的值不为 **null**，则映射到该列的属性或元素是存在的。 如果元素的任何子集存在，则映射到行的元素将存在。  
  
> [!NOTE]  
>  带 **批注的元素始终存在** 。 因此，不能在 **非常量** 元素上使用 XPath 谓词。  
  
 将节点集转换为 **字符串** 或 **数字** 时，如果在带批注的架构中检查任何) ，并且该类型用于确定所需的转换，则其 XDR 类型 (。  
  
## <a name="mapping-xdr-data-types-to-xpath-data-types"></a>将 XDR 数据类型映射到 XPath 数据类型  
 节点的 XPath 数据类型从架构中的 XDR 数据类型派生，如下表所示 (节点 **雇员 id** 用于说明用途) 。  
  
|XDR 数据类型|等效<br /><br /> XPath 数据类型|使用的 SQL Server 转换|  
|-------------------|------------------------------------|--------------------------------|  
|Nonebin.base64bin.hex|不可用|NoneEmployeeID|  
|boolean|boolean|CONVERT(bit, EmployeeID)|  
|number、int、float、i1、i2、i4、i8、r4、r8、ui1、ui2、ui4、ui8|number|CONVERT(float(53), EmployeeID)|  
|id、idref、idrefsentity、entities、enumerationnotation、nmtoken、nmtokens、chardate、Timedate、Time.tz、string、uri、uuid|string|CONVERT(nvarchar(4000), EmployeeID, 126)|  
|fixed14.4|无（在 XPath 中没有等效于 fixed14.4 XDR 数据类型的数据类型）|CONVERT(money, EmployeeID)|  
|date|string|LEFT(CONVERT(nvarchar(4000), EmployeeID, 126), 10)|  
|time<br /><br /> time.tz|string|SUBSTRING(CONVERT(nvarchar(4000), EmployeeID, 126), 1 + CHARINDEX(N'T', CONVERT(nvarchar(4000), EmployeeID, 126)), 24)|  
  
 日期和时间转换的设计目的是在数据库中使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **datetime** 数据类型还是 **字符串** 存储该值。 请注意， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **datetime** 数据类型不使用 **时区**，其精度比 XML **time** 数据类型的精度更小。 若要包括 **时区** 数据类型或其他精度，请 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 **字符串** 类型将数据存储在中。  
  
 在某一节点从其 XDR 数据类型转换为 XPath 数据类型时，有时候可能需要执行附加的转换（从一个 XPath 数据类型转换为另一个 XPath 数据类型）。 例如，请考虑下面的 XPath 查询：  
  
```  
(@m + 3) = 4  
```  
  
 如果 @m 是固定的 **14.4** XDR 数据类型，则使用以下内容完成从 XDR 数据类型到 XPath 数据类型的转换：  
  
```  
CONVERT(money, m)  
```  
  
 在此转换中，将节点 `m` 从 **固定的 14.4** 转换为 **money**。 但如果添加值 3，则要求附加的转换：  
  
```  
CONVERT(float(CONVERT(money, m))  
```  
  
 该 XPath 表达式计算为：  
  
```  
CONVERT(float(CONVERT(money, m)) + CONVERT(float(53), 3) = CONVERT(float(53), 3)  
```  
  
 如下表中所示，这一转换同样适用于其他 XPath 表达式（例如文字表达式或复合表达式）。  
  
|   | X 未知 | X 为字符串 | X 为数字 | X 是布尔值 |
| - | ------------ | ----------- | ----------- | ------------ |
| **string(X)** |CONVERT (nvarchar(4000), X, 126)|-|CONVERT (nvarchar(4000), X, 126)|CASE WHEN X THEN N'true' ELSE N'false' END|  
| **number(X)** |CONVERT (float(53), X)|CONVERT (float(53), X)|-|CASE WHEN X THEN 1 ELSE 0 END|  
| **boolean(X)** |-|LEN (X) > 0|X != 0|-|  
  
## <a name="examples"></a>示例  
  
### <a name="a-convert-a-data-type-in-an-xpath-query"></a>A. 在 XPath 查询中转换数据类型  
 在针对带批注的 XSD 架构指定的以下 XPath 查询中，该查询选择 "**雇员 id** " 属性值为 "e-1" 的所有 **雇员** 节点，其中，"E-" 是使用 **sql： id 前缀** 批注指定的前缀。  
  
 `Employee[@EmployeeID="E-1"]`  
  
 该查询中的谓词等效于 SQL 表达式：  
  
 `N'E-' + CONVERT(nvarchar(4000), Employees.EmployeeID, 126) = N'E-1'`  
  
 因为 "**雇员** id" 是 (**idref**、 **idrefs**、 **nmtoken**、 **nmtokens** 等) 数据类型值中的一个 **id** ，所以，可以使用前面所述的转换规则将 **雇员** id 转换为 **字符串** XPath 数据类型。  
  
 `CONVERT(nvarchar(4000), Employees.EmployeeID, 126)`  
  
 “E-”前缀将添加到字符串，然后结果将与 `N'E-1'` 进行比较。  
  
### <a name="b-perform-several-data-type-conversions-in-an-xpath-query"></a>B. 在 XPath 查询中执行若干数据类型转换  
 考虑以下根据带批注的 XSD 架构指定的 XPath 查询：`OrderDetail[@UnitPrice * @OrderQty > 98]`  
  
 此 XPath 查询返回 **\<OrderDetail>** 满足该谓词的所有元素 `@UnitPrice * @OrderQty > 98` 。 如果在带批注的架构中使用 **固定的 14.4** 数据类型对 **单价** 进行批注，则此谓词等效于 SQL 表达式：  
  
 `CONVERT(float(53), CONVERT(money, OrderDetail.UnitPrice)) * CONVERT(float(53), OrderDetail.OrderQty) > CONVERT(float(53), 98)`  
  
 在 XPath 查询中转换值时，第一个转换将 XDR 数据类型转换为 XPath 数据类型。 因为 " **单价** " 的 XSD 数据类型是 **固定的 14.4**，如上表中所述，这是使用的第一个转换：  
  
```  
CONVERT(money, OrderDetail.UnitPrice))   
```  
  
 由于算术运算符将其操作数转换 **为 xpath 数据** 类型，因此第二个转换 (从一个 xpath 数据类型转换为另一个 xpath 数据类型) 应用于将值转换为 **float (53)** (**Float (53)** 接近 XPath **数字** 数据类型) ：  
  
```  
CONVERT(float(53), CONVERT(money, OrderDetail.UnitPrice))   
```  
  
 假设 **orderqty** 特性没有 XSD 数据类型，则在单个转换中将 **orderqty** 转换为 **数字** XPath 数据类型：  
  
```  
CONVERT(float(53), OrderDetail.OrderQty)  
```  
  
 同样，将值98转换 **为 XPath 数据** 类型：  
  
```  
CONVERT(float(53), 98)  
```  
  
> [!NOTE]  
>  如果在该架构中使用的 XSD 数据类型与数据库中的基础 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型不兼容，或者不可能执行 XPath 数据类型转换，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会返回错误。 例如，如果使用 **id 前缀** 批注批注了 "**雇员** id" 属性，则 XPath 将 `Employee[@EmployeeID=1]` 生成一个错误，因为 **雇员** **id 具有 id 前缀** 批注，因此不能转换为 **数字**。  
  
  
