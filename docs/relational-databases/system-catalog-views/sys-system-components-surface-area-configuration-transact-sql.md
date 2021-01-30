---
description: sys.system_components_surface_area_configuration (Transact-SQL)
title: sys.system_components_surface_area_configuration (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.system_components_surface_area_configuration_TSQL
- system_components_surface_area_configuration
- system_components_surface_area_configuration_TSQL
- sys.system_components_surface_area_configuration
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_components_surface_area_configuration catalog view
ms.assetid: d9920008-3387-4f9e-8f21-47473f2ba04f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 07cce3c6c49609843baa1b2cb5c3a463260a3ab4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195152"
---
# <a name="syssystem_components_surface_area_configuration-transact-sql"></a>sys.system_components_surface_area_configuration (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于可通过外围应用配置器组件启用或禁用的每个可执行系统对象返回一行。 有关详细信息，请参阅 [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**component_name**|**sysname**|组件名称。 这将使用关键字排序规则 Latin1_General_CI_AS_KS_WS。 不能为 NULL。|  
|**database_name**|**sysname**|包含此对象的数据库。 这将使用关键字排序规则 Latin1_General_CI_AS_KS_WS。 必须是下列选项之一：<br /><br /> 主<br /><br /> **msdb**<br /><br /> **mssqlsystemresource**|  
|**schema_name**|**sysname**|包含此对象的架构。 这将使用关键字排序规则 Latin1_General_CI_AS_KS_WS。 不能为 NULL。|  
|**object_name**|**sysname**|对象的名称。 这将使用关键字排序规则 Latin1_General_CI_AS_KS_WS。 不能为 NULL。|  
|**state**|**tinyint**|0 - 禁用<br /><br /> 1 = 启用|  
|type|**char(2)**|对象类型。 可以是以下其中一个值：<br /><br /> P = SQL_STORED_PROCEDURE<br /><br /> PC = CLR_STORED_PROCEDURE<br /><br /> FN = SQL_SCALAR_FUNCTION<br /><br /> FS = CLR_SCALAR_FUNCTION<br /><br /> FT = CLR_TABLE_VALUED_FUNCTION<br /><br /> IF = SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> TF = SQL_TABLE_VALUED_FUNCTION<br /><br /> X = EXTENDED_STORED_PROCEDURE|  
|**type_desc**|**nvarchar(60)**|对于对象类型的友好名称的说明。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
