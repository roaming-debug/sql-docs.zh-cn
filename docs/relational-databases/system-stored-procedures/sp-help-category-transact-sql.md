---
description: sp_help_category (Transact-SQL)
title: sp_help_category (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_category
- sp_help_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_category
ms.assetid: 8cad1dcc-b43e-43bd-bea0-cb0055c84169
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 68c8d1fcf243de008f889d4037bbd38a590c6403
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208923"
---
# <a name="sp_help_category-transact-sql"></a>sp_help_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提供有关作业、警报或操作员的指定类的信息。  
   
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_category [ [ @class = ] 'class' ]   
     [ , [ @type = ] 'type' ]   
     [ , [ @name = ] 'name' ]   
     [ , [ @suffix = ] suffix ]   
```  
  
## <a name="arguments"></a>参数  
`[ @class = ] 'class'` 要请求其信息的类。 *类* 为 **varchar (8)**，默认值为 **JOB**。 *类* 可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**任务**|提供有关作业类别的信息。|  
|**发出**|提供有关警报类别的信息。|  
|**操作员**|提供有关操作员类别的信息。|  
  
`[ @type = ] 'type'` 请求其信息的类别的类型。 *类型* 为 **varchar (12)**，默认值为 NULL，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**LOCAL**|本地作业类别。|  
|**MULTI-SERVER**|多服务器作业类别。|  
|**NONE**|**作业** 之外的类的类别。|  
  
`[ @name = ] 'name'` 请求其信息的类别的名称。 *名称* 为 **sysname**，默认值为 NULL。  
  
`[ @suffix = ] suffix` 指定结果集中 **category_type** 列是 ID 还是名称。 *后缀* 为 **bit**，默认值为 **0**。 **1** 将 **category_type** 显示为名称， **0** 将其显示为 ID。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 当 **\@ 后缀** 为 **0** 时， **sp_help_category** 将返回以下结果集：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**category_id**|**int**|类别 ID|  
|**category_type**|**tinyint**|类别的类型：<br /><br /> **1** = 本地<br /><br /> **2** = 多服务器<br /><br /> **3** = 无|  
|name|**sysname**|类别名称|  
  
 当 **\@ 后缀** 为 **1** 时， **sp_help_category** 将返回以下结果集：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**category_id**|**int**|类别 ID|  
|**category_type**|**sysname**|类别的类型。 **本地**、**多服务器** 或 **无**|  
|name|**sysname**|类别名称|  
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_help_category** 。  
  
 如果未指定参数，则结果集将提供有关所有作业类别的信息。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-local-job-information"></a>A. 返回本地作业信息  
 以下示例将返回有关在本地管理的作业的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_category  
    @type = N'LOCAL' ;  
GO  
```  
  
### <a name="b-returning-alert-information"></a>B. 返回警报信息  
 以下示例将返回有关 Replication 警报类别的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_category  
    @class = N'ALERT',  
    @name = N'Replication' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md)   
 [sp_delete_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-category-transact-sql.md)   
 [sp_update_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-category-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
