---
description: sysarticleupdates (Transact-SQL)
title: sysarticleupdates (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticleupdates_TSQL
- sysarticleupdates
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticleupdates system table
ms.assetid: 11a53bcd-a215-4d0b-9db8-233981d3ef5d
author: cawrites
ms.author: chadam
ms.openlocfilehash: b7fbaed24bf7b17207d366bb55882a3eb92b8660
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178041"
---
# <a name="sysarticleupdates-transact-sql"></a>sysarticleupdates (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个支持即时更新订阅的项目在表中各占一行。 该表存储在复制的数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|为项目提供唯一 ID 号的标识列。|  
|**pubid**|**int**|项目所属发布的 ID。|  
|**sync_ins_proc**|**int**|处理插入同步事务的存储过程 ID。|  
|**sync_upd_proc**|**int**|处理更新同步事务的存储过程 ID。|  
|**sync_del_proc**|**int**|处理删除同步事务的存储过程 ID。|  
|**autogen**|**bit**|指示自动生成存储过程：<br /><br /> **0** = False，不自动。<br /><br /> **1** = True，自动。|  
|**sync_upd_trig**|**int**|项目表上自动版本控制触发器的 ID。|  
|**conflict_tableid**|**int**|冲突表的 ID。|  
|**ins_conflict_proc**|**int**|用于将冲突写入 **conflict_table** 的过程的 ID。|  
|**identity_support**|**bit**|指定使用排队更新时是否启用自动标识范围处理。 **0** 表示没有标识范围支持。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
