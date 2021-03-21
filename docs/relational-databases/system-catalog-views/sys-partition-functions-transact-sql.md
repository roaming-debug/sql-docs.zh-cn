---
description: sys.partition_functions (Transact-SQL)
title: sys.partition_functions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.partition_functions_TSQL
- partition_functions
- sys.partition_functions
- partition_functions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partition_functions catalog view
ms.assetid: 96515727-728b-4bea-804a-36ce915b8b75
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5757db5ee8f22b5a55368afa927be486e8d468fd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754917"
---
# <a name="syspartition_functions-transact-sql"></a>sys.partition_functions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的每个分区函数都占一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|分区函数的名称。 在该数据库中是唯一的。|  
|**function_id**|**int**|分区函数 ID。 在该数据库中是唯一的。|  
|type|**char(2)**|函数类型。<br /><br /> R = 范围|  
|**type_desc**|**nvarchar(60)**|函数类型。<br /><br /> RANGE|  
|**端数**|**int**|函数创建的分区数。|  
|**boundary_value_on_right**|**bit**|用于区域划分。<br /><br /> 1 = 边界值包括在边界的 RIGHT 区域内。<br /><br /> 0 = 边界值包括在边界的 LEFT 区域中。|  
|**is_system**||**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 1 = 对象用于全文索引片段。<br /><br /> 0 = 对象不用于全文索引片段。|  
|create_date|**datetime**|函数的创建日期。|  
|modify_date|**datetime**|上次使用 ALTER 语句修改函数的日期。|  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;分区函数目录视图 ](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.partition_range_values (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
 [sys.partition_parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md)  
  
  
