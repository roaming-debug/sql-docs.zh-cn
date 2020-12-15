---
description: SCHEMATA (Transact-SQL)
title: 架构 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SCHEMATA_TSQL
- SCHEMATA
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.SCHEMATA view
- SCHEMATA view
ms.assetid: 69617642-0f54-4b25-b62f-5f39c8909601
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 847e046c89a11ec8d26db47d33f6f605c24cef28
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474708"
---
# <a name="schemata-transact-sql"></a>SCHEMATA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为当前数据库中的每个架构返回一行。 若要从这些视图中检索信息，请指定 INFORMATION_SCHEMA 的完全限定名称 **。**_view_name_。 若要检索有关实例中所有数据库的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请查询 [Sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**CATALOG_NAME**|**sysname**|当前数据库的名称|  
|**SCHEMA_NAME**|**nvarchar (** 128 **)**|返回架构名称。|  
|**SCHEMA_OWNER**|**nvarchar (** 128 **)**|架构所有者名称。<br /><br /> **&#42;&#42; 重要 &#42;&#42;** 不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠的方式是查询 sys.objects 目录视图。|  
|**DEFAULT_CHARACTER_SET_CATALOG**|**varchar (** 6 **)**|始终返回 NULL。|  
|**DEFAULT_CHARACTER_SET_SCHEMA**|**varchar (** 3 **)**|始终返回 NULL。|  
|**DEFAULT_CHARACTER_SET_NAME**|**sysname**|返回默认字符集的名称。|  

**示例**  
下面的示例返回有关 master 数据库中的架构的信息：  
```sql  
SELECT * FROM master.INFORMATION_SCHEMA.SCHEMATA;
```  

## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.schemas (Transact-SQL)](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)   
 [sys.sys字符集 &#40;Transact-sql&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)  
  
