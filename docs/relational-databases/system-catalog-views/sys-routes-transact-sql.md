---
description: sys.routes (Transact-SQL)
title: sys.databases (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- routes
- routes_TSQL
- sys.routes
- sys.routes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.routes catalog view
ms.assetid: 8fc65915-8bd6-425b-95d9-6a8468cb1e48
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: c056f72ea2ef662132d0f2063550956a48a3d947
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197811"
---
# <a name="sysroutes-transact-sql"></a>sys.routes (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  在目录视图中，每个路由对应一行。 Service Broker 使用路由定位服务的网络地址。   

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|路由名称，在数据库中唯一。 不可为 NULL。|  
|**route_id**|**int**|路由的标识符。 不可为 NULL。|  
|principal_id|**int**|拥有路由的数据库主体的标识符。 可以为 null.|  
|**remote_service_name**|**nvarchar(256)**|远程服务的名称。 可以为 null.|  
|**broker_instance**|**nvarchar(128)**|承载远程服务的 Broker 的标识符。 可以为 null.|  
|**lifetime**|**datetime**|路由过期的日期和时间。 注意，该值不使用本地时区， 而是显示 UTC 的过期时间。 可以为 null.|  
|**address**|**nvarchar(256)**|Service Broker 将远程服务的消息发送到的网络地址。 可以为 null. 对于 SQL 托管实例，地址必须是本地的。|  
|**mirror_address**|**nvarchar(256)**|在地址中指定的服务器镜像伙伴的网络地址。 可以为 null.|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
  
