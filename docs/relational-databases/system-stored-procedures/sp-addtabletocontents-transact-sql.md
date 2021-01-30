---
description: sp_addtabletocontents (Transact-SQL)
title: sp_addtabletocontents (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addtabletocontents_TSQL
- sp_addtabletocontents
helpviewer_keywords:
- sp_addtabletocontents
ms.assetid: 2ea27001-74f4-463e-bf1b-b6b5a86b9219
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 469d9255813e1a072539e6aa8ca7035378ce2b92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198411"
---
# <a name="sp_addtabletocontents-transact-sql"></a>sp_addtabletocontents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将源表中当前不在跟踪表内的任何行的引用插入到合并跟踪表中。 如果已使用 **bcp** 大容量加载大量数据，则使用此选项，这将不会激发合并跟踪触发器。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addtabletocontents [ @table_name = ] 'table_name'  
    [ , [ @owner_name = ] 'owner_name' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @table_name = ] 'table_name'` 表的名称。 *table_name* **sysname**，无默认值。  
  
`[ @owner_name = ] 'owner_name'` 表所有者的名称。 *owner_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @filter_clause = ] 'filter_clause'` 指定筛选器子句，该子句控制应将新加载的数据的哪些行添加到合并跟踪表。 *filter_clause* 为 **nvarchar (4000)**，默认值为 NULL。 如果 *filter_clause* 为 **null**，则添加所有大容量加载的行。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addtabletocontents** 仅用于合并复制。  
  
 *Table_name* 中的行按其 **rowguidcol** 进行引用，并且将引用添加到合并跟踪表。 在将数据大容量复制到使用合并复制发布的表中之后，应使用 **sp_addtabletocontents** 。 该存储过程将启动对已复制行的跟踪，并确保下一次同步中包括这些新行。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addtabletocontents**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
