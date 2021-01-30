---
description: sys.xml_schema_collections (Transact-SQL)
title: sys.xml_schema_collections (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_collections_TSQL
- sys.xml_schema_collections
- xml_schema_collections
- xml_schema_collections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_collections catalog view
ms.assetid: f3f7f3dc-029f-4942-ab3c-75fa9814e40f
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8533727e5e59196b84625010f079e5a3864c9d0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192524"
---
# <a name="sysxml_schema_collections-transact-sql"></a>sys.xml_schema_collections (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为每个 XML 架构集合返回一行。 XML 架构集合是一组命名的 XSD 定义。 XML 架构集合自身包含在关系架构中，由架构范围内的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 名称标识。 下列元组是唯一的：xml_collection_id、以及 schema_id 和 name。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|xml_collection_id|**int**|XML 架构集合的 ID。 在数据库中是唯一的。|  
|schema_id|**int**|包含此 XML 架构集合的关系架构的 ID。|  
|principal_id|**int**|如果不是架构所有者，则为特定所有者的 ID。 默认情况下，架构包含的对象由架构所有者拥有。 但是，可以通过使用 ALTER AUTHORIZATION 语句更改所有权来指定备用所有者。<br /><br /> NULL = 没有代替的单个所有者。|  
|name|**sysname**|XML 架构集合的名称。|  
|create_date|**datetime**|创建 XML 架构集合的日期。|  
|modify_date|**datetime**|上次修改 XML 架构集合的日期。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [XML 架构 &#40;XML 类型系统&#41; 目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
