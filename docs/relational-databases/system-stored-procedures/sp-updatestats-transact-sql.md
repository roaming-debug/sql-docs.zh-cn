---
description: sp_updatestats (Transact-SQL)
title: sp_updatestats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_updatestats_TSQL
- sp_updatestats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_updatestats
ms.assetid: 01184651-6e61-45d9-a502-366fecca0ee4
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0cfaffcf1df13da39a093043d84e28f4433d3a71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209264"
---
# <a name="sp_updatestats-transact-sql"></a>sp_updatestats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

`UPDATE STATISTICS`针对当前数据库中的所有用户定义表和内部表运行。  
  
有关的详细信息 `UPDATE STATISTICS` ，请参阅 [UPDATE STATISTICS &#40;transact-sql&#41;](../../t-sql/statements/update-statistics-transact-sql.md)。 有关统计信息的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
    
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_updatestats [ [ @resample = ] 'resample']  
```  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="arguments"></a>参数  
`[ @resample = ] 'resample'` 指定 **sp_updatestats** 将使用 [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md) 语句的 "重新采样" 选项。 如果未指定 **"重新采样"** ，则 **sp_updatestats** 使用默认采样更新统计信息。 重新 **采样** 是 **varchar (8)** ，默认值为 NO。  
  
## <a name="remarks"></a>备注  
  `UPDATE STATISTICS` 通过 `ALL` 在数据库中的所有用户定义表和内部表中指定关键字，sp_updatestats 执行。 sp_updatestats 显示指示其进度的消息。 完成更新之后，此存储过程将报告已为所有的表更新了统计信息。  
  
**sp_updatestats** 更新已禁用的非聚集索引的统计信息，并且不更新已禁用聚集索引的统计信息。  
  
对于基于磁盘的表， **sp_updatestats** 会根据 **sys.dm_db_stats_properties** 目录视图中的 **modification_counter** 信息更新统计信息，并更新至少有一行修改的统计信息。 **Sp_updatestats** 执行时，将始终更新内存优化表的统计信息。 因此， **sp_updatestats** 不需要执行更多的操作。  
  
**sp_updatestats** 可以触发存储过程或其他已编译代码的重新编译。 但是，如果在引用的表和索引上只有一个查询计划，则 **sp_updatestats** 可能不会导致重新编译。 在这些情况下，即便更新了统计信息也不必进行重新编译。  
  
对于兼容性级别低于90的数据库，执行 **sp_updatestats** 不会保留特定统计信息的最新 NORECOMPUTE 设置。 对于兼容性级别为90或更高的数据库，sp_updatestats 会保留特定统计信息的最新 NORECOMPUTE 选项。 有关禁用和重新启用统计信息更新的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
  
## <a name="permissions"></a>权限  

若要运行 **sp_updatestats**，用户必须是数据库 (的所有者 `dbo` ，而不只是角色的成员 `db_owner`) 或作为 sysadmin 固定服务器角色的成员。

## <a name="examples"></a>示例  
以下示例更新 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中各表的统计信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_updatestats;   
```  

## <a name="automatic-index-and-statistics-management"></a>自动索引和统计信息管理
利用[自适应索引碎片整理](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)等解决方案，自动管理一个或多个数据库的索引碎片整理和统计信息更新。 此过程根据碎片级别以及其他参数，自动选择是重新生成索引还是重新组织索引，并使用线性阈值更新统计信息。

## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_autostats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [sp_createstats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)   
 [系统存储过程](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
 
