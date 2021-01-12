---
description: sys.xml_schema_wildcards (Transact-SQL)
title: sys.xml_schema_wildcards (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.xml_schema_wildcards
- sys.xml_schema_wildcards_TSQL
- xml_schema_wildcards
- xml_schema_wildcards_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_wildcards catalog view
ms.assetid: 7cedfe9a-e99e-4777-8a28-98674b6e5cff
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2cc2f255b528e07d075b29a3aabc0a110e2ba867
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097814"
---
# <a name="sysxml_schema_wildcards-transact-sql"></a>sys.xml_schema_wildcards (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为每个 XML 架构组件返回一行，该组件是 Attribute-Wildcard (**类型** 的 **V**) 或 Element-Wildcard (**)** **类型**，两者都 **symbol_space** **N**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||从 [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md)中继承列。|  
|**process_content**|**char(1)**|指示处理内容的方式。<br /><br /> S = 严格验证（必须验证）<br /><br /> L = 非严格验证（在可能的情况下进行验证）<br /><br /> P = 跳过验证|  
|**process_content_desc**|**nvarchar(60)**|有关处理内容的方式的说明：<br /><br /> **STRICT_VALIDATION**<br /><br /> **LAX_VALIDATION**<br /><br /> **SKIP_VALIDATION**|  
|**disallow_namespaces**|**bit**|0 = 仅允许在 [sys.xml_schema_wildcard_namespaces](../../relational-databases/system-catalog-views/sys-xml-schema-wildcard-namespaces-transact-sql.md) 中枚举的命名空间。<br /><br /> 1 = 只禁止命名空间。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [XML 架构 &#40;XML 类型系统&#41; 目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
