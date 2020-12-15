---
description: ROUTINE_COLUMNS (Transact-SQL)
title: ROUTINE_COLUMNS (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- ROUTINE_COLUMNS_TSQL
- ROUTINE_COLUMNS
dev_langs:
- TSQL
helpviewer_keywords:
- ROUTINE_COLUMNS view
- INFORMATION_SCHEMA.ROUTINE_COLUMNS view
ms.assetid: 91dbc61b-e4c0-4826-976c-b2fce88b7793
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb990b9cb78d36bfe680562574eda0688422c7c3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462728"
---
# <a name="routine_columns-transact-sql"></a>ROUTINE_COLUMNS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为当前数据库中当前用户可访问的表值函数所返回的每一列返回一行。  
  
 若要从此视图中检索信息，请指定 INFORMATION_SCHEMA 的完全限定名称 **。**_view_name_。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|表值函数的目录或数据库名称。|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|包含表值函数的架构的名称。<br /><br /> <strong> \* \* 重要 \* 说明 \* </strong>不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|表值函数的名称。|  
|**COLUMN_NAME**|**nvarchar (** 128 **)**|列名称。|  
|**ORDINAL_POSITION**|**int**|列标识号。|  
|**COLUMN_DEFAULT**|**nvarchar (** 4000 **)**|列的默认值。|  
|**IS_NULLABLE**|**varchar (** 3 **)**|如果此列允许 NULL，则返回 YES。 否则，返回 NO。|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|系统提供的数据类型。|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|二进制数据、字符数据或文本和图像数据的最大长度（字符）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。 否则，返回 NULL。 有关详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。|  
|**CHARACTER_OCTET_LENGTH**|**int**|二进制数据、字符数据或文本和图像数据的最大长度（字节）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。 否则，返回 NULL。|  
|**NUMERIC_PRECISION**|**tinyint**|近似数字数据、精确数字数据、整数数据或货币数据的精度。 否则，返回 NULL。|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|近似数字数据、精确数字数据、整数数据或货币数据的精度基数。 否则，返回 NULL。|  
|**NUMERIC_SCALE**|**tinyint**|近似数字数据、精确数字数据、整数数据或货币数据的小数位数。 否则，返回 NULL。|  
|**DATETIME_PRECISION**|**smallint**|**Datetime** 和 ISO **integer** 数据类型的子类型代码。 对于其他数据类型，返回 NULL。|  
|**CHARACTER_SET_CATALOG**|**varchar (** 6 **)**|返回 **master**。 如果该列是字符数据或 **文本** 数据类型，则指示字符集所在的数据库。 否则，返回 NULL。|  
|**CHARACTER_SET_SCHEMA**|**varchar (** 3 **)**|始终返回 NULL。|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|如果此列为字符数据或 **文本** 数据类型，则返回字符集的唯一名称。 否则，返回 NULL。|  
|**COLLATION_CATALOG**|**varchar (** 6 **)**|始终返回 NULL。|  
|**COLLATION_SCHEMA**|**varchar (** 3 **)**|始终返回 NULL。|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|如果列为字符数据或 **文本** 数据类型，则返回排序顺序的唯一名称。 否则，返回 NULL。|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|如果此列是别名数据类型，则此列是在其中创建用户定义数据类型的数据库的名称。 否则，返回 NULL。|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|如果该列是用户定义的数据类型，则此列是包含用户定义数据类型的架构的名称。 否则，返回 NULL。<br /><br /> <strong> \* \* 重要 \* 说明 \* </strong>不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。|  
|**DOMAIN_NAME**|**nvarchar (** 128 **)**|如果列是用户定义数据类型，则此列是该用户定义数据类型的名称。 否则，返回 NULL。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
