---
description: CREATE VIEW (Transact-SQL)
title: CREATE VIEW (Transact-SQL)
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 781dc13180d75e771402822259ad5ef6a913ac28
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170309"
---
# <a name="create-view-transact-sql"></a>CREATE VIEW (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  创建虚拟表，其内容（列和行）由查询定义。 使用此语句可以创建数据库中一个或多个表中数据的视图。 例如，可以将视图用于以下用途：  
  
-   集中、简化和自定义每个用户对数据库的认识。  
  
-   用作安全机制，方法是允许用户通过视图访问数据，而不授予用户直接访问底层基表的权限。  
  
-   提供向后兼容接口来模拟架构已更改的表。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE [ OR ALTER ] VIEW [ schema_name . ] view_name [ (column [ ,...n ] ) ]
[ WITH <view_attribute> [ ,...n ] ]
AS select_statement
[ WITH CHECK OPTION ]
[ ; ]  
  
<view_attribute> ::=
{  
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]
}
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

OR ALTER  
 适用范围：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 开始）。   
  
 有条件地删除视图（仅当其已存在时）。 
 
 *schema_name*  
 视图所属架构的名称。  
  
 view_name  
 视图名称。 视图名称必须符合有关标识符的规则。 可以选择是否指定视图所有者名称。  
  
 *column*  
 视图中的列使用的名称。 仅在下列情况下需要列名：列是从算术表达式、函数或常量派生的；两个或更多的列可能会具有相同的名称（通常是由于联接的原因）；视图中的某个列的指定名称不同于其派生来源列的名称。 还可以在 SELECT 语句中分配列名。  
  
 如果未指定 column，则视图列将获得与 SELECT 语句中的列相同的名称。  
  
> [!NOTE]  
>  在视图的各列中，列名的权限在 CREATE VIEW 或 ALTER VIEW 语句间均适用，与基础数据源无关。 例如，如果在 CREATE VIEW 语句中授予了 SalesOrderID 列上的权限，则 ALTER VIEW 语句可以将 SalesOrderID 列改名（例如改为 OrderRef），但仍具有与使用 SalesOrderID 的视图相关联的权限   。  
  
 AS  
 指定视图要执行的操作。  
  
 select_statement  
 定义视图的 SELECT 语句。 该语句可以使用多个表和其他视图。 需要相应的权限才能在已创建视图的 SELECT 子句引用的对象中选择。  
  
 视图不必是具体某个表的行和列的简单子集。 可以使用多个表或带任意复杂性的 SELECT 子句的其他视图创建视图。  
  
 在索引视图定义中，SELECT 语句必须是单个表的语句或带有可选聚合的多表 JOIN。  
  
 视图定义中的 SELECT 子句不能包括下列内容：  
  
-   ORDER BY 子句，除非在 SELECT 语句的选择列表中也有一个 TOP 子句。  
  
    > [!IMPORTANT]  
    >  ORDER BY 子句仅用于确定视图定义中的 TOP 或 OFFSET 子句返回的行。 ORDER BY 不保证在查询视图时得到有序结果，除非在查询本身中也指定了 ORDER BY。  
  
-   INTO 关键字  
  
-   OPTION 子句  
  
-   引用临时表或表变量。  
  
 因为 select_statement 使用 SELECT 语句，所以按照 FROM 子句的指定，使用 \<join_hint> 和 \<table_hint> 提示是有效的。 有关详细信息，请参阅 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md) 和 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)。 
  
 UNION 或 UNION ALL 分隔的函数和多个 SELECT 语句可在 select_statement 中使用。  
  
 CHECK OPTION  
 要求对该视图执行的所有数据修改语句都必须符合 select_statement 中所设置的条件。 通过视图修改行时，WITH CHECK OPTION 可确保提交修改后，仍可通过视图看到数据。  
  
