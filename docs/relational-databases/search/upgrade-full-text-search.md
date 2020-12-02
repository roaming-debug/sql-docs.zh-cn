---
description: 升级全文搜索
title: 升级全文搜索 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], installing
- migrating full-text indexes [SQL Server]
- upgrading Full-Text Search
- installing Full-Text Search
- full-text search [SQL Server], upgrading
ms.assetid: 2fee4691-f2b5-472f-8ccc-fa625b654520
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4759838a20e721031db8e4ea5e644cc3822285a8
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91868945"
---
# <a name="upgrade-full-text-search"></a>升级全文搜索
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  将全文搜索升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 是在安装期间以及在附加、还原或使用复制数据库向导复制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本的数据库文件和全文目录期间执行的。  
  
  
##  <a name="upgrade-a-server-instance"></a><a name="Upgrade_Server"></a> 升级服务器实例  
 对于就地升级， [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 实例将与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的旧版本并行安装，且数据将被迁移。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的旧版本安装了全文搜索，则将自动安装新版本的全文搜索。 并行安装表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例级存在下列每个组件。  
  
 断字符、词干分析器和筛选器  
 每个实例目前均使用自己的一组断字符、词干分析器和筛选器，而不受这些组件的操作系统版本的限制。 而且，这些组件在各个实例级也很容易注册和配置。 有关详细信息，请参阅 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) 和 [配置和管理搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md)。  
  
 筛选器后台程序宿主  
 全文筛选器后台程序宿主进程可以安全地加载和驱动用于索引和查询的可扩展外部组件（例如断字符、词干分析器和筛选器），而不会损害全文引擎的完整性。 服务器实例对所有多线程筛选器使用多线程进程，并对所有单线程筛选器使用单线程进程。  
  
> [!NOTE]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 引入了 FDHOST Launcher 服务 (MSSQLFDLauncher) 的服务帐户。 该服务将服务帐户信息传播到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]特定实例的筛选器后台程序宿主进程。 有关设置服务帐户的信息，请参阅 [设置用于全文筛选器后台程序启动器的服务帐户](../../relational-databases/search/set-the-service-account-for-the-full-text-filter-daemon-launcher.md)。  
  
 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中，每个全文检索都驻留在属于文件组的全文目录中，它们均具有物理路径，并被视为数据库文件。 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本中，全文目录是一个包含一组全文检索的逻辑对象或虚拟对象。 因此，新的全文目录不会视为带有物理路径的数据库文件。 但是，在升级包含数据文件的所有全文目录期间，将在相同磁盘上创建新的文件组。 这可以在升级后维护旧磁盘的 I/O 行为。 该目录的所有全文检索均被放置到新的文件组中（如果存在根路径）。 如果旧的全文目录路径无效，升级过程将全文检索保留在与基表相同的文件组中，或者对于分区表，则保留在主文件组中。  
  
  
##  <a name="full-text-upgrade-options"></a><a name="FT_Upgrade_Options"></a> 全文升级选项  
 将服务器实例升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]时，您可以通过用户界面选择以下全文升级选项之一。  
  
**导入**  
 导入全文目录。 一般情况下，导入速度比重新生成速度要快很多。 例如，当仅使用一个 CPU 时，导入的运行速度比重新生成要快 10 倍左右。 不过，导入的全文目录不能使用随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的最新版本安装的新断字符。 若要确保查询结果的一致性，必须重新生成全文目录。  
  
> [!NOTE]  
>  重新生成可以以多线程模式运行，如果可用的 CPU 在 10 个以上，且您允许重新生成操作使用所有这些 CPU，则重新生成操作的运行速度可能比导入更快。  
  
 如果全文目录不可用，则会重新生成关联的全文检索。 此选项仅对 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库可用。  
  
 有关导入全文检索的影响的信息，请参阅本主题后面的“有关选择全文升级选项的注意事项”。  
  
 **重新生成**  
 使用新的和增强的断字符重新生成全文目录。 重新生成索引可能需要一些时间，且升级后可能需要占用大量的 CPU 和内存。  
  
 **重置**  
 重置全文目录。 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]升级时，将删除全文目录文件，但会保留全文目录和全文检索的元数据。 在进行升级后，所有全文检索将禁用更改跟踪，并且不会自动启动爬网。 在升级完成后，目录将保留为空，直至手动执行完全填充。  
  
##  <a name="considerations-for-choosing-a-full-text-upgrade-option"></a><a name="Choosing_Upgade_Option"></a> 有关选择全文升级选项的注意事项  
 为升级选择升级选项时，请考虑以下几点：  
  
