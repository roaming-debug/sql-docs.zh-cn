---
description: COLUMNS (Transact-SQL)
title: 列 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- COLUMNS
- COLUMNS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS view
- INFORMATION_SCHEMA.COLUMNS view
ms.assetid: bbf7ac4a-7444-4351-a590-a9f71e0bc495
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 510409a417ff35edb932b0689f6cfb17967ce171
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210268"
---
# <a name="columns-transact-sql"></a>COLUMNS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为当前数据库中当前用户可访问的每一列返回一行。  
  
 若要从这些视图中检索信息，请指定 **INFORMATION_SCHEMA**_.view_name_ 的完全限定名称。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|表限定符。|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|包含该表的架构的名称。<br /><br /> **&#42;&#42; 重要 &#42;&#42;** 不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|表名。|  
|**COLUMN_NAME**|**nvarchar (** 128 **)**|列名称。|  
|**ORDINAL_POSITION**|**int**|列标识号。|  
|**COLUMN_DEFAULT**|**nvarchar (** 4000 **)**|列的默认值。|  
|**IS_NULLABLE**|**varchar (** 3 **)**|列的为空性。 如果列允许 NULL，则该列将返回 YES。 否则，返回 NO。|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|系统提供的数据类型。|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|二进制数据、字符数据或文本和图像数据的最大长度（字符）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。 否则，返回 NULL。 有关详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。|  
|**CHARACTER_OCTET_LENGTH**|**int**|二进制数据、字符数据或文本和图像数据的最大长度（字节）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。 否则，返回 NULL。|  
|**NUMERIC_PRECISION**|**tinyint**|近似数字数据、精确数字数据、整数数据或货币数据的精度。 否则，返回 NULL。|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|近似数字数据、精确数字数据、整数数据或货币数据的精度基数。 否则，返回 NULL。|  
|**NUMERIC_SCALE**|**int**|近似数字数据、精确数字数据、整数数据或货币数据的小数位数。 否则，返回 NULL。|  
|**DATETIME_PRECISION**|**smallint**|**Datetime** 和 ISO **interval** 数据类型的子类型代码。 对于其他数据类型，返回 NULL。|  
|**CHARACTER_SET_CATALOG**|**nvarchar (** 128 **)**|返回 **master**。 如果该列是字符数据或 **文本** 数据类型，则指示字符集所在的数据库。 否则，返回 NULL。|  
|**CHARACTER_SET_SCHEMA**|**nvarchar (** 128 **)**|始终返回 NULL。|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|如果此列为字符数据或 **文本** 数据类型，则返回字符集的唯一名称。 否则，返回 NULL。|  
|**COLLATION_CATALOG**|**nvarchar (** 128 **)**|始终返回 NULL。|  
|**COLLATION_SCHEMA**|**nvarchar (** 128 **)**|始终返回 NULL。|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|如果列为字符数据或 **文本** 数据类型，则返回排序规则的唯一名称。 否则，返回 NULL。|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|如果此列是别名数据类型，则此列是在其中创建用户定义数据类型的数据库的名称。 否则，返回 NULL。|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|如果列是用户定义数据类型，则此列将返回该用户定义数据类型的架构名称。 否则，返回 NULL。<br /><br /> **&#42;&#42; 重要 &#42;&#42;** 不要使用 INFORMATION_SCHEMA 视图来确定数据类型的架构。 查找类型的架构的唯一可靠方式是使用 TYPEPROPERTY 函数。|  
|**DOMAIN_NAME**|**nvarchar (** 128 **)**|如果列是用户定义数据类型，则此列是该用户定义数据类型的名称。 否则，返回 NULL。|  
  
## <a name="remarks"></a>备注  
 INFORMATION_SCHEMA 的 **ORDINAL_POSITION** 列 **。列** 视图与 COLUMNS_UPDATED 函数所返回列的位模式不兼容。 若要获取与 COLUMNS_UPDATED 兼容的位模式，你必须在查询 INFORMATION_SCHEMA 时引用 COLUMNPROPERTY 系统函数的 **ColumnID** 属性 **。列** 视图。 例如：  
  
```  
USE AdventureWorks2012;  
GO  
SELECT TABLE_NAME, COLUMN_NAME, COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME), COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.sys字符集 &#40;Transact-sql&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)   
 [COLUMNS_UPDATED (Transact-SQL)](../../t-sql/functions/columns-updated-transact-sql.md)  
  
