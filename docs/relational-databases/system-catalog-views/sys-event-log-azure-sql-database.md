---
description: sys.event_log (Azure SQL Database)
title: Azure SQL Database (sys.event_log) |Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- event_log
- sys.event_log_TSQL
- event_log_TSQL
- sys.event_log
dev_langs:
- TSQL
helpviewer_keywords:
- event_log
- sys.event_log
ms.assetid: ad5496b5-e5c7-4a18-b5a0-3f985d7c4758
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: ae823cddc60a82c343bc949515493f9405a51a9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203858"
---
# <a name="sysevent_log-azure-sql-database"></a>sys.event_log (Azure SQL Database)

[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  返回成功的 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 数据库连接、连接失败和死锁。 您可以使用此信息跟踪与 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 相关的数据库活动或排除其故障。  
  
> [!CAUTION]  
> 对于具有大量数据库或大量登录的安装，sys.event_log 中的活动可能会导致性能的限制、高 CPU 使用率，并可能导致登录失败。 查询 sys.event_log 可以导致问题。 Microsoft 正在努力解决此问题。 同时，为了降低此问题的影响，请限制 sys.event_log 的查询。 NewRelic SQL Server 插件的用户应访问 [Microsoft Azure SQL 数据库的插件优化 & 性能调整](https://discuss.newrelic.com/t/microsoft-azure-sql-database-plugin-tuning-performance-tweaks/30729) ，以了解其他配置信息。  
  
 `sys.event_log` 视图包含以下各列。  
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|数据库的名称。 如果连接失败，并且用户未指定数据库名称，则此列为空白。|  
|**start_time**|**datetime2**|聚合间隔开始的 UTC 日期和时间。 对于聚合事件，时间始终为 5 分钟的倍数。 例如：<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**end_time**|**datetime2**|聚合间隔结束的 UTC 日期和时间。 对于聚合事件， **End_time** 始终比同一行中对应的 **start_time** 正好晚5分钟。 对于未聚合的事件， **start_time** 和 **end_time** 等于事件的实际 UTC 日期和时间。|  
|**event_category**|**nvarchar (64)**|生成此事件的高级组件。<br /><br /> 有关可能值的列表，请参阅 [事件类型](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) 。|  
|event_type|**nvarchar (64)**|事件的类型。<br /><br /> 有关可能值的列表，请参阅 [事件类型](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) 。|  
|**event_subtype**|**int**|发生的事件的子类型。<br /><br /> 有关可能值的列表，请参阅 [事件类型](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) 。|  
|**event_subtype_desc**|**nvarchar (64)**|事件子类型的说明。<br /><br /> 有关可能值的列表，请参阅 [事件类型](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) 。|  
|severity |**int**|错误的严重性。 可能的值有：<br /><br /> 0 = 信息<br />1 = 警告<br />2 = 错误|  
|**event_count**|**int**|在指定的时间间隔内，针对指定数据库发生此事件的次数 (**start_time** 并 **end_time**) 。|  
|description|**nvarchar(max)**|对事件的详细说明。<br /><br /> 有关可能值的列表，请参阅 [事件类型](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) 。|  
|**additional_data**|**XML**|*注意：对于 Azure SQL 数据库 V12，此值始终为 NULL。请参阅 [示例](#Deadlock) 部分，了解如何检索 V12 的死锁事件。*<br /><br /> 对于 **死锁** 事件，此列包含死锁图。 对于其他事件类型，该列为 NULL。 |  
  
##  <a name="event-types"></a><a name="EventTypes"></a> 事件类型

 此视图中的每一行记录的事件由类别标识 (**event_category**) 、事件类型 (**event_type**) 和子 **类型 (event_subtype) 。** 下表列出了此视图中收集的事件类型。  
  
 对于 " **连接** " 类别中的事件，"sys.database_connection_stats" 视图中提供了摘要信息。  
  
> [!NOTE]  
> 此视图不包括所有可能发生的 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 数据库事件，而仅限此处列出的事件。 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 的未来版本中可能会添加其他类别、事件类型和子类型。  
  
|**event_category**|event_type|**event_subtype**|**event_subtype_desc**|severity |description|  
|-------------------------|---------------------|------------------------|------------------------------|------------------|---------------------|  
|**连接**|**connection_successful**|0|**connection_successful**|0|已成功连接到数据库。|  
|**连接**|**connection_failed**|0|**invalid_login_name**|2|登录名在此版本的 SQL Server 中无效。|  
|**连接**|**connection_failed**|1|**windows_auth_not_supported**|2|在此版本的 SQL Server 中不支持 Windows 登录名。|  
|**连接**|**connection_failed**|2|**attach_db_not_supported**|2|用户已请求附加不受支持的数据库文件。|  
|**连接**|**connection_failed**|3|**change_password_not_supported**|2|用户已请求更改不受支持的用户登录密码。|  
|**连接**|**connection_failed**|4|**login_failed_for_user**|2|用户登录失败。|  
|**连接**|**connection_failed**|5|**login_disabled**|2|登录名已禁用。|  
|**连接**|**connection_failed**|6|**failed_to_open_db**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 无法打开数据库。 原因可能是该数据库不存在，或缺少身份验证而无法打开数据库。|  
|**连接**|**connection_failed**|7|**blocked_by_firewall**|2|不允许客户端 IP 地址访问该服务器。|  
|**连接**|**connection_failed**|8|**client_close**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 在建立连接时，客户端可能已超时。 尝试增加连接超时值。|  
|**连接**|**connection_failed**|9|**重新配置**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 连接失败，因为数据库目前正在经历重新配置。|  
|**连接**|**connection_terminated**|0|**idle_connection_timeout**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 连接闲置的时间超过了系统定义的阈值。|  
|**连接**|**connection_terminated**|1|**重新配置**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 由于数据库重新配置，该会话已终止。|  
|**连接**|**调整**|*\<reason code>*|**reason_code**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 请求受到限制。  限制原因代码： *\<reason code>* 。 有关详细信息，请参阅 [引擎限制](/previous-versions/azure/dn338079(v=azure.100))。|  
|**连接**|**throttling_long_transaction**|40549|**long_transaction**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 由于您有长时间运行的事务，已终止会话。 请尝试缩短您的事务的运行时间。 有关详细信息，请参阅 [资源限制](/previous-versions/azure/dn338081(v=azure.100))。|  
|**连接**|**throttling_long_transaction**|40550|**excessive_lock_usage**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 由于会话获取的锁过多，已终止该会话。 请尝试在单个事务中读取或修改更少的行。 有关详细信息，请参阅 [资源限制](/previous-versions/azure/dn338081(v=azure.100))。|  
|**连接**|**throttling_long_transaction**|40551|**excessive_tempdb_usage**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 由于过度使用 TEMPDB，已终止该会话。 请尝试修改您的查询以减少使用临时表空间。 有关详细信息，请参阅 [资源限制](/previous-versions/azure/dn338081(v=azure.100))。|  
|**连接**|**throttling_long_transaction**|40552|**excessive_log_space_usage**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 由于过度使用事务日志空间，已终止该会话。 请尝试在单个事务中修改更少的行。 有关详细信息，请参阅 [资源限制](/previous-versions/azure/dn338081(v=azure.100))。|  
|**连接**|**throttling_long_transaction**|40553|**excessive_memory_usage**|2|*注意：仅适用于 Azure SQL 数据库 V11。*<br /><br /> 由于过度使用内存，已终止该会话。 请尝试修改您的查询以处理更少的行。 有关详细信息，请参阅 [资源限制](/previous-versions/azure/dn338081(v=azure.100))。|  
|**搜索引擎优化**|**deadlock**|0|**deadlock**|2|发生死锁。|  
  
## <a name="permissions"></a>权限

 有权访问 **master** 数据库的用户对此视图具有只读访问权限。  
  
## <a name="remarks"></a>备注  
  
### <a name="event-aggregation"></a>事件聚合

 在 5 分钟的间隔内收集和聚合此视图的事件信息。 " **Event_count** " 列表示特定数据库在给定时间间隔内发生特定 **event_type** 和 **event_subtype** 的次数。  
  
> [!NOTE]  
> 某些事件（如死锁）不会聚集。 对于这些事件， **event_count** 将为1， **start_time** 并且 **end_time** 将等于事件发生时的实际 UTC 日期和时间。  
  
 例如，如果用户在 2/5/2012 (UTC) 的 11:00 到 11:05 之间由于登录名无效而七次均无法连接到数据库 Database1，则此信息将出现在此视图的单一行内：  
  
|**database_name**|**start_time**|**end_time**|**event_category**|event_type|**event_subtype**|**event_subtype_desc**|severity |**event_count**|description|**additional_data**|  
|------------------------|---------------------|-------------------|-------------------------|---------------------|------------------------|------------------------------|------------------|----------------------|---------------------|--------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`connectivity`|`connection_failed`|`4`|`login_failed_for_user`|`2`|`7`|`Login failed for user.`|`NULL`|  
  
### <a name="interval-start_time-and-end_time"></a>间隔 start_time 和 end_time  
 事件发生的时间或 _之后_**start_time** 和该间隔 _之前_**end_time** *时，* 聚合间隔中包含一个事件。 例如，恰好在 `2012-10-30 19:25:00.0000000` 发生的事件将只包含在如下所示的第二个间隔内：  
  
```
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```

### <a name="data-updates"></a>数据更新

 此视图中的数据会随时间推移而累积。 通常，数据将在聚合间隔开始后的一小时内累积，但可能需要多达 24 小时才能使所有数据都出现在此视图中。 在此期间，可能会定期更新单一行中的信息。  
  
### <a name="data-retention"></a>数据保留

 此视图中的数据最长保留30天，或者可能不太多，具体取决于数据库的数目和每个数据库生成的唯一事件数。 要将此信息保留更长期间，请将数据复制到单独的数据库。 在对视图进行初始复制后，视图中的行可能会随数据的累积而进行更新。 为了使数据副本保持最新状态，请定期对表中的行进行扫描，以查看现有行的事件计数的增加并确定新行（您可以通过使用开始时间和结束时间来确定唯一的行），然后使用这些更改更新您的数据副本。  
  
### <a name="errors-not-included"></a>未包括的错误

 此视图可能并未包含所有连接和错误信息：  
  
- 此视图不包含 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 可能会发生的所有数据库错误，只包括本主题的 [事件类型](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) 中指定的错误。  
- 如果数据中心内存在计算机故障 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ，则事件表中可能缺少少量数据。  
- 如果通过 DoSGuard 拦截了 IP 地址，则无法收集来自该 IP 地址的连接尝试事件，这些事件不会出现在此视图中。  
  
## <a name="examples"></a>示例  
  
### <a name="simple-examples"></a>简单示例

 以下查询将返回在 9/25/2011 与 9/28/2011 (UTC) 之间发生的所有事件。 默认情况下，查询结果按) **start_time** (升序排序。  

```sql
SELECT * FROM sys.event_log
WHERE start_time >= '2011-09-25 12:00:00'
    AND end_time <= '2011-09-28 12:00:00';  
```

下面的查询将返回数据库 Database1 (仅适用于 Azure SQL 数据库 V11) 的所有死锁事件。  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'deadlock'
    AND database_name = 'Database1';  
```

<a name="Deadlock"></a> 下面的查询将返回数据库 Database1 (仅适用于 Azure SQL 数据库 V12) 的所有死锁事件。  

```sql
WITH CTE AS (  
       SELECT CAST(event_data AS XML)  AS [target_data_XML]  
   FROM sys.fn_xe_telemetry_blob_target_read_file('dl', null, null, null)  
)  
SELECT target_data_XML.value('(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
target_data_XML.query('/event/data[@name=''xml_report'']/value/deadlock') AS deadlock_xml,  
target_data_XML.query('/event/data[@name=''database_name'']/value').value('(/value)[1]', 'nvarchar(100)') AS db_name  
FROM CTE  
```

以下事件对在 9/25/2011 (UTC) 的 10:00 与 11:00 之间发生的 SQL 工作线程事件返回硬中止。  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'throttling'
    AND event_subtype = 4194307
    AND start_time >= '2011-09-25 10:00:00'
    AND end_time <= '2011-09-25 11:00:00';  
```

### <a name="db-scoped-extended-event"></a>DB-Scoped 扩展事件

 使用以下示例代码设置 (XEvent) 会话的 db 范围扩展事件：  

```sql
IF EXISTS  
    (SELECT * from sys.database_event_sessions  
        WHERE name = 'azure_monitor_deadlock_session')  
BEGIN  
    ALTER EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE  
        DROP TARGET package0.ring_buffer;  
  
    DROP EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE;  
END  
  
CREATE EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    ADD EVENT sqlserver.database_xml_deadlock_report  
    ADD TARGET package0.ring_buffer  
    (  
        SET max_memory = 2048, max_events_limit = 10  
    )  
    WITH (STARTUP_STATE = ON,  
          EVENT_RETENTION_MODE = ALLOW_SINGLE_EVENT_LOSS);  
  
ALTER EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    STATE = START;  
```

### <a name="check-for-deadlock"></a>检查死锁

使用以下查询来检查是否存在死锁。  

```sql
WITH CTE AS (  
    SELECT CAST(xet.target_data AS XML)  AS [target_data_XML]  
        FROM            sys.dm_xe_database_session_targets AS xet  
             INNER JOIN sys.dm_xe_database_sessions        AS xe  
                 ON (xe.address = xet.event_session_address)  
        WHERE xe.name = 'azure_monitor_deadlock_session'  
)  
, CTE2 AS (  
    SELECT  
            T2.EventData.query('.').value(  
                '(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
            T2.EventData.query('.').query(  
                '(/event/data/value/deadlock)[1]')     AS deadlock_xml  
        FROM CTE  
            CROSS Apply [target_data_XML].nodes(  
                '/RingBufferTarget/event') AS T2(EventData)  
)  
SELECT * FROM CTE2;  
```

## <a name="see-also"></a>另请参阅

 [Azure SQL 数据库中的扩展事件](/azure/azure-sql/database/xevent-db-diff-from-svr)  
