---
title: SQL Server Integration Services (SSIS) Scale Out Worker | Microsoft Docs
description: 详细了解 SQL Server Integration Services (SSIS) Scale Out 的 Scale Out Master 组件。
ms.custom: performance
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 35131a210e54557d63cb3e1986bddfd9d75e35ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338368"
---
# <a name="integration-services-ssis-scale-out-worker"></a>Integration Services (SSIS) Scale Out Worker

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Scale Out Worker 运行 Scale Out Worker 服务以从 Scale Out Master 中请求执行任务。 然后，Worker 通过 `ISServerExec.exe` 在本地运行包。

## <a name="configure-the-scale-out-worker-service"></a>配置 Scale Out Worker 服务
可使用 `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config` 文件配置 Scale Out Worker 服务。 更新配置文件之后，必须重启服务。

|配置  |说明  |默认值|
|---------|---------|---------|
|DisplayName|Scale Out Worker 的显示名称。 没有在 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017 中使用。|计算机名称|
|说明|Scale Out Worker 的说明。 没有在 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017 中使用。|空|
|MasterEndpoint|连接到 Scale Out Master 的终结点。|该终结点在 Scale Out Worker 安装期间设置|
|MasterHttpsCertThumbprint|用于验证 Scale Out Master 的客户端 TLS/SSL 证书的指纹|客户端证书的指纹在 Scale Out Worker 安装期间指定。|
|WorkerHttpsCertThumbprint|用于对 Scale Out Worker 进行身份验证的 Scale Out Master 证书的指纹。|证书的指纹在 Scale Out Worker 安装期间自动创建并安装|
|StoreLocation|Worker 证书的存储位置。|LocalMachine|
|StoreName|Worker 证书所在位置的存储名称。|My|
|AgentHeartbeatInterval|Scale Out Worker 检测信号的间隔时间。|00:01:00|
|TaskHeartbeatInterval|Scale Out Worker 报告任务状态的间隔时间。|00:00:10|
|HeartbeatErrorTolerance|上一次任务检测信号成功后，如果接收到检测信号的错误响应，任务将终止。|00:10:00|
|TaskRequestMaxCPU|Scale Out Worker 请求任务的 CPU 上限。|70.0|
|TaskRequestMinMemory|Scale Out Worker 请求任务的内存（以 MB 表示）下限。|100.0|
|MaxTaskCount|Scale Out Worker 可保留的最大任务数。|10|
|LeaseInterval|Scale Out Worker 可保留任务的租用间隔。|00:01:00|
|TasksRootFolder|任务日志的文件夹。 如果值为空，则使用 `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Tasks` 文件夹路径。 [account] 是运行 Scale Out Worker 服务的帐户。 默认情况下，该帐户为 SSISScaleOutWorker140。|空|
|TaskLogLevel|Scale Out Worker 的任务日志级别。 (Verbose 0x01, Information 0x02, Warning 0x04, Error 0x08, Progress 0x10, CriticalError 0x20, Audit 0x40)|126（信息、警告、错误、进度、CriticalError 和审核）|
|TaskLogSegment|任务日志文件的时间跨度。|00:00:00|
|TaskLogEnabled|指定是否启用任务日志。|true|
|ExecutionLogCacheFolder|用于缓存包执行日志的文件夹。 如果值为空，则使用 `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Agent\ELogCache` 文件夹路径。 [account] 是运行 Scale Out Worker 服务的帐户。 默认情况下，该帐户为 SSISScaleOutWorker140。|空|
|ExecutionLogMaxBufferLogCount|在内存的一个执行日志缓冲区中，缓存的执行日志的最大数量。|10000|
|ExecutionLogMaxInMemoryBufferCount|用于执行日志的内存中，执行日志缓冲区的最大数量。|10|
|ExecutionLogRetryCount|执行日志记录失败时的重试次数。|3|
|ExecutionLogRetryTimeout|执行日志记录失败时的重试超时时间。 如果达到 ExecutionLogRetryTimeout，则忽略 ExecutionLogRetryCount。 |7.00:00:00（7 天）|
|AgentId|Scale Out Worker 的 Worker 代理 ID|自动生成|
||||    

## <a name="view-the-scale-out-worker-log"></a>查看 Scale Out Worker 日志
Scale Out Worker 服务的日志文件位于 `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Agent` 文件夹。

在 `TasksRootFolder` 的 `WorkerSettings.config` 文件中配置每项单独任务的日志位置。 如果未指定值，则日志位于 `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks` 文件夹。 

[account] 参数是运行 Scale Out Worker 服务的帐户  。 默认情况下，该帐户为 `SSISScaleOutWorker140`。

## <a name="next-steps"></a>后续步骤
[Integration Services (SSIS) Scale Out Master](integration-services-ssis-scale-out-master.md)
