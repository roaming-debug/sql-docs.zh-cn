---
description: 事务锁定和行版本控制指南
title: 事务锁定和行版本控制指南
ms.custom: seo-dt-2019
ms.date: 03/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- guide, transaction locking and row versioning
- transaction locking and row versioning guide
- lock compatibility matrix, [SQL Server]
- lock granularity and hierarchies, [SQL Server]
- deadlocks, [SQL Server]
- lock escalation, [SQL Server]
- lock partitioning, [SQL Server]
ms.assetid: 44fadbee-b5fe-40c0-af8a-11a1eecf6cb7
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47545adc6e3e620cd74585d477bdd028f1e8625f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352496"
---
# <a name="transaction-locking-and-row-versioning-guide"></a>事务锁定和行版本控制指南
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在任意数据库中，事务管理不善常常导致用户很多的系统中出现争用和性能问题。 随着访问数据的用户数量的增加，拥有能够高效地使用事务的应用程序也变得更为重要。 本指南说明 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用的锁定和行版本控制机制，以确保每个事务的物理完整性并提供有关应用程序如何高效控制事务的信息。  
  
适用范围：[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]（[!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] 到 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]，除非特别指出）和 [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]。 
  
##  <a name="transaction-basics"></a><a name="Basics"></a> 事务基本知识  
 事务是作为单个逻辑工作单元执行的一系列操作。 一个逻辑工作单元必须有四个属性，称为原子性、一致性、隔离性和持久性 (ACID) 属性，只有这样才能成为一个事务。  
  
 **原子性**  
 事务必须是原子工作单元；对于其数据修改，要么全都执行，要么全都不执行。  
  
 **一致性**  
 事务在完成时，必须使所有的数据都保持一致状态。 在相关数据库中，所有规则都必须应用于事务的修改，以保持所有数据的完整性。 事务结束时，所有的内部数据结构（如 B 树索引或双向链表）都必须是正确的。  
  
 **隔离**  
 由并发事务所做的修改必须与任何其他并发事务所做的修改隔离。 事务识别数据时数据所处的状态，要么是另一并发事务修改它之前的状态，要么是第二个事务修改它之后的状态，事务不会识别中间状态的数据。 这称为可串行性，因为它能够重新装载起始数据，并且重播一系列事务，以使数据结束时的状态与原始事务执行的状态相同。  
  
 **持续性**  
 完成完全持久的事务之后，它的影响将永久存在于系统中。 该修改即使出现系统故障也将一直保持。 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 和更高版本将启用延迟的持久事务。 提交延迟的持久事务后，该事务日志记录将保留在磁盘上。 有关延迟事务持续性的详细信息，请参阅主题[事务持续性](../relational-databases/logs/control-transaction-durability.md)。  
  
 SQL 程序员要负责启动和结束事务，同时强制保持数据的逻辑一致性。 程序员必须定义数据修改的顺序，使数据相对于其组织的业务规则保持一致。 程序员将这些修改语句包括到一个事务中，使 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 能够强制该事务的物理完整性。  
  
 企业数据库系统（如[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例）有责任提供一种机制，保证每个事务的物理完整性。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]提供：  
  
-   锁定设备，使事务保持隔离。  
  
-   通过记录设备，保证事务持久性。 对于完全持久的事务，在其提交之前，日志记录将强制写入磁盘。 因此，即使服务器硬件、操作系统或 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 实例自身出现故障，该实例也可以在重新启动时使用事务日志，将所有未完成的事务自动地回滚到系统出现故障的点。 提交延迟的持久事务后，该事务日志记录将强制写入磁盘。 如果在日志记录强制写入磁盘前系统出现故障，此类事务可能会丢失。 有关延迟事务持续性的详细信息，请参阅主题[事务持续性](../relational-databases/logs/control-transaction-durability.md)。  
  
