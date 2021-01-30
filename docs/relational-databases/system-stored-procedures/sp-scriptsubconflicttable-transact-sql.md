---
description: sp_scriptsubconflicttable (Transact-SQL)
title: sp_scriptsubconflicttable (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_scriptsubconflicttable
- sp_scriptsubconflicttable_TSQL
helpviewer_keywords:
- sp_scriptsubconflicttable
ms.assetid: 13867145-3dad-47a4-8d50-a65175418479
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ed5d504e669e1bdf5fb6677880e87271739c1cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189957"
---
# <a name="sp_scriptsubconflicttable-transact-sql"></a>sp_scriptsubconflicttable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为给定排队订阅项目生成用于在订阅服务器上创建冲突表的脚本。 生成的脚本在订阅服务器的订阅数据库上执行。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_scriptsubconflicttable [@publication =] 'publication'    , [@article =] 'article'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含项目的发布的名称。 名称在数据库中必须是唯一的。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 订阅项目的名称。 *项目* 是 **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**cmdtext**|**nvarchar(4000)**|为排队订阅项目返回用于在订阅服务器上创建冲突表的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 该脚本在订阅服务器的订阅数据库上执行。|  
  
## <a name="remarks"></a>备注  
 **sp_scriptsubconflicttable** 用于具有手动应用初始快照的订阅的订阅服务器。 冲突表为订阅服务器上的可选表。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_scriptsubconflicttable**。  
  
## <a name="see-also"></a>另请参阅  
 [排队更新冲突的检测和解决](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
