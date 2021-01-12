---
description: sys.partition_parameters (Transact-SQL)
title: sys.partition_parameters (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- partition_parameters_TSQL
- partition_parameters
- sys.partition_parameters_TSQL
- sys.partition_parameters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partition_parameters catalog view
ms.assetid: 2012ed9d-3ea3-4c29-9b78-dfa54a392dce
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ec24418a893f488e67249222bfbf75bc1096813c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096823"
---
# <a name="syspartition_parameters-transact-sql"></a>sys.partition_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  每个分区函数的参数在表中对应一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**function_id**|**int**|此参数所属分区函数的 ID。|  
|**parameter_id**|**int**|参数的 ID。 在分区函数中是唯一的，从 1 开始。|  
|**system_type_id**|**tinyint**|参数的系统类型的 ID。 对应于 **sys.databases** 目录视图的 **system_type_id** 列。|  
|**max_length**|**smallint**|参数的最大长度（字节）。|  
|**精度**|**tinyint**|如果参数是基于数值的，则表示参数的精度；否则为 0。|  
|**scale**|**tinyint**|如果参数是基于数值的，则表示参数的小数位数；否则为 0。|  
|**collation_name**|**sysname**|如果参数基于字符，则为参数的排序规则名称；否则为 NULL。|  
|**user_type_id**|**int**|类型的 ID。 在该数据库中是唯一的。 对于系统数据类型， **user_type_id**  =  **system_type_id**。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;分区函数目录视图 ](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.partition_functions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
 [sys.partition_range_values (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)  
  
  