-   事务管理特性，强制保持事务的原子性和一致性。 事务启动之后，就必须成功完成（提交），否则[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例将撤消该事务启动之后对数据所做的所有修改。 此操作称为回滚事务，因为它将数据恢复到那些更改发生前的状态。  
  
### <a name="controlling-transactions"></a>控制事务  
 应用程序主要通过指定事务启动和结束的时间来控制事务。 可以使用 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句或数据库应用程序编程接口 (API) 函数来指定这些时间。 系统还必须能够正确处理那些在事务完成之前便终止事务的错误。 有关详细信息，请参阅[事务](../t-sql/language-elements/transactions-transact-sql.md)、[ODBC 中的事务](../relational-databases/native-client/odbc/performing-transactions-in-odbc.md)以及 [SQL Server Native Client (OLEDB) 中的事务](../relational-databases/native-client-ole-db-transactions/transactions.md)。  
  
 默认情况下，事务按连接级别进行管理。 在一个连接上启动一个事务后，该事务结束之前，在该连接上执行的所有 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句都是该事务的一部分。 但是，在多个活动的结果集 (MARS) 会话中，[!INCLUDE[tsql](../includes/tsql-md.md)] 显式或隐式事务将变成批范围的事务，这种事务按批处理级别进行管理。 当批处理完成时，如果批范围的事务还没有提交或回滚，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将自动回滚该事务。 有关详细信息，请参阅[使用多重活动结果集 (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)。  
  
#### <a name="starting-transactions"></a>启动事务  
 使用 API 函数和 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句，可以在 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例中将事务作为显式、自动提交或隐式事务来启动。  
  
 **显式事务**  
 显式事务是指这样的事务：您在其中通过 API 函数或发出 [!INCLUDE[tsql](../includes/tsql-md.md)] BEGIN TRANSACTION、COMMIT TRANSACTION、COMMIT WORK、ROLLBACK TRANSACTION 或 ROLLBACK WORK [!INCLUDE[tsql](../includes/tsql-md.md)] 语句明确定义事务的开始和结束。 当事务结束时，连接将返回到启动显式事务前所处的事务模式，或者是隐式模式，或者是自动提交模式。  
  
 您可以使用显式事务中除下列语句之外的所有 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句：  
  
:::row:::
    :::column:::
        ALTER DATABASE
    :::column-end:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT INDEX
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        CREATE FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        RECONFIGURE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT INDEX
    :::column-end:::
    :::column:::
        CREATE FULLTEXT INDEX
    :::column-end:::
    :::column:::
        RESTORE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        BACKUP
    :::column-end:::
    :::column:::
        DROP DATABASE
    :::column-end:::
    :::column:::
        全文系统存储过程
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        sp_dboption 用于设置数据库选项，或在显式事务或隐式事务内部修改 master 数据库的任何系统过程。
    :::column-end:::
:::row-end:::

> [!NOTE]  
> UPDATE STATISTICS 可在显式事务内使用。 但是，UPDATE STATISTICS 提交独立于封闭的事务，并且不能回滚。  
  
 **自动提交事务**  
 自动提交模式是 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]的默认事务管理模式。 每个 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句在完成时，都被提交或回滚。 如果一个语句成功地完成，则提交该语句；如果遇到错误，则回滚该语句。 只要没有显式事务或隐性事务覆盖自动提交模式，与[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例的连接就以此默认模式操作。 自动提交模式也是 ADO、OLE DB、ODBC 和 DB 库的默认模式。  
  
 **隐式事务**  
 当连接以隐性事务模式进行操作时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例将在提交或回滚当前事务后自动启动新事务。 无须描述事务的开始，只需提交或回滚每个事务。 隐性事务模式生成连续的事务链。 通过 API 函数或 [!INCLUDE[tsql](../includes/tsql-md.md)] SET IMPLICIT_TRANSACTIONS ON 语句，将隐性事务模式设置为打开。  此模式也称为 Autocommit OFF，请参阅 [JDBC 中的 setAutoCommit 方法](../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md) 
  
 为连接将隐性事务模式设置为打开之后，当[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例首次执行下列任何语句时，都会自动启动一个事务：  
  
:::row:::
    :::column:::
        ALTER TABLE
    :::column-end:::
    :::column:::
        FETCH
    :::column-end:::
    :::column:::
        REVOKE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE
    :::column-end:::
    :::column:::
        GRANT
    :::column-end:::
    :::column:::
        SELECT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DELETE
    :::column-end:::
    :::column:::
        INSERT
    :::column-end:::
    :::column:::
        TRUNCATE TABLE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DROP
    :::column-end:::
    :::column:::
        OPEN
    :::column-end:::
    :::column:::
        UPDATE
    :::column-end:::
:::row-end:::

-  **批处理级事务**  
   只能应用于多个活动结果集 (MARS)，在 MARS 会话中启动的 [!INCLUDE[tsql](../includes/tsql-md.md)] 显式或隐式事务变为批处理级事务。 当批处理完成时没有提交或回滚的批处理级事务自动由 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进行回滚。  
  
-  **分布式事务**  
   分布式事务跨越两个或多个称为资源管理器的服务器。 称为事务管理器的服务器组件必须在资源管理器之间协调事务管理。 如果分布式事务由 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 分布式事务处理协调器 (MS DTC) 之类的事务管理器或其他支持 Open Group XA 分布式事务处理规范的事务管理器来协调，则在这样的分布式事务中，每个 [!INCLUDE[msCoName](../includes/msconame-md.md)]实例都可以作为资源管理器来运行。 有关详细信息，请参阅 MS DTC 文档。  
  
   跨越两个或多个数据库的单个[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例中的事务实际上是分布式事务。 该实例对分布式事务进行内部管理；对于用户而言，其操作就像本地事务一样。  
  
   对于应用程序而言，管理分布式事务很像管理本地事务。 当事务结束时，应用程序会请求提交或回滚事务。 不同的是，分布式提交必须由事务管理器管理，以尽量避免出现因网络故障而导致事务由某些资源管理器成功提交，但由另一些资源管理器回滚的情况。 通过分两个阶段（准备阶段和提交阶段）管理提交进程可避免这种情况，这称为两阶段提交 (2PC)。  
  
   -  **准备阶段**  
      当事务管理器收到提交请求时，它会向该事务涉及的所有资源管理器发送准备命令。 然后，每个资源管理器将尽力使该事务持久，并且所有保存该事务日志映像的缓冲区将被刷新到磁盘中。 当每个资源管理器完成准备阶段时，它会向事务管理器返回准备成功或准备失败的消息。 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 引入了延迟的事务持续性。 在提交延迟的持久事务后，该事务的日志图像将刷入磁盘。 有关延迟事务持续性的详细信息，请参阅主题[事务持续性](../relational-databases/logs/control-transaction-durability.md)。  
  
   -  **提交阶段**  
      如果事务管理器从所有资源管理器收到准备成功的消息，它将向每个资源管理器发送一个提交命令。 然后，资源管理器就可以完成提交。 如果所有资源管理器都报告提交成功，那么事务管理器就会向应用程序发送一个成功通知。 如果任一资源管理器报告准备失败，那么事务管理器将向每个资源管理器发送一个回滚命令，并向应用程序表明提交失败。  
  
      [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]应用程序可以通过 [!INCLUDE[tsql](../includes/tsql-md.md)] 或数据库 API 来管理分布式事务。 有关详细信息，请参阅 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)。  
  
#### <a name="ending-transactions"></a>结束事务  
 您可以使用 COMMIT 或 ROLLBACK 语句，或者通过相应 API 函数来结束事务。  
  
-  **COMMIT**  
   如果事务成功，则提交。 COMMIT 语句保证事务的所有修改在数据库中都永久有效。 COMMIT 语句还释放事务使用的资源（例如，锁）。  
  
-  **ROLLBACK**  
   如果事务中出现错误，或用户决定取消事务，则回滚该事务。 ROLLBACK 语句通过将数据返回到它在事务开始时所处的状态，来取消事务中的所有修改。 ROLLBACK 还释放事务占用的资源。  
  
> [!NOTE]  
> 在为支持多个活动的结果集 (MARS) 而建立的连接中，只要还有待执行的请求，就无法提交通过 API 函数启动的显式事务。 如果在未完成的操作还在运行时尝试提交此类事务，将导致出现错误。  
  
#### <a name="errors-during-transaction-processing"></a>事务处理过程中的错误  
 如果某个错误使事务无法成功完成，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 会自动回滚该事务，并释放该事务占用的所有资源。 如果客户端与[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例的网络连接中断了，那么当网络向实例通知该中断后，该连接的所有未完成事务均会被回滚。 如果客户端应用程序失败或客户端计算机崩溃或重新启动，也会中断连接，而且当网络向[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例通知该中断后，该实例会回滚所有未完成的连接。 如果客户端从该应用程序注销，所有未完成的事务也会被回滚。  
  
 如果批中出现运行时语句错误（如违反约束），那么[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]中的默认行为是只回滚产生该错误的语句。 可以使用 `SET XACT_ABORT` 语句更改此行为。 在执行 `SET XACT_ABORT` ON 之后，任何运行时语句错误将导致当前事务的自动回滚。 编译错误（如语法错误）不受 `SET XACT_ABORT` 的影响。 有关详细信息，请参阅 [SET XACT_ABORT (Transact-SQL)](../t-sql/statements/set-xact-abort-transact-sql.md)。  
  
 出现错误时，纠正操作（`COMMIT` 或 `ROLLBACK`）应包括在应用程序代码中。 处理错误（包括事务中的错误）的一种有效工具是 [!INCLUDE[tsql](../includes/tsql-md.md)] `TRY...CATCH` 构造。 有关包括事务的示例的详细信息，请参阅 [TRY...CATCH (Transact-SQL)](../t-sql/language-elements/try-catch-transact-sql.md)。 从 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 开始，可使用 `THROW` 语句引发异常并将执行转移到 `TRY...CATCH` 构造的 `CATCH` 块。 有关详细信息，请参阅 [THROW (Transact-SQL)](../t-sql/language-elements/throw-transact-sql.md)。  
  
##### <a name="compile-and-run-time-errors-in-autocommit-mode"></a>自动提交模式下的编译和运行时错误  
 在自动提交模式下，有时看起来好像[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例回滚了整个批处理而不是仅仅一个 SQL 语句。 当遇到的错误是编译错误而非运行时错误时，会发生这种情况。 编译错误会阻止[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]生成执行计划，这样批处理中的任何语句都不会执行。 尽管看起来好像是回滚了产生错误的语句之前的所有语句，但该错误阻止了批处理中的所有语句的执行。 在下面的示例中，由于发生编译错误，第三个批处理中的 `INSERT` 语句都没有执行。 但看起来好像是前两个 `INSERT` 语句没有执行便进行了回滚。  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUSE (3, 'ccc');  -- Syntax error.  
GO  
SELECT * FROM TestBatch;  -- Returns no rows.  
GO  
```  
  
 在下面的示例中，第三个 `INSERT` 语句产生运行时重复主键错误。 由于前两个 `INSERT` 语句成功地执行并且提交，因此它们在运行时错误之后被保留下来。  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUES (1, 'ccc');  -- Duplicate key error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用延迟的名称解析，直到执行时才解析对象名称。 在下面的示例中，执行并提交了前两个 `INSERT` 语句，在第三个 `TestBatch` 语句由于引用一个不存在的表而产生运行时错误之后，这两行仍然保留在 `INSERT` 表中。  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBch VALUES (3, 'ccc');  -- Table name error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```   
  
##  <a name="locking-and-row-versioning-basics"></a><a name="Lock_Basics"></a> 锁定和行版本控制基本知识  
 当多个用户同时访问数据时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用以下机制确保事务的完整性和保持数据库的一致性：  
  
-  **锁定**    

   每个事务对所依赖的资源（如行、页或表）请求不同类型的锁。 锁可以阻止其他事务以某种可能会导致事务请求锁出错的方式修改资源。 当事务不再依赖锁定的资源时，它将释放锁。  
  
-  **行版本控制**    

   当启用了基于行版本控制的隔离级别时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]将维护修改的每一行的版本。 应用程序可以指定事务使用行版本查看事务或查询开始时存在的数据，而不是使用锁保护所有读取。 通过使用行版本控制，读取操作阻止其他事务的可能性将大大降低。  
  
 锁定和行版本控制可以防止用户读取未提交的数据，还可以防止多个用户尝试同时更改同一数据。 如果不进行锁定或行版本控制，对数据执行的查询可能会返回数据库中尚未提交的数据，从而产生意外的结果。  
  
 应用程序可以选择事务隔离级别，为事务定义保护级别，以防被其他事务所修改。 可以为各个 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句指定表级别的提示，进一步定制行为以满足应用程序的要求。  
  
### <a name="managing-concurrent-data-access"></a>管理并发数据访问  
 同时访问一种资源的用户被视为并发访问资源。 并发数据访问需要某些机制，以防止多个用户试图修改其他用户正在使用的资源时产生负面影响。  
  
#### <a name="concurrency-effects"></a>并发影响  
 修改数据的用户会影响同时读取或修改相同数据的其他用户。 即这些用户可以并发访问数据。 如果数据存储系统没有并发控制，则用户可能会看到以下负面影响：  
  
-   **丢失更新** 
  
     当两个或多个事务选择同一行，然后基于最初选定的值更新该行时，会发生丢失更新问题。 每个事务都不知道其他事务的存在。 最后的更新将覆盖由其他事务所做的更新，这将导致数据丢失。  
  
     例如，两个编辑人员制作了同一文档的电子副本。 每个编辑人员独立地更改其副本，然后保存更改后的副本，这样就覆盖了原始文档。 最后保存其更改副本的编辑人员覆盖另一个编辑人员所做的更改。 如果在一个编辑人员完成并提交事务之前，另一个编辑人员不能访问同一文件，则可避免此问题。  
  
-   **未提交的依赖关系（脏读）**  
  
     当第二个事务选择其他事务正在更新的行时，会发生未提交的依赖关系问题。 第二个事务正在读取的数据还没有提交并且可能由更新此行的事务所更改。  
  
     例如，一个编辑人员正在更改电子文档。 在更改过程中，另一个编辑人员复制了该文档（该副本包含到目前为止所做的全部更改）并将其分发给预期的用户。 此后，第一个编辑人员认为目前所做的更改是错误的，于是删除了所做的编辑并保存了文档。 分发给用户的文档包含不再存在的编辑内容，并且这些编辑内容应视为从未存在过。 如果在第一个编辑人员保存最终更改并提交事务之前，任何人都不能读取更改的文档，则可以避免此问题。  
  
-   **不一致的分析（不可重复读）**  
  
     当第二个事务多次访问同一行而且每次读取不同的数据时，会发生不一致的分析问题。 不一致的分析与未提交的依赖关系类似，因为其他事务也是正在更改第二个事务正在读取的数据。 但是，在不一致的分析中，第二个事务读取的数据是由已进行了更改的事务提交的。 此外，不一致的分析涉及多次（两次或更多）读取同一行，而且每次信息都被其他事务更改，因此我们称之为“不可重复读”。  
  
     例如，编辑人员两次读取同一文档，但在两次读取之间，作者重写了该文档。 当编辑人员第二次读取文档时，文档已更改。 原始读取不可重复。 如果在编辑人员完成最后一次读取文档之前，作者不能更改文档，则可以避免此问题。  
  
-   **虚拟读取**  
  
     执行两个相同的查询但第二个查询返回的行集合是不同的，此时就会发生虚拟读取。 下面的示例显示了何时会出现幻读。 假定下面两个事务同时执行。 由于第二个事务中的 `INSERT` 语句更改了两个事务所用的数据，所以第一个事务中的两个 `SELECT` 语句可能返回不同的结果。  
  
    ```sql  
    --Transaction 1  
    BEGIN TRAN;  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    --The INSERT statement from the second transaction occurs here.  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    COMMIT;  
    ```  
  
    ```sql  
    --Transaction 2  
    BEGIN TRAN;  
    INSERT INTO dbo.employee  
      (Id, Name) VALUES(6 ,'New');  
    COMMIT;   
    ```  
  
-   **由于行更新导致读取缺失和重复读**  
  
    -   缺失一个更新行或多次看到某更新行  
  
         在 `READ UNCOMMITTED` 级别运行的事务不会发出共享锁来防止其他事务修改当前事务读取的数据。 在 READ COMMITTED 级别运行的事务会发出共享锁，但是在读取行后会释放行锁或页锁。 无论哪种情况，在您扫描索引时，如果另一个用户在您读取期间更改行的索引键列，则在键更改将行移至您的扫描位置之前的位置时，该行可能会再次出现。 同样，在键更改将行移至您已读取的索引中的某位置时，该行将不会出现。 若要避免此问题，请使用 `SERIALIZABLE` 或 `HOLDLOCK` 提示或行版本控制。 有关详细信息，请参阅[表提示 (Transact-SQL)](../t-sql/queries/hints-transact-sql-table.md)。  
  
    -   缺失非更新目标的一行或多行  
  
         使用 `READ UNCOMMITTED` 时，如果使用分配顺序扫描（使用 IAM 页）查询读取行，当其他事务导致页拆分时，可能会缺失行。 当使用已提交的读取时不会发生这种情况，因为在页拆分期间将会保持表锁；当表没有聚集索引时也不会发生这种情况，因为更新不会导致页拆分。  
  
#### <a name="types-of-concurrency"></a>并发类型  
 当许多人试图同时修改数据库中的数据时，必须实现一个控制系统，使一个人所做的修改不会对他人所做的修改产生负面影响。 这称为并发控制。  
  
 并发控制理论根据建立并发控制的方法而分为两类：  
  
-   悲观并发控制  
  
     一个锁定系统，可以阻止用户以影响其他用户的方式修改数据。 如果用户执行的操作导致应用了某个锁，只有这个锁的所有者释放该锁，其他用户才能执行与该锁冲突的操作。 这种方法之所以称为悲观并发控制，是因为它主要用于数据争用激烈的环境中，以及发生并发冲突时用锁保护数据的成本低于回滚事务的成本的环境中。  
  
-   乐观并发控制  
  
     在乐观并发控制中，用户读取数据时不锁定数据。 当一个用户更新数据时，系统将进行检查，查看该用户读取数据后其他用户是否又更改了该数据。 如果其他用户更新了数据，将产生一个错误。 一般情况下，收到错误信息的用户将回滚事务并重新开始。 这种方法之所以称为乐观并发控制，是由于它主要在以下环境中使用：数据争用不大且偶尔回滚事务的成本低于读取数据时锁定数据的成本。  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持一定范围的并发控制。 用户通过为游标上的连接或并发选项选择事务隔离级别来指定并发控制的类型。 这些特性可以使用 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句或通过数据库应用程序编程接口（API，如 ADO、ADO.NET、OLE DB 和 ODBC）的属性和特性来定义。  
  
#### <a name="isolation-levels-in-the-ssdenoversion"></a>[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 中的隔离级别  
 事务指定一个隔离级别，该隔离级别定义一个事务必须与由其他事务进行的资源或数据更改相隔离的程度。 隔离级别从允许的并发副作用（例如，脏读或虚拟读取）的角度进行描述。  
  
 事务隔离级别控制：  
  
-   读取数据时是否占用锁以及所请求的锁类型。  
-   占用读取锁的时间。  
-   引用其他事务修改的行的读取操作是否：  
    -   在该行上的排他锁被释放之前阻塞其他事务。  
    -   检索在启动语句或事务时存在的行的已提交版本。  
    -   读取未提交的数据修改。  
  
> [!IMPORTANT]  
> 选择事务隔离级别不影响为保护数据修改而获取的锁。 事务总是在其修改的任何数据上获取排他锁并在事务完成之前持有该锁，不管为该事务设置了什么样的隔离级别。 对于读取操作，事务隔离级别主要定义保护级别，以防受到其他事务所做更改的影响。  
  
 较低的隔离级别可以增强许多用户同时访问数据的能力，但也增加了用户可能遇到的并发副作用（例如脏读或丢失更新）的数量。 相反，较高的隔离级别减少了用户可能遇到的并发副作用的类型，但需要更多的系统资源，并增加了一个事务阻塞其他事务的可能性。 应平衡应用程序的数据完整性要求与每个隔离级别的开销，在此基础上选择相应的隔离级别。 最高隔离级别（可序列化）保证事务在每次重复读取操作时都能准确检索到相同的数据，但需要通过执行某种级别的锁定来完成此操作，而锁定可能会影响多用户系统中的其他用户。 最低隔离级别（未提交读）可以检索其他事务已经修改、但未提交的数据。 在未提交读中，所有并发副作用都可能发生，但因为没有读取锁定或版本控制，所以开销最少。  
  
##### <a name="database-engine-isolation-levels"></a>数据库引擎隔离级别  
 ISO 标准定义了下列隔离级别，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]支持所有这些隔离级别：  
  
|隔离级别|定义|  
|---------------------|----------------|  
|**未提交的读取**|隔离事务的最低级别，只能保证不读取物理上损坏的数据。 在此级别上，允许脏读，因此一个事务可能看见其他事务所做的尚未提交的更改。|  
|**已提交的读取**|允许事务读取另一个事务以前读取（未修改）的数据，而不必等待第一个事务完成。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]保留写锁（在所选数据上获取）直到事务结束，但是一执行 SELECT 操作就释放读锁。 这是[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]默认级别。|  
|**可重复的读取**|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]保留在所选数据上获取的读锁和写锁，直到事务结束。 但是，因为不管理范围锁，可能发生虚拟读取。|  
|**可序列化**|隔离事务的最高级别，事务之间完全隔离。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]保留在所选数据上获取的读锁和写锁，在事务结束时释放它们。 SELECT 操作使用分范围的 WHERE 子句时获取范围锁，主要为了避免虚拟读取。<br /><br /> **注意：** 请求可序列化隔离级别时，复制的表上的 DDL 操作和事务可能失败。 这是因为复制查询使用的提示可能与可序列化隔离级别不兼容。|  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 还支持使用行版本控制的其他两个事务隔离级别。 一个是已提交读隔离的实现，另一个是事务隔离级别（快照）。  
  
|行版本控制隔离级别|定义|  
|------------------------------------|----------------|  
|**读取已提交的快照 (RCSI)**|当 READ_COMMITTED_SNAPSHOT 数据库选项设置为 ON 时，已提交读隔离使用行版本控制提供语句级读取一致性。 读取操作只需要 SCH-S 表级别的锁，不需要页锁或行锁。 即，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 使用行版本控制为每个语句提供一个在事务上一致的数据快照，因为该数据在语句开始时就存在。 不使用锁来防止其他事务更新数据。 用户定义的函数可以返回在包含 UDF 的语句开始后提交的数据。<br /><br /> 如果 `READ_COMMITTED_SNAPSHOT` 数据库选项设置为 OFF（这是默认设置），当前事务运行读取操作时，已提交读隔离使用共享锁来防止其他事务修改行。 共享锁还会阻止语句在其他事务完成之前读取由这些事务修改的行。 两个实现都满足已提交读隔离的 ISO 定义。|  
|**快照**|快照隔离级别使用行版本控制来提供事务级别的读取一致性。 读取操作不获取页锁或行锁，只获取 SCH-S 表锁。 读取其他事务修改的行时，读取操作将检索启动事务时存在的行的版本。 当 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项设置为 ON 时，只能对数据库使用快照隔离。 默认情况下，用户数据库的此选项设置为 OFF。<br /><br /> 注意：[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不支持元数据的版本控制。 因此，对于在快照隔离下运行的显式事务中可以执行的 DDL 操作存在限制。 在快照隔离下，以下 DDL 语句不允许出现在 BEGIN TRANSACTION 语句后：ALTER TABLE、CREATE INDEX、CREATE XML INDEX、ALTER INDEX、DROP INDEX、DBCC REINDEX、ALTER PARTITION FUNCTION、ALTER PARTITION SCHEME 或任何常用语言运行时(CLR) DDL 语句。 在隐式事务中使用快照隔离时，允许使用这些语句。 根据定义，隐式事务为单个语句，这使得它可以强制应用快照隔离的语义，即便使用 DDL 语句也是如此。 违反此原则会导致错误 3961: `Snapshot isolation transaction failed in database '%.*ls' because the object accessed by the statement has been modified by a DDL statement in another concurrent transaction since the start of this transaction. It is not allowed because the metadata is not versioned. A concurrent update to metadata could lead to inconsistency if mixed with snapshot isolation.`|  
  
 下表显示了不同隔离级别导致的并发副作用。  
  
|隔离级别|脏读|不可重复读|虚拟读取|  
|---------------------|----------------|------------------------|-------------|  
|**未提交的读取**|是|是|是|  
|**已提交的读取**|否|是|是|  
|**可重复的读取**|否|否|是|  
|**快照**|否|否|否|  
|**可序列化**|否|否|否|  
  
 有关每个事务隔离级别控制的特定类型的锁定或行版本控制的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。  
  
 可以使用 [!INCLUDE[tsql](../includes/tsql-md.md)] 或通过数据库 API 来设置事务隔离级别。  
  
 **[!INCLUDE[tsql](../includes/tsql-md.md)]**                                      
 [!INCLUDE[tsql](../includes/tsql-md.md)] 脚本使用 `SET TRANSACTION ISOLATION LEVEL` 语句。  
  
 **ADO**  
 ADO 应用程序将 Connection 对象的 `IsolationLevel` 属性设置为 adXactReadUncommitted、adXactReadCommitted、adXactRepeatableRead 或 adXactReadSerializable。  
  
 **ADO.NET**  
 使用 `System.Data.SqlClient` 管理命名空间的 ADO.NET 应用程序可以调用 `SqlConnection.BeginTransaction` 方法，并将 IsolationLevel 选项设置为 Unspecified、Chaos、ReadUncommitted、ReadCommitted、RepeatableRead、Serializable 和 Snapshot。  
  
 **OLE DB**  
 开始事务时，使用 OLE DB 的应用程序调用 `ITransactionLocal::StartTransaction`，其中 isoLevel 设置为 ISOLATIONLEVEL_READUNCOMMITTED、ISOLATIONLEVEL_READCOMMITTED、ISOLATIONLEVEL_REPEATABLEREAD、ISOLATIONLEVEL_SNAPSHOT 或 ISOLATIONLEVEL_SERIALIZABLE。  
  
 在自动提交模式下指定事务隔离级别时，OLE DB 应用程序可以将 DBPROPSET_SESSION 属性 DBPROP_SESS_AUTOCOMMITISOLEVELS 设置为 DBPROPVAL_TI_CHAOS、DBPROPVAL_TI_READUNCOMMITTED、DBPROPVAL_TI_BROWSE、DBPROPVAL_TI_CURSORSTABILITY、DBPROPVAL_TI_READCOMMITTED、DBPROPVAL_TI_REPEATABLEREAD、DBPROPVAL_TI_SERIALIZABLE、DBPROPVAL_TI_ISOLATED 或 DBPROPVAL_TI_SNAPSHOT。  
  
 **ODBC**  
 ODBC 应用程序调用 `SQLSetConnectAttr`，其中 Attribute 设置为 SQL_ATTR_TXN_ISOLATION，ValuePtr 设置为 SQL_TXN_READ_UNCOMMITTED、SQL_TXN_READ_COMMITTED、SQL_TXN_REPEATABLE_READ 或 SQL_TXN_SERIALIZABLE 。  
  
 对于快照事务，应用程序调用 `SQLSetConnectAttr`，其中 Attribute 设置为 SQL_COPT_SS_TXN_ISOLATION，ValuePtr 设置为 SQL_TXN_SS_SNAPSHOT。 可以使用 SQL_COPT_SS_TXN_ISOLATION 或 SQL_ATTR_TXN_ISOLATION 检索快照事务。  
  
##  <a name="locking-in-the-database-engine"></a><a name="Lock_Engine"></a> 数据库引擎中的锁定  
 锁定是 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]用来同步多个用户同时对同一个数据块的访问的一种机制。  
  
 在事务获取数据块当前状态的依赖关系（比如通过读取或修改数据）之前，它必须保护自己不受其他事务对同一数据进行修改的影响。 事务通过请求锁定数据块来达到此目的。 锁有多种模式，如共享或排他。 锁模式定义了事务对数据所拥有的依赖关系级别。 如果某个事务已获得特定数据的锁，则其他事务不能获得会与该锁模式发生冲突的锁。 如果事务请求的锁模式与已授予同一数据的锁发生冲突，则[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例将暂停事务请求直到第一个锁释放。  
  
 当事务修改某个数据块时，它将持有保护所做修改的锁直到事务结束。 事务持有（所获取的用来保护读取操作的）锁的时间长度，取决于事务隔离级别设置。 一个事务持有的所有锁都在事务完成（无论是提交还是回滚）时释放。  
  
 应用程序一般不直接请求锁。 锁由[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]的一个部件（称为“锁管理器”）在内部管理。 当[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例处理 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]查询处理器会决定将要访问哪些资源。 查询处理器根据访问类型和事务隔离级别设置来确定保护每一资源所需的锁的类型。 然后，查询处理器将向锁管理器请求适当的锁。 如果与其他事务所持有的锁不会发生冲突，锁管理器将授予该锁。  
  
## <a name="lock-granularity-and-hierarchies"></a>锁粒度和层次结构  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]具有多粒度锁定，允许一个事务锁定不同类型的资源。 为了尽量减少锁定的开销，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]自动将资源锁定在适合任务的级别。 锁定在较小的粒度（例如行）可以提高并发度，但开销较高，因为如果锁定了许多行，则需要持有更多的锁。 锁定在较大的粒度（例如表）会降低了并发度，因为锁定整个表限制了其他事务对表中任意部分的访问。 但其开销较低，因为需要维护的锁较少。  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]通常必须获取多粒度级别上的锁才能完整地保护资源。 这组多粒度级别上的锁称为锁层次结构。 例如，为了完整地保护对索引的读取，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例可能必须获取行上的共享锁以及页和表上的意向共享锁。  
  
 下表列出了[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]可以锁定的资源。  
  
|资源|说明|  
|--------------|-----------------|  
|RID|用于锁定堆中的单个行的行标识符。|  
|KEY|索引中用于保护可序列化事务中的键范围的行锁。|  
|PAGE|数据库中的 8 KB 页，例如数据页或索引页。|  
|EXTENT|一组连续的八页，例如数据页或索引页。|  
|HoBT|堆或 B 树。 用于保护没有聚集索引的表中的 B 树（索引）或堆数据页的锁。|  
|TABLE|包括所有数据和索引的整个表。|  
|FILE|数据库文件。|  
|APPLICATION|应用程序专用的资源。|  
|METADATA|元数据锁。|  
|ALLOCATION_UNIT|分配单元。|  
|DATABASE|整个数据库。|  
  
> [!NOTE]  
> 使用 [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md) 的 LOCK_ESCALATION 选项会对 HoBT 和 TABLE 锁带来影响。  
  
## <a name="lock-modes"></a><a name="lock_modes"></a>锁模式  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用不同的锁模式锁定资源，这些锁模式确定了并发事务访问资源的方式。  
  
 下表显示了[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用的资源锁模式。  
  
|锁模式|说明|  
|---------------|-----------------|  
|**共享 (S)**|用于不更改或不更新数据的读取操作，如 SELECT 语句。|  
|**更新 (U)**|用于可更新的资源中。 防止当多个会话在读取、锁定以及随后可能进行的资源更新时发生常见形式的死锁。|  
|**排他 (X)**|用于数据修改操作，例如 INSERT、UPDATE 或 DELETE。 确保不会同时对同一资源进行多重更新。|  
|**意向**|用于建立锁的层次结构。 意向锁包含三种类型：意向共享 (IS)、意向排他 (IX) 和意向排他共享 (SIX)。|  
|**架构**|在执行依赖于表架构的操作时使用。 架构锁包含两种类型：架构修改 (Sch-M) 和架构稳定性 (Sch-S)。|  
|**大容量更新 (BU)**|在将数据大容量复制到表中且指定了 TABLOCK 提示时使用。|  
|**键范围**|当使用可序列化事务隔离级别时保护查询读取的行的范围。 确保再次运行查询时其他事务无法插入符合可序列化事务的查询的行。|  
  
### <a name="shared-locks"></a><a name="shared"></a>共享锁  
 共享锁（S 锁）允许并发事务在封闭式并发控制下读取 (SELECT) 资源。 资源上存在共享锁（S 锁）时，任何其他事务都不能修改数据。 读取操作一完成，就立即释放资源上的共享锁（S 锁），除非将事务隔离级别设置为可重复读或更高级别，或者在事务持续时间内用锁定提示保留共享锁（S 锁）。  
  
### <a name="update-locks"></a><a name="update"></a>更新锁  
 更新锁（U 锁）可以防止常见的死锁。 在可重复读或可序列化事务中，此事务读取数据 [获取资源（页或行）的共享锁（S 锁）]，然后修改数据 [此操作要求锁转换为排他锁（X 锁）]。 如果两个事务获得了资源上的共享模式锁，然后试图同时更新数据，则一个事务尝试将锁转换为排他锁（X 锁）。 共享模式到排他锁的转换必须等待一段时间，因为一个事务的排他锁与其他事务的共享模式锁不兼容；发生锁等待。 第二个事务试图获取排他锁（X 锁）以进行更新。 由于两个事务都要转换为排他锁（X 锁），并且每个事务都等待另一个事务释放共享模式锁，因此发生死锁。  
  
 若要避免这种潜在的死锁问题，请使用更新锁（U 锁）。 一次只有一个事务可以获得资源的更新锁（U 锁）。 如果事务修改资源，则更新锁（U 锁）转换为排他锁（X 锁）。  
  
### <a name="exclusive-locks"></a><a name="exclusive"></a>排他锁  
 排他锁（X 锁）可以防止并发事务对资源进行访问。 使用排他锁（X 锁）时，任何其他事务都无法修改数据；仅在使用 NOLOCK 提示或未提交读隔离级别时才会进行读取操作。  
  
 数据修改语句（如 INSERT、UPDATE 和 DELETE）合并了修改和读取操作。 语句在执行所需的修改操作之前首先执行读取操作以获取数据。 因此，数据修改语句通常请求共享锁和排他锁。 例如，UPDATE 语句可能根据与一个表的联接修改另一个表中的行。 在此情况下，除了请求更新行上的排他锁之外，UPDATE 语句还将请求在联接表中读取的行上的共享锁。  
  
### <a name="intent-locks"></a><a name="intent"></a>意向锁  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用意向锁来保护共享锁（S 锁）或排他锁（X 锁）放置在锁层次结构的底层资源上。 意向锁之所以命名为意向锁，是因为在较低级别锁前可获取它们，因此会通知意向将锁放置在较低级别上。  
  
 意向锁有两种用途：  
  
-   防止其他事务以会使较低级别的锁无效的方式修改较高级别资源。 
-   提高[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]在较高的粒度级别检测锁冲突的效率。  
  
 例如，在该表的页或行上请求共享锁（S 锁）之前，在表级请求共享意向锁。 在表级设置意向锁可防止另一个事务随后在包含那一页的表上获取排他锁（X 锁）。 意向锁可以提高性能，因为[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]仅在表级检查意向锁来确定事务是否可以安全地获取该表上的锁。 而不需要检查表中的每行或每页上的锁以确定事务是否可以锁定整个表。  
  
<a name="lock_intent_table"></a>意向锁包括意向共享 (IS)、意向排他 (IX) 以及意向排他共享 (SIX)。  
  
|锁模式|说明|  
|---------------|-----------------|  
|**意向共享 (IS)**|保护针对层次结构中某些（而并非所有）低层资源请求或获取的共享锁。|  
|**意向排他 (IX)**|保护针对层次结构中某些（而并非所有）低层资源请求或获取的排他锁。 IX 是 IS 的超集，它也保护针对低层级别资源请求的共享锁。|  
|**意向排他共享 (SIX)**|保护针对层次结构中某些（而并非所有）低层资源请求或获取的共享锁以及针对某些（而并非所有）低层资源请求或获取的意向排他锁。 顶级资源允许使用并发 IS 锁。 例如，获取表上的 SIX 锁也将获取正在修改的页上的意向排他锁以及修改的行上的排他锁。 虽然每个资源在一段时间内只能有一个 SIX 锁，以防止其他事务对资源进行更新，但是其他事务可以通过获取表级的 IS 锁来读取层次结构中的低层资源。|  
|**意向更新 (IU)**|保护针对层次结构中所有低层资源请求或获取的更新锁。 仅在页资源上使用 IU 锁。 如果进行了更新操作，IU 锁将转换为 IX 锁。|  
|**共享意向更新 (SIU)**|S 锁和 IU 锁的组合，作为分别获取这些锁并且同时持有两种锁的结果。 例如，事务执行带有 PAGLOCK 提示的查询，然后执行更新操作。 带有 PAGLOCK 提示的查询将获取 S 锁，更新操作将获取 IU 锁。|  
|**更新意向排他 (UIX)**|U 锁和 IX 锁的组合，作为分别获取这些锁并且同时持有两种锁的结果。|  
  
### <a name="schema-locks"></a><a name="schema"></a>架构锁  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]在表数据定义语言 (DDL) 操作（例如添加列或删除表）的过程中使用架构修改 (Sch-M) 锁。 保持该锁期间，Sch-M 锁将阻止对表进行并发访问。 这意味着 Sch-M 锁在释放前将阻止所有外围操作。  
  
 某些数据操作语言 (DML) 操作（例如表截断）使用 Sch-M 锁阻止并发操作访问受影响的表。  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]在编译和执行查询时使用架构稳定性 (Sch-S) 锁。 Sch-S 锁不会阻止某些事务锁，其中包括排他 (X) 锁。 因此，在编译查询的过程中，其他事务（包括那些针对表使用 X 锁的事务）将继续运行。 但是，无法针对表执行获取 Sch-M 锁的并发 DDL 操作和并发 DML 操作。  
  
### <a name="bulk-update-locks"></a><a name="bulk_update"></a>大容量更新锁  
 大容量更新锁（BU 锁）允许多个线程将数据并发地大容量加载到同一表，同时防止其他不进行大容量加载数据的进程访问该表。 在满足以下两个条件时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用大容量更新 (BU) 锁。  
  
-   使用 [!INCLUDE[tsql](../includes/tsql-md.md)] BULK INSERT 语句或 OPENROWSET(BULK) 函数，或者使用某个大容量插入 API 命令（如 .NET SqlBulkCopy、OLEDB 快速加载 API 或 ODBC 大容量复制 API）将数据大容量复制到表。  
-   指定了 TABLOCK 提示或使用 sp_tableoption 设置 table lock on bulk load 表选项  。  
  
> [!TIP]  
> 与持有较少限制性大容量更新锁的 BULK INSERT 语句不同，具有 TABLOCK 提示的 INSERT INTO…SELECT 语句持有一个针对表的排他 (X) 锁。 也就是说您不能使用并行插入操作插入行。  
  
### <a name="key-range-locks"></a><a name="key_range"></a>键范围锁  
 在使用可序列化事务隔离级别时，对于 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句读取的记录集，键范围锁可以隐式保护该记录集中包含的行范围。 键范围锁可防止虚拟读取。 通过保护行之间键的范围，它还防止对事务访问的记录集进行虚拟插入或删除。  
  
## <a name="lock-compatibility"></a><a name="lock_compatibility"></a>锁兼容性  
 锁兼容性控制多个事务能否同时获取同一资源上的锁。 如果资源已被另一事务锁定，则仅当请求锁的模式与现有锁的模式相兼容时，才会授予新的锁请求。 如果请求锁的模式与现有锁的模式不兼容，则请求新锁的事务将等待释放现有锁或等待锁超时间隔过期。 例如，没有与排他锁兼容的锁模式。 如果具有排他锁（X 锁），则在释放排他锁（X 锁）之前，其他事务均无法获取该资源的任何类型（共享、更新或排他）的锁。 另一种情况是，如果共享锁（S 锁）已应用到资源，则即使第一个事务尚未完成，其他事务也可以获取该项的共享锁或更新锁（U 锁）。 但是，在释放共享锁之前，其他事务无法获取排他锁。  
  
<a name="lock_compat_table"></a>下表显示了最常见的锁模式的兼容性。  
  
||现有的授予模式||||||  
|------|---------------------------|------|------|------|------|------|  
|**请求的模式**|**IS**|**S**|**U**|**IX**|**SIX**|**X**|  
|**意向共享 (IS)**|是|是|是|是|是|否|  
|**共享 (S)**|是|是|是|否|否|否|  
|**更新 (U)**|是|是|否|否|否|否|  
|**意向排他 (IX)**|是|否|否|是|否|否|  
|**意向排他共享 (SIX)**|是|否|否|否|否|否|  
|**排他 (X)**|否|否|否|否|否|否|  
  
> [!NOTE]  
> 意向排他锁（IX 锁）与 IX 锁模式兼容，因为 IX 表示打算只更新部分行而不是所有行。 还允许其他事务尝试读取或更新部分行，只要这些行不是其他事务当前更新的行即可。 此外，如果两个事务尝试更新同一行，将在表级和页级上授予这两个事务 IX 锁。 但是，将在行级授予一个事务 X 锁。 另一个事务必须在该行级锁被删除前等待。  
  
<a name="lock_matrix"></a>使用下表可以确定 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中所有可用的锁模式的兼容性。  
  
 ![lock_conflicts](../relational-databases/media/LockConflictTable.png)  
  
## <a name="key-range-locking"></a>键范围锁定  
 在使用可序列化事务隔离级别时，对于 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句读取的记录集，键范围锁可以隐式保护该记录集中包含的行范围。 可序列化隔离级别要求每当在事务期间执行任一查询时，该查询都必须获取相同的行集。 键范围锁可防止其他事务插入其键值位于可序列化事务读取的键值范围内的新行，从而确保满足此要求。  
  
 键范围锁可防止虚拟读取。 通过保护行之间的键范围，它还可以防止对事务访问的记录集进行虚拟插入。  
  
 键范围锁放置在索引上，指定开始键值和结束键值。 此锁将阻止任何要插入、更新或删除任何带有该范围内的键值的行的尝试，因为这些操作会首先获取索引上的锁。 例如，可序列化事务可以发出 `SELECT` 语句，该语句读取键值与条件 `BETWEEN 'AAA' AND 'CZZ'` 匹配的所有行。 从“AAA”到“CZZ”范围内的键值上的键范围锁可阻止其他事务插入带有该范围内的键值（例如“ADG”、“BBD”或“CAL”）的行          。  
  
### <a name="key-range-lock-modes"></a><a name="key_range_modes"></a>键范围锁模式  
 键范围锁包括按范围-行格式指定的范围组件和行组件：  
  
-   范围表示保护两个连续索引项之间的范围的锁模式。  
-   行表示保护索引项的锁模式。  
-   模式表示使用的组合锁模式。 键范围锁模式由两部分组成。 第一部分表示用于锁定索引范围 (RangeT) 的锁类型，第二部分表示用于锁定特定键 (K) 的锁类型 。 这两部分用连字符 (-) 连接，例如 RangeT-K 。  
  
    |范围|行|“模式”|说明|  
    |-----------|---------|----------|-----------------|  
    |RangeS|S|RangeS-S|共享范围，共享资源锁；可序列化范围扫描。|  
    |RangeS|U|RangeS-U|共享范围，更新资源锁；可串行更新扫描。|  
    |RangeI|Null|RangeI-N|插入范围，空资源锁；用于在索引中插入新键之前测试范围。|  
    |RangeX|X|RangeX-X|排他范围，排他资源锁；用于更新范围中的键。|  
  
> [!NOTE]  
> 内部空锁模式与所有其他锁模式兼容。  
  
 键范围锁模式有一个兼容性矩阵，表示哪些锁与在重叠键和范围上获取的其他锁兼容。  
  
||现有的授予模式|||||||  
|------|---------------------------|------|------|------|------|------|------|  
|**请求的模式**|**S**|**U**|**X**|**RangeS-S**|**RangeS-U**|**RangeI-N**|**RangeX-X**|  
|**共享 (S)**|是|是|否|是|是|是|否|  
|**更新 (U)**|是|否|否|是|否|是|否|  
|**排他 (X)**|否|否|否|否|否|是|否|  
|**RangeS-S**|是|是|否|是|是|否|否|  
|**RangeS-U**|是|否|否|是|否|否|否|  
|**RangeI-N**|是|是|是|否|否|是|否|  
|**RangeX-X**|否|否|否|否|否|否|否|  
  
### <a name="conversion-locks"></a><a name="lock_conversion"></a>转换锁  
 当键范围锁与其他锁重叠时，将创建转换锁。  
  
|锁定 1|锁 2|转换锁|  
|------------|------------|---------------------|  
|S|RangeI-N|RangeI-S|  
|U|RangeI-N|RangeI-U|  
|X|RangeI-N|RangeI-X|  
|RangeI-N|RangeS-S|RangeX-S|  
|RangeI-N|RangeS-U|RangeX-U|  
  
 在不同的复杂环境下（有时是在运行并发进程时），可以在一小段时间内观察到转换锁。  
  
### <a name="serializable-range-scan-singleton-fetch-delete-and-insert"></a>可序列化范围扫描、单独提取、删除和插入  
 键范围锁定确保以下操作是可序列化的：  
  
-   范围扫描查询  
-   对不存在的行的单独提取  
-   删除操作  
-   插入操作  
  
 必须满足下列条件才能发生键范围锁定：  
  
-   事务隔离级别必须设置为 SERIALIZABLE。  
-   查询处理器必须使用索引来实现范围筛选谓词。 例如，SELECT 语句中的 WHERE 子句可以用以下谓词建立范围条件：ColumnX BETWEEN N **'** AAA **'** AND N **'** CZZ **'** 。 仅当 ColumnX 被索引键覆盖时，才能获取键范围锁。  
  
### <a name="examples"></a>示例  
 以下表和索引用作随后的键范围锁定示例的基础。  
  
 ![btree](../relational-databases/media/btree4.png)  
  
#### <a name="range-scan-query"></a>范围扫描查询  
 为了确保范围扫描查询是可序列化的，每次在同一事务中执行的相同查询应返回同样的结果。 其他事务不能在范围扫描查询中插入新行；否则这些插入将成为虚拟插入。 例如，以下查询将使用上图中的表和索引：  
  
```sql  
SELECT name  
FROM mytable  
WHERE name BETWEEN 'A' AND 'C';  
```  
  
 键范围锁放置在与数据行范围（名称在值 `Adam` 与 `Dale` 之间的行）对应的索引项上，以防止添加或删除满足上述查询条件的新行。 尽管此范围中的第一个名称是 `Adam`，但是此索引项的 RangeS-S 模式键范围锁确保了以字母 A 开头的新名称（例如 `Abigail`）不能添加在 `Adam` 之前。 同样，`Dale` 索引项的 RangeS-S 键范围锁确保了以字母 C 开头的新名称（例如 `Clive`）不能添加在 `Carlos` 之后。  
  
> [!NOTE]  
> 包含的 RangeS-S 锁数量为 n+1，此处 n 是满足查询条件的行数 。  
  
#### <a name="singleton-fetch-of-nonexistent-data"></a>对不存在的数据的单独提取  
 如果事务中的查询试图选择不存在的行，则以后在相同的事务中发出这一查询时，必须返回相同的结果。 不允许其他事务插入不存在的行。 例如，对于下面的查询：  
  
```sql  
SELECT name  
FROM mytable  
WHERE name = 'Bill';  
```  
  
 键范围锁放置在与从 `Ben` 到 `Bing` 的名称范围对应的索引项上，因为名称 `Bill` 将插入到这两个相邻的索引项之间。 RangeS-S 模式键范围锁放置在索引项 `Bing` 上。 这样可阻止其他任何事务在索引项 `Bill` 与 `Ben` 之间插入值（例如 `Bing`）。  
  
#### <a name="delete-operation"></a>删除操作  
 在事务中删除值时，在事务执行删除操作期间不必锁定该值所属的范围。 锁定删除的键值直至事务结束足以保持可序列化性。 例如，对于下面的 DELETE 语句：  
  
```sql  
DELETE mytable  
WHERE name = 'Bob';  
```  
  
 排他锁（X 锁）放置在与名称 `Bob` 对应的索引项上。 其他事务可以在删除的值 `Bob` 的前后插入或删除值。 但是任何试图读取、插入或删除值 `Bob` 的事务都将被阻塞，直到删除的事务提交或回滚为止。  
  
 可以使用三个基本锁模式执行范围删除：行锁、页锁或表锁。 行、页或表锁定策略由查询优化器确定，也可以由用户通过查询优化器提示（例如 ROWLOCK、PAGLOCK 或 TABLOCK）来指定。 当使用 PAGLOCK 或 TABLOCK 时，如果从某个索引页中删除所有的行，则[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]将立即释放该索引页。 相反，当使用 ROWLOCK 时，所有删除的行只是标记为已删除；以后通过后台任务从索引页中删除它们。  
  
#### <a name="insert-operation"></a>插入操作  
 在事务中插入值时，在事务执行插入操作期间不必锁定该值所属的范围。 锁定插入的键值直至事务结束足以维护可序列化性。 例如，对于下面的 INSERT 语句：  
  
```sql  
INSERT mytable VALUES ('Dan');  
```  
  
 RangeI-N 模式键范围锁放置在与名称 David 对应的索引项上，以测试范围。 如果已授权锁，则插入 `Dan`，并且排他锁（X 锁）将放置在值 `Dan` 上。 RangeI-N 模式键范围锁仅对测试范围是必需的，而不在执行插入操作的事务期间保留。 其他事务可以在插入的值 `Dan` 的前后插入或删除值。 但是，任何试图读取、插入或删除值 `Dan` 的事务都将被阻塞，直到插入的事务提交或回滚为止。  
  
## <a name="lock-escalation"></a>锁升级
锁升级是将许多较细粒度的锁转换成数量更少的较粗粒度的锁的过程，这样可以减少系统开销，但却增加了并发争用的可能性。

当 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 获取低级别的锁时，它还将在包含更低级别对象的对象上放置意向锁：

-   当锁定行或索引键范围时，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 将在包含这些行或键的页上放置意向锁。
-   当锁定页时，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 将在包含这些页的更高级别对象上放置意向锁。 除了对象上的意向锁以外，以下对象上还需要意向页锁：
    -  非聚集索引的叶级页
    -  聚集索引的数据页
    -  堆数据页

[!INCLUDE[ssde_md](../includes/ssde_md.md)] 可以为同一语句执行行锁定和页锁定，以最大程度地减少锁的数量，并降低需要进行锁升级的可能性。 例如，数据库引擎可以在非聚集索引上放置页锁（如果在索引节点中选择了足够的连续键来满足查询），而在数据上放置行锁。

升级锁时，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 会尝试将表的意向锁改为对应的全锁，例如，将意向排他 (IX) 锁改为排他 (X) 锁，或将意向共享 (IS) 锁改为共享 (S) 锁。 如果锁升级尝试成功并获取全表锁，将释放事务在堆或索引上所持有的所有堆或 B 树锁、页锁（PAGE 锁）或行级锁（RID 锁）。 如果无法获取全锁，当时不会发生锁升级，而数据库引擎将继续获取行、键或页锁。

[!INCLUDE[ssde_md](../includes/ssde_md.md)] 不会将行锁或键范围锁升级到页锁，而是将它们直接升级到表锁。 同样，页锁始终升级到表锁。 对于关联的分区，已分区表的锁定可以升级到 HoBT 级别，而不是表锁。 HoBT 级锁不一定会锁定该分区的对齐 HoBT。

> [!NOTE]
> HoBT 级锁通常会增加并发情况，但是当锁定不同分区的每个事务都希望将其排他锁扩展到其他分区时，有可能会发生死锁。 在极少数情况下，TABLE 锁定粒度可能更适合。

如果由于并发事务所持有的锁冲突而导致锁升级尝试失败，则 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 将对事务获取的其他 1,250 个锁重试锁升级。

每个升级事件主要在单个 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句级别操作。 当事件启动时，只要活动语句满足升级阈值的要求，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 就会尝试升级当前事务在活动语句所引用的任何表中持有的所有锁。 如果升级事件在语句访问表之前启动，则不会尝试升级该表上的锁。 如果锁升级成功，只要表被当前语句引用并且包括在升级事件中，上一个语句中事务获取的、在事件启动时仍被持有的锁都将被升级。

例如，假定某个会话执行下列操作：

-  开始一个事务。
-  更新 `TableA`。 这将在 TableA 中生成排他行锁，直到事务完成后才会释放该锁。
-  更新 `TableB`。 这将在 TableB 中生成排他行锁，直到事务完成后才会释放该锁。
-  执行联接 `TableA` 与 `TableC` 的 SELECT。 查询执行计划要求先从 `TableA` 中检索行，然后才从 `TableC` 中检索行。
-  SELECT 语句在从 `TableA` 中检索行时（此时还没有访问 `TableC`）触发锁升级。

如果锁升级成功，只有会话在 `TableA` 中持有的锁才会升级。 这包括来自 SELECT 语句的共享锁和来自上一个 UPDATE 语句的排他锁。 由于决定是否应进行锁升级时只考虑会话的 SELECT 语句在 `TableA` 中获取的锁，所以一旦升级成功，会话在 `TableA` 中持有的所有锁都将被升级到该表的排他锁，而 `TableA` 的所有其他较低粒度的锁（包括意向锁）都将被释放。

不会尝试升级 `TableB` 的锁，因为 SELECT 语句中没有 `TableB` 的活动引用。 同样，也不会尝试升级 `TableC` 上尚未升级的锁，因为发生升级时尚未访问过该表。

### <a name="lock-escalation-thresholds"></a>锁升级阈值

如果没有使用 `ALTER TABLE SET LOCK_ESCALATION` 选项来禁用表的锁升级并且满足以下任一条件，则将触发锁升级：

-  单个 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句在单个未分区表或索引上获得至少 5,000 个锁。
-  单个 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句在已分区表的单个分区上获得至少 5,000 个锁，并且 `ALTER TABLE SET LOCK_ESCALATION` 选项设为 AUTO。
-  [!INCLUDE[ssde_md](../includes/ssde_md.md)] 实例中的锁数量超出了内存或配置阈值。

如果由于锁冲突导致无法升级锁，则每当获取 1,250 个新锁时 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 便会触发锁升级。

### <a name="escalation-threshold-for-a-transact-sql-statement"></a>Transact-SQL 语句的升级阈值
当 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 在每 1,250 个新获取的锁上检查可能的升级时，并且当且仅当 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句已在表的单个引用上获得至少 5,000 个锁时，才会进行锁升级。 当 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句在表的单个引用上获得至少 5,000 个锁时，将触发锁升级。 例如，如果该语句在一个索引上获取 3,000 个锁，在同一表中的另一个索引上获取 3,000 个锁，这种情况下不会触发锁升级。 同样，如果语句中含有表的自联接，并且表的每一个引用仅在表中获取 3,000 个锁，则不会触发锁升级。

只有触发升级时已经访问的表才会发生锁升级。 假定某个 SELECT 语句是一个按 `TableA`、`TableB` 和 `TableC` 顺序访问三个表的联接。 该语句在 `TableA` 的聚集索引中获取 3,000 个行锁，在 `TableB` 的聚集索引中获取至少 5,000 个行锁，但是尚未访问 `TableC`。 当 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 检测到该语句在 `TableB` 中获取至少 5,000 个行锁时，会尝试升级当前事务在 `TableB` 中持有的所有锁。 它还会尝试升级当前事务在 `TableA` 中持有的所有锁，但是由于 `TableA` 中锁的数量小于 5,000，因此，升级无法成功。 但它不会尝试在 `TableC` 中进行锁升级，因为发生升级时尚未访问该表。

### <a name="escalation-threshold-for-an-instance-of-the-database-engine"></a>数据库引擎实例的升级阈值
每当锁的数量大于锁升级的内存阈值时，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 都会触发锁升级。 内存阈值取决于[锁配置选项](../database-engine/configure-windows/configure-the-locks-server-configuration-option.md)的设置：

-   如果“锁”选项设置为默认值 0，当锁对象使用的内存是数据库引擎所使用内存的 24%（不包括 AWE 内存）时，将达到锁升级阈值。 用于表示锁的数据结构大约有 100 个字节长。 该阈值是动态的，因为数据库引擎动态地获得和释放内存，以针对变化的工作负载进行调整。

-   如果“锁”选项设置为非 0 值，则锁升级阈值是“所”选项值的 40%（或者更低，如果存在内存压力）。

[!INCLUDE[ssde_md](../includes/ssde_md.md)] 可以为升级选择任何会话中的活动语句，而且，只要实例中使用的锁内存保持在阈值之上，每获取 1,250 个新锁，它就会为升级选择语句。

### <a name="escalating-mixed-lock-types"></a>升级混合锁类型
发生锁升级时，为堆或索引选择的锁必须足够强，才能满足限制性最强的较低级别的锁的要求。

例如，假定会话执行下列操作：

-  开始一个事务。
-  更新包含聚集索引的表。
-  发出引用同一个表的 SELECT 语句。

UPDATE 语句将获取下列锁：

-  已更新数据行上的排他锁（X 锁）。
-  包含那些行的聚集索引页上的意向排他锁（IX 锁）。
-  聚集索引上的 IX 锁和表上的 IX 锁。

SELECT 语句将获取下列锁：

-  所读取的所有数据行上的共享锁（S 锁），除非行已被来自 UPDATE 语句的 X 锁保护。
-  包含那些行的所有聚集索引页上的意向共享锁，除非页已被 IX 锁保护。
-  在聚集索引或表上不会获取锁，因为它们已被 IX 锁保护。

如果 SELECT 获取了触发锁升级的足够锁并且升级成功，表上的 IX 锁将被转换为 X 锁，而所有行、页和索引锁都将被释放。 更新和读取操作都受表上的 X 锁保护。

### <a name="reducing-locking-and-escalation"></a>减少锁定和升级
在大多数情况下，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 在使用默认的锁定和锁升级设置进行操作时提供最佳的性能。 如果 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 实例生成大量锁并且频繁进行锁升级，请考虑通过下列方法减少锁定数量：

-   对于读取操作，使用不会生成共享锁的隔离级别：
    -  当 READ_COMMITTED_SNAPSHOT 数据库选项为 ON 时，使用 READ COMMITTED 隔离级别。
    -  使用 SNAPSHOT 隔离级别。
    -  使用 READ UNCOMMITTED 隔离级别。 此隔离级别只能用于能对脏读进行操作的系统。    
  
    > [!NOTE]
    > 更改隔离级别会影响 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 实例上的所有表。

-   使用 PAGLOCK 或 TABLOCK 表提示，使数据库引擎使用页、堆或索引锁而不是行锁。 但是，使用此选项增加了用户阻止其他用户尝试访问相同数据的问题，对于并发用户较多的系统，不应使用此选项。

-   对于已分区表，使用 [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md) 的 LOCK_ESCALATION 选项将锁升级到 HoBT 级别而不是表级别，或者禁用锁升级。

-   将大批操作分成多个小批操作。 例如，假设你运行以下查询以从审核表中删除几十万条旧记录，然后发现它导致了会阻止其他用户的锁升级：
   
    ```sql
    DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    ```

    通过一次删除几百条记录，可以显著减少每个事务累积的锁数量，并防止锁升级。 例如：

    ```sql
    SET ROWCOUNT 500
    delete_more:
      DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    IF @@ROWCOUNT > 0 GOTO delete_more
    SET ROWCOUNT 0
    ```

-   通过尽量提高查询的效率，减少查询的锁占用时间。 大型扫描或大量的书签查找可能会增加锁升级的几率；此外，它还会增加死锁的可能性，并且通常会对并发和性能产生不利影响。 找到导致锁升级的查询后，寻找机会创建新索引，或向现有索引添加列以删除索引或表扫描，并最大程度地提高索引查找的效率。 请考虑使用[数据库引擎优化顾问](../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)对查询执行自动索引分析。 有关详细信息，请参阅[教程：数据库引擎优化顾问](../tools/dta/tutorial-database-engine-tuning-advisor.md)。
    此优化的一个目标是使索引查找尽可能少地返回行，以最大程度地降低书签查找的成本（最大程度地提高特定查询的索引选择性）。 如果 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 估计书签查找逻辑运算符可能返回多个行，则它可能使用 PREFETCH 来执行书签查找。 如果 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 使用 PREFETCH 进行书签查找，则它必须将部分查询的事务隔离级别提升至可重复读取部分查询。 这意味着，在已提交读隔离级别看起来类似于 SELECT 语句的内容可能会获得数千个键锁（在两个聚集索引和一个非聚集索引上），这会导致此类查询超出锁升级阈值。 如果你发现已升级的锁是共享表锁（但该锁在默认的已提交读隔离级别并不常见），则这一点特别重要。 如果书签查找 WITH PREFETCH 子句导致升级，请考虑向索引查找中出现的非聚集索引，或查询计划中书签查找逻辑运算符下方的索引扫描逻辑运算符添加其他列。 可以创建覆盖索引（一种索引，可包含查询所使用表中的全部列），也可以至少创建一个包含联接条件或 WHERE 子句所使用列的索引（如果无法在选择列列表中包含所有列）。
    嵌套循环联接也可能使用 PREFETCH，并且这会导致相同的锁定行为。
   
-   如果其他 SPID 当前持有不兼容的表锁，则不会发生锁升级。 锁升级始终升级至表锁，而不是页锁。 此外，如果由于另一个 SPID 持有不兼容的 TAB 锁而导致锁升级尝试失败，则尝试升级的查询在等待 TAB 锁时不会被阻止。 相反，它会继续在其原始、更细化的级别（行、键或页）获取锁，并定期进行其他升级尝试。 因此，阻止特定表发生锁升级的一种方法是获取并持有与已升级锁类型不兼容的其他连接的锁。 表级的 IX（意向排他）锁不会锁定任何行或页，但仍与已升级的 S（共享）或 X（排他）TAB 锁不兼容。 例如，假设你需要运行一个批处理作业，该作业会修改 mytable 表中的大量行，并且导致了由锁升级造成的阻止行为。 如果此作业始终在不到一个小时内完成，你可以创建一个包含以下代码的 [!INCLUDE[tsql](../includes/tsql-md.md)] 作业，并将此新作业计划为在批处理作业开始时间之前的几分钟启动：
  
    ```sql
    BEGIN TRAN
    SELECT * FROM mytable (UPDLOCK, HOLDLOCK) WHERE 1=0
    WAITFOR DELAY '1:00:00'
    COMMIT TRAN
    ```
   
    此查询将获取 mytable 表的 IX 锁，并持有该锁一个小时，这会阻止在此时间段内该表发生锁升级。 此批处理作业不会修改任何数据或阻止其他查询（除非另一个查询使用 TABLOCK 提示强制执行表锁，或者管理员已使用 sp_indexoption 存储过程禁用了页锁或行锁）。

还可以使用跟踪标志 1211 和 1224 来禁用所有或某些锁升级。 不过，这些[跟踪标志](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)会对整个 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 全局禁用所有锁升级。 锁升级在 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 中非常有用，它最大程度地提高了查询效率（该效率会因为获取和释放数千个锁产生的开销而降低）。 此外，锁升级还可以帮助最大程度地减少跟踪锁所需的内存。 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 可以为锁结构动态分配的内存是有限的，因此，如果禁用锁升级并且锁内存增长到足够大，则尝试为任何查询分配额外的锁可能会失败并出现以下错误：

```Error: 1204, Severity: 19, State: 1
The SQL Server cannot obtain a LOCK resource at this time. Rerun your statement when there are fewer active users or ask the system administrator to check the SQL Server lock and memory configuration.
```

> [!NOTE]
> 在出现[错误 1204](../relational-databases/errors-events/mssqlserver-1204-database-engine-error.md) 时，它将停止处理当前语句并导致活动事务回滚。 如果重启数据库服务，则回滚本身可能会阻止用户或导致较长的数据库恢复时间。

> [!NOTE]
> 使用锁提示（如 ROWLOCK）只会更改初始锁计划。 锁提示不会阻止锁升级。 

此外，使用 `lock_escalation` 扩展事件 (xEvent) 来监视锁升级，如以下示例中所示：

```sql
-- Session creates a histogram of the number of lock escalations per database 
CREATE EVENT SESSION [Track_lock_escalation] ON SERVER 
ADD EVENT sqlserver.lock_escalation(SET collect_database_name=(1),collect_statement=(1)
    ACTION(sqlserver.database_id,sqlserver.database_name,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,sqlserver.sql_text,sqlserver.username))
ADD TARGET package0.histogram(SET source=N'sqlserver.database_id')
GO
```

> [!IMPORTANT]
> 应使用 `lock_escalation` 扩展事件 (xEvent)，而不是 SQL 跟踪或 SQL 探查器中的 Lock:Escalation 事件类

## <a name="dynamic-locking"></a><a name="dynamic_locks"></a>动态锁定
 使用低级锁（如行锁）可以降低两个事务同时在相同数据块上请求锁的可能性，从而提高并发性。 使用低级锁还会增加锁的数量以及管理锁所需的资源。 使用高级表锁或页锁可以减少开销，但代价是降低了并发性。  
  
 ![锁定成本与并发成本](../relational-databases/media/lockcht.png) 
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 使用动态锁定策略确定最经济的锁。 执行查询时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]会根据架构和查询的特点自动决定最合适的锁。 例如，为了缩减锁定的开销，优化器可能在执行索引扫描时在索引中选择页级锁。  
  
 动态锁定具有下列优点：  
  
-   简化数据库管理。 数据库管理员不必调整锁升级阈值。  
-   提高性能。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]通过使用适合任务的锁使系统开销减至最小。  
-   应用程序开发人员可以集中精力进行开发。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]将自动调整锁定。  
  
 自 [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] 起，锁升级的行为已发生改变，其中引入了 `LOCK_ESCALATION` 选项。 有关详细信息，请参阅 [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md) 的 `LOCK_ESCALATION` 选项。 
   
