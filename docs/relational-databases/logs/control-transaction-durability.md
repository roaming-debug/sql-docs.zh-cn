---
title: 控制事务持久性 | Microsoft Docs
description: 了解延迟持久事务。 SQL Server 事务提交可以是完全持久或延迟持久（也称作迟缓提交）。
ms.custom: ''
ms.date: 09/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- delayed durability
- Lazy Commit
ms.assetid: 3ac93b28-cac7-483e-a8ab-ac44e1cc1c76
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1333d56acfb81a7f06a688520558fb15a59dc71d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473838"
---
# <a name="control-transaction-durability"></a>控制事务持续性
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事务提交可以是完全持久、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 默认设置或延迟的持久（也称作迟缓提交）。    
    
 完全持久事务提交是同步的，仅在事务的日志记录写入磁盘后报告提交成功，并将控制权归还客户端。 延迟持久事务提交是异步的，并在事务的日志记录写入磁盘之前报告提交成功。 事务要成为持久事务，必须将事务日志条目写入磁盘。 当事务日志条目刷新到磁盘时，延迟持久事务成为持久事务。    
    
 本主题详细说明延迟持久事务。    
    
## <a name="full-vs-delayed-transaction-durability"></a>完全与延迟事务持续性    
 完全和延迟事务持续性各有其优缺点。 一个应用程序可能同时包含完全和延迟持久事务。 您应该仔细考虑业务需求以及每个应用程序如何满足这些需求。    
    
### <a name="full-transaction-durability"></a>完全事务持续性    
 完全持久事务在将控制权归还给客户端之前将事务日志写入磁盘。 只要存在以下情况，就应使用完全持久事务：    
    
-   系统无法承受任何数据丢失。     
    有关可能在何种情况下会丢失一些数据的信息，请参阅 [在什么情况下会丢失数据？](../../relational-databases/logs/control-transaction-durability.md#bkmk_DataLoss) 部分。    
    
-   造成瓶颈的原因不是事务日志写入延迟。    
    
 通过在内存中保留事务日志记录并批量写入事务日志，延迟事务持续性可以缩短延迟，因而减少了所需的 I/O 操作。 延迟事务持续性可能会减少日志 I/O 争用，从而减少系统中的等待。    
    
 **完全事务持续性保证**    
    
-   事务提交成功后，该事务所做的更改就对系统中的其他事务可见。 有关事务隔离级别的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md) 或 [具有内存优化表的事务](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)。    
    
-   提交并不保证持续性。 在事务提交成功并将控制权归还给客户端之前，对应的日志记录会保存到磁盘。    
    
### <a name="delayed-transaction-durability"></a>延迟事务持续性    
 延迟事务持续性使用向磁盘的异步日志写入来实现。 事务日志记录保留在缓冲区中并在缓冲区充满或发生缓冲区刷新事件时写入磁盘。 延迟事务持续性可能会减少系统中的延迟和争用，因为：    
    
-   事务提交处理不会等待日志 IO 完成就将控制权归还给客户端。    
    
-   并发事务争用日志 IO 的可能性更小；日志缓冲区现在可以更大的区块刷新到磁盘，从而减少争用和提高吞吐量。    
    
    > [!NOTE]    
    >  如果并发度很高，特别是如果填充日志缓冲区的速度比刷新缓冲区的速度快，仍然可能发生日志 I/O 争用。    
    
 #### <a name="when-to-use-delayed-transaction-durability"></a>何时使用延迟事务持续性    
    
 适合使用延迟事务持续性的部分情况如下：    
    
 **可容忍丢失部分数据。**     
 如果可以容忍一定的数据丢失，例如只要有大部分数据即可，个别记录不是非常重要，就值得考虑延迟持续性。 如果无法容忍任何数据丢失，则不要使用延迟事务持续性。    
    
 **在事务日志写入时遭遇瓶颈。**     
 如果性能问题是由于事务日志写入延迟造成的，则应用程序可能适合使用延迟事务持续性。    
    
 **工作负荷的争用率很高。**     
 如果系统工作负载争用级别很高，则会花费大量时间等待锁释放。 延迟事务持续性会缩短提交时间，因此能够更快地释放锁，从而实现更大的吞吐量。    
    
 ### <a name="delayed-transaction-durability-guarantees"></a>延迟事务持续性保证   
    
-   事务提交成功后，该事务所做的更改就对系统中的其他事务可见。    
    
-   事务持续性只能通过将内存中事务日志刷新到磁盘来保证。 内存中事务日志在以下情况下刷新到磁盘：    
    
    -   同一个数据库中完全持久的事务在数据库中做出更改并成功提交。   
    
    -   用户成功执行系统存储过程 `sp_flush_log` 。     
    
        如果成功提交完全持久的事务或 sp_flush_log，则保证之前提交的所有延迟持续性事务都已成为持久事务。
        
    - 即使所有事务都是延迟持久事务，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 也会尝试基于日志生成和计时将日志刷新到磁盘。 如果 IO 设备保持正常运行，此操作通常可以成功。 但是， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不提供除持久事务和 sp_flush_log 以外的任何有力的持续性保证。      
    
  
    
