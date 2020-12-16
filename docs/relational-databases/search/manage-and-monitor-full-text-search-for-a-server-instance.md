---
description: 管理和监视服务器实例的全文搜索
title: 管理和监视 SQL Server 实例的全文搜索
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], about
- full-text search [SQL Server], server management
ms.assetid: 2733ed78-6d33-4bf9-94da-60c3141b87c8
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: ef6e746b3c9bbb78439631c1ce14ee725c30c29a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468648"
---
# <a name="manage-and-monitor-full-text-search-for-a-server-instance"></a>管理和监视服务器实例的全文搜索
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  针对服务器实例的全文管理包括：  
  
-   诸如管理 FDHOST Launcher 服务 (MSSQLFDLauncher)、重新启动筛选器后台程序宿主进程（如果您更改服务帐户凭据）、配置服务器范围内的全文属性以及备份全文目录之类的系统管理任务。 例如，在服务器级，您可以指定在整体上与服务器实例的默认语言不同的默认全文语言。  
  
-   配置全文语言组件（断字符和词干分析器、同义词库文件、非索引字和非索引字表）。  
  
-   配置用户数据库以进行全文搜索。 这涉及为数据库创建一个或多个全文目录并对每个要对其执行全文查询的表或索引视图定义一个全文索引。  
  
##  <a name="viewing-or-changing-server-properties-for-full-text-search"></a><a name="props"></a> 查看和更改全文搜索的服务器属性  
 可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中查看 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]实例的全文属性。  
  
#### <a name="to-view-and-change-server-properties-for-full-text-search"></a>查看和更改全文搜索的服务器属性  
  
1.  在“对象资源管理器”中，右键单击服务器，再单击“属性” 。  
  
2.  在“服务器属性”对话框中，单击“高级”页以查看有关全文搜索的服务器信息。 全文属性如下所示：  

    -   **默认全文语言**  
  
         指定全文检索列的默认语言。 全文检索数据的语言分析取决于数据的语言。 该选项的默认值为服务器的语言。 有关与所显示设置相对应的语言，请参阅 [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)。  
  
    -   **全文升级选项**  
  
         此服务器属性控制将数据库从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 升级到更高版本时迁移全文索引的方式。 此属性适用于以下升级方式：附加数据库、还原数据库备份、还原文件备份或使用复制数据库向导复制数据库。  
  
         可以选择的选项如下：  
  
         **导入**  
         导入全文目录。 一般情况下，导入速度比重新生成速度要快很多。 例如，当仅使用一个 CPU 时，导入的运行速度比重新生成要快 10 倍左右。 不过，导入的全文目录不能使用 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]中引入的新的和增强的断字符，因此最终可能还是要重新生成全文目录。  
  
        > [!NOTE]  
        >  重新生成可以以多线程模式运行，如果可用的 CPU 在 10 个以上，且您允许重新生成操作使用所有这些 CPU，则重新生成操作的运行速度可能比导入更快。  
  
         如果全文目录不可用，则会重新生成关联的全文检索。 此选项仅对 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库可用。  
  
         **重新生成**  
         使用新的和增强的断字符重新生成全文目录。 重新生成索引可能需要一些时间，且升级后可能需要占用大量的 CPU 和内存。  
  
         **重置**  
         重置全文目录。 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 将删除全文目录文件，但会保留全文目录和全文索引的元数据。 在进行升级后，所有全文检索将禁用更改跟踪，并且不会自动启动爬网。 在升级完成后，目录将保留为空，直至手动执行完全填充。  
  
         有关选择全文升级选项的信息，请参阅[升级全文搜索](../../relational-databases/search/upgrade-full-text-search.md)。  
  
        > [!NOTE]  
        >  也可以使用 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)**upgrade_option** 操作来设置全文升级选项。  
  
##  <a name="viewing-additional-full-text-server-properties"></a><a name="metadata"></a> 查看其他全文服务器属性  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数可用来获取全文搜索的各种服务器级属性的值。 此信息可用于全文搜索的管理和故障排除。  
  
 下表列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务器实例全文属性及其相关的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数。  
  
|属性|说明|函数|  
|--------------|-----------------|--------------|  
|**IsFullTextInstalled**|全文组件是否安装在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的当前实例中。|[FULLTEXTSERVICEPROPERTY](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)<br /><br /> [SERVERPROPERTY](../../t-sql/functions/serverproperty-transact-sql.md)|  
||||  
|**LoadOSResources**|此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中是否注册并使用了操作系统断字符和筛选器。|FULLTEXTSERVICEPROPERTY|  
|**VerifySignature**|指定全文引擎是否只加载已签名的二进制文件。|FULLTEXTSERVICEPROPERTY|  
  
##  <a name="monitoring-full-text-search-activity"></a><a name="monitor"></a> 监视全文搜索活动  
 有几个动态管理视图和函数可用来监视服务器实例上的全文搜索活动。  
  
 **查看与正在进行填充活动的全文目录有关的信息**  
  
-   [sys.dm_fts_active_catalogs (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md)  
  
 **查看筛选器后台程序宿主进程的当前活动**  
  
-   [sys.dm_fts_fdhosts (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-fdhosts-transact-sql.md)  
  
 **查看与正在进行的索引填充有关的信息**  
  
-   [sys.dm_fts_index_population (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
  
 **查看作为爬网或爬网范围的一部分使用的内存池中的内存缓冲区。**  
  
-   [sys.dm_fts_memory_buffers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)  
  
 **查看可供全文爬网或全文爬网范围的全文收集器组件使用的共享内存池**  
  
-   [sys.dm_fts_memory_pools (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
  
 **查看每个全文索引批次的相关信息**  
  
-   [sys.dm_fts_outstanding_batches (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)  
  
 **查看与正在进行的填充操作有关的特定范围的相关信息**  
  
-   [sys.dm_fts_population_ranges (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-population-ranges-transact-sql.md)  
  
  