## <a name="deadlocks"></a><a name="deadlocks"></a> 死锁数  
 在两个或多个任务中，如果每个任务锁定了其他任务试图锁定的资源，此时会造成这些任务永久阻塞，从而出现死锁。 例如：  
  
-   事务 A 获取了行 1 的共享锁。  
-   事务 B 获取了行 2 的共享锁。  
-   现在，事务 A 请求行 2 的排他锁，但在事务 B 完成并释放其对行 2 持有的共享锁之前被阻止。  
-   现在，事务 B 请求行 1 的排他锁，但在事务 A 完成并释放其对行 1 持有的共享锁之前被阻止。  
  
 事务 B 完成之后事务 A 才能完成，但是事务 B 由事务 A 阻塞。该条件也称为循环依赖关系：事务 A 依赖于事务 B，事务 B 通过对事务 A 的依赖关系关闭循环。  
  
 除非某个外部进程断开死锁，否则死锁中的两个事务都将无限期等待下去。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 死锁监视器定期检查陷入死锁的任务。 如果监视器检测到循环依赖关系，将选择其中一个任务作为牺牲品，然后终止其事务并提示错误。 这样，其他任务就可以完成其事务。 对于事务以错误终止的应用程序，它还可以重试该事务，但通常要等到与它一起陷入死锁的其他事务完成后执行。  
  
 死锁经常与正常阻塞混淆。 事务请求被其他事务锁定的资源的锁时，发出请求的事务一直等到该锁被释放。 默认情况下，除非设置了 LOCK_TIMEOUT，否则 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 事务不会超时。 因为发出请求的事务未执行任何操作来阻塞拥有锁的事务，所以该事务是被阻塞，而不是陷入了死锁。 最后，拥有锁的事务将完成并释放锁，然后发出请求底事务将获取锁并继续执行。  
  