> [!NOTE]
>  CHECK OPTION 仅适用于通过视图进行的更新。 它不适用于直接对视图的基础表执行的任何更新。  
  
 ENCRYPTION  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
 对 [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md) 表中包含 CREATE VIEW 语句文本的项进行加密。 使用 WITH ENCRYPTION 可防止在 SQL Server 复制过程中发布视图。  
  
 SCHEMABINDING  
 将视图绑定到基础表的架构。 如果指定了 SCHEMABINDING，则不能按照将影响视图定义的方式修改基表或表。 必须首先修改或删除视图定义本身，才能删除将要修改的表的依赖关系。 使用 SCHEMABINDING 时，select_statement 必须包含所引用的表、视图或用户定义函数的两部分名称 (schema.object)。 所有被引用对象都必须在同一个数据库内。  
  
 不能删除参与了使用 SCHEMABINDING 子句创建的视图的视图或表，除非该视图已被删除或更改而不再具有架构绑定。 否则，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将引发错误。 另外，如果对参与具有架构绑定的视图的表执行 ALTER TABLE 语句，而这些语句又会影响视图定义，则这些语句将会失败。  
  
 VIEW_METADATA  
 指定为引用视图的查询请求浏览模式的元数据时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将向 DB-Library、ODBC 和 OLE DB API 返回有关视图的元数据信息，而不返回基表的元数据信息。 浏览模式元数据是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例向这些客户端 API 返回的附加元数据。 如果使用此元数据，客户端 API 将可以实现可更新客户端游标。 浏览模式的元数据包含结果集中的列所属的基表的相关信息。  
  
 对于使用 VIEW_METADATA 创建的视图，浏览模式的元数据在描述结果集内视图中的列时，将返回视图名，而不返回基表名。  
  
 当使用 WITH VIEW_METADATA 创建视图时，如果该视图具有 INSTEAD OF INSERT 或 INSTEAD OF UPDATE 触发器，则视图的所有列（timestamp 列除外）都可更新。 有关可更新视图的详细信息，请参阅“备注”。  
  
## <a name="remarks"></a>备注  
 只能在当前数据库中创建视图。 CREATE VIEW 必须是查询批处理中的第一条语句。 视图最多可以包含 1,024 列。  
  
 通过视图进行查询时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将进行检查以确保语句中任何位置被引用所有数据库对象都存在，这些对象在语句的上下文中有效，以及数据修改语句没有违反任何数据完整性规则。 如果检查失败，将返回错误消息。 如果检查成功，则将操作转换为对基础表的操作。  
  
 如果某个视图依赖于已删除的表（或视图），则当有人试图使用该视图时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将产生错误消息。 如果创建了新表或视图（该表的结构与以前的基表没有不同之处）以替换删除的表或视图，则视图将再次可用。 如果新表或视图的结构发生更改，则必须删除并重新创建该视图。  
  
 如果未使用 SCHEMABINDING 子句创建视图，请在对视图下影响视图定义的对象进行更改时，运行 [sp_refreshview](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md)。 否则，当查询视图时，可能会生成意外结果。  
  
 创建视图时，有关该视图的信息将存储在下列目录视图中：[sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)、[sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) 和 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)。 CREATE VIEW 语句的文本将存储在 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) 目录视图中。  
  
 对通过 numeric 或 float 表达式定义的视图使用索引所得到的查询结果，可能不同于不对视图使用索引的类似查询所得到的结果 。 这种差异可能是由对基础表进行 INSERT、DELETE 或 UPDATE 操作时的舍入错误引起的。  
  
 创建视图时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将保存 SET QUOTED_IDENTIFIER 和 SET ANSI_NULLS 的设置。 使用视图时，将使用这些原始设置来分析视图。 因此，访问视图时，SET QUOTED_IDENTIFIER 和 SET ANSI_NULLS 的任何客户端会话设置都不会影响视图定义。  
  
## <a name="updatable-views"></a>可更新的视图  
 只要满足下列条件，即可通过视图修改基础基表的数据：  
  
-   任何修改（包括 UPDATE、INSERT 和 DELETE 语句）都只能引用一个基表的列。  
  
-   视图中被修改的列必须直接引用表列中的基础数据。 不能通过任何其他方式对这些列进行派生，如通过以下方式：  
  
    -   聚合函数：AVG、COUNT、SUM、MIN、MAX、GROUPING、STDEV、STDEVP、VAR 和 VARP。  
  
    -   计算。 不能从使用其他列的表达式中计算该列。 使用集合运算符 UNION、UNION ALL、CROSSJOIN、EXCEPT 和 INTERSECT 形成的列将计入计算结果，且不可更新。  
  
