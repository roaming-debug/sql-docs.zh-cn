---
description: MSsubscription_articles (Transact-SQL)
title: MSsubscription_articles (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_articles
- MSsubscription_articles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_articles system table
ms.assetid: dbc1737f-261e-4017-b9cd-703b9fc4ac78
author: cawrites
ms.author: chadam
ms.openlocfilehash: c8ae84063723c76745776a4e530f7fb8d63b008a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098504"
---
# <a name="mssubscription_articles-transact-sql"></a>MSsubscription_articles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSsubscription_articles** 表包含有关排队订阅中的项目的信息。 只为排队更新和以排队更新为故障转移的即时更新复制类型填充该表。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|为该项目服务的代理 ID。|  
|**artid**|**int**|**Sysarticles** 表中的项目 ID。|  
|**文章**|**sysname**|**Sysarticles** 表中项目的名称。|  
|**dest_table**|**sysname**|**Sysarticles** 表中的目标表的名称。|  
|**owner**|**sysname**|订阅的所有者。|  
|**cft_table**|**sysname**|该项目的冲突表名称（用于排队更新复制类型）。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
