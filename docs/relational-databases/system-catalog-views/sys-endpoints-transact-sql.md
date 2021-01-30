---
description: sys.endpoints (Transact-SQL)
title: sys.databases (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- endpoints
- sys.endpoints
- endpoints_TSQL
- sys.endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.endpoints catalog view
ms.assetid: e6dafa4e-e47e-43ec-acfc-88c0af53c1a1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee59a374da6b17f595adebceec0c816d1b72862a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203887"
---
# <a name="sysendpoints-transact-sql"></a>sys.endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在系统中创建的每个端点在都表中占一行。 始终只能有一个 SYSTEM 端点。  
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|终结点的名称。 在服务器中是唯一的。 不可为 null。|  
|**endpoint_id**|**int**|端点 ID。 在服务器中是唯一的。 ID 小于 65536 的端点是系统端点。 不可为 null。|  
|principal_id|**int**|被创建并拥有该端点的服务器主体的 ID。 可以为 Null。|  
|**protocol**|**tinyint**|端点协议。<br /><br /> 1 = HTTP<br /><br /> 2 = TCP<br /><br /> 3 = Name pipes<br /><br /> 4 = Shared memory<br /><br /> 5 = Virtual Interface Adapter (VIA)<br /><br /> 不可为 null。|  
|**protocol_desc**|**nvarchar(60)**|端点协议的说明。 可以为 null. 以下值之一：<br /><br /> **HTTP**<br /><br /> **TCP**<br /><br /> **NAMED_PIPES**<br /><br /> **SHARED_MEMORY**<br /><br /> **VIA** 注意： VIA 协议已弃用。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|  
|type |**tinyint**|端点负载的类型。<br /><br /> 1 = SOAP<br /><br /> 2 = TSQL<br /><br /> 3 = SERVICE_BROKER<br /><br /> 4 = DATABASE_MIRRORING<br /><br /> 不可为 null。|  
|**type_desc**|**nvarchar(60)**|端点负载类型的说明。 可以为 Null。 以下值之一：<br /><br /> **SOAP**<br /><br /> **TSQL**<br /><br /> **SERVICE_BROKER**<br /><br /> **DATABASE_MIRRORING**|  
|**state**|**tinyint**|端点的状态。<br /><br /> 0 = STARTED，侦听并处理请求。<br /><br /> 1 = STOPPED，侦听但不处理请求。<br /><br /> 2 = DISABLED，不进行侦听。<br /><br /> 默认状态为 1。 可以为 Null。|  
|**state_desc**|**nvarchar(60)**|端点状态的说明。<br /><br /> STARTED = 侦听并处理请求。<br /><br /> STOPPED = 侦听但不处理请求。<br /><br /> DISABLED = 不进行侦听。<br /><br /> 默认状态为 STOPPED。<br /><br /> 可以为 Null。|  
|**is_admin_endpoint**|**bit**|指示端点是否用于管理。<br /><br /> 0 = 非管理端点。<br /><br /> 1 = 端点为管理端点。<br /><br /> 不可为 null。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
