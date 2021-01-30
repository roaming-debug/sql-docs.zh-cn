---
description: sp_helpdistributiondb (Transact-SQL)
title: sp_helpdistributiondb (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistributiondb_TSQL
- sp_helpdistributiondb
helpviewer_keywords:
- sp_helpdistributiondb
ms.assetid: a2917020-26d1-4011-99f8-9212d120fd2d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e9740605d1f3eca456ba506a692b5fc827273e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179297"
---
# <a name="sp_helpdistributiondb-transact-sql"></a>sp_helpdistributiondb (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回指定分发数据库的属性。 此存储过程在分发服务器上对分发数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpdistributiondb [ [ @database= ] 'database_name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @database = ] 'database_name'` 要为其返回属性的数据库的名称。 *database_name* 是 **sysname**， **%** 对于与分发服务器关联的所有数据库，默认值为，用户具有权限。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|分发数据库的名称。|  
|**min_distretention**|**int**|事务被删除前的最小保持期（以小时为单位）。|  
|**max_distretention**|**int**|事务被删除前的最大保持期（以小时为单位）。|  
|**history retention**|**int**|保留历史记录的小时数。|  
|**history_cleanup_agent**|**sysname**|历史记录清除代理的名称。|  
|**distribution_cleanup_agent**|**sysname**|分发清除代理的名称。|  
|**status**|**int**|仅限内部使用。|  
|**data_folder**|**nvarchar(255)**|用于存储数据库文件的目录的名称。|  
|**data_file**|**nvarchar(255)**|数据库文件名。|  
|**data_file_size**|**int**|以 MB 为单位的数据文件初始大小。|  
|**log_folder**|**nvarchar(255)**|数据库日志文件的目录名。|  
|**log_file**|**nvarchar(255)**|日志文件的名称。|  
|**log_file_size**|**int**|以 MB 为单位的日志文件初始大小。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpdistributiondb** 在所有类型的复制中使用。  
  
## <a name="permissions"></a>权限  
 分发数据库中 **db_owner** 固定数据库角色的成员或 **replmonitor** 角色的成员，以及使用分发数据库的发布的发布访问列表中的用户可以执行 **sp_helpdistributiondb** 以返回与文件相关的信息。 **公共** 角色的成员可以执行 **sp_helpdistributiondb** ，以返回其具有访问权限的分发数据库的非文件相关信息。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistributiondb &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)   
 [sp_changedistributiondb &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_dropdistributiondb &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
