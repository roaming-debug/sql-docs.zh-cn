---
description: sp_enumeratependingschemachanges (Transact-SQL)
title: sp_enumeratependingschemachanges (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_enumeratependingschemachanges
- sp_enumeratependingschemachanges_TSQL
helpviewer_keywords:
- sp_enumeratependingschemachanges
ms.assetid: df169b21-d10a-41df-b3a1-654cfb58bc21
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7a75e22f94688d4d8819b22b1b478bd3a3a66ce1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193564"
---
# <a name="sp_enumeratependingschemachanges-transact-sql"></a>sp_enumeratependingschemachanges (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回所有的挂起架构更改的列表。 此存储过程可用于 [sp_markpendingschemachange](../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md)，这使管理员可以跳过所选挂起的架构更改，使其不会被复制。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_enumeratependingschemachanges [ @publication = ] 'publication'   
    [ , [ @starting_schemaversion = ] starting_schemaversion ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @starting_schemaversion = ] starting_schemaversion` 要包含在结果集中的最小架构更改数。  
  
## <a name="result-set"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**article_name**|**sysname**|应用了架构更改的项目的名称，或应用于整个发布的架构更改的 **发布范围** 。|  
|**schemaversion**|**int**|挂起的架构更改的编号。|  
|**schematype**|**sysname**|表示架构更改类型的文本值。|  
|**schematext**|**nvarchar(max)**|说明架构更改的 [!INCLUDE[tsql](../../includes/tsql-md.md)]。|  
|**schemastatus**|**nvarchar (10)**|指示架构更改是否针对项目挂起，可以是下列值之一：<br /><br /> **active** = 架构更改处于挂起状态<br /><br /> **非活动** = 架构更改处于非活动状态<br /><br /> **skip** = 不复制架构更改|  
|**schemaguid**|**uniqueidentifier**|标识架构更改。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_enumeratependingschemachanges** 用于合并复制。  
  
 与 [sp_markpendingschemachange](../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md)一起使用的 **sp_enumeratependingschemachanges** 用于合并复制的可支持性，仅在其他纠正措施（如重新初始化）未能更正此情况时才使用。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_enumeratependingschemachanges**。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sysmergeschemachange &#40;Transact-sql&#41;](../../relational-databases/system-tables/sysmergeschemachange-transact-sql.md)  
  
  
