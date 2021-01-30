---
description: sp_helpmergearticle (Transact-SQL)
title: sp_helpmergearticle (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergearticle
- sp_helpmergearticle_TSQL
helpviewer_keywords:
- sp_helpmergearticle
ms.assetid: 0fb9986a-3c33-46ef-87bb-297396ea5a6a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 43365f58dbfe76c89a5e7e41a0c65c65188edfdf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179291"
---
# <a name="sp_helpmergearticle-transact-sql"></a>sp_helpmergearticle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关项目的信息。 此存储过程用于在发布服务器上对发布数据库执行，或在正重新发布的订阅服务器上对订阅数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergearticle [ [ @publication = ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要检索其信息的发布的名称。 *发布* 为 **sysname**，默认值为 **%** ，它返回当前数据库的所有发布中包含的所有合并项目的相关信息。  
  
`[ @article = ] 'article'` 要返回其信息的项目的名称。 *项目* 是 **sysname**，默认值为 **%** ，它返回给定发布中所有合并项目的相关信息。  
  
## <a name="result-set"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|项目标识符。|  
|name|**sysname**|项目的名称。|  
|**source_owner**|**sysname**|源对象所有者的名称。|  
|**source_object**|**sysname**|从其中添加项目的源对象的名称。|  
|**sync_object_owner**|**sysname**|定义发布项目的视图所有者的名称。|  
|**sync_object**|**sysname**|用于建立分区初始数据的自定义对象的名称。|  
|description|**nvarchar(255)**|对项目的说明。|  
|**status**|**tinyint**|项目的状态，可以为以下值之一：<br /><br /> **1** = 非活动<br /><br /> **2** = 活动<br /><br /> **5** = 数据定义语言 (DDL) 操作挂起<br /><br /> **6** = 使用新生成的快照的 DDL 操作<br /><br /> 注意：在重新初始化项目时，值 **5** 和 **6** 将更改为 **2**。|  
|**creation_script**|**nvarchar(255)**|用于在订阅数据库中创建项目的可选项目架构脚本的路径和名称。|  
|**conflict_table**|**nvarchar (270)**|存储插入或更新冲突的表的名称。|  
|**article_resolver**|**nvarchar(255)**|项目的自定义冲突解决程序。|  
|**subset_filterclause**|**nvarchar(1000)**|用于指定水平筛选的 WHERE 子句。|  
|**pre_creation_command**|**tinyint**|预创建方法，可以为以下值之一：<br /><br /> **0** = 无<br /><br /> **1** = 删除<br /><br /> **2** = 删除<br /><br /> **3** = 截断|  
|**schema_option**|**二进制 (8)**|项目的架构生成选项位图。 有关此位图选项的信息，请参阅 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 或 [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。|  
|type |**smallint**|项目类型，可以为以下值之一：<br /><br /> **10** = 表<br /><br /> **32** = 存储过程<br /><br /> **64** = 视图或索引视图<br /><br /> **128** = 用户定义函数<br /><br /> **160** = 仅限同义词架构|  
|**column_tracking**|**int**|列级跟踪的设置;其中， **1** 表示列级跟踪处于开启状态， **0** 表示列级跟踪处于关闭状态。|  
|**resolver_info**|**nvarchar(255)**|项目冲突解决程序名。|  
|**vertical_partition**|**bit**|如果项目已垂直分区，则为;其中， **1** 表示项目已垂直分区， **0** 表示不表示项目。|  
|**destination_owner**|**sysname**|目标对象的所有者。 只适用于合并存储过程、视图和用户定义函数 (UDF) 架构项目。|  
|**identity_support**|**int**|如果启用自动标识范围处理，则为;，其中 **1** 是启用的， **0** 是禁用的。|  
|**pub_identity_range**|**bigint**|分配新标识值时要使用的范围大小。 有关详细信息，请参阅 [复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)的 "合并复制" 部分。|  
|**identity_range**|**bigint**|分配新标识值时要使用的范围大小。 有关详细信息，请参阅 [复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)的 "合并复制" 部分。|  
|**threshold**|**int**|用于运行 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 或早期版本的的订阅服务器的百分比值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 **阈值** 控制合并代理分配新标识范围的时间。 如果使用了在阈值中指定的百分比值，合并代理将创建新的标识范围。 有关详细信息，请参阅 [复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)的 "合并复制" 部分。|  
|**verify_resolver_signature**|**int**|如果在合并复制中使用冲突解决程序之前验证数字签名，则为; 否则为。其中， **0** 表示不验证签名， **1** 表示验证签名是否来自受信任的源。|  
|**destination_object**|**sysname**|目标对象的名称。 只适用于合并存储过程、视图和 UDF 架构项目。|  
|**allow_interactive_resolver**|**int**|如果对项目使用交互式冲突解决程序，则为;其中， **1** 表示使用此解析程序， **0** 表示不使用。|  
|**fast_multicol_updateproc**|**int**|启用或禁用合并代理以将更改应用于一个 UPDATE 语句中同一行中的多个列;其中， **1** 表示在一条语句中更新多个列， **0** 表示单独的 UPDATE 语句对于每个更新的列都是问题。|  
|**check_permissions**|**int**|一个整数值，表示已验证的表级权限的位图。 有关可能值的列表，请参阅 [&#40;transact-sql&#41;sp_addmergearticle ](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。|  
|**processing_order**|**int**|数据更改应用于发布中的项目的顺序。|  
|**upload_options**|**tinyint**|定义对具有客户端订阅的订阅服务器上所进行更新的限制，可以为下列值之一：<br /><br /> **0** = 对于包含客户端订阅的订阅服务器上所做的更新没有限制;所有更改都将上载到发布服务器。<br /><br /> **1** = 允许在具有客户端订阅的订阅服务器上进行更改，但不会将它们上载到发布服务器。<br /><br /> **2** = 不允许在具有客户端订阅的订阅服务器上进行更改。<br /><br /> 有关详细信息，请参阅[使用仅下载项目优化合并复制性能](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)。|  
|**identityrangemanagementoption**|**int**|如果启用自动标识范围处理，则为;，其中 **1** 是启用的， **0** 是禁用的。|  
|**delete_tracking**|**bit**|如果复制删除，则为;其中， **1** 表示复制删除， **0** 表示不复制删除。|  
|**compensate_for_errors**|**bit**|指示在同步过程中遇到错误时是否采取补偿操作;其中， **1** 表示执行了补偿操作， **0** 表示未采取补偿操作。|  
|**partition_options**|**tinyint**|定义项目数据的分区方式，当所有行只属于一个分区或只属于一个订阅时，这将可以实现性能优化。 *partition_options* 可以是下列值之一。<br /><br /> **0** = 项目的筛选是静态的，或者不为每个分区生成唯一的数据子集;也就是说，它是一个 "重叠" 分区。<br /><br /> **1** = 分区重叠，并且在订阅服务器上进行的数据操作语言 (DML) 更新不能更改行所属的分区。<br /><br /> **2** = 对项目的筛选将生成不重叠分区，但多个订阅服务器可以接收相同的分区。<br /><br /> **3** = 对项目的筛选将生成对每个订阅唯一的非重叠分区。|  
|**artid**|**uniqueidentifier**|唯一标识项目的标识符。|  
|**pubid**|**uniqueidentifier**|唯一标识在其中发布项目的发布的标识符。|  
|**stream_blob_columns**|**bit**|表示在复制二进制大型对象列时是否使用数据流优化。 **1** 表示正在使用优化， **0** 表示不使用优化。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpmergearticle** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有发布数据库中 **db_owner** 固定数据库角色的成员、分发数据库中的 **replmonitor** 角色或发布的发布访问列表才能执行 **sp_helpmergearticle**。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_helpmergearticle](../../relational-databases/replication/codesnippet/tsql/sp-helpmergearticle-tran_1.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改项目属性](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [sp_addmergearticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_dropmergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
