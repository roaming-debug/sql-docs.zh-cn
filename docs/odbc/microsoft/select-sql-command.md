---
description: SELECT - SQL 命令
title: SELECT-SQL 命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- select [ODBC]
ms.assetid: 2149c3ca-3a71-446d-8d53-3d056e2f301a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fc6ae552bc4e6e8bd681aa3d47ffadeaf81fd0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99153513"
---
# <a name="select---sql-command"></a>SELECT - SQL 命令
检索一个或多个表中的数据。  
  
 Visual FoxPro ODBC 驱动程序支持此命令的本机 Visual FoxPro 语言语法。 有关特定于驱动程序的信息，请参阅 **驱动程序备注**。  
  
## <a name="syntax"></a>语法  
  
```  
  
SELECT [ALL | DISTINCT]  
   [Alias.] Select_Item [AS Column_Name]  
   [, [Alias.] Select_Item [AS Column_Name] ...]   
FROM [DatabaseName!]Table [Local_Alias]  
   [, [DatabaseName!]Table [Local_Alias] ...]   
[WHERE JoinCondition [AND JoinCondition  
...]  
   [AND | OR FilterCondition [AND | OR FilterCondition ...]]]  
[GROUP BY GroupColumn [, GroupColumn ...]]  
[HAVING FilterCondition]  
[UNION [ALL] SELECTCommand]  
[ORDER BY Order_Item [ASC | DESC] [, Order_Item [ASC | DESC] ...]]  
```  
  
## <a name="arguments"></a>参数  
  
