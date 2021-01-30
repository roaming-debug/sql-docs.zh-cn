---
description: sys.resource_usage (Azure SQL Database)
title: Azure SQL Database (sys.resource_usage) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.resource_usage_TSQL
- resource_usage
- sys.resource_usage
- resource_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- resource_usage
- sys.resource_usage
ms.assetid: b90147a3-fd8e-408e-961d-5c7000e068ad
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 1c9ef43ee6834a890359ea46bcab7ff388ec7d4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193012"
---
# <a name="sysresource_usage-azure-sql-database"></a>sys.resource_usage (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

    
> [!IMPORTANT]
>  此功能处于预览状态。 请不要依赖于此功能的特定实现，因为此功能在将来的版本中可能更改或删除。  
> 
>  当处于预览状态时，[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 运营团队可能针对此 DMV 打开和关闭数据集合：  
> 
>  -   如果打开，DMV 在聚合时将返回当前数据。  
> -   如果关闭，则 DMV 返回历史数据，这些数据可能是旧数据。  
  
 为当前服务器中的用户数据库提供资源使用情况数据的每小时摘要。 历史数据将保留 90 天。  
  
 对于每个用户数据库，以连续方式为每小时提供一行信息。 即使数据库在该小时内处于闲置状态，也有对应的一行，并且该数据库的 usage_in_seconds 值将为 0。 将相应地为该小时累积存储使用情况和 SKU 信息。  
  
|列|数据类型|说明|  
|-------------|---------------|-----------------|  
|time|**datetime**|时间 (UTC)（以小时增量表示）。|  
|database_name|**nvarchar**|用户数据库的名称。|  
|sku|**nvarchar**|SKU 的名称。 下面是可能的值：<br /><br /> Web<br /><br /> Microsoft Store<br /><br /> 基本<br /><br /> 标准<br /><br /> Premium|  
|usage_in_seconds|**int**|该小时内使用的 CPU 时间之和。<br /><br /> 注意：此列不推荐用于 V11，不适用于 V12。 **值始终设置为0。**|  
|storage_in_megabytes|**decimal**|该小时的最大存储大小，包括数据库数据、索引、存储过程和元数据。|  
  
## <a name="permissions"></a>权限  
 此视图可用于具有连接到虚拟 **master** 数据库的权限的所有用户角色。  
  
  
