---
description: sysschemaarticles (Transact-SQL)
title: sysschemaarticles (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysschemaarticles_TSQL
- sysschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysschemaarticles system table
ms.assetid: 67a1c039-c283-4a9c-bacc-b9b3973590c3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7a4dc32b20c4b17c683df1a1e1aea88ee8559be4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180370"
---
# <a name="sysschemaarticles-transact-sql"></a>sysschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  跟踪事务发布和快照发布的纯架构项目。 该表存储在发布数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|文章 ID。|  
|**creation_script**|**nvarchar(255)**|用于创建目标表的项目架构脚本的路径和名称。|  
|description|**nvarchar(255)**|项目的描述性条目。|  
|**dest_object**|**sysname**|如果项目是纯架构项目（如存储过程、视图或 UDF），则为订阅数据库中的对象名称。|  
|name|**sysname**|发布中的纯架构项目的名称。|  
|**objid**|**int**|项目基对象的对象标识符。 它可以是过程、视图、索引视图或 UDF 的对象标识符。|  
|**pubid**|**int**|发布的 ID。|  
|**pre_creation_cmd**|**tinyint**|指定当应用该项目的快照时，如果系统在订阅服务器上检测到同名的现有对象，系统应采取什么操作：<br /><br /> **0** = 无。<br /><br /> **1** = 删除目标表。<br /><br /> **2** = 删除目标表。<br /><br /> **3** = 截断目标表。|  
|**status**|**int**|用于指示项目状态的位图。|  
|type |**tinyint**|指示纯架构项目类型的值：<br /><br /> **32** = 存储过程。<br /><br /> **64** = 视图或索引视图。 <br /><br /> **96** = 聚合函数。<br /><br /> **128** = 函数。|  
|**schema_option**|**二进制 (8)**|给定项目的架构生成选项的位掩码。 它指定在目标数据库中为所有 CALL/MCALL/XCALL 语法自动创建存储过程，也可以是以下一个或多个值按位执行逻辑或运算的结果：<br /><br /> **0x00** = 快照代理禁用脚本，并使用 *creation_script*。<br /><br /> **0x01** = (CREATE TABLE、CREATE PROCEDURE) 生成对象创建。 该值是存储过程项目的默认值。<br /><br /> **0x02** = 生成项目的自定义存储过程（如果已定义）。<br /><br /> **0x10** = 生成相应的聚集索引。<br /><br /> **0x20** = 将用户定义数据类型转换为基本数据类型。<br /><br /> **0x40**= 生成相应的非聚集索引 (es) 。<br /><br /> **0x80**= 包含主键上已声明的引用完整性。<br /><br /> **0x73** = 生成 CREATE TABLE 语句，创建聚集索引和非聚集索引，将用户定义数据类型转换为基本数据类型，并生成要在订阅服务器上应用的自定义存储过程脚本。 该值是除存储过程项目以外的所有项目的默认值。<br /><br /> **0x100**= 如果已定义，则复制表项目的用户触发器。<br /><br /> **0x200**= 复制 foreign key 约束。 如果所引用的表不是发布的一部分，则不会复制已发布表的任何外键约束。<br /><br /> **0x400**= 复制 check 约束。<br /><br /> **0x800**= 复制默认值。<br /><br /> **0x1000**= 复制列级排序规则。<br /><br /> **0x2000**= 复制与已发布项目源对象关联的扩展属性。<br /><br /> **0x4000**= 在对表项目定义时复制唯一键。<br /><br /> **0x8000**= 使用 ALTER table 语句将表项目上的主键和唯一键作为约束复制。|  
|**dest_owner**|**sysname**|目标数据库中表的所有者。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
