---
description: sp_helpstats (Transact-SQL)
title: sp_helpstats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpstats
- sp_helpstats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpstats
ms.assetid: 00ab3cfd-2736-4fc0-b1b2-16dd49fb2fe5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c3b0d76b08d8be5aefcbfceee570d893d6ca4659
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198720"
---
# <a name="sp_helpstats-transact-sql"></a>sp_helpstats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关指定表中的列和索引的统计信息。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)] 若要获取有关统计信息的信息，请查询 [sys.databases](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) 和 [sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md) 目录视图。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpstats[ @objname = ] 'object_name'   
     [ , [ @results = ] 'value' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @objname = ] 'object_name'` 指定要为其提供统计信息的表。 *object_name* 为 **nvarchar (520)** ，且不能为 null。 可以指定一个一部分或两部分名称。  
  
`[ @results = ] 'value'` 指定要提供的信息的范围。 有效条目为 " **所有** " 和 " **统计**"。 **所有** 索引的统计信息以及创建了统计信息的列的统计信息; **STATS** 仅列出与索引不关联的统计信息。 *值* 为 **nvarchar (5)** ，默认值为 STATS。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 下表对结果集中的列进行了说明。  
  
|列名称|说明|  
|-----------------|-----------------|  
|**statistics_name**|统计信息的名称。 返回 **sysname** ，且不能为 null。|  
|**statistics_keys**|统计信息所基于的键。 返回 **nvarchar (2078)** 且不能为 null。|  
  
## <a name="remarks"></a>备注  
 可以使用 DBCC SHOW_STATISTICS 显示特定索引或统计信息的相关详细统计信息。 有关详细信息，请参阅 [DBCC SHOW_STATISTICS &#40;transact-sql&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md) 和 [sp_helpindex &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例通过执行 `sp_createstats`，为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的所有用户表的所有合格列创建单列统计信息。 然后，运行 `sp_helpstats` 以查找在 `Customer` 表中创建的结果统计信息。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_createstats;  
GO  
EXEC sp_helpstats   
@objname = 'Sales.Customer',  
@results = 'ALL';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `statistics_name               statistics_keys`  
  
 `----------------------------  ----------------`  
  
 `_WA_Sys_00000003_22AA2996     AccountNumber`  
  
 `AK_Customer_AccountNumber     AccountNumber`  
  
 `AK_Customer_rowguid           rowguid`  
  
 `CustomerType                  CustomerType`  
  
 `IX_Customer_TerritoryID       TerritoryID`  
  
 `ModifiedDate                  ModifiedDate`  
  
 `PK_Customer_CustomerID        CustomerID`  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
