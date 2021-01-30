---
description: sp_register_custom_scripting (Transact-SQL)
title: sp_register_custom_scripting (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_register_custom_scripting
- sp_register_custom_scripting_TSQL
helpviewer_keywords:
- sp_register_custom_scripting
ms.assetid: a8159282-de3b-4b9e-bdc9-3d3fce485c7f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9239f89c5f65f0a4af3ddfd5588a92551dfbcdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210446"
---
# <a name="sp_register_custom_scripting-transact-sql"></a>sp_register_custom_scripting (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  复制允许用户定义的自定义存储过程替换事务复制中使用的一个或多个默认过程。 对复制的表进行架构更改时，将重新创建这些存储过程。 **sp_register_custom_scripting** 注册 [!INCLUDE[tsql](../../includes/tsql-md.md)] 当发生架构更改时执行的存储过程或脚本文件，以编写新的用户定义的自定义存储过程的定义脚本。 这个新的用户定义的自定义存储过程应反映表的新架构。 在发布服务器上对发布数据库执行 **sp_register_custom_scripting** ，并在架构发生更改时在订阅服务器上执行已注册的脚本文件或存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_register_custom_scripting [ @type  = ] 'type'  
    [ @value = ] 'value'   
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @type = ] 'type'` 要注册的自定义存储过程或脚本的类型。 *类型* 为 **varchar (16)**，无默认值，并且可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**insert**|复制 INSERT 语句时，将执行注册的自定义存储过程。|  
|**update**|复制 UPDATE 语句时，将执行注册的自定义存储过程。|  
|**delete**|复制 DELETE 语句时，将执行注册的自定义存储过程。|  
|**custom_script**|在数据定义语言 (DDL) 触发器的末尾执行脚本。|  
  
`[ @value = ] 'value'` 要注册的脚本文件的存储过程或名称和完全限定路径的名称 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 *值* 为 **nvarchar (1024)**，无默认值。  
  
> [!NOTE]  
>  指定 NULL 作为 *值* 参数将取消注册先前注册的脚本，这与运行 [sp_unregister_custom_scripting](../../relational-databases/system-stored-procedures/sp-unregister-custom-scripting-transact-sql.md)相同。  
  
 如果 " *类型* " 的值为 " **custom_script**"，则需要脚本文件的名称和完整路径 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 否则， *值* 必须是已注册的存储过程的名称。  
  
`[ @publication = ] 'publication'` 正在为其注册自定义存储过程或脚本的发布的名称。 *发布* 为 **sysname**，默认值为 **NULL**。  
  
`[ @article = ] 'article'` 正在为其注册自定义存储过程或脚本的项目的名称。 *项目* 的默认值为 **sysname**，默认值为 **NULL**。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_register_custom_scripting** 用于快照复制和事务复制。  
  
 应在对复制的表进行架构更改前执行此存储过程。 有关使用此存储过程的详细信息，请参阅 [重新生成自定义事务过程以反映架构更改](../../relational-databases/replication/transactional/transactional-articles-regenerate-to-reflect-schema-changes.md)。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员或 **db_ddladmin** 固定数据库角色的成员才能 **sp_register_custom_scripting** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [sp_unregister_custom_scripting &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-unregister-custom-scripting-transact-sql.md)  
  
  
