---
description: MSsub_identity_range (Transact-SQL)
title: MSsub_identity_range (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsub_identity_range_TSQL
- MSsub_identity_range
dev_langs:
- TSQL
helpviewer_keywords:
- MSsub_identity_range system table
ms.assetid: 26e20d28-14ed-44fc-af3b-4de386de4bb8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 98e4e7ac62890b95b99dd3020e83bc866aa1ad53
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092368"
---
# <a name="mssub_identity_range-transact-sql"></a>MSsub_identity_range (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSsub_identity_range** 表提供对订阅的标识范围管理支持。 此表存储在订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**objid**|**int**|包含由复制管理的标识列的表的 ID。|  
|**range**|**bigint**|控制将在调整时在订阅服务器中指派的连续标识值的范围大小。|  
|**last_seed**|**bigint**|当前范围的下限。|  
|**threshold**|**int**|用于控制分发代理何时分配新标识范围的百分比值。 当使用 " *阈值* " 中指定的百分比值时，分发代理将创建一个新的标识范围。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
