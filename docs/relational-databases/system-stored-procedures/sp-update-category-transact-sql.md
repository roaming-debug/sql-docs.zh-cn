---
description: sp_update_category (Transact-SQL)
title: sp_update_category (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_category
- sp_update_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_category
ms.assetid: 098b926a-b078-4122-a5e1-3ef54b979dd4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c771e031254821562933cd2e7f4d9f51903edba6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184727"
---
# <a name="sp_update_category-transact-sql"></a>sp_update_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改类别的名称。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_update_category  
     [@class =] 'class' ,   
     [@name =] 'old_name' ,  
     [@new_name =] 'new_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @class = ] 'class'` 要更新的类别的类。 *类* 为 **varchar (8)**，无默认值，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**ALERT**|更新警报类别。|  
|**任务**|更新作业类别。|  
|**操作员**|更新操作员类别。|  
  
`[ @name = ] 'old_name'` 类别的当前名称。 *old_name* **sysname**，无默认值。  
  
`[ @new_name = ] 'new_name'` 类别的新名称。 *new_name* **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_update_category** 。  
  
## <a name="permissions"></a>权限  
 若要运行此存储过程，用户必须被授予 **sysadmin** 固定服务器角色。  
  
## <a name="examples"></a>示例  
 以下示例将作业类别的名称由 `AdminJobs` 重命名为 `Administrative Jobs`。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_category  
  @class = N'JOB',  
  @name = N'AdminJobs',  
  @new_name = N'Administrative Jobs' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md)   
 [sp_delete_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-category-transact-sql.md)   
 [sp_help_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-category-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
