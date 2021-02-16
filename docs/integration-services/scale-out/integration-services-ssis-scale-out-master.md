---
title: Scale Out Master | Microsoft Docs
description: 了解 SQL Server Integration Services (SSIS) Scale Out Master 服务的 Scale Out Master 组件。
ms.custom: performance
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 8f3b205acd32ba48b86ac797fd99fe4cef4fccf5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347400"
---
# <a name="integration-services-ssis-scale-out-master"></a>Integration Services (SSIS) Scale Out Master

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Scale Out Master 通过 SSISDB 目录和 Scale Out Master 服务来管理 Scale Out 系统。 

SSISDB 目录可存储 Scale Out Worker、包和执行的所有信息。 它可提供接口，以便在 Scale Out 中启用 Scale Out Worker 和执行包。有关详细信息，请参阅[演练：设置 Integration Services Scale Out](walkthrough-set-up-integration-services-scale-out.md) 和[在 Integration Services 中运行包](run-packages-in-integration-services-ssis-scale-out.md)。

Scale Out Master 服务是一项 Windows 服务，负责与 Scale Out Worker 的通信。 它通过 HTTPS 返回 Scale Out Worker 上包执行的状态，并对 SSISDB 中的数据进行操作。 

## <a name="scale-out-views-and-stored-procedures-in-ssisdb"></a>SSISDB 中的 Scale Out 视图和存储过程

### <a name="views"></a>视图

- [[catalog].[master_properties]](../../integration-services/system-views/catalog-master-properties-ssisdb-database.md)
- [[catalog].[worker_agents]](../../integration-services/system-views/catalog-worker-agents-ssisdb-database.md)

### <a name="stored-procedures"></a>存储过程

- 有关管理 Scale Out Worker：
    - [[catalog].[disable_worker_agent]](../../integration-services/system-stored-procedures/catalog-disable-worker-agent-ssisdb-database.md)
    - [[catalog].[enable_worker_agent]](../../integration-services/system-stored-procedures/catalog-enable-worker-agent-ssisdb-database.md)

- 有关在 Scale Out 中运行包：
    - [[catalog].[create_execution]](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)
    - [[catalog].[add_execution_worker]](../../integration-services/system-stored-procedures/catalog-add-execution-worker-ssisdb-database.md)
    - [[catalog].[start_execution]](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)

## <a name="configure-the-scale-out-master-service"></a>配置 Scale Out Master 服务

使用 `<drive>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config` 文件配置 Scale Out Master 服务。 更新配置文件之后，必须重启服务。


|配置  |说明  |默认值  |
|---------|---------|---------|
|PortNumber|用于与 Scale Out Worker 进行通信的网络端口号。|8391|
|SSLCertThumbprint|用于保护与 Scale Out 工作器之间通信的 TLS/SSL 证书的指纹。|在 Scale Out Master 安装期间指定的 TLS/SSL 证书的指纹|
|SqlServerName|包含 SSISDB 目录的 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 的名称。 例如，ServerName\\InstanceName。|与 Scale Out Master 一起安装的 SQL Server 的名称。|
|CleanupCompletedJobsIntervalInMs|清理已完成的执行作业的间隔时间（以毫秒为单位）。|43200000|
|DealWithExpiredTasksIntervalInMs|处理过期的执行作业的间隔时间（以毫秒为单位）。|300000|
|MasterHeartbeatIntervalInMs|Scale Out Master 检测信号的间隔时间（以毫秒为单位）。 此属性指定 Scale Out Master 更新其在 SSISDB 目录中联机状态的间隔时间。|30000|
|SqlConnectionTimeoutInSecs|连接到 SSISDB 时的 SQL 连接超时值（以秒为单位）。|15|
||||    

## <a name="view-the-scale-out-master-service-log"></a>查看 Scale Out Master 服务日志

Scale Out Master 服务日志文件位于 `<drive>:\Users\[account]\AppData\Local\SSIS\ScaleOut\Master` 文件夹。 

[account] 参数指运行 Scale Out Master 服务的帐户  。 默认情况下，此帐户为 `SSISScaleOutMaster140`。

## <a name="next-steps"></a>后续步骤

[Integration Services (SSIS) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
