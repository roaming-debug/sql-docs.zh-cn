---
description: sys.systypes (Transact-SQL)
title: Transact-sql)  (sys.sys类型 |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.systypes_TSQL
- systypes_TSQL
- sys.systypes
- systypes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.systypes compatibility view
- systypes system table
ms.assetid: 1b0b1d0c-5f7b-470b-bd52-8bfa922d7889
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9bd2a68f151a136a5bb9de7efd170c7178d8001e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464668"
---
# <a name="syssystypes-transact-sql"></a>sys.systypes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为数据库中定义的每种系统提供的数据类型和每种用户定义的数据类型返回一行。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|数据类型名称。|  
|**xtype**|**tinyint**|物理存储类型。|  
|**status**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|扩展用户类型。 如果数据类型的数字超过 32,767，则溢出或返回 NULL。|  
|**length**|**smallint**|数据类型的物理长度。|  
|**xprec**|**tinyint**|服务器使用的内部精度。 不在查询中使用。|  
|**xscale**|**tinyint**|服务器使用的内部小数位数。 不在查询中使用。|  
|**tdefault**|**int**|特定存储过程的 ID，此存储过程包含对该数据类型的完整性检查功能。|  
|**域**|**int**|特定存储过程的 ID，此存储过程包含对该数据类型的完整性检查功能。|  
|**标识号**|**smallint**|所有者类型的架构 ID。<br /><br /> 对于从旧版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级的数据库，架构 ID 等于所有者的用户 ID。<br /><br /> **\* \* 重要 \* 提示 \*** 如果使用以下任意 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DDL 语句，则必须使用 [sys.databases](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)目录视图，而不是 **sys.sys类型**。<br /><br /> ALTER AUTHORIZATION ON TYPE<br /><br /> CREATE TYPE<br /><br /> 如果用户数和角色数超过 32,767，则发生溢出或返回 NULL。|  
|**保护**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**collationid**|**int**|如果基于字符，则 **collationid** 是当前数据库的排序规则的 id;否则为 NULL。|  
|**usertype**|**smallint**|用户类型 ID。 如果数据类型的数字超过 32,767，则溢出或返回 NULL。|  
|variable|**bit**|可变长度数据类型。<br /><br /> 1 = True<br /><br /> 0 = False|  
|**allownulls**|**bit**|指示此数据类型的默认为空性。 如果使用 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 或 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)指定了可为 null 性，则会重写此默认值。|  
|**type**|**tinyint**|物理存储数据类型。|  
|**printfmt**|**varchar(255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|此数据类型的精度级别。<br /><br /> -1 = **xml** 或大值类型。|  
|**scale**|**tinyint**|此数据类型根据精度确定的小数位数。<br /><br /> NULL = 数据类型不是数值。|  
|**归类**|**sysname**|如果基于字符，则 **排序规则** 是当前数据库的排序规则;否则为 NULL。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的兼容性视图 &#40;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
