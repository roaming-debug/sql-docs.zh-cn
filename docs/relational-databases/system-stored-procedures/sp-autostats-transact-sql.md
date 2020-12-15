---
description: sp_autostats (Transact-SQL)
title: sp_autostats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_autostats_TSQL
- sp_autostats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_autostats
ms.assetid: d1df8c15-ee73-49eb-9d13-6e98943c3e38
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b3ccfa642b98165dcbdad57adac38f300063ccdb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462758"
---
# <a name="sp_autostats-transact-sql"></a>sp_autostats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  显示或更改索引、统计信息对象、表或索引视图的自动统计信息更新选项 AUTO_UPDATE_STATISTICS。  
  
 有关 AUTO_UPDATE_STATISTICS 选项的详细信息，请参阅[transact-sql&#41;和 STATISTICS &#40;ALTER DATABASE SET 选项](../../t-sql/statements/alter-database-transact-sql-set-options.md)。 [](../../relational-databases/statistics/statistics.md)  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_autostats [ @tblname = ] 'table_or_indexed_view_name'   
    [ , [ @flagc = ] 'stats_flag' ]   
    [ , [ @indname = ] 'statistics_name' ]  
```  
  
## <a name="arguments"></a>自变量  
`[ @tblname = ] 'table_or_indexed_view_name'` 要在其上显示 AUTO_UPDATE_STATISTICS 选项的表或索引视图的名称。 *table_or_indexed_view_name* 为 **nvarchar (776)**，无默认值。  
  
`[ @flagc = ] 'stats_flag'` 将 AUTO_UPDATE_STATISTICS 选项更新为以下值之一：  
  
 **开启** = 打开  
  
 **关** = 关闭  
  
 如果未指定 *stats_flag* ，则显示当前 AUTO_UPDATE_STATISTICS 设置。 *stats_flag* 是 **varchar (10)**，默认值为 NULL。  
  
`[ @indname = ] 'statistics_name'` 显示或更新 AUTO_UPDATE_STATISTICS 选项的统计信息的名称。 若要显示索引的统计信息，您可以使用索引的名称；索引及其相应统计信息对象具有相同的名称。  
  
 *statistics_name* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 如果指定了 *stats_flag* ， **sp_autostats** 会报告已执行但未返回结果集的操作。  
  
 如果未指定 *stats_flag* ， **sp_autostats** 将返回以下结果集。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**Index Name**|**sysname**|索引或统计信息的名称。|  
|**AUTOSTATS**|**varchar (3)**|AUTO_UPDATE_STATISTICS 选项的当前值。|  
|**上次更新时间**|**datetime**|最近更新统计信息的日期。|  
  
 表或索引视图的结果集包括为索引创建的统计信息、使用 AUTO_CREATE_STATISTICS 选项生成的单列统计信息以及使用 [CREATE statistics](../../t-sql/statements/create-statistics-transact-sql.md) 语句创建的统计信息。  
  
## <a name="remarks"></a>备注  
 如果禁用了指定的索引，或者指定的表具有被禁用的聚集索引，将显示错误消息。  
  
 对于内存优化表，AUTO_UPDATE_STATISTICS 始终为 OFF。  
  
## <a name="permissions"></a>权限  
 若要更改 AUTO_UPDATE_STATISTICS 选项，需要 **db_owner** 固定数据库角色的成员身份，或者对 *table_name* 具有 ALTER 权限。若要显示 AUTO_UPDATE_STATISTICS 选项，要求具有 **public** 角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-display-the-status-of-all-statistics-on-a-table"></a>A. 显示表的所有统计信息的状态  
 下面的内容显示 `Product` 表的所有统计信息的状态。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product';  
GO  
```  
  
### <a name="b-enable-auto_update_statistics-for-all-statistics-on-a-table"></a>B. 为表上的所有统计信息启用 AUTO_UPDATE_STATISTICS  
 下面的内容为 `Product` 表上的所有统计信息启用 AUTO_UPDATE_STATISTICS 选项。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product', 'ON';  
GO  
```  
  
### <a name="c-disable-auto_update_statistics-for-a-specific-index"></a>C. 为特定索引禁用 AUTO_UPDATE_STATISTICS  
 以下示例为 `AK_Product_Name` 表上的 `Product` 索引禁用 AUTO_UPDATE_STATISTICS 选项。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_autostats 'Production.Product', 'OFF', AK_Product_Name;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [统计信息](../../relational-databases/statistics/statistics.md)   
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_createstats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
