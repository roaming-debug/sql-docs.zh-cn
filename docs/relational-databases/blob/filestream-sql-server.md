---
title: FILESTREAM (SQL Server) | Microsoft Docs
description: 了解 FILESTREAM，它是在文件系统中存储数据的 SQL Server 功能。 了解它如何存储、保护数据及提供对数据的访问。
ms.custom: ''
ms.date: 01/11/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server]
- FILESTREAM [SQL Server], about
- FILESTREAM [SQL Server], overview
ms.assetid: 9a5a8166-bcbe-4680-916c-26276253eafa
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 58b68bdf2996446ed08a37297e0c9776c2274832
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483659"
---
# <a name="filestream-sql-server"></a>FILESTREAM (SQL Server)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only.md)]

借助 FILESTREAM，基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的应用程序可以将非结构化的数据（如文档和图像）存储在文件系统中。 应用程序在利用丰富的流式 API 和文件系统的性能的同时，还可保持非结构化数据和对应的结构化数据之间的事务一致性。  
  
FILESTREAM 通过将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] varbinary(max) **二进制大型对象 (BLOB) 数据作为文件存储在 NTFS 或 ReFS 文件系统中，将** 与该文件系统集成在一起。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可插入、更新、查询、搜索和备份 FILESTREAM 数据。 Win32 文件系统接口提供对数据的流访问权限。  
  
FILESTREAM 使用 NT 系统缓存来缓存文件数据。 这有助于减少 FILESTREAM 数据可能对 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 性能产生的任何影响。 由于没有使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 缓冲池，因此该内存可用于查询处理。  
  
在安装或升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时，并不会自动启用 FILESTREAM。 您必须使用 SQL Server 配置管理器和 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]来启用 FILESTREAM。 若要使用 FILESTREAM，您必须创建或修改数据库以包含一个特殊类型的文件组。 然后，创建或修改某个表，以使其包含一个具有 FILESTREAM 属性的 **varbinary(max)** 列。 在完成这些任务后，您可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和 Win32 来管理 FILESTREAM 数据。  

## <a name="when-to-use-filestream"></a>何时使用 FILESTREAM

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，BLOB 可以是将数据存储在表中的标准 **varbinary(max)** 数据，也可以是将数据存储在文件系统中的 FILESTREAM **varbinary(max)** 对象。 数据的大小和应用情况决定您应该使用数据库存储还是文件系统存储。 如果满足以下条件，则应考虑使用 FILESTREAM：  

- 所存储的对象平均大于 1 MB。  
- 快速读取访问很重要。
- 您开发的是使用中间层作为应用程序逻辑的应用程序。  

对于较小的对象，将 **varbinary(max)** BLOB 存储在数据库中通常会提供更为优异的流性能。  

## <a name="filestream-storage"></a>FILESTREAM 存储

FILESTREAM 存储以 **varbinary(max)** 列的形式实现，在该列中数据以 BLOB 的形式存储在文件系统中。 BLOB 的大小仅受文件系统容量大小的限制。 文件大小为 2 GB 的 **varbinary(max)** 标准限制不适用于存储在文件系统中的 BLOB。  
  
若要指定列应将数据存储在文件系统中，请对 **varbinary(max)** 列指定 FILESTREAM 属性。 这样 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 会将该列的所有数据存储在文件系统，而不是数据库文件中。  
  
FILESTREAM 数据必须存储在 FILESTREAM 文件组中。 FILESTREAM 文件组是包含文件系统目录而非文件本身的专用文件组。 这些文件系统目录称为“数据容器” 。 数据容器是 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 存储与文件系统存储之间的接口。 

使用 FILESTREAM 存储时，请考虑以下内容：  

- 如果表包含 FILESTREAM 列，则每一行都必须具有唯一的一个非 Null 行 ID。  
- 可以将多个数据容器添加到 FILESTREAM 文件组。  
- 不能嵌套 FILESTREAM 数据容器。  
- 使用故障转移群集时，FILESTREAM 文件组必须位于共享磁盘资源上。  
- FILESTREAM 文件组可位于压缩卷上。

### <a name="integrated-management"></a>集成管理

由于 FILESTREAM 作为 **varbinary(max)** 列实现并直接集成到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]中，因此绝大多数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理工具和函数在使用过程中不会修改 FILESTREAM 数据。 例如，您可以对 FILESTREAM 数据使用所有备份模式和恢复模式，并且 FILESTREAM 数据是与数据库中的结构化数据一起进行备份的。 如果您不想将 FILESTREAM 数据与关系数据一起备份，则可以使用部分备份将 FILESTREAM 文件组排除在外。  

### <a name="integrated-security"></a>集成安全性

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，保护 FILESTREAM 数据的方式与其他数据相同，即通过在表级或列级授予权限。 如果用户具有访问表中 FILESTREAM 列的权限，则用户可打开关联文件。  

> [!NOTE]
> FILESTREAM 数据不支持加密。  

仅对用于运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户的帐户授予访问 FILESTREAM 容器的权限。 建议不对其他帐户授予访问数据容器的权限。

