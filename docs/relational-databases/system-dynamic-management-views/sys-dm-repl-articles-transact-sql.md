---
description: sys.dm_repl_articles (Transact-SQL)
title: sys.dm_repl_articles (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_repl_articles_TSQL
- dm_repl_articles
- dm_repl_articles_TSQL
- sys.dm_repl_articles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_articles dynamic management function
ms.assetid: 794d514e-bacd-432e-a8ec-3a063a97a37b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34151e89476ae62cdcce719b676477b9659ae3b4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096470"
---
# <a name="sysdm_repl_articles-transact-sql"></a>sys.dm_repl_articles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关在复制拓扑中作为项目发布的数据库对象的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**artcache_db_address**|**varbinary(8)**|发布数据库的缓存数据库结构的内存中地址。|  
|**artcache_table_address**|**varbinary(8)**|已发布表项目的缓存表结构的内存中地址。|  
|**artcache_schema_address**|**varbinary(8)**|已发布表项目的缓存项目架构结构的内存中地址。|  
|**artcache_article_address**|**varbinary(8)**|已发布表项目的缓存项目结构的内存中地址。|  
|**artid**|**bigint**|唯一标识该表中的每一项。|  
|**artfilter**|**bigint**|用于水平筛选项目的存储过程 ID。|  
|**artobjid**|**bigint**|已发布对象的 ID。|  
|**artpubid**|**bigint**|项目所属发布的 ID。|  
|**artstatus**|**tinyint**|项目选项和状态的位掩码，它可以是对下面的一个或多个值执行逻辑位或运算的结果：<br /><br /> **1** = 项目处于活动状态。<br /><br /> **8** = 在 INSERT 语句中包括列名称。<br /><br /> **16** = 使用参数化语句。<br /><br /> **24** = 在 INSERT 语句中包括列名称，并使用参数化语句。<br /><br /> 例如，使用参数化语句的活动项目在此列中的值为 17。 如果值为 0，则表示项目处于非活动状态，而且未定义其他属性。|  
|**arttype**|**tinyint**|项目的类型：<br /><br /> **1** = 基于日志的项目。<br /><br /> **3** = 具有手动筛选器的基于日志的项目。<br /><br /> **5** = 具有手动视图的基于日志的项目。<br /><br /> **7** = 具有手动筛选器和手动视图的基于日志的项目。<br /><br /> **8** = 存储过程执行。<br /><br /> **24** = 可序列化存储过程执行。<br /><br /> **32** = 存储过程仅)  (架构。<br /><br /> **64** = 仅) 查看 (架构。<br /><br /> **128** = 仅) 函数 (架构。|  
|**wszArtdesttable**|**nvarchar (514)**|目标中已发布对象的名称。|  
|**wszArtdesttableowner**|**nvarchar (514)**|目标中已发布对象的所有者。|  
|**wszArtinscmd**|**nvarchar (510)**|用于插入的命令或存储过程。|  
|**cmdTypeIns**|**int**|用于插入存储过程的调用语法，可以为以下值之一。<br /><br /> **1** = 调用<br /><br /> **2** = SQL<br /><br /> **3** = 无<br /><br /> **7** = 未知|  
|**wszArtdelcmd**|**nvarchar (510)**|用于删除的命令或存储过程。|  
|**cmdTypeDel**|**int**|用于删除存储过程的调用语法，可以为以下值之一。<br /><br /> **0** = XCALL<br /><br /> **1** = 调用<br /><br /> **2** = SQL<br /><br /> **3** = 无<br /><br /> **7** = 未知|  
|**wszArtupdcmd**|**nvarchar (510)**|用于更新的命令或存储过程。|  
|**cmdTypeUpd**|**int**|用于更新存储过程的调用语法，可以为以下值之一。<br /><br /> **0** = XCALL<br /><br /> **1** = 调用<br /><br /> **2** = SQL<br /><br /> **3** = 无<br /><br /> **4** = MCALL<br /><br /> **5** = VCALL<br /><br /> **6** = SCALL<br /><br /> **7** = 未知|  
|**wszArtpartialupdcmd**|**nvarchar (510)**|用于部分更新的命令或存储过程。|  
|**cmdTypePartialUpd**|**int**|用于部分更新存储过程的调用语法，可以为以下值之一。<br /><br /> **2** = SQL|  
|**numcol**|**int**|垂直筛选项目的分区中的列数。|  
|**artcmdtype**|**tinyint**|当前复制的命令类型，可以为下列值之一。<br /><br /> **1** = 插入<br /><br /> **2** = 删除<br /><br /> **3** = 更新<br /><br /> **4** = UPDATETEXT<br /><br /> **5** = 无<br /><br /> **6** = 仅限内部使用<br /><br /> **7** = 仅限内部使用<br /><br /> **8** = 部分更新|  
|**artgeninscmd**|**nvarchar (510)**|基于项目中所包含列的 INSERT 命令模板。|  
|**artgendelcmd**|**nvarchar (510)**|DELETE 命令模板，可以包括项目中包含的主键或列，具体取决于所使用的调用语法。|  
|**artgenupdcmd**|**nvarchar (510)**|UPDATE 命令模板，可以包括主键、更新列或完整的列列表，具体取决于所使用的调用语法。|  
|**artpartialupdcmd**|**nvarchar (510)**|部分 UPDATE 命令模板，其中包括主键和更新列。|  
|**artupdtxtcmd**|**nvarchar (510)**|UPDATETEXT 命令模板，其中包括主键和更新列。|  
|**artgenins2cmd**|**nvarchar (510)**|在并发快照处理期间协调项目时使用的 INSERT 命令模板。|  
|**artgendel2cmd**|**nvarchar (510)**|在并发快照处理期间协调项目时使用的 DELETE 命令模板。|  
|**fInReconcile**|**tinyint**|在并发快照处理期间指示当前是否正在协调项目。|  
|**fPubAllowUpdate**|**tinyint**|指示发布是否允许更新订阅。|  
|**intPublicationOptions**|**bigint**|指定其他发布选项的位图，其中位选项值包括：<br /><br /> **0x1** -已启用对等复制。<br /><br /> **0x2** -仅发布本地更改。<br /><br /> **0x4** -为非 SQL Server 订阅服务器启用。|  
  
## <a name="permissions"></a>权限  
 要求对发布数据库具有 VIEW DATABASE STATE 权限才能调用 **dm_repl_articles**。  
  
## <a name="remarks"></a>备注  
 只为复制项目缓存中当前加载的复制的数据库对象返回信息。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与复制相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)  
  
  