> [!NOTE]
> 死锁有时称为抱死。  
  
 不只是关系数据库管理系统，任何多线程系统上都会发生死锁，并且对于数据库对象的锁之外的资源也会发生死锁。 例如，多线程操作系统中的一个线程要获取一个或多个资源（例如，内存块）。 如果要获取的资源当前为另一线程所拥有，则第一个线程可能必须等待拥有线程释放目标资源。 这就是说，对于该特定资源，等待线程依赖于拥有线程。 在[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例中，当获取非数据库资源（例如，内存或线程）时，会话会死锁。  
  
 ![关系图显示了事务死锁](../relational-databases/media/deadlock.png)  
  
 在示例中，对于 `Part` 表锁资源，事务 T1 依赖于事务 T2。 同样，对于 `Supplier` 表锁资源，事务 T2 依赖于事务 T1。 因为这些依赖关系形成了一个循环，所以在事务 T1 和事务 T2 之间存在死锁。  
  
 当表进行了分区并且 `ALTER TABLE` 的 `LOCK_ESCALATION` 设置设为 AUTO 时也会发生死锁。 当 `LOCK_ESCALATION` 设为 AUTO 时，通过允许 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 在 HoBT 级别而非 TABLE 级别锁定表分区会增加并发情况。 但是，当单独的事务在某个表中持有分区锁并希望在其他事务分区上的某处持有锁时，会导致发生死锁。 通过将 `LOCK_ESCALATION` 设置为 `TABLE` 可以避免这种类型的死锁，但此设置会因强制某个分区的大量更新以等待某个表锁而减少并发情况。  
  
### <a name="detecting-and-ending-deadlocks"></a>检测和结束死锁  
 在两个或多个任务中，如果每个任务锁定了其他任务试图锁定的资源，此时会造成这些任务永久阻塞，从而出现死锁。 下图清楚地显示了死锁状态，其中：  
  
-   任务 T1 具有资源 R1 的锁（通过从 R1 指向 T1 的箭头指示），并请求资源 R2 的锁（通过从 T1 指向 R2 的箭头指示）。  
-   任务 T2 具有资源 R2 的锁（通过从 R2 指向 T2 的箭头指示），并请求资源 R1 的锁（通过从 T2 指向 R1 的箭头指示）。  
-   因为这两个任务都需要有资源可用才能继续，而这两个资源又必须等到其中一个任务继续才会释放出来，所以陷入了死锁状态。  
  
 ![关系图显示了处于死锁状态的任务](../relational-databases/media/Task_Deadlock_State.png)  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]自动检测 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的死锁循环。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]选择一个会话作为死锁牺牲品，然后终止当前事务（出现错误）来打断死锁。  
  
#### <a name="resources-that-can-deadlock"></a><a name="deadlock_resources"></a>可以死锁的资源  
 每个用户会话可能有一个或多个代表它运行的任务，其中每个任务可能获取或等待获取各种资源。 以下类型的资源可能会造成阻塞，并最终导致死锁。  
  
-   **锁**。 等待获取资源（如对象、页、行、元数据和应用程序）的锁可能导致死锁。 例如，事务 T1 在行 r1 上有共享锁（S 锁）并等待获取行 r2 的排他锁（X 锁）。 事务 T2 在行 r2 上有共享锁（S 锁）并等待获取行 r1 的排他锁（X 锁）。 这将导致一个锁循环，其中，T1 和 T2 都等待对方释放已锁定的资源。  
  
-   **工作线程**。 排队等待可用工作线程的任务可能导致死锁。 如果排队等待的任务拥有阻塞所有工作线程的资源，则将导致死锁。 例如，会话 S1 启动事务并获取行 r1 的共享锁（S 锁）后，进入睡眠状态。 在所有可用工作线程上运行的活动会话正尝试获取行 r1 的排他锁（X 锁）。 因为会话 S1 无法获取工作线程，所以无法提交事务并释放行 r1 的锁。 这将导致死锁。  
  
-   **内存**。 当并发请求等待获得内存，而当前的可用内存无法满足其需要时，可能发生死锁。 例如，两个并发查询（Q1 和 Q2）作为用户定义函数执行，分别获取 10MB 和 20MB 的内存。 如果每个查询需要 30MB 而可用总内存为 20MB，则 Q1 和 Q2 必须等待对方释放内存，这将导致死锁。  
  
-   **并行查询与执行相关的资源**。 通常与交换端口关联的处理协调器、发生器或使用者线程至少包含一个不属于并行查询的进程时，可能会相互阻塞，从而导致死锁。 此外，当并行查询启动执行时，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将根据当前的工作负荷确定并行度或工作线程数。 如果系统工作负荷发生意外更改，例如，当新查询开始在服务器中运行或系统用完工作线程时，则可能发生死锁。  
  
-   **多重活动结果集 (MARS) 资源**。 这些资源用于控制在 MARS 下交叉执行多个活动请求。 有关详细信息，请参阅[使用多重活动结果集 (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)。  
  
    -   **用户资源**。 线程等待可能被用户应用程序控制的资源时，该资源将被视为外部资源或用户资源，并将按锁进行处理。  
  
    -   **会话互斥体**。 在一个会话中运行的任务是交叉的，意味着在某一给定时间只能在该会话中运行一个任务。 任务必须独占访问会话互斥体，才能运行。  
  
    -   **事务互斥体**。 在一个事务中运行的所有任务是交叉的，意味着在某一给定时间只能在该事务中运行一个任务。 任务必须独占访问事务互斥体，才能运行。  
  
     任务必须获取会话互斥体，才能在 MARS 下运行。 如果任务在事务下运行，则它必须获取事务互斥体。 这将确保在某一给定会话和给定事务中一次仅有一个任务处于活动状态。 获取所需互斥体后，任务就可以执行了。 任务完成或在请求过程中生成时，它将按获取的相反顺序先释放事务互斥体，然后释放会话互斥体。 但是，这些资源可能导致死锁。 在下面的代码示例中，两个任务（用户请求 U1 和用户请求 U2）在同一会话中运行。  
  
    ```  
    U1:    Rs1=Command1.Execute("insert sometable EXEC usp_someproc");  
    U2:    Rs2=Command2.Execute("select colA from sometable");  
    ```  
  
     用户请求 U1 执行的存储过程已获取会话互斥体。 如果执行该存储过程花费了很长时间，则[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]会认为存储过程正在等待用户的输入。 用户等待 U2 的结果集时，用户请求 U2 正在等待会话互斥体，U1 正在等待用户资源。 死锁状态的逻辑说明如下：  
  
 ![LogicFlowExamplec](../relational-databases/media/udb9_LogicFlowExamplec.png)  
  
### <a name="deadlock-detection"></a><a name="deadlock_detection"></a>死锁检测  
 上面列出的所有资源均参与[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]死锁检测方案。 死锁检测是由锁监视器线程执行的，该线程定期搜索[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例的所有任务。 以下几点说明了搜索进程：  
  
-   默认时间间隔为 5 秒。  
-   如果锁监视器线程查找死锁，根据死锁的频率，死锁检测时间间隔将从 5 秒开始减小，最小为 100 毫秒。  
-   如果锁监视器线程停止查找死锁，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]将两个搜索间的时间间隔增加到 5 秒。  
-   如果刚刚检测到死锁，则假定必须等待锁的下一个线程正进入死锁循环。 检测到死锁后，第一对锁等待将立即触发死锁搜索，而不是等待下一个死锁检测时间间隔。 例如，如果当前时间间隔为 5 秒且刚刚检测到死锁，则下一个锁等待将立即触发死锁检测器。 如果锁等待是死锁的一部分，则将会立即检测它，而不是在下一个搜索期间才检测。  
  
 通常，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]仅定期执行死锁检测。 因为系统中遇到的死锁数通常很少，定期死锁检测有助于减少系统中死锁检测的开销。  
  
 锁监视器对特定线程启动死锁搜索时，会标识线程正在等待的资源。 然后，锁监视器查找特定资源的所有者，并递归地继续执行对那些线程的死锁搜索，直到找到一个循环。 用这种方式标识的循环形成一个死锁。  
  
 检测到死锁后，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]通过选择其中一个线程作为死锁牺牲品来结束死锁。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]终止正为线程执行的当前批处理，回滚死锁牺牲品的事务并将 1205 错误返回到应用程序。 回滚死锁牺牲品的事务会释放事务持有的所有锁。 这将使其他线程的事务解锁，并继续运行。 1205 死锁牺牲品错误将有关死锁涉及的线程和资源的信息记录在错误日志中。  
  
 默认情况下，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]选择运行回滚开销最小的事务的会话作为死锁牺牲品。 此外，用户也可以使用 `SET DEADLOCK_PRIORITY` 语句指定死锁情况下会话的优先级。 可以将 DEADLOCK_PRIORITY 设置为 LOW、NORMAL 或 HIGH，也可以将其设置为范围（-10 到 10）间的任一整数值。 死锁优先级的默认设置为 NORMAL。 如果两个会话的死锁优先级不同，则会选择优先级较低的会话作为死锁牺牲品。 如果两个会话的死锁优先级相同，则会选择回滚开销最低的事务的会话作为死锁牺牲品。 如果死锁循环中会话的死锁优先级和开销都相同，则会随机选择死锁牺牲品。  
  
 使用 CLR 时，死锁监视器将自动检测托管过程中访问的同步资源（监视器、读取器/编写器锁和线程联接）的死锁。 但是，死锁是通过在已选为死锁牺牲品的过程中引发异常来解决的。 因此，请务必理解异常不会自动释放牺牲品当前拥有的资源；必须显式释放资源。 用于标识死锁牺牲品的异常与异常行为一样，也会被捕获和解除。  
  
### <a name="deadlock-information-tools"></a><a name="deadlock_tools"></a>死锁信息工具  
 若要查看死锁信息，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 提供了监视工具，分别为 SQL Profiler 中的 system\_health xEvent 会话、两个跟踪标志以及死锁图形事件。  

#### <a name="deadlock-extended-event"></a><a name="deadlock_xevent"></a> 死锁扩展事件
自 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 起，应使用 `xml_deadlock_report` 扩展事件 (xEvent)，而不使用 SQL 跟踪或 SQL 探查器中的死锁图事件类。

此外，自 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 起，当发生死锁时，systemhealth* **\_** _ 会话已捕获所有包含死锁图的 `xml_deadlock_report` xEvent。 由于 _system\_health* 会话默认情况下处于启用状态，因此不需要将单独的 xEvent 会话配置为捕获死锁信息。 

捕获的死锁图通常具有三个不同的节点：
-   **牺牲品列表**。 死锁牺牲品进程标识符。
-   **进程列表**。 死锁中涉及的全部进程的信息。
-   **资源列表**。 死锁中涉及的资源的信息。

如果记录 `xml_deadlock_report` xEvent，打开 system\_health 会话文件或环形缓冲区，[!INCLUDE[ssManStudio](../includes/ssManStudio-md.md)] 会显示死锁中涉及的任务和资源的图形描述，如以下示例所示： 

![xEvent 死锁图](../relational-databases/media/udb9_xEventDeadlockGraphc.png)

以下查询可以查看 system\_health 会话环形缓冲区捕获的所有死锁事件：

```sql
SELECT xdr.value('@timestamp', 'datetime') AS [Date],
    xdr.query('.') AS [Event_Data]
FROM (SELECT CAST([target_data] AS XML) AS Target_Data
            FROM sys.dm_xe_session_targets AS xt
            INNER JOIN sys.dm_xe_sessions AS xs ON xs.address = xt.event_session_address
            WHERE xs.name = N'system_health'
              AND xt.target_name = N'ring_buffer'
    ) AS XML_Data
CROSS APPLY Target_Data.nodes('RingBufferTarget/event[@name="xml_deadlock_report"]') AS XEventData(xdr)
ORDER BY [Date] DESC
```

[!INCLUDE[ssResult](../includes/ssresult-md.md)]