-   被修改的列不受 GROUP BY、HAVING 或 DISTINCT 子句的影响。  
  
-   TOP 在视图的 select_statement 中的任何位置都不会与 WITH CHECK OPTION 子句一起使用。  
  
 上述限制应用于视图的 FROM 子句中的任何子查询，就像其应用于视图本身一样。 通常情况下，[!INCLUDE[ssDE](../../includes/ssde-md.md)]必须能够明确跟踪从视图定义到一个基表的修改。 有关详细信息，请参阅[通过视图修改数据](../../relational-databases/views/modify-data-through-a-view.md)。  
  
 如果上述限制妨碍直接通过视图修改数据，则可以考虑以下选项：  
  
-   **INSTEAD OF 触发器**  
  
     可以对视图创建 INSTEAD OF 触发器，以使视图可更新。 将执行 INSTEAD OF 触发器，而不是执行对其定义了触发器的数据修改语句。 此触发器允许用户指定必须发生以处理数据修改语句的操作集合。 因此，如果存在给定的数据修改语句（INSERT、UPDATE 或 DELETE）的视图的 INSTEAD OF 触发器，则可通过该语句更新相应的视图。 有关 INSTEAD OF 触发器的详细信息，请参阅 [DML 触发器](../../relational-databases/triggers/dml-triggers.md)。  
  
-   **分区视图**  
  
     如果视图为分区视图，则可遵循某些限制对其进行更新。 必要时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将本地分区视图辨别为所有参与表和视图都在同一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的视图，而将分布式分区视图辨别为视图中至少有一个表驻留在其他或远程服务器上的视图。  
  
## <a name="partitioned-views"></a>分区视图  
 分区视图是通过对成员表使用 UNION ALL 所定义的视图，这些成员表的结构相同，但作为多个表分别存储在同一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中，或存储在称为联合数据库服务器的自主 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务器实例组中。  
  
> [!NOTE]  
>  对一个服务器的本地数据进行分区的首选方法是通过分区表。 有关详细信息，请参阅 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。  
  
 在设计分区方案时，必须明确每个分区上包含的数据。 例如，`Customers` 表的数据分布在三个服务器位置的三个成员表中：`Customers_33` 上的 `Server1`、`Customers_66` 上的 `Server2` 和 `Customers_99` 上的 `Server3`。  
  
 `Server1` 的分区视图通过以下方式进行定义：  
  
```sql
--Partitioned view as defined on Server1  
CREATE VIEW Customers  
AS  
--Select from local member table.  
SELECT *  
FROM CompanyData.dbo.Customers_33  
UNION ALL  
--Select from member table on Server2.  
SELECT *  
FROM Server2.CompanyData.dbo.Customers_66  
UNION ALL  
--Select from member table on Server3.  
SELECT *  
FROM Server3.CompanyData.dbo.Customers_99;  
```  
  
 一般情况下，如果视图为下列格式，则称其为分区视图：  
  
```syntaxsql
SELECT <select_list1>  
FROM T1  
UNION ALL  
SELECT <select_list2>  
FROM T2  
UNION ALL  
...  
SELECT <select_listn>  
FROM Tn;  
```  
  
## <a name="conditions-for-creating-partitioned-views"></a>创建分区视图的条件  
  
