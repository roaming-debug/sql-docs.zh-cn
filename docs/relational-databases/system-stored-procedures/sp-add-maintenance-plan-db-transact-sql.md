---
description: sp_add_maintenance_plan_db (Transact-SQL)
title: sp_add_maintenance_plan_db (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_maintenance_plan_db_TSQL
- sp_add_maintenance_plan_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_maintenance_plan_db
ms.assetid: 76f4fefa-5b99-4deb-beed-e198987a45a9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e8cc84a2afd70a4e5bd89a581b0fb1ac1dcc229f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209013"
---
# <a name="sp_add_maintenance_plan_db-transact-sql"></a>sp_add_maintenance_plan_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将数据库与维护计划关联。  
  
> [!NOTE]  
>  数据库维护计划使用这些存储过程。 但此功能已由不使用此存储过程的维护计划取代。 可使用此过程对从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本升级的安装维护数据库维护计划。  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_add_maintenance_plan_db [ @plan_id = ] 'plan_id' ,   
     [ @db_name = ] 'database_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @plan_id = ] 'plan_id'` 指定维护计划的计划 ID。 *plan_id* 是 **uniqueidentifier**，并且必须是有效 id。  
  
`[ @db_name = ] 'database_name'` 指定要添加到维护计划的数据库的名称。 在添加到计划中之前，数据库必须已创建或存在。 database_name 的数据类型为 sysname。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_add_maintenance_plan_db** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_add_maintenance_plan_db** 执行。  
  
## <a name="examples"></a>示例  
 此示例将 **AdventureWorks2012** 数据库添加到 **sp_add_maintenance_plan** 中创建的维护计划。  
  
```  
EXECUTE   sp_add_maintenance_plan_db N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC',N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>另请参阅  
 [维护计划](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [数据库维护计划存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  
