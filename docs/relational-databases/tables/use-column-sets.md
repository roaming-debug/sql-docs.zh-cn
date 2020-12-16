---
description: 使用列集
title: 使用列集 | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2015
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- sparse columns, column sets
- column sets
ms.assetid: a4f9de95-dc8f-4ad8-b957-137e32bfa500
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d9d295edae69bc3354bab3151808fa5c4ff0ca7a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482334"
---
# <a name="use-column-sets"></a>使用列集
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  使用稀疏列的表可以指定一个列集以返回表中的所有稀疏列。 列集是一种非类型化的 XML 表示形式，它将表的所有稀疏列组合成为一种结构化的输出。 列集与计算列的相似之处在于，列集并不是物理地存储在表中。 列集与计算列的不同之处在于，列集可直接更新。  
  
 当表中有很多列，且分别对这些列进行操作很烦琐时，应考虑使用列集。 当应用程序通过对具有很多列的表使用列集来选择和插入数据时，可能会在某种程度上改进性能。 但是，当对表中的列定义很多索引时，列集的性能则会下降。 这是因为执行计划所需的内存量增加。  
  
 若要定义列集，请在 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 或 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 语句中使用 *<column_set_name>* FOR ALL_SPARSE_COLUMNS 关键字。  
  
## <a name="guidelines-for-using-column-sets"></a>列集的使用准则  
 使用列集时，请考虑以下准则：  
  
-   稀疏列和列集可以作为同一语句的一部分添加。  
  
-   如果某个表已包含稀疏列，则不能向该表中添加列集。  
  
-   列集无法更改。 若要更改列集，必须删除稀疏列和列集再重新创建。  
  
-   可以向不包含任何稀疏列的表中添加列集。 如果以后向表中添加稀疏列，它们将显示在列集中。  
  
-   每个表只能有一个列集。  
  
-   列集是可选的，不是必须使用列集才能使用稀疏列。  
  
-   不能对列集定义约束或默认值。  
  
-   计算列不能包含列集列。  
  
-   包含列集的表不支持分布式查询。  
  
-   复制功能不支持列集。  
  
-   变更数据捕获不支持列集。  
  
-   列集不能是任何类型的索引的一部分。 这包括 XML 索引、全文索引和索引视图。 列集不能作为任何索引中的包含列添加。  
  
-   列集不能用在筛选索引或筛选统计信息的筛选表达式中。  
  
-   当视图包括列集时，该列集会在视图中显示为一个 XML 列。  
  
-   索引视图定义中不能包含列集。  
  
-   如果包括含列集的表的已分区视图按名称指定稀疏列，则该已分区视图是可更新的。 如果已分区视图引用列集，则是不可更新的。  
  
-   查询通知不能引用列集。  
  
-   XML 数据的大小限制为 2 GB。 如果一行中的所有非 null 稀疏列的数据加起来超过此限额，则查询或 DML 操作将产生错误。  
  
