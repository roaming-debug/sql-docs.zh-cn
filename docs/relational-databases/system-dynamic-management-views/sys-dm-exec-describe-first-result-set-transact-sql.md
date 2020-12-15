---
title: sys.dm_exec_describe_first_result_set (Transact-SQL)
description: sys.dm_exec_describe_first_result_set (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_describe_first_result_set
- sys.dm_exec_describe_first_result_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_describe_first_result_set catalog view
ms.assetid: 6ea88346-0bdb-4f0e-9f1f-4d85e3487d23
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 06/10/2016
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 21b823aeaa319263a3e90a3ddd917e9605506c0c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472858"
---
# <a name="sysdm_exec_describe_first_result_set-transact-sql"></a>sys.dm_exec_describe_first_result_set (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

此动态管理函数将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句作为参数，并描述该语句的第一个结果集的元数据。  
  
 **sys.dm_exec_describe_first_result_set** 具有与 [sys.dm_exec_describe_first_result_set_for_object &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md) 相同的结果集定义，并且类似于 sp_describe_first_result_set &#40;[transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)。  
  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.dm_exec_describe_first_result_set(@tsql, @params, @include_browse_information)  
```  
  
## <a name="arguments"></a>自变量  
 *\@tsql*  
 一个或多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 *SQL_batch* 可以是 **nvarchar (** _n_ *_)_* 或 **nvarchar (max)**。  
  
 *\@params*  
 \@params 为批处理参数提供声明字符串 [!INCLUDE[tsql](../../includes/tsql-md.md)] ，类似于 sp_executesql。 参数可以为 **nvarchar (n)** 或 **nvarchar (max)**。  
  
 一个字符串，其中包含已嵌入到 _batch 中的所有参数的定义 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 字符串必须是 Unicode 常量或 Unicode 变量。 每个参数定义由参数名称和数据类型组成。 *n* 是表示附加参数定义的占位符。 必须在参数中定义 stmt 中指定的每个参数 \@ 。 如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中的语句或批处理不包含参数， \@ 则无需参数。 该参数的默认值为 NULL。  
  
 *\@include_browse_information*  
 如果设置为 1，则分析每个查询，就好像它在查询中使用 FOR BROWSE 选项。 将返回其他键列和源表信息。  
  
## <a name="table-returned"></a>返回的表  
 此通用元数据是作为结果集返回的。 结果元数据的每一列中的行使用下表所示的格式描述列类型和为空性。 如果对于每个控制路径不存在第一个语句，则返回的结果集不包含任何行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit**|指定列是出于浏览和参考目的而额外添加的列，并不会实际出现在结果集中。|  
|**column_ordinal**|**int**|在结果集中包含列的序号位置。 第一列的位置将指定为 1。|  
|name|**sysname**|包含列的名称（如果可以确定名称）。 否则，将包含 NULL。|  
|**is_nullable**|**bit**|包含下列值：<br /><br /> 如果列允许使用 NULL，则值为 1。<br /><br /> 如果列不允许使用 NULL，则值为 0。<br /><br /> 如果无法确定列是否允许使用 NULL，则值为 1。|  
|**system_type_id**|**int**|包含 sys.databases 中指定的列数据类型的 system_type_id。 对于 CLR 类型，即使 system_type_name 列返回 NULL，该列也会返回值 240。|  
|**system_type_name**|**nvarchar(256)**|包含为列数据类型指定的名称和参数（例如，length、precision、scale）。<br /><br /> 如果数据类型是用户定义的别名类型，则会在此处指定基本系统类型。<br /><br /> 如果数据类型是 CLR 用户定义类型，则在此列中返回 NULL。|  
|**max_length**|**smallint**|列的最大长度（字节）。<br /><br /> -1 = 列数据类型为 **varchar (max)**、 **nvarchar (max)**、 **varbinary (max)** 或 **xml**。<br /><br /> 对于 **text** 列， **max_length** 值将是16，或者是 **sp_tableoption "text in row"** 设置的值。|  
|**精度**|**tinyint**|如果为基于数值的列，则为该列的精度。 否则，返回 0。|  
|**scale**|**tinyint**|如果基于数值，则为列的小数位数。 否则，返回 0。|  
|**collation_name**|**sysname**|如果列包含的是字符，则为该列的排序规则的名称。 否则，返回 NULL。|  
|**user_type_id**|**int**|对于 CLR 和别名类型，包含在 sys.types 中指定的列数据类型的 user_type_id。 否则为 NULL。|  
|**user_type_database**|**sysname**|对于 CLR 和别名类型，包含在其中定义相应类型的数据库的名称。 否则为 NULL。|  
|**user_type_schema**|**sysname**|对于 CLR 和别名类型，包含在其中定义相应类型的架构的名称。 否则为 NULL。|  
|**user_type_name**|**sysname**|对于 CLR 和别名类型，包含类型的名称。 否则为 NULL。|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|对于 CLR 类型，返回定义类型的程序集和类的名称。 否则为 NULL。|  
|**xml_collection_id**|**int**|包含 sys.columns 中指定的列数据类型的 xml_collection_id。 如果返回的类型与 XML 架构集合不关联，则该列返回 NULL。|  
|**xml_collection_database**|**sysname**|包含定义与此类型关联的 XML 架构集合的数据库。 如果返回的类型与 XML 架构集合不关联，则该列返回 NULL。|  
|**xml_collection_schema**|**sysname**|包含定义与此类型关联的 XML 架构集合的架构。 如果返回的类型与 XML 架构集合不关联，则该列返回 NULL。|  
|**xml_collection_name**|**sysname**|包含与此类型关联的 XML 架构集合的名称。 如果返回的类型与 XML 架构集合不关联，则该列返回 NULL。|  
|**is_xml_document**|**bit**|如果返回的数据类型为 XML，并且保证该类型是完整的 XML 文档（包含根节点，与 XML 片段相对），则返回 1。 否则，返回 0。|  
|**is_case_sensitive**|**bit**|如果列具有区分大小写的字符串类型，则返回 1。 反之，则返回 0。|  
|**is_fixed_length_clr_type**|**bit**|如果列具有固定长度 CLR 类型，则返回 1。 反之，则返回 0。|  
|**source_server**|**sysname**|源服务器（如果它来自远程服务器）的名称。 该名称在 sys.databases 中出现。 如果列来自本地服务器或无法确定它来自的服务器，则返回 NULL。 仅在请求浏览信息时填充它。|  
|**source_database**|**sysname**|此结果中的列返回的源数据库的名称。 如果无法确定该数据库，则返回 NULL。 仅在请求浏览信息时填充它。|  
|**source_schema**|**sysname**|此结果中的列返回的源架构的名称。 如果无法确定该架构，则返回 NULL。 仅在请求浏览信息时填充它。|  
|**source_table**|**sysname**|此结果中的列返回的源表的名称。 如果无法确定该表，则返回 NULL。 仅在请求浏览信息时填充它。|  
|**source_column**|**sysname**|结果列返回的源列的名称。 如果无法确定该列，则返回 NULL。 仅在请求浏览信息时填充它。|  
|**is_identity_column**|**bit**|如果列是标识列，则返回 1；否则，返回 0。 如果无法确定列是否为标识列，则返回 NULL。|  
|**is_part_of_unique_key**|**bit**|如果列是唯一索引的一部分（包括唯一约束和主约束），则返回 1；否则，返回 0。 如果无法确定列是否为唯一索引的一部分，则返回 NULL。 仅在请求浏览信息时填充它。|  
|**is_updateable**|**bit**|如果可以更新列，则返回 1；否则，返回 0。 如果无法确定是否可以更新列，则返回 NULL。|  
|**is_computed_column**|**bit**|如果列是计算列，则返回 1；否则，返回 0。 如果无法确定列是否为计算列，则返回 NULL。|  
|**is_sparse_column_set**|**bit**|如果列是稀疏列，则返回 1；否则，返回 0。 如果无法确定列是否为稀疏列集的一部分，则返回 NULL。|  
|**ordinal_in_order_by_list**|**smallint**|此列在 ORDER BY 列表中的位置。 如果在 ORDER BY 列表中不显示该列或无法唯一确定 ORDER BY 列表，则返回 NULL。|  
|**order_by_list_length**|**smallint**|ORDER BY 列表的长度。 如果没有 ORDER BY 列表，或者无法唯一确定 ORDER BY 列表，则返回 NULL。 请注意，对于 sp_describe_first_result_set 返回的所有行，该值是相同的。|  
|**order_by_is_descending**|**smallint NULL**|如果 ordinal_in_order_by_list 不为 NULL，则 **order_by_is_descending** 列报告此列的 ORDER BY 子句的方向。 否则，它报告 NULL。|  
|error_number|**int**|包含函数返回的错误号。 如果未发生错误，则该列包含 NULL。|  
|**error_severity**|**int**|包含函数返回的严重性。 如果未发生错误，则该列包含 NULL。|  
|**error_state**|**int**|包含函数返回的 状态消息。 如果未发生错误，则该列包含 NULL。|  
|**error_message**|**nvarchar (4096)**|包含函数返回的消息。 如果未发生错误，则该列包含 NULL。|  
|**error_type**|**int**|包含一个整数，它表示返回的错误。 映射到 error_type_desc。 请参阅“备注”中的列表。|  
|**error_type_desc**|**nvarchar(60)**|包含一个简短的大写字符串，它表示返回的错误。 映射到 error_type。 请参阅“备注”中的列表。|  
  
## <a name="remarks"></a>备注  
 此函数使用与 **sp_describe_first_result_set** 相同的算法。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_describe_first_result_set ](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)。  
  
 下表列出了错误类型及其说明。  
  
|错误类型|错误类型|说明|  
|-----------------|-----------------|-----------------|  
|1|MISC|未描述的所有错误。|  
|2|SYNTAX|在批处理中发生语法错误。|  
|3|CONFLICTING_RESULTS|由于两个可能的第一个语句之间发生冲突，无法确定结果。|  
|4|DYNAMIC_SQL|由于动态 SQL 可能会返回第一个结果，无法确定结果。|  
|5|CLR_PROCEDURE|由于 CLR 存储过程可能会返回第一个结果，无法确定结果。|  
|6|CLR_TRIGGER|由于 CLR 触发器可能会返回第一个结果，无法确定结果。|  
|7|EXTENDED_PROCEDURE|由于扩展存储过程可能会返回第一个结果，无法确定结果。|  
|8|UNDECLARED_PARAMETER|无法确定结果，因为一个或多个结果集的列的数据类型可能取决于未声明的参数。|  
|9|RECURSION|由于批处理包含递归语句，无法确定结果。|  
|10|TEMPORARY_TABLE|由于批处理包含临时表且 **sp_describe_first_result_set** 不支持该临时表，无法确定结果。|  
|11|UNSUPPORTED_STATEMENT|由于批处理包含 **sp_describe_first_result_set** 不支持的语句（例如，FETCH、REVERT 等），无法确定结果。|  
|12|OBJECT_TYPE_NOT_SUPPORTED|\@不支持传递给函数的 object_id (（即不是存储过程）) |  
|13|OBJECT_DOES_NOT_EXIST|\@在系统目录中找不到传递给函数的 object_id。|  
  
## <a name="permissions"></a>权限  
 要求具有执行 \@ tsql 参数的权限。  
  
## <a name="examples"></a>示例  
 本主题中的其他示例 [&#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) 可以改编为使用 **sys.dm_exec_describe_first_result_set** sp_describe_first_result_set。  
  
### <a name="a-returning-information-about-a-single-transact-sql-statement"></a>A. 返回有关单个 Transact-SQL 语句的信息  
 以下代码返回有关 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句结果的信息。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_exec_describe_first_result_set  
(N'SELECT object_id, name, type_desc FROM sys.indexes', null, 0) ;  
```  
  
### <a name="b-returning-information-about-a-procedure"></a>B. 返回有关过程的信息  
 下面的示例创建一个名为 pr_TestProc 的存储过程，该存储过程返回两个结果集。 然后，该示例说明 **sys.dm_exec_describe_first_result_set** 返回有关过程中的第一个结果集的信息。  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE PROC Production.TestProc  
AS  
SELECT Name, ProductID, Color FROM Production.Product ;  
SELECT Name, SafetyStockLevel, SellStartDate FROM Production.Product ;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set  
('Production.TestProc', NULL, 0) ;  
```  
  
### <a name="c-returning-metadata-from-a-batch-that-contains-multiple-statements"></a>C. 从包含多个语句的批处理中返回元数据  
 以下示例计算包含两个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的批处理。 该结果集描述返回的第一个结果集。  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set(  
N'SELECT CustomerID, TerritoryID, AccountNumber FROM Sales.Customer WHERE CustomerID = @CustomerID;  
SELECT * FROM Sales.SalesOrderHeader;',  
N'@CustomerID int', 0) AS a;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_describe_first_result_set &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sp_describe_undeclared_parameters &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)  
  
  
