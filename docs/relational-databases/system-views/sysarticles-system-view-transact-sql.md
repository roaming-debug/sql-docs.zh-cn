---
description: sysarticles（系统视图）(Transact-SQL)
title: sysarticles (系统视图)  (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticles
- sysarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticles view
ms.assetid: 18f8c9b3-cab7-4e8f-8754-11ac38c3f789
author: stevestein
ms.author: sstein
ms.openlocfilehash: 21e1ca1548eee3fd9b17674cdf8ee7d32845b43f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160337"
---
# <a name="sysarticles-system-view-transact-sql"></a>sysarticles（系统视图）(Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **Sysarticles** 视图显示项目属性。 此视图存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|为项目提供唯一 ID 号的标识列。|  
|**creation_script**|**nvarchar(255)**|项目的架构脚本。|  
|**del_cmd**|**nvarchar(255)**|在 DELETE 时所执行的命令；否则根据日志构造。|  
|description|**nvarchar(255)**|项目的描述性条目。|  
|**dest_table**|**sysname**|目标表的名称。|  
|**filter**|**int**|存储过程 ID，用于水平分区。|  
|**filter_clause**|**ntext**|项目的 WHERE 子句，用于水平筛选。|  
|**ins_cmd**|**nvarchar(255)**|在 INSERT 中执行的命令；否则从日志构造。|  
|name|**sysname**|与项目关联的名称，在发布内是唯一的。|  
|**objid**|**int**|已发布的表对象 ID。|  
|**pubid**|**int**|项目所属发布的 ID。|  
|**pre_creation_cmd**|**tinyint**|DROP TABLE、DELETE TABLE 或 TRUNCATE 的预创建命令：<br /><br /> **0** = 无。<br /><br /> **1** = 删除。<br /><br /> **2** = 删除。<br /><br /> **3** = 截断。|  
|**status**|**tinyint**|项目选项和状态的位掩码，可以是对以下一个或多个值执行逻辑位或运算的结果：<br /><br /> **1** = 项目处于活动状态。<br /><br /> **8** = 在 INSERT 语句中包括列名称。<br /><br /> **16** = 使用参数化语句。<br /><br /> **24** = 在 INSERT 语句中包括列名称，并使用参数化语句。<br /><br /> **64** = 项目的水平分区由可转换的订阅定义。<br /><br /> 例如，使用参数化语句的活动项目在此列中的值为 **17** 。 如果值为 **0** ，则表示项目处于非活动状态，而且未定义其他属性。|  
|**sync_objid**|**int**|表示项目定义的表或视图的 ID。|  
|type |**tinyint**|项目的类型：<br /><br /> **1** = 基于日志的项目。<br /><br /> **3** = 具有手动筛选器的基于日志的项目。<br /><br /> **5** = 具有手动视图的基于日志的项目。<br /><br /> **7** = 具有手动筛选器和手动视图的基于日志的项目。<br /><br /> **8** = 存储过程执行。<br /><br /> **24** = 可序列化存储过程执行。<br /><br /> **32** = 存储过程仅)  (架构。<br /><br /> **64** = 仅) 查看 (架构。<br /><br /> **128** = 仅) 函数 (架构。|  
|**upd_cmd**|**nvarchar(255)**|在 UPDATE 时执行的命令；否则根据日志构造。|  
|**schema_option**|**二进制 (8)**|项目的架构生成选项的位掩码，这些选项用于控制项目架构的哪些部分可以进行脚本处理，以便传递给订阅服务器。 有关架构选项的详细信息，请参阅 [sp_addarticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。|  
|**dest_owner**|**sysname**|目标数据库中表的所有者。|  
|**ins_scripting_proc**|**int**|复制 INSERT 语句时执行的已注册自定义存储过程或脚本。|  
|**del_scripting_proc**|**int**|复制 DELETE 语句时执行的已注册自定义存储过程或脚本。|  
|**upd_scripting_proc**|**int**|复制 UPDATE 语句时执行的已注册自定义存储过程或脚本。|  
|**custom_script**|**nvarchar(2048)**|在 DDL 触发器的末尾执行的已注册自定义存储过程或脚本。|  
|**fire_triggers_on_snapshot**|**bit**|指示在应用快照时是否执行所复制的触发器，可以为下列值之一：<br /><br /> **0** = 不执行触发器。<br /><br /> **1** = 执行触发器。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sysarticles &#40;Transact-sql&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)  
  
  
