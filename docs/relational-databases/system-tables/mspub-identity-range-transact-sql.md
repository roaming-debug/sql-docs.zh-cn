---
description: MSpub_identity_range (Transact-SQL)
title: MSpub_identity_range (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpub_identity_range_TSQL
- MSpub_identity_range
dev_langs:
- TSQL
helpviewer_keywords:
- MSpub_identity_range system table
ms.assetid: 68746eef-32e1-42bc-aff0-9798cd0e88b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 526620d1fe5ed7c00eed23b4433f3f7630205153
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098537"
---
# <a name="mspub_identity_range-transact-sql"></a>MSpub_identity_range (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSpub_identity_range** 表提供标识范围管理支持。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**objid**|**int**|包含由复制管理的标识列的表的 ID。|  
|**range**|**bigint**|控制将在调整的订阅中分配的连续标识值的范围大小。|  
|**pub_range**|**bigint**|控制将在调整的发布中分配的连续标识值的范围大小。|  
|**current_pub_range**|**bigint**|发布使用的当前范围。 如果在 **sp_changearticle** 更改之后或在下一范围调整之前查看，则它可能与 *pub_range* 不同。|  
|**threshold**|**int**|用于控制分发代理何时分配新标识范围的百分比值。 当使用 " *阈值* " 中指定的百分比值时，分发代理将创建一个新的标识范围。|  
|**last_seed**|**bigint**|当前范围的下限。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
