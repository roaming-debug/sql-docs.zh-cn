---
description: sp_add_targetservergroup (Transact-SQL)
title: sp_add_targetservergroup (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_targetservergroup
- sp_add_targetservergroup_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_targetservergroup
ms.assetid: acb69343-d766-46ff-b771-0c7655c5231a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f070ce20e9efc67f8d40b5597a7118aeecddfd32
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192387"
---
# <a name="sp_add_targetservergroup-transact-sql"></a>sp_add_targetservergroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  添加指定的服务器组。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_add_targetservergroup [ @name = ] 'name'   
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'` 要创建的服务器组的名称。 *名称* 为 **sysname**，无默认值。 *名称* 不能包含逗号。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 目标服务器组提供将一组目标服务器作为作业目标的简单方法。 有关详细信息，请参阅 [sp_apply_job_to_targets](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能执行此过程。  
  
## <a name="examples"></a>示例  
 以下示例创建名为 `Servers Processing Customer Orders` 的目标服务器组。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_targetservergroup  
    'Servers Processing Customer Orders' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_apply_job_to_targets &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_targetservergroup &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-targetservergroup-transact-sql.md)   
 [sp_help_targetservergroup &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-targetservergroup-transact-sql.md)   
 [sp_update_targetservergroup &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-targetservergroup-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
