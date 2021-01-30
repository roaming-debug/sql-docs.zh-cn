---
description: sp_unregister_custom_scripting (Transact-SQL)
title: sp_unregister_custom_scripting (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_unregister_custom_scripting_TSQL
- sp_unregister_custom_scripting
helpviewer_keywords:
- sp_unregister_custom_scripting
ms.assetid: b6e9e0d2-9144-434d-88af-4874f2582399
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7dc5ff01b5dfb5742233dce8f89384edbc9db957
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209671"
---
# <a name="sp_unregister_custom_scripting-transact-sql"></a>sp_unregister_custom_scripting (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  此存储过程将删除通过执行 sp_register_custom_scripting 注册的用户定义的自定义存储过程或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本[](../../relational-databases/system-stored-procedures/sp-register-custom-scripting-transact-sql.md)文件。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_unregister_custom_scripting [ @type  = ] 'type'  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @type = ] 'type'` 要删除的自定义存储过程或脚本的类型。 *类型* 为 **varchar (16)**，无默认值，并且可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**insert**|复制 INSERT 语句时，执行注册的自定义存储过程或脚本。|  
|**update**|复制 UPDATE 语句时，执行注册的自定义存储过程或脚本。|  
|**delete**|复制 DELETE 语句时，执行注册的自定义存储过程或脚本。|  
|**custom_script**|在数据定义语言 (DDL) 触发器的结尾执行已注册的自定义存储过程或脚本。|  
  
`[ @publication = ] 'publication'` 要为其删除自定义存储过程或脚本的发布的名称。 *发布* 为 **sysname**，默认值为 NULL。  
  
`[ @article = ] 'article'` 要为其删除自定义存储过程或脚本的项目的名称。 *项目* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_unregister_custom_scripting** 用于快照复制和事务复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员或 **db_ddladmin** 固定数据库角色的成员才能 **sp_unregister_custom_scripting** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_register_custom_scripting &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-register-custom-scripting-transact-sql.md)  
  
  
