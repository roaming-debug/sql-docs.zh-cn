---
description: sys.types (Transact-SQL)
title: sys.databases (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- types
- types_TSQL
- sys.types_TSQL
- sys.types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.types catalog view
- table-valued parameters,sys.types
ms.assetid: a5dbc842-71a0-4f62-b5e0-f560a99b7f8c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 09a7add1433bc9eb8184843f24d113e3706a4c36
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475088"
---
# <a name="systypes-transact-sql"></a>sys.types (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  每个系统类型和用户定义类型都在表中对应一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|类型的名称。 在架构内是唯一的。|  
|**system_type_id**|**tinyint**|类型的内部系统类型的 ID。|  
|**user_type_id**|**int**|类型的 ID。 在该数据库中是唯一的。 对于系统数据类型， **user_type_id**  =  **system_type_id**。|  
|schema_id|**int**|类型所属架构的 ID。|  
|principal_id|**int**|如果个体所有者与架构所有者不同，则表示该所有者的 ID。 默认情况下，架构包含的对象由架构所有者拥有。 不过，通过使用 ALTER AUTHORIZATION 语句更改所有权可以指定备用所有者。<br /><br /> 如果没有另外的个体所有者，则值为 NULL。|  
|**max_length**|**smallint**|类型的最大长度（字节）。<br /><br /> -1 = 列数据类型为 **varchar (max)**、 **nvarchar (max)**、 **varbinary (max)** 或 **xml**。<br /><br /> 对于 **text** 列， **max_length** 值将为16。|  
|**精度**|**tinyint**|如果类型基于数值，则表示类型的最大精度；否则，该值为 0。|  
|**scale**|**tinyint**|如果类型基于数值，则表示类型的最大小数位数；否则，该值为 0。|  
|**collation_name**|**sysname**|如果类型基于字符，则表示类型排序规则的名称；否则，该值为 NULL。|  
|**is_nullable**|**bit**|类型可以为 Null。|  
|**is_user_defined**|**bit**|1 = 用户定义类型。<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。|  
|**is_assembly_type**|**bit**|1 = 类型的实现是在 CLR 程序集中定义的。<br /><br /> 0 = 类型基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。|  
|**default_object_id**|**int**|使用 [sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)绑定到类型的独立默认的 ID。<br /><br /> 0 = 不存在默认值。|  
|**rule_object_id**|**int**|使用 [sp_bindrule](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)绑定到类型的独立规则的 ID。<br /><br /> 0 = 不存在规则。|  
|**is_table_type**|**bit**|指示该类型为表。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [&#40;Transact-sql&#41;的标量类型目录视图 ](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
