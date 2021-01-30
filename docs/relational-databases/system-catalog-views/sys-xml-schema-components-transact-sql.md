---
description: sys.xml_schema_components (Transact-SQL)
title: sys.xml_schema_components (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xml_schema_components
- sys.xml_schema_components_TSQL
- sys.xml_schema_components
- xml_schema_components_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_components catalog view
ms.assetid: 70142d3a-f8b5-4ee2-8287-3935f0f67aa2
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: d34983fbd1e36a2e2ea2c295160bf907647d3432
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199271"
---
# <a name="sysxml_schema_components-transact-sql"></a>sys.xml_schema_components (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  针对 XML 架构的每个组件返回一行。 Namespace_id (**collection_id**， ) 是包含命名空间的复合外键。 对于命名的组件， **symbol_space**、 **名称**、 **scoping_xml_component_id**、 **is_qualified**、 **xml_namespace_id**、 **xml_collection_id** 的值是唯一的。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**xml_component_id**|**int**|数据库中 XML 架构组件的唯一 ID。|  
|**xml_collection_id**|**int**|包含此组件的命名空间的 XML 架构集合的 ID。|  
|**xml_namespace_id**|**int**|集合中的 XML 命名空间的 ID。|  
|**is_qualified**|**bit**|1 = 该组件具有显式命名空间限定符。<br /><br /> 0 = 这是一个本地范围的组件。 在这种情况下，对、namespace_id **collection_id** 是指 "无命名空间" **targetNamespace**。<br /><br /> 对于通配符组成部分，该值将等于 1。|  
|name |**nvarchar**<br /><br /> **(4000)**|XML 架构组件的唯一名称。 如果该组件未命名，则为 NULL。|  
|**symbol_space**|**char(1)**|此符号名称唯一的空间，基于 **种类**：<br /><br /> N = 无<br /><br /> T = 类型<br /><br /> E = 元素<br /><br /> M = 模型–组<br /><br /> A = 属性<br /><br /> G = 属性–组|  
|**symbol_space_desc**|**nvarchar**<br /><br /> **(60)**|基于 **类型** 的此符号名称唯一的空间说明：<br /><br /> NONE<br /><br /> TYPE<br /><br /> ELEMENT<br /><br /> MODEL_GROUP<br /><br /> ATTRIBUTE<br /><br /> ATTRIBUTE_GROUP|  
|**好**|**char(1)**|XML 架构组件的类型。<br /><br /> N = 任何类型（特殊的内部组件）<br /><br /> Z = 任意简单类型（特殊的内部组件）<br /><br /> P = Primitive 类型（内部类型）<br /><br /> S = 简单类型<br /><br /> L = 列表类型<br /><br /> U = 联合类型<br /><br /> C = 复杂的简单类型（派生自简单类型）<br /><br /> K = 复杂类型<br /><br /> E = 元素<br /><br /> M = 模型–组<br /><br /> W = 元素-通配符<br /><br /> A = 属性<br /><br /> G = 属性–组<br /><br /> V = 属性-通配符|  
|**kind_desc**|**nvarchar**<br /><br /> **(60)**|对 XML 架构组件类型的说明：<br /><br /> ANY_TYPE<br /><br /> ANY_SIMPLE_TYPE<br /><br /> PRIMITIVE_TYPE<br /><br /> SIMPLE_TYPE<br /><br /> LIST_TYPE<br /><br /> UNION_TYPE<br /><br /> COMPLEX_SIMPLE_TYPE<br /><br /> COMPLEX_TYPE<br /><br /> ELEMENT<br /><br /> MODEL_GROUP<br /><br /> ELEMENT_WILDCARD<br /><br /> ATTRIBUTE<br /><br /> ATTRIBUTE_GROUP<br /><br /> ATTRIBUTE_WILDCARD|  
|**派生**|**char(1)**|派生类型的派生方法：<br /><br /> N = 无（非派生）<br /><br /> X = 扩展<br /><br /> R = 限制<br /><br /> S = 替换|  
|**derivation_desc**|**nvarchar**<br /><br /> **(60)**|对派生类型的派生方法的说明：<br /><br /> NONE<br /><br /> EXTENSION<br /><br /> RESTRICTION<br /><br /> SUBSTITUTION|  
|**base_xml_component_id**|**int**|该组件派生自的组件的 ID。 如果不存在，则为 NULL。|  
|**scoping_xml_component_id**|**int**|范围组件的唯一 ID。 如果不存在（全局范围），则为 NULL。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [XML 架构 &#40;XML 类型系统&#41; 目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
