---
description: sys.dm_repl_schemas (Transact-SQL)
title: sys.dm_repl_schemas (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_repl_schemas_TSQL
- dm_repl_schemas
- sys.dm_repl_schemas_TSQL
- sys.dm_repl_schemas
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_schemas dynamic management function
ms.assetid: 6f5fefff-8492-4360-bd5b-a97287367914
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 549f5beaae81d181f391936d1acecbc4aeb5b77a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095065"
---
# <a name="sysdm_repl_schemas-transact-sql"></a>sys.dm_repl_schemas (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关通过复制发布的表列的信息。  
  
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**artcache_schema_address**|**varbinary(8)**|已发布的表项目的缓存架构结构的内存中地址。|  
|**tabid**|**bigint**|已复制的表的 ID。|  
|**indexid**|**smallint**|已发布的表上的聚集索引的 ID。|  
|**idSch**|**bigint**|表架构的 ID。|  
|**tabschema**|**nvarchar (510)**|表架构的名称。|  
|**ccTabschema**|**smallint**|表架构的字符长度。|  
|**tabname**|**nvarchar (510)**|已发布表的名称。|  
|**ccTabname**|**smallint**|已发布的表名的字符长度。|  
|**rowsetid_delete**|**bigint**|已删除的行的 ID。|  
|**rowsetid_insert**|**bigint**|已插入的行的 ID。|  
|**num_pk_cols**|**int**|主键列数。|  
|**pcitee**|**二进制 (8000)**|指向用于对计算列进行计算的查询表达式结构的指针。|  
|**re_numtextcols**|**int**|已复制的表中的二进制大型对象列数。|  
|**re_schema_lsn_begin**|**二进制 (8000)**|架构版本日志记录的开始日志序列号 (LSN)。|  
|**re_schema_lsn_end**|**二进制 (8000)**|架构版本日志记录的结束 LSN。|  
|**re_numcols**|**int**|已发布的列数。|  
|**re_colid**|**int**|发布服务器上的列标识符。|  
|**re_awcName**|**nvarchar (510)**|已发布的列的名称。|  
|**re_ccName**|**smallint**|列名中的字符数。|  
|**re_pk**|**tinyint**|已发布的列是否为主键的一部分。|  
|**re_unique**|**tinyint**|已发布的列是否为唯一索引的一部分。|  
|**re_maxlen**|**smallint**|已发布的列的最大长度。|  
|**re_prec**|**tinyint**|已发布的列的精度。|  
|**re_scale**|**tinyint**|已发布的列的小数位数。|  
|**re_collatid**|**bigint**|已发布的列的排序规则 ID。|  
|**re_xvtype**|**smallint**|已发布的列的类型。|  
|**re_offset**|**smallint**|已发布的列的偏移量。|  
|**re_bitpos**|**tinyint**|已发布的列的位位置（以字节向量表示）。|  
|**re_fNullable**|**tinyint**|指定已发布的列是否支持 NULL 值。|  
|**re_fAnsiTrim**|**tinyint**|指定是否对已发布的列使用 ANSI 剪裁。|  
|**re_computed**|**smallint**|指定已发布的列是否为计算列。|  
|**se_rowsetid**|**bigint**|行集的 ID。|  
|**se_schema_lsn_begin**|**二进制 (8000)**|架构版本日志记录的开始 LSN。|  
|**se_schema_lsn_end**|**二进制 (8000)**|架构版本日志记录的结束 LSN。|  
|**se_numcols**|**int**|列数。|  
|**se_colid**|**int**|订阅服务器上的列的 ID。|  
|**se_maxlen**|**smallint**|列的最大长度。|  
|**se_prec**|**tinyint**|列的精度。|  
|**se_scale**|**tinyint**|列的小数位数。|  
|**se_collatid**|**bigint**|列的排序规则 ID。|  
|**se_xvtype**|**smallint**|列的类型。|  
|**se_offset**|**smallint**|列的偏移量。|  
|**se_bitpos**|**tinyint**|列的位位置（以字节向量表示）。|  
|**se_fNullable**|**tinyint**|指定列是否支持 NULL 值。|  
|**se_fAnsiTrim**|**tinyint**|指定是否对列使用 ANSI 剪裁。|  
|**se_computed**|**smallint**|指定列是否为计算列。|  
|**se_nullBitInLeafRows**|**int**|指定列值是否为 NULL。|  
  
## <a name="permissions"></a>权限  
 要求对发布数据库具有 VIEW DATABASE STATE 权限才能调用 **dm_repl_schemas**。  
  
## <a name="remarks"></a>备注  
 只为复制项目缓存中当前加载的复制的数据库对象返回信息。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与复制相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)  
  
  

