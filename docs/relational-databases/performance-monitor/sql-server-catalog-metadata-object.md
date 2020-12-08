---
title: SQL Server - Catalog Metadata 对象 | Microsoft Docs
description: 了解 SQLServer:Catalog Metadata 性能对象，该对象提供针对 SQL Server 的目录元数据的计数器。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Catalog Metadata
ms.assetid: 665e63e6-4bd2-4091-92a5-327364db2f8d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a02c8d6253df487ebf5c01c8658f448ec680022c
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505811"
---
# <a name="sql-server-catalog-metadata-object"></a>SQL Server, 目录元数据对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
**SQLServer:Catalog Metadata** 性能对象提供针对 SQL Server 的目录元数据提供计数器。

下表介绍了 SQL Server **目录元数据** 性能对象。


|**SQL Server 目录元数据计数器**|说明|  
|-------------|-----------------|  
|**Cache Entries Count**|目录元数据缓存中的项数。|
|**Cache Entries Pinned Count**|目录元数据缓存中已固定的项数。|
|**Cache Hit Ratio**|目录元数据缓存中命中次数和查找次数之比。|
|**Cache Hit Ratio Base**|仅供内部使用。|

每个数据库都有该计数器的一个实例。

## <a name="see-also"></a>另请参阅  
[监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)
