---
description: sys.allocation_units (Transact-SQL)
title: sys.allocation_units (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.allocation_units_TSQL
- sys.allocation_units
- allocation_units_TSQL
- allocation_units
dev_langs:
- TSQL
helpviewer_keywords:
- sys.allocation_units catalog view
ms.assetid: ec9de780-68fd-4551-b70b-2d3ab3709b3e
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 06b45e2bb57f8b39595beee13014ac72b835e4e1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464678"
---
# <a name="sysallocation_units-transact-sql"></a>sys.allocation_units (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  数据库中的每个分配单元都在表中占一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|allocation_unit_id|**bigint**|分配单元的 ID。 在数据库中是唯一的。|  
|type|**tinyint**|分配单元的类型：<br /><br /> 0 = 已删除<br /><br /> 1 = 行内数据（所有数据类型，但 LOB 数据类型除外）<br /><br /> 2 = (LOB) 数据 (**text**、 **ntext**、 **image**、 **xml**、大值类型和 CLR 用户定义类型的大型对象) <br /><br /> 3 = 行溢出数据|  
|type_desc|**nvarchar(60)**|对分配单元类型的说明：<br /><br /> **删除**<br /><br /> **IN_ROW_DATA**<br /><br /> **LOB_DATA**<br /><br /> **ROW_OVERFLOW_DATA**|  
|container_id|**bigint**|与分配单元关联的存储容器的 ID。<br /><br /> 如果 type = 1 或 3，则 container_id = sys.partitions.hobt_id。<br /><br /> 如果 type 为 2，则 container_id = sys.partitions.partition_id。<br /><br /> 0 = 标记为要延迟删除的分配单元|  
|data_space_id|**int**|此分配单元所在文件组的 ID。|  
|total_pages|**bigint**|此分配单元分配或保留的总页数。|  
|used_pages|**bigint**|实际使用的总页数。|  
|data_pages|**bigint**|包含下列数据的已使用页的数目：<br /><br /> 行内数据<br /><br /> LOB 数据<br /><br /> 行溢出数据<br /><br /> <br /><br /> 请注意，返回的值不包括内部索引页面和分配管理页面。|  
  
> [!NOTE]  
>  在删除或重新生成大型索引时，或者在删除或截断大型表时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将延迟实际页释放及其关联锁，直至事务提交完毕为止。 延迟的删除操作不会立即释放已分配的空间。 因此，删除或截断大型对象之后立即由 sys.allocation_units 返回的值可能不会反映实际可用的磁盘空间。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