1.  选择 `list`  
  
    -   在视图定义的列列表中，选择成员表中的所有列。  
  
    -   确保每个 `select list` 中的同一序号位置上的列属于同一类型，包括排序规则。 列仅仅属于可隐式转换的类型（如通常情况下的 UNION）是不够的。  
  
         此外，至少有一列（例如 `<col>`）必须按照相同的序号位置显示在所有选择列表中。 按照以下方式定义 `<col>`：成员表 `T1, ..., Tn` 分别在 `C1, ..., Cn` 上定义 CHECK 约束 `<col>`。  
  
         在表 `C1` 上定义的约束 `T1` 必须是以下格式：  
  
        ```syntaxsql
        C1 ::= < simple_interval > [ OR < simple_interval > OR ...]  
        < simple_interval > :: =   
        < col > { < | > | \<= | >= | = < value >}   
        | < col > BETWEEN < value1 > AND < value2 >  
        | < col > IN ( value_list )  
        | < col > { > | >= } < value1 > AND  
        < col > { < | <= } < value2 >  
        ```
  
    -   约束必须按照以下方式定义：`<col>` 的任何指定值最多只能满足一个 `C1, ..., Cn` 约束，从而使约束形成一组不联接或不重叠的间隔。 定义不联接的约束的列 `<col>` 称为分区列。 请注意，分区列在基础表中可能有不同的名称。 约束必须处于启用和信任状态，以使它们满足分区依据列的上述条件。 如果约束被禁用，则使用 ALTER TABLE 的 CHECK CONSTRAINT constraint_name 选项重新启用约束检查，并使用 WITH CHECK 选项对其进行验证。  
  
         以下示例显示有效的约束集合：  
  
        ```syntaxsql
        { [col < 10], [col between 11 and 20] , [col > 20] }  
        { [col between 11 and 20], [col between 21 and 30], [col between 31 and 100] }  
        ```  
  
    -   在选择列表中不能多次使用同一列。  
  
2.  分区列  
  
    -   分区列是表的 PRIMARY KEY 的一部分。  
  
    -   分区列不能是计算列、标识列、默认列或 timestamp 列。  
  
    -   如果成员表中的同一列上存在多个约束，则数据库引擎将忽略所有约束，且在确定视图是否为分区视图时不考虑这些约束。 若要满足分区视图的条件，确保在分区依据列上只有一个分区约束。  
  
    -   分区列的可更新性没有限制。  
  
3.  成员表或基础表 `T1, ..., Tn`  
  
    -   表可以是本地表，也可以是通过由四部分组成的名称或基于 OPENDATASOURCE 或 OPENROWSET 的名称引用的运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的其他计算机中的表。 OPENDATASOURCE 和 OPENROWSET 语法可以指定表名，但不能指定直接传递查询。 有关详细信息，请参阅 [OPENDATASOURCE (Transact-SQL)](../../t-sql/functions/opendatasource-transact-sql.md) 和 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)。  
  
         如果一个或多个成员表是远程表，则视图将被称为“分布式分区视图”，并且将应用附加条件。 本部分后面将对其进行说明。  
  
    -   在用 UNION ALL 语句组合的表集合中，同一个表不能出现两次。  
  
    -   成员表不能对表中的计算列创建索引。  
  
    -   成员表在编号相同的列上具有所有 PRIMARY KEY 约束。  
  
    -   视图中的所有成员表都具有相同的 ANSI 填充设置。 这可以使用 sp_configure 中的 user options 选项或 SET 语句进行设置 。  
  
## <a name="conditions-for-modifying-data-in-partitioned-views"></a>在分区视图中修改数据的条件  
 下面的限制适用于在分区视图中修改数据的语句：  
  
-   即使基础成员表对这些列具有 DEFAULT 约束或允许 NULL 值，INSERT 语句也会为视图中的所有列提供值。 对于那些具有 DEFAULT 定义的成员表列，这些语句无法显式地使用关键字 DEFAULT。  
  
-   插入到分区依据列中的值至少满足一个基础约束；否则，插入操作将因约束冲突而失败。  
  
-   即使列中包含在相应成员表中定义的 DEFAULT 值，UPDATE 语句也不能指定 DEFAULT 关键字作为 SET 子句中的值。  
  
-   不能通过 INSERT 或 UPDATE 语句修改视图中作为一个或多个成员表中的标识列的列。  
  
-   如果其中的一个成员表包含 timestamp 列，则无法使用 INSERT 或 UPDATE 语句来修改数据。  
  
-   如果一个成员表包含触发器或 ON UPDATE CASCADE/SET NULL/SET DEFAULT 或 ON DELETE CASCADE/SET NULL/SET DEFAULT 约束，则不能修改视图。  
  
-   如果语句中存在与相同视图或任何成员表的自联接，则不允许对分区视图使用 INSERT、UPDATE 和 DELETE 操作。  
  
