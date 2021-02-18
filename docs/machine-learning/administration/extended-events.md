---
title: 用扩展事件监视脚本
description: 了解如何使用扩展事件来监视和疑难解答与 SQL Server 机器学习服务、SQL Server Launchpad 以及 Python 或 R 作业外部脚本有关的操作。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/04/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: d8324e49ce4de61096880275a07df5a20e288a93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337161"
---
# <a name="monitor-python-and-r-scripts-with-extended-events-in-sql-server-machine-learning-services"></a>在 SQL Server 机器学习服务中使用扩展事件监视 Python 和 R 脚本
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

了解如何使用扩展事件来监视和疑难解答与 SQL Server 机器学习服务、SQL Server Launchpad 以及 Python 或 R 作业外部脚本有关的操作。

## <a name="extended-events-for-sql-server-machine-learning-services"></a>SQL Server 机器学习服务的扩展事件

若要查看与 SQL Server 机器学习服务相关的事件列表，请从 Azure Data Studio 或 SQL Server Management Studio 运行以下查询。

```sql
SELECT o.name AS event_name, o.description
FROM sys.dm_xe_objects o
JOIN sys.dm_xe_packages p
ON o.package_guid = p.guid
WHERE o.object_type = 'event'
AND p.name = 'SQLSatellite';
```

有关如何扩展事件的详细信息，请参阅[扩展事件工具](../../relational-databases/extended-events/extended-events-tools.md)。

## <a name="additional-events-specific-to-machine-learning-services"></a>特定于机器学习服务的其他事件

其他扩展事件可用于与 SQL Server 机器学习服务相关并由其使用的组件，例如 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 和 BXLServer，以及启动 Python 或 R 运行时的附属进程。 这些额外的扩展事件是从外部流程中触发的；因此，必须使用外部实用工具捕获它们。

