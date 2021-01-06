---
title: 数据库文件和文件组 | Microsoft Docs
description: 了解数据库文件以及如何在 SQL Server 中创建用于分配和管理目的的文件组。 查看示例、规则和建议。
ms.custom: contperf-fy20q4
ms.date: 05/29/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], files
- filegroups [SQL Server]
- transaction logs [SQL Server], about
- transaction logs [SQL Server], files
- .mdf files
- data files [SQL Server]
- default filegroups
- files [SQL Server], about files and filegroups
- secondary files [SQL Server]
- log files [SQL Server]
- .ndf files
- files [SQL Server]
- .ldf files
- database files [SQL Server]
- databases [SQL Server], filegroups
- filegroups [SQL Server], types
- primary filegroups [SQL Server]
- user-defined filegroups [SQL Server]
- filegroups [SQL Server], about filegroups
- primary files [SQL Server]
- file types [SQL Server]
ms.assetid: 9ca11918-480d-4838-9198-cec221ef6ad0
author: stevestein
ms.author: sstein
ms.openlocfilehash: f14eaec691218c4f7a9cad82dabb2f31f36b6245
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878740"
---
# <a name="database-files-and-filegroups"></a>数据库文件和文件组
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库至少具有两个操作系统文件：一个数据文件和一个日志文件。 数据文件包含数据和对象，例如表、索引、存储过程和视图。 日志文件包含恢复数据库中的所有事务所需的信息。 为了便于分配和管理，可以将数据文件集合起来，放到文件组中。  
  
## <a name="database-files"></a>数据库文件  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库具有三种类型的文件，如下表所示：  
  
|文件|说明|  
|----------|-----------------|  
|主|包含数据库的启动信息，并指向数据库中的其他文件。 每个数据库有一个主要数据文件。 主要数据文件的建议文件扩展名是 .mdf。|  
|辅助副本|用户定义的可选数据文件。 通过将每个文件放在不同的磁盘驱动器上，可将数据分散到多个磁盘中。 次要数据文件的建议文件扩展名是 .ndf。|  
|事务日志|此日志包含用于恢复数据库的信息。 每个数据库必须至少有一个日志文件。 事务日志的建议文件扩展名是 .ldf。|  
  
 例如，简单数据库“Sales”包括一个包含所有数据和对象的主要文件和一个包含事务日志信息的日志文件。 可以创建一个更复杂的数据库“Orders”，其中包括一个主要文件和五个次要文件。 数据库中的数据和对象分散在所有六个文件中，而四个日志文件包含事务日志信息。  
  
 默认情况下，数据和事务日志放在同一驱动器和路径上，以处理单磁盘系统。 对于生产环境，这可能不是最佳选择。 建议将数据和日志文件放在不同的磁盘上。  

### <a name="logical-and-physical-file-names"></a>逻辑和物理文件名称
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件具有两种文件名类型：

**logical_file_name：** logical_file_name 是在所有 Transact-SQL 语句中引用物理文件时所使用的名称。 逻辑文件名必须符合 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符规则，而且在数据库中的逻辑文件名中必须是唯一的。

**os_file_name：** os_file_name 是包括目录路径的物理文件的名称。 它必须符合操作系统文件命名规则。

 有关 `NAME` 和 `FILENAME` 参数的详细信息，请参阅 [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)。

> [!IMPORTANT]
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据和日志文件可以保存在 FAT 或 NTFS 文件系统中。 由于 NTFS 在安全方面具有优势，因此，建议在 Windows 系统上使用 NTFS 文件系统。 

> [!WARNING]
> NTFS 压缩文件系统不支持读/写数据文件组和日志文件。 仅允许只读数据库和只读次要文件组保存在 NTFS 压缩文件系统中。
> 为了节省空间，强烈建议使用[数据压缩](../../relational-databases/data-compression/data-compression.md)而不是文件系统压缩。

如果多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例在一台计算机上运行，则每个实例都会接收到不同的默认目录来保存在该实例中创建的数据库文件。 有关详细信息，请参阅 [SQL Server 的默认实例和命名实例的文件位置](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)。

### <a name="data-file-pages"></a>数据文件页
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件中的页按顺序编号，文件的首页以 0 开始。 数据库中的每个文件都有一个唯一的文件 ID 号。 若要唯一标识数据库中的页，需要同时使用文件 ID 和页码。 下例显示了包含 4-MB 主数据文件和 1-MB 次要数据文件的数据库中的页码。

![data_file_pages](../../relational-databases/databases/media/data-file-pages.gif)

