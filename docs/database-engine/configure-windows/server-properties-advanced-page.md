---
title: 服务器属性（“高级”页）| Microsoft Docs
description: 熟悉 SQL Server 高级服务器设置。 了解 FILESTREAM 选项、各种阈值、全文升级替代选项和其他设置。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.advanced.f1
ms.assetid: cc5e65c2-448e-4f37-9ad4-2dfb1cc84ebe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5df2313ce8dc5067828b8d21dc416694b2c17853
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673436"
---
# <a name="server-properties---advanced-page"></a>服务器属性 -“高级”页
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此页可以查看或修改高级服务器设置。  
  
 **查看“服务器属性”页**  
  
-   [查看或更改服务器属性 (SQL Server)](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)  
  
## <a name="containment"></a>Containment  
 启用包含的数据库  
 指示此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是否允许包含数据库。 如果为 **True**，则可以创建、还原或附加包含数据库。 如果为 **False**，则包含数据库不能创建、还原或附加到此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 更改包含属性可能会对数据库的安全性造成影响。 启用包含数据库将使数据库所有者授予对此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的访问权限。 禁用包含数据库可以阻止用户进行连接。 若要了解包含属性的影响，请参阅 [Contained Databases](../../relational-databases/databases/contained-databases.md) 和 [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md)。  
  
## <a name="filestream"></a>FILESTREAM  
 **FILESTREAM 访问级别**  
 显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上支持的 FILESTREAM 的当前级别。 若要更改访问级别，请选择以下值之一：  
  
 **已禁用**  
 无法将二进制大型对象 (BLOB) 数据存储在文件系统中。 这是默认值。  
  
 **已启用 Transact-SQL 访问**  
 可使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]访问 FILESTREAM 数据，但不能通过文件系统进行访问。  
  
 **已启用完全访问**  
 FILESTREAM 数据可使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 以及通过文件系统进行访问。  
  
 在首次启用 FILESTREAM 时，您可能需要重新启动计算机才能配置驱动程序。  
  
 **FILESTREAM 共享名称**  
 显示在安装过程中选择的 FILESTREAM 共享的只读名称。 有关详细信息，请参阅 [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md)。  
  
## <a name="miscellaneous"></a>杂项  
 **允许触发器激发其他触发器**  
 允许触发器激发其他触发器。 触发器最多可以嵌套 32 级。 有关详细信息，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md) 中的“嵌套触发器”部分。  
  
 **阻塞的进程阈值**  
 生成阻塞的进程报告时使用的阈值（以秒为单位）。 该阈值可介于 0 到 86,400 之间。 默认情况下，不生成阻塞的进程报告。 有关详细信息，请参阅 [阻塞的进程阈值服务器配置选项](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md)。  
  
 **游标阈值**  
 指定游标集中的行数，超过此行数，将异步生成游标键集。 当游标为结果集生成键集时，查询优化器会估算将为该结果集返回的行数。 如果查询优化器估算出的返回行数大于此阈值，则将异步生成游标，使用户能够在继续填充游标的同时从该游标中提取行。 否则，同步生成游标，查询将一直等待到返回所有行。  
  
 如果设置为 -1，则将同步生成所有键集；这适用于较小的游标集。 如果设置为 0，则将异步生成所有游标键集。 如果设置为其他值，则查询优化器将比较游标集中的预期行数，并在该行数超过所设置的数量时异步生成键集。 有关详细信息，请参阅 [配置 cursor threshold 服务器配置选项](../../database-engine/configure-windows/configure-the-cursor-threshold-server-configuration-option.md)。  
  
 **默认全文语言**  
 指定全文检索列的默认语言。 全文检索数据的语言分析取决于数据的语言。 该选项的默认值为服务器的语言。 有关与所显示设置相对应的语言，请参阅 [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)。  
  
 **默认语言**  
 所有新登录名的默认语言，除非另行指定。  
  
 **全文升级选项**  
 控制在从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]升级数据库时迁移全文检索的方式。 此属性适用于以下升级方式：附加数据库、还原数据库备份、还原文件备份或使用复制数据库向导复制数据库。  
  
 可以选择的选项如下：  
  
 **导入**  
 导入全文目录。 该操作的执行速度比 **“重新生成”** 的执行速度快很多。 不过，导入的全文目录不能使用 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]中引入的新的和增强的断字符。 因此，最终可能还是要重新生成全文目录。  
  
 如果全文目录不可用，则会重新生成关联的全文检索。 此选项仅对 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库可用。  
  
 **重新生成**  
 使用新的和增强的断字符重新生成全文目录。 重新生成索引可能需要一些时间，且升级后可能需要占用大量的 CPU 和内存。  
  
 **重置**  
 重置全文目录。 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 将删除全文目录文件，但会保留全文目录和全文索引的元数据。 在进行升级后，所有全文检索将禁用更改跟踪，并且不会自动启动爬网。 在升级完成后，目录将保留为空，直至手动执行完全填充。  
  
 有关如何选择全文升级选项的信息，请参阅 [升级全文搜索](../../relational-databases/search/upgrade-full-text-search.md)。  
  