-   需要查询结果的一致性吗？  
  
     [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 将安装新的断字符以供全文和语义搜索使用。 在建立索引时和查询时均使用断字符。 如果您没有重新生成全文目录，搜索结果可能不一致。 如果您发出查找某一短语的全文查询和当前断字符，该短语是由以前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中的断字符以不同方式进行断字的，则可能不会检索包含该短语的文档或行。 其原因在于，建立了索引的短语是使用与正使用的查询不同的逻辑进行断字的。 解决方法是使用新的断字符来重新填充（重新生成）全文目录，以便索引时行为和查询时行为完全相同。 可以选择“重新生成”选项来完成此操作，也可以在选择“导入”选项后手动重新生成。  
  
-   是否基于整数全文键列生成了任何全文检索？  
  
     重新生成执行内部优化，在某些情况下该优化可提高升级的全文检索的查询性能。 具体来说，如果您具有的全文目录包含基表的全文键列为整数数据类型的全文检索，则重新生成将在升级后实现全文查询的理想性能。 在这种情况下，我们强烈建议使用 **“重新生成”** 选项。  
  
    > [!NOTE]  
    >  对于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中的全文索引，建议采用整数数据类型作为全文键列。 有关详细信息，请参阅 [改进全文索引的性能](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)。  
  
-   使服务器实例处于联机状态的优先级是什么？  
  
     升级期间的导入或重新生成操作会占用很多 CPU 资源，这会延迟其余服务器实例的升级和联机。 如果使服务器实例尽快处于联机状态非常重要，并且希望在升级后运行手动填充，则适合 **“重置”** 。  
  
## <a name="ensure-consistent-query-results-after-importing-a-full-text-index"></a>导入全文索引后确保查询结果的一致性  
 如果在将 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]时导入了全文目录，则由于旧断字符和新断字符的行为略有差异，可能会出现查询和全文检索内容不匹配。 在这种情况下，若要保证查询和全文检索内容之间的完全匹配，请选择以下选项之一：  
  
-   重新生成包含全文检索的全文目录 ([ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)*catalog_name* REBUILD)  
  
-   对全文检索执行 FULL POPULATION ([ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) ON *table_name* START FULL POPULATION)。  
  
 有关断字符的详细信息，请参阅 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)。  
  
## <a name="upgrade-noise-word-files-to-stoplists"></a>将干扰词文件升级到非索引字表  
将数据库从 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 升级到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]后，将不再使用干扰词文件。 然而，旧的干扰词文件存储在 FTDATA\ FTNoiseThesaurusBak 文件夹中，您可以稍后在更新或生成相应的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 非索引字表时使用它们。  
  
 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]升级后：  
  
-   如果您在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]安装中从未添加、修改或删除过任何干扰词文件，则该系统非索引字表应该可以满足您的需要。  
  
-   如果在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中修改过干扰词文件，则这些修改在升级期间将会丢失。 若要重新创建这些更新，必须在对应的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 非索引字表中手动重新创建这些修改。 有关详细信息，请参阅 [ALTER FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)。  
  
-   如果不希望将任何非索引字应用于全文检索（例如，如果您在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 安装中删除或清除过干扰词文件），则必须关闭每个已升级的全文检索的非索引字表。 请运行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句（将 *database* 替换为已升级的数据库的名称，并将 *table* 替换为 *table* 的名称）：  
  
    ```  
    Use database;   
    ALTER FULLTEXT INDEX ON table  
       SET STOPLIST OFF;  
    GO  
    ```  
  
     STOPLIST OFF 子句删除非索引字筛选，并触发对表的填充，而不会筛选任何视为干扰词的词语。  
  
## <a name="backup-and-imported-full-text-catalogs"></a>备份和导入全文目录  
 对于在升级期间重新生成或重置的全文目录（以及对于新的全文目录），全文目录只是一个逻辑概念，并不驻留在文件组中。 因此，若要备份 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中的全文目录，必须逐个识别包含目录全文检索的每个文件组并将它们一一备份。 有关详细信息，请参阅 [备份和还原全文目录和索引](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)。  
  
 对于从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]导入的全文目录，该全文目录在其自己的文件组中仍然是数据库文件。 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中除不存在 MSFTESQL 服务以外，全文目录的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]备份进程仍然适用。 有关 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 进程的信息，请参阅 SQL Server 2005 联机丛书中的 [备份和还原全文目录](/previous-versions/sql/sql-server-2005/ms142511(v=sql.90)) 。  
  
##  <a name="migrating-full-text-indexes-when-upgrading-a-database-to-sscurrent"></a><a name="Upgrade_Db"></a> 将数据库升级到 时迁移全文检索 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
 使用附加、还原或复制数据库向导可以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本中的数据库文件和全文目录升级到现有 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 服务器实例。 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 导入、重置或重新生成全文检索（如果有）。 **upgrade_option** 服务器属性控制在升级这些数据库期间服务器实例使用哪个全文升级选项。  
  
 将任何 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库附加、还原或复制到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]后，该数据库将立即变为可用，然后自动进行升级。 导入可能需要数小时，而重新生成所需的时间最多时可能十倍于此，具体取决于要编制索引的数据量。 另请注意，将升级选项设置为“导入”时，如果全文目录不可用，则会重新生成关联的全文检索。  
  
 **更改服务器实例的全文升级行为**  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)]使用 [sp\_fulltext\_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md) 的 **upgrade\_option** 操作  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **：** 使用“服务器属性”对话框的“全文升级选项”。 有关详细信息，请参阅 [管理和监视服务器实例的全文搜索](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md)。  
  
