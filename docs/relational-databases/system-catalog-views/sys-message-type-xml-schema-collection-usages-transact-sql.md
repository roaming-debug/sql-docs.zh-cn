---
description: sys.message_type_xml_schema_collection_usages (Transact-SQL)
title: sys.message_type_xml_schema_collection_usages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- message_type_xml_schema_collection_usages_TSQL
- sys.message_type_xml_schema_collection_usages_TSQL
- sys.message_type_xml_schema_collection_usages
- message_type_xml_schema_collection_usages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.message_type_xml_schema_collection_usages catalog view
ms.assetid: 544f61a1-c7b7-44b4-bf8d-980ba87d0665
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 425d48cf74ead30b156636f75eb7c301a18f259d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191315"
---
# <a name="sysmessage_type_xml_schema_collection_usages-transact-sql"></a>sys.message_type_xml_schema_collection_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  由 XML 架构集合验证的每种服务消息类型都在该目录视图中占一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**message_type_id**|**int**|服务消息类型的 ID。 不可为 NULL。|  
|**xml_collection_id**|**int**|包含验证 XML 架构命名空间的集合的 ID。 不可为 NULL。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
  
