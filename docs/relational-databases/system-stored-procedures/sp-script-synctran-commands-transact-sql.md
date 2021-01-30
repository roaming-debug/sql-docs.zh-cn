---
description: sp_script_synctran_commands (Transact-SQL)
title: sp_script_synctran_commands (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_script_synctran_commands
- sp_script_synctran_commands_TSQL
helpviewer_keywords:
- sp_script_synctran_commands
ms.assetid: f132694a-dd05-405b-9d84-21acce9e564a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9d52c3c63009d6dd5fdc33fca3f9c2c1598e96e8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189976"
---
# <a name="sp_script_synctran_commands-transact-sql"></a>sp_script_synctran_commands (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  生成一个脚本，该脚本包含要应用于可更新订阅的订阅服务器上的 **sp_addsynctrigger** 调用。 对于发布中的每个项目，都有一个 **sp_addsynctrigger** 调用。 生成的脚本还包含创建处理已排队发布所需的 **MSsubsciption_articles** 表的 **sp_addqueued_artinfo** 调用。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_script_synctran_commands [@publication = ] 'publication'  
    [ , [@article = ] 'article']  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要编写脚本的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 要编写脚本的项目的名称。 *项目* 的默认值为 **sysname**，默认值为 **all**，表示为所有项目编写脚本。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="results-set"></a>结果集  
 **sp_script_synctran_commands** 返回由单个 **nvarchar (4000)** 列组成的结果集。 结果集构成了创建要在订阅服务器上应用的 **sp_addsynctrigger** 和 **sp_addqueued_artinfo** 调用所需的完整脚本。  
  
## <a name="remarks"></a>备注  
 **sp_script_synctran_commands** 用于快照复制和事务复制。  
  
 **sp_addqueued_artinfo** 用于排队的可更新订阅。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_script_synctran_commands**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addsynctriggers &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsynctriggers-transact-sql.md)   
 [sp_addqueued_artinfo &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addqueued-artinfo-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