> [!NOTE]
> SQL 登录名不能与 FILESTREAM 容器配合工作。 只有 NTFS 或 ReFS 身份验证可与 FILESTREAM 容器配合工作。

## <a name="accessing-blob-data-with-transact-sql-and-file-system-streaming-access"></a><a name="dual"></a> 使用 Transact-SQL 和文件系统流访问来访问 BLOB 数据

在将数据存储在 FILESTREAM 列中之后，即可通过使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 事务或 Win32 API 访问文件。  
  
### <a name="transact-sql-access"></a>Transact-SQL 访问

可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]来插入、更新和删除 FILESTREAM 数据：  

- 您可以使用插入操作用 null 值、空值或相对较短的内联数据预填充 FILESTREAM 字段。 但是，大量数据将以流的方式更有效地导入到使用 Win32 接口的文件中。  
- 更新 FILESTREAM 字段时，即会修改文件系统中的基础 BLOB 数据。 将 FILESTREAM 字段设置为 NULL 即会删除与该字段相关联的 BLOB 数据。 你不能使用作为 UPDATE [!INCLUDE[tsql](../../includes/tsql-md.md)] .**Write() 实现的** 成块更新以对数据执行部分更新。 
- 当删除行或者删除或截断包含 FILESTREAM 数据的表时，将会删除文件系统中的基础 BLOB 数据。

### <a name="file-system-streaming-access"></a>文件系统流访问

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事务上下文中支持 Win32 流。 在事务之内，可以使用 FILESTREAM 函数来获取文件的逻辑 UNC 文件系统路径。 接着使用 OpenSqlFilestream API 来获取文件句柄。 然后 Win32 文件流接口（如 ReadFile() 和 WriteFile()）可使用此句柄通过文件系统访问并更新文件。  

由于文件操作属于事务操作，因此您无法通过文件系统删除或重命名 FILESTREAM 文件。  

**语句模型**

FILESTREAM 文件系统访问通过使用文件打开和关闭来构建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句模型。 当打开文件句柄时，语句开始；当关闭句柄时，语句结束。 例如，关闭写句柄时将激发表上注册的任何可能的 AFTER 触发器，就如 UPDATE 语句结束一样。

**存储命名空间**

在 FILESTREAM 中， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 控制 BLOB 物理文件系统的命名空间。 [PathName](../../relational-databases/system-functions/pathname-transact-sql.md)是一个新增的内部函数，它提供对应于表中每个 FILESTREAM 单元的 BLOB 的逻辑 UNC 路径。 应用程序使用此逻辑路径来获取 Win32 句柄并通过使用常见的 Win32 文件系统接口对 BLOB 数据进行操作。 如果 FILESTREAM 列的值为 NULL，则此函数将返回 NULL。  

**事务文件系统访问**

[GET_FILESTREAM_TRANSACTION_CONTEXT()](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md)是一个新增的内部函数，它提供表示与会话相关联的当前事务的标记。 事务必须已启动，且仍未被中止或提交。 通过获取标记，应用程序将 FILESTREAM 文件系统流操作与已启动的事务绑定在一起。 在没有显式启动的事务的情况下，此函数将返回 NULL。  

在提交或中止事务之前必须关闭所有文件句柄。 如果在事务范围之外仍有句柄处于打开状态，则对句柄执行的其他读取将失败，对句柄执行的其他写入将成功，但实际数据不会写入磁盘中。 与此类似，如果数据库或 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例关闭，则所有处于打开状态的句柄均无效。  

**事务持续性**

在事务提交时， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 通过使用 FILESTREAM 可确保通过文件系统流访问进行修改的 FILESTREAM BLOB 数据的事务持久性。  

**隔离语义**

隔离语义由 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 事务隔离级别决定。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和文件系统访问支持已提交读隔离级别。 支持重复的读取操作以及可序列化隔离和快照隔离。 不支持脏读。  

文件系统访问的打开操作不等待任何锁。 与此相反，如果打开操作因为事务隔离而无法访问数据，则打开操作将立即失败。 如果由于隔离冲突而无法继续执行打开操作，则流 API 调用将失败并返回 ERROR_SHARING_VIOLATION 错误。  

为了允许执行部分更新，应用程序可发出一个设备 FS 控制 (FSCTL_SQL_FILESTREAM_FETCH_OLD_CONTENT) 以将旧内容提取到处于打开状态的句柄所引用的文件中。 这将触发服务器端的旧内容复制操作。 为了提升应用程序性能并在处理超大型文件时避免发生潜在的超时问题，建议您使用异步 I/O。  

如果在写入句柄之后发出 FSCTL，则将保持最后一个写入操作并丢失之前对句柄执行的写入操作。

**文件系统 API 和支持的隔离级别**

当文件系统 API 由于隔离冲突而无法打开文件时，将返回 ERROR_SHARING_VIOLATION 异常。 两个事务尝试访问同一文件时，将发生此隔离冲突。 访问操作的结果取决于打开该文件的模式和运行事务的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。 下表概括了访问同一文件的两个事务的可能结果。

