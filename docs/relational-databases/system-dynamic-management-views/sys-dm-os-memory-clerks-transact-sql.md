---
description: sys.dm_os_memory_clerks (Transact-SQL)
title: sys.dm_os_memory_clerks (Transact-SQL)
ms.custom: ''
ms.date: 02/18/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_clerks
- sys.dm_os_memory_clerks
- dm_os_memory_clerks_TSQL
- sys.dm_os_memory_clerks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_clerks dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0cd75d2eb6e613f36cecee8e79e3c8d6c99eed8d
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575328"
---
# <a name="sysdm_os_memory_clerks-transact-sql"></a>sys.dm_os_memory_clerks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中当前处于活动状态的全部内存分配器的集合。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_memory_clerks**。 
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**memory_clerk_address**|**varbinary(8)**|指定内存分配器的唯一内存地址。 这是主键列。 不可为 null。|  
|type |**nvarchar(60)**|指定内存分配器的类型。 每个分配器都具有特定类型，例如，CLR Clerks MEMORYCLERK_SQLCLR。 不可为 null。|  
|name |**nvarchar(256)**|指定在内部为此内存分配器分配的名称。 一个组件可拥有多个特定类型的内存分配器。 组件可选择使用特定名称来标识相同类型的内存分配器。 不可为 null。|  
|**memory_node_id**|**smallint**|指定内存节点的 ID。 不可为 Null。|  
|**single_pages_kb**|**bigint**|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]。 有关详细信息，请参阅 [从 SQL Server 2012 (11. x) 开始对内存管理所做的更改 ](../memory-management-architecture-guide.md#changes-to-memory-management-starting-with-)。|  
|**pages_kb**|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指定为此内存分配器分配的页内存量 (KB)。 不可为 null。|  
|**multi_pages_kb**|**bigint**|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]。 有关详细信息，请参阅 [从 SQL Server 2012 (11. x) 开始对内存管理所做的更改 ](../memory-management-architecture-guide.md#changes-to-memory-management-starting-with-)。<br /><br /> 分配的多页内存量 (KB)。 这是使用内存节点的多页分配器分配的内存量。 此内存在缓冲池外面分配，利用了内存节点虚拟分配器的优势。 不可为 null。|  
|**virtual_memory_reserved_kb**|**bigint**|指定内存分配器保留的虚拟内存量。 不可为 null。|  
|**virtual_memory_committed_kb**|**bigint**|指定内存分配器提交的虚拟内存量。 提交的内存量应始终小于保留的内存量。 不可为 null。|  
|**awe_allocated_kb**|**bigint**|指定在物理内存中锁定且未由操作系统调出的内存量 (KB) 。 不可为 null。|  
|**shared_memory_reserved_kb**|**bigint**|指定内存分配器保留的共享内存量。 保留以供共享内存和文件映射使用的内存量。 不可为 null。|  
|**shared_memory_committed_kb**|**bigint**|指定内存分配器提交的共享内存量。 不可为 null。|  
|**page_size_in_bytes**|**bigint**|指定此内存分配器的页分配粒度。 不可为 null。|  
|**page_allocator_address**|**varbinary(8)**|指定页分配器的地址。 此地址对内存分配器是唯一的，可在 **sys.dm_os_memory_objects** 中用于查找绑定到此职员的内存对象。 不可为 null。|  
|**host_address**|**varbinary(8)**|指定用于此内存分配器的主机的内存地址。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.dm_os_hosts ](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md)。 诸如 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 之类 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的组件通过主机接口访问内存资源。<br /><br /> 0x00000000 = 属于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的内存分配器。<br /><br /> 不可为 null。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  

## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]对于基本、S0 和 S1 服务目标以及弹性池中的数据库，[服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database)帐户或[Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure)帐户是必需的。 对于所有其他 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="remarks"></a>注解

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存管理器由一个三层的层次结构组成。 该层次结构的底层为内存节点。 中间层由内存分配器、内存缓存和内存池组成。 顶层由内存对象组成。 这些对象用于分配的实例中的内存 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 内存节点提供低级分配器的界面和实现。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，只有内存内存分配器可访问内存节点。 内存分配器访问内存节点界面以分配内存。 内存节点还会跟踪 Clerk 分配的内存以进行诊断。 分配大量内存的每个组件，都必须使用分配器界面来创建其自己的内存分配器并分配其全部内存。 各组件会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动时频繁创建其相应的分配器。  

### <a name="cachestore-and-userstore"></a>缓存存储区和 USERSTORE

缓存存储区和 USERSTORE 是内存分配器，但充当实际缓存。 通常，缓存会保留分配，直到缓存删除策略释放这些分配。 若要避免重新创建它，缓存的分配会尽可能长时间保留在缓存中，并且通常会从缓存中删除（如果它太旧而不会有用），或者当新信息需要内存空间时 (，请参阅 [时钟手动扫描](sys-dm-os-memory-cache-clock-hands-transact-sql.md#remarks)) 。 这是缓存生存期控制和可见性控件的两个主要控件之一。

缓存存储和用户存储的不同之处在于它们控制分配的生存期。 对于缓存存储，项的生存期由 SQLOS 的缓存框架完全控制。 对于用户存储，条目生存期仅由存储区部分控制。 每个用户存储的实现可能特定于内存分配的性质，因此用户存储区会在其条目的生存期内进行控制。 

可见性控件管理条目的可见性。 缓存中的条目可能存在，但可能不可见。 例如，如果将某个缓存项标记为仅供单一使用，则该项在使用后将不可见。 此外，缓存条目可能会标记为 "已更新";它将继续驻留在缓存中，但对任何查找都不可见。 对于这两个存储区，项可见性由缓存框架控制。

有关详细信息，请参阅 [SQLOS 缓存](https://docs.microsoft.com/archive/blogs/slavao/sqlos-caching)。

### <a name="objectstore"></a>OBJECTSTORE

对象存储是一个简单的池。 它用于缓存同类数据。 池中的所有项都被视为是等同的。  对象存储实现最大大小，以相对于其他缓存来控制大小。

有关详细信息，请参阅 [SQLOS 缓存](https://docs.microsoft.com/archive/blogs/slavao/sqlos-caching)。

## <a name="types"></a>类型

下表列出了内存分配器类型：

|类型  |说明  |
|---------|---------|
|CACHESTORE_BROKERDSH     |     此缓存存储用于通过 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) 对话框安全标头缓存来存储分配    |
|CACHESTORE_BROKERKEK     |   此缓存存储用于通过 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  密钥交换密钥缓存来存储分配    |
|CACHESTORE_BROKERREADONLY     |     此缓存存储用于通过 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) 只读缓存来存储分配    |
|CACHESTORE_BROKERRSB     |  此缓存存储用于通过 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) [远程服务绑定](../../t-sql/statements/create-remote-service-binding-transact-sql.md) 缓存来存储分配。    |
|CACHESTORE_BROKERTBLACS     |     此缓存存储用于通过 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) 来存储安全访问结构的分配。    |
|CACHESTORE_BROKERTO     |  此缓存存储用于通过 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) [传输对象](../performance-monitor/sql-server-broker-to-statistics-object.md) 缓存来存储分配   |
|CACHESTORE_BROKERUSERCERTLOOKUP     |    此缓存存储用于通过 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  用户证书查找缓存来存储分配  |
|CACHESTORE_COLUMNSTOREOBJECTPOOL     |     此缓存存储用于[段](../system-catalog-views/sys-column-store-segments-transact-sql.md)和[字典](../system-catalog-views/sys-column-store-dictionaries-transact-sql.md)的[列存储索引](../indexes/columnstore-indexes-overview.md)分配   |
|CACHESTORE_CONVPRI     |  此缓存存储用于存储分配， [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) 跟踪   [会话优先级](../system-catalog-views/sys-conversation-priorities-transact-sql.md)     |
|CACHESTORE_EVENTS     |     此缓存存储用于通过[Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) [事件通知](../service-broker/event-notifications.md)存储分配 |
|CACHESTORE_FULLTEXTSTOPLIST     |    此内存分配器用于通过为 [非索引字表](../search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) 功能 Full-Text 引擎进行分配。       |
|CACHESTORE_NOTIF     |    此缓存存储用于通过 [查询通知](../../connect/ado-net/sql/query-notifications-sql-server.md) 功能进行的分配    |
|CACHESTORE_OBJCP     |    此缓存存储用于缓存对象，其中包含已编译的计划 (CP) ：存储过程、函数、触发器。 为了说明，在创建存储过程的查询计划之后，其计划存储在此缓存中。  |
|CACHESTORE_PHDR     |    在编译查询过程中，此缓存存储用于分析视图、约束和默认 algebrizer 树的过程中的临时内存缓存。 分析查询后，应释放内存。 一些示例包括：在一个批处理中的多个语句（一批中插入或更新一个批处理），包含大型动态生成的查询的 T-sql 批处理，IN 子句中有大量值。      |
|CACHESTORE_QDSRUNTIMESTATS     |   此缓存存储用于缓存 [查询存储](../performance/monitoring-performance-by-using-the-query-store.md)  运行时统计信息 |
|CACHESTORE_SEARCHPROPERTYLIST     |     此缓存存储用于通过对 [属性列表](../search/search-document-properties-with-search-property-lists.md) 缓存 Full-Text 引擎进行分配  |
|CACHESTORE_SEHOBTCOLUMNATTRIBUTE     |  存储引擎使用此缓存存储区来缓存堆或 B 树 (HoBT) 列元数据结构。      |
|CACHESTORE_SQLCP     |    此缓存存储用于在计划缓存中缓存即席查询、预定义的语句和服务器端游标。 即席查询通常是在不显式参数化的情况下提交给服务器的语言事件 T-sql 语句。 预定义的语句也使用此缓存存储-这些语句由应用程序使用 API 调用（如[SQLPrepare () ](../../odbc/reference/syntax/sqlprepare-function.md) /  [SQLExecute](../../odbc/reference/syntax/sqlexecute-function.md) (ODBC) 或 SqlCommand）提交[。 Prepare](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlcommand.prepare) / [SqlCommand.ExecuteNonQuery](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlcommand.executenonquery) (ADO.NET) 并将在服务器上显示[sp_prepare](../system-stored-procedures/sp-prepare-transact-sql.md) / [sp_execute](../system-stored-procedures/sp-execute-transact-sql.md)或[sp_prepexec](../system-stored-procedures/sp-prepexec-transact-sql.md)系统过程执行。 此外，服务器端游标将从此缓存存储中使用 ([sp_cursoropen](../system-stored-procedures/sp-cursoropen-transact-sql.md)， [sp_cursorfetch](../system-stored-procedures/sp-cursorfetch-transact-sql.md) [sp_cursorclose](../system-stored-procedures/sp-cursorclose-transact-sql.md)) 。    |
|CACHESTORE_STACKFRAMES     |    此缓存存储用于与堆栈帧相关的内部 SQL OS 结构的分配。     |
|CACHESTORE_SYSTEMROWSET     |   此缓存存储用于与事务日志记录和恢复相关的内部结构的分配。      |
|CACHESTORE_TEMPTABLES     |     此缓存存储用于与 [临时表和表变量缓存](../databases/tempdb-database.md#performance-improvements-in-tempdb-for-sql-server) （计划缓存的一部分）相关的分配。    |
|CACHESTORE_VIEWDEFINITIONS     |     在查询优化过程中，此缓存存储用于缓存视图定义。    |
|CACHESTORE_XML_SELECTIVE_DG     |     此缓存存储用于缓存 xml 结构以便进行 XML 处理。    |
|CACHESTORE_XMLDBATTRIBUTE     |     此缓存存储用于缓存 XML 活动的 XML 属性结构，如 [XQuery](../../xquery/xquery-language-reference-sql-server.md)。    |
|CACHESTORE_XMLDBELEMENT     |   此缓存存储用于缓存 XML 活动的 XML 元素结构，如 [XQuery](../../xquery/xquery-language-reference-sql-server.md)。      |
|CACHESTORE_XMLDBTYPE     |    此缓存存储用于缓存 XML 活动（如 XQuery）的 XML 结构。     |
|CACHESTORE_XPROC     |   此缓存存储用于缓存 Xprocs 在计划缓存中的 [扩展存储 (过程) ](../extended-stored-procedures-programming/database-engine-extended-stored-procedures-programming.md) 的结构。     |
|MEMORYCLERK_BACKUP     |    此内存分配器用于通过 [备份](../../t-sql/statements/backup-transact-sql.md) 功能进行各种分配    |
|MEMORYCLERK_BHF     |      在查询执行期间，此内存分配器用于 (BLOB) 管理的二进制大型对象的分配 (Blob 句柄支持)   |
|MEMORYCLERK_BITMAP     |     此内存分配器用于通过用于位图筛选的 SQL OS 功能进行分配    |
|MEMORYCLERK_CSILOBCOMPRESSION     |  此内存分配器用于通过[列存储索引 (BLOB) 压缩的二进制大型对象](../data-compression/data-compression.md#columnstore-and-columnstore-archive-compression)进行分配    |
|MEMORYCLERK_DRTLHEAP     |    此内存分配器用于按 SQL OS 功能分配   <br /><br />**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本   |
|MEMORYCLERK_EXPOOL     |    此内存分配器用于按 SQL OS 功能分配   <br /><br />**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本   |
|MEMORYCLERK_EXTERNAL_EXTRACTORS     |   此内存分配器用于通过 [批处理模式](../query-processing-architecture-guide.md#batch-mode-execution) 操作的查询执行引擎分配   <br /><br />**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本   |
|MEMORYCLERK_FILETABLE     |      此内存分配器用于 [filetable](../blob/filetables-sql-server.md) 功能的各种分配。   |
|MEMORYCLERK_FSAGENT     |      此内存分配器用于 [FILESTREAM](../blob/filestream-sql-server.md) 功能的各种分配。    |
|MEMORYCLERK_FSCHUNKER     |      此内存分配器用于创建 filestream 块的 [filestream](../blob/filestream-sql-server.md) 功能的各种分配。   |
|MEMORYCLERK_FULLTEXT     |     此内存分配器用于 Full-Text 引擎结构的分配。   |
|MEMORYCLERK_FULLTEXT_SHMEM     |   此内存分配器用于通过使用完整文本后台程序进程与共享内存连接性相关的引擎结构 Full-Text 分配。      |
|MEMORYCLERK_HADR     |     此内存分配器用于 AlwaysOn 功能的内存分配     |
|MEMORYCLERK_HOST     |    此内存分配器用于按 SQL OS 功能分配。     |
|MEMORYCLERK_LANGSVC     |     此内存分配器用于通过 SQL T-sql 语句和命令 (分析器、algebrizer 等进行分配 )     |
|MEMORYCLERK_LWC     |   此内存分配器用于 Full-Text [语义搜索](../search/semantic-search-sql-server.md) 引擎的分配       |
|MEMORYCLERK_POLYBASE     |    此内存分配器跟踪 SQL Server 内 [Polybase](../polybase/polybase-guide.md) 功能的内存分配。     |
|MEMORYCLERK_QSRANGEPREFETCH     |  在查询扫描范围预提取过程中，此内存分配器用于执行查询。      |
|MEMORYCLERK_QUERYDISKSTORE     |     此内存分配器用于 [查询存储](../performance/monitoring-performance-by-using-the-query-store.md) SQL Server 内的内存分配。    |
|MEMORYCLERK_QUERYDISKSTORE_HASHMAP     |   此内存分配器用于 [查询存储](../performance/monitoring-performance-by-using-the-query-store.md) SQL Server 内的内存分配。      |
|MEMORYCLERK_QUERYDISKSTORE_STATS     |  此内存分配器用于 [查询存储](../performance/monitoring-performance-by-using-the-query-store.md) SQL Server 内的内存分配。      |
|MEMORYCLERK_QUERYPROFILE     |  此内存分配器在服务器启动期间用于启用查询分析    <br /><br />**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本   |
|MEMORYCLERK_RTLHEAP     |    此内存分配器用于按 SQL OS 功能分配。  <br /><br />**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本   |
|MEMORYCLERK_SECURITYAPI     |    此内存分配器用于按 SQL OS 功能分配。  <br /><br />**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本   |
|MEMORYCLERK_SERIALIZATION     |     仅限内部使用    |
|MEMORYCLERK_SLOG     |     此内存分配器用于 sLog (辅助内存中日志流的分配) [加速数据库恢复](../accelerated-database-recovery-concepts.md) <br /><br />**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本   |
|MEMORYCLERK_SNI     |     此内存分配器将 (SNI) 组件的服务器网络接口分配内存。 SNI 管理连接和 [TDS](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/893fcc7e-8a39-4b3c-815a-773b7b982c50) 数据包 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  |
|MEMORYCLERK_SOSMEMMANAGER     |   此内存分配器 (SOS) 线程计划、内存和 i/o 管理分配结构。     |
|MEMORYCLERK_SOSNODE     |     此内存分配器 (SOS) 线程计划、内存和 i/o 管理分配结构。    |
|MEMORYCLERK_SOSOS     |     此内存分配器 (SOS) 线程计划、内存和 i/o 管理分配结构。    |
|MEMORYCLERK_SPATIAL     |    [空间数据](../spatial/spatial-data-sql-server.md)组件使用此内存分配器进行内存分配。     |
|MEMORYCLERK_SQLBUFFERPOOL     |    此内存分配器始终跟踪 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据页和索引页内的最大内存使用者。 缓冲池或数据缓存会将数据和索引页保存在内存中，以便快速访问数据。 有关详细信息，请参阅 [缓冲区管理](../memory-management-architecture-guide.md#buffer-management)。     |
|MEMORYCLERK_SQLCLR     |     [SQLCLR](../clr-integration/clr-integration-overview.md)使用此内存分配器进行分配。     |
|MEMORYCLERK_SQLCLRASSEMBLY     |     此内存分配器用于 [SQLCLR ](../clr-integration/clr-integration-overview.md) 程序集的分配。     |
|MEMORYCLERK_SQLCONNECTIONPOOL     |     此内存分配器会缓存客户端应用程序可能需要服务器跟踪的服务器上的信息。 一个示例是通过  [sp_prepexecrpc](../system-stored-procedures/sp-prepexecrpc-transact-sql.md)创建准备句柄的应用程序。 应用程序应正确地 unprepare (在执行之后) 这些句柄。  |
|MEMORYCLERK_SQLEXTENSIBILITY     |    此内存分配器用于在上运行外部 Python 或 R 脚本的 [扩展性框架](../../machine-learning/concepts/extensibility-framework.md) 分配 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  <br /><br />**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本   |
|MEMORYCLERK_SQLGENERAL     |   SQL 引擎内的多个使用者可以使用此内存分配器。 示例包括复制内存、内部调试/诊断、部分 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动功能、某些 SQL 分析器功能、生成系统索引、初始化全局内存对象、在服务器和链接服务器查询中创建 OLEDB 连接、服务器端探查器跟踪、创建显示计划数据、部分安全功能、计算列的编译、用于并行结构的内存、用于某些 XML 功能的内存     |
|MEMORYCLERK_SQLHTTP     |    不推荐使用     |
|MEMORYCLERK_SQLLOGPOOL     |   此内存分配器由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志池使用。  日志池是在读取事务日志时用于提高性能的缓存。 具体而言，它可以在多个日志读取期间提高日志缓存利用率，减少磁盘 i/o 日志读取次数，并允许共享日志扫描。 日志池的主要使用者为 AlwaysOn (更改捕获和发送) 、重做管理器、数据库恢复-分析/重做/撤消、事务运行时回滚、复制/CDC、备份/还原。    |
|MEMORYCLERK_SQLOPTIMIZER     |     此内存分配器用于编译查询的不同阶段中的内存分配。 某些用途包括查询优化、索引统计信息管理器、视图定义编译、直方图生成。   |
|MEMORYCLERK_SQLQERESERVATIONS     |     此内存分配器用于内存授予分配，这是分配给查询以在执行查询期间执行排序和哈希操作的内存。 有关查询执行保留 (内存授予) 的详细信息，请参阅 [此博客](https://techcommunity.microsoft.com/t5/sql-server-support/memory-grants-the-mysterious-sql-server-memory-consumer-with/ba-p/333994)     |
|MEMORYCLERK_SQLQUERYCOMPILE     |    查询优化器使用此内存分配器在查询编译期间分配内存。   |
|MEMORYCLERK_SQLQUERYEXEC     |    此内存分配器用于以下几个方面的分配： [批处理模式处理](../query-processing-architecture-guide.md#batch-mode-execution)、 [并行查询](../query-processing-architecture-guide.md#parallel-query-processing) 执行、查询执行上下文、 [空间索引分割](../spatial/spatial-indexes-overview.md#tessellation)、排序和哈希操作 (对表进行排序、) 哈希表、一些 DVM 处理、 [更新统计信息](../statistics/update-statistics.md) 执行    |
|MEMORYCLERK_SQLQUERYPLAN     |     此内存分配器用于按 [堆](../indexes/heaps-tables-without-clustered-indexes.md) 页管理、 [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) 分配和 [sp_cursor * 存储过程](../system-stored-procedures/cursor-stored-procedures-transact-sql.md) 分配进行分配   |
|MEMORYCLERK_SQLSERVICEBROKER     |   [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)内存分配使用此内存分配器。       |
|MEMORYCLERK_SQLSERVICEBROKERTRANSPORT     |     [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)传输内存分配使用此内存分配器。    |
|MEMORYCLERK_SQLSLO_OPERATIONS     |      此内存分配器用于收集性能统计信息 <br /><br />适用范围：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   |
|MEMORYCLERK_SQLSOAP     |     不推荐使用    |
|MEMORYCLERK_SQLSOAPSESSIONSTORE     |    不推荐使用     |
|MEMORYCLERK_SQLSTORENG     |   此内存分配器用于多个存储引擎组件的分配。 组件的示例包括数据库文件的结构、数据库快照副本文件管理器、死锁监视器、DBTABLE 结构、日志管理器结构、某些 tempdb 版本结构、某些服务器启动功能、并行查询中子线程的执行上下文。      |
|MEMORYCLERK_SQLTRACE     |     此内存分配器用于服务器端 [SQL 跟踪](../sql-trace/sql-trace.md) 内存分配。     |
|MEMORYCLERK_SQLUTILITIES     |    SQL Server 中的多个分配器可以使用此内存分配器。 示例包括备份和还原、日志传送、数据库镜像、DBCC 命令、服务器端的 BCP 代码、某些查询并行运行、日志扫描缓冲区。    |
|MEMORYCLERK_SQLXML     |     此内存分配器用于执行 XML 操作时的内存分配。    |
|MEMORYCLERK_SQLXP     |     此内存分配器用于调用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [扩展存储过程](../extended-stored-procedures-reference/database-engine-extended-stored-procedures-reference.md)时的内存分配。    |
|MEMORYCLERK_SVL     |    此内存分配器用于内部 SQL OS 结构的分配 |
|MEMORYCLERK_TEST     |    仅限内部使用   |
|MEMORYCLERK_UNITTEST     |      仅限内部使用  |
|MEMORYCLERK_WRITEPAGERECORDER     |    此内存分配器用于通过写入页记录器进行分配。   |
|MEMORYCLERK_XE     |    此内存分配器用于 [扩展事件](../extended-events/extended-events.md) 内存分配      |
|MEMORYCLERK_XE_BUFFER     |      此内存分配器用于 [扩展事件](../extended-events/extended-events.md) 内存分配   |
|MEMORYCLERK_XLOG_SERVER     |   此内存分配器用于 Xlog 的分配，用于 SQL Azure 数据库中的日志文件管理   <br /><br />适用范围：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |
|MEMORYCLERK_XTP     |    此内存分配器用于 [内存中 OLTP](../in-memory-oltp/in-memory-oltp-in-memory-optimization.md) 内存分配。     |
|OBJECTSTORE_LBSS     |    此对象存储用于分配临时的 Lob 变量、参数和表达式的中间结果。 使用此存储的示例是 (TVP) 的 [表值参数](../../connect/ado-net/sql/table-valued-parameters.md) 。 有关此空间中的修复的详细信息，请参阅 [知识库文章 4468102](https://support.microsoft.com/topic/kb4468102-fix-excessive-memory-usage-when-you-trace-rpc-events-that-involve-table-valued-parameters-in-sql-server-2016-and-2017-c68aa214-26f1-98de-6b4d-c7dcad82dbd4) 和  [知识库文章 4051359](https://support.microsoft.com/topic/kb4051359-fix-sql-server-runs-out-of-memory-when-table-valued-parameters-are-captured-in-extended-events-sessions-in-sql-server-2016-even-if-collecting-statement-or-data-stream-isn-t-enabled-a3639efa-0618-82a8-f6b1-8cdcba29ce6d) 。     |
|OBJECTSTORE_LOCK_MANAGER     |      此内存分配器跟踪中的 [锁管理器](../sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine) 所进行的分配 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。   |
|OBJECTSTORE_SECAUDIT_EVENT_BUFFER     |   此对象存储用于 [SQL Server 审核](../security/auditing/sql-server-audit-database-engine.md) 内存分配。        |
|OBJECTSTORE_SERVICE_BROKER     |     此对象存储由 [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)    |
|OBJECTSTORE_SNI_PACKET     |     此对象存储由服务器网络接口用于管理连接的 (SNI) 组件使用    |
|OBJECTSTORE_XACT_CACHE     |    此对象存储用于缓存事务信息     |
|USERSTORE_DBMETADATA     |      此对象存储用于元数据结构     |
|USERSTORE_OBJPERM     |     此存储用于跟踪对象安全/权限的结构     |
|USERSTORE_QDSSTMT     |  此缓存存储用于缓存 [查询存储](../performance/monitoring-performance-by-using-the-query-store.md)  语句       |
|USERSTORE_SCHEMAMGR     |    架构管理器缓存会将有关数据库对象的不同类型的元数据信息存储在内存中， (例如，表) 。 此存储的一个常见用户可能是具有如下对象的 tempdb 数据库：表、临时过程、表变量、表值参数、工作表、工作文件、版本存储区。  |
|USERSTORE_SXC     |    此用户存储用于分配来存储所有 [RPC](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/619c43b6-9495-4a58-9e49-a4950db245b3) 参数。     |
|USERSTORE_TOKENPERM     |    TokenAndPermUserStore 是单个 SOS 用户存储，用于跟踪安全上下文、登录名、用户、权限和审核的安全条目。 分配多个哈希表来存储这些对象。    |

## <a name="see-also"></a>另请参阅  

 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_sys_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
  