-   以下语句不支持将数据批量导入分区视图：bcp 或 BULK INSERT 和 INSERT ...SELECT * FROM OPENROWSET(BULK...) 语句使用格式化文件大容量导入数据。 但是，可以使用 [INSERT](../../t-sql/statements/insert-transact-sql.md) 语句在分区视图中插入多行。  
  
    > [!NOTE]  
    >  若要更新分区视图，用户必须具有对成员表的 INSERT、UPDATE 和 DELETE 权限。  
  
## <a name="additional-conditions-for-distributed-partitioned-views"></a>分布式分区视图的附加条件  
 对于分布式分区视图（其中有一个或多个成员表为远程表），适用下列附加条件：  
  
-   将启动分布式事务以确保更新所影响的所有节点间的原子性。  
  
-   将 XACT_ABORT SET 选项设置为 ON，以使 INSERT、UPDATE 或 DELETE 语句生效。  
  
-   在分区视图中引用的远程表的所有 smallmoney 类型的列都将映射为 money 。 因此，本地表中相应的列（在选择列表中的相同序号位置中）必须也为 money 类型。  
  
-   数据库兼容级别为 110 和更高级别时，在分区视图中引用的远程表的所有 smalldatetime 类型的列都将映射为 smalldatetime 。 本地表中相应的列（在选择列表中的相同序号位置中）必须为 smalldatetime。 这是对早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的行为的更改，在早期版本中，在分区视图中引用的远程表的所有 smalldatetime 类型的列都将映射为 datetime，并且本地表中相应的列必须为 datetime 类型  。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
-   分区视图中的所有链接服务器都不能是环回链接服务器。 这是一个指向同一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的链接服务器。  
  
 对于涉及可更新分区视图和远程表的 INSERT、UPDATE 和 DELETE 操作，忽略 SET ROWCOUNT 选项的设置。  
  
 设置了成员表和分区视图的定义后，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器将生成智能计划，从而有效利用查询访问成员表中的数据。 通过使用 CHECK 约束定义，查询处理器在成员表间映射键值的分布。 用户发出查询时，查询处理器将映射与 WHERE 子句中指定的值进行比较，然后生成使成员服务器间的数据传输量减到最少的执行计划。 因此，虽然有些成员表可能位于远程服务器中，但是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将解析分布式查询，使得必须传输的分布式数据量减到最少。  
  
## <a name="considerations-for-replication"></a>有关复制的考虑事项  
 若要对复制所涉及的成员表创建分区视图，需要考虑下列事项：  
  
-   如果基础表涉及到包含更新订阅的合并复制或事务复制，请确保 uniqueidentifier 列也包含在选择列表中。 
  
     对分区视图进行的任何 INSERT 操作都必须为 uniqueidentifier 列提供 NEWID() 值。 因为无法使用 DEFAULT 关键字，所以对 uniqueidentifier 列的任何 UPDATE 操作都必须提供 NEWID() 值。  
  
-   通过视图进行的更新复制与在两个不同的数据库中复制表时相同：表由不同的复制代理进行处理，因此不能保证更新的顺序。  
  
## <a name="permissions"></a>权限  
 要求在数据库中具有 CREATE VIEW 权限，并具有在其中创建视图的架构的 ALTER 权限。  
  
## <a name="examples"></a>示例  

下列示例使用 AdventureWorks 2012 和 AdventureWorksDW 数据库。  

### <a name="a-using-a-simple-create-view"></a>A. 使用简单 CREATE VIEW  
 以下示例使用简单 `SELECT` 语句创建视图。 当需要频繁地查询列的某种组合时，简单视图非常有用。 此视图的数据来自 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `HumanResources.Employee` 和 `Person.Person` 表。 这些数据提供有关 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 的雇员的姓名和雇用日期信息。 对于负责跟踪工作年限的人员，可创建此视图，但是不能授予此人访问这些表中的所有数据的权限。  
  
```sql
CREATE VIEW hiredate_view  
AS   
SELECT p.FirstName, p.LastName, e.BusinessEntityID, e.HireDate  
FROM HumanResources.Employee e   
JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
  
```  
  
### <a name="b-using-with-encryption"></a>B. 使用 WITH ENCRYPTION  
 以下示例使用 `WITH ENCRYPTION` 选项，并显示计算列、重命名列以及多个列。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]。  
  