|事务 1|事务 2|在 SQL Server 2008 上的结果|在 SQL Server 2008 R2 和更高版本上的结果|  
|-------------------|-------------------|--------------------------------|------------------------------------------------------|  
|打开以进行读取。|打开以进行读取。|都成功。|都成功。|  
|打开以进行读取。|打开以进行写入。|都成功。 事务 2 下的写入操作不影响在事务 1 中执行的读取操作。|都成功。 事务 2 下的写入操作不影响在事务 1 中执行的读取操作。|  
|打开以进行写入。|打开以进行读取。|事务 2 下的打开失败，并发生 ERROR_SHARING_VIOLATION 异常。|都成功。|  
|打开以进行写入。|打开以进行写入。|事务 2 下的打开失败，并发生 ERROR_SHARING_VIOLATION 异常。|事务 2 下的打开失败，并发生 ERROR_SHARING_VIOLATION 异常。|  
|打开以进行读取。|打开以进行 SELECT。|都成功。|都成功。|  
|打开以进行读取。|打开以进行 UPDATE 或 DELETE。|都成功。 事务 2 下的写入操作不影响在事务 1 中执行的读取操作。|都成功。 事务 2 下的写入操作不影响在事务 1 中执行的读取操作。|  
|打开以进行写入。|打开以进行 SELECT。|事务 2 在事务 1 提交或结束事务前或该事务锁超时前一直阻塞。|都成功。|  
|打开以进行写入。|打开以进行 UPDATE 或 DELETE。|事务 2 在事务 1 提交或结束事务前或该事务锁超时前一直阻塞。|事务 2 在事务 1 提交或结束事务前或该事务锁超时前一直阻塞。|  
|打开以进行 SELECT。|打开以进行读取。|都成功。|都成功。|  
|打开以进行 SELECT。|打开以进行写入。|都成功。 事务 2 下的写入操作不影响事务 1。|都成功。 事务 2 下的写入操作不影响事务 1。|  
|打开以进行 UPDATE 或 DELETE。|打开以进行读取。|事务 2 下的打开操作失败，并发生 ERROR_SHARING_VIOLATION 异常。|都成功。|  
|打开以进行 UPDATE 或 DELETE。|打开以进行写入。|事务 2 下的打开操作失败，并发生 ERROR_SHARING_VIOLATION 异常。|事务 2 下的打开操作失败，并发生 ERROR_SHARING_VIOLATION 异常。|  
|打开以进行可重复读取的 SELECT。|打开以进行读取。|都成功。|都成功。|  
|打开以进行可重复读取的 SELECT。|打开以进行写入。|事务 2 下的打开操作失败，并发生 ERROR_SHARING_VIOLATION 异常。|事务 2 下的打开操作失败，并发生 ERROR_SHARING_VIOLATION 异常。|

**从远程客户端写透**

对 FILESTREAM 数据的远程文件系统访问是通过 Server Message Block (SMB) 协议启用的。 如果客户端为远程客户端，则客户端不对任何写入操作进行缓存。 写入操作将始终发送到服务器。 服务器端可对数据进行缓存。 建议运行在远程客户端上的应用程序合并小型写入操作，以减小使用较大数据大小的写入操作数量。  

不支持通过使用 FILESTREAM 句柄创建内存映射视图（内存映射 I/O）。 如果内存映射用于 FILESTREAM 数据，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将无法保证数据的一致性和持久性或数据库的完整性。  

## <a name="related-tasks"></a>Related Tasks

[启用和配置 FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)  
[创建启用了 FILESTREAM 的数据库](../../relational-databases/blob/create-a-filestream-enabled-database.md)  
[创建表以存储 FILESTREAM 数据](../../relational-databases/blob/create-a-table-for-storing-filestream-data.md)  
[使用 Transact-SQL 访问 FILESTREAM 数据](../../relational-databases/blob/access-filestream-data-with-transact-sql.md) [为 FILESTREAM 数据创建客户端应用程序](../../relational-databases/blob/create-client-applications-for-filestream-data.md)  
[使用 OpenSqlFilestream 访问 FILESTREAM 数据](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
[对 FILESTREAM 数据进行部分更新](../../relational-databases/blob/make-partial-updates-to-filestream-data.md)  
[避免与 FILESTREAM 应用程序中的数据库操作冲突](../../relational-databases/blob/avoid-conflicts-with-database-operations-in-filestream-applications.md)  
[移动启用了 FILESTREAM 的数据库](../../relational-databases/blob/move-a-filestream-enabled-database.md)  
[在故障转移群集中设置 FILESTREAM](../../relational-databases/blob/set-up-filestream-on-a-failover-cluster.md)  
[将防火墙配置为进行 FILESTREAM 访问](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)

## <a name="related-content"></a>相关内容

[FILESTREAM 与其他 SQL Server 功能的兼容性](../../relational-databases/blob/filestream-compatibility-with-other-sql-server-features.md)
<br>[Filestream 和 FileTable 动态管理视图 (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Filestream 和 FileTable 目录视图 (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[Filestream 和 FileTable 系统存储过程 (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)

