---
description: sys.service_queue_usages (Transact-SQL)
title: sys.service_queue_usages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.service_queue_usages
- sys.service_queue_usages_TSQL
- service_queue_usages
- service_queue_usages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_queue_usages catalog view
ms.assetid: d58dcdaf-f82d-43d9-941b-f520581442bf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 38f1bd8d054dfbc4b8a90ce9a9fa9554bee24685
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204488"
---
# <a name="sysservice_queue_usages-transact-sql"></a>sys.service_queue_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于服务与服务队列之间的每个引用，此目录视图将返回与其对应的一行。 一个服务只能与一个队列关联。 而一个队列可与多个服务关联。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**service_id**|**int**|服务的标识符。 在数据库中是唯一的。 不可为 NULL。|  
|**service_queue_id**|**int**|服务使用的服务队列的标识符。 不可为 NULL。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.databases &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-services-transact-sql.md)  
  
  
