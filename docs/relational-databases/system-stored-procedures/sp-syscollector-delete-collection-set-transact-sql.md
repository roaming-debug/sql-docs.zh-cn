---
description: sp_syscollector_delete_collection_set (Transact-SQL)
title: sp_syscollector_delete_collection_set (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_delete_collection_set_TSQL
- sp_syscollector_delete_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_delete_collecton_set
ms.assetid: 29c63a74-4db4-4068-bd57-9fb519b0c598
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ffc5af0517be9e294594d1de595b8571c338b838
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211820"
---
# <a name="sp_syscollector_delete_collection_set-transact-sql"></a>sp_syscollector_delete_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除用户定义的收集组及其所有收集项。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syscollector_delete_collection_set [[ @collection_set_id = ] collection_set_id OUTPUT ]  
    , [[ @name = ] 'name' ]  
```  
  
## <a name="arguments"></a>参数  
 [ @collection_set_id =] *collection_set_id*  
 收集组的唯一标识符。 *collection_set_id* 为 **int** ，并且 *name* 为 NULL 时必须具有值。  
  
 [ @name =] "*name*"  
 收集组的名称。 *名称* 为 **sysname** ，并且 *collection_set_id* 为 NULL 时必须具有值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 sp_syscollector_delete_collection_set 必须在 msdb 系统数据库的上下文中运行。  
  
 *Collection_set_id* 或 *name* 必须具有值，两者都不能为 NULL。 若要获取这些值，请查询 syscollector_collection_set 系统视图。  
  
 不能删除系统定义的收集组。  
  
## <a name="permissions"></a>权限  
 需要具有 dc_admin（拥有 EXECUTE 权限）固定数据库角色的成员身份才能执行此过程。  
  
## <a name="examples"></a>示例  
 下面的示例将删除指定 *collection_set_id* 的用户定义的收集组。  
  
```  
USE msdb;  
GO  
EXEC dbo.sp_syscollector_delete_collection_set  
    @collection_set_id = 4;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [数据收集](../../relational-databases/data-collection/data-collection.md)   
 [syscollector_collection_sets (Transact-SQL)](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)  
  
  
