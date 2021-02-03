---
description: DROP INDEX (Transact-SQL)
title: DROP INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_INDEX_TSQL
- DROP INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- nonclustered indexes [SQL Server], removing
- MAXDOP index option, DROP INDEX statement
- index removal [SQL Server]
- spatial indexes [SQL Server], dropping
- removing indexes
- deleting indexes
- dropping indexes
- MOVE TO clause
- clustered indexes, removing
- indexes [SQL Server], dropping
- filtered indexes [SQL Server], dropping
- ONLINE option
- indexes [SQL Server], moving
- XML indexes [SQL Server], dropping
- DROP INDEX statement
ms.assetid: 2b1464c8-934c-405f-8ef7-2949346b5372
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 163b64d3ecd5f4f21cfbbc5fef6944f7610f78ea
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237231"
---
# <a name="drop-index-transact-sql"></a>DROP INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从当前数据库中删除一个或多个关系索引、空间索引、筛选索引或 XML 索引。 通过指定 MOVE TO 选项，可以在单个事务中删除聚集索引并将生成的表移动到另一个文件组或分区方案。  
  
 DROP INDEX 语句不适用于通过定义 PRIMARY KEY 或 UNIQUE 约束创建的索引。 若要删除该约束和相应的索引，请使用带有 DROP CONSTRAINT 子句的 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)。  
  
> [!IMPORTANT]
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的将来版本将删除在 `<drop_backward_compatible_index>` 中定义的语法。 请避免在新的开发工作中使用该功能，并考虑修改当前使用该功能的应用程序。 请改用在 `<drop_relational_or_xml_index>` 下指定的语法。 使用向后兼容语法无法删除 XML 索引。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server (All options except filegroup and filestream apply to Azure SQL Database.)  
  
DROP INDEX [ IF EXISTS ]   
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
| <drop_backward_compatible_index> [ ,...n ]  
}  
  
<drop_relational_or_xml_or_spatial_index> ::=  
    index_name ON <object>   
    [ WITH ( <drop_clustered_index_option> [ ,...n ] ) ]  
  
<drop_backward_compatible_index> ::=  
    [ owner_name. ] table_or_view_name.index_name  
  
<object> ::=  
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
  