![system_health_xevent_query_result](../relational-databases/media/system_health_qry.png)

以下示例显示了单击以上结果的第一个链接之后的输出：

```xml
<event name="xml_deadlock_report" package="sqlserver" timestamp="2018-02-18T08:26:24.698Z">
  <data name="xml_report">
    <type name="xml" package="package0" />
    <value>
      <deadlock>
        <victim-list>
          <victimProcess id="process27b9b0b9848" />
        </victim-list>
        <process-list>
          <process id="process27b9b0b9848" taskpriority="0" logused="0" waitresource="KEY: 5:72057594214350848 (1a39e6095155)" waittime="1631" ownerId="11088595" transactionname="SELECT" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27b9f79fac0" lockMode="S" schedulerid="9" kpid="15336" status="suspended" spid="62" sbid="0" ecid="0" priority="0" trancount="0" lastbatchstarted="2018-02-18T00:26:22.893" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="7908" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088595" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p1" line="3" stmtstart="78" stmtend="180" sqlhandle="0x0300050020766505ca3e07008ba8000001000000000000000000000000000000000000000000000000000000">
SELECT c2, c3 FROM t1 WHERE c2 BETWEEN @p1 AND @p1+    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x020000006263ec01ebb919c335024a072a2699958d3fcce60000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p1 4
END
   </inputbuf>
          </process>
          <process id="process27b9ee33c28" taskpriority="0" logused="252" waitresource="KEY: 5:72057594214416384 (e5b3d7e750dd)" waittime="1631" ownerId="11088593" transactionname="UPDATE" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27ba15a4490" lockMode="X" schedulerid="6" kpid="5584" status="suspended" spid="58" sbid="0" ecid="0" priority="0" trancount="2" lastbatchstarted="2018-02-18T00:26:22.890" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="15316" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088593" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p2" line="3" stmtstart="76" stmtend="150" sqlhandle="0x03000500599a5906ce3e07008ba8000001000000000000000000000000000000000000000000000000000000">
UPDATE t1 SET c2 = c2+1 WHERE c1 = @p    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x02000000008fe521e5fb1099410048c5743ff7da04b2047b0000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p2 4
END
   </inputbuf>
          </process>
        </process-list>
        <resource-list>
          <keylock hobtid="72057594214350848" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="cidx" id="lock27b9dd26a00" mode="X" associatedObjectId="72057594214350848">
            <owner-list>
              <owner id="process27b9ee33c28" mode="X" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9b0b9848" mode="S" requestType="wait" />
            </waiter-list>
          </keylock>
          <keylock hobtid="72057594214416384" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="idx1" id="lock27afa392600" mode="S" associatedObjectId="72057594214416384">
            <owner-list>
              <owner id="process27b9b0b9848" mode="S" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9ee33c28" mode="X" requestType="wait" />
            </waiter-list>
          </keylock>
        </resource-list>
      </deadlock>
    </value>
  </data>
</event>
```

有关详细信息，请参阅 [使用 system_health 会话](../relational-databases/extended-events/use-the-system-health-session.md)

#### <a name="trace-flag-1204-and-trace-flag-1222"></a><a name="deadlock_traceflags"></a>跟踪标志 1204 和跟踪标志 1222  
 发生死锁时，跟踪标志 1204 和跟踪标志 1222 会返回在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 错误日志中捕获的信息。 跟踪标志 1204 会报告由死锁所涉及的每个节点设置格式的死锁信息。 跟踪标志 1222 会设置死锁信息的格式，顺序为先按进程，然后按资源。 可以同时启用这两个跟踪标志，以获取同一个死锁事件的两种表示形式。  

