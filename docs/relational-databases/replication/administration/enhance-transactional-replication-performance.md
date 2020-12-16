---
title: 增强事务复制性能 | Microsoft Docs
description: 除了 SQL Server 中提高复制性能的常规性能提示外，还可了解有关事务复制的其他技术。
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], design and performance
- performance [SQL Server replication], transactional replication
- designing databases [SQL Server], replication performance
- performance [SQL Server replication], snapshot replication
- snapshot replication [SQL Server], performance
- subscriptions [SQL Server replication], performance considerations
- agents [SQL Server replication], performance
- Distribution Agent, performance
- transactional replication, performance
- Log Reader Agent, performance
ms.assetid: 67084a67-43ff-4065-987a-3b16d1841565
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 3aa0d62d275f3c92a6a21aba4c60b083e004e10b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467438"
---
# <a name="enhance-transactional-replication-performance"></a>增强事务复制性能
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  在考虑 [增强常规复制性能](../../../relational-databases/replication/administration/enhance-general-replication-performance.md)中介绍的常规性能提示后，还需要考虑特定于事务复制的其他几个方面。  
  
## <a name="database-design"></a>数据库设计  
  
-   在应用程序设计中，最大限度地减小事务大小。  
  
     默认情况下，事务复制根据事务边界传播更改。 如果事务越小，则分发代理因网络问题而重新发送事务的可能性就越小。 如果重新发送事务需要代理，则发送的数据量会较小。 

  
## <a name="distributor-configuration"></a>分发服务器配置  
  
-   在专用服务器上配置分发服务器。  
  
     通过配置远程分发服务器可以减少发布服务器的处理开销。 有关详细信息，请参阅 [Configure Distribution](../../../relational-databases/replication/configure-distribution.md)。  
  
-   适当调整分发数据库的大小。  
  
     在系统承受典型负载的情况下对复制进行测试，以确定存储命令需要多少空间。 确保数据库足以存储命令，而无需频繁地自动增大。 有关更改数据库大小的详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql.md)。  
  
## <a name="publication-design"></a>发布设计  
  
-   在对已发布的表进行批更新时，复制存储过程执行。  
  
     如果所做的批处理更新有时会影响订阅服务器上的大量行，则应考虑使用存储过程更新已发布的表，并发布存储过程的执行。 分发代理不是为每个受影响的行都发送一个更新或删除，而是在订阅服务器上用相同的参数值执行相同的过程。 有关详细信息，请参阅 [Publishing Stored Procedure Execution in Transactional Replication](../../../relational-databases/replication/transactional/publishing-stored-procedure-execution-in-transactional-replication.md)。  
  
-   将项目分布在多个发布上。  
  
     如果不能使用 [-SubscriptionStreams 参数](#subscriptionstreams)，请考虑创建多个发布。 将项目分布在这些发布上使复制可以并行地将更改应用到订阅服务器。  
  
## <a name="subscription-considerations"></a>订阅注意事项  
  
-   如果在相同的发布服务器中拥有多个发布（这是新建发布向导中的默认情况），则使用独立代理而非共享代理。  
  
-   连续运行而不是按频繁的计划运行代理。  
  
     将代理设置为连续运行而不是创建频繁的计划（如每分钟）会提高复制性能，因为代理不必启动和停止。 将分发代理设置为连续运行后，以低滞后时间将更改传播到拓扑中连接的其他服务器。 有关详细信息，请参阅：  
  
    -   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]：[指定同步计划](../../../relational-databases/replication/specify-synchronization-schedules.md)  
  
## <a name="distribution-agent-and-log-reader-agent-parameters"></a>分发代理和日志读取器代理参数  
代理配置文件参数经常调整，以增加具有高流量 OLTP 系统的日志读取器和分发代理的吞吐量。 

执行测试来确定最佳值，以提升日志读取器和分发代理的性能。 此测试得出的结论是，工作负载是在不同的情况下使用不同的值的决定性因素，因此，没有可以针对每种情况提升其性能的单个值调整。 

