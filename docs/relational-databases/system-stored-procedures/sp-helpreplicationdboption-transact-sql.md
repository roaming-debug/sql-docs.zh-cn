---
description: sp_helpreplicationdboption (Transact-SQL)
title: sp_helpreplicationdboption (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpreplicationdboption_TSQL
- sp_helpreplicationdboption
helpviewer_keywords:
- sp_helpreplicationdboption
ms.assetid: 143ce689-108b-49d7-9892-fd3a86897f38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17c47d247e154ed80f0daeb8bacdb4984864fa03
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210807"
---
# <a name="sp_helpreplicationdboption-transact-sql"></a>sp_helpreplicationdboption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  显示是否为发布服务器上的数据库启用复制。 此存储过程在发布服务器的任何数据库中执行。 *Oracle 发布服务器不支持。*  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpreplicationdboption [ [ @dbname =] 'dbname' ]  
    [ , [ @type = ] 'type' ]  
    [ , [ @reserved = ] reserved ]  
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] 'dbname'` 数据库的名称。 *dbname* 的值为 **sysname**，默认值为 **%** 。 如果为 **%** ，则结果集包含发布服务器上的所有数据库，否则仅返回有关指定数据库的信息。 如下所述，将不会返回用户对其不具有适当权限的任何数据库的信息。  
  
`[ @type = ] 'type'` 将结果集限制为仅包含已启用指定复制选项 *类型* 值的数据库。 *类型* 为 **sysname**，可以为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**发布**|允许事务复制。|  
|**合并发布**|允许合并复制。|  
|**允许复制** (默认值) |允许事务复制或合并复制。|  
  
`[ @reserved = ] reserved` 指定是否返回有关现有发布和订阅的信息。 *reserved* 为 **bit**，默认值为0。 如果为 **1**，则结果集包含有关指定数据库是否具有任何现有发布或订阅的信息。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|数据库的名称。|  
|**id**|**int**|数据库标识符。|  
|**transpublish**|**bit**|如果已为快照或事务发布启用了数据库，则为;如果值为 **1** ，则表示启用了快照或事务发布。|  
|**mergepublish**|**bit**|如果数据库已启用合并发布，则为; 否则为。如果值为 **1** ，则表示启用了合并发布。|  
|**dbowner**|**bit**|如果用户是 **db_owner** 固定数据库角色的成员，则为; 否则为。如果值为 **1** ，则表示该用户是此角色的成员。|  
|**dbreadonly**|**bit**|如果数据库标记为只读，则为。如果值为 **1** ，则表示该数据库是只读的。|  
|**haspublications**|**bit**|如果数据库具有任何现有发布，则为; 否则为。其中，值为 **1** 表示存在现有发布。|  
|**haspullsubscriptions**|**bit**|如果数据库具有任何现有的请求订阅，则为;如果值为 **1** ，则表示存在现有请求订阅。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpreplicationdboption** 用于快照复制、事务复制和合并复制。  
  
## <a name="permissions"></a>权限  
 **Sysadmin** 固定服务器角色的成员可以对任何数据库执行 **sp_helpreplicationdboption** 。 **Db_owner** 固定数据库角色的成员可以对该数据库执行 **sp_helpreplicationdboption** 。  
  
## <a name="see-also"></a>另请参阅  
 [sp_replicationdboption &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