> [!IMPORTANT]
> 避免在导致死锁的工作负载密集型系统上使用跟踪标志 1204 和 1222。 使用这些跟踪标志可能会导致性能问题。 改用死锁扩展事件(#deadlock_xevent)。
  
 除了定义跟踪标志 1204 和 1222 的属性之外，下表还显示了它们之间的相似之处和不同之处。  
  
|properties|跟踪标志 1204 和跟踪标志 1222|仅跟踪标志 1204|仅跟踪标志 1222|  
|--------------|-----------------------------------------|--------------------------|--------------------------|  
|输出格式|在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 错误日志中捕获输出。|主要针对死锁所涉及的节点。 每个节点都有一个专用部分，并且最后一部分说明死锁牺牲品。|返回采用不符合 XML 架构定义 (XSD) 架构的类 XML 格式的信息。 该格式有三个主要部分。 第一部分声明死锁牺牲品； 第二部分说明死锁所涉及的每个进程； 第三部分说明与跟踪标志 1204 中的节点同义的资源。|  
|标识属性|**SPID:<x\> ECID:<x\>** 标识并行进程中的系统进程 ID 线程。 条目 `SPID:<x> ECID:0`（其中，<x\> 将替换为 SPID 值）表示主线程。 条目 `SPID:<x> ECID:<y>`（其中，<x\> 将替换为 SPID 值，<y\> 大于 0）表示具有相同 SPID 的子线程。<br /><br /> **BatchID**（对于跟踪标志 1222 为 sbid）。 标识代码执行从中请求锁或持有锁的批处理。 多个活动的结果集 (MARS) 禁用后，BatchID 值为 0。 MARS 启用后，活动批处理的值为 1 到 n。 如果会话中没有活动的批处理，则 BatchID 为 0。<br /><br /> **模式**。 指定线程所请求的、获得的或等待的特定资源的锁的类型。 模式可以为 IS（意向共享）、S（共享）、U（更新）、IX（意向排他）、SIX（意向排他共享）和 X（排他）。<br /><br /> **行编号**（对于跟踪标志 1222 为行）。 列出发生死锁时当前批处理中正在执行的语句的行数。<br /><br /> **Input Buf**（对于跟踪标志 1222 为 inputbuf）。 列出当前批处理中的所有语句。|**Node**。 表示死锁链中的项数。<br /><br /> **List**。 锁所有者可能属于以下列表：<br /><br /> **Grant List**。 枚举资源的当前所有者。<br /><br /> **Convert List**。 枚举尝试将其锁转换为较高级别的当前所有者。<br /><br /> **Wait List**。 枚举对资源的当前新锁请求。<br /><br /> **Statement Type**。 说明线程对其具有权限的 DML 语句的类型（SELECT、INSERT、UPDATE 或 DELETE）。<br /><br /> **Victim Resource Owner**。 指定 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 选择作为牺牲品来中断死锁循环的参与线程。 选定的线程和所有的现有子线程都将终止。<br /><br /> **Next Branch**。 表示死锁循环中涉及的两个或多个具有相同 SPID 的子线程。|**deadlock victim**。 表示选为死锁牺牲品的任务的物理内存地址（请参阅 [sys.dm_os_tasks (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)）。 如果任务为无法解析的死锁，则它可能为 0（零）。 不能选择正在回滚的任务作为死锁牺牲品。<br /><br /> **executionstack**。 表示发生死锁时正在执行的 [!INCLUDE[tsql](../includes/tsql-md.md)] 代码。<br /><br /> **Priority**。 表示死锁优先级。 在某些情况下，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]可能在短时间内改变死锁优先级以更好地实现并发。<br /><br /> **logused**。 任务使用的日志空间。<br /><br /> **owner id**。可控制请求的事务的 ID。<br /><br /> **status**。 任务的状态。 为下列值之一：<br /><br /> >> **pending**。 正在等待工作线程。<br /><br /> >> **runnable**。 可以运行，但正在等待量程。<br /><br /> >> **running**。 当前正在计划程序上运行。<br /><br /> >> **suspended**。 执行已挂起。<br /><br /> >> **done**。 任务已完成。<br /><br /> >> **spinloop**。 正在等待自旋锁释放。<br /><br /> **waitresource**。 任务需要的资源。<br /><br /> **waittime**。 等待资源的时间（毫秒）。<br /><br /> **schedulerid**。 与此任务关联的计划程序。 请参阅 [sys.dm_os_schedulers (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)。<br /><br /> **hostname**。 工作站的名称。<br /><br /> **isolationlevel**。 当前事务隔离级别。<br /><br /> **Xactid**。 可控制请求的事务的 ID。<br /><br /> **currentdb**。 数据库的 ID。<br /><br /> **lastbatchstarted**。 客户端进程上次启动批处理执行的时间。<br /><br /> **lastbatchcompleted**。 客户端进程上次完成批处理执行的时间。<br /><br /> **clientoption1 和 clientoption2**。 此客户端连接上的 Set 选项。 这是一个位掩码，包含有关 SET 语句（如 SET NOCOUNT 和 SET XACTABORT）通常控制的选项的信息。<br /><br /> **associatedObjectId**。 表示 HoBT（堆或 b 树）ID。|  
|资源属性|**RID**。 标识持有锁或请求锁的表中的单行。 RID 表示为 RID: db_id:file_id:page_no:row_no。 例如，`RID: 6:1:20789:0`。<br /><br /> **OBJECT**。 标识持有锁或请求锁的表。 OBJECT 表示为 OBJECT: db_id:object_id。 例如，`TAB: 6:2009058193`。<br /><br /> **KEY**。 标识持有锁或请求锁的索引中的键范围。 KEY 表示为 KEY: db_id:hobt_id（索引键哈希值）。 例如，`KEY: 6:72057594057457664 (350007a4d329)`。<br /><br /> **PAG**。 标识持有锁或请求锁的页资源。 PAG 表示为 PAG: db_id:file_id:page_no。 例如，`PAG: 6:1:20789`。<br /><br /> **EXT**。 标识区结构。 EXT 表示为 EXT: db_id:file_id:extent_no。 例如，`EXT: 6:1:9`。<br /><br /> **DB**。 标识数据库锁。 **DB 以下列方式之一表示：**<br /><br /> DB: db_id<br /><br /> DB: db_id[BULK-OP-DB]，这标识备份数据库持有的数据库锁。<br /><br /> DB: db_id[BULK-OP-LOG]，这标识此特定数据库的备份日志持有的锁。<br /><br /> **APP**。 标识应用程序资源持有的锁。 APP 表示为 APP: lock_resource。 例如，`APP: Formf370f478`。<br /><br /> **METADATA**。 表示死锁所涉及的元数据资源。 由于 METADATA 具有许多子资源，因此，返回的值取决于已发生死锁的子资源。 例如，METADATA.USER_TYPE 返回 `user_type_id =` <integer_value>。 有关 METADATA 资源和子资源的详细信息，请参阅 [sys.dm_tran_locks (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。<br /><br /> **HOBT**。 表示死锁所涉及的堆或 b 树。|此跟踪标志没有任何排他。|此跟踪标志没有任何排他。|  
  
##### <a name="trace-flag-1204-example"></a>跟踪标志 1204 示例  
 下面的示例显示启用跟踪标志 1204 时的输出。 在此示例中，节点 1 中的表为没有索引的堆，节点 2 中的表为具有非聚集索引的堆。 节点 2 中索引键在发生死锁时正在进行更新。  
  
```  
Deadlock encountered .... Printing deadlock information  
Wait-for graph  
  
Node:1  
  
RID: 6:1:20789:0               CleanCnt:3 Mode:X Flags: 0x2  
 Grant List 0:  
   Owner:0x0315D6A0 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:55 ECID:0 XactLockInfo: 0x04D9E27C  
   SPID: 55 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
BEGIN TRANSACTION  
   EXEC usp_p2  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x03A3DAD0   
     Mode: U SPID:54 BatchID:0 ECID:0 TaskProxy:(0x04976374) Value:0x315d200 Cost:(0/868)  
  
Node:2  
  
KEY: 6:72057594057457664 (350007a4d329) CleanCnt:2 Mode:X Flags: 0x0  
 Grant List 0:  
   Owner:0x0315D140 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:54 ECID:0 XactLockInfo: 0x03A3DAF4  
   SPID: 54 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
     BEGIN TRANSACTION  
       EXEC usp_p1  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
  
Victim Resource Owner:  
 ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
```  
  
##### <a name="trace-flag-1222-example"></a>跟踪标志 1222 示例  
 下面的示例显示启用跟踪标志 1222 时的输出。 在此示例中，一个表为没有索引的堆，另一个表为具有非聚集索引的堆。 在第二个表中，索引键在发生死锁时正在进行更新。  
  
```  
deadlock-list  
 deadlock victim=process689978  
  process-list  
   process id=process6891f8 taskpriority=0 logused=868   
   waitresource=RID: 6:1:20789:0 waittime=1359 ownerId=310444   
   transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:42.733 XDES=0x3a3dad0   
   lockMode=U schedulerid=1 kpid=1952 status=suspended spid=54   
   sbid=0 ecid=0 priority=0 transcount=2   
   lastbatchstarted=2005-09-05T11:22:42.733   
   lastbatchcompleted=2005-09-05T11:22:42.733   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310444 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p1 line=6 stmtstart=202   
     sqlhandle=0x0300060013e6446b027cbb00c69600000100000000000000  
     UPDATE T2 SET COL1 = 3 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600856aa70f503b8104000000000000000000000000  
     EXEC usp_p1       
    inputbuf  
      BEGIN TRANSACTION  
       EXEC usp_p1  
   process id=process689978 taskpriority=0 logused=380   
   waitresource=KEY: 6:72057594057457664 (350007a4d329)     
   waittime=5015 ownerId=310462 transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:44.077 XDES=0x4d9e258 lockMode=U   
   schedulerid=1 kpid=3024 status=suspended spid=55 sbid=0 ecid=0   
   priority=0 transcount=2 lastbatchstarted=2005-09-05T11:22:44.077   
   lastbatchcompleted=2005-09-05T11:22:44.077   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310462 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p2 line=6 stmtstart=200   
     sqlhandle=0x030006004c0a396c027cbb00c69600000100000000000000  
     UPDATE T1 SET COL1 = 4 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600d688e709b85f8904000000000000000000000000  
     EXEC usp_p2       
    inputbuf  
      BEGIN TRANSACTION  
        EXEC usp_p2      
  resource-list  
   ridlock fileid=1 pageid=20789 dbid=6 objectname=AdventureWorks2016.dbo.T2   
   id=lock3136940 mode=X associatedObjectId=72057594057392128  
    owner-list  
     owner id=process689978 mode=X  
    waiter-list  
     waiter id=process6891f8 mode=U requestType=wait  
   keylock hobtid=72057594057457664 dbid=6 objectname=AdventureWorks2016.dbo.T1   
   indexname=nci_T1_COL1 id=lock3136fc0 mode=X   
   associatedObjectId=72057594057457664  
    owner-list  
     owner id=process6891f8 mode=X  
    waiter-list  
     waiter id=process689978 mode=U requestType=wait  
```  
  
#### <a name="profiler-deadlock-graph-event"></a>事件探查器死锁图形事件  
这是 SQL Profiler 中表示死锁所涉及的任务和资源的图形描述的事件。 以下示例显示启用死锁图形事件时 SQL Profiler 的输出。  
  
 ![ProfilerDeadlockGraphc](../relational-databases/media/udb9_ProfilerDeadlockGraphc.png)  
  
有关死锁事件的详细信息，请参阅 [Lock:Deadlock 事件类](../relational-databases/event-classes/lock-deadlock-event-class.md)。

有关运行 SQL Profiler 死锁图形的详细信息，请参阅[保存死锁图形 (SQL Server Profiler)](../relational-databases/performance/save-deadlock-graphs-sql-server-profiler.md)。  
  
### <a name="handling-deadlocks"></a>处理死锁  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 实例选择某事务作为死锁牺牲品时，将终止当前批处理，回滚事务并将错误消息 1205 返回应用程序。  
  
 `Your transaction (process ID #52) was deadlocked on {lock | communication buffer | thread} resources with another process and has been chosen as the deadlock victim. Rerun your transaction.`  
  
 因为任何提交 [!INCLUDE[tsql](../includes/tsql-md.md)] 查询的应用程序可被选为死锁牺牲品，应用程序应具有可处理错误消息 1205 的错误处理程序。 如果应用程序不处理该错误，可以继续操作，但是不知道自己的事务已回滚而且可能出错。  
  
 通过实现捕获 1205 号错误消息的错误处理程序，使应用程序得以处理该死锁情况并采取补救措施（例如，可以自动重新提交陷入死锁中的查询）。 通过自动重新提交查询，用户不必知道发生了死锁。  
  
 应用程序在重新提交其查询前应短暂暂停。 这样会给死锁涉及的另一个事务一个机会来完成并释放构成死锁循环一部分的该事务的锁。 这将把重新提交的查询请求其锁时，死锁重新发生的可能性降到最低。  
  
### <a name="minimizing-deadlocks"></a><a name="deadlock_minimizing"></a>将死锁减至最少  
 尽管死锁不能完全避免，但遵守特定的编码惯例可以将发生死锁的机会降至最低。 将死锁减至最少可以增加事务的吞吐量并减少系统开销，因为只有很少的事务：  
  
-   回滚，撤消事务执行的所有工作。  
-   由于死锁时回滚而由应用程序重新提交。  
  
 下列方法有助于将死锁减至最少：  
  
-   按同一顺序访问对象。  
-   避免事务中的用户交互。  
-   保持事务简短并处于一个批处理中。  
-   使用较低的隔离级别。  
-   使用基于行版本控制的隔离级别。  
    -   将 `READ_COMMITTED_SNAPSHOT` 数据库选项设置为 ON，以使读取提交的事务可以使用行版本控制。  
    -   使用快照隔离。  
-   使用绑定连接。  
  
#### <a name="access-objects-in-the-same-order"></a>按同一顺序访问对象  
 如果所有并发事务按同一顺序访问对象，则发生死锁的可能性会降低。 例如，如果两个并发事务先获取 Supplier 表上的锁，然后获取 Part 表上的锁，则在其中一个事务完成之前，另一个事务将在 Supplier 表上被阻塞  。 当第一个事务提交或回滚之后，第二个事务将继续执行，这样就不会发生死锁。 将存储过程用于所有数据修改可以使对象的访问顺序标准化。  
  
 ![deadlock2](../relational-databases/media/dedlck2.png)  
  
#### <a name="avoid-user-interaction-in-transactions"></a>避免事务中的用户交互  
 避免编写包含用户交互的事务，因为没有用户干预的批处理的运行速度远快于用户必须手动响应查询时的速度（例如回复输入应用程序请求的参数的提示）。 例如，如果事务正在等待用户输入，而用户去吃午餐或甚至回家过周末了，则用户就耽误了事务的完成。 这将降低系统的吞吐量，因为事务持有的任何锁只有在事务提交或回滚后才会释放。 即使不出现死锁的情况，在占用资源的事务完成之前，访问同一资源的其他事务也会被阻塞。  
  
#### <a name="keep-transactions-short-and-in-one-batch"></a>保持事务简短并处于一个批处理中  
 在同一数据库中并发执行多个需要长时间运行的事务时通常会发生死锁。 事务的运行时间越长，它持有排他锁或更新锁的时间也就越长，从而会阻塞其他活动并可能导致死锁。  
  
 保持事务处于一个批处理中可以最小化事务中的网络通信往返量，减少完成事务和释放锁可能遭遇的延迟。  
  
#### <a name="use-a-lower-isolation-level"></a>使用较低的隔离级别  
 确定事务是否能在较低的隔离级别上运行。 实现已提交读允许事务读取另一个事务已读取（未修改）的数据，而不必等待第一个事务完成。 使用较低的隔离级别（例如已提交读）比使用较高的隔离级别（例如可序列化）持有共享锁的时间更短。 这样就减少了锁争用。  
  
#### <a name="use-a-row-versioning-based-isolation-level"></a>使用基于行版本控制的隔离级别  
 `READ_COMMITTED_SNAPSHOT` 数据库选项设置为 ON 时，在读取操作期间，已提交读隔离级别下运行的事务使用行版本控制而不是共享锁。  
  
> [!NOTE]  
> 某些应用程序依赖于已提交读隔离的锁定和阻塞行为。 对于这些应用程序，要启用此选项必须进行一些更改。  
  
 快照隔离也使用行版本控制，该级别在读操作期间不使用共享锁。 必须将 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项设置为 ON，事务才能在快照隔离下运行。  
  
 实现这些隔离级别可使得在读写操作之间发生死锁的可能性降至最低。  
  
#### <a name="use-bound-connections"></a>使用绑定连接  
 使用绑定连接，同一应用程序打开的两个或多个连接可以相互合作。 可以像主连接获取的锁那样持有次级连接获取的任何锁，反之亦然。 这样它们就不会互相阻塞。  
  
## <a name="lock-partitioning"></a><a name="lock_partitioning"></a>锁分区  
 对于大型计算机系统，在经常被引用的对象上放置的锁可能会变成性能瓶颈，因为获取和释放锁对内部锁资源造成了争用。 锁分区通过将单个锁资源拆分为多个锁资源而提高了锁性能。 此功能只适用于拥有 16 个或更多 CPU 的系统，它是自动启用的，而且无法禁用。 只有对象锁可以分区。拥有子类型的对象锁不能分区。 有关详细信息，请参阅 [sys.dm_tran_locks (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。  
  
### <a name="understanding-lock-partitioning"></a>了解锁分区  
 锁任务访问几个共享资源，其中两个通过锁分区进行优化：  
  
-   **调节锁**。 它控制对锁资源（例如行或表）的访问。  
  
     不进行锁分区，一个调节锁就得管理单个锁资源的所有锁请求。 在具有大量活动的系统上，在锁请求等待释放调节锁时会出现资源争用的现象。 在这种情况下，获取锁可能变成了一个瓶颈，并且可能会对性能造成负面影响。  
  
     为了减少对单个锁资源的争用，锁分区将单个锁资源拆分成多个锁资源，以便将负荷分布到多个调节锁上。  
  
-   **内存**。 它用于存储锁资源结构。  
  
     获取调节锁后，锁结构将存储在内存中，然后即可对其进行访问和可能的修改。 将锁访问分布到多个资源中有助于消除在 CPU 之间传输内存块的需要，这有助于提高性能。  
  
### <a name="implementing-and-monitoring-lock-partitioning"></a>实现和监视锁分区  
 默认情况下，对于具有 16 个或更多 CPU 的系统，锁分区是打开的。 启用锁分区后，将在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 错误日志中记录一条信息性消息。  
  
 获取已分区资源的锁时：  
  
-   只能获取单个分区的 NL、SCH-S、IS、IU 和 IX 锁模式。  
  
-   对于以分区 ID 0 开始并且按照分区 ID 顺序排列的所有分区，必须获取非 NL、SCH-S、IS、IU 和 IX 模式的共享锁 (S)、排他锁 (X) 和其他锁。 已分区资源的这些锁将比相同模式中未分区资源的锁占用更多的内存，因为每个分区都是一个有效的单独锁。 内存的增加由分区数决定。 Windows 性能监视器中 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 锁计数器将显示已分区和未分区锁所使用内存信息。  
  
 启动一个事务时，它将被分配给一个分区。 对于此事务，可以分区的所有锁请求都使用分配给该事务的分区。 按照此方法，不同事务对相同对象的锁资源的访问被分布到不同的分区中。  
  
 `sys.dm_tran_locks` 动态管理视图中的 `resource_lock_partition` 列为锁分区资源提供锁分区 ID。 有关详细信息，请参阅 [sys.dm_tran_locks (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)。  
  
### <a name="working-with-lock-partitioning"></a>使用锁分区  
 以下代码示例说明了锁分区。 在这些示例中，为了显示一个具有 16 个 CPU 的计算机系统上的锁分区行为，在两个不同的会话中执行了两个事务。  
  
 这些 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句创建了后续示例中使用的测试对象。  
  
```sql  
-- Create a test table.  
CREATE TABLE TestTable  (col1 int);  
GO  
  
-- Create a clustered index on the table.  
CREATE CLUSTERED INDEX ci_TestTable   
    ON TestTable (col1);  
GO  
  
-- Populate the table.  
INSERT INTO TestTable VALUES (1);  
GO  
```  
  
#### <a name="example-a"></a>示例 A  
 会话 1：  
  
 在一个事务中执行一个 `SELECT` 语句。 由于 `HOLDLOCK` 锁提示的原因，此语句将获取并保留一个对此表的意向共享锁（IS 锁）（此例中忽略行锁和页锁）。 IS 锁只能在分配给事务的分区中获取。 对于此示例，假定 IS 锁是在 ID 为 7 的分区中获取的。  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 会话 2：  
  
 启动事务，在此事务下运行 `SELECT` 语句将获取共享锁（S 锁）并将其保留在表中。 将获取所有分区的 S 锁，这将产生多个表锁，每个分区一个。 例如，在具有 16 个 cpu 的系统上，将对锁分区 ID 为 0-15 的锁分区发出 16 个 S 锁。 由于 S 锁与分区 ID 7 上由会话 1 中的事务持有的 IS 锁兼容，因此事务之间没有阻塞。  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCK, HOLDLOCK);  
```  
  
 会话 1：  
  
 将在会话 1 下仍处于活动状态的事务下执行以下 `SELECT` 语句。 由于排他 (X) 表锁提示，因此事务将尝试获取该表的 X 锁。 但是，由会话 2 中的事务持有的 S 锁将阻塞分区 ID 0 的 X 锁。  
  
```sql  
SELECT col1  
FROM TestTable  
WITH (TABLOCKX);  
```  
  
#### <a name="example-b"></a>示例 B  
 会话 1：  
  
 在一个事务中执行一个 `SELECT` 语句。 由于 `HOLDLOCK` 锁提示的原因，此语句将获取并保留一个对此表的意向共享锁（IS 锁）（此例中忽略行锁和页锁）。 IS 锁只能在分配给事务的分区中获取。 对于此示例，假定 IS 锁是在 ID 为 6 的分区中获取的。  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 会话 2：  
  
 在一个事务中执行一个 `SELECT` 语句。 由于 `TABLOCKX` 锁提示，事务将尝试获取表的排他锁（X 锁）。 请记住，必须获取从分区 ID 0 开始的所有分区的 X 锁。 将获取所有分区 ID 0-5 的 X 锁，但它会被为分区 ID 6 获取的 IS 锁阻塞。  
  
 对于尚未获取 X 锁的分区 ID 7-15，其他事务可以继续获取锁。  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCKX, HOLDLOCK);  
```   
  
##  <a name="row-versioning-based-isolation-levels-in-the-ssdenoversion"></a><a name="Row_versioning"></a>[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]中基于行版本控制的隔离级别  
 从 [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] 开始，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 提供现有事务隔离级别（已提交读）的实现，该实现使用行版本控制提供语句级快照。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 还提供一个事务隔离级别（快照），该级别也使用行版本控制提供事务级快照。  
  
 行版本控制是 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的一般框架，它在修改或删除行时调用写入时复制机制。 这要求在运行事务时，行的旧版本必须可供需要早先事务一致状态的事务使用。 行版本控制可用于执行以下操作：  
  
-   在触发器中生成插入的和删除的表 。 对任何由触发器修改的行都将生成副本。 这包括由启动触发器的语句修改的行，以及由触发器进行的任何数据修改。  
-   支持多个活动的结果集 (MARS)。 如果 MARS 会话在存在活动结果集的情况下发出一条数据修改语句（例如 `INSERT`、`UPDATE` 或 `DELETE`），受修改语句影响的行将进行版本控制。  
-   支持指定 ONLINE 选项的索引操作。  
-   支持基于行版本控制的事务隔离级别：  
    -   新实现的已提交读隔离级别，使用行版本控制提供语句级的读取一致性。  
    -   新快照隔离级别，提供事务级的读取一致性。  
  
 `tempdb` 数据库必须具有足够的空间用于版本存储区。 在 `tempdb` 已满的情况下，更新操作将停止生成版本，并继续执行，但是因为所需的特定行版本不再存在，读取操作可能会失败。 这将影响诸如触发器、MARS 和联机索引的操作。  
  
 已提交读和快照事务的行版本控制的使用过程分为两个步骤：  
  
1.  将 `READ_COMMITTED_SNAPSHOT` 和 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项之一或两者设置为 ON。  
2.  在应用程序中设置相应的事务隔离级别：  

    -   当 `READ_COMMITTED_SNAPSHOT` 数据库选项设置为 ON 时，设置已提交读隔离级别的事务使用行版本控制。  
    -   当 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项设置为 ON 时，事务可以设置快照隔离级别。  
  
 当 `READ_COMMITTED_SNAPSHOT` 或 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项设置为 ON 时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 向使用行版本控制操作数据的每个事务分配一个事务序列号 (XSN)。 事务在执行 `BEGIN TRANSACTION` 语句时启动。 但是，事务序列号在执行 BEGIN TRANSACTION 语句后的第一次读/写操作时开始增加。 事务序列号在每次分配时都增加 1。  
  
 当 `READ_COMMITTED_SNAPSHOT` 或 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项设置为 ON 时，将维护数据库中执行的所有数据修改的逻辑副本（版本）。 特定的事务每次修改行时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 实例都存储以前提交的`tempdb` 中行的图像版本。 每个版本都标记有进行此更改的事务的事务序列号。 已修改行的版本使用链接列表链接在一起。 最新的行值始终存储在当前数据库中，并与 `tempdb` 中存储的版本控制行链接在一起。  
  
> [!NOTE]  
> 修改大型对象 (LOB) 时，只有已更改的片段才会复制到 `tempdb` 中的版本存储区。  
  
 行版本将保持足够长的时间，以满足在基于行版本控制的隔离级别下运行的事务的要求。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]跟踪最早的可用事务序列号，并定期删除带有比最早使用的可用序列号更低的事务序列号的所有行版本。  
  
 两个数据库选项都设置为 OFF 时，只对由触发器或 MARS 会话修改的行或由联机索引操作读取的行生成副本。 这些行版本将在不再需要时被释放。 后台线程会定期执行来删除陈旧的行版本。  
  
> [!NOTE]  
> 对于短期运行的事务，已修改行的版本将可能缓存在缓冲池中，而不会写入 `tempdb` 数据库的磁盘文件中。 如果只是临时需要副本行，它将只是简单地从缓冲池中删除而不会引发 I/O 开销。  
  
### <a name="behavior-when-reading-data"></a>读取数据时的行为  
 当在基于行版本控制的隔离下运行的事务读取数据时，读取操作不会获取正被读取的数据上的共享锁（S 锁），因此不会阻塞正在修改数据的事务。 同时，由于减少了所获取的锁的数量，因此最大程度地降低了锁定资源的开销。 使用行版本控制的已提交读隔离和快照隔离旨在提供副本数据的语句级或事务级读取一致性。  
  
 所有查询，包括在基于行版本控制的隔离级别下运行的事务，都在编译和执行期间获取 Sch-S（架构稳定性）锁。 因此，当并发事务持有表的 Sch-M（架构修改）锁时，将阻塞查询。 例如，数据定义语言 (DDL) 操作在修改表的架构信息之前获取 Sch-M 锁。 查询事务，包括在基于行版本控制的隔离级别下运行的事务，都会在尝试获取 Sch-S 锁时被阻塞。 相反，持有 Sch-S 锁的查询将阻塞尝试获取 Sch-M 锁的并发事务。  
  
 当使用快照隔离级别的事务启动时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例将记录所有当前活动的事务。 当快照事务读取具有版本链的行时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]按照该链检索行，其事务序列号为：  
  
-   最接近但低于读取行的快照事务序列号。  
  
-   不在快照事务启动时活动的事务列表中。  
  
 由快照事务执行的读取操作将检索在快照事务启动时已提交的每行的最新版本。 这提供了在事务启动时存在的数据的事务一致快照。  
  
 使用行版本控制的已提交读事务以大致相同的方式运行。 不同之处在于选择行版本时，已提交读事务不使用其自身的事务序列号。 每次启动语句时，已提交读事务将读取为[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例发出的最新事务序列号。 这是用于为该语句选择正确的行版本的事务序列号。 这使已提交读事务可以查看每个语句启动时存在的数据的快照。  
  
> [!NOTE]  
> 即使使用行版本控制的已提交读事务提供了在语句级别上事务一致的数据视图，但此类事务生成或访问的行版本还将保留，直到事务完成时为止。  
  
### <a name="behavior-when-modifying-data"></a>修改数据时的行为  
 在使用行版本控制的已提交读事务中，使用阻塞性扫描（其中读取数据值时将在数据行上采用更新锁（U 锁）完成选择要更新的行。 这与不使用行版本控制的已提交读事务相同。 如果数据行不符合更新标准，在该行上将释放更新锁并且将锁定下一行并对其进行扫描。  
  
 在快照隔离下运行的事务对数据修改采用乐观方法：获取数据上的锁后，才执行修改以强制应用约束。 否则，直到数据修改时才获取数据上的锁。 当数据行符合更新标准时，快照事务将验证未被并发事务（在快照事务开始后提交）修改的数据行。 如果数据行已在快照事务以外修改，则将出现更新冲突，同时快照事务也将终止。 更新冲突由[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]处理，无法禁用更新冲突检测。  
  
> [!NOTE]  
> 当快照事务访问以下任意项目时，在快照隔离下运行的更新操作将在已提交读隔离下内部执行：  
>  
> 具有 FOREIGN KEY 约束的表。  
>  
> 在其他表的 FOREIGN KEY 约束中引用的表。  
>  
> 引用多个表的索引视图。  
>  
> 但是，即使是在这些条件下，更新操作仍将继续验证数据是否未经其他事务修改。 如果数据已被其他事务修改，则快照事务将遭遇更新冲突并终止。  
  
### <a name="behavior-in-summary"></a>行为摘要  
 下表概括了使用行版本控制的快照隔离与已提交读隔离之间的差异。  
  
|properties|使用行版本控制的已提交读隔离级别|快照隔离级别|  
|--------------|----------------------------------------------------------|------------------------------|  
|必须设置为 ON 以便启用所需支持的数据库选项。|READ_COMMITTED_SNAPSHOT|ALLOW_SNAPSHOT_ISOLATION|  
|会话如何请求特定类型的行版本控制。|使用默认的已提交读隔离级别，或运行 SET TRANSACTION ISOLATION LEVEL 语句来指定 READ COMMITTED 隔离级别。 这可以在事务启动后完成。|需要执行 SET TRANSACTION ISOLATION LEVEL 来在事务启动前指定 SNAPSHOT 隔离级别。|  
|由语句读取的数据的版本。|在每条语句启动前提交的所有数据。|在每个事务启动前提交的所有数据。|  
|如何处理更新。|从行版本恢复到实际的数据，以选择要更新的行并使用选择的数据行上的更新锁。 获取要修改的实际数据行上的排他锁。 没有更新冲突检测。|使用行版本选择要更新的行。 尝试获取要修改的实际数据行上的排他锁，如果数据已被其他事务修改，则出现更新冲突，同时快照事务也将终止。|  
|有更新冲突检测。|无。|集成支持。 无法禁用。|  
  
### <a name="row-versioning-resource-usage"></a>行版本控制资源的使用情况  
 行版本控制框架支持 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中提供的下列功能：  
  
-   触发器  
-   多个活动的结果集 (MARS)  
-   联机索引  
  
 另外，行版本控制框架还支持下列基于行版本控制的事务隔离级别（默认情况下禁用）：  
  
-   `READ_COMMITTED_SNAPSHOT` 数据库选项为 ON 时，`READ_COMMITTED` 事务使用行版本控制提供语句级读取一致性。  
-   `ALLOW_SNAPSHOT_ISOLATION` 数据库选项为 ON 时，`SNAPSHOT` 事务使用行版本控制提供事务级读取一致性。  
  
 基于行版本控制的隔离级别通过消除对读取操作使用共享锁来减少事务获取的锁数目。 这样就减少了管理锁所用资源，从而提高了系统性能。 另外还减少了其他事务获取的锁阻塞事务的次数，也就提高了性能。  
  
 基于行版本控制的隔离级别增加了数据修改所需的资源。 启用这些选项会导致要复制数据库中要修改的所有数据。 即使没有使用基于行版本控制隔离的活动事务，也将修改前的数据备份在 tempdb 中。 修改后的数据包括一个指向存储在 tempdb 中的修改前的数据的指针。 对于大型对象，只将对象中更改过的部分复制到 tempdb 中。  
  
#### <a name="space-used-in-tempdb"></a>TempDB 中使用的空间  
 对于每个[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例，tempdb 都必须具有足够的空间以容纳在该实例中为每个数据库生成的行版本。 数据库管理员必须确保 TempDB 具有足够的空间来支持版本存储区。 TempDB 中有两种版本存储区：  
  
-   联机索引生成版本存储区，用于所有数据库中的联机索引生成操作。  
-   公共版本存储区，用于所有数据库中的所有其他数据修改操作。  
  
 只要活动事务需要访问行版本，就必须存储行版本。 后台线程每隔一分钟删除一次不再需要的行版本，从而释放 TempDB 中的版本空间。 如果长时间运行的事务符合下列任何一个条件，则会阻止释放版本存储区中的空间：  
  
-   使用基于行版本控制的隔离。  
-   使用触发器、MARS 或联机索引生成操作。  
-   生成行版本。  
  
> [!NOTE]  
> 在事务内部调用了触发器后，即使触发器完成后不再需要行版本，由触发器创建的行版本将仍然受到维护直到事务结束。 这也同样适用于使用行版本控制的已提交读事务。 对于这种事务类型，只有事务中的每条语句需要数据库的事务一致视图。 这表示语句完成后将不再需要在事务中为它创建的行版本。 但是，由事务中的每条语句创建的行版本将受到维护，直到事务完成。  
  
 当 TempDB 运行空间不足时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]强制收缩版本存储区。 在执行收缩进程的过程中，尚未生成行版本且运行时间最长的事务被标记为牺牲品。 在错误日志中为每个作为牺牲品的事务生成消息 3967。 如果某个事务被标记为牺牲品，则该事务不能再读取版本存储区中的行版本。 当其尝试读取行版本时，会生成消息 3966 且该事务会被回滚。 如果收缩进程成功，则 tempdb 中就有可用空间。 否则 tempdb 运行空间不足，并出现下列情况：  
  
-   写操作继续执行但不生成版本。 错误日志中会生成一条信息消息 (3959)，但写数据的事务不受影响。  
  
-   尝试访问由于 tempdb 完全回滚而未生成的行版本的事务终止，并生成错误消息 3958。  
  
#### <a name="space-used-in-data-rows"></a>数据行中使用的空间  
 每个数据库行的结尾处最多可以使用 14 个字节记录行版本控制信息。 行版本控制信息包含提交版本的事务的事务序列号和指向版本行的指针。 如果符合下列任何一种条件，则第一次修改行时或插入新行时添加这 14 个字节：  
  
-   `READ_COMMITTED_SNAPSHOT` 或 `ALLOW_SNAPSHOT_ISOLATION` 选项为 ON。  
-   表有触发器。  
-   正在使用多个活动的结果集 (MARS)。  
-   当前正在对表执行联机索引生成操作。  
  
 如果符合下列所有条件，则第一次修改数据库行时，将从行中删除这 14 个字节：  
  
-   `READ_COMMITTED_SNAPSHOT` 和 `ALLOW_SNAPSHOT_ISOLATION` 选项为 OFF。  
-   表不再有触发器。  
-   当前没有使用 MARS。  
-   当前没有执行联机索引生成操作。  
  
 如果使用了行版本控制功能，则可能需要为数据库分配额外的磁盘空间，才能使每个数据库行可多使用 14 个字节。 如果当前页上没有足够的可用空间，则添加行版本控制信息会导致拆分索引页或分配新的数据页。 例如，如果平均行长度为 100 个字节，则额外的 14 个字节会导致现有表增大 14%。  
  
 减少[填充因子](../relational-databases/indexes/specify-fill-factor-for-an-index.md)可能有助于避免或减少索引页碎片。 若要查看表或视图的数据和索引的碎片信息，可以使用 [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)。  
  
#### <a name="space-used-in-large-objects"></a>大型对象中使用的空间  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]支持六种数据类型（最多可以容纳大小为 2 GB 的大型字符串）：`nvarchar(max)`、`varchar(max)`、`varbinary(max)`、`ntext`、`text` 和 `image`。 使用这些数据类型的大型字符串存储在一系列与数据行链接的数据片段中。 行版本控制信息存储在用于存储这些大型字符串的每个片段中。 数据片段是表中专用于大型对象的页集合。  
  
 新的大型值添加到数据库中时，系统会为它们分配数据片段，每个片段最多可以存储 8040 个字节的数据。 早期版本的[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]中，每个片段最多可以存储 8080 个字节的 `ntext`、`text` 或 `image` 数据。  
  
 数据库从早期版本的 `ntext` 升级到 `text` 时，现有的 `image`、[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 大型对象 (LOB) 数据并未更新来为行版本控制信息释放一些空间。 但第一次修改 LOB 数据时，该数据会动态升级以实现版本控制信息的存储。 即使未生成行版本也是如此。 LOB 数据升级后，每个片段最多可以存储的字节数从 8080 个减少到 8040 个。 升级过程相当于先删除 LOB 值再重新插入相同值。 即使只修改一个字节也会升级 LOB 数据。 对于每个 `ntext`、`text` 或 `image` 列，这是一次性操作，但每个操作可能生成大量页分配和 I/O 活动，具体情况取决于 LOB 数据的大小。 如果完整记录修改，还会生成大量日志记录活动。 如果数据库恢复模式未设置为 FULL，则按最小方式记录 WRITETEXT 操作和 UPDATETEXT 操作。  
  
 在早期版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中不使用 `nvarchar(max)`、`varchar(max)` 和 `varbinary(max)` 数据类型。 因此，这些数据类型不存在升级问题。  
  
 应该分配足够的磁盘空间来满足此要求。  
  
#### <a name="monitoring-row-versioning-and-the-version-store"></a>监视行版本控制和版本存储区  
 为了监视行版本控制、版本存储区和快照隔离进程以了解性能和问题，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供了一些工具，包括动态管理视图 (DMV) 和 Windows 系统监视器中的性能计数器。  
  
##### <a name="dmvs"></a>DMV  
 下列 DMV 提供有关 tempdb 的当前系统状态、版本存储区以及使用行版本控制的事务的信息。  
  
 sys.dm_db_file_space_usage。 返回数据库中每个文件的空间使用信息。 有关详细信息，请参阅 [sys.dm_db_file_space_usage (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)。  
  
 sys.dm_db_session_space_usage。 返回会话为数据库进行的页分配和释放活动。 有关详细信息，请参阅 [sys.dm_db_session_space_usage (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)。  
  
 sys.dm_db_task_space_usage。 返回任务为数据库进行的页分配和释放活动。 有关详细信息，请参阅 [sys.dm_db_task_space_usage (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)。  
  
 sys.dm_tran_top_version_generators。 返回一个虚拟表，其中包含生成的版本是版本存储区中最多的对象。 该表按 database_id 和 rowset_id 对前 256 位的聚合记录长度进行分组。 可以使用此函数来查找版本存储区的最大使用者。 有关详细信息，请参阅 [sys.dm_tran_top_version_generators (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-top-version-generators-transact-sql.md)。  
  
 sys.dm_tran_version_store。 返回一个虚拟表，其中显示有公共版本存储区中的所有版本记录。 有关详细信息，请参阅 [sys.dm_tran_version_store (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md)。  

 sys.dm_tran_version_store_space_usage。 返回一个虚拟表，该表显示每个数据库的版本存储记录使用的 tempdb 中的总空间。 有关详细信息，请参阅 [sys.dm_tran_version_store_space_usage (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-space-usage.md)。  

> [!NOTE]  
> 由于 sys.dm_tran_top_version_generators 和 sys.dm_tran_version_store 都查询整个版本存储区（可能非常大），因此运行这两者时可能要占用大量资源。  
> sys.dm_tran_version_store_space_usage 高效且运行开销低，因为它不会浏览单个版本存储记录，并返回每个数据库 tempdb 中使用的聚合版本存储空间
  
 sys.dm_tran_active_snapshot_database_transactions。 返回一个虚拟表，其中包含使用行版本控制的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例中的所有数据库中的所有活动事务。 但系统事务不会显示在此 DMV 中。 有关详细信息，请参阅 [sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md)。  
  
 sys.dm_tran_transactions_snapshot。 返回一个虚拟表，其中显示有每个事务使用的快照。 该快照包含了使用行版本控制的活动事务的序列号。 有关详细信息，请参阅 [sys.dm_tran_transactions_snapshot (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-transactions-snapshot-transact-sql.md)。  
  
 sys.dm_tran_current_transaction。 返回一行，其中显示有当前会话中与行版本控制相关的事务状态信息。 有关详细信息，请参阅 [sys.dm_tran_current_transaction (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-transaction-transact-sql.md)。  
  
 sys.dm_tran_current_snapshot。 返回一个虚拟表，其中显示有当前快照隔离事务启动时的所有活动事务。 如果当前事务正在使用快照隔离，则该函数不返回行。 sys.dm_tran_current_snapshot 类似于 sys.dm_tran_transactions_snapshot，只不过它仅返回当前快照的活动事务。 有关详细信息，请参阅 [sys.dm_tran_current_snapshot (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-snapshot-transact-sql.md)。  
  
##### <a name="performance-counters"></a>性能计数器  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 性能计数器提供有关受 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程影响的系统性能的信息。 下列性能计数器监视 tempdb、版本存储区以及使用行版本控制的事务。 这些性能计数器包含在 SQLServer:Transactions 性能对象中。  
  
 **Free Space in tempdb (KB)** 。 监视 tempdb 数据库中的可用空间 (KB)。 tempdb 中必须有足够的可用空间来容纳支持快照隔离的版本存储区。  
  
 下列公式可以用来粗略估计版本存储区的大小。 对于长时间运行的事务，监视生成速率和清除速率对于估计版本存储区的最大大小会非常有用。  
  
 [公共版本存储区的大小] = 2 * [每分钟生成的版本存储区数据] \* [事务的最长运行时间（分钟）]  
  
 事务的最长运行时间不应该包括联机索引生成时间。 对于超大型表，由于这些操作可能要花很长的时间，因此联机索引生成使用单独的版本存储区。 当联机索引生成处于活动状态时，联机索引生成版本存储区的近似大小等于表（包括所有索引）中修改的数据量。  
  
 **Version Store Size (KB)** 。 监视所有版本存储区的大小 (KB)。 此信息有助于确定版本存储区在 tempdb 数据库中所需的空间大小。 监视计数器一段时间，可以获得有用的信息来估计在 tempdb 数据库中所需的额外空间。  
  
 `Version Generation rate (KB/s)`. 监视所有版本存储区中的版本生成速率（KB/秒）。  
  
 `Version Cleanup rate (KB/s)`. 监视所有版本存储区中的版本清除速率（KB/秒）。  
  
> [!NOTE]  
> Version Generation rate (KB/s) 和 Version Cleanup rate (KB/s) 的信息可以用于预测 tempdb 空间要求。  
  
 **Version Store unit count**。 监视版本存储区单元的计数。  
  
 **Version Store unit creation**。 监视自启动实例后创建用于存储行版本的版本存储区单元总数。  
  
 **Version Store unit truncation**。 监视自启动实例后被截断的版本存储区单元总数。 当 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 确定不需要任何存储在版本存储区单元中的版本行来运行活动事务时，版本存储区单元即被截断。  
  
 **Update conflict ratio**。 监视存在更新冲突的更新快照事务与更新快照事务总数的比值。  
  
 **Longest Transaction Running Time**。 监视使用行版本控制的事务的最长运行时间（秒）。 这可用于确定是否存在事务的运行时间不合适的情况。  
  
 **Transactions**。 监视活动事务的总数， 不包括系统事务。  
  
 `Snapshot Transactions`. 监视活动快照事务的总数。  
  
 `Update Snapshot Transactions`. 监视执行更新操作的活动快照事务的总数。  
  
 `NonSnapshot Version Transactions`. 监视生成版本记录的活动非快照事务的总数。  
  
> [!NOTE]  
> Update Snapshot Transactions 与 NonSnapshot Version Transactions 之和表示参与版本生成的事务的总数。 Snapshot Transactions 与 Update Snapshot Transactions 之差表示只读快照事务数。  
  
### <a name="row-versioning-based-isolation-level-example"></a>基于行版本控制的隔离级别示例  
 下列示例说明使用行版本控制的快照隔离事务与已提交读事务的行为差异。  
  
#### <a name="a-working-with-snapshot-isolation"></a>A. 使用快照隔离  
 在此示例中，在快照隔离下运行的事务将读取数据，然后由另一事务修改此数据。 快照事务不阻塞由其他事务执行的更新操作，它忽略数据的修改继续从版本化的行读取数据。 但是，当快照事务尝试修改已由其他事务修改的数据时，快照事务将生成错误并终止。  
  
 在会话 1 上：  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Enable snapshot isolation on the database.  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Start a snapshot transaction  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 在会话 2 上：  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under snapshot isolation shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 在会话 1 上：  
  
```sql  
    -- Reissue the SELECT statement - this shows  
    -- the employee having 48 vacation hours.  The  
    -- snapshot transaction is still reading data from  
    -- the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 在会话 2 上：  
  
```sql  
-- Commit the transaction; this commits the data  
-- modification.  
COMMIT TRANSACTION;  
GO  
```  
  
 在会话 1 上：  
  
```sql  
    -- Reissue the SELECT statement - this still   
    -- shows the employee having 48 vacation hours  
    -- even after the other transaction has committed  
    -- the data modification.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- Because the data has been modified outside of the  
    -- snapshot transaction, any further data changes to   
    -- that data by the snapshot transaction will cause   
    -- the snapshot transaction to fail. This statement   
    -- will generate a 3960 error and the transaction will   
    -- terminate.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION  
GO  
```  
  
#### <a name="b-working-with-read-committed-using-row-versioning"></a>B. 使用通过行版本控制的已提交读  
 在此示例中，使用行版本控制的已提交读事务与其他事务并发运行。 已提交读事务的行为与快照事务的行为有所不同。 与快照事务相同的是，即使其他事务修改了数据，已提交读事务也将读取版本化的行。 然而，与快照事务不同的是，已提交读将执行下列操作：  
  
-   在其他事务提交数据更改后，读取修改的数据。  
-   能够更新由其他事务修改的数据，而快照事务不能。  
  
 在会话 1 上：  
  
```sql  
USE AdventureWorks2016;  -- Or any earlier version of the AdventureWorks database.  
GO  
  
-- Enable READ_COMMITTED_SNAPSHOT on the database.  
-- For this statement to succeed, this session  
-- must be the only connection to the AdventureWorks2016  
-- database.  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
GO  
  
-- Start a read-committed transaction  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 在会话 2 上：  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under read-committed using row versioning shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 在会话 1 上：  
  
```sql  
    -- Reissue the SELECT statement - this still shows  
    -- the employee having 48 vacation hours.  The  
    -- read-committed transaction is still reading data   
    -- from the versioned row and the other transaction   
    -- has not committed the data changes yet.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 在会话 2 上：  
  
```sql  
-- Commit the transaction.  
COMMIT TRANSACTION;  
GO  
```  
  
 在会话 1 上：  
  
```sql  
    -- Reissue the SELECT statement which now shows the   
    -- employee having 40 vacation hours.  Being   
    -- read-committed, this transaction is reading the   
    -- committed data. This is different from snapshot  
    -- isolation which reads from the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- This statement, which caused the snapshot transaction   
    -- to fail, will succeed with read-committed using row versioning.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION;  
GO  
```  
  
### <a name="enabling-row-versioning-based-isolation-levels"></a>启用基于行版本控制的隔离级别  
 数据库管理员可以通过在 ALTER DATABASE 语句中使用 `READ_COMMITTED_SNAPSHOT` 和 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项来控制行版本控制的数据库级别设置。  
  
 当 `READ_COMMITTED_SNAPSHOT` 数据库选项设置为 ON 时，用于支持该选项的机制将立即激活。 设置 READ_COMMITTED_SNAPSHOT 选项时，数据库中只允许存在执行 `ALTER DATABASE` 命令的连接。 在 ALTER DATABASE 完成之前，数据库中不允许有其他打开的连接。 数据库不必处于单用户模式。  
  
 以下 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句启用 `READ_COMMITTED_SNAPSHOT`：  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
```  
  
 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项设置为 ON 时，数据库中数据已修改的所有活动事务完成之前，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 实例不会为已修改的数据生成行版本。 如果存在活动的修改事务，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将该选项的状态设置为 `PENDING_ON`。 所有修改事务完成后，该选项的状态更改为 ON。 在该选项完全处于 ON 状态之前，用户无法在数据库中启动快照事务。 数据库管理员将 `ALLOW_SNAPSHOT_ISOLATION` 选项设置为 OFF 时，数据库将跳过 PENDING_OFF 状态。  
  
 下面的 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句将启用 ALLOW_SNAPSHOT_ISOLATION：  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
```  
  
 下表列出并说明了 ALLOW_SNAPSHOT_ISOLATION 选项的各个状态。 同时使用 ALTER DATABASE 和 ALLOW_SNAPSHOT_ISOLATION 选项不会妨碍当前正在访问数据库数据的用户。  
  
|当前数据库的快照隔离框架状态|说明|  
|----------------------------------------------------------------|-----------------|  
|OFF|未启用对快照隔离事务的支持。 不允许执行快照隔离事务。|  
|PENDING_ON|对快照隔离事务的支持处于转换状态（从 OFF 到 ON）。 打开的事务必须完成。<br /><br /> 不允许执行快照隔离事务。|  
|ON|已启用对快照隔离事务的支持。<br /><br /> 允许执行快照事务。|  
|PENDING_OFF|对快照隔离事务的支持处于转换状态（从 ON 到 OFF）。<br /><br /> 此后启动的快照事务无法访问此数据库。 更新事务仍会导致此数据库中出现版本控制开销。 现有快照事务仍可以访问此数据库，不会遇到任何问题。 直到数据库快照隔离状态为 ON 时处于活动状态的所有快照事务完成后，状态 PENDING_OFF 才变为 OFF。|  
  
 使用 `sys.databases` 目录视图可以确定两个行版本控制数据库选项的状态。  
  
 对用户表和存储在 master 和 msdb 中的某些系统表的任何更新都会生成行版本。  
  
 在 master 和 msdb 数据库中，`ALLOW_SNAPSHOT_ISOLATION` 选项自动设置为 ON，并且不能禁用。  
  
 在 master 数据库、tempdb 数据库或 msdb 数据库中，用户不能将 `READ_COMMITTED_SNAPSHOT` 选项设置为 ON。  
  
### <a name="using-row-versioning-based-isolation-levels"></a>使用基于行版本控制的隔离级别  
 行版本控制框架在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中始终处于启用状态，并被多个功能使用。 它除了提供基于行版本控制的隔离级别之外，还用于支持对触发器和多个活动结果集 (MARS) 会话的修改，以及 ONLINE 索引操作的数据读取。  
  
 基于行版本控制的隔离级别是在数据库级别上启用的。 访问已启用数据库的对象的任何应用程序可以使用以下隔离级别运行查询：  
  
-   已提交读隔离级别，通过将 `READ_COMMITTED_SNAPSHOT` 数据库选项设置为 `ON` 来使用行版本控制，如下面的代码示例所示：  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET READ_COMMITTED_SNAPSHOT ON;  
    ```  
  
     为 `READ_COMMITTED_SNAPSHOT` 启用数据库后，在已提交读隔离级别下运行的所有查询将使用行版本控制，这意味着读取操作不会阻止更新操作。  
  
-   快照隔离，通过将 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项设置为 `ON` 实现，如下面的代码示例所示：  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET ALLOW_SNAPSHOT_ISOLATION ON;  
    ```  
  
     在快照隔离下运行的事务可以访问数据库中为快照启用的表。 若要访问没有为快照启用的表，则必须更改隔离级别。 例如，下面的代码示例显示了在快照事务下运行时联接两个表的 `SELECT` 语句。 一个表属于未启用快照隔离的数据库。 当 `SELECT` 语句在快照隔离下运行时，该语句无法成功执行。  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
     下面的代码示例显示了已修改为从事务隔离级别更改为已提交读隔离级别的相同 `SELECT` 语句。 由于此更改，`SELECT` 语句将成功执行。  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            WITH (READCOMMITTED)  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
#### <a name="limitations-of-transactions-using-row-versioning-based-isolation-levels"></a>使用基于行版本控制的隔离级别的事务的限制  
 使用基于行版本控制的隔离级别时，请考虑下列限制：  
  
-   `READ_COMMITTED_SNAPSHOT` 无法在 tempdb、msdb 或 master 中启用。  
-   全局临时表存储在 tempdb 中。 访问快照事务中的全局临时表时，必须执行下列操作之一：  
    -   在 tempdb 中将 `ALLOW_SNAPSHOT_ISOLATION` 数据库选项设置为 ON。  
    -   使用隔离提示更改语句的隔离级别。  
-   如果出现以下情况，快照事务将失败：  
    -   从快照事务启动之后到访问数据库前的期间内，数据库设置为只读。  
    -   如果访问多个数据库的对象，数据库状态以如下方式更改：从快照事务启动后到访问数据库前的期间内，发生数据库恢复。 例如：将数据库设置为 OFFLINE，然后设置为 ONLINE，数据库将自动关闭和打开，或数据库将分离和附加。  
-   快照隔离不支持分布式事务，包括分布式分区数据库中的查询。  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不会保留多个版本的系统元数据。 表中的数据定义语言 (DDL) 语句和其他数据库对象（索引、视图、数据类型、存储过程和公共语言运行时函数）会更改元数据。 如果 DDL 语句修改一个对象，那么在快照隔离下对该对象的任何并发引用都将导致快照事务失败。 READ_COMMITTED_SNAPSHOT 数据库选项为 ON 时，已提交读事务没有此限制。  
  
     例如，数据库管理员执行下面的 `ALTER INDEX` 语句。  
  
    ```sql  
    USE AdventureWorks2016;  
    GO  
    ALTER INDEX AK_Employee_LoginID  
        ON HumanResources.Employee REBUILD;  
    GO  
    ```  
  
     执行 `ALTER INDEX` 语句后，任何在执行 `HumanResources.Employee` 语句时处于活动状态的快照事务，如果试图引用 `ALTER INDEX` 表，都将收到错误。 而使用行版本控制的已提交读事务不受影响。  
  
    > [!NOTE]  
    > BULK INSERT 操作可能会导致对目标表元数据的更改（例如，禁用约束检查时）。 如果出现这种情况，访问大容量插入表的并发快照隔离事务将失败。  
  
   
## <a name="customizing-locking-and-row-versioning"></a>自定义锁定和行版本控制  
  
### <a name="customizing-the-lock-time-out"></a>自定义锁超时  
 当 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 实例由于其他事务已拥有资源的冲突锁而无法将锁授予给某个事务时，将阻止第一个事务，等待现有锁被释放。 默认情况下，没有强制的超时期限，并且除了尝试访问数据（有可能被无限期阻塞）外，没有其他方法可以测试某个资源是否在锁定之前已被锁定。  
  
> [!NOTE]  
> 在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中，使用 sys.dm_os_waiting_tasks 动态管理视图确定某个进程是否被阻塞以及被谁阻塞。 在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的早期版本中，使用 sp_who 系统存储过程。  
  
 `LOCK_TIMEOUT` 设置允许应用程序设置语句等待阻塞资源的最长时间。 如果某个语句等待的时间超过 LOCK_TIMEOUT 的设置时间，则被阻塞的语句自动取消，并会有错误消息 1222 (`Lock request time-out period exceeded`) 返回给应用程序。 但是，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不会回滚或取消任何包含语句的事务。 因此，应用程序必须具有可以捕获错误消息 1222 的错误处理程序。 如果应用程序不能捕获错误，则会在不知道事务中已有个别语句被取消的情况下继续运行，由于事务中后面的语句可能依赖于从未执行过的语句，因此会出现错误。  
  
 实现捕获错误消息 1222 的错误处理程序后，应用程序可以处理超时情况，并采取补救措施，例如：自动重新提交被阻塞的语句或回滚整个事务。  
  
 若要确定当前的 `LOCK_TIMEOUT` 设置，请执行 `@@LOCK_TIMEOUT` 函数：  
  
```sql  
SELECT @@lock_timeout;  
GO  
```  
  
### <a name="customizing-transaction-isolation-level"></a>自定义事务隔离级别  
 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 的默认隔离级别为 READ COMMITTED。 如果应用程序必须在其他隔离级别运行，则它可以使用以下方法设置隔离级别：  
  
-   运行 [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md) 语句。  
-   使用 System.Data.SqlClient 托管命名空间的 ADO.NET 应用程序可以使用 SqlConnection.BeginTransaction 方法来指定 IsolationLevel 选项。  
-   使用了 ADO 的应用程序可以设置 `Autocommit Isolation Levels` 属性。  
-   启动事务时，使用 OLE DB 的应用程序可以调用 ITransactionLocal::StartTransaction，并在调用时将 isoLevel 设置为所需的事务隔离级别。 在自动提交模式下指定隔离级别时，使用 OLE DB 的应用程序可以将 DBPROPSET_SESSION 属性 DBPROP_SESS_AUTOCOMMITISOLEVELS 设置为所需的事务隔离级别。  
-   使用 ODBC 的应用程序可以使用 SQLSetConnectAttr 设置 SQL_COPT_SS_TXN_ISOLATION 属性。  
  
指定隔离级别后，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 会话中的所有查询语句和数据操作语言 (DML) 语句的锁定行为都将在该隔离级别进行操作。 隔离级别将在会话终止或将其设置为其他级别后失效。  
  
下面的示例设置 `SERIALIZABLE` 隔离级别：  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
SELECT BusinessEntityID  
    FROM HumanResources.Employee;  
GO  
```  
  
必要时，可以通过指定表级提示来替代各个查询语句或 DML 语句的隔离级别。 指定表级提示不会影响会话中的其他语句。 建议仅在确实必要时才使用表级提示更改默认行为。  
  
读取元数据时，甚至当隔离级别被设置为在读取数据时不请求共享锁的级别时，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]也可能需要获取锁。 例如，在未提交读隔离级别下运行的事务在读取数据时将不获取共享锁，但是在读取系统目录视图时可能会请求锁。 这意味着在查询表时如果某个并发事务正在修改该表的元数据，则未提交读事务可能会导致阻塞。  
  
若要确定当前设置的事务隔离级别，请使用 `DBCC USEROPTIONS` 语句，如下面的示例所示。 该结果集可能与系统的结果集不同。  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
DBCC USEROPTIONS;  
GO  
```  
  
[!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```
Set Option                   Value  
---------------------------- -------------------------------------------  
textsize                     2147483647  
language                     us_english  
dateformat                   mdy  
datefirst                    7  
...                          ...  
Isolation level              repeatable read  
 
(14 row(s) affected)   
 
DBCC execution completed. If DBCC printed error messages, contact your system administrator.
```  
  
### <a name="locking-hints"></a>锁提示  
 可以在 SELECT、INSERT、UPDATE 及 DELETE 语句中为单个表引用指定锁提示。 提示指定 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例用于表数据的锁类型或行版本控制。 当需要对对象所获得锁类型进行更精细控制时，可以使用表级锁提示。 这些锁提示覆盖会话的当前事务隔离级别。  
  
 有关特定锁提示及其行为的详细信息，请参阅[表提示 (Transact-SQL)](../t-sql/queries/hints-transact-sql-table.md)。  
  
> [!NOTE]  
> [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 几乎总是会选择正确的锁级别。 建议只在必要时才使用表级锁提示来更改默认的锁行为。 禁止锁级别反过来会影响并发。  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]在读取元数据时可能必须获取锁，即使是处理使用了防止在读取数据时请求共享锁的锁提示的选择。 例如，使用 `NOLOCK` 提示的 `SELECT` 在读取数据时不获取共享锁，但有时在读取系统目录视图时可能会请求锁。 这意味着可能会阻止使用 `NOLOCK` 的 `SELECT`语句。  
  
 如下面的示例中所示，如果将事务隔离级别设置为 `SERIALIZABLE`，并且在 `NOLOCK` 语句中使用表级锁提示 `SELECT`，则不采用通常用于维护可序列化事务的键范围锁。  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT JobTitle  
    FROM HumanResources.Employee WITH (NOLOCK);  
GO  
  
-- Get information about the locks held by   
-- the transaction.  
SELECT    
        resource_type,   
        resource_subtype,   
        request_mode  
    FROM sys.dm_tran_locks  
    WHERE request_session_id = @@spid;  
  
-- End the transaction.  
ROLLBACK;  
GO  
```  
  
 引用 `HumanResources.Employee` 唯一采用的锁是架构稳定性锁（Sch-S 锁）。 在这种情况下，不再保证可序列化性。  
  
 在 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 中，`ALTER TABLE` 的 `LOCK_ESCALATION` 选项可以禁用表锁，并在已分区表上启用 HoBT 锁。 此选项不是一个锁提示，但是可用来减少锁升级。 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../t-sql/statements/alter-table-transact-sql.md)。  
  
###  <a name="customizing-locking-for-an-index"></a><a name="Customize"></a> 自定义索引的锁定  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]使用动态锁定策略，这种策略能够在大多数情况下自动为查询选择最佳锁定粒度。 建议您不要替代启用页锁定和行锁定的默认锁定级别，除非透彻地了解了表或索引的访问模式且这些访问模式保持一致，并且存在有待解决的资源争用问题。 替代锁定级别可以明显妨碍对表或索引的并发访问。 例如，对用户时常访问的大型表仅指定表级锁可能会造成瓶颈，因为用户必须等待表级锁释放后才能访问该表。  
  
 在为数不多的情况下，不允许页锁定或行锁定可能会有好处，但必须透彻地了解访问模式且这些访问模式保持一致。 例如，某个数据库应用程序使用的查找表在批处理进程中每周更新一次。 并发读取器使用共享锁 (S) 访问表，每周批处理更新使用排他锁 (X) 访问表。 关闭表的页锁定和行锁定可以使读取器通过共享表锁对表进行并发访问，从而在整周内降低锁定开销。 在批处理作业运行时，由于它获得了排他表锁，因此可以高效地完成更新。  
  
 由于每周批处理更新在运行时将阻止并发读取器访问表，因此关闭页锁定和行锁定可能是可取的，也可能不可取。 如果批处理作业仅更改少数几行或几页，则可以更改锁定级别以允许行级别或页级别的锁定，这将允许其他会话读取表中的数据而不会受到阻止。 如果批处理作业要进行大量更新，则获取表的排他锁可能是确保批处理作业高效完成的最佳途径。  
  
 当两个并发操作获得同一个表的行锁然后进行阻止时，偶尔会出现死锁，因为这两个操作都需要锁定该页。 如果不允许使用行锁，则会强行使其中一个操作等待，从而避免死锁。  
  
 使用 `CREATE INDEX` 和 `ALTER INDEX` 语句来设置索引使用的锁定粒度。 该锁设置适用于索引页和表页。 此外，`CREATE TABLE` 和 `ALTER TABLE` 语句可用于设置 `PRIMARY KEY` 和 `UNIQUE` 约束上的锁定粒度。 对于向后兼容，还可以使用 `sp_indexoption` 系统存储过程设置粒度。 若要显示给定索引的当前锁定选项，请使用 `INDEXPROPERTY` 函数。 可以禁止将页级别的锁、行级别的锁或二者的组合用于指定的索引。  
  
|禁止的锁|访问索引的锁|  
|----------------------|-----------------------|  
|页级别|行级别的锁和表级别的锁|  
|行级别|页级别的锁和表级别的锁|  
|页级别和行级别|表级别的锁|  
  
##  <a name="advanced-transaction-information"></a><a name="Advanced"></a> 高级事务信息  
  
### <a name="nesting-transactions"></a>嵌套事务  
 显式事务可以嵌套。 这主要是为了支持存储过程中的一些事务，这些事务可以从已在事务中的进程调用，也可以从没有活动事务的进程中调用。  
  
 下列示例显示了嵌套事务的用途。 TransProc 过程强制执行其事务，而不管执行事务的进程的事务模式。 如果在事务活动时调用 TransProc，很可能会忽略 TransProc 中的嵌套事务，而根据对外部事务采取的最终操作提交或回滚其 `INSERT` 语句 。 如果由不含未完成事务的进程执行 `TransProc`，该过程结束时，`COMMIT TRANSACTION` 将有效地提交 `INSERT` 语句。  
  
```sql  
SET QUOTED_IDENTIFIER OFF;  
GO  
SET NOCOUNT OFF;  
GO  
CREATE TABLE TestTrans(Cola INT PRIMARY KEY,  
               Colb CHAR(3) NOT NULL);  
GO  
CREATE PROCEDURE TransProc @PriKey INT, @CharCol CHAR(3) AS  
BEGIN TRANSACTION InProc  
INSERT INTO TestTrans VALUES (@PriKey, @CharCol)  
INSERT INTO TestTrans VALUES (@PriKey + 1, @CharCol)  
COMMIT TRANSACTION InProc;  
GO  
/* Start a transaction and execute TransProc. */  
BEGIN TRANSACTION OutOfProc;  
GO  
EXEC TransProc 1, 'aaa';  
GO  
/* Roll back the outer transaction, this will  
   roll back TransProc's nested transaction. */  
ROLLBACK TRANSACTION OutOfProc;  
GO  
EXECUTE TransProc 3,'bbb';  
GO  
/* The following SELECT statement shows only rows 3 and 4 are   
   still in the table. This indicates that the commit  
   of the inner transaction from the first EXECUTE statement of  
   TransProc was overridden by the subsequent rollback. */  
SELECT * FROM TestTrans;  
GO  
```  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]将忽略内部事务的提交。 根据最外部事务结束时采取的操作，将提交或者回滚内部事务。 如果提交外部事务，也将提交内部嵌套事务。 如果回滚外部事务，也将回滚所有内部事务，不管是否单独提交过内部事务。  
  
 对 `COMMIT TRANSACTION` 或 `COMMIT WORK` 的每次调用都适用于上次执行的 `BEGIN TRANSACTION`。 如果嵌套 `BEGIN TRANSACTION` 语句，那么 `COMMIT` 语句只应用于最后一个嵌套的事务，也就是在最内部的事务。 即使嵌套事务内部的 `COMMIT TRANSACTION` transaction_name 语句引用外部事务的事务名称，该提交也只应用于最内部的事务。  
  
 `ROLLBACK TRANSACTION` 语句的 transaction_name 参数引用一组已命名的嵌套事务的内部事务是不合法的。 transaction_name 只能引用最外部事务的事务名称。 如果在一组嵌套事务的任意级别执行使用外部事务名称的 ROLLBACK TRANSACTION transaction_name 语句，那么所有嵌套事务都将回滚。 如果在一组嵌套事务的任意级别执行没有 transaction_name 参数的 `ROLLBACK WORK` 或 `ROLLBACK TRANSACTION` 语句，那么所有嵌套事务都将回滚，包括最外部事务。  
  
 `@@TRANCOUNT` 函数记录当前事务的嵌套级别。 每个 `BEGIN TRANSACTION` 语句以 1 为增量递增 `@@TRANCOUNT`。 每个 `COMMIT TRANSACTION` 或 `COMMIT WORK` 语句以 1 为增量递增 `@@TRANCOUNT`。 没有事务名称的 `ROLLBACK WORK` 或 `ROLLBACK TRANSACTION` 语句将回滚所有嵌套事务，并将 `@@TRANCOUNT` 递减到 0。 在一组嵌套事务中，使用最外部事务的事务名称的 `ROLLBACK TRANSACTION` 将回滚所有嵌套事务，并将 `@@TRANCOUNT` 减小到 0。 在无法确定是否已经在事务中时，可使用 `SELECT @@TRANCOUNT` 确定是等于 1 还是大于 1。 如果 `@@TRANCOUNT` 为 0，表明不在事务中。  
  
### <a name="using-bound-sessions"></a>使用绑定会话  
 绑定会话有利于在同一台服务器上的多个会话之间协调操作。 绑定会话允许一个或多个会话共享相同的事务和锁，并可以使用同一数据，而不会有锁冲突。 可以从同一个应用程序内的多个会话中创建绑定会话，也可以从包含不同会话的多个应用程序中创建绑定会话。  
  
 若要参与绑定会话，会话必须调用 `sp_getbindtoken` 或 `srv_getbindtoken`（通过开放式数据服务）来获取绑定令牌。 绑定令牌是一个字符串，它唯一地标识每个绑定事务。 然后，将绑定令牌发送给要与当前会话绑定的其他会话。 其他会话通过调用 sp_bindsession，并使用从第一个会话中接收到的绑定令牌绑定到事务。  
  
> [!NOTE]  
> 会话必须包含活动的用户事务，`sp_getbindtoken` 或 `srv_getbindtoken` 才能成功。  
  
 必须将绑定令牌从执行第一个会话的应用程序代码传输到随后将其会话绑定到第一个会话的应用程序代码。 没有应用程序可以用来获取由另一个进程启动的事务绑定令牌的 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句或 API 函数。 可以用来传输绑定令牌的方法包括：  
  
-   如果所有会话都是从同一个应用程序进程启动的，绑定令牌就可以存储在共用内存中，也可以作为参数传递到函数中。  
  
-   如果会话是从不同的应用程序进程启动的，那么可以使用进程间通信 (IPC)（例如，远程过程调用 [RPC] 或动态数据交换 [DDE]）来传输绑定令牌。  
  
-   可以将绑定令牌存储在 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例中的某个表中，该表可由要绑定到第一个会话的进程读取。  
  
 在一组绑定会话中，任何时候只能有一个会话是活动的。 如果有一个会话正在实例上执行一个语句，或包含从实例挂起的结果，则在当前会话完成处理或取消当前语句之前，其他绑定到该会话的会话都不能访问该实例。 如果该实例正在忙于处理来自另一个绑定会话的语句，则将出现错误，指明事务空间正在使用中，会话应稍后重试。  
  
 绑定会话后，每个会话仍保留其隔离级别设置。 使用 SET TRANSACTION ISOLATION LEVEL 更改某个会话的隔离级别设置不会影响绑定到该会话的任何其他会话的设置。  
  
#### <a name="types-of-bound-sessions"></a>绑定会话的类型  
 有两种类型的绑定会话：本地绑定会话和分布式绑定会话。  
  
-   **本地绑定会话**  
    允许绑定会话共享单个[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例中的单个事务的事务空间。  
  
-   **分布式绑定会话**  
    允许在使用 [!INCLUDE[msCoName](../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 提交或回滚整个事务之前，绑定会话可以共享跨越两个或多个实例的同一事务。  
  
 分布式绑定会话不是用字符串绑定令牌标识，而是用分布式事务标识号标识。 如果本地事务中涉及到绑定会话，且该会话在远程服务器上使用 `SET REMOTE_PROC_TRANSACTIONS ON` 执行 RPC，MS DTC 将该本地绑定事务自动提升为分布式绑定事务，并且 MS DTC 会话也会启动。  
  
#### <a name="when-to-use-bound-sessions"></a>何时使用绑定会话  
 在早期版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中，绑定会话主要用于开发必须执行 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句（代表调用它们的进程）的扩展存储过程。 让调用进程在绑定令牌中作为扩展存储过程的一个参数进行传递，可使该过程加入到调用进程的事务空间中，从而将扩展存储过程与该调用进程结合在一起。  
  
 在 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]中，使用 CLR 编写的存储过程比扩展存储过程更安全、具有更高的伸缩性并且更稳定。 CLR 存储过程使用 SqlContext 对象（而非 `sp_bindsession`）联接调用会话的上下文。  
  
 绑定会话可以用来开发三层应用程序，在这些应用程序中，业务逻辑合并到在单个业务事务上协同工作的单独程序中。 必须对这些程序进行编码，以仔细协调它们对数据库的访问。 由于两个会话共享同一个锁，因此两个程序不得同时修改同一数据。 在任何时间点，事务中只能有一个会话在执行，不存在并行执行操作。 只能在定义完善的时间点于会话之间切换事务，例如，已完成所有 DML 语句且已检索其结果时。  
  
### <a name="coding-efficient-transactions"></a>编写有效的事务  
 尽可能使事务保持简短很重要。 当事务启动后，数据库管理系统 (DBMS) 必须在事务结束之前保留很多资源，以保护事务的原子性、一致性、隔离性和持久性 (ACID) 属性。 如果修改数据，则必须用排他锁保护修改过的行，以防止任何其他事务读取这些行，并且必须将排他锁控制到提交或回滚事务时为止。 根据事务隔离级别设置，`SELECT` 语句可以获取必须控制到提交或回滚事务时为止的锁。 特别是在有很多用户的系统中，必须尽可能使事务保持简短以减少并发连接间的资源锁定争夺。 在有少量用户的系统中，运行时间长、效率低的事务可能不会成为问题，但是在有上千个用户的系统中，将不能忍受这样的事务。 从 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 开始支持延迟持久事务。 延迟持久事务并不保证持续性。 有关详细信息，请参阅主题[事务持续性](../relational-databases/logs/control-transaction-durability.md)。  
  
#### <a name="coding-guidelines"></a><a name="guidelines"></a>编码指导原则  
 以下是编写有效事务的指导原则：  
  
-   不要在事务处理期间要求用户输入。  
    在事务启动之前，获得所有需要的用户输入。 如果在事务处理期间还需要其他用户输入，则回滚当前事务，并在提供了用户输入之后重新启动该事务。 即使用户立即响应，作为人，其反应时间也要比计算机慢得多。 事务占用的所有资源都要保留相当长的时间，这有可能会造成阻塞问题。 如果用户没有响应，事务仍然会保持活动状态，从而锁定关键资源直到用户响应为止，但是用户可能会几分钟甚至几个小时都不响应。  
  
-   在浏览数据时，尽量不要打开事务。  
    在所有预备的数据分析完成之前，不应启动事务。  
  
-   尽可能使事务保持简短。  
    在知道要进行的修改之后，启动事务，执行修改语句，然后立即提交或回滚。 只有在需要时才打开事务。  
  
-   若要减少阻塞，请考虑针对只读查询使用基于行版本控制的隔离级别。  
  
-   灵活地使用更低的事务隔离级别。  
    可以很容易地编写出许多使用只读事务隔离级别的应用程序。 并不是所有事务都要求可序列化的事务隔离级别。  
  
-   灵活地使用更低的游标并发选项，例如开放式并发选项。  
    在并发更新的可能性很小的系统中，处理“别人在您读取数据后更改了数据”的偶然错误的开销要比在读取数据时始终锁定行的开销小得多。  
  
-   在事务中尽量使访问的数据量最小。  
    这样可以减少锁定的行数，从而减少事务之间的争夺。  
    
-   尽可能避免使用悲观锁定提示（如 holdlock）。 
    诸如 HOLDLOCK 或 SERIALIZABLE 隔离级别之类的提示可能会导致进程即使在获取共享锁时也要等待，并且会降低并发性

-   尽可能避免使用隐式事务。隐式事务会因其性质而导致不可预知的行为。 请参阅[隐式事务和并发问题](#implicit-transactions-and-avoiding-concurrency-and-resource-problems)

-   使用缩减的[填充因子](indexes/specify-fill-factor-for-an-index.md)设计索引。缩减填充因子可能有助于避免或减少索引页碎片，从而减少索引搜寻时间，尤其是从磁盘检索时。 若要查看表或视图的数据和索引的碎片信息，可以使用 sys.dm_db_index_physical_stats。 
  
#### <a name="implicit-transactions-and-avoiding-concurrency-and-resource-problems"></a>隐式事务以及避免并发问题和资源问题  
 为了防止并发问题和资源问题，应小心管理隐式事务。 使用隐式事务时，`COMMIT` 或 `ROLLBACK` 后的下一个 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句会自动启动一个新事务。 这可能会在应用程序浏览数据时（甚至在需要用户输入时）打开一个新事务。 在完成保护数据修改所需的最后一个事务之后，应关闭隐性事务，直到再次需要使用事务来保护数据修改。 此过程使 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 能够在应用程序浏览数据以及获取用户输入时使用自动提交模式。  
  
 另外，启用快照隔离级别后，尽管新事务不会控制锁，但是长时间运行的事务将阻止从 `tempdb`中删除旧版本。  
  
### <a name="managing-long-running-transactions"></a>管理长时间运行的事务  
 “长时间运行的事务”是一个未及时提交或回滚事务的活动事务。 例如，如果事务的开始和结束由用户控制，则导致长时间运行事务的一般原因是用户在开始事务之后便离开，而事务等待用户的响应。  
  
 长时间运行的事务可能导致数据库的严重问题，如下所示：  
  
-   如果服务器实例在活动事务已执行很多未提交的修改后关闭，后续重新启动的恢复阶段持续时间将远远多于恢复间隔服务器配置选项或 `ALTER DATABASE ... SET TARGET_RECOVERY_TIME` 选项指定的时间。 这些选项分别控制活动检查点和间接检查点的频率。 有关检查点类型的详细信息，请参阅[数据库检查点 (SQL Server)](../relational-databases/logs/database-checkpoints-sql-server.md)。  
  
-   更重要的是，尽管等待事务可能生成很小的日志，但是它无限期阻止日志截断，导致事务日志不断增加并可能填满。 如果事务日志填满，数据库将无法再执行任何更新。 有关详细信息，请参阅 [SQL Server 事务日志体系结构和管理指南](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)、[解决事务日志已满的问题（SQL Server 错误 9002）](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)和[事务日志 (SQL Server)](../relational-databases/logs/the-transaction-log-sql-server.md)。  
  
#### <a name="discovering-long-running-transactions"></a>发现长时间运行的事务  
 若要查看长时间运行的事务，请使用下列方法之一：  
  
-   **sys.dm_tran_database_transactions**  
  
    此动态管理视图返回有关数据库级事务的信息。 对于长时间运行的事务，最需要注意的列包括：第一条日志记录的时间 (database_transaction_begin_time)、事务的当前状态 (database_transaction_state) 和事务日志中开始记录的日志序列号 (LSN) (database_transaction_begin_lsn)  。  
  
    有关详细信息，请参阅 [sys.dm_tran_database_transactions (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)。  
  
-   `DBCC OPENTRAN`  
  
    通过此语句，您可以标识该事务所有者的用户 ID，因此可以隐性地跟踪该事务的源以得到更加有序的终止（将其提交而非回滚）。 有关详细信息，请参阅 [DBCC OPENTRAN (Transact-SQL)](../t-sql/database-console-commands/dbcc-opentran-transact-sql.md)。  
  
#### <a name="stopping-a-transaction"></a>停止事务  
 您可能必须使用 KILL 语句。 但是，在使用此语句时请务必小心，特别是在运行重要的进程时。 有关详细信息，请参阅 [KILL (Transact-SQL)](../t-sql/language-elements/kill-transact-sql.md)。  
  
##  <a name="additional-reading"></a><a name="Additional_Reading"></a> 其他阅读主题   
[行版本控制的系统开销](/archive/blogs/sqlserverstorageengine/overhead-of-row-versioning)   
[扩展事件](../relational-databases/extended-events/extended-events.md)   
[sys.dm_tran_locks (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)     
[动态管理视图和函数 (Transact-SQL)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)      
[与事务相关的动态管理视图和函数 (Transact-SQL)](../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)