结果： 
- 对于具有少量事务（少于 500 个命令）的工作负载的日志读取器代理，较高的 ReadBatchSize 值可能提升吞吐量。 但是，对于具有大量事务的工作负载来说，更改此值将不会提升性能。 
    - 当有多个日志读取器代理和多个分发代理以并行方式在同一服务器上运行时，较高的 ReadBatchSize 值将导致分发数据库的争用。 
- 对于分发代理
    - 增加 CommitBatchSize 可改善吞吐量。 需要权衡的问题是，如果发生失败，分发代理必须回滚并重新开始，以重新应用更大数量的事务。 
    - 增加 SubscriptionStreams 值的确对分发代理的总吞吐量有所帮助，因为订阅服务器的多个连接将以并行方式应用批量更改。 但是，根据处理器的数量和其他元数据条件（例如主键、外键、唯一约束和索引），较高的 SubscriptionStreams 值实际上可能会产生负面影响。 此外，如果流未能执行或提交，则分发代理将回退到使用单个流来重试失败的批处理。


有关此测试的详细信息，请参阅博客[优化复制代理配置文件参数以获得更好的性能](/archive/blogs/sql_server_team/optimizing-replication-agent-profile-parameters-for-better-performance)。


### <a name="log-reader-agent"></a>日志读取器代理

#### <a name="readbatchsize"></a>ReadBatchSize
- 增大日志读取器代理的 **-ReadBatchSize** 参数的值。  
  
日志读取器代理和分发代理支持事务读取和提交操作的批大小。 批大小的默认值为 500 个事务。 日志读取器代理从日志中读取特定数量的事务，而不管这些事务是否标记为复制。 将大量事务写入发布数据库，而其中只有一小部分标记为复制时，则应使用 -ReadBatchSize 参数增加日志读取器代理的读取批大小。 此参数不适用于 Oracle 发布服务器。  

   - 当 ReadBatchSize 增至 5000 时，少量事务（少于 500 个命令）的工作负载每秒处理命令的数量将增加。 
   - 对于较大数量的工作负载（具有 500 到 1000 个命令的事务），增加 ReadBatchSize 对性能改善不大。 增加 ReadBatchSize 将导致更大数量的事务一次性写入到分发数据库。 这会增加时间事务，且命令对分发代理是可见的，并导致复制过程的延迟。  

#### <a name="pollinginterval"></a>PollingInterval
- 减小日志读取器代理的 **-PollingInterval** 参数的值。  
  
**-PollingInterval** 参数指定为要复制的事务查询已发布数据库的事务日志的频率。 默认值为 5 秒。 如果减小此值，日志的轮询将更加频繁，这会使事务从发布数据库传递到分发数据库的滞后时间较低。 但是，应该对较低滞后时间的需要和因频繁轮询导致的服务器上增加的负荷之间进行平衡。   
  
#### <a name="maxcmdsintran"></a>MaxCmdsInTran
- 要解决意外、一次性瓶颈，请使用日志读取器代理的 **–MaxCmdsInTran** 参数。  
  
**–MaxCmdsInTran** 参数指定日志读取器向分发数据库写入命令时组成一个事务的语句的最大数量。 使用此参数能够使日志读取器代理和分发代理在订阅服务器上应用命令时将发布服务器上的大事务（由许多命令组成）分成若干个较小的事务。 指定此参数可以减少分发服务器的争用问题并缩短发布服务器与订阅服务器之间的滞后时间。 由于初始事务是以较小的单元应用的，订阅服务器可以在初始事务结束之前访问一个较大的逻辑发布服务器事务的行，因而会破坏事务的原子性。 默认值为 0 ，这将保持发布服务器的事务边界。 此参数不适用于 Oracle 发布服务器。  
  
   > [!WARNING]  
   >  **MaxCmdsInTran** 并非始终开启。 其之所以存在，是为了解决某人在单个事务中意外执行了大量 DML 操作（在整个事务进入分发数据库并持有锁之前，导致命令分发延迟等）的问题。 如果你经常遇到这种情况，请审查应用程序并找到减少事务大小的方法。  
  
### <a name="distribution-agent"></a>分发代理

