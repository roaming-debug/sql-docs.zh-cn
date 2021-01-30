---
description: MSmerge_replinfo (Transact-SQL)
title: MSmerge_replinfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_replinfo_TSQL
- MSmerge_replinfo
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_replinfo system table
ms.assetid: b0924094-c0cc-49c1-869a-65be0d0465a0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 57e1f5f193a4861dba284ed8b11adb6fa394531f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205923"
---
# <a name="msmerge_replinfo-transact-sql"></a>MSmerge_replinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个订阅在 **MSmerge_replinfo** 表中各占一行。 此表可追踪有关订阅的信息。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**repid**|**uniqueidentifier**|副本的唯一 ID。|  
|**use_interactive_resolver**|**bit**|指定调解期间是否使用交互式冲突解决程序。<br /><br /> **0** = 不使用交互式冲突解决程序。<br /><br /> **1** = 使用交互式冲突解决程序。|  
|**validation_level**|**int**|对订阅执行的验证类型。 指定的验证级别可以是以下值之一：<br /><br /> **0** = 无验证。<br /><br /> **1** = 仅限行计数验证。<br /><br /> **2** = 行计数和校验和验证。<br /><br /> **3** = 行计数和二进制校验和验证。|  
|**resync_gen**|**bigint**|用于重新同步订阅的生成数。 如果值为 **-1** ，则表示订阅未标记为重新同步。|  
|login_name|**sysname**|创建订阅的用户名。|  
|**hostname**|**sysname**|为订阅生成分区时由参数化行筛选器使用的值。|  
|**merge_jobid**|**binary(16)**|此订阅的合并作业 ID。|  
|**sync_info**|**int**|仅供内部使用。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
