---
description: INDEXPROPERTY (Transact-SQL)
title: INDEXPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INDEXPROPERTY
- INDEXPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INDEXPROPERTY function
- indexes [SQL Server], viewing
- indexes [SQL Server], properties
ms.assetid: 998d5788-4871-44a8-8125-0d9390868b84
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: af0ad0bfc117c3e1278761ecd23ee35b3f5945c3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97422136"
---
# <a name="indexproperty-transact-sql"></a>INDEXPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  根据指定的表标识号、索引或统计信息名称以及属性名称，返回已命名的索引或统计信息属性值。 对于 XML 索引，返回 NULL。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
INDEXPROPERTY ( object_ID , index_or_statistics_name , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 object_ID  
 一个表达式，包含要为其提供索引属性信息的表或索引视图的对象标识号。 object_id 的数据类型为 int。  
  
 index_or_statistics_name  
 一个表达式，包含要为其返回属性信息的索引或统计信息的名称。 index_or_statistics_name 的数据类型为 nvarchar(128)。  
  
 *property*  
 一个表达式，包含要返回的数据库属性的名称。 property 的数据类型为 varchar(128)，它可以为以下值之一。  
  
> [!NOTE]  
>  除非另外注明，否则出现以下情况时将返回 NULL：property 不是有效的属性名称；object_ID 不是有效的对象 ID；object_ID 不是指定属性支持的对象类型；调用方无权查看对象的元数据。  
  
|属性|说明|值|  
|--------------|-----------------|-----------|  
|**IndexDepth**|索引的深度。|索引级别数。<br /><br /> NULL = XML 索引或输入无效。|  
|**IndexFillFactor**|创建索引或最后重新生成索引时使用的填充因子值。|填充因子|  
|**IndexID**|指定表或索引视图上索引的索引 ID。|索引 ID|  
|**IsAutoStatistics**|统计信息是由 ALTER DATABASE 的 AUTO_CREATE_STATISTICS 选项生成的。|1 = True<br /><br /> 0 = False 或 XML 索引。|  
|**IsClustered**|索引是聚集的。|1 = True<br /><br /> 0 = False 或 XML 索引。|  
|**IsDisabled**|索引被禁用。|1 = True<br /><br /> 0 = False<br /><br /> NULL = 输入无效。|  
|**IsFulltextKey**|索引是表的全文和语义索引键。|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 1 = True<br /><br /> 0 = False 或 XML 索引。<br /><br /> NULL = 输入无效。|  
|**IsHypothetical**|索引是假设的，不能直接用作数据访问路径。 假设索引包含列级统计信息，由数据库引擎优化顾问维护和使用。|1 = True<br /><br /> 0 = False 或 XML 索引<br /><br /> NULL = 输入无效。|  
|**IsPadIndex**|索引指定每个内部节点上将要保持空闲的空间。|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 1 = True<br /><br /> 0 = False 或 XML 索引。|  
|**IsPageLockDisallowed**|通过 ALTER INDEX 的 ALLOW_PAGE_LOCKS 选项设置的页锁定值。|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 1 = 不允许页锁定。<br /><br /> 0 = 允许页锁定。<br /><br /> NULL = 输入无效。|  
|**IsRowLockDisallowed**|通过 ALTER INDEX 的 ALLOW_ROW_LOCKS 选项设置的行锁定值。|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 1 = 不允许行锁定。<br /><br /> 0 = 允许行锁定。<br /><br /> NULL = 输入无效。|  
|**IsStatistics**|index_or_statistics_name 是通过 CREATE STATISTICS 语句或 ALTER DATABASE 的 AUTO_CREATE_STATISTICS 选项创建的统计信息。|1 = True<br /><br /> 0 = False 或 XML 索引。|  
|**IsUnique**|索引是唯一的。|1 = True<br /><br /> 0 = False 或 XML 索引。|  
|**IsColumnstore**|索引为 xVelocity 内存优化的列存储索引。|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 1 = True<br /><br /> 0 = False| 
|**IsOptimizedForSequentialKey**|索引是否已启用优化最后一页插入。|**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 及更高版本。 <br><br>1 = True<br><br>0 = False| 
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="exceptions"></a>例外  
 出现错误时或调用方没有查看对象的权限时，将返回 NULL。  
  
 用户只能查看符合如下条件的安全对象的元数据：该安全对象为该用户所有，或已授予该用户对该安全对象的权限。 也就是说，如果用户对该对象没有任何权限，则那些会生成元数据的内置函数（如 INDEXPROPERTY）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
 以下示例针对 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `Employee` 表的 `PK_Employee_BusinessEntityID` 索引返回 IsClustered、IndexDepth 和 IndexFillFactor 属性的值。  
  
```sql  
SELECT   
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IsClustered')AS [Is Clustered],  
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IndexDepth') AS [Index Depth],  
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IndexFillFactor') AS [Fill Factor];  
  
```  
  
 下面是结果集：  
  
```  
Is Clustered Index Depth Fill Factor   
------------ ----------- -----------   
1            2           0  
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例检查 `FactResellerSales` 表上某一索引的属性。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT   
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IsClustered') AS [Is Clustered],  
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IsColumnstore') AS [Is Columnstore Index],  
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IndexFillFactor') AS [Fill Factor];  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [统计信息](../../relational-databases/statistics/statistics.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  

