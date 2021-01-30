---
description: sp_fulltext_service (Transact-SQL)
title: sp_fulltext_service (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_service
- sp_fulltext_service_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], properties
- sp_fulltext_service
- Full-Text Search Upgrade Option
ms.assetid: 17a91433-f9b6-4a40-88c4-8c704ec2de9f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6fbbbe3451994687542f755b4d48953c7dbbe2e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187319"
---
# <a name="sp_fulltext_service-transact-sql"></a>sp_fulltext_service (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 全文搜索的服务器属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_fulltext_service [ [@action=] 'action'   
     [ , [ @value= ] value ] ]  
```  
  
## <a name="arguments"></a>参数  
`[ @action = ] 'action'` 要更改或重置的属性。 *操作* 为 **nvarchar (100) ，** 无默认值。 有关 *c*：属性及其说明以及可设置的值的列表，请参阅 *value* 参数下的表。 该参数将返回下列属性：数据类型、当前运行值、最小值或最大值以及不推荐使用的状态（如果适用）。  
  
`[ @value = ] value` 指定属性的值。 *值* **sql_variant**，默认值为 NULL。 如果 @value 为 null，则 **sp_fulltext_service** 返回当前设置。 此表列出了操作属性及其说明以及可设置的值。  
  
> [!NOTE]  
>  未来版本的中将删除以下操作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ： **clean_up**、 **connect_timeout**、 **data_timeout** 和 **resource_usage**。 请避免在新的开发工作中使用这些操作，并考虑修改当前使用上述任意操作的应用程序。  
  
|操作|数据类型|说明|  
|------------|---------------|-----------------|  
|**clean_up**|**int**|支持它仅仅是为了保持向后兼容。 该值始终为 0。|  
|**connect_timeout**|**int**|支持它仅仅是为了保持向后兼容。 该值始终为 0。|  
|**data_timeout**|**int**|支持它仅仅是为了保持向后兼容。 该值始终为 0。|  
|**load_os_resources**|**int**|指示是否在此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中注册并使用操作系统断字、词干分析器以及筛选器。 下列其中一项：<br /><br /> 0 = 仅使用特定于此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的筛选器和断字符。<br /><br /> 1 = 加载操作系统筛选器和断字符。<br /><br /> 默认情况下，禁用此属性以避免由操作系统更新引起疏忽性行为更改。 如果允许使用操作系统资源，则可以访问在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 索引服务注册但未安装特定于实例的资源的语言类型和文档类型的资源。 如果启用加载操作系统资源，请确保操作系统资源是受信任的已签名二进制文件;否则，在 **verify_signature** () 设置为1时，将无法加载它们。|  
|**master_merge_dop**|**int**|指定主合并进程要使用的线程数。 此值不应超过可用 CPU 或 CPU 内核的数量。<br /><br /> 如果未指定该参数，则服务将使用 4 或可用 CPU 或 CPU 内核数这两者中的较小者。|  
|**pause_indexing**|**int**|指定当全文索引当前正在运行时是否应让其暂停，或者当全文索引当前处于暂停状态时是否应让其恢复运行。<br /><br /> 0 = 让服务器实例的全文索引活动恢复运行。<br /><br /> 1 = 暂停服务器实例的全文索引活动。|  
|**resource_usage**|**int**|在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本中不起作用，因而被忽略。|  
|**update_languages**|Null|更新在全文搜索中注册的语言和筛选器的列表。 这些语言是在配置索引和全文查询时指定的。 筛选器后台程序宿主使用筛选器从相应的文件格式（如 **varbinary**、 **varbinary (max)**、 **image** 或 **xml**）中提取文本信息以进行全文索引。<br /><br /> 有关详细信息，请参阅 [查看或更改注册的筛选器和断字符](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md)。|  
|**upgrade_option**|**int**|控制在将数据库从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 升级到更高版本时迁移全文索引的方式。 此属性适用于以下升级方式：附加数据库、还原数据库备份、还原文件备份或使用复制数据库向导复制数据库。<br /><br /> 下列其中一项：<br /><br /> 0 = 使用新的和增强的断字符重新生成全文目录。 重新生成索引可能需要一些时间，且升级后可能需要占用大量的 CPU 和内存。<br /><br /> 1 = 重置全文目录。 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 将删除全文目录文件，但会保留全文目录和全文索引的元数据。 在进行升级后，所有全文检索将禁用更改跟踪，并且不会自动启动爬网。 在升级完成后，目录将保留为空，直至手动执行完全填充。<br /><br /> 2 = 导入全文目录。 一般情况下，导入速度比重新生成速度要快很多。 例如，当仅使用一个 CPU 时，导入的运行速度比重新生成要快 10 倍左右。 不过，导入的全文目录不能使用新的和增强的断字符，因此最终可能还是要重新生成全文目录。<br /><br /> 注意：重新生成可以在多线程模式下运行，如果有10个以上的 Cpu 可用，则如果允许 rebuild 使用所有 Cpu，则重新生成操作的运行速度可能比导入更快。<br /><br /> 如果全文目录不可用，则会重新生成关联的全文检索。 此选项仅对 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库可用。<br /><br /> 有关选择全文升级选项的信息，请参阅[升级全文搜索](../../relational-databases/search/upgrade-full-text-search.md)。<br /><br /> 注意：若要在中设置此属性 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，请使用 " **全文升级选项** " 属性。 有关详细信息，请参阅 [管理和监视服务器实例的全文搜索](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md)。|  
|**verify_signature**|**int**|指示全文引擎是否只加载已签名的二进制文件。 默认情况下，仅加载已签名的可信二进制文件。<br /><br /> 1 = 验证是否只加载已签名的可信二进制文件（默认值）。<br /><br /> 0 = 不验证二进制文件是否已签名。|  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 只有 **serveradmin** 固定服务器角色的成员或系统管理员才能 **sp_fulltext_service** 执行。  
  
## <a name="examples"></a>示例  
  
### <a name="a-updating-the-list-of-registered-languages"></a>A. 更新已注册的语言的列表  
 下面的示例更新已向全文搜索注册的语言的列表。  
  
```  
EXEC sp_fulltext_service 'update_languages';  
GO  
```  
  
### <a name="b-changing-the-full-text-upgrade-option-to-reset-full-text-catalogs"></a>B. 更改全文升级选项以重置全文目录  
 下面的示例更改全文升级选项以重置全文目录。 这会将它们彻底删除。 此示例指定了可选的 `@action` 和 `@value` 关键字。  
  
```  
EXEC sp_fulltext_service @action='upgrade_option', @value=1;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [全文搜索](../../relational-databases/search/full-text-search.md)   
 [FULLTEXTSERVICEPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