文件标头页是包含有关文件属性信息的首页。 在文件开始处的其他几页也包含系统信息（例如分配映射）。 有一个存储在主数据文件和第一个日志文件中的系统页是包含数据库属性信息的数据库引导页。
### <a name="file-size"></a>文件大小
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件可以从它们最初指定的大小开始自动增长。 在定义文件时，您可以指定一个特定的增量。 每次填充文件时，其大小均按此增量来增长。 如果文件组中有多个文件，则它们在所有文件被填满之前不会自动增长。

 有关页和页类型的详细信息，请参阅[页和盘区体系结构指南](../..//relational-databases/pages-and-extents-architecture-guide.md)。


每个文件还可以指定一个最大大小。 如果没有指定最大大小，文件可以一直增长到用完磁盘上的所有可用空间。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作为数据库嵌入某应用程序，而该应用程序的用户无法迅速与系统管理员联系，则此功能就特别有用。 用户可以使文件根据需要自动增长，以减轻监视数据库中的可用空间和手动分配额外空间的管理负担。  

有关事务日志文件管理的详细信息，请参阅[管理事务日志文件的大小](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md#Recommendations)。   

## <a name="database-snapshot-files"></a>数据库快照文件
数据库快照存储其“写入时复制”数据时所用的文件格式取决于快照是由用户创建，还是在内部使用：

* 用户创建的数据库快照将其数据存储在一个或多个稀疏文件中。 稀疏文件技术是 NTFS 文件系统的一项功能。 首先，稀疏文件不包含任何用户数据，并且没有为稀疏文件分配用于用户数据的磁盘空间。 若要大致了解如何在数据库快照中使用稀疏文件以及数据库快照如何增长，请参阅 [查看数据库快照的稀疏文件的大小](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md)。 
* 数据库快照通过特定的 DBCC 命令在内部使用。 这些命令包括 DBCC CHECKDB、DBCC CHECKTABLE、DBCC CHECKALLOC 和 DBCC CHECKFILEGROUP。 内部数据库快照使用原始数据库文件的稀疏备用数据流。 和稀疏文件一样，备用数据库流也是 NTFS 文件系统的一项功能。 使用稀疏备用数据流，可以进行多项数据分配，使其与单个文件或文件夹进行关联，但不影响文件大小或卷统计信息。 
  
## <a name="filegroups"></a>文件组  
* 此文件组包含主要数据文件和未放入其他文件组的所有次要文件。 
* 可以创建用户定义的文件组，用于将数据文件集合起来，以便于管理、数据分配和放置。  
  
 例如：可以分别在三个磁盘驱动器上创建 `Data1.ndf`、`Data2.ndf` 和 `Data3.ndf`，然后将它们分配给文件组 `fgroup1`。 然后，可以明确地在文件组 `fgroup1` 上创建一个表。 对表中数据的查询将分散到三个磁盘上，从而提高了性能。 通过使用在 RAID（独立磁盘冗余阵列）条带集上创建的单个文件也能获得同样的性能提高。 但是，文件和文件组使您能够轻松地在新磁盘上添加新文件。  
  
 下表列出了存储在文件组中的所有数据文件。  
  
|文件组|说明|  
|---------------|-----------------|  
|主|包含主要文件的文件组。 所有系统表都是主要文件组的一部分。|  
|内存优化数据|内存优化文件组基于 Filestream 文件组|  
|文件流||    
|用户定义|用户首次创建数据库或以后修改数据库时创建的任何文件组。|  
  
### <a name="default-primary-filegroup"></a>默认 (Primary) 文件组  
 如果在数据库中创建对象时没有指定对象所属的文件组，对象将被分配给默认文件组。 不管何时，只能将一个文件组指定为默认文件组。 默认文件组中的文件必须足够大，能够容纳未分配给其他文件组的所有新对象。  
  
 PRIMARY 文件组是默认文件组，除非使用 ALTER DATABASE 语句进行了更改。 但系统对象和表仍然分配给 PRIMARY 文件组，而不是新的默认文件组。  
 
### <a name="memory-optimized-data-filegroup"></a>内存优化数据文件组

有关内存优化文件组的详细信息，请参阅[内存优化文件组](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)。

### <a name="filestream-filegroup"></a>Filestream 文件组

有关 Filestream 文件组的详细信息，请参阅 [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md#filestream-storage) 和 [创建启用了 FILESTREAM 的数据库](../../relational-databases/blob/create-a-filestream-enabled-database.md)。

### <a name="file-and-filegroup-example"></a>文件和文件组示例
 以下示例在 SQL Server 实例上创建了一个数据库。 该数据库包括一个主数据文件、一个用户定义文件组和一个日志文件。 主数据文件在主文件组中，而用户定义文件组包含两个次要数据文件。 ALTER DATABASE 语句将用户定义文件组指定为默认文件组。 然后通过指定用户定义文件组来创建表。 （此示例使用通用路径 `c:\Program Files\Microsoft SQL Server\MSSQL.1` 来避免指定 SQL Server 版本。）

```sql
USE master;
GO
-- Create the database with the default data
-- filegroup, filestream filegroup and a log file. Specify the
-- growth increment and the max size for the
-- primary data file.
CREATE DATABASE MyDB
ON PRIMARY
  ( NAME='MyDB_Primary',
    FILENAME=
       'c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\data\MyDB_Prm.mdf',
    SIZE=4MB,
    MAXSIZE=10MB,
    FILEGROWTH=1MB),
FILEGROUP MyDB_FG1
  ( NAME = 'MyDB_FG1_Dat1',
    FILENAME =
       'c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\data\MyDB_FG1_1.ndf',
    SIZE = 1MB,
    MAXSIZE=10MB,
    FILEGROWTH=1MB),
  ( NAME = 'MyDB_FG1_Dat2',
    FILENAME =
       'c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\data\MyDB_FG1_2.ndf',
    SIZE = 1MB,
    MAXSIZE=10MB,
    FILEGROWTH=1MB),
FILEGROUP FileStreamGroup1 CONTAINS FILESTREAM
  ( NAME = 'MyDB_FG_FS',
    FILENAME = 'c:\Data\filestream1')
LOG ON
  ( NAME='MyDB_log',
    FILENAME =
       'c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\data\MyDB.ldf',
    SIZE=1MB,
    MAXSIZE=10MB,
    FILEGROWTH=1MB);
GO
ALTER DATABASE MyDB 
  MODIFY FILEGROUP MyDB_FG1 DEFAULT;
GO

-- Create a table in the user-defined filegroup.
USE MyDB;
CREATE TABLE MyTable
  ( cola int PRIMARY KEY,
    colb char(8) )
ON MyDB_FG1;
GO

-- Create a table in the filestream filegroup
CREATE TABLE MyFSTable
(
    cola int PRIMARY KEY,
  colb VARBINARY(MAX) FILESTREAM NULL
)
GO
```

下图总结了上述示例的结果（Filestream 数据除外）。

![filegroup_example](../../relational-databases/databases/media/filegroup-example.gif)

## <a name="file-and-filegroup-fill-strategy"></a>文件和文件组填充策略
文件组对组内的所有文件都使用按比例填充策略。 当数据写入文件组时，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]按文件中的可用空间比例将数据写入文件组中的每个文件，而不是将所有数据都写入第一个文件直至其变满为止。 然后再写入下一个文件。 例如，如果文件 f1 有 100 MB 可用空间，文件 f2 有 200 MB 可用空间，则从文件 f1 中提供一个盘区，从文件 f2 中提供两个盘区，依此类推。 这样，两个文件几乎同时填满，并且可获得简单的条带化。

例如，某个文件组由三个文件组成，它们都设置为自动增长。 当文件组中所有文件的空间都已用完时，只扩展第一个文件。 当第一个文件已满，无法再向文件组中写入更多数据时，将扩展第二个文件。 当第二个文件已满，无法再向文件组中写入更多数据时，将扩展第三个文件。 当第三个文件已满，无法再向文件组中写入更多数据时，将再次扩展第一个文件，依此类推。

## <a name="rules-for-designing-files-and-filegroups"></a>文件和文件组的设计规则
下列规则适用于文件和文件组：
- 一个文件或文件组不能由多个数据库使用。 例如，任何其他数据库都不能使用包含 sales 数据库中的数据和对象的文件 sales.mdf 和 sales.ndf。
- 一个文件只能是一个文件组的成员。
- 事务日志文件不能属于任何文件组。

## <a name="recommendations"></a><a name="Recommendations"></a> 建议
使用文件和文件组时的建议： 
- 大多数数据库在只有单个数据文件和单个事务日志文件的情况下性能良好。
- 如果使用多个数据文件，请为附加文件创建第二个文件组，并将其设置为默认文件组。 这样，主文件将只包含系统表和对象。
- 若要使性能最大化，请在尽可能多的不同可用磁盘上创建文件或文件组。 将争夺空间最激烈的对象置于不同的文件组中。
- 使用文件组将对象放置在特定的物理磁盘上。
- 将在同一联接查询中使用的不同表置于不同的文件组中。 由于采用并行磁盘 I/O 对联接数据进行搜索，所以此步骤可改善性能。
- 将最常访问的表和属于这些表的非聚集索引置于不同的文件组中。 如果文件位于不同的物理磁盘上，由于采用并行 I/O，所以使用不同的文件组可改善性能。
- 请勿将事务日志文件置于已有其他文件和文件组的同一物理磁盘上。
- 如果需要使用 [Diskpart](/windows-server/administration/windows-commands/diskpart) 等工具扩展数据库文件所在的卷或分区，则应备份所有系统数据库和用户数据库，还要先停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。 此外，磁盘卷成功扩展后，应考虑运行 [`DBCC CHECKDB`](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 命令，确保驻留在该卷上的所有数据库的物理完整性。

有关事务日志文件管理建议的详细信息，请参阅[管理事务日志文件的大小](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md#Recommendations)。   

## <a name="related-content"></a>相关内容  
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)    
 [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)      
 [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)    
 [页和区体系结构指南](../../relational-databases/pages-and-extents-architecture-guide.md)    
 [管理事务日志文件的大小](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)