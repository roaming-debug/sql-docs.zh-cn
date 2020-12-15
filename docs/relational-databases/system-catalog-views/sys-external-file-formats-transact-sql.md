---
description: 'sys.external_file_formats (Transact-sql) '
title: sys.external_file_formats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a89efb2c-0a3a-4b64-9284-6e93263e29ac
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dc5bd073b8948adbe7a31eebb43b70fa3d4358aa
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473008"
---
# <a name="sysexternal_file_formats-transact-sql"></a>sys.external_file_formats (Transact-sql) 
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  对于、和的当前数据库中的每个外部文件格式，都包含一行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSDS](../../includes/sssds-md.md)] [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。  
  
 为服务器上的每个外部文件格式都包含一行 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|file_format_id|**int**|外部文件格式的对象 ID。||  
|name|**sysname**|文件格式的名称。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和中 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ，这对于数据库是唯一的。 在中 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，这对于服务器是唯一的。||  
|format_type|**tinyint**|文件格式类型。|DELIMITEDTEXT、RCFILE、ORC、PARQUET|  
|field_terminator|**nvarchar (10)**|对于 format_type = DELIMITEDTEXT，这是字段终止符。||  
|string_delimiter|**nvarchar (10)**|对于 format_type = DELIMITEDTEXT，这是字符串分隔符。||  
|date_format|**nvarchar(50)**|对于 format_type = DELIMITEDTEXT，这是用户定义的日期和时间格式。||  
|use_type_default|**bit**|对于 "format_type = 分隔文本"，指定当 PolyBase 将数据从 HDFS 文本文件导入到时如何处理缺失值 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。|0-将缺失值存储为字符串 "NULL"。<br /><br /> 1-将缺失值存储为列默认值。|  
|serde_method|**nvarchar(255)**|对于 format_type = RCFILE，这是序列化/反序列化方法。||  
|row_terminator|**nvarchar (10)**|对于 format_type = DELIMITEDTEXT，这是终止外部 Hadoop 文件中每行的字符串。|始终为 "\n"。|  
|encoding|**nvarchar (10)**|对于 format_type = DELIMITEDTEXT，这是用于外部 Hadoop 文件的编码方法。|始终为 "UTF8"。|  
|data_compression|**nvarchar(255)**|用于外部数据的数据压缩方法。|对于 format_type = DELIMITEDTEXT：<br /><br /> -"org. Org.apache.hadoop.io.compress.defaultcodec"。<br />-"org. Org.apache.hadoop.io.compress.gzipcodec"。<br /><br /> 对于 format_type = RCFILE：<br /><br /> -"org. Org.apache.hadoop.io.compress.defaultcodec"。<br /><br /> 对于 format_type = ORC：<br /><br /> -"org. Org.apache.hadoop.io.compress.defaultcodec"。<br />-"org. 为使用 org.apache.io.compress.snappycodec"。<br /><br /> 对于 format_type = PARQUET：<br /><br /> -"org. Org.apache.hadoop.io.compress.gzipcodec"。<br />-"org. 为使用 org.apache.io.compress.snappycodec"。|  
  
## <a name="permissions"></a>权限  
 目录视图中仅显示用户拥有的安全对象的元数据，或用户对其拥有某些权限的安全对象的元数据。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.external_data_sources &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)   
 [sys.external_tables &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](../../t-sql/statements/create-external-file-format-transact-sql.md)  
  
  