```sql
CREATE VIEW Purchasing.PurchaseOrderReject  
WITH ENCRYPTION  
AS  
SELECT PurchaseOrderID, ReceivedQty, RejectedQty,   
    RejectedQty / ReceivedQty AS RejectRatio, DueDate  
FROM Purchasing.PurchaseOrderDetail  
WHERE RejectedQty / ReceivedQty > 0  
AND DueDate > CONVERT(DATETIME,'20010630',101) ;  
GO  
  
```  
  
### <a name="c-using-with-check-option"></a>C. 使用 WITH CHECK OPTION  
 以下示例显示名为 `SeattleOnly` 的视图，此视图引用了五个表，并允许进行数据修改，以便仅适用于居住在西雅图的雇员。  
  
```sql
CREATE VIEW dbo.SeattleOnly  
AS  
SELECT p.LastName, p.FirstName, e.JobTitle, a.City, sp.StateProvinceCode  
FROM HumanResources.Employee e  
INNER JOIN Person.Person p  
ON p.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN Person.BusinessEntityAddress bea   
    ON bea.BusinessEntityID = e.BusinessEntityID   
    INNER JOIN Person.Address a   
    ON a.AddressID = bea.AddressID  
    INNER JOIN Person.StateProvince sp   
    ON sp.StateProvinceID = a.StateProvinceID  
WHERE a.City = 'Seattle'  
WITH CHECK OPTION ;  
GO  
```  
  
### <a name="d-using-built-in-functions-within-a-view"></a>D. 使用视图中的内置函数  
 以下示例显示包含内置函数的视图定义。 使用函数时，必须为派生列指定一个列名。  
  
```sql
CREATE VIEW Sales.SalesPersonPerform  
AS  
SELECT TOP (100) SalesPersonID, SUM(TotalDue) AS TotalSales  
FROM Sales.SalesOrderHeader  
WHERE OrderDate > CONVERT(DATETIME,'20001231',101)  
GROUP BY SalesPersonID;  
GO  

```  
  
### <a name="e-using-partitioned-data"></a>E. 使用分区数据  
 以下示例将使用名称分别为 `SUPPLY1`、`SUPPLY2`、`SUPPLY3` 和 `SUPPLY4` 的表。 这些表对应于位于四个国家/地区的四个办事处的供应商表。  
  
```sql
--Create the tables and insert the values.  
CREATE TABLE dbo.SUPPLY1 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 1 and 150),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY2 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 151 and 300),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY3 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 301 and 450),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY4 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 451 and 600),  
supplier CHAR(50)  
);  
GO  
--Create the view that combines all supplier tables.  
CREATE VIEW dbo.all_supplier_view  
WITH SCHEMABINDING  
AS  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY1  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY2  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY3  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY4;  
GO
INSERT dbo.all_supplier_view VALUES ('1', 'CaliforniaCorp'), ('5', 'BraziliaLtd')    
, ('231', 'FarEast'), ('280', 'NZ')  
, ('321', 'EuroGroup'), ('442', 'UKArchip')  
, ('475', 'India'), ('521', 'Afrique');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>示例：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-creating-a-simple-view"></a>F. 创建简单视图  
 下面的示例通过仅从源表选择某些列来创建视图。  
  
```sql
CREATE VIEW DimEmployeeBirthDates AS  
SELECT FirstName, LastName, BirthDate   
FROM DimEmployee;  
```  
  
### <a name="g-create-a-view-by-joining-two-tables"></a>G. 通过联接两个表来创建视图  
 以下示例通过搭配使用 `SELECT` 语句和 `OUTER JOIN` 来创建视图。 联接查询的结果将会填充视图。  
  
```sql
CREATE VIEW view1  
AS 
SELECT fis.CustomerKey, fis.ProductKey, fis.OrderDateKey, 
  fis.SalesTerritoryKey, dst.SalesTerritoryRegion  
FROM FactInternetSales AS fis   
LEFT OUTER JOIN DimSalesTerritory AS dst   
ON (fis.SalesTerritoryKey=dst.SalesTerritoryKey);  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-view-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [DROP VIEW (Transact-SQL)](../../t-sql/statements/drop-view-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [创建存储过程](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_refreshview (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md)   
 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sys.views (Transact-SQL)](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

