---
description: CHANGETABLE (Transact-SQL)
title: CHANGETABLE (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CHANGETABLE_TSQL
- CHANGETABLE
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGETABLE
- change tracking [SQL Server], CHANGETABLE
ms.assetid: d405fb8d-3b02-4327-8d45-f643df7f501a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d3d4e72681f16689c6241c8f9d7b35119d898722
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196133"
---
# <a name="changetable-transact-sql"></a>CHANGETABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回表的更改跟踪信息。可使用此语句返回表的所有更改或特定行的更改跟踪信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql
CHANGETABLE (  
    { CHANGES table , last_sync_version  
    | VERSION table , <primary_key_values> } )  
[AS] table_alias [ ( column_alias [ ,...n ] )  
  
<primary_key_values> ::=  
( column_name [ , ...n ] ) , ( value [ , ...n ] )  
```  
  
## <a name="arguments"></a>参数  
 更改 *表* ， *last_sync_version*  
 返回自 *last_sync_version* 指定的版本以来对某个表进行的所有更改的跟踪信息。  
  
 *table*  
 是要从中获取跟踪的更改的用户定义表。 必须对表启用更改跟踪。 可以使用由一部分、两部分、三部分或四部分构成的表名。 表名可以是表的同义词。  
  
 *last_sync_version*  
 获取更改时，调用应用程序必须指定所需更改的起始点。 last_sync_version 指定该起始点。 该函数返回在该版本之后更改的所有行的信息。 应用程序将查询以接收版本大于 last_sync_version 的更改。  
  
 通常情况下，在获取更改之前，应用程序将调用 **CHANGE_TRACKING_CURRENT_VERSION ( # B1** 获取将在下次需要更改时使用的版本。 因此，该应用程序不必解释或了解实际值。  
  
 由于 last_sync_version 是由调用应用程序获取的，因此该应用程序必须保持该值。 如果应用程序丢失该值，则需要重新初始化数据。  
  
 *last_sync_version* 为 **bigint**。 值必须是标量。 表达式将导致语法错误。  
  
 如果值为 NULL，则返回所有跟踪的更改。  
  
 应该验证 *last_sync_version* ，以确保它不太旧，因为某些或全部更改信息可能已根据为数据库配置的保留期进行清理。 有关详细信息，请参阅 [CHANGE_TRACKING_MIN_VALID_VERSION&#41;transact-sql&#41;&#40;transact-sql ](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md) 和 [ALTER Database SET &#40;选项 ](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
 版本 *表*{<primary_key_values>}  
 返回指定行的最新更改跟踪信息。 主键值必须标识该行。 <primary_key_values> 标识主键列并指定值。 可以按任意顺序指定主键列名称。  
  
 *表*  
 是用户定义的从中获取更改跟踪信息的表。 必须对表启用更改跟踪。 可以使用由一部分、两部分、三部分或四部分构成的表名。 表名可以是表的同义词。  
  
 column_name  
 指定一个或多个主键列的名称。 可以按任意顺序指定多个列名。  
  
 值  
 是主键的值。 如果有多个主键列，则这些值的指定顺序必须与列在 *column_name* 列表中的显示顺序相同。  
  
 方式 *table_alias* [ (*column_alias* [,.。。*n* ] ) ]  
 为 CHANGETABLE 返回的结果提供名称。  
  
 table_alias  
 是 CHANGETABLE 返回的表的别名。 *table_alias* 是必需的，并且必须是有效的 [标识符](../../relational-databases/databases/database-identifiers.md)。  
  
 column_alias  
 是由 CHANGETABLE 返回的列的可选列别名或列别名列表。 这使得在结果中存在重复的名称时可以自定义列名。  
  
## <a name="return-types"></a>返回类型  
 **table**  
  
## <a name="return-values"></a>返回值  
  
### <a name="changetable-changes"></a>CHANGETABLE CHANGES  
 指定了 CHANGES 时，返回零个或多个具有以下列的行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|SYS_CHANGE_VERSION|**bigint**|与上次对行的更改关联的版本值|  
|SYS_CHANGE_CREATION_VERSION|**bigint**|与上次插入操作关联的版本值。|  
|SYS_CHANGE_OPERATION|**nchar(1)**|指定更改的类型：<br /><br /> **U** = 更新<br /><br /> **I** = 插入<br /><br /> **D** = 删除|  
|SYS_CHANGE_COLUMNS|**varbinary(4100)**|列出自 last_sync_version（基准版本）以后发生了更改的列。 请注意，计算列永远不会列出为已更改。<br /><br /> 以下任何一个条件为真时，值为 NULL：<br /><br /> 未启用列更改跟踪。<br /><br /> 操作是插入操作或删除操作。<br /><br /> 在一个操作中更新了所有非主键列。 不应直接解释此二进制值。 相反，若要对其进行解释，请使用 [CHANGE_TRACKING_IS_COLUMN_IN_MASK ( # B1 ](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md)。|  
|SYS_CHANGE_CONTEXT|**varbinary(128)**|可以通过使用 [WITH](../../relational-databases/system-functions/with-change-tracking-context-transact-sql.md) 子句作为 INSERT、UPDATE 或 DELETE 语句的一部分来更改上下文信息。|  
|\<primary key column value>|与用户表列相同|被跟踪表的主键值。 这些值在用户表中唯一标识各行。|  
  
### <a name="changetable-version"></a>CHANGETABLE VERSION  
 指定 VERSION 后，返回一个具有以下列的行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|SYS_CHANGE_VERSION|**bigint**|与行关联的当前更改版本值。<br /><br /> 如果在超过更改跟踪保留期的时段内没有进行更改，或者在启用更改跟踪之后未更改行，则值为 NULL。|  
|SYS_CHANGE_CONTEXT|**varbinary(128)**|更改可以在 INSERT、UPDATE 或 DELETE 语句中使用 WITH 子句选择指定的上下文信息。|  
|\<primary key column value>|与用户表列相同|被跟踪表的主键值。 这些值在用户表中唯一标识各行。|  
  
## <a name="remarks"></a>备注  
 CHANGETABLE 函数通常在对表的查询的 FROM 子句中使用。  
  
## <a name="changetablechanges"></a>CHANGETABLE(CHANGES...)  
 要获取新行或修改过的行的行数据，请使用主键列将结果集与用户表联接起来。 对于用户表中已更改的每一行，只返回一行，即使在 *last_sync_version* 值后对同一行进行了多次更改。  
  
 永远不将对主键列的更改标记为更新。 如果主键值更改，则会视作删除旧值并插入新值。  
  
 如果删除一行，然后插入具有旧的主键值的行，则将更改视作对行中所有列的更新。  
  
 为 SYS_CHANGE_OPERATION 和 SYS_CHANGE_COLUMNS 列返回的值相对于指定的基线 (last_sync_version) 。 例如，如果在版本10和版本15的更新操作中进行插入操作，并且基线 *last_sync_version* 为12，则会报告更新。 如果 *last_sync_version* 值为8，则将报告 insert。 SYS_CHANGE_COLUMNS 永远不会报告计算列，因为列已更新。  
  
 通常，将跟踪在用户表中插入、更新或删除数据的所有操作，其中包括 MERGE 语句。  
  
 不会跟踪影响用户表数据的以下操作：  
  
-   执行 UPDATETEXT 语句  
  
     不推荐使用此语句，以后的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中将删除它。 但是，将跟踪使用 UPDATE 语句的 .WRITE 子句进行的更改。  
  
-   使用 TRUNCATE TABLE 删除行  
  
     某个表被截断时，将会重置与该表关联的更改跟踪版本信息，就像刚刚对该表启用了更改跟踪一样。 客户端应用程序应始终验证其上一次同步的版本。 如果表已截断，验证将会失败。  
  
## <a name="changetableversion"></a>CHANGETABLE(VERSION...)  
 如果指定的主键不存在，则会返回空结果集。  
  
 如果未进行更改的时间超过保持期（例如，清除操作删除了更改信息），或者在为表启用更改跟踪后从未更改行，则 SYS_CHANGE_VERSION 的值可能为 NULL。  
  
## <a name="permissions"></a>权限  
 需要对 *表* 值指定的表具有以下权限才能获取更改跟踪信息：  
  
-   主键列的 SELECT 权限  
  
-   VIEW CHANGE TRACKING  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-rows-for-an-initial-synchronization-of-data"></a>A. 返回数据初始同步的行  
 下面的示例演示了如何获取表数据的初始同步的数据。 该查询返回所有行数据及其关联的版本。 您可以随后将此数据插入或添加到要包含同步数据的系统中。  
  
```sql  
-- Get all current rows with associated version  
SELECT e.[Emp ID], e.SSN, e.FirstName, e.LastName,  
    c.SYS_CHANGE_VERSION, c.SYS_CHANGE_CONTEXT  
FROM Employees AS e  
CROSS APPLY CHANGETABLE   
    (VERSION Employees, ([Emp ID], SSN), (e.[Emp ID], e.SSN)) AS c;  
```  
  
### <a name="b-listing-all-changes-that-were-made-since-a-specific-version"></a>B. 列出自特定版本起进行的所有更改  
 下面的示例列出了从指定的版本 (`@last_sync_version)`) 起在表中进行的所有更改。 [Emp ID] 和 SSN 是组合主键中的列。  
  
```sql  
DECLARE @last_sync_version bigint;  
SET @last_sync_version = <value obtained from query>;  
SELECT [Emp ID], SSN,  
    SYS_CHANGE_VERSION, SYS_CHANGE_OPERATION,  
    SYS_CHANGE_COLUMNS, SYS_CHANGE_CONTEXT   
FROM CHANGETABLE (CHANGES Employees, @last_sync_version) AS C;  
```  
  
### <a name="c-obtaining-all-changed-data-for-a-synchronization"></a>C. 获取同步的所有已更改数据  
 下面的示例演示如何获取所有已更改数据。 此查询将更改跟踪信息与用户表联接，以便返回用户表信息。 使用 `LEFT OUTER JOIN` 以便为删除的行返回一行。  
  
```sql  
-- Get all changes (inserts, updates, deletes)  
DECLARE @last_sync_version bigint;  
SET @last_sync_version = <value obtained from query>;  
SELECT e.FirstName, e.LastName, c.[Emp ID], c.SSN,  
    c.SYS_CHANGE_VERSION, c.SYS_CHANGE_OPERATION,  
    c.SYS_CHANGE_COLUMNS, c.SYS_CHANGE_CONTEXT   
FROM CHANGETABLE (CHANGES Employees, @last_sync_version) AS c  
    LEFT OUTER JOIN Employees AS e  
        ON e.[Emp ID] = c.[Emp ID] AND e.SSN = c.SSN;  
```  
  
### <a name="d-detecting-conflicts-by-using-changetableversion"></a>D. 使用 CHANGETABLE(VERSION...) 检测冲突  
 下面的示例演示了如何只更新在上次同步之后未更改的行。 使用 `CHANGETABLE` 获取特定行的版本号。 如果行已经更新，则不进行更改，并且查询将返回有关对行的最近更改的信息。  
  
```sql  
-- @last_sync_version must be set to a valid value  
UPDATE  
    SalesLT.Product  
SET  
    ListPrice = @new_listprice  
FROM  
    SalesLT.Product AS P  
WHERE  
    ProductID = @product_id AND  
    @last_sync_version >= ISNULL (  
        (SELECT CT.SYS_CHANGE_VERSION FROM   
            CHANGETABLE(VERSION SalesLT.Product,  
            (ProductID), (P.ProductID)) AS CT),  
        0);  
```  
  
## <a name="see-also"></a>另请参阅  
 [变更跟踪函数 (Transact-SQL)](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [跟踪数据更改 (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [CHANGE_TRACKING_IS_COLUMN_IN_MASK &#40;Transact-sql&#41;](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md)   
 [CHANGE_TRACKING_CURRENT_VERSION (Transact-SQL)](../../relational-databases/system-functions/change-tracking-current-version-transact-sql.md)   
 [CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)  
  
  
