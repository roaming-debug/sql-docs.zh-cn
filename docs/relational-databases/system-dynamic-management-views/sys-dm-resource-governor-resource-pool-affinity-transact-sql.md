---
description: sys.dm_resource_governor_resource_pool_affinity (Transact-SQL)
title: sys.dm_resource_governor_resource_pool_affinity (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_resource_pool_affinity_TSQL
- sys.dm_resource_governor_resource_pool_affinity
- dm_resource_governor_resource_pool_affinity
- dm_resource_governor_resource_pool_affinity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_resource_governor_resource_pool_affinity
- sys.dm_resource_governor_resource_pool_affinity
ms.assetid: a197ec19-a2ba-44f5-a4f2-3eee33ebd77d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d5fa2993e4ae716f72ad93bc388c466e69f00da3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097537"
---
# <a name="sysdm_resource_governor_resource_pool_affinity-transact-sql"></a>sys.dm_resource_governor_resource_pool_affinity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  跟踪资源池亲关联。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  
  
|Colmn 名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|Pool_id|**int**|资源池的 ID。 不可为 null。|  
|Processor_group|**smallint**|Windows 逻辑处理器组的 ID。 不可为 null。|  
|Scheduler_mask|**bigint**|表示与此池相关联的计划程序的二进制掩码。 不可为 null。|  
  
## <a name="remarks"></a>备注  
 使用 AUTO 的关联创建的池将不会在此视图中出现，因为它们没有关联。 有关详细信息，请参阅 [CREATE RESOURCE pool &#40;transact-sql&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md) 和 [ALTER Resource pool &#40;transact-sql&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md) 语句。  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_resource_governor_external_resource_pool_affinity (Transact SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)  
  
  
