---
description: sp_replmonitorhelppublisher (Transact-SQL)
title: sp_replmonitorhelppublisher (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublisher_TSQL
- sp_replmonitorhelppublisher
helpviewer_keywords:
- sp_replmonitorhelppublisher
ms.assetid: 171501fe-4b74-4647-96c3-7691c777e01b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6252047b509af4311045185197994426c1d019ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198286"
---
# <a name="sp_replmonitorhelppublisher-transact-sql"></a>sp_replmonitorhelppublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为与分发服务器关联的一个或多个发布服务器返回当前状态信息。 在分发服务器的分发数据库上执行此存储过程，用于监视复制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replmonitorhelppublisher [ [ @publisher = ] 'publisher' ]  
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 是要监视其状态的发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。 如果为 NULL，则返回使用分发服务器的所有发布服务器的信息。  
  
`[ @refreshpolicy = ] refreshpolicy` 仅限内部使用。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**distribution_db**|**sysname**|给定发布服务器使用的分发数据库的名称。|  
|**status**|**int**|与此发布服务器中的发布关联的所有复制代理的最大值状态，可以是下列值之一：<br /><br /> **1** = 已启动<br /><br /> **2** = 成功<br /><br /> **3** = 正在进行<br /><br /> **4** = 空闲<br /><br /> **5** = 正在重试<br /><br /> **6** = 失败|  
|**warning**|**int**|由属于此发布服务器上的某发布的订阅生成的最大阈值警告，可以是以下一个或多个值的“逻辑或”结果。<br /><br /> **1** = 过期-对事务发布的订阅尚未在保持期阈值内同步。<br /><br /> **2** = 滞后时间-将数据从事务发布服务器复制到订阅服务器所用的时间超过了阈值（以秒为单位）。<br /><br /> **4** = mergeexpiration-合并发布的订阅尚未在保持期阈值内同步。<br /><br /> **8** = mergefastrunduration-通过快速网络连接完成合并订阅同步所用的时间超过了阈值（以秒为单位）。<br /><br /> **16** = mergeslowrunduration-完成合并订阅同步所用的时间超过了慢速或拨号网络连接的阈值（以秒为单位）。<br /><br /> **32** = mergefastrunspeed-合并订阅的同步过程中的行传递速率未能维持快速网络连接上的阈值速率（以每秒行数为单位）。<br /><br /> **64** = mergeslowrunspeed-合并订阅的同步过程中的行传递速率未能维持慢速或拨号网络连接的阈值速率（以每秒行数为单位）。|  
|**publicationcount**|**int**|属于发布服务器的发布的数量。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replmonitorhelppublisher** 用于所有类型的复制。  
  
## <a name="permissions"></a>权限  
 只有分发服务器上 **sysadmin** 固定服务器角色的成员或分发数据库中 **db_owner** 或 **replmonitor** 固定数据库角色的成员才能执行 **sp_replmonitorhelppublisher**。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
