---
title: 使用 DMV 监视脚本
description: 使用动态管理视图 (DMV) 监视 Python 和 R 外部脚本在 SQL Server 机器学习服务中的执行情况。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 91ae86607ee2127af21ae0d47f80580df32c93cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337100"
---
# <a name="monitor-sql-server-machine-learning-services-using-dynamic-management-views-dmvs"></a>使用动态管理视图 (DMV) 监视 SQL Server 机器学习服务
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

使用动态管理视图 (DMV) 监视外部脚本（Python 和 R）的执行情况、使用的资源、诊断问题，以及 SQL Server 机器学习服务中的性能调整。

在本文中，你将找到特定于 SQL Server 机器学习服务的 DMV。 你还将找到显示以下内容的示例查询：

+ 机器学习的设置和配置选项
+ 运行外部 Python 或脚本的活动会话
+ 适用于 Python 和 R 的外部运行时的执行统计信息
+ 外部脚本的性能计数器
+ OS、SQL Server 和外部资源池的内存使用情况
+ SQL Server 和外部资源池的内存配置
+ Resource Governor 资源池，包括外部资源池
+ 已安装的 Python 和 R 包

有关 DMV 的更多常规信息，请参阅[系统动态管理视图](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)。

> [!TIP]
> 还可以使用自定义报表监视 SQL Server 机器学习服务。 有关详细信息，请参阅[在 Management Studio 中使用自定义报表监视机器学习](monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)。

## <a name="dynamic-management-views"></a>动态管理视图

监视 SQL Server 中的机器学习工作负荷时，可以使用以下动态管理视图。 若要查询 DMV，需要对实例拥有 `VIEW SERVER STATE` 权限。

| 动态管理视图 | 类型 | 说明 |
|-------------------------|------|-------------|
| [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md) | 执行 | 为运行外部脚本的每个活动工作线程帐户都返回一行。 |
| [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md) | 执行 | 为每种类型的外部脚本请求返回一行。 |
| [sys.dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) | 执行 | 为服务器维护的每个性能计数器返回一行。 如果使用搜索条件 `WHERE object_name LIKE '%External Scripts%'`，则可使用此信息来确定运行了多少个脚本、使用哪种身份验证模式运行了哪些脚本，或针对整个实例发出了多少次 R 或 Python 调用。 |
| [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md) | Resource Governor | 返回有关 Resource Governor 中的当前外部资源池状态、资源池的当前配置以及资源池统计信息的信息。 |
| [sys.dm_resource_governor_external_resource_pool_affinity](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md) | Resource Governor | 返回有关 Resource Governor 中的当前外部资源池配置的 CPU 关联信息。 对于 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]（每个计划程序都映射到其中的单个处理器）中的每个计划程序，相应地返回一行。 使用此视图可以监视计划程序的情况或标识失控任务。 |

有关监视 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 实例的信息，请参阅[目录视图](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)和[与 Resource Governor 相关的动态管理视图](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)。

## <a name="settings-and-configuration"></a>设置和配置

查看机器学习服务的安装设置和配置选项。

![设置和配置查询的输出](media/dmv-settings-and-configuration.png "设置和配置查询的输出")

运行下面的查询以获取此输出。 有关所使用的视图和函数的详细信息，请参阅 [sys.dm_server_registry](../../relational-databases/system-dynamic-management-views/sys-dm-server-registry-transact-sql.md)、[sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) 和 [SERVERPROPERTY](../../t-sql/functions/serverproperty-transact-sql.md)。

```sql
SELECT CAST(SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS INT) AS IsMLServicesInstalled
    , CAST(value_in_use AS INT) AS ExternalScriptsEnabled
    , COALESCE(SIGN(SUSER_ID(CONCAT (
                    CAST(SERVERPROPERTY('MachineName') AS NVARCHAR(128))
                    , '\SQLRUserGroup'
                    , CAST(serverproperty('InstanceName') AS NVARCHAR(128))
                    ))), 0) AS ImpliedAuthenticationEnabled
    , COALESCE((
            SELECT CAST(r.value_data AS INT)
            FROM sys.dm_server_registry AS r
            WHERE r.registry_key LIKE 'HKLM\Software\Microsoft\Microsoft SQL Server\%\SuperSocketNetLib\Tcp'
            AND r.value_name = 'Enabled'
            ), - 1) AS IsTcpEnabled
FROM sys.configurations
WHERE name = 'external scripts enabled';
```

