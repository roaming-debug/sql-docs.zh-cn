---
description: sp_lookupcustomresolver (Transact-SQL)
title: sp_lookupcustomresolver (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_lookupcustomresolver_TSQL
- sp_lookupcustomresolver
helpviewer_keywords:
- sp_lookupcustomresolver
ms.assetid: 356a7b8a-ae53-4fb5-86ee-fcfddbf23ddd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bdafb84d97bad1672780008ae845287ce105bb14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185363"
---
# <a name="sp_lookupcustomresolver-transact-sql"></a>sp_lookupcustomresolver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回在分发服务器上注册的基于 COM 的自定义冲突解决程序组件的业务逻辑处理程序或类标识符 (CLSID) 值的相关信息。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_lookupcustomresolver [ @article_resolver = ] 'article_resolver'   
    [, [ @resolver_clsid = ] 'resolver_clsid' OUTPUT ]  
    [ , [ @is_dotnet_assembly = ] is_dotnet_assembly OUTPUT ]  
    [ , [ @dotnet_assembly_name = ] 'dotnet_assembly_name' OUTPUT ]  
    [ , [ @dotnet_class_name = ] 'dotnet_class_name' OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @article_resolver = ] 'article_resolver'` 指定正在注销的自定义业务逻辑的名称。 *article_resolver* 为 **nvarchar (255)**，无默认值。 如果被删除的业务逻辑是 COM 组件，则此参数是该组件的友好名称。 如果业务逻辑是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework 程序集，则此参数是该程序集的名称。  
  
`[ @resolver_clsid = ] 'resolver_clsid' OUTPUT` 与 *article_resolver* 参数中指定的自定义业务逻辑的名称关联的 COM 对象的 CLSID 值。 *resolver_clsid* 为 **nvarchar (50)**，默认值为 NULL。  
  
`[ @is_dotnet_assembly = ] 'is_dotnet_assembly' OUTPUT` 指定正在注册的自定义业务逻辑的类型。 *is_dotnet_assembly* 为 **bit**，默认值为0。 **1** 指示正在注册的自定义业务逻辑是一个业务逻辑处理程序程序集; **0** 指示它是一个 COM 组件。  
  
`[ @dotnet_assembly_name = ] 'dotnet_assembly_name' OUTPUT` 实现业务逻辑处理程序的程序集的名称。 *dotnet_assembly_name* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @dotnet_class_name = ] 'dotnet_class_name' OUTPUT` 是重写 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> 以实现业务逻辑处理程序的类的名称。 *dotnet_class_name* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。 未从发布服务器调用该存储过程时使用此参数。 如果未指定，则假定本地服务器是发布服务器。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_lookupcustomresolver** 用于合并复制。  
  
 当组件未在分发时注册时， **sp_lookupcustomresolver** 返回 *resolver_clsid* 的 NULL 值，当注册属于注册为业务逻辑处理程序的 .NET Framework 程序集时，返回值 "00000000-0000-0000-0000-000000000000"。  
  
 **sp_lookupcustomresolver** 由 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 和 [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) 调用以验证指定的 *article_resolver*。  
  
## <a name="permissions"></a>权限  
 只有发布数据库上 **db_owner** 固定数据库角色的成员才能执行 **sp_lookupcustomresolver**。  
  
## <a name="see-also"></a>另请参阅  
 [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [在合并同步过程中执行业务逻辑](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md)   
 [为合并项目实现业务逻辑处理程序](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [指定合并项目冲突解决程序](../../relational-databases/replication/publish/specify-a-merge-article-resolver.md)   
 [sp_registercustomresolver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
