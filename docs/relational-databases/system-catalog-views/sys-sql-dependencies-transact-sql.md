---
description: sys.sql_dependencies (Transact-SQL)
title: sys.sql_dependencies (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sql_dependencies
- sql_dependencies_TSQL
- sys.sql_dependencies_TSQL
- sys.sql_dependencies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_dependencies catalog view
ms.assetid: 1779aa87-a0b8-470a-a286-d7cc0b93ad2e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2032bc1ba6658b18649f5908bebd4ae9405522b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99150704"
---
# <a name="syssql_dependencies-transact-sql"></a>sys.sql_dependencies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对在定义另一引用对象的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式或语句中引用的被引用的实体的每一依赖关系，均存在对应的一行。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改用 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) 。  

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|class |**tinyint**|标识被引用的实体的类：<br /><br /> 0 = 对象或列 (非绑定到架构的引用仅) <br /><br /> 1 = 对象或列（架构绑定引用）<br /><br /> 2 = 类型（架构绑定引用）<br /><br /> 3 = XML 架构集合（架构绑定引用）<br /><br /> 4 = 分区函数（架构绑定引用）|  
|**class_desc**|**nvarchar(60)**|被引用的实体的类的说明：<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_NON_SCHEMA_BOUND**<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_SCHEMA_BOUND**<br /><br /> **TYPE_REFERENCE**<br /><br /> **XML_SCHEMA_COLLECTION_REFERENCE**<br /><br /> **PARTITION_FUNCTION_REFERENCE**|  
|object_id|**int**|引用对象的 ID。|  
|column_id|**int**|如果引用 ID 是一列，则为引用列的 ID；否则为 0。|  
|**referenced_major_id**|**int**|被引用的实体的 ID，由类的值解释，具体如下：<br /><br /> 0、1 = 对象或列的对象 ID。<br /><br /> 2 = 类型 ID。<br /><br /> 3 = XML 架构集合 ID。|  
|**referenced_minor_id**|**int**|被引用实体的 Minor-ID，由类的值解释，如下所示：<br /><br /> 当 class =:<br /><br /> 0， **referenced_minor_id** 是列 id;如果不是列，则为0。<br /><br /> 1， **referenced_minor_id** 是列 id;如果不是列，则为0。<br /><br /> 否则， **referenced_minor_id** = 0。|  
|**is_selected**|**bit**|选中了对象或列。|  
|**is_updated**|**bit**|更新了对象或列。|  
|**is_select_all**|**bit**|对象用在了 SELECT * 子句中（仅限对象级）。|  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
