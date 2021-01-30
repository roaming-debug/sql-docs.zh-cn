---
description: sysmergeschemaarticles (Transact-SQL)
title: sysmergeschemaarticles (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergeschemaarticles_TSQL
- sysmergeschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemaarticles system table
ms.assetid: b5085979-2f76-48e1-bf3b-765a84003dd9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 579acde4f82556f176e5923a66db48c1faafe27f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210199"
---
# <a name="sysmergeschemaarticles-transact-sql"></a>sysmergeschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  跟踪合并复制的仅限架构的项目。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|合并复制中仅限架构的项目的名称。|  
|type |**tinyint**|指示仅限架构的项目的类型，可以是以下类型之一：<br /><br /> **0x20** = 存储过程仅限架构的项目。<br /><br /> **0x40** = 查看仅限架构的项目或索引视图仅限架构的项目。|  
|**objid**|**int**|项目基对象的对象标识符。 可以是过程、视图、索引视图或用户定义函数的对象标识符。|  
|**artid**|**uniqueidentifier**|文章 ID。|  
|description|**nvarchar(255)**|项目的说明。|  
|**pre_creation_command**|**tinyint**|在订阅数据库中创建项目时采取的默认操作：<br /><br /> **0 =** 无-如果表已存在于订阅服务器上，则不执行任何操作。<br /><br /> **1** = 删除表，然后重新创建它。<br /><br /> **2** = 删除-基于子集筛选器中的 WHERE 子句发出删除。<br /><br /> **3** = 截断-与 **2** 相同，但不删除行。 不过，不要使用 WHERE 子句。|  
|**pubid**|**uniqueidentifier**|发布的唯一标识符。|  
|**status**|**tinyint**|指示仅限架构的项目的状态，可以是以下状态之一：<br /><br /> **1** = 未同步-用于发布表的初始处理脚本在下一次运行快照代理时运行。<br /><br /> **2** = 活动-已运行发布表的初始处理脚本。<br /><br /> **5** = 要添加 New_inactive。<br /><br /> **6** = New_active。|  
|**creation_script**|**nvarchar(255)**|用于创建目标表的可选项目架构预创建脚本的路径和名称。|  
|**schema_option**|**二进制 (8)**|给定的仅限架构的项目的架构生成选项的位图，它可以是以下一个或多个值的按位逻辑或结果：<br /><br /> **0x00** = 快照代理禁用脚本，并使用提供的 CreationScript。<br /><br /> **0x01** = 生成对象创建 (CREATE TABLE、创建过程等) 上。<br /><br /> **0x10** = 生成相应的聚集索引。<br /><br /> **0x20** = 将用户定义数据类型转换为基本数据类型。<br /><br /> **0x40** = 生成相应的非聚集索引。<br /><br /> **0x80** = 在主键上包含已声明的引用完整性。<br /><br /> **0x100** = 如果已定义，则复制表项目的用户触发器。<br /><br /> **0x200** = 复制 foreign key 约束。 如果被所引用的表不是发布的一部分，则不会复制已发布表的任何外键约束。<br /><br /> **0x400** = 复制 check 约束。<br /><br /> **0x800** = 复制默认值。<br /><br /> **0x1000** = 复制列级排序规则。<br /><br /> **0x2000** = 复制与已发布项目源对象关联的扩展属性。<br /><br /> **0x4000** = 复制表项目中定义的唯一键。<br /><br /> **0x8000** = 使用 ALTER table 语句将表项目上的主键和唯一键作为约束复制。<br /><br /> 有关 **schema_option** 的可能值的详细信息，请参阅 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。|  
|**destination_object**|**sysname**|订阅数据库中的目标对象名称。 该值仅应用于仅限架构的项目，例如，存储过程、视图和 UDF。|  
|**destination_owner**|**sysname**|订阅数据库中对象的所有者（如果不是 **dbo**）。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