-   有关 COLUMNS_UPDATED 函数返回的数据的信息，请参阅 [使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。  
  
## <a name="guidelines-for-selecting-data-from-a-column-set"></a>从列集中选择数据的准则  
 从列集中选择数据时，请考虑下面的准则：  
  
-   从概念上讲，列集是一种可更新的 XML 计算列，它将一组基础关系列聚合为一种 XML 表示形式。 列集仅仅支持 ALL_SPARSE_COLUMNS 属性。 此属性用于聚合特定行的所有稀疏列中的所有非 null 值。  
  
-   在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 表编辑器中，列集显示为一个可编辑的 XML 字段。 按如下格式定义列集：  
  
    ```  
    <column_name_1>value1</column_name_1><column_name_2>value2</column_name_2>...  
    ```  
  
     下面给出了一些列集值的示例：  
  
    -   `<sparseProp1>10</sparseProp1><sparseProp3>20</sparseProp3>`  
  
    -   `<DocTitle>Bicycle Parts List</DocTitle><Region>West</Region>`  
  
-   在列集的 XML 表示中将忽略包含 null 值的稀疏列。  
  
> [!WARNING]  
>  添加列集会更改 `SELECT *` 查询的行为。 此查询会将列集作为 XML 列返回，而不返回单个稀疏列。 架构设计人员和软件开发人员必须小心，不要破坏现有应用程序。  
  
## <a name="inserting-or-modifying-data-in-a-column-set"></a>在列集中插入或修改数据  
 可以通过以下方法来执行稀疏列的数据操作：使用单个列的名称，或者引用列集的名称并使用列集的 XML 格式来指定列集的值。 稀疏列可以按任何顺序出现在 XML 列中。  
  
 当使用 XML 列集插入或更新稀疏列值时，插入基础稀疏列的值将从 **xml** 数据类型隐式转换为另一种类型。 对于数值列，数值列的 XML 中的空白值将转换为空字符串。 这会导致在数值列中插入零，如下面的示例所示。  
  
```sql  
CREATE TABLE t (i int SPARSE, cs xml column_set FOR ALL_SPARSE_COLUMNS);  
GO  
INSERT t(cs) VALUES ('<i/>');  
GO  
SELECT i FROM t;  
GO  
```  
  
 在本示例中，没有为 `i`列指定值，但插入了 `0` 值。  
  
## <a name="using-the-sql_variant-data-type"></a>使用 sql_variant 数据类型  
 **sql_variant** 日期类型可以存储多种不同的数据类型，如 **int****char** 和 **date**。 列集会输出数据类型信息（例如与 **sql_variant** 值关联的小数位数、精度以及区域设置信息）作为生成的 XML 列中的属性。 如果尝试在自定义生成的 XML 语句中将这些属性作为对列集的插入或更新操作的输入提供，则其中一些属性是必需的，并会为一些属性分配默认值。 下表列出数据类型以及在未提供值时服务器所生成的默认值。  
  
|数据类型|localeID*|sqlCompareOptions|sqlCollationVersion|SqlSortId|最大长度|精度|缩放|  
|---------------|----------------|-----------------------|-------------------------|---------------|--------------------|---------------|-----------|  
|**char**、 **varchar**、 **binary**|-1|'Default'|0|0|8000|不适用**|不适用|  
|**nvarchar**|-1|'Default'|0|0|4000|不适用|不适用|  
|**decimal**、 **float**、 **real**|不适用|不适用|不适用|不适用|不适用|18|0|  
|**integer**、 **bigint**、 **tinyint**、 **smallint**|不适用|不适用|不适用|不适用|不适用|不适用|不适用|  
|**datetime2**|不适用|不适用|不适用|不适用|不适用|不适用|7|  
|**日期时间偏移**|不适用|不适用|不适用|不适用|不适用|不适用|7|  
|**datetime**、 **date**、 **smalldatetime**|不适用|不适用|不适用|不适用|不适用|不适用|不适用|  
|**money**、 **smallmoney**|不适用|不适用|不适用|不适用|不适用|不适用|不适用|  
|**time**|不适用|不适用|不适用|不适用|不适用|不适用|7|  
  
 \*  localeID -1 表示默认区域设置。 英语的区域设置为 1033。  
  
 ** 不适用 = 在针对列集的选择操作期间不会为这些属性输出值。 当调用方在插入或更新操作期间使用为列集提供的 XML 表达形式为该属性指定值时，会生成错误。  
  
## <a name="security"></a>安全性  
 列集的安全模式的工作原理类似于表与列之间存在的安全模式。 可以将列集视为一个小型表，选择操作类似于针对该微型表的 SELECT * 操作。 但是，列集与稀疏列之间的关系是分组关系，而不是严格意义上的容器关系。 安全模式检查列集列的安全性，并允许对基础稀疏列执行 DENY 操作。 安全模式的其他特征有：  
  
-   可以为列集列授予安全权限，也可以撤消这一权限，这与表中的其他任何列类似。  
  
-   列集列的 SELECT、INSERT、UPDATE、DELETE 和 REFERENCES 权限的 GRANT 或 REVOKE 不会传播到该列集的基础成员列。 它仅仅适用于列集列的使用。 列集的 DENY 权限则会传播到表的基础稀疏列。  
  
-   对列集列执行 SELECT、INSERT、UPDATE 和 DELETE 语句要求用户对该列集列拥有对应的权限，此外还必须对表中的所有稀疏列都拥有对应的权限。 因为列集表示表中的所有稀疏列，所以您必须对所有稀疏列（包括您可能不会更改的稀疏列）都拥有权限。  
  
-   对稀疏列或列集执行 REVOKE 语句会默认将安全性设置为其父对象的安全性。  
  
## <a name="examples"></a>示例  
 在下面的示例中，文档表包含列 `DocID` 和 `Title`的通用集。 生产组希望所有生产文档都有一个 `ProductionSpecification` 列和一个 `ProductionLocation` 列。 市场组希望所有市场文档都有一个 `MarketingSurveyGroup` 列。  
  
### <a name="a-creating-a-table-that-has-a-column-set"></a>A. A. 创建具有列集的表  
 下面的示例创建使用稀疏列并包括列集 `SpecialPurposeColumns`的表。 该示例在表中插入两行，然后从表中选择数据。  
  
> [!NOTE]  
>  该表只有五列，以便易于显示和读取。  
  
```sql  
USE AdventureWorks2012;  
GO  
  
CREATE TABLE DocumentStoreWithColumnSet  
    (DocID int PRIMARY KEY,  
     Title varchar(200) NOT NULL,  
     ProductionSpecification varchar(20) SPARSE NULL,  
     ProductionLocation smallint SPARSE NULL,  
     MarketingSurveyGroup varchar(20) SPARSE NULL,  
     MarketingProgramID int SPARSE NULL,  
     SpecialPurposeColumns XML COLUMN_SET FOR ALL_SPARSE_COLUMNS);  
GO  
```  
  
### <a name="b-inserting-data-to-a-table-by-using-the-names-of-the-sparse-columns"></a>B. 使用稀疏列的名称向表中插入数据  
 下面的示例向示例 A 中创建的表插入两行。这些示例使用稀疏列的名称，而不引用列集。  
  
```sql  
INSERT DocumentStoreWithColumnSet (DocID, Title, ProductionSpecification, ProductionLocation)  
VALUES (1, 'Tire Spec 1', 'AXZZ217', 27);  
GO  
  
INSERT DocumentStoreWithColumnSet (DocID, Title, MarketingSurveyGroup)  
VALUES (2, 'Survey 2142', 'Men 25 - 35');  
GO  
```  
  
### <a name="c-inserting-data-to-a-table-by-using-the-name-of-the-column-set"></a>C. 使用列集的名称向表中插入数据  
 下面的示例向示例 A 中创建的表插入第三行。这一次不使用稀疏列的名称， 而是使用列集的名称，插入操作以 XML 格式为四个稀疏列中的两列提供值。  
  
```sql  
INSERT DocumentStoreWithColumnSet (DocID, Title, SpecialPurposeColumns)  
VALUES (3, 'Tire Spec 2', '<ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation>');  
GO  
```  
  
### <a name="d-observing-the-results-of-a-column-set-when-select--is-used"></a>D. 观察使用 SELECT * 时的列集结果  
 下面的示例从包含列集的表中选择所有列。 它返回具有稀疏列的组合值的 XML 列， 而不是单独返回每个稀疏列。  
  
```sql  
SELECT DocID, Title, SpecialPurposeColumns FROM DocumentStoreWithColumnSet ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 DocID  Title        SpecialPurposeColumns  
 1      Tire Spec 1  <ProductionSpecification>AXZZ217</ProductionSpecification><ProductionLocation>27</ProductionLocation>  
 2      Survey 2142  <MarketingSurveyGroup>Men 25 - 35</MarketingSurveyGroup>  
 3      Tire Spec 2  <ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation> 
 ```
  
### <a name="e-observing-the-results-of-selecting-the-column-set-by-name"></a>E. 观察按名称选择列集的结果  
 因为生产部门对市场数据不感兴趣，所以本示例添加 `WHERE` 子句以限制输出。 本示例使用列集的名称。  
  
```sql  
SELECT DocID, Title, SpecialPurposeColumns  
FROM DocumentStoreWithColumnSet  
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 DocID  Title        SpecialPurposeColumns  
 1      Tire Spec 1  <ProductionSpecification>AXZZ217</ProductionSpecification><ProductionLocation>27</ProductionLocation>  
 3      Tire Spec 2  <ProductionSpecification>AXW9R411</ProductionSpecification><ProductionLocation>38</ProductionLocation>  
 ```
 
### <a name="f-observing-the-results-of-selecting-sparse-columns-by-name"></a>F. 观察按名称选择稀疏列的结果  
 当表包含列集时，您仍然可以使用各列名称来查询表，如下例所示。  
  
```sql  
SELECT DocID, Title, ProductionSpecification, ProductionLocation   
FROM DocumentStoreWithColumnSet  
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 DocID  Title        ProductionSpecification ProductionLocation`  
 1      Tire Spec 1  AXZZ217                 27`  
 3      Tire Spec 2  AXW9R411                38`  
 ```
 
### <a name="g-updating-a-table-by-using-a-column-set"></a>G. 使用列集来更新表  
 下面的示例用第三个记录所在行使用的两个稀疏列的新值来更新该记录。  
  
```sql  
UPDATE DocumentStoreWithColumnSet  
SET SpecialPurposeColumns = '<ProductionSpecification>ZZ285W</ProductionSpecification><ProductionLocation>38</ProductionLocation>'  
WHERE DocID = 3 ;  
GO  
```  
  
> [!IMPORTANT]  
>  使用列集的 UPDATE 语句更新表中的所有稀疏列。 未引用的稀疏列将更新为 NULL。  
  
 下面的示例更新第三个记录，但仅仅指定两个已填充列的其中一列的值。 在 `ProductionLocation` 语句中未包括第二列 `UPDATE` ，所以该列更新为 NULL。  
  
```sql  
UPDATE DocumentStoreWithColumnSet  
SET SpecialPurposeColumns = '<ProductionSpecification>ZZ285W</ProductionSpecification>'  
WHERE DocID = 3 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)  
  
  