有关如何操作的详细信息，请参阅[从外部进程收集事件](#bkmk_externalevents)一节。

<a name="bkmk_xeventtable"></a> 

## <a name="table-of-extended-events"></a>扩展事件表

|事件|说明|说明|  
|-----------|-----------------|---------|  
|connection_accept|接受一个新连接时发生。 此事件用于记录所有连接尝试。||  
|failed_launching|启动失败。|指示一个错误。|  
|satellite_abort_connection|中止连接记录||  
|satellite_abort_received|通过卫星连接接收中止消息时触发。||  
|satellite_abort_sent|通过卫星连接发送中止消息时触发。||  
|satellite_authentication_completion|对基于 TCP 或命名管道的连接的身份验证完成时触发。||  
|satellite_authorization_completion|对基于 TCP 或命名管道的连接的授权完成时触发。||  
|satellite_cleanup|卫星调用清理时触发。|仅从外部进程触发。 请参阅有关从外部进程收集事件的说明。|  
|satellite_data_chunk_sent|卫星连接完成发送单个数据区块时触发。|发送区块时，此事件报告发送的行数、列数、所用的 SNI 数据包数目以及所用时间（以毫秒为单位）。 这些信息可帮助你了解传递不同类型的数据时花费了多长时间，以及使用了多少个数据包。|  
|satellite_data_receive_completion|通过卫星连接接收查询所需的所有数据时触发。|仅从外部进程触发。 请参阅有关从外部进程收集事件的说明。|  
|satellite_data_send_completion|通过卫星连接发送会话所需的所有数据时触发。||  
|satellite_data_send_start|数据传输开始时触发。| 数据传输开始时随即发送第一个数据区块。|  
|satellite_error|用于跟踪 sql 卫星错误||  
|satellite_invalid_sized_message|消息大小无效||  
|satellite_message_coalesced|用于跟踪在网络层合并的消息||  
|satellite_message_ring_buffer_record|消息环形缓冲区记录||  
|satellite_message_summary|有关消息传送的摘要信息||  
|satellite_message_version_mismatch|消息的版本字段不匹配||  
|satellite_messaging|用于跟踪消息传送事件（绑定、取消绑定等）||  
|satellite_partial_message|用于跟踪网络层的部分消息||  
|satellite_schema_received|通过 SQL 接收和读取架构消息时触发。||  
|satellite_schema_sent|通过卫星发送架构消息时触发。|仅从外部进程触发。 请参阅有关从外部进程收集事件的说明。|  
|satellite_service_start_posted|当服务启动消息发布到 launchpad 时触发。|这可以通知 launchpad 启动外部进程，并且该事件包含此新会话的 ID。|  
|satellite_unexpected_message_received|接收到意外消息时触发。|指示一个错误。|  
|stack_trace|请求进行进程的内存转储时发生。|指示一个错误。|  
|trace_event|用于跟踪目的|这些事件可以包含 SQL Server、Launchpad 和外部进程跟踪消息。 这包括从 R 到 stdout 和 stderr 的输出。|  
|launchpad_launch_start|launchpad 开始启动卫星时触发。|仅从 Launchpad 触发。 请参阅有关从 launchpad.exe 收集事件的说明。|  
|launchpad_resume_sent|Launchpad 已启动卫星并将恢复消息发送到 SQL Server 时触发。|仅从 Launchpad 触发。 请参阅有关从 launchpad.exe 收集事件的说明。|  
|satellite_data_chunk_sent|卫星连接完成发送单个数据区块时触发。|包含列数、行数、数据包数量，以及发送数据区块所用的时间等信息。|  
|satellite_sessionId_mismatch|消息的会话 ID 不匹配||  

<a name="bkmk_externalevents"></a>

### <a name="collecting-events-from-external-processes"></a>从外部进程收集事件

SQL Server 机器学习服务启动在 SQL Server 进程外部运行的一些服务。 若要捕获与这些外部进程相关的事件，必须创建一个事件跟踪配置文件，并将该文件放在与进程的可执行文件相同的目录中。  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!IMPORTANT]
> 自 SQL Server 2019 起，隔离机制已发生更改。 因此，需要为存储事件跟踪配置文件的目录授予适当的权限。 有关如何设置这些权限的详细信息，请参阅 [Windows 上 SQL Server 2019 中的“文件权限”部分：机器学习服务的隔离更改](../install/sql-server-machine-learning-services-2019.md#file-permissions)。
::: moniker-end

+ **[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]**   
  
    若要捕获与 Launchpad 相关的事件，请将 .xml 文件放在 SQL Server 实例的 Binn 目录中  。 在默认安装中，此目录为：

    `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\MSSQL\Binn`.  
  
+ **BXLServer** 是使用外部脚本语言支持 SQL 可扩展性的附属进程，如 R 和 Python。 为每个外部语言实例启动单独的 BxlServer 实例。
  
    若要捕获与 BXLServer 相关的事件，请将 .xml 文件放在 R 或 Python 安装目录中  。 在默认安装中，此目录为：
     
    **R：** `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64`。  

    **Python：** `C:\Program Files\Microsoft SQL Server\MSSQL_version_number.MSSQLSERVER\PYTHON_SERVICES\Lib\site-packages\revoscalepy\rxLibs`。

此配置文件的名称必须与可执行文件相同，并使用“[名称].xevents.xml”的格式。 换而言之，这些文件的名称如下所示：

+ `Launchpad.xevents.xml`
+ `bxlserver.xevents.xml`

配置文件自身具有以下格式：

```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
<event_session name="[session name]" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="you">Xevent for launchpad or bxl server.</description>  
    <event package="SQLSatellite" name="[XEvent Name 1]" />  
    <event package="SQLSatellite" name="[XEvent Name 2]" />  
    <target package="package0" name="event_file">  
      <parameter name="filename" value="[SessionName].xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ 若要配置跟踪，请编辑会话名称  占位符、文件名的占位符 (`[SessionName].xel`) 和要捕获的事件的名称（如 `[XEvent Name 1]`、`[XEvent Name 1]`）。  
+ 可能出现任意数量的“event package”标记，并且只要名称属性正确，就会收集此标记。

### <a name="example-capturing-launchpad-events"></a>示例：捕获 Launchpad 事件

下面的示例显示了 Launchpad 服务的事件跟踪定义：

```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
<event_session name="sqlsatelliteut" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="hay">Xevent for sql tdd runner.</description>  
    <event package="SQLSatellite" name="launchpad_launch_start" />  
    <event package="SQLSatellite" name="launchpad_resume_sent" />  
    <target package="package0" name="event_file">  
      <parameter name="filename" value="launchpad_session.xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ 将 .xml 文件放在 SQL Server 实例的 Binn 目录中  。
+ 此文件必须命名为 `Launchpad.xevents.xml`。

### <a name="example-capturing-bxlserver-events"></a>示例：捕获 BXLServer 事件  

下面的示例显示了 BXLServer 可执行文件的事件跟踪定义。
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<event_sessions>  
 <event_session name="sqlsatelliteut" maxMemory="1" dispatchLatency="1" MaxDispatchLatency="2 SECONDS">  
    <description owner="hay">Xevent for sql tdd runner.</description>  
    <event package="SQLSatellite" name="satellite_abort_received" />  
    <event package="SQLSatellite" name="satellite_authentication_completion" />  
    <event package="SQLSatellite" name="satellite_cleanup" />  
    <event package="SQLSatellite" name="satellite_data_receive_completion" />  
    <event package="SQLSatellite" name="satellite_data_send_completion" />  
    <event package="SQLSatellite" name="satellite_data_send_start" />  
    <event package="SQLSatellite" name="satellite_schema_sent" />   
    <event package="SQLSatellite" name="satellite_unexpected_message_received" />    
    <event package="SQLSatellite" name="satellite_data_chunk_sent" />   
    <target package="package0" name="event_file">  
      <parameter name="filename" value="satellite_session.xel" />  
      <parameter name="max_file_size" value="10" />  
      <parameter name="max_rollover_files" value="10" />  
    </target>  
  </event_session>  
</event_sessions>  
```

+ 将 .xml 文件放在与 BXLServer 可执行文件相同的目录中  。
+ 此文件必须命名为 `bxlserver.xevents.xml`。

## <a name="next-steps"></a>后续步骤

- [使用 SQL Server Management Studio 中的自定义报表监视 Python 和 R 脚本执行](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)
- [使用动态管理视图 (DMV) 监视 SQL Server 机器学习服务](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)