> [!NOTE]  
>  也可以使用 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)upgrade_option 操作来设置全文升级选项。  
  
 附加、还原或复制 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库后，该数据库将立即变为可用，然后自动进行升级。 如果数据库具有全文检索，升级过程将导入、重置或重新生成它们，具体取决于 **全文升级选项** 服务器属性的设置。 如果将升级选项设置为“导入”或“重新生成”，在升级过程中将无法使用全文检索。 导入可能需要数小时，而重新生成所需的时间最多时可能十倍于此，具体取决于要编制索引的数据量。 另请注意，当升级选项设置为“导入”时，如果全文目录不可用，将重新生成关联的全文检索。 有关查看或更改“全文升级选项”属性设置的信息，请参阅[管理和监视服务器实例的全文搜索](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md)。  
  
 **最大文本复制大小**  
 指定使用单个 INSERT、UPDATE、WRITETEXT 或 UPDATETEXT 语句可以向已复制列或已捕获列添加的 **text**、 **ntext**、 **varchar(max)** 、 **nvarchar(max)** 、 **xml** 和 **image** 数据的最大大小（以字节为单位）。 对设置的更改将立即生效。 有关详细信息，请参阅 [配置 max text repl size 服务器配置选项](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md)。  
  
 **启动时扫描存储过程**  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在启动时扫描并自动执行存储过程。 如果设置为 **True**，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将扫描并自动运行服务器上定义的所有存储过程。 如果设置为 **False** （默认值），则不执行扫描。 有关详细信息，请参阅 [配置 scan for startup procs 服务器配置选项](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md)。  
  
 **两位数年份截止**  
 指示可作为两位数年份输入的最高年数。 可将所列年份及其之前的 99 年作为两位数年份输入。 所有其他年份必须作为四位数年份输入。  
  
 例如，2049 的默认设置表明：作为 '3/14/49' 输入的日期将被解释为 2049 年 3 月 14 日，而作为 '3/14/50' 输入的日期则将被解释为 1950 年 3 月 14 日。 有关详细信息，请参阅 [配置两位数年份截止服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。  
  
## <a name="network"></a>网络  
 **网络数据包大小**  
 设置整个网络使用的数据包大小（字节）。 默认数据包大小为 4096 个字节。 应用程序进行批量复制操作，或者发送或接收大量 **text** 或 **image** 数据时，数据包大小大于默认值可以提高效率，因为这样可以减少网络读写操作。 如果应用程序发送和接收的信息量很小，可以将数据包的大小设置为 512 个字节，这对于大多数数据传输来说已经足够了。 有关详细信息，请参阅 [配置 network packet size 服务器配置选项](../../database-engine/configure-windows/configure-the-network-packet-size-server-configuration-option.md)。  
  
> [!NOTE]  
>  除非您确信能够提高性能，否则不要更改数据包的大小。 对于大多数应用程序而言，默认数据包大小为最佳数值。  
  
 **远程登录超时值**  
 指定从远程登录尝试失败返回之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 等待的秒数。 此设置影响为执行异类查询所创建的与 OLE DB 访问接口的连接。 默认值为 20 秒。 如果该值为 0，则允许无限期等待。 有关详细信息，请参阅 [配置 remote login timeout 服务器配置选项](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md)。  
  
 对设置的更改将立即生效。  
  
## <a name="parallelism"></a>并行度：  
 **并行的开销阈值**  
 指定阈值，在高于该阈值时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将创建并运行查询并行计划。 开销指的是在特定硬件配置中运行串行计划估计需要花费的时间（秒）。 只能为对称多处理器设置此选项。 有关详细信息，请参阅 [配置并行的开销阈值服务器配置选项](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md)。  
  
 **锁**  
 设置可用锁的最大数目，以限制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为锁分配的内存量。 默认设置为 0，即允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据不断变化的系统要求动态地分配和释放锁。  
  
 推荐的配置是允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 动态地使用锁。 有关详细信息，请参阅 [配置 locks 服务器配置选项](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md)。  
  
 **最大并行度**  
 限制执行并行计划时所使用的处理器数（最多为 64 个）。 如果默认值为 0，则使用所有可用的处理器。 如果该值为 1，则取消生成并行计划。 如果该值大于 1，则将限制执行的单个查询所使用的最大处理器数。 如果指定的值比可用的处理器数大，则使用实际可用数量的处理器。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。  
  
 **查询等待值**  
 指定在超时之前查询等待资源的秒数（0 到 2147483647）。如果使用默认值 -1，则按估计查询开销的 25 倍计算超时值。 有关详细信息，请参阅 [配置查询等待值服务器配置选项](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md)。  
  
## <a name="see-also"></a>另请参阅  
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  
  
  