> [!NOTE]  
>  在下面的参数中，引用的 *子查询* 是 select 内的 select，必须用括号括起来。 在同一级别最多可以有两个不嵌套) 在 WHERE 子句中的子查询 (。  (参见参数的部分。 ) 子查询可以包含多个联接条件。  
  
 [ALL &#124; DISTINCT]  [*Alias*.] *Select_Item* [AS *Column_Name*] [，[*Alias*.] *Select_Item* [AS *Column_Name*] ...]  
 SELECT 子句指定查询结果中显示的字段、常量和表达式。  
  
 默认情况下，所有行都显示查询结果中的所有行。  
  
 DISTINCT 从查询结果中排除任何行的重复项。  
  
> [!NOTE]  
>  每个 SELECT 子句只能使用一次 DISTINCT。  
  
 *别名*。 限定项名称匹配。 用 *Select_Item* 指定的每一项都会生成一个查询结果列。 如果两个或多个项具有相同的名称，请在项名称之前包含表别名和一个句点，以防止重复列。  
  
 *Select_Item* 指定要包含在查询结果中的项。 项可以是以下项之一：  
  
-   FROM 子句中的表的字段名称。  
  
-   一个常数，它指定相同的常量值显示在查询结果的每一行中。  
  
-   可以是用户定义函数的名称的表达式。  
  
 **带 SELECT 的用户定义函数**  
  
 尽管在 SELECT 子句中使用用户定义的函数具有明显的优势，但您还应考虑以下限制：  
  
-   使用 SELECT 执行的操作的速度可能受此类用户定义函数的执行速度的限制。 使用以 C 或汇编语言编写的 API 和用户定义的函数，涉及用户定义函数的大容量操作可能更好。  
  
-   将值传递给从 SELECT 调用的用户定义函数的唯一可靠方法是调用时传递给函数的自变量列表。  
  
-   即使您试验并发现在某个版本的 FoxPro 中可正常运行的已被认为禁止的操作，也无法保证它将继续在更高版本中使用。  
  
 除了这些限制之外，用户定义函数在 SELECT 子句中是可接受的。 但请记住，使用 SELECT 可能会降低性能。  
  
 以下字段函数可用于作为字段或涉及字段的表达式的 select 项：  
  
-   平均 (*Select_Item*) -数值数据列的平均值。  
  
-   计数 (*Select_Item*) -计算列中选定项的数目。 计数 ( * ) 计算查询输出中的行数。  
  
-   最小 (*Select_Item*) -确定列中 *Select_Item* 的最小值。  
  
-   ) 的最大 (*Select_Item* 确定列中 *Select_Item* 的最大值。  
  
-   SUM (*Select_Item*) -总计数值数据的列。  
  
 不能嵌套字段函数。  
  
 作为 *Column_Name*  
 指定查询输出中列的标题。 此方法在以下情况下很有用： *Select_Item* 是表达式或包含字段函数，并且你想要为该列指定一个有意义的名称。 *Column_Name* 可以是表达式，但不能包含字符 (例如，空格) 表字段名称中不允许使用的字符。  
  
 从 [*DatabaseName*！]*Table* [*Local_Alias*] [，[*DatabaseName*！]*Table* [*Local_Alias*] ...]  
 列出包含查询所检索的数据的表。 如果未打开任何表，Visual FoxPro 将显示 " **打开** " 对话框，以便您可以指定文件位置。 在查询完成后，表将保持打开状态。  
  
 *DatabaseName*！ 指定数据库的名称，而不是使用数据源指定的数据库的名称。 如果数据库未指定数据源，则必须包含该表的数据库名称。 将感叹号 (！ ) 分隔符添加到数据库名称之后、表名称之前。  
  
 *Local_Alias* 指定 *表* 中名为的表的临时名称。 如果指定本地别名，则必须在整个 SELECT 语句中使用本地别名，而不是表名称。 本地别名不会影响 Visual FoxPro 环境。  
  
 WHERE *JoinCondition* [AND *JoinCondition* ...]   [AND &#124; 或 *FilterCondition* [AND &#124; or *FilterCondition* ]]  
 告诉视觉 FoxPro 仅在查询结果中包含某些记录。 从多个表中检索数据所需的位置。  
  
 *JoinCondition* 指定链接 FROM 子句中的表的字段。 如果在查询中包含多个表，则应在第一个表之后指定每个表的联接条件。  
  
> [!IMPORTANT]  
>  创建联接条件时，请考虑以下信息：  
  
-   如果在查询中包含两个表，但未指定联接条件，则只要满足筛选条件，则第一个表中的每个记录都将联接到第二个表中的每个记录。 这样的查询可能会产生冗长的结果。  
  
-   联接带有空字段的表时要格外小心，因为 Visual FoxPro 匹配空字段。 例如，如果你联接 CUSTOMER.ZIP 和 INVOICE.ZIP，并且客户包含100空邮政编码，而发票包含400空邮政编码，则查询输出包含从空字段产生的40000个额外记录。 使用 **空的 ( )** 函数可从查询输出中消除空记录。  
  
-   必须使用 AND 运算符来连接多个联接条件。 每个联接条件都具有以下形式：  
  
     *FieldName1 比较 FieldName2*  
  
     *FieldName1* 是来自一个表的字段的名称， *FieldName2* 是另一个表中的字段的名称，而 " *比较* " 是下表中所述的运算符之一。  
  
|运算符|比较|  
|--------------|----------------|  
|=|等于|  
|==|完全相等|  
|LIKE|SQL LIKE|  
|<>，！ =，#|不等于|  
|>|超过|  
|>=|大于或等于|  
|<|小于|  
|<=|小于或等于|  
  
 将 = 运算符用于字符串时，它的行为方式不同，具体取决于 SET ANSI 的设置。 设置 ANSI 设置为 OFF 时，Visual FoxPro 以 Xbase 用户熟悉的方式处理字符串比较。 设置 ANSI 设置为 ON 时，Visual FoxPro 将遵循 ANSI 标准来比较字符串。 有关 Visual FoxPro 执行字符串比较的方式的详细信息，请参阅 [SET ANSI](../../odbc/microsoft/set-ansi-command.md) 和 [set EXACT](../../odbc/microsoft/set-exact-command.md) 。  
  
 *FilterCondition* 指定要在查询结果中包括的记录必须满足的条件。 您可以根据需要在查询中包含任意多个筛选条件，并使用 AND 或 OR 运算符将它们连接起来。 您也可以使用 NOT 运算符来反转逻辑表达式的值，也可以使用 **空的 ( )** 检查是否有空字段。 在以下示例中， *FilterCondition* 可以采用任何形式：  
  
 **示例 1** *FieldName1 比较 FieldName2*  
  
 `customer.cust_id = orders.cust_id`  
  
 **示例 2** *FieldName 比较表达式*  
  
 `payments.amount >= 1000`  
  
 **示例 3** *FieldName 比较* 所有 (*子查询*)   
  
 `company < ALL ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 当筛选条件包括 "全部" 时，该字段必须满足由子查询生成的所有值的比较条件，然后才能将其记录包含在查询结果中。  
  
 **示例 4** *FieldName 比较* &#124; (*子查询*)   
  
 `company < ANY ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 当筛选条件包含 ANY 或 SOME 时，该字段必须满足子查询生成的值中至少一个值的比较条件。  
  
 下面的示例将检查该字段中的值是否在指定的值范围内：  
  
 **示例 5** *FieldName* [NOT] *Start_Range* 和 *End_Range*  
  
 `customer.postalcode BETWEEN 90000 AND 99999`  
  
 下面的示例检查是否至少有一行符合子查询中的条件。 如果筛选条件包含 EXISTS，筛选条件的计算结果为 True ( ) ，除非子查询的计算结果为空集。  
  
 **示例 6** [NOT] (*子查询*)   
  
 `EXISTS ;`  
  
 `(SELECT * FROM orders WHERE customer.postalcode =`  
  
 `orders.postalcode)`  
  
 **示例 7** *FieldName* [NOT] IN *Value_Set*  
  
 `customer.postalcode NOT IN ("98052","98072","98034")`  
  
 当筛选条件包含在中时，该字段必须包含其中一个值，然后才能将其记录包含在查询结果中。  
  
 **示例 8** *FieldName* [NOT] IN (*子查询*)   
  
 `customer.cust_id IN ;`  
  
 `(SELECT orders.cust_id FROM orders WHERE orders.city="Seattle")`  
  
 此处的字段必须包含子查询返回的值之一，然后才能将其记录包含在查询结果中。  
  
 **示例 9** *FieldName* [NOT] LIKE *cExpression*  
  
 `customer.country NOT LIKE "USA"`  
  
 此筛选条件搜索与 *cExpression* 匹配的每个字段。 可以使用百分号 (% ) 和下划线 ( _ ) 通配符作为 *cExpression* 的一部分。 下划线表示字符串中的单个未知字符。  
  
 GROUP BY *GroupColumn* [， *GroupColumn* ...]  
 基于一个或多个列中的值对查询中的行进行分组。 *GroupColumn* 可以是下列其中一项：  
  
-   常规表字段的名称。  
  
-   包含 SQL 字段函数的字段。  
  
-   一个数值表达式，指示列在结果表中的位置。  (最左侧的列号为1。 )   
  
 具有 *FilterCondition*  
 指定要包含在查询结果中的组必须满足的筛选条件。 HAVING 应与 GROUP BY 一起使用，并且可以包含所需的任意多个筛选条件，并由 AND 或 OR 运算符连接。 您也可以使用 NOT 来反转逻辑表达式的值。  
  
 *FilterCondition* 不能包含子查询。  
  
 HAVING 子句不带 GROUP BY 子句的行为与 WHERE 子句类似。 可以在 HAVING 子句中使用本地别名和字段函数。 如果 HAVING 子句不包含字段函数，请使用 WHERE 子句提高性能。  
  
 [UNION [ALL] *SELECTCommand*]  
 将一个 SELECT 的最终结果与另一个 SELECT 的结果组合在一起。 默认情况下，UNION 检查合并的结果并消除重复的行。 使用括号组合多个联合子句。  
  
 ALL 会阻止联合从合并的结果中消除重复的行。  
  
 联合子句遵循以下规则：  
  
-   不能使用 UNION 组合子查询。  
  
-   这两个 SELECT 命令的查询输出中的列数必须相同。  
  
-   每个 SELECT 查询结果中的每一列的数据类型和宽度必须与另一个 SELECT 中对应列的数据类型和宽度相同。  
  
-   只有最后一个 SELECT 语句才能具有 ORDER BY 子句，该子句必须按数字引用输出列。 如果包含 ORDER BY 子句，则会影响完整结果。  
  
 您还可以使用 UNION 子句来模拟外部联接。  
  
 在查询中联接两个表时，输出中只包括具有匹配值的记录。 如果父表中的记录在子表中没有相应的记录，则不会在输出中包含父表中的记录。 外部联接使您可以在输出中包含父表中的所有记录以及子表中的匹配记录。 若要在 Visual FoxPro 中创建外部联接，必须使用嵌套的 SELECT 命令，如以下示例中所示：  
  
```  
SELECT customer.company, orders.order_id, orders.emp_id ;  
FROM customer, orders ;  
WHERE customer.cust_id = orders.cust_id ;  
UNION ;  
SELECT customer.company, 0, 0 ;  
FROM customer ;  
WHERE customer.cust_id NOT IN ;  
(SELECT orders.cust_id FROM orders)  
```  
  
> [!NOTE]  
>  请确保在每个分号之前都包含空格。 否则，会收到错误。  
  
 UNION 子句之前的命令部分从两个表中选择了具有匹配值的记录。 不包含关联发票的客户公司。 Command 子句之后的命令部分选择 customer 表中的记录，这些记录在 orders 表中没有匹配的记录。  
  
 有关该命令的第二部分，请注意以下事项：  
  
-   括号内的 SELECT 语句首先处理。 此语句在 orders 表中创建所有客户编号的选择。  
  
-   WHERE 子句查找 customer 表中不在 orders 表中的所有客户编号。 因为该命令的第一部分提供订单表中具有客户编号的所有公司，所以 customer 表中的所有公司现在都包含在查询结果中。  
  
-   由于联合中包含的表的结构必须是相同的，因此第二个 SELECT 语句中有两个占位符用于表示第一个 SELECT 语句 *orders.order_id* 和 *orders.emp_id* 。  
  
    > [!NOTE]  
    >  占位符的类型必须与它们所表示的字段的类型相同。 如果该字段是日期类型，则占位符应为 {//}。 如果该字段是字符字段，则占位符应为空字符串 ( "" ) 。  
  
 ORDER BY *Order_Item* [ASC &#124; desc] [， *Order_Item* [asc &#124; DESC] ...]  
 根据一个或多个列中的数据对查询结果进行排序。 每个 *Order_Item* 都必须对应于查询结果中的某一列，并且可以是下列项之一：  
  
-   FROM 表中的一个字段，该字段也是主 SELECT 子句中的 select 项， (不在子查询) 中。  
  
-   一个数值表达式，指示列在结果表中的位置。  (最左侧的列是数字1。 )   
  
 ASC 根据订单项或项指定查询结果的升序顺序，并且是 ORDER BY 的默认值。  
  
 DESC 指定查询结果的降序顺序。  
  
 如果未指定 order BY 顺序，查询结果将显示为无序。  
  
## <a name="remarks"></a>备注  
 SELECT 是像任何其他 Visual FoxPro 命令一样内置于 Visual FoxPro 中的 SQL 命令。 使用 SELECT 引发查询时，Visual FoxPro 将解释查询并从表中检索指定的数据。 您可以在命令提示符窗口或 Visual FoxPro 程序中创建选择查询， (与任何其他 Visual FoxPro 命令) 。  
  
> [!NOTE]  
>  选择不遵守通过 SET FILTER 指定的当前筛选条件。  
  
## <a name="driver-remarks"></a>驱动程序备注  
 当应用程序将 ODBC SQL 语句发送到数据源时，Visual FoxPro ODBC 驱动程序会在不进行转换的情况下将命令转换为 Visual FoxPro SELECT 命令，除非该命令包含 ODBC 转义序列。 ODBC 转义序列中包含的项将转换为 Visual FoxPro 语法。 有关使用 ODBC 转义序列的详细信息，请参阅 [时间和日期函数](../../odbc/microsoft/time-and-date-functions-visual-foxpro-odbc-driver.md) 和 *Microsoft odbc 程序员参考* 中的 [转义序列](../../odbc/reference/develop-app/escape-sequences-in-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TABLE-SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INSERT-SQL](../../odbc/microsoft/insert-sql-command.md)   
 [设置 ANSI](../../odbc/microsoft/set-ansi-command.md)   
 [设置精确](../../odbc/microsoft/set-exact-command.md)
