---
description: sys.columns (Transact-SQL)
title: sys.databases (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.columns_TSQL
- sys.columns
- columns_TSQL
- columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.columns catalog view
ms.assetid: 323ac9ea-fc52-4b8c-8a7e-e0e44f8ed86c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9a2d25739eaf041a5c69b52b8c0ce27dc56cde89
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477528"
---
# <a name="syscolumns-transact-sql"></a>sys.columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为包含列的对象（如视图或表）的每一列返回一行。 下面是包含列的对象类型的列表。  
  
-   表值程序集函数 (FT)  
  
-   内联表值 SQL 函数 (IF)  
  
-   内部表 (IT)  
  
-   系统表 (S)  
  
-   表值 SQL 函数 (TF)  
  
-   用户表 (U)  
  
-   视图 (V)  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|此列所属对象的 ID。|  
|name|**sysname**|列的名称。 在对象中是唯一的。|  
|column_id|**int**|列的 ID。 在对象中是唯一的。<br /><br /> 列 ID 可以不按顺序排列。|  
|system_type_id|**tinyint**|列的系统类型的 ID。|  
|user_type_id|**int**|用户定义的列类型的 ID。<br /><br /> 若要返回类型的名称，请在此列上联接到 [sys.databases](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) 目录视图。|  
|max_length|**smallint**|列的最大长度（字节）。<br /><br /> -1 = 列数据类型为 **varchar (max)**、 **nvarchar (max)**、 **varbinary (max)** 或 **xml**。<br /><br /> 对于 **text** 列，max_length 值将是16，或者是 sp_tableoption "text in row" 设置的值。|  
|精准率|**tinyint**|如果基于数值，则为该列的精度；否则为 0。|  
|scale|**tinyint**|如果基于数值，则为列的小数位数；否则为 0。|  
|collation_name|**sysname**|如果基于字符，则为该列排序规则的名称；否则为 NULL。|  
|is_nullable|**bit**|1 = 列可为空。|  
|is_ansi_padded|**bit**|1 = 如果列为字符、二进制或变量类型，则该列使用 ANSI_PADDING ON 行为。<br /><br /> 0 = 列不是字符、二进制或变量类型。|  
|is_rowguidcol|**bit**|1 = 列为声明的 ROWGUIDCOL。|  
|is_identity|**bit**|1 = 列具有标识值|  
|is_computed|**bit**|1 = 列为计算列。|  
|is_filestream|**bit**|1 = 列为 FILESTREAM 列。|  
|is_replicated|**bit**|1 = 列已复制。|  
|is_non_sql_subscribed|**bit**|1 = 列具有非 SQL Server 订阅服务器。|  
|is_merge_published|**bit**|1 = 列已合并发布。|  
|is_dts_replicated|**bit**|1 = 使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 复制列。|  
|is_xml_document|**bit**|1 = 内容为完整的 XML 文档。<br /><br /> 0 = 内容是文档片段，或列的数据类型不是 **xml**。|  
|xml_collection_id|**int**|如果列的数据类型为 **xml** ，并且已键入 xml，则为非零值。 该值将为包含列的验证 XML 架构命名空间的集合的 ID。<br /><br /> 0 = 没有 XML 架构集合。|  
|default_object_id|**int**|默认对象的 ID，无论该对象是单独的对象 [sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)，还是内联列级默认约束。 内联列级默认对象的 parent_object_id 列是对该表本身的反引用。<br /><br /> 0 = 无默认值。|  
|rule_object_id|**int**|使用 sys.sp_bindrule 绑定到列的独立规则的 ID。<br /><br /> 0 = 无独立规则。 有关列级检查约束，请参阅 [&#40;transact-sql&#41;sys.check_constraints ](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md)。|  
|is_sparse|**bit**|1 = 列为稀疏列。 有关详细信息，请参阅 [使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。|  
|is_column_set|**bit**|1 = 列为列集。 有关详细信息，请参阅 [使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。|  
|generated_always_type|**tinyint**|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 标识生成列值 (对于系统表中的列，将始终为 0) ：<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END<br /><br /> 有关详细信息，请参阅 [&#40;关系数据库&#41;的临时表 ](../../relational-databases/tables/temporal-tables.md)。|  
|generated_always_type_desc|**nvarchar(60)**|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 值的文本说明 `generated_always_type` (始终 NOT_APPLICABLE 系统表中的列)  <br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END|  
|encryption_type|**int**|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 加密类型：<br /><br /> 1 = 确定性加密<br /><br /> 2 = 随机加密|  
|encryption_type_desc|**nvarchar (64)**|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 加密类型说明：<br /><br /> 随机化<br /><br /> DETERMINISTIC|  
|encryption_algorithm_name|**sysname**|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 加密算法的名称。<br /><br /> 仅支持 AEAD_AES_256_CBC_HMAC_SHA_512。|  
|column_encryption_key_id|**int**|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> CEK 的 ID。|  
|column_encryption_key_database_name|**sysname**|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDW_md](../../includes/sssds-md.md)]。<br /><br /> 列加密密钥与列的数据库不同时存在的数据库的名称。 如果该键存在于与列相同的数据库中，则为 NULL。|  
|is_hidden|**bit**|**适用于**：[!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 指示是否隐藏列：<br /><br /> 0 = 常规、非隐藏、可见列<br /><br /> 1 = 隐藏列|  
|is_masked|**bit**|**适用于**：[!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 指示是否由动态数据掩码屏蔽列：<br /><br /> 0 = 常规、非掩码列<br /><br /> 1 = 列被屏蔽|  


 
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.all_columns &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [ &#40;Transact-sql 的 tem_columnssys.sys&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)  
  
