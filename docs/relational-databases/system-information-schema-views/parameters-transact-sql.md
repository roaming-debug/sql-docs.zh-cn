---
description: PARAMETERS (Transact-SQL)
title: 参数 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- PARAMETERS_TSQL
- PARAMETERS
dev_langs:
- TSQL
helpviewer_keywords:
- PARAMETERS view
- INFORMATION_SCHEMA.PARAMETERS view
ms.assetid: 06ded0ca-7d21-4400-864a-b801e855b257
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 481e104f4762638fb4c71a548c3a390e1babe762
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478928"
---
# <a name="parameters-transact-sql"></a>PARAMETERS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对于当前用户在当前数据库中可以访问的用户定义函数或存储过程的每个参数，相应地返回一行。 对于函数，该视图还会返回一行返回值信息。  
  
 若要从这些视图中检索信息，请指定 INFORMATION_SCHEMA 的完全限定名称 **。**_view_name_。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**SPECIFIC_CATALOG**|**nvarchar (** 128 **)**|以此为参数的例程的目录名称。|  
|**SPECIFIC_SCHEMA**|**nvarchar (** 128 **)**|以此为参数的例程的架构名称。<br /><br /> <strong> \* \* 重要 \* 说明 \* </strong>不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。|  
|**SPECIFIC_NAME**|**nvarchar (** 128 **)**|以此为参数的例程的名称。|  
|**ORDINAL_POSITION**|**int**|参数的序号位置从 1 开始。 对于函数的返回值，则为 0。|  
|**PARAMETER_MODE**|**nvarchar (** 10 **)**|如果是输入参数，则返回 IN；如果是输出参数，则返回 OUT；如果是输入/输出参数，则返回 INOUT。|  
|**IS_RESULT**|**nvarchar (** 10 **)**|如果指示作为函数的例程的结果，则返回 YES。 否则，返回 NO。|  
|**AS_LOCATOR**|**nvarchar (** 10 **)**|如果声明为定位器，则返回 YES。 否则，返回 NO。|  
|**PARAMETER_NAME**|**nvarchar (** 128 **)**|参数的名称。 如果该名称对应于函数的返回值，则为 NULL。|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|系统提供的数据类型。|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|二进制或字符数据类型的最大长度（字符）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。 否则，返回 NULL。|  
|**CHARACTER_OCTET_LENGTH**|**int**|二进制或字符数据类型的最大长度（字节）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。 否则，返回 NULL。|  
|**COLLATION_CATALOG**|**nvarchar (** 128 **)**|始终返回 NULL。|  
|**COLLATION_SCHEMA**|**nvarchar (** 128 **)**|始终返回 NULL。|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|参数排序规则的名称。 如果不是一种字符类型，则返回 NULL。|  
|**CHARACTER_SET_CATALOG**|**nvarchar (** 128 **)**|参数字符集的编录名称。 如果不是一种字符类型，则返回 NULL。|  
|**CHARACTER_SET_SCHEMA**|**nvarchar (** 128 **)**|始终返回 NULL。|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|参数字符集的名称。 如果不是一种字符类型，则返回 NULL。|  
|**NUMERIC_PRECISION**|**tinyint**|近似数字数据、精确数字数据、整数数据或货币数据的精度。 否则，返回 NULL。|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|近似数字数据、精确数字数据、整数数据或货币数据的精度基数。 否则，返回 NULL。|  
|**NUMERIC_SCALE**|**tinyint**|近似数字数据、精确数字数据、整数数据或货币数据的小数位数。 否则，返回 NULL。|  
|**DATETIME_PRECISION**|**smallint**|如果参数类型为 **datetime** 或 **smalldatetime**，则精度（以秒为单位）。 否则，返回 NULL。|  
|**INTERVAL_TYPE**|**nvarchar (** 30 **)**|NULL。 留待将来使用。|  
|**INTERVAL_PRECISION**|**smallint**|NULL。 留待将来使用。|  
|**USER_DEFINED_TYPE_CATALOG**|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|**USER_DEFINED_TYPE_SCHEMA**|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|**USER_DEFINED_TYPE_NAME**|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|**SCOPE_CATALOG**|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|**SCOPE_SCHEMA**|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|**SCOPE_NAME**|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)  
  
