---
description: sp_enumcustomresolvers (Transact-SQL)
title: sp_enumcustomresolvers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_enumcustomresolvers
- sp_enumcustomresolvers_TSQL
helpviewer_keywords:
- sp_enumcustomresolvers
ms.assetid: 81bd0d3a-48dc-42b1-b662-c630f61fc630
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8972aa1aaa4e567c60efb9653a75e6fa196211b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99156777"
---
# <a name="sp_enumcustomresolvers-transact-sql"></a>sp_enumcustomresolvers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回所有可用的业务逻辑处理程序以及在分发服务器上注册的自定义冲突解决程序的列表。 此存储过程在发布服务器的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_enumcustomresolvers [ [ @distributor =] 'distributor']  
```  
  
## <a name="arguments"></a>参数  
`[ @distributor = ] 'distributor'` 自定义冲突解决程序所在的分发服务器的名称。 *分发服务器* 的默认值为 **sysname**，默认值为 NULL。 *不推荐使用此参数，该参数将从以后的版本中删除。*  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**article_resolver**|**nvarchar(255)**|业务逻辑处理程序或冲突解决程序的友好名称。|  
|**resolver_clsid**|**nvarchar(50)**|基于 COM 的冲突解决程序的类 ID (CLSID)。 对于业务逻辑处理程序，此列返回 CLSID 值 0。|  
|**is_dotnet_assembly**|**bit**|指示注册是否针对业务逻辑处理程序。<br /><br /> **0** = 基于 COM 的冲突解决程序<br /><br /> **1** = 业务逻辑处理程序|  
|**dotnet_assembly_name**|**nvarchar(255)**|实现业务逻辑处理程序的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework 程序集的名称。|  
|**dotnet_class_name**|**nvarchar(255)**|实现业务逻辑处理程序时优先级高于 <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> 的类的名称。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_enumcustomresolvers** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员和 **db_owner** 固定数据库角色的成员才能执行 **sp_enumcustomresolvers**。  
  
## <a name="see-also"></a>另请参阅  
 [为合并项目实现业务逻辑处理程序](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [为合并项目实现自定义冲突解决程序](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [sp_lookupcustomresolver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