##  <a name="considerations-for-restoring-a-ssversion2005-full-text-catalog-to-sscurrent"></a><a name="Considerations_for_Restore"></a> 有关将 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 全文目录还原到 的注意事项 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
 将全文数据从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的其中一种方法是将完整数据库备份还原到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。  
  
 导入 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 全文目录时，您可以备份和还原数据库和目录文件。 其行为与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中相同：  
  
-   完整数据库备份将包括全文目录。 若要引用全文目录，请使用其 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 文件名，即 sysft_+*catalog-name*。  
  
-   如果全文目录处于脱机状态，备份将失败。  
  
 有关备份和还原 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 全文目录的详细信息，请参阅 联机丛书中的 [备份和还原全文目录](./back-up-and-restore-full-text-catalogs-and-indexes.md) 和 [文件备份和还原和全文目录](/previous-versions/sql/sql-server-2008-r2/ms190643(v=sql.105))[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。  
  
 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]上还原数据库时，将为全文目录创建新的数据库文件。 该文件的默认名称是 ftrow_ *catalog-name*.ndf。 例如，如果 *catalog-name* 是 `cat1`， [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 数据库文件的默认名称则为 `ftrow_cat1.ndf`。 但是，如果目标目录中已使用该默认名称，新数据库文件则命名为 `ftrow_`*catalog-name*`{`*GUID*`}.ndf`，其中 *GUID* 是新文件的全局唯一标识符。  
  
 导入目录后， **sys.database_files** 和 **sys.master_files** 经过更新以删除目录项且 **sys.fulltext_catalogs** 中的 **path** 列设置为 NULL。  
  
 **备份数据库**  
  
-   [完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/full-database-backups-sql-server.md)  
  
-   [事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)。（仅限于完整恢复模式）  
  
 **还原数据库备份**  
  
-   [完整数据库还原（简单恢复模式）](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
  
-   [完整数据库还原（完整恢复模式）](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)  
  
### <a name="example"></a>示例  
 以下示例在 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句中使用 MOVE 子句，还原名为 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 的 `ftdb1`数据库。 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库、日志和目录文件将被移动到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 服务器实例上的新位置，如下所示：  
  
-   数据库文件 `ftdb1.mdf`移动到 `C:\Program Files\Microsoft SQL Server\MSSQL.1MSSQL13.MSSQLSERVER\MSSQL\DATA\ftdb1.mdf`。  
  
-   日志文件 `ftdb1_log.ldf`移动到日志磁盘驱动器上的日志目录 *log_drive*`:\`*log_directory*`\ftdb1_log.ldf`中。  
  
-   与 `sysft_cat90` 目录对应的目录文件移动到 `C:\temp`。 导入全文索引后，它们将自动放置在数据库文件 C:\ftrow_sysft_cat90.ndf 中，并删除 C:\temp。  
  
```  
RESTORE DATABASE [ftdb1] FROM  DISK = N'C:\temp\ftdb1.bak' WITH  FILE = 1,  
   MOVE N'ftdb1' TO N'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\DATA\ftdb1.mdf',  
    MOVE N'ftdb1_log' TO N'log_drive:\log_directory\ftdb1_log.ldf',  
    MOVE N'sysft_cat90' TO N'C:\temp';  
```  
  
##  <a name="attaching-a-sql-server-2005-database-to-sscurrent"></a><a name="Attaching_2005_ft_catalogs"></a> 将 SQL Server 2005 数据库附加到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本中，全文目录是表示一组全文检索的逻辑概念。 全文目录是虚拟对象，不属于任何文件组。 但是，将包含全文目录文件的 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库附加到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 服务器实例时，会将目录文件从其先前位置与其他数据库文件一起附加，这与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中相同。  
  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中附加的每个全文目录的状态与从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]分离数据库时相同。 如果分离操作挂起任意全文检索填充，该填充将在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]上恢复，全文检索随后即可用于全文搜索。  
  
 如果 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 找不到全文目录文件，或者在未指定新位置的情况下在附加操作期间移动全文文件，行为则取决于选择的全文升级选项。 如果全文升级选项为“导入”或“重新生成”，则重新生成附加的全文目录。 如果全文升级选项为“重置”，则重置附加的全文目录。  
  
 有关分离和附加数据库的详细信息，请参阅[数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)、[CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)、[sp_attach_db](../../relational-databases/system-stored-procedures/sp-attach-db-transact-sql.md) 和 [sp_detach_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [全文搜索入门](../../relational-databases/search/get-started-with-full-text-search.md)   
 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [配置和管理搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md)  
  
