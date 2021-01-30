---
description: sys.syscolumns (Transact-SQL)
title: " (Transact-sql) sys.sys列 |Microsoft Docs"
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-enginel, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscolumns
- sys.syscolumns_TSQL
- syscolumns_TSQL
- syscolumns
dev_langs:
- TSQL
helpviewer_keywords:
- syscolumns system table
- sys.syscolumns compatibility view
ms.assetid: 863fd87b-ff33-4ac5-9aa9-df21140681da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9a90b11d3bd6905fd71e253e27b03ef0e44e716
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180713"
---
# <a name="syssyscolumns-transact-sql"></a>sys.syscolumns (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  为每个表和视图中的每列返回一行，并为数据库中的存储过程的每个参数返回一行。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|列或过程参数的名称。|  
|**id**|**int**|此列所属表的对象 ID，或者与此参数关联的存储过程的 ID。|  
|**xtype**|**tinyint**|**Sys.databases** 中的物理存储类型。|  
|**typestat**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|扩展的用户定义数据类型的 ID。 如果数据类型的数字超过 32,767，则溢出或返回 NULL。|  
|**length**|**smallint**|**Sys** 中的最大物理存储长度。**类型**。|  
|**xprec**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xscale**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colid**|**smallint**|列 ID 或参数 ID。|  
|**xoffset**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bitpos**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**保护**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colstat**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cdefault**|**int**|此列的默认值的 ID。|  
|**域名**|**int**|此列的规则或 CHECK 约束的 ID。|  
|**数字**|**smallint**|过程分组时的子过程号。<br /><br /> 0 = 非过程项|  
|**colorder**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**autoval**|varbinary(8000)|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offset**|**smallint**|此列所在行的偏移量。|  
|**collationid**|**int**|列的排序规则的 ID。 对于非字符列，此值为 NULL。|  
|**status**|**tinyint**|用于说明列或参数的属性的位图：<br /><br /> 0x08 = 列允许空值。<br /><br /> 0x10 = 在添加 **varchar** 或 **varbinary** 列后，ANSI 填充已生效。 为 **varchar** 保留尾随空格，并保留 **varbinary** 列的尾随零。<br /><br /> 0x40 = 参数为 OUTPUT 参数。<br /><br /> 0x80 = 列为标识列。|  
|type |**tinyint**|**Sys** 中的物理存储类型。**类型**。|  
|**usertype**|**smallint**|**Sys.databases 类型** 的用户定义数据类型的 ID。 如果数据类型的数字超过 32,767，则溢出或返回 NULL。|  
|**printfmt**|**varchar(255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|此列的精度级别。<br /><br /> -1 = **xml** 或大值类型。|  
|**scale**|**int**|此列的小数位数。<br /><br /> NULL = 数据类型不是数值。|  
|**iscomputed**|**int**|指示列是否为计算列的标志：<br /><br /> 0 = 非计算列。<br /><br /> 1 = 计算列。|  
|**isoutparam**|**int**|指示过程参数是否为输出参数：<br /><br /> 1 = True<br /><br /> 0 = False|  
|**isnullable**|**int**|指示列是否允许空值：<br /><br /> 1 = True<br /><br /> 0 = False|  
|**归类**|**sysname**|列的排序规则的名称。 如果不是基于字符的列，则为 NULL。|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
