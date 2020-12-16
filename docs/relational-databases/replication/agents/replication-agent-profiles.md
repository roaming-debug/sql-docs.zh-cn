---
description: 复制代理配置文件
title: 复制代理配置文件 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Distribution Agent, profiles
- replication [SQL Server], agents and profiles
- replication agent profiles [SQL Server]
- Merge Agent, profiles
- agents [SQL Server replication], profiles
- Queue Reader Agent, profiles
- profiles [SQL Server], replication agents
- Snapshot Agent, profiles
- Log Reader Agent, profiles
ms.assetid: 0e980725-e42f-4283-94cb-d8a6dba5df62
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: eaf55751313b9bc8cfa7a39808efc26d5980c449
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475898"
---
# <a name="replication-agent-profiles"></a>复制代理配置文件
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  在配置复制时，将在分发服务器上安装一组代理配置文件。 代理配置文件包含一组在代理每次运行时都要使用的参数：在代理启动过程中，每个代理都会登录到分发服务器，并查询其配置文件中的参数。 对于使用 Web 同步的合并订阅，配置文件会下载并存储在订阅服务器中。 如果配置文件发生了更改，订阅服务器中的配置文件将在合并代理下次运行时更新。 有关 Web 同步的详细信息，请参阅 [Web Synchronization for Merge Replication](../../../relational-databases/replication/web-synchronization-for-merge-replication.md)。  
  
 复制为每个代理提供一个默认的配置文件，同时还为日志读取器代理、分发代理和合并代理提供附加的预定义配置文件。 除了所提供的配置文件之外，您还可以创建符合自己应用程序要求的配置文件。 使用代理配置文件可以轻松地更改与该配置文件关联的所有代理的键参数。 例如，如果您有 20 个快照代理并且需要更改查询超时值（ **-QueryTimeout** 参数），则可以更新这些快照代理使用的配置文件，这样该此类型的所有代理将在下次运行时自动开始使用新值。  
  
 您也可能对代理的不同实例拥有不同的配置文件。 例如，以拨号连接方式连接到发布服务器和分发服务器的合并代理可以通过使用“慢速链接”  配置文件而采用一组更适合慢速通信链接的参数。  
  
> [!NOTE]  
>  如果在命令行中为代理参数指定了值，则该值将覆盖代理配置文件中为同一参数设置的值。  
  
 **使用和修改代理配置文件**  
  