#### <a name="subscriptionstreams"></a>SubscriptionStreams
- 为分发代理增加 –SubscriptionStreams 参数。  
  
**–SubscriptionStreams** 参数可以显著提高聚合复制吞吐量。 它使到一台订阅服务器的多个连接可以并行应用批量更改，同时在使用单线程时保持多个事务特征的存在。 如果有一个连接无法执行或提交，则所有连接将中止当前批处理，而且代理将用单独的流重试失败的批处理。 在重试阶段完成之前，订阅服务器上会存在临时事务不一致。 失败的批处理成功提交后，订阅服务器将恢复到事务一致状态。  
  
可以使用 [sp_addsubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) 的 `@subscriptionstreams` 来指定此代理参数的值。  

有关实施订阅流的详细信息，请参阅[导航 SQL 复制 subscriptionStream 设置](https://blogs.msdn.microsoft.com/repltalk/2010/03/01/navigating-sql-replication-subscriptionstreams-setting)。
  
### <a name="blocking-monitor-thread"></a>阻止监视器线程

分发代理维护阻止监视器线程，该线程检测会话间的阻止。 如果阻止监视器线程检测到会话间的阻止，分发代理会进行切换，以使用一个会话来重新应用之前不能应用的当前命令批处理。

阻止监视器线程可以检测到分发代理会话之间的阻止。 但是，在以下情况下阻止监视器线程无法检测到阻止：
- 发生阻止的会话不是分发代理会话。
- 会话死锁冻结分发代理的活动。

在此情况下，分发代理会协调所有会话，一旦执行命令就立即全部提交。 如果以下条件成立，会话会发生死锁：

- 分发代理会话和非分发代理会话之间发生阻止。
- 分发代理正在等待所有会话完成命令执行，此后，分发代理会协调所有会话以一同提交。

例如，将 SubscriptionStreams 参数配置为 8。 会话 10 至会话 17 是分发代理会话。 会话 18 不是分发代理会话。 会话 18 阻止会话 10，会话 11 阻止会话 18。 此外，会话 10 和会话 11 必须一同提交。 但是，由于阻止，分发代理无法一同提交会话 10 和会话 11。 因此，分发代理无法协调这八个会话以实现一同提交，直到会话 10 和会话 11 完成命令执行。

此示例导致没有会话正在执行命令的状态。 达到 QueryTimeout 属性中指定的时间时，分发代理会取消所有会话。

> [!Note]
> 默认情况下，QueryTimeout 属性的值为 5 分钟。

你可能会注意到在此查询超时期限内，分发代理性能计数器的以下趋势： 

- Dist: Delivered Trans/sec 性能计数器的值始终是 0。
- Dist: Delivered Trans/sec 性能计数器的值始终是 0。
- Dist: Delivery Latency 性能计数器报告值增加，直到解决线程死锁问题。

SQL Server 联机丛书中的“复制分发代理”主题包含对以下 SubscriptionStreams 参数的说明：“如果有一个连接无法执行或提交，则所有连接将中止当前批处理，而且代理将用单独的流重试失败的批处理。”

分发代理使用一个会话来重试之前不能应用的批处理。 分发代理成功应用批处理后，将继续使用多个会话，无需重新启动。

#### <a name="commitbatchsize"></a>CommitBatchSize
- 增大分发代理的 **-CommitBatchSize** 参数的值。  
  
提交一组事务的开销是固定的；通过不经常提交大量事务，就可以将开销分散在大量数据上。  增加 CommitBatchSize（最多 200）可提升性能，因为将向订阅服务器提交更多事务。 但是，增大此参数的优势因应用更改的开销由其他因素（例如包含日志的最大磁盘 I/O）限制而降低。 另外，需要考虑以下权衡问题：必须回滚任何导致分发代理重新开始的失败并重新应用大量事务。 对于不可靠的网络，越小的值导致失败的几率越小，如果导致失败，将回滚并重新应用较少量事务。  
  

## <a name="see-more"></a>查看详细信息
  
[处理复制代理配置文件](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
[查看和修改复制代理命令提示符参数 (SQL Server Management Studio)](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
[Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
