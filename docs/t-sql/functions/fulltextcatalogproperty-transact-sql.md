---
description: FULLTEXTCATALOGPROPERTY (Transact-SQL)
title: FULLTEXTCATALOGPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FULLTEXTCATALOGPROPERTY_TSQL
- FULLTEXTCATALOGPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], properties
- FULLTEXTCATALOGPROPERTY function
- status information [SQL Server], full-text catalogs
ms.assetid: f841dc79-2044-4863-aff0-56b8bb61f250
author: cawrites
ms.author: chadam
ms.openlocfilehash: 57e53c26b7f1797daf6a1ccc53f7578eabda734a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087228"
---
# <a name="fulltextcatalogproperty-transact-sql"></a>FULLTEXTCATALOGPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回有关 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中的全文目录属性的信息。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
FULLTEXTCATALOGPROPERTY ('catalog_name' ,'property')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
  
> [!NOTE]  
>  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中将删除以下属性：LogSize 和 PopulateStatus。 应避免在新的开发工作中使用这些属性，并着手修改当前使用上述任意属性的应用程序。  
  
_catalog\_name_  
包含全文目录名称的表达式。  
  
_property_  
包含全文目录属性名称的表达式。 下表列出了这些属性，并提供对返回的信息的说明。  
  
|属性|说明|  
|--------------|-----------------|  
|**AccentSensitivity**|区分重音设置。<br /><br /> 0 = 不区分重音<br /><br /> 1 = 区分重音|  
|**IndexSize**|全文目录的逻辑大小 (MB)。 包括语义关键字短语和文档相似性索引的大小。<br /><br /> 有关详细信息，请参阅本主题后面的“备注”。|  
|**ItemCount**|包括目录中所有全文索引、关键短语索引和文档相似性索引在内的索引项的数量|  
|**LogSize**|支持它仅仅是为了保持向后兼容。 始终返回 0。<br /><br /> 与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Search 服务全文目录关联的错误日志组合集的大小，以字节为单位。|  
|**MergeStatus**|指示是否正在进行主合并。<br /><br /> 0 = 未在进行主合并<br /><br /> 1 = 正在进行主合并|  
|**PopulateCompletionAge**|上一次全文索引填充的完成时间与 01/01/1990 00:00:00 之间的时间差（秒）。<br /><br /> 仅针对完全和增量爬网填充进行了更新。 如果未发生填充，则返回 0。|  
|**PopulateStatus**|0 = 空闲<br /><br /> 1 = 正在进行完全填充<br /><br /> 2 = 已暂停<br /><br /> 3 = 已中止<br /><br /> 4 = Recovering<br /><br /> 5 = 关闭<br /><br /> 6 = 正在进行增量填充<br /><br /> 7 = 正在生成索引<br /><br /> 8 = 磁盘已满。 已暂停。<br /><br /> 9 = 更改跟踪|  
|**UniqueKeyCount**|全文目录中的唯一键数。|  
|**ImportStatus**|指示是否正在导入全文目录。<br /><br /> 0 = 未在导入全文目录。<br /><br /> 1 = 将导入全文目录。|  
  
## <a name="return-types"></a>返回类型  
**int**  
  
## <a name="exceptions"></a>例外  
出现错误时或调用方没有查看对象的权限时将返回 NULL。  
  
在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中，用户只能查看安全对象的元数据。 这些安全对象为该用户所有，或已授予该用户对这些安全对象的权限。 因此，如果用户对该对象没有任何权限，则发出元数据的内置函数（如 FULLTEXTCATALOGPROPERTY）可能会返回 NULL。 有关详细信息，请参阅 [sp_help_fulltext_catalogs (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
FULLTEXTCATALOGPROPERTY ('_catalog\_name_','**IndexSize**') 只查看状态为 4 或 6 的片段，如 [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md) 中所示。 这些片段是逻辑索引的一部分。 因此，**IndexSize** 属性仅返回逻辑索引大小。 

但是，在索引合并期间，实际的索引大小可能会两倍于其逻辑大小。 若要找出合并期间将由全文索引使用的实际大小，请使用 [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) 系统存储过程。 该过程查看与全文索引关联的所有片段。 

全文填充可能会失败。 如果限制全文目录文件的增长，且没有足够的空间用于合并进程，则它可能会失败。 在这种情况下，FULLTEXTCATALOGPROPERTY ('_catalog\_name_','**IndexSize**') 返回 0，并将以下错误写入全文日志：  
  
`Error: 30059, Severity: 16, State: 1. A fatal error occurred during a full-text population and caused the population to be cancelled. Population type is: FULL; database name is FTS_Test (id: 13); catalog name is t1_cat (id: 5); table name t1 (id: 2105058535). Fix the errors that are logged in the full-text crawl log. Then, resume the population. The basic Transact-SQL syntax for this is: ALTER FULLTEXT INDEX ON table_name RESUME POPULATION.`  
  
应用程序不应在紧凑循环中等待，检查 **PopulateStatus** 属性是否进入空闲状态。 进入空闲状态指示填充已完成。 此检查将使 CPU 周期背离数据库和全文搜索进程，并导致超时。 一般最好使用 OBJECTPROPERTYEX 系统函数检查对应的表级 **PopulateStatus** 属性，即 **TableFullTextPopulateStatus**。 此属性以及 OBJECTPROPERTYEX 中的其他新的全文属性可以提供有关全文索引表的更详尽的信息。 有关详细信息，请参阅 [OBJECTPROPERTYEX (Transact-SQL) ](../../t-sql/functions/objectpropertyex-transact-sql.md)。  
  
## <a name="examples"></a>示例  
以下示例将返回名为 `Cat_Desc` 的全文目录中的全文索引项数目。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT fulltextcatalogproperty('Cat_Desc', 'ItemCount');  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[FULLTEXTSERVICEPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
[元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
[sp_help_fulltext_catalogs (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
  
  