<drop_clustered_index_option> ::=  
{  
    MAXDOP = max_degree_of_parallelism  
  | ONLINE = { ON | OFF }  
  | MOVE TO { partition_scheme_name ( column_name )   
            | filegroup_name  
            | "default"   
            }  
  [ FILESTREAM_ON { partition_scheme_name   
            | filestream_filegroup_name   
            | "default" } ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database  
  
DROP INDEX  
{ <drop_relational_or_xml_or_spatial_index> [ ,...n ]   
}  
  
<drop_relational_or_xml_or_spatial_index> ::=   
    index_name ON <object>  
  
<object> ::=   
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP INDEX index_name ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 IF EXISTS  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 到[当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。  
  
 仅当索引已存在时对其进行有条件地删除。  
  
 index_name  
 要删除的索引名称。  
  
 *database_name*  
 数据库的名称。  
  
 *schema_name*  
 表或视图所属架构的名称。  
  
 table_or_view_name  
 与该索引关联的表或视图的名称。 只有表支持空间索引。  
  
 若要显示对象的索引报表，请使用 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目录视图。  
  
 Azure SQL 数据库支持由三部分组成的名称格式 database_name.[schema_name].object_name，其中 database_name 为当前数据库，database_name 为 tempdb，object_name 以 # 开头。  
  
 \<drop_clustered_index_option>  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 控制聚集索引选项。 这些选项不能与其他索引类型一起使用。  
  
 MAXDOP = max_degree_of_parallelism  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]（仅限性能级别 P2 和 P3）。  
  
 在索引操作期间替代 max degree of parallelism 配置选项。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。 使用 MAXDOP 可以限制在执行并行计划的过程中使用的处理器数量。 最大数量为 64 个处理器。  
  
> [!IMPORTANT]  
>  空间索引或 XML 索引不允许使用 MAXDOP。  
  
 max_degree_of_parallelism 可以是：  
  
 1  
 取消生成并行计划。  
  
 \>1  
 将并行索引操作中使用的最大处理器数量限制为指定数量。  
  
 0（默认值）  
 根据当前系统工作负荷使用实际的处理器数量或更少数量的处理器。  
  
 有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
> [!NOTE]  
>  并非在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个版本中均提供并行索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅 [SQL Server 2016 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
 ONLINE = ON | OFF  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
 指定在索引操作期间基础表和关联的索引是否可用于查询和数据修改操作。 默认为 OFF。  
  
 ON  
 不保留长期表锁。 这样便允许继续对基础表进行查询或更新。  
  
 OFF  
 应用表锁，该表在索引操作期间不可用。  
  
 只能在删除聚集索引时指定 ONLINE 选项。 有关详细信息，请参见“备注”部分。  
  
> [!NOTE]  
>  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的各版本中均不提供联机索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅 [SQL Server 2016 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
 MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ **)**  | _filegroup\_name_ |  **"** default **"**  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 支持将 "default" 作为文件组名称。  
  
 指定一个位置，以移动当前处于聚集索引叶级别的数据行。 数据将以堆的形式移动到这一新位置。 可以将分区方案或文件组指定为新位置，但该分区方案或文件组必须已存在。 MOVE TO 对索引视图或非聚集索引无效。 如果未指定分区方案或文件组，则生成的表将位于为聚集索引定义的同一分区方案或文件组中。  
  
 如果使用 MOVE TO 删除了聚集索引，则将重新生成所有对基表的非聚集索引，但这些索引会保留在其原始文件组或分区方案中。 如果基表移动到其他文件组或分区方案中，这些非聚集索引不会通过移动来与基表（堆）的新位置一致。 因此，即使非聚集索引以前与聚集索引对齐，它们也可能不再与堆对齐。 有关已分区索引的详细信息，请参阅[已分区表和已分区索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。  
  
 _partition_scheme_name_ **(** _column_name_ **)**  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 指定分区方案作为生成表的位置。 该分区方案必须已通过执行 [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) 或 [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) 创建完毕。 如果未指定位置但表已分区，则表将包含在与现有聚集索引相同的分区方案中。  
  
 方案中的列名不限制为索引定义中的列。 可以指定基表中的任何列。  
  
 filegroup_name  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定文件组作为生成表的位置。 如果未指定位置并且表是未分区的，则生成的表将包含在与聚集索引相同的文件组中。 该文件组必须已存在。  
  
 "default"   
 指定生成表的默认位置。  
  
> [!NOTE]
>  在此上下文中，default 不是关键字。 它是默认文件组的标识符，必须对其进行分隔，就像在 MOVE TO "default" 或 MOVE TO [default] 中一样   。 如果指定了“default”，则必须将当前会话的 QUOTED_IDENTIFIER 选项设置为 ON 。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。  
  
 FILESTREAM_ON { partition_scheme_name | filestream_filegroup_name | "default" }    
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定一个位置，当前处于聚集索引叶级别的 FILESTREAM 表将移至此位置。 数据将以堆的形式移动到这一新位置。 可以将分区方案或文件组指定为新位置，但该分区方案或文件组必须已存在。 FILESTREAM ON 对于索引视图或非聚集索引无效。 如果未指定分区方案，则数据将位于为聚集索引定义的同一分区方案中。  
  
 partition_scheme_name  
 指定 FILESTREAM 数据的分区方案。 该分区方案必须已通过执行 [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) 或 [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) 创建完毕。 如果未指定位置但表已分区，则表将包含在与现有聚集索引相同的分区方案中。  
  
 如果为 MOVE TO 指定了分区方案，则必须为 FILESTREAM ON 使用同一个分区方案。  
  
 filestream_filegroup_name  
 指定 FILESTREAM 数据的 FILESTREAM 文件组。 如果未指定位置并且表未分区，则数据将包括在默认 FILESTREAM 文件组中。  
  
 "default"   
 指定 FILESTREAM 数据的默认位置。  
  
> [!NOTE]
>  在此上下文中，default 不是关键字。 它是默认文件组的标识符，必须对其进行分隔，就像在 MOVE TO "default" 或 MOVE TO [default] 中一样   。 如果指定了 "default"，则当前会话的 QUOTED_IDENTIFIER 选项必须为 ON。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 删除非聚集索引时，将从元数据中删除索引定义，并从数据库文件中删除索引数据页（B 树）。 删除聚集索引时，将从元数据中删除索引定义，并且存储于聚集索引叶级别的数据行将存储到生成的未排序表（堆）中。 将重新获得以前由索引占有的所有空间。 此后可将该空间用于任何数据库对象。  
  
 如果索引所在的文件组脱机或设置为只读，则不能删除该索引。  
  
 删除索引视图的聚集索引时，将自动删除同一视图的所有非聚集索引和自动创建的统计信息。 手动创建的统计信息不会删除。  
  
 保留 _table\_or\_view\_name_ **.** _index\_name_ 语法是为了保持后向兼容性。 XML 索引或空间索引无法使用向后兼容的语法删除。  
  
 删除带有 128 个或更多区数的索引时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将延迟实际页释放及其关联的锁，直到提交事务为止。  
  
 有时，需要删除并重新创建索引以重新组织或重新生成索引，例如在大容量加载之后应用新的填充因子值或重新组织数据。 若要执行该操作，使用 [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) 更为有效，尤其是对于聚集索引而言。 ALTER INDEX REBUILD 具有优化功能，可避免重新生成非聚集索引所造成的开销。  
  
## <a name="using-options-with-drop-index"></a>使用带选项的 DROP INDEX  
 删除聚集索引时，可以设置以下索引选项：MAXDOP、ONLINE 和 MOVE TO。  
  
 使用 MOVE TO 删除聚集索引并将生成表移动到一个事务中的另一个文件组或分区方案。  
  
 当指定 ONLINE = ON 时，DROP INDEX 事务不会阻塞对基础数据和关联非聚集索引的修改。 一次只能联机删除一个聚集索引。 有关 ONLINE 选项的完整说明，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
 如果聚集索引在视图上被禁用，或者它在叶级别数据行中包含 text、ntext、image、varchar(max)、nvarchar(max)、varbinary(max) 或 xml 列，则不能联机删除该索引      。  
  
 使用 ONLINE = ON 和 MOVE TO 选项要求附加的临时磁盘空间。  
  
 删除索引后，生成的堆将出现在 sys.indexes 目录视图中，且 name 列中为 NULL 。 若要查看表名，请将 sys.indexes 和 sys.tables 联接在 object_id 上  。 有关示例查询的信息，请参阅示例 D。  
  
 在运行 [!INCLUDE[ssEnterpriseEd2005](../../includes/ssenterpriseed2005-md.md)] 或更高版本的多处理器计算机中，DROP INDEX 可以使用更多处理器执行与删除聚集索引有关的扫描和排序操作，就像其他查询所做的一样。 可以通过指定 MAXDOP 索引选项手动配置用于运行 DROP INDEX 语句的处理器数目。 有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
 删除聚集索引时，除非修改了分区方案，否则相应的堆分区将保留其数据压缩设置。 如果分区方案已更改，则所有分区都将重新生成为未压缩状态 (DATA_COMPRESSION = NONE)。 若要删除聚集索引并更改分区方案，需要执行以下两个步骤：  
  
1.  删除聚集索引。  
  
2.  通过使用指定压缩选项的 ALTER TABLE ...REBUILD ... 选项来修改表。  
  
脱机删除聚集索引时，只删除较高级别的聚集索引；因此，该操作的执行速度很快。 如果联机删除聚集索引，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将重新生成堆两次，一次针对步骤 1，一次针对步骤 2。 有关数据压缩的详细信息，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。  
  
## <a name="xml-indexes"></a>XML 索引  
 删除 XML 索引时，不能指定选项。 此外，不能使用 _table\_or\_view\_name_ **.** _index\_name_ 语法。 删除主 XML 索引时，将自动删除所有关联的辅助 XML 索引。 有关详细信息，请参阅 [XML 索引 (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md)。  
  
## <a name="spatial-indexes"></a>空间索引  
 只有表支持空间索引。 删除空间索引时，无法指定任何选项或使用 **.** _index\_name_。 正确的语法如下：  
  
 DROP INDEX spatial_index_name ON spatial_table_name;   
  
 有关空间索引的详细信息，请参阅[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。  
  
## <a name="permissions"></a>权限  
 若要执行 DROP INDEX，至少需要对表或视图拥有 ALTER 权限。 默认情况下，将向 **sysadmin** 固定服务器角色以及 **db_ddladmin** 和 **db_owner** 固定数据库角色授予此权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-dropping-an-index"></a>A. 删除索引  
 以下示例删除 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `ProductVendor` 表上的索引 `IX_ProductVendor_VendorID`。  
  
```sql  
DROP INDEX IX_ProductVendor_BusinessEntityID   
    ON Purchasing.ProductVendor;  
GO  
```  
  
### <a name="b-dropping-multiple-indexes"></a>B. 删除多个索引  
 以下示例删除 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的单个事务中的两个索引。  
  
```sql  
DROP INDEX  
    IX_PurchaseOrderHeader_EmployeeID ON Purchasing.PurchaseOrderHeader,  
    IX_Address_StateProvinceID ON Person.Address;  
GO  
```  
  
### <a name="c-dropping-a-clustered-index-online-and-setting-the-maxdop-option"></a>C. 联机删除聚集索引并设置 MAXDOP 选项  
 以下示例在 `ONLINE` 选项设置为 `ON` 和 `MAXDOP` 设置为 `8` 的情况下删除聚集索引。 因为未指定 MOVE TO 选项，生成的表将与索引存储在相同的文件组中。 此示例使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
```sql  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials WITH (ONLINE = ON, MAXDOP = 2);  
GO  
```  
  
### <a name="d-dropping-a-clustered-index-online-and-moving-the-table-to-a-new-filegroup"></a>D. 联机删除聚集索引并将表移动到新文件组  
 以下示例使用 `NewGroup` 子句联机删除一个聚集索引并将生成表（堆）移动到文件组 `MOVE TO` 。 在移动之前和之后，将查询 `sys.indexes`、 `sys.tables`和 `sys.filegroups` 目录视图，以验证索引和表在文件组中的位置。 （从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，可使用 DROP INDEX IF EXISTS 语法。）  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql  
--Create a clustered index on the PRIMARY filegroup if the index does not exist.  
CREATE UNIQUE CLUSTERED INDEX  
    AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
        ON Production.BillOfMaterials (ProductAssemblyID, ComponentID,   
        StartDate)  
    ON 'PRIMARY';  
GO  
-- Verify filegroup location of the clustered index.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U')  
GO  
--Create filegroup NewGroup if it does not exist.  
IF NOT EXISTS (SELECT name FROM sys.filegroups  
                WHERE name = N'NewGroup')  
    BEGIN  
    ALTER DATABASE AdventureWorks2012  
        ADD FILEGROUP NewGroup;  
    ALTER DATABASE AdventureWorks2012  
        ADD FILE (NAME = File1,  
            FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\File1.ndf')  
        TO FILEGROUP NewGroup;  
    END  
GO  
--Verify new filegroup  
SELECT * from sys.filegroups;  
GO  
-- Drop the clustered index and move the BillOfMaterials table to  
-- the Newgroup filegroup.  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
DROP INDEX AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate   
    ON Production.BillOfMaterials   
    WITH (ONLINE = ON, MOVE TO NewGroup);  
GO  
-- Verify filegroup location of the moved table.  
SELECT t.name AS [Table Name], i.name AS [Index Name], i.type_desc,  
    i.data_space_id, f.name AS [Filegroup Name]  
FROM sys.indexes AS i  
    JOIN sys.filegroups AS f ON i.data_space_id = f.data_space_id  
    JOIN sys.tables as t ON i.object_id = t.object_id  
        AND i.object_id = OBJECT_ID(N'Production.BillOfMaterials','U');  
GO  
```  
  
### <a name="e-dropping-a-primary-key-constraint-online"></a>E. 联机删除 PRIMARY KEY 约束  
 在创建 PRIMARY KEY 或 UNIQUE 约束时创建的索引不能使用 DROP INDEX 来删除。 可以使用 ALTER TABLE DROP CONSTRAINT 语句将其删除。 有关详细信息，请参阅 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)。  
  
 以下示例通过删除 PRIMARY KEY 约束删除了具有该约束的聚集索引。 `ProductCostHistory` 表没有 FOREIGN KEY 约束。 如果具有此类约束，则必须首先将其删除。  
  
```sql  
-- Set ONLINE = OFF to execute this example on editions other than Enterprise Edition.  
ALTER TABLE Production.TransactionHistoryArchive  
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID  
WITH (ONLINE = ON);  
```  
  
### <a name="f-dropping-an-xml-index"></a>F. 删除 XML 索引  
 以下示例将删除 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `ProductModel` 表上的 XML 索引。  
  
```sql  
DROP INDEX PXML_ProductModel_CatalogDescription   
    ON Production.ProductModel;  
```  
  
### <a name="g-dropping-a-clustered-index-on-a-filestream-table"></a>G. 删除 FILESTREAM 表中的聚集索引  
 以下示例使用 `MyPartitionScheme` 子句和 `MOVE TO` 子句联机删除聚集索引并将生成的表（堆）和 FILESTREAM 数据移至 `FILESTREAM ON` 分区方案。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql  
DROP INDEX PK_MyClusteredIndex   
    ON dbo.MyTable   
    WITH (MOVE TO MyPartitionScheme,  
          FILESTREAM_ON MyPartitionScheme);  
GO  
```  

  
## <a name="see-also"></a>另请参阅  
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
 [ALTER PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)   
 [CREATE XML INDEX (Transact-SQL)](../../t-sql/statements/create-xml-index-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.filegroups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)  
