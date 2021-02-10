---
description: 'sys.dm_cluster_endpoints (Transact-sql) '
title: sys.dm_cluster_endpoints (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, big-data-clusters
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_cluster_endpoints
- dm_cluster_endpoints_TSQL
- dm_cluster_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cluster_endpoints dynamic management view
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-ver15||>=sql-server-linux-2017'
ms.openlocfilehash: 0a1b3dd8242301e080581272a569bf55c06427c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052128"
---
# <a name="sysdm_cluster_endpoints-transact-sql"></a>sys.dm_cluster_endpoints (Transact-sql) 
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|`sysname`|在 SQL 大数据群集中外部公开的服务的名称。 终结点的唯一标识符。 此视图的键。 不可为 null。 |  
|description|`nvarchar(4000)`|服务说明。 不可为 null。 |
|endpoint|`sysname`|终结点 url 或连接属性。 不可为 null。 |
|protocol_desc|`sysname`|终结点协议说明 |

## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。

## <a name="see-also"></a>另请参阅

[什么是 [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ss-nover.md)] ](../../big-data-cluster/big-data-cluster-overview.md)？
