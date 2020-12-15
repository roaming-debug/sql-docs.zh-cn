---
description: DOMAINS (Transact-SQL)
title: 域 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DOMAINS_TSQL
- DOMAINS
dev_langs:
- TSQL
helpviewer_keywords:
- DOMAINS view
- INFORMATION_SCHEMA.DOMAINS view
ms.assetid: f0b734d5-816f-4b10-a60c-615931b515c2
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1621d12ef0382f4df071d41a8bc3f84b447c8b20
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482451"
---
# <a name="domains-transact-sql"></a>DOMAINS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为当前数据库中可由当前用户访问的每个别名数据类型返回一行。  
  
 若要从这些视图中检索信息，请指定 INFORMATION_SCHEMA 的完全限定名称 **。**_view_name_。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|包含该别名数据类型的数据库。|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|包含别名数据类型的架构名称。<br /><br /> **&#42;&#42; 重要 &#42;&#42;** 不要使用 INFORMATION_SCHEMA 视图来确定数据类型的架构。 查找类型的架构的唯一可靠方式是使用 TYPEPROPERTY 函数。|  
|**DOMAIN_NAME**|**sysname**|别名数据类型。|  
|**DATA_TYPE**|**sysname**|系统提供的数据类型。|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|二进制数据、字符数据或文本和图像数据的最大长度（字符）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。 否则，返回 NULL。 有关详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。|  
|**CHARACTER_OCTET_LENGTH**|**int**|二进制数据、字符数据或文本和图像数据的最大长度（字节）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。 否则，返回 NULL。|  
|**COLLATION_CATALOG**|**varchar (** 6 **)**|始终返回 NULL。|  
|**COLLATION_SCHEMA**|**varchar (** 3 **)**|始终返回 NULL。|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|如果列为字符数据或 **文本** 数据类型，则返回排序顺序的唯一名称。 否则，返回 NULL。|  
|**CHARACTER_SET_CATALOG**|**varchar (** 6 **)**|返回 **master**。 如果该列是字符数据或 **文本** 数据类型，则指示字符集所在的数据库。 否则，返回 NULL。|  
|**CHARACTER_SET_SCHEMA**|**varchar (** 3 **)**|始终返回 NULL。|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|如果此列为字符数据或 **文本** 数据类型，则返回字符集的唯一名称。 否则，返回 NULL。|  
|**NUMERIC_PRECISION**|**tinyint**|近似数字数据、精确数字数据、整数数据或货币数据的精度。 否则，返回 NULL。|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|近似数字数据、精确数字数据、整数数据或货币数据的精度基数。 否则，返回 NULL。|  
|**NUMERIC_SCALE**|**tinyint**|近似数字数据、精确数字数据、整数数据或货币数据的小数位数。 否则，返回 NULL。|  
|**DATETIME_PRECISION**|**smallint**|**Datetime** 和 ISO **interval** 数据类型的子类型代码。 对于其他数据类型，该列返回 NULL。|  
|**DOMAIN_DEFAULT**|**nvarchar (** 4000 **)**|定义语句的实际文本 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.sys字符集 &#40;Transact-sql&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
