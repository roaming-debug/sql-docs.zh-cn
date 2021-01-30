---
description: sys.all_parameters (Transact-SQL)
title: sys.all_parameters (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- all_parameters_TSQL
- sys.all_parameters
- all_parameters
- sys.all_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_parameters catalog view
ms.assetid: eecbb68e-9b4c-4243-94e2-8096a9cc7892
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 17c1c62c582a4e8937e82b7ba911955a6805ab26
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205545"
---
# <a name="sysall_parameters-transact-sql"></a>sys.all_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  显示属于用户定义对象或系统对象的所有参数的并集。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|此参数所属对象的 ID。|  
|name|**sysname**|参数的名称， 在对象中是唯一的。 如果对象是标量函数，则参数名称为表示返回值的行中的空字符串。|  
|**parameter_id**|**int**|参数的 ID， 在对象中是唯一的。 如果对象是标量函数，则 **parameter_id** = 0 表示返回值。|  
|**system_type_id**|**tinyint**|参数的系统类型的 ID。|  
|**user_type_id**|**int**|用户定义的参数类型的 ID。<br /><br /> 若要返回类型的名称，请在此列上联接到 [sys.databases](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) 目录视图。|  
|**max_length**|**smallint**|参数的最大长度（以字节为单位）。<br /><br /> -1 = 列数据类型为 **varchar (max)**、 **nvarchar (max)**、 **varbinary (max)** 或 **xml**。|  
|**精度**|**tinyint**|如果参数是数值，则为该参数的精度；否则为 0。|  
|**scale**|**tinyint**|如果参数是数值，则为该参数的小数位数；否则为 0。|  
|is_output|**bit**|1 = 参数为输出值（或返回值）；否则为 0。|  
|**is_cursor_ref**|**bit**|1 = 参数为游标引用参数。|  
|**has_default_value**|**bit**|1 = 参数有默认值。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 只维护该目录视图中的 CLR 对象的默认值；因此，对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 对象，此列始终包含 0 值。 若要查看对象中参数的默认值 [!INCLUDE[tsql](../../includes/tsql-md.md)] ，请查询 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)目录视图的 "**定义**" 列，或使用 [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md)系统函数。|  
|**is_xml_document**|**bit**|1 = 内容为完整的 XML 文档。<br /><br /> 0 = 内容是文档片段，或列的数据类型不是 **xml**。|  
|**default_value**|**sql_variant**|如果 **has_default_value** 为1，则此列的值为参数的默认值; 否则为。否则为 NULL。|  
|**xml_collection_id**|**int**|用于验证参数的 XML 架构集合的 ID。<br /><br /> 如果参数的数据类型为 **xml** ，并且已键入 xml，则为非零值。<br /><br /> 0 = 没有 XML 架构集合，或参数不为 XML。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys. parameters &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)   
 [ &#40;Transact-sql 的 tem_parameterssys.sys&#41;](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md)  
  
  