查询返回以下列：

| 列                       | 说明 |
|------------------------------|-------------|
| IsMLServicesInstalled        | 如果为实例安装了 SQL Server 机器学习服务，则返回 1。 否则，返回 0。 |
| ExternalScriptsEnabled       | 如果为实例启用了外部脚本，则返回 1。 否则，返回 0。 |
| ImpliedAuthenticationEnabled | 如果启用了隐式身份验证，则返回 1。 否则，返回 0。 通过验证是否存在 SQLRUserGroup 登录名来检查隐式身份验证的配置。 |
| IsTcpEnabled                 | 如果为实例启用了 TCP/IP 协议，则返回 1。 否则，返回 0。 有关详细信息，请参阅[默认 SQL Server 网络协议配置](../../database-engine/configure-windows/default-sql-server-network-protocol-configuration.md)。 |

## <a name="active-sessions"></a>Active sessions

查看运行外部脚本的活动会话。

![活动设置查询的输出](media/dmv-active-sessions.png "活动设置查询的输出")

运行下面的查询以获取此输出。 有关所使用的动态管理视图的详细信息，请参阅 [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)、[sys.dm_external_script_requests](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) 和 [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)。

```sql
SELECT r.session_id, r.blocking_session_id, r.status, DB_NAME(s.database_id) AS database_name
    , s.login_name, r.wait_time, r.wait_type, r.last_wait_type, r.total_elapsed_time, r.cpu_time
    , r.reads, r.logical_reads, r.writes, er.language, er.degree_of_parallelism, er.external_user_name
FROM sys.dm_exec_requests AS r
INNER JOIN sys.dm_external_script_requests AS er
ON r.external_script_request_id = er.external_script_request_id
INNER JOIN sys.dm_exec_sessions AS s
ON s.session_id = r.session_id;
```

查询返回以下列：

| 列                | 说明 |
|-----------------------|-------------|
| session_id            | 标识与每个活动主连接关联的会话。 |
| blocking_session_id   | 正在阻塞请求的会话的 ID。 如果此列为 NULL，则表示请求未被阻塞，或锁定会话的会话信息不可用（或无法进行标识）。 |
| status                | 请求的状态。 |
| database_name         | 每个会话的当前数据库的名称。 |
| login_name            | 当前执行的会话所使用的 SQL Server 登录名。 |
| wait_time             | 如果请求当前被阻塞，则此列返回当前等待的持续时间（以毫秒为单位）。 不可为 null。 |
| wait_type             | 如果请求当前被阻塞，则此列返回等待类型。 有关等待类型的信息，请参阅 [sys.dm_os_wait_stats](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)。 |
| last_wait_type        | 如果此请求先前已经阻塞，则此列返回上次等待的类型。 |
| total_elapsed_time    | 请求到达后经过的总时间（毫秒）。 |
| cpu_time              | 请求所使用的 CPU 时间（毫秒）。 |
| reads                 | 此请求执行的读取数。 |
| logical_reads         | 此请求已经执行的逻辑读取数。 |
| Writes                | 此请求执行的写入数。 |
| 语言              | 表示支持的脚本语言的关键字。 |
| degree_of_parallelism | 数字，指示已创建的并行进程数。 此值可能与请求的并行进程数不同。 |
| external_user_name    | 在其下执行脚本的 Windows 工作线程帐户。 |

## <a name="execution-statistics"></a>执行统计信息

查看适用于 R 和 Python 的外部运行时的执行统计信息。 当前只有 RevoScaleR、revoscalepy 或 microsoftml 包函数的统计信息可用。

![执行统计信息查询的输出](media/dmv-execution-statistics.png "执行统计信息查询的输出")

运行下面的查询以获取此输出。 有关所使用的动态管理视图的详细信息，请参阅 [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md)。 查询只返回多次执行的函数。

