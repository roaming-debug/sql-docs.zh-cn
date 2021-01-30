---
description: sys.module_assembly_usages (Transact-SQL)
title: sys.module_assembly_usages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- module_assembly_usages_TSQL
- module_assembly_usages
- sys.module_assembly_usages_TSQL
- sys.module_assembly_usages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.module_assembly_usages catalog view
ms.assetid: b0f9ffab-6ac7-49d5-8369-477fa6b1c02b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f27e8d337cd7a732031405fbc3184748fbceefc0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191298"
---
# <a name="sysmodule_assembly_usages-transact-sql"></a>sys.module_assembly_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为每个模块到程序集引用返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|SQL 对象的对象标识号。 在数据库中是唯一的。|  
|**assembly_id**|**int**|创建此模块所基于的程序集的 ID。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
