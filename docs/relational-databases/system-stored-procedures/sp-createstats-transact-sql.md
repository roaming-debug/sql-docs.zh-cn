---
description: sp_createstats (Transact-SQL)
title: sp_createstats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_createstats_TSQL
- sp_createstats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_createstats
ms.assetid: 8204f6f2-5704-40a7-8d51-43fc832eeb54
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7af34bd1bbe065012b18826f7edaec31940d1e50
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466868"
---
# <a name="sp_createstats-transact-sql"></a>sp_createstats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  调用 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句，以便对统计信息对象中尚未处于第一列的列创建单列统计信息。 创建单列统计信息会增加直方图的数目，这可能会改进基数估计、查询计划和查询性能。 统计信息对象的第一列具有直方图；其他列没有直方图。  
  
 在查询执行时间很重要并且不能等待查询优化器以生成单列统计信息时，sp_createstats 对于基准确定之类的应用程序十分有用。 在大多数情况下，无需使用 sp_createstats;查询优化器根据需要生成单列统计信息，以便在 **AUTO_CREATE_STATISTICS** 选项打开时改进查询计划。  
  
 有关统计信息的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。 有关生成单列统计信息的详细信息，请参阅 [ALTER DATABASE SET Options 中 &#40;transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)的 **AUTO_CREATE_STATISTICS** 选项。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_createstats   
    [   [ @indexonly =   ] { 'indexonly'   | 'NO' } ]   
    [ , [ @fullscan =    ] { 'fullscan'    | 'NO' } ]   
    [ , [ @norecompute = ] { 'norecompute' | 'NO' } ]  
    [ , [ @incremental = ] { 'incremental' | 'NO' } ]  
```  
  
## <a name="arguments"></a>自变量  
`[ @indexonly = ] 'indexonly'` 仅对现有索引中的列创建统计信息，而不是任何索引定义中的第一列。 **indexonly** 为 **char (9)**。 默认值为 NO。  
  
`[ @fullscan = ] 'fullscan'` 将 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句与 **FULLSCAN** 选项一起使用。 **fullscan** 为 **char (9)**。  默认值为 NO。  
  
`[ @norecompute = ] 'norecompute'` 将 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) 语句与 **NORECOMPUTE** 选项一起使用。 **norecompute** 为 **char (12)**。  默认值为 NO。  
  
`[ @incremental = ] 'incremental'`使用带有 **增量 = ON** 选项的 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)语句。 **增量** 为 **char (12)**。  默认值为 NO。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 每个新统计信息对象的名称与创建该统计信息的列相同。  
  
## <a name="remarks"></a>备注  
 sp_createstats 不会创建或更新作为现有统计信息对象中第一列的列的统计信息; 这包括为索引创建的统计信息的第一列、带有用 AUTO_CREATE_STATISTICS 选项生成的单列统计信息的列以及用 CREATE STATISTICS 语句创建的统计信息的第一列。 sp_createstats 不会在已禁用索引的第一列上创建统计信息，除非该列用于另一个已启用索引。 sp_createstats 不会在具有已禁用的聚集索引的表上创建统计信息。  
  
 当表包含列集时，sp_createstats 不会在稀疏列上创建统计信息。 有关列集和稀疏列的详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md) 和 [使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。  
  
## <a name="permissions"></a>权限  
 要求具有 db_owner 固定数据库角色中的成员资格。  
  
## <a name="examples"></a>示例  
  
### <a name="a-create-single-column-statistics-on-all-eligible-columns"></a>A. 为所有适于统计的列创建单列统计信息  
 下面的示例将为当前数据库中所有适于统计的列创建单列统计信息。  
  
```  
EXEC sp_createstats;  
GO  
```  
  
### <a name="b-create-single-column-statistics-on-all-eligible-index-columns"></a>B. 对所有适于统计的索引列创建单列统计信息  
 下面的示例将为已处于索引中且不是索引中的第一列的所有适于统计的列创建单列统计信息。  
  
```  
EXEC sp_createstats 'indexonly';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [统计信息](../../relational-databases/statistics/statistics.md)   
 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)   
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
