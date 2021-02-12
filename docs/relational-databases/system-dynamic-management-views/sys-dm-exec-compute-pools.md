---
description: 'sys.dm_exec_compute_pools (Transact-sql) '
title: sys.dm_exec_compute_pools (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, big-data-clusters
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_compute_pools
- dm_exec_compute_pools_TSQL
- dm_exec_compute_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_compute_pools dynamic management view
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-ver15||>=sql-server-linux-2017'
ms.openlocfilehash: 7e9459c31529b8446b49b776ed009c429f904b18
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071521"
---
# <a name="sysdm_exec_compute_pools-transact-sql"></a>sys.dm_exec_compute_pools (Transact-sql) 
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|`sysname`|计算池的名称。 不可为 null。 返回 `default` 默认计算池的。 |
|compute_pool_id|`int`|池的唯一标识符。 此视图的键。|  
|location|`sysname`|SQL 大数据群集中的控制器的终结点。 不可为 null。 |

## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。

## <a name="see-also"></a>另请参阅

[什么是 [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ss-nover.md)] ](../../big-data-cluster/big-data-cluster-overview.md)？
