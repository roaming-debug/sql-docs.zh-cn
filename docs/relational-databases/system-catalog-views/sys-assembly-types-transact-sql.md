---
description: sys.assembly_types (Transact-SQL)
title: sys.assembly_types (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- assembly_types
- sys.assembly_types
- sys.assembly_types_TSQL
- assembly_types_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_types catalog view
ms.assetid: 35f0384f-7a6d-41b1-9461-f1406d68f317
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e97d8409852ea5c65b212d77944921a73b69be05
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752227"
---
# <a name="sysassembly_types-transact-sql"></a>sys.assembly_types (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  由 CLR 程序集定义的每个用户定义的类型对应一行。 以下 **sys.assembly_types** 会出现在继承列的列表中 (在 **)** 之后 [&#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) rule_object_id 中看到 sys.databases。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|从中创建此类型的程序集的 ID。|  
|**assembly_class**|**sysname**|程序集内定义此类型的类的名称。|  
|**is_binary_ordered**|**bit**|对此类型的字节进行排序等同于对该类型使用比较运算符进行排序。|  
|**is_fixed_length**|**bit**|类型的长度始终与 max_length 相同。|  
|**prog_id**|**nvarchar(40)**|向 COM 公开的类型的 ProgID。|  
|**assembly_qualified_name**|**nvarchar(4000)**|程序集限定类型名。 该名称使用适合传递到 Type.GetType() 的格式。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [&#40;Transact-sql&#41;的标量类型目录视图 ](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)  
  
  
