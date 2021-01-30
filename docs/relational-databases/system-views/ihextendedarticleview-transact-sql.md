---
description: IHextendedArticleView (Transact-SQL)
title: IHextendedArticleView (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHextendedArticleView_TSQL
- IHextendedArticleView
dev_langs:
- TSQL
helpviewer_keywords:
- IHextendedArticleView view
ms.assetid: 19ef0a12-3214-4bb0-9c25-a665897e65a2
author: stevestein
ms.author: sstein
ms.openlocfilehash: c91f65a94e0eb2699b4146d1bafdeebc0c4201d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186200"
---
# <a name="ihextendedarticleview-transact-sql"></a>IHextendedArticleView (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **IHextendedArticleView** 视图显示有关非 SQL Server 发布中的项目的信息。 此视图存储在 **分发** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|发布服务器的唯一标识符。|  
|**publication_id**|**int**|发布的唯一标识符。|  
|**文章**|**sysname**|项目的名称。|  
|**destination_object**|**sysname**|在订阅服务器上已发布对象的名称。|  
|**source_owner**|**sysname**|在发布服务器上已发布对象的所有者。|  
|**source_object**|**sysname**|在发布服务器上发布的对象的名称。|  
|description|**nvarchar(255)**|项目的说明。|  
|**creation_script**|**nvarchar(255)**|项目的架构创建脚本。|  
|**del_cmd**|**nvarchar(255)**|执行 DELETE 的命令。|  
|**filter**|**int**|用于定义水平分区的存储过程的标识符。|  
|**filter_clause**|**ntext**|用于水平筛选项目的 WHERE 子句。|  
|**ins_cmd**|**nvarchar(255)**|执行 INSERT 的命令。|  
|**pre_creation_cmd**|**tinyint**|DROP TABLE、DELETE TABLE 或 TRUNCATE 的预创建命令：<br /><br /> **0** = 无。<br /><br /> **1** = 删除。<br /><br /> **2** = 删除。<br /><br /> **3** = 截断。|  
|**status**|**tinyint**|项目选项和状态的位掩码，可以是对以下一个或多个值执行逻辑位或运算的结果：<br /><br /> **1** = 项目处于活动状态。<br /><br /> **8** = 在 INSERT 语句中包括列名称。<br /><br /> **16** = 使用参数化语句。<br /><br /> **24** = 在 INSERT 语句中包括列名称，并使用参数化语句。<br /><br /> 例如，使用参数化语句的活动项目在此列中的值为 **17** 。 如果值为 **0** ，则表示项目处于非活动状态，而且未定义其他属性。|  
|type |**tinyint**|项目的类型：<br /><br /> **1** = 基于日志的项目。<br /><br /> **3** = 具有手动筛选器的基于日志的项目。<br /><br /> **5** = 具有手动视图的基于日志的项目。<br /><br /> **7** = 具有手动筛选器和手动视图的基于日志的项目。|  
|**upd_cmd**|**nvarchar(255)**|执行 UPDATE 的命令。|  
|**schema_option**|**binary**|指示要编写脚本的内容。有关支持的架构选项的列表，请参阅 [sp_addarticle &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 。|  
|**dest_owner**|**sysname**|在目标数据库中已发布对象的所有者。|  
  
## <a name="see-also"></a>另请参阅  
 [异类数据库复制](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
