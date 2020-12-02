---
description: 表列属性 (SQL Server Management Studio)
title: 表列属性 (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65558
- vdtsql.chm:69657
- vdt.ppg.columns
ms.assetid: 09830897-cc10-46b8-95f5-e0e9681b668c
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0ddf8c0360de470d5640ed650c4b8339061c41b4
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91810473"
---
# <a name="table-column-properties-sql-server-management-studio"></a>表列属性 (SQL Server Management Studio)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  这些属性显示在表设计器的底部窗格中。 除非另行说明，否则在选定列后可以在“属性”窗口中编辑这些属性。 **“列属性”** 可以按类别或字母顺序显示。 许多属性仅针对特定的数据类型显示或有所更改。  
  
> [!NOTE]  
>  如果表是为复制发布的，则必须使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理对象 (SMO) 对架构进行更改。 使用表设计器或数据库关系图设计器更改架构后，会尝试删除并重新创建表。 由于您不能删除已发布的对象，因此架构更改将失败。  
  
 **常规**  
 展开此项可显示“名称”、“允许空值”、“数据类型”、“默认值或绑定”、“长度”、“精度”和“小数位数”。  
  
 **名称**  
 显示所选列的名称。  
  
 **允许 Null 值**  
 指示此列是否允许空值。 若要编辑此属性，请在表设计器的顶部窗格中单击与列对应的“允许空值”复选框。  
  
 **数据类型**  
 显示所选列的数据类型。 若要编辑此属性，请单击该属性的值，展开下拉列表，然后选择其他值。  
  
 **默认值或绑定**  
 当没有为此列指定值时显示此列的默认值。 此字段的值可以是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 默认约束的值，也可以是此列被绑定到的全局约束的名称。 该下拉列表中包含数据库中定义的所有全局默认值。 若要将该列绑定到某个全局默认值，请从下拉列表中进行选择。 另外，若要为该列创建默认约束，请直接以文本格式键入默认值。  
  
 **长度**  
 显示基于字符的数据类型所允许的字符数。 此属性仅可用于基于字符的数据类型。  
  
 **缩放**  
 显示此列中的值小数点右侧可以允许的最大位数。 对于非数值数据类型，此属性显示 **0** 。  
  
 **精度**  
 显示此列中的值的最大位数。 对于非数值数据类型，此属性显示 **0** 。  
  
 **表设计器**  
 展开“表设计器”部分。  
  
 **排序规则**  
 显示当使用列值对查询结果的行进行排序时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 默认情况下对列应用的排序规则顺序。 若要编辑排序规则，请选择该属性，单击属性值右侧显示的省略号 (   )，以打开“排序规则”对话框。  
  
 **计算所得的列规范**  
 显示计算所得的列的相关信息。 该属性显示的值与“公式”  子属性的值相同，可显示计算所得的列的公式。  
  
> [!NOTE]  
>   若要更改“计算所得的列规范”  属性显示的值，必须展开该属性，再编辑“公式”  子属性。  
  
-   **公式** 显示计算所得的列的公式。 若要编辑此属性，请直接键入新公式。  
  
-   **是持久的** 指示是否存储公式的计算结果。 如果此属性设置为“否”  ，则只存储公式，每次引用此列时都会计算公式的值。 若要编辑此属性，请单击该属性的值，展开下拉列表，然后选择其他值。  
  
 有关详细信息，请参阅 [Specify Computed Columns in a Table](../../relational-databases/tables/specify-computed-columns-in-a-table.md)。  
  
 **简洁数据类型**  
 按与 SQL CREATE TABLE 语句同样的格式显示有关字段的数据类型的信息。 例如，一个包含可变长度字符串（最大长度为 20 个字符）的字段将表示为“varchar(20)”。 若要更改此属性，请直接键入值。  
  
 **说明**  
 显示描述此列的文本。 若要编辑该说明，请选择属性，单击属性值右侧显示的省略号 (   )，然后在“说明属性”对话框中编辑说明。  
  
 **具有确定性**  
 显示是否可以明确地确定所选列的数据类型。  
  
 **是 DTS 发布的**  
 显示该列是否是 DTS 发布的。 （[不推荐使用 Data Transformation Services](/previous-versions/sql/sql-server-2008-r2/cc707786(v=sql.105))）。 
  
 **全文本规范**  
 显示全文检索的相关信息。 此属性的值是“是全文索引的”子属性的值，指示此列是否为全文索引列。  
  
> [!NOTE]  
>  若要更改“全文本规范”属性显示的值，必须展开该属性，再编辑“是全文索引的”子属性。  
  
-   “是全文索引的”指示此列是否为全文索引列。 只有在可对此列的数据类型进行全文搜索并且为此列所属的表指定了全文索引时，此属性才可设置为“是”。 若要编辑此属性，请单击该属性的值，展开下拉列表，然后选择其他值。  
  
-   “全文类型列”显示对此列进行全文索引所基于的列的名称。 如果此列的“数据类型”  属性为 **image** 或 **varbinary**，则必须设置此属性。 此属性中指定的列必须是 **[n]char、[n]varchar** 或 **xml** 类型，并且此属性的下拉列表中只包含这三种数据类型的列。 此属性指定的列中的行指示可全文搜索列中相应行的文档类型。 若要编辑此属性，请单击该属性的值，展开下拉列表，然后选择其他值。  
  
-   **语言** 指示用于对该列进行索引的断字符的语言。 该属性中存储的值实际上是断字符的区域设置标识符。 有关断字符和 LCID 的详细信息，请参阅“断字符和词干分析器”。 若要编辑此属性，请单击该属性的值，展开下拉列表，然后选择其他值。  
  
 **统计语义**  
 选择是否为所选列启用统计语义索引。 有关详细信息，请参阅[语义搜索 (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md)。  
  
 如果您在选择 **“统计语义”** 前选择某一 **“语言”**，并且所选语言没有关联的语义语言模型，则 **“统计语义”** 复选框将设置为 **“否”** 并且无法修改。 如果您在选择 **“语言”** 前为 **“统计语义”** 选项选择 **“是”**，则 **“语言”** 列中提供的语言将限制为存在语义语言模型支持的那些语言。  
  
 **具有非 SQL Server 订户**  
 指示是否要将列复制到非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的订阅服务器。  
  
 **标识规范**  
 显示此列是否以及如何对其值强制唯一性的相关信息。 此属性的值指示此列是否为标识列以及是否与子属性“是标识” 的值相同。  
  
> [!NOTE]  
>   若要更改“标识规范”  属性显示的值，必须展开该属性，再编辑“是标识”  子属性。  
  
-   **是标识** 指示此列是否为标识列。 若要编辑此属性，请单击该属性的值，展开下拉列表，然后选择其他值。  
  
-   **标识种子** 显示在此标识列的创建过程中指定的种子值。 此值将赋给表中的第一行。 如果将此单元格保留为空白，则默认情况下，会将值 1 赋给该单元格。 若要编辑此属性，请直接键入新值。  
  
-   **标识增量** 显示在此标识列的创建过程中指定的增量值。 此值是基于 **“标识种子”** 依次为每个后续行增加的增量。 如果将此单元格保留为空白，则默认情况下，会将值 1 赋给该单元格。 若要编辑此属性，请直接键入新值。  
  
 **是可索引的**  
 显示是否可以对所选列进行索引。 例如，不能对不确定的计算列进行索引。  
  
 **是合并发布的**  
 显示该列是否是合并发布的。  
  
 **不用于复制**  
 指示在复制期间是否保留原始标识值。 有关复制的详细信息，请参阅 CREATE TABLE。 若要编辑此属性，请单击该属性的值，展开下拉列表，然后选择其他值。  
  
 **复制**  
 显示是否在其他位置复制此列。  
  
 **RowGuid**  
 指示 SQL Server 是否将该列用作 ROWGUID。 只有对唯一标识列才可将此值设置为 **“是”** 。 若要编辑此属性，请单击该属性的值，展开下拉列表，然后选择其他值。  
  
 **大小**  
 显示该列的数据类型所允许的大小（字节）。 例如，某个 nchar 数据类型的长度为 10（字符数），但在 Unicode 字符集中，该数据类型的大小为 20。  
  
> [!NOTE]  
>  **(max)** 数据类型的长度对于每一行都会有所不同。 **sp_help** 返回 (-1) 作为 **(max)** 列的长度。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 显示 -1 作为列大小。  
  