-   [处理复制代理配置文件](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
## <a name="snapshot-agent-profiles"></a>快照代理配置文件  
 下表显示了快照代理的默认配置文件中定义的参数。 有关这些参数的详细信息，请参阅 [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)。  
  
||默认值|  
|-|-------------|  
|**-BcpBatchSize**|100000|  
|**-HistoryVerboseLevel**|2|  
|**-LoginTimeout**|15|  
|**-QueryTimeout**|1800|  
  
## <a name="log-reader-agent-profiles"></a>日志读取器代理配置文件  
 下表显示了日志读取器代理的配置文件中定义的参数。 表中的每一列都表示一个已命名的配置文件。 有关这些参数的详细信息，请参阅 [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)。  
  
||默认值|详细历史记录|  
|-|-------------|---------------------|  
|**-HistoryVerboseLevel**|1|2|  
|**-LoginTimeout**|15|15|  
|**-LogScanThreshold**|500000|500000|  
|**-PollingInterval**|5|5|  
|**-QueryTimeout**|1800|1800|  
|**-ReadBatchSize**|500|500|  
  
## <a name="distribution-agent-profiles"></a>分发代理配置文件  
 下表显示了分发代理的配置文件中定义的参数。 表中的每一列都表示一个已命名的配置文件。 有关这些参数的详细信息，请参阅 [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)。  
  
||默认值|详细历史记录|Windows 同步管理器|出现数据一致性错误时继续|OLEDB 流的分发配置文件|  
|-|-------------|---------------------|-------------------------------------|-----------------------------------------|----------------------------------------------|  
|**-BcpBatchSize**|100000|100000|1000|100000|2147473647|  
|**-CommitBatchSize**|100|100|100|100|100|  
|**-CommitBatchThreshold**|1000|1000|1000|1000|1000|  
|**-HistoryVerboseLevel**|1|2|1|1|1|  
|**-KeepAliveMessageInterval**|300|300|300|300|300|  
|**-LoginTimeout**|15|15|15|15|15|  
|**-MaxBcpThreads**|1|1|1|1|1|  
|**-MaxDeliveredTransactions**|0|0|0|0|0|  
|**-OledbStreamThreshold**|Null|Null|Null|Null|32768|  
|**-PacketSize**|Null|Null|Null|Null|32768|  
|**-PollingInterval**|5|5|5|5|5|  
|**-QueryTimeout**|1800|1800|1800|1800|1800|  
|**-SkipErrors**|Null|Null|Null|**-SkipErrors** 2601:2627:20598|Null|  
|**-TransactionsPerHistory**|100|100|100|100|100|  
|**-UseOledbStreaming**|Null|Null|Null|Null|**-UseOledbStreaming**|  
  
## <a name="merge-agent-profiles"></a>合并代理配置文件  
 下表显示了合并代理的配置文件中定义的参数。 表中的每一列都表示一个已命名的配置文件。 有关这些参数的详细信息，请参阅 [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)。  
  
||默认值|详细历史记录|Windows 同步管理器|行计数验证|行计数和校验和验证|慢速链接|高卷服务器对服务器|  
|-|-------------|---------------------|-------------------------------------|-------------------------|--------------------------------------|---------------|------------------------------------|  
|**-BcpBatchSize**|100000|100000|1000|100000|100000|100000|100000|  
|**-ChangesPerHistory**|100|50|50|100|100|100|1000|  
|**-DestThreads**|2|1|1|1|1|1|4|  
|**-DownloadGenerationsPerBatch**|50|50|50|50|50|1|500|  
|**-DownloadReadChangesPerBatch**|100|100|100|100|100|100|100|  
|**-DownloadWriteChangesPerBatch**|100|100|100|100|100|100|100|  
|**-FastRowCount**|1|1|1|1|1|1|1|  
|**-HistoryVerboseLevel**|2|3|1|1|2|1|2|  
|**-KeepAliveMessageInterval**|300|300|300|300|300|300|300|  
|**-LoginTimeout**|15|15|15|15|15|15|15|  
|**-MaxDownloadChanges**|0|0|0|0|0|0|0|  
|**-MaxUploadChanges**|0|0|0|0|0|0|0|  
|**-MetadataRetentionCleanup**|1|1|1|1|1|1|1|  
|**-NumDeadlockRetries**|5|5|5|5|5|5|5|  
|**-ParallelUploadDownload**|Null|Null|Null|Null|Null|Null|1|  
|**-PollingInterval**|60|60|60|60|60|60|60|  
|**-QueryTimeout**|300|300|300|300|300|300|600|  
|**-QueueSizeMultiplier**|Null|Null|Null|Null|Null|Null|5|  
|**-SrcThreads**|2|2|2|2|2|1|3|  
|**-StartQueueTimeout**|0|0|0|0|0|0|0|  
|**-UploadGenerationsPerBatch**|50|50|50|50|50|1|500|  
|**-UploadReadChangesPerBatch**|100|100|100|100|100|100|100|  
|**-UploadWriteChangesPerBatch**|100|100|100|100|100|100|100|  
|**-Validate**|0|0|0|1|3|0|0|  
|**-ValidateInterval**|60|60|60|60|60|60|60|  
  
## <a name="queue-reader-agent-profiles"></a>队列读取器代理配置文件  
 下表显示了队列读取器代理的默认配置文件中定义的参数。 有关这些参数的详细信息，请参阅 [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)。  
  
||默认值|  
|-|-------------|  
|**-HistoryVerboseLevel**|1|  
|**-LoginTimeout**|15|  
|**-PollingInterval**|5|  
|**-QueryTimeout**|1800|  
  
## <a name="see-also"></a>另请参阅  
 [复制代理管理](../../../relational-databases/replication/agents/replication-agent-administration.md)   
 [查看和修改复制代理命令提示符参数 (SQL Server Management Studio)](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)   
 [Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
  
