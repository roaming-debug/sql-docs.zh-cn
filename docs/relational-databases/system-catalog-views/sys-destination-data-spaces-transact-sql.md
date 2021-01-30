---
description: sys.destination_data_spaces (Transact-SQL)
title: sys.destination_data_spaces (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.destination_data_spaces
- destination_data_spaces_TSQL
- destination_data_spaces
- sys.destination_data_spaces_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.destination_data_spaces catalog view
ms.assetid: 92df932b-ad5c-43f8-81f4-b158823ab189
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: be85e170fd9a8c0d081a498516622fa2c91460a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203933"
---
# <a name="sysdestination_data_spaces-transact-sql"></a>sys.destination_data_spaces (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  分区方案的每个数据空间目标对应一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**partition_scheme_id**|**int**|分区到数据空间的分区架构的 ID。|  
|**destination_id**|**int**|分区架构内唯一的目标映射的 ID（从 1 开始的序号）。|  
|**data_space_id**|**int**|此架构的目标的数据映射到的数据空间的 ID。|  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