## <a name="how-to-control-transaction-durability"></a>如何控制事务持续性    
    
###  <a name="database-level-control"></a><a name="bkmk_DbControl"></a> 数据库级别控制    
 您作为 DBA，可以控制用户是否可通过以下语句对数据库使用延迟事务持续性。 您必须使用 ALTER DATABASE 来设置延迟持续性设置。    
    
```sql    
ALTER DATABASE ... SET DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }    
```    
    
 **DISABLED**    
 [默认] 使用此设置时，不管提交级别设置如何 (DELAYED_DURABILITY=[ON | OFF])，对数据库提交的所有事务都是完全持久事务。 无需更改和重新编译存储过程。 这样能确保任何数据都不会因延迟持续性面临风险。    
    
 **ALLOWED**    
 使用此设置时，每个事务的持续性都在事务级别确定 - DELAYED_DURABILITY = { OFF | ON }。 有关详细信息，请参阅 [原子块级别控制 - 本机编译存储过程](../../relational-databases/logs/control-transaction-durability.md#CompiledProcControl)和[提交级别控制 – Transact-SQL](../../relational-databases/logs/control-transaction-durability.md#bkmk_T-SQLControl)。    
    
 **FORCED**    
 使用此设置，对数据库提交的每个事务都是延迟持久事务。 无论事务指定完全持久 (DELAYED_DURABILITY = OFF) 还是不进行任何指定，事务都是延迟持久事务。 当数据库适合使用延迟事务持续性，并且您不希望更改任何应用程序代码时，此设置很有用。    
    
###  <a name="atomic-block-level-control---natively-compiled-stored-procedures"></a><a name="CompiledProcControl"></a> 原子块级别控制 - 本机编译存储过程    
 下面的代码面向原子块内部。    
    
```sql    
DELAYED_DURABILITY = { OFF | ON }    
```    
    
 **OFF**    
 [默认] 事务是完全持久事务，除非数据库选项 DELAYED_DURABLITY = FORCED 有效（在这种情况下，提交是异步的，因而是延迟持久事务）。 有关详细信息，请参阅 [Database level control](../../relational-databases/logs/control-transaction-durability.md#bkmk_DbControl) 。    
    
 **ON**    
 事务是延迟持久事务，除非数据库选项 DELAYED_DURABLITY = DISABLED 有效（在这种情况下，提交是同步的，因而是完全持久事务）。  有关详细信息，请参阅 [Database level control](../../relational-databases/logs/control-transaction-durability.md#bkmk_DbControl) 。    
    
 **示例代码：**    
    
```sql    
CREATE PROCEDURE <procedureName> ...    
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER    
AS BEGIN ATOMIC WITH     
(    
    DELAYED_DURABILITY = ON,    
    TRANSACTION ISOLATION LEVEL = SNAPSHOT,    
    LANGUAGE = N'English'    
    ...    
)    
END    
```    
    
### <a name="table-1-durability-in-atomic-blocks"></a>表 1：原子块中的持续性    
    
|原子块持续性选项|没有现有事务|事务正在进行（完全或延迟持久）|    
|------------------------------------|-----------------------------|---------------------------------------------------------|    
|**DELAYED_DURABILITY = OFF**|原子块启动新的完全持久事务。|原子块在现有事务中创建一个保存点，然后开始新事务。|    
|**DELAYED_DURABILITY = ON**|原子块启动新的延迟持久事务。|原子块在现有事务中创建一个保存点，然后开始新事务。|    
    
###  <a name="commit-level-control--tsql"></a><a name="bkmk_T-SQLControl"></a> 提交级别控制 -[!INCLUDE[tsql](../../includes/tsql-md.md)]    
 COMMIT 语法已扩展，您可以强制实施延迟事务持续性。 如果 DELAYED_DURABILITY 在数据库级别设置为 DISABLED 或 FORCED（请参阅上文），则忽略此 COMMIT 选项。    
    
```sql    
COMMIT [ { TRAN | TRANSACTION } ] [ transaction_name | @tran_name_variable ] ] [ WITH ( DELAYED_DURABILITY = { OFF | ON } ) ]    
    
```    
    
 **OFF**    
 [默认] 事务 COMMIT 是完全持久事务，除非数据库选项 DELAYED_DURABLITY = FORCED 有效（在这种情况下，提交是异步的，因而是延迟持久事务）。 有关详细信息，请参阅 [Database level control](../../relational-databases/logs/control-transaction-durability.md#bkmk_DbControl) 。    
    
 **ON**    
 事务 COMMIT 是延迟持久事务，除非数据库选项 DELAYED_DURABLITY = DISABLED 有效（在这种情况下，提交是同步的，因而是完全持久事务）。 有关详细信息，请参阅 [Database level control](../../relational-databases/logs/control-transaction-durability.md#bkmk_DbControl) 。    
    
### <a name="summary-of-options-and-their-interactions"></a>各个选项及其交互的总结    
 此表总结了数据库级别延迟持续性设置与提交级别设置之间的交互。 数据库级别设置始终优先于提交级别设置。    
    
|提交设置/数据库设置|DELAYED_DURABILITY = DISABLED|DELAYED_DURABILITY = ALLOWED|DELAYED_DURABILITY = FORCED|    
|--------------------------------------|-------------------------------------|------------------------------------|-----------------------------------|    
|**DELAYED_DURABILITY = OFF** 数据库级别事务。|事务是完全持久事务。|事务是完全持久事务。|事务是延迟持久事务。|    
|**DELAYED_DURABILITY = ON** 数据库级别事务。|事务是完全持久事务。|事务是延迟持久事务。|事务是延迟持久事务。|    
|**DELAYED_DURABILITY = OFF** 跨数据库或分布式事务。|事务是完全持久事务。|事务是完全持久事务。|事务是完全持久事务。|    
|**DELAYED_DURABILITY = ON** 跨数据库或分布式事务。|事务是完全持久事务。|事务是完全持久事务。|事务是完全持久事务。|    
    
## <a name="how-to-force-a-transaction-log-flush"></a>如何强制执行事务日志刷新    
 有两种方法可以强制将事务日志刷新到磁盘。    
    
-   执行任何可改变相应数据库的完全持久事务。 这会强制将之前提交的所有延迟持续性事务的日志记录刷新到磁盘。    
    
-   执行系统存储过程 `sp_flush_log`。 此过程会强制将之前提交的所有延迟持久事务的日志记录刷新到磁盘。 有关详细信息，请参阅 [sys.sp_flush_log (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-flush-log-transact-sql.md)。    
    
##  <a name="delayed-durability-and-other-ssnoversion-features"></a><a name="bkmk_OtherSQLFeatures"></a> 延迟持续性和其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能    
 **更改跟踪和更改数据捕获**    
 具有更改跟踪属性的所有事务都是完全持久事务。 如果一个事务对支持更改跟踪的表执行了任何写入操作，则该事务具有更改跟踪属性。 使用变更数据捕获 (CDC) 的数据库不支持使用延迟的持续性。    
    
 **崩溃恢复**    
 一致性可得到保证，但已提交的延迟持久事务的一些更改可能会丢失。    
    
 **跨数据库和 DTC**    
 如果事务跨数据库或是分布式事务，则无论数据库或事务提交设置如何，它都是完全持久事务。    
    
 **AlwaysOn 可用性组和镜像**    
 延迟持久事务并不能保证主数据库或任何辅助数据库的持续性。 此外，它们也不保证了解辅助数据库的事务。 提交后，在从同步辅助数据接收到任何确认之前，控制权就会归还客户端。 当主副本上的磁盘刷新时，会继续复制辅助副本。   
    
 **故障转移群集**    
 某些延迟持久事务写入可能会丢失。    
    
 **事务复制**    
 事务复制不支持延迟持久事务。    
    
 **日志传送**    
 传送的日志中仅包含已成为持久事务的事务。    
    
 **日志备份**    
 备份中仅包含已成为持久事务的事务。    
    
##  <a name="when-can-i-lose-data"></a><a name="bkmk_DataLoss"></a> 在什么情况下会丢失数据？    
 如果你对表实施延迟持续性，则应了解某些情况会导致数据丢失。 如果无法容忍任何数据丢失，则不要对表使用延迟持续性。    
    
### <a name="catastrophic-events"></a>灾难性事件    
 发生灾难性事件（如服务器崩溃）时，将丢失已提交但未保存到磁盘的所有事务的数据。 根据数据库中的任何表（持久内存优化或基于磁盘）执行完全持久的事务时，或调用 `sp_flush_log` 时，延迟的持久事务保存到磁盘。 如果你在使用延迟的持久事务，那么你可能想要在数据库中创建一个小型表，你可定期更新该表或调用 `sp_flush_log` ，以保存所有未完成的已提交事务。 事务日志还会在变满时刷新，但这难以预测，也无法进行控制。    
    
### <a name="ssnoversion-shutdown-and-restart"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关闭和重启    
 对于延迟的持久性， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的意外关闭和预期关闭/重新启动没有区别。 与灾难性事件类似，应制定针对数据丢失的计划。 在进行计划的关闭/重新启动时，一些尚未写入磁盘的事务可能会首先保存到磁盘，但不应对其进行计划。 虽然计划了关闭/重启，但无论是否计划，都会像灾难性事件一样丢失数据。    
    
## <a name="see-also"></a>另请参阅    
 [具有内存优化表的事务](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)    
    
  