```sql
SELECT language, counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE counter_value > 0
ORDER BY language, counter_name;
```

查询返回以下列：

| 列        | 描述 |
|---------------|-------------|
| 语言      | 已注册的外部脚本语言的名称。 |
| counter_name  | 已注册的外部脚本函数的名称。 |
| counter_value | 已在服务器上调用已注册外部脚本函数的实例的总数。 此值是累计的（从在实例上安装该功能的时间开始），且不能重置。 |

## <a name="performance-counters"></a>性能计数器

查看与执行外部脚本相关的性能计数器。

![性能计数器查询的输出](media/dmv-performance-counters.png "性能计数器查询的输出")

运行下面的查询以获取此输出。 有关所使用的动态管理视图的详细信息，请参阅 [sys.dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)。

```sql
SELECT counter_name, cntr_value
FROM sys.dm_os_performance_counters 
WHERE object_name LIKE '%External Scripts%'
```

**sys.dm_os_performance_counters** 输出外部脚本的以下性能计数器：

| 计数器                   | 说明 |
|---------------------------|-------------|
| 执行总次数          | 由本地或远程调用启动的外部进程数。 |
| 并行执行       | 脚本包含 _\@parallel_ 规范的次数，以及 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 能够生成并使用并行查询计划的次数。 |
| 流式执行      | 调用流式处理功能的次数。 |
| SQL CC 执行         | 远程实例化调用并将 SQL Server 用作计算上下文的外部脚本运行次数。 |
| 默示身份验证。登录名      | 使用隐式身份验证进行 ODBC 环回调用（即，[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 代表发送脚本请求的用户执行调用）的次数。 |
| 总执行时间 (ms) | 从开始调用到完成调用所消耗的时间。 |
| 执行错误          | 脚本报告错误的次数。 此计数不包括 R 或 Python 错误。 |

## <a name="memory-usage"></a>内存使用率

查看有关 OS、SQL Server 和外部池所使用的内存的信息。

![内存使用情况查询的输出](media/dmv-memory-usage.png "内存使用情况查询的输出")

运行下面的查询以获取此输出。 有关所使用的动态管理视图的详细信息，请参阅 [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md) 和 [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)。

```sql
SELECT physical_memory_kb, committed_kb
    , (SELECT SUM(peak_memory_kb)
        FROM sys.dm_resource_governor_external_resource_pools AS ep
        ) AS external_pool_peak_memory_kb
FROM sys.dm_os_sys_info;
```

查询返回以下列：

| 列                       | 说明                                                                                                           |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| physical_memory_kb           | 计算机上的物理内存总量。                                                                   |
| committed_kb                 | 内存管理器中已提交的内存 (KB)。 不包括内存管理器中的保留内存。 |
| external_pool_peak_memory_kb | 所有外部资源池使用的最大内存量的总和 (KB)。                          |

## <a name="memory-configuration"></a>内存配置

查看 SQL Server 和外部资源池中最大内存配置百分比的相关信息。 如果 SQL Server 运行时的默认值为 `max server memory (MB)`，则将其被视为 100% 的 OS 内存。

![内存配置查询的输出](media/dmv-memory-configuration.png "内存配置查询的输出")

运行下面的查询以获取此输出。 有关所使用的视图的详细信息，请参阅 [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) 和 [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md)。

```sql
SELECT 'SQL Server' AS name
    , CASE CAST(c.value AS BIGINT)
        WHEN 2147483647 THEN 100
        ELSE (SELECT CAST(c.value AS BIGINT) / (physical_memory_kb / 1024.0) * 100 FROM sys.dm_os_sys_info)
        END AS max_memory_percent
FROM sys.configurations AS c
WHERE c.name LIKE 'max server memory (MB)'
UNION ALL
SELECT CONCAT ('External Pool - ', ep.name) AS pool_name, ep.max_memory_percent
FROM sys.dm_resource_governor_external_resource_pools AS ep;
```

查询返回以下列：

| 列             | 说明 |
|--------------------|-------------|
| name               | 外部资源池或 SQL Server 的名称。 |
| max_memory_percent | SQL Server 或外部资源池可使用的最大内存。 |

## <a name="resource-pools"></a>资源池

在 [SQL Server Resource Governor](../../relational-databases/resource-governor/resource-governor.md) 中，[资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)表示实例的部分物理资源。 可以指定针对传入应用程序请求（包括执行外部脚本）可在资源池内使用的 CPU、物理 IO 和内存的使用量限制。 查看用于 SQL Server 和外部脚本的资源池。

![资源池查询的输出](media/dmv-resource-pools.png "资源池查询的输出")

运行下面的查询以获取此输出。 有关所使用的动态管理视图的详细信息，请参阅 [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md) 和 [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md)。

```sql
SELECT CONCAT ('SQL Server - ', p.name) AS pool_name
    , p.total_cpu_usage_ms, p.read_io_completed_total, p.write_io_completed_total
FROM sys.dm_resource_governor_resource_pools AS p
UNION ALL
SELECT CONCAT ('External Pool - ', ep.name) AS pool_name
    , ep.total_cpu_user_ms, ep.read_io_count, ep.write_io_count
FROM sys.dm_resource_governor_external_resource_pools AS ep;
```

查询返回以下列：

| 列                   | 说明  |
|--------------------------|--------------|
| pool_name                | 资源池的名称。 SQL Server 资源池以 `SQL Server` 为前缀，外部资源池以 `External Pool` 为前缀。 |
| total_cpu_usage_hours    | 自重置 Resource Govenor 统计信息以来的累计 CPU 使用量（毫秒）。 |
| read_io_completed_total  | 自重置 Resource Governor 统计信息以来完成的读取 IO 总数。              |
| write_io_completed_total | 自重置 Resource Governor 统计信息以来完成的写入 IO 总数。             |

## <a name="installed-packages"></a>已安装的包

可通过执行输出相关信息的 R 或 Python 脚本来查看 SQL Server 机器学习服务中安装的 R 和 Python 包。

### <a name="installed-packages-for-r"></a>已安装的 R 包

查看 SQL Server 机器学习服务中安装的 R 包。

![已安装的 R 包查询的输出](media/dmv-installed-packages-r.png "已安装的 R 包查询的输出")

运行下面的查询以获取此输出。 查询使用 R 脚本来确定随 SQL Server 一起安装的 R 包。

```sql
EXECUTE sp_execute_external_script @language = N'R'
, @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((Package NVARCHAR(255), Version NVARCHAR(100), Depends NVARCHAR(4000)
    , License NVARCHAR(1000), LibPath NVARCHAR(2000)));
```

返回的列包括：

| 列  | 说明                                                 |
|---------|-------------------------------------------------------------|
| 程序包 | 已安装包的名称。                              |
| 版本 | 包的版本。                                     |
| 依赖的对象 | 列出已安装的包所依赖的包。 |
| 许可 | 已安装包的许可证。                          |
| LibPath | 可在其中找到包的目录。                   |

### <a name="installed-packages-for-python"></a>已安装的 Python 包

查看 SQL Server 机器学习服务中安装的 Python 包。

![已安装的 Python 包查询的输出](media/dmv-installed-packages-python.png "已安装的 Python 包查询的输出")

运行下面的查询以获取此输出。 查询使用 Python 脚本来确定随 SQL Server 一起安装的 Python 包。

```sql
EXECUTE sp_execute_external_script @language = N'Python'
, @script = N'
import pkg_resources
import pandas
OutputDataSet = pandas.DataFrame(sorted([(i.key, i.version, i.location) for i in pkg_resources.working_set]))'
WITH result sets((Package NVARCHAR(128), Version NVARCHAR(128), Location NVARCHAR(1000)));
```

返回的列包括：

| 列   | 说明                               |
|----------|-------------------------------------------|
| 程序包  | 已安装包的名称。            |
| 版本  | 包的版本。                   |
| 位置 | 可在其中找到包的目录。 |

## <a name="next-steps"></a>后续步骤

+ [机器学习的扩展事件](extended-events.md)
+ [与 Resource Governor 相关的动态管理视图](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)
+ [系统动态管理视图](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [在 Management Studio 中使用自定义报表监视机器学习](monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)
