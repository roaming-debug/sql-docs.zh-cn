---
description: sp_showrowreplicainfo (Transact-SQL)
title: sp_showrowreplicainfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_showrowreplicainfo_TSQL
- sp_showrowreplicainfo
helpviewer_keywords:
- sp_showrowreplicainfo
ms.assetid: 6a9dbc1a-e1e1-40c4-97cb-8164a2288f76
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f2a66e8e6123adef864ca2beadef31cd4a9cbffe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193483"
---
# <a name="sp_showrowreplicainfo-transact-sql"></a>sp_showrowreplicainfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示有关在合并复制中用作项目的表中的行的信息。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_showrowreplicainfo [ [ @ownername = ] 'ownername' ]  
    [ , [ @tablename =] 'tablename' ]   
        , [ @rowguid =] rowguid   
    [ , [ @show = ] 'show' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @ownername = ] 'ownername'` 表所有者的名称。 *ownername* 的值为 **sysname**，默认值为 NULL。 如果数据库包含多个同名的表，但每个表具有不同的所有者，则该参数对于区分这些表很有用。  
  
`[ @tablename = ] 'tablename'` 包含要为其返回信息的行的表的名称。 *tablename* 的值为 **sysname**，默认值为 NULL。  
  
`[ @rowguid = ] rowguid` 行的唯一标识符。 *rowguid* 是 **uniqueidentifier**，无默认值。  
  
`[ @show = ] 'show'` 确定要在结果集中返回的信息量。 *show* 为 **nvarchar (20)** ，默认值为 BOTH。 如果为 **row**，则仅返回行版本信息。 如果是 **列**，则只返回列版本信息。 如果 **两者同时** 返回，则返回行和列信息。  
  
## <a name="result-sets-for-row-information"></a>行信息的结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|server_name|**sysname**|承载生成行版本项目的数据库的服务器名称。|  
|**db_name**|**sysname**|生成此项目的数据库的名称。|  
|**db_nickname**|**binary(6)**|生成此项目的数据库的别名。|  
|**version**|**int**|项的版本。|  
|**current_state**|**nvarchar (9)**|返回有关行的当前状态的信息。<br /><br /> **y** 行数据表示行的当前状态。<br /><br /> **n** 行数据不表示行的当前状态。<br /><br /> **\<n/a>** -不适用。<br /><br /> **\<unknown>** -无法确定当前状态。|  
|**rowversion_table**|**nchar (17)**|指示是否将行版本存储在 [MSmerge_contents](../../relational-databases/system-tables/msmerge-contents-transact-sql.md) 表或 [MSmerge_tombstone](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md) 表中。|  
|**comment**|**nvarchar(255)**|有关此行版本项目的附加信息。 通常，该字段为空。|  
  
## <a name="result-sets-for-column-information"></a>列信息的结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|server_name|**sysname**|承载生成列版本项目的数据库的服务器名称。|  
|**db_name**|**sysname**|生成此项目的数据库的名称。|  
|**db_nickname**|**binary(6)**|生成此项目的数据库的别名。|  
|**version**|**int**|项的版本。|  
|**colname**|**sysname**|列版本项目表示的项目列的名称。|  
|**comment**|**nvarchar(255)**|有关此列版本项目的附加信息。 通常，该字段为空。|  
  
## <a name="result-set-for-both"></a>行信息和列信息的结果集  
 如果为 *show* 选择了 **两个** 值，则返回行和列结果集。  
  
## <a name="remarks"></a>备注  
 **sp_showrowreplicainfo** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 **sp_showrowreplicainfo** 只能由发布数据库上的 **db_owner** 固定数据库角色的成员或发布数据库上的发布访问列表 (PAL) 成员执行。  
  
## <a name="see-also"></a>另请参阅  
 [检测并解决合并复制冲突](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
