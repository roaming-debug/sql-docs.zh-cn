---
title: 数据库级别运行状况检测
description: 了解适用于 SQL Server Always On 可用性组的数据库级别运行状况检测功能。
ms.custom: seo-lt-2019
ms.date: 01/19/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- AlwaysOn
- DB_FAILOVER
- Always On
- High Availability
- SQL Server
ms.assetid: d74afd28-25c3-48a1-bc3f-e353bee615c2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 14a7b2fbb5834ae45c5262f73ffc81a684bba300
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583825"
---
# <a name="availability-group-database-level-health-detection-failover-option"></a>可用性组数据库级别运行状况检测故障转移选项
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
从 SQL Server 2016 开始，配置 AlwaysOn 可用性组时可使用数据库级别运行状况检测 (DB_FAILOVER) 选项。 数据库级别运行状况检测可检测到数据库不再处于联机状态以及其他问题，然后将触发可用性组的自动故障转移。

数据库级别运行状况检测针对整个可用性组启用，因此数据库级别运行状况检测会监视可用性组中的每一个数据库。 不能针对可用性组中的特定数据库有选择地启用此选项。

## <a name="benefits-of-database-level-health-detection-option"></a>数据库级别运行状况检测选项的优点
---
可用性组数据库级别运行状况检测选项是一个广泛推荐的有用选项，有助于保证数据库的高可用性。 应考虑对所有可用性组启用此选项。 如果应用程序依赖几个数据库实现高可用性，则可将这些数据库分在一个可用性组中并启用数据库运行状况选项。

例如，在数据库级别运行状况检测选项处于启用状态时，如果 SQL Server 不能写入其中一个数据库的事务日志文件，该数据库的状态会改为指示故障，可用性组会快速故障转移，然后应用程序可能重新连接，并在数据库再次联机后继续努力将中断次数降到最低。

### <a name="enabling-database-level-health-detection"></a>启用数据库级别运行状况检测

尽管通常建议使用“数据库运行状况”选项，但此选项“默认处于禁用状态”，目的是与以前版本的默认设置保持后向兼容  。

可通过几种简单的方式来启用数据库级别运行状况检测设置：

1. 在 SQL Server Management Studio 中，连接到 SQL Server 数据库引擎。 使用“对象资源管理器”窗口，右键单击“AlwaysOn 高可用性”节点，然后运行“新建可用性组向导”  。 勾选“指定名称”页面上的“数据库级别运行状况检测”复选框  。 然后完成向导中的其余页面。

   ![AlwaysOn“启用数据库运行状况”复选框](../../../database-engine/availability-groups/windows/media/always-on-enable-database-health-checkbox.png)

2. 在 SQL Server Management Studio 中查看现有可用性组的“属性”  。 连接到 SQL Server。 使用“对象资源管理器”窗口，展开“AlwaysOn 高可用性”节点。 展开可用性组。 右键单击该可用性组，然后选择“属性”。 勾选“数据库级别运行状况检测”选项，然后单击“确定”或编写更改脚本  。

   ![Alwayson 可用性组属性数据库级别运行状况检测](../../../database-engine/availability-groups/windows/media/always-on-ag-properties-database-level-health-detection.png)


3. 用于执行 CREATE AVAILABILITY GROUP 的 Transact-SQL 语法  。 DB_FAILOVER 参数接受值 ON 或 OFF。

   ```sql
   CREATE AVAILABILITY GROUP [Contoso-ag]
   WITH (DB_FAILOVER=ON)
   FOR DATABASE [AutoHa-Sample]
   REPLICA ON
       N'SQLSERVER-0' WITH (ENDPOINT_URL = N'TCP://SQLSERVER-0.DOMAIN.COM:5022',
         FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
       N'SQLSERVER-1' WITH (ENDPOINT_URL = N'TCP://SQLSERVER-1.DOMAIN.COM:5022',
        FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
    ```

4. 用于执行 ALTER AVAILABILITY GROUP 的 Transact-SQL 语法  。 DB_FAILOVER 参数接受值 ON 或 OFF。

   ```sql
   ALTER AVAILABILITY GROUP [Contoso-ag] SET (DB_FAILOVER = ON);

   ALTER AVAILABILITY GROUP [Contoso-ag] SET (DB_FAILOVER = OFF);
   ```

### <a name="caveats"></a>注意事项

请务必注意，“数据库级别运行状况检测”选项当前不会引起 SQL Server 监视磁盘运行时间，并且 SQL Server 不会直接监视数据库文件可用性。 如果磁盘驱动器故障或不可用，单独这一问题不一定会触发可用性组执行自动故障转移。

举例来说，当数据库处于没有活动事务且没有物理写入的空闲状态时，如果某些数据库文件不可访问，SQL Server 可能不会向文件执行任何读写 IO 的操作，可能也不会立刻更改该数据库的状态，因此不会触发故障转移。 随后，当出现数据库检查点或发生物理读写以完成查询时，SQL Server 可能此时才发现文件问题并通过更改数据库状态做出反应，然后，由于数据库运行状况更改，已启用数据库级别运行状况监测的可用性组将发生故障转移。

另一个例子是，当 SQL Server 数据库引擎需要读取数据页面以完成查询时，如果数据页面在缓冲池内存中缓存，则可能不需要通过物理访问进行磁盘读取即可完成查询请求。 因此，数据文件丢失或不可用可能不会立刻触发故障转移（即使已启用数据库运行状况选项），因为数据库状态不会立即更改。


## <a name="database-failover-is-separate-from-flexible-failover-policy"></a>数据库故障转移与灵活的故障转移策略相互独立
数据库级别运行状况检测可实现灵活的故障转移策略，用于针对故障转移策略配置 SQL Server 进程运行状况的阈值。 数据库级别运行状况检测使用 DB_FAILOVER 参数进行配置，而可用性组选项 FAILURE_CONDITION_LEVEL 单独用于配置 SQL Server 进程运行状况检测。 两个选项是独立的。

## <a name="managing-and-monitoring-database-level-health-detection"></a>管理和监视数据库级别运行状况检测

### <a name="dynamic-management-views"></a>动态管理视图

系统 DMV sys.availability_groups 显示 db_failover 列，该列指示数据库级别运行状况检测选项为关 (0) 还是开 (1)。

```sql
select name, db_failover from sys.availability_groups
```


dmv 输出示例：

|name  |  db_failover|
|---------|---------|
| Contoso-ag | 1  |

### <a name="errorlog"></a>ErrorLog
当可用性组由于执行数据库级别运行状况检测发生故障转移后，SQL Server 错误日志（或 sp_readerrorlog 中的文本）将显示错误消息 41653。

例如，此错误日志摘录显示由于磁盘问题导致事务日志写入失败，随后名为 AutoHa-Sample 的数据库关闭，进而触发数据库级别运行状况检测，使可用性组执行故障转移。

>2016-04-25 12:20:21.08 spid1s      错误:17053，严重性:16，状态：1.
>
>2016-04-25 12:20:21.08 spid1s      SQLServerLogMgr::LogWriter:遇到操作系统错误 21 (设备未就绪)。
>2016-04-25 12:20:21.08 spid1s      日志刷新过程中出现写入错误。
>
>2016-04-25 12:20:21.08 spid79      错误:9001，严重性:21，状态:4.
>
>2016-04-25 12:20:21.08 spid79      数据库“AutoHa-Sample”的日志不可用。 有关相应错误消息，请查看事件日志。 修复所有错误后重新启动数据库。
>
>**2016-04-25 12:20:21.15 spid79      错误:41653，严重性:21，状态:1。**
>
>**2016-04-25 12:20:21.15 spid79      数据库“AutoHa-Sample”遇到错误(错误类型:2 'DB_SHUTDOWN')，导致可用性组“Contoso-ag”发生故障。有关所遇到的错误的信息，请参阅 SQL Server 错误日志。如果此状况继续存在，请与系统管理员联系。**
>
>2016-04-25 12:20:21.17 spid79      数据库“AutoHa-Sample”的状态信息 - 强化的 Lsn:“(34:664:1)”    提交 LSN:“(34:656:1)”    提交时间:“Apr 25 2016 12:19PM”
>
>2016-04-25 12:20:21.19 spid15s     AlwaysOn 可用性组与辅助数据库的连接终止，因为主要数据库“AutoHa-Sample”位于可用性副本“SQLServer-0”上，其副本 ID为: {c4ad5ea4-8a99-41fa-893e-189154c24b49}。 这只是一条信息性消息。 不需要任何用户操作。
>
>2016-04-25 12:20:21.21 spid75      AlwaysOn:可用性组“Contoso-ag”的本地副本正在准备转换为解析角色，以便响应来自 Windows Server 故障转移群集(WSFC)群集的请求。 这只是一条信息性消息。 不需要任何用户操作。
>
>2016-04-25 12:20:21.21 spid75      可用性组“ag”中本地可用性副本的状态已从“PRIMARY_NORMAL”更改为“RESOLVING_NORMAL”。  由于可用性组脱机，因此状态发生更改。  副本脱机的原因包括：已删除关联的可用性组，用户已将 Windows Server 故障转移群集 (WSFC) 管理控制台中的关联可用性组设置为离线，或可用性组正在故障转移到其他 SQL Server 实例。  有关详细信息，请参阅 SQL Server 错误日志、Windows Server 故障转移群集 (WSFC) 管理控制台或 WSFC 日志。

### <a name="extended-event-sqlserveravailability_replica_database_fault_reporting"></a>扩展事件 sqlserver.availability_replica_database_fault_reporting

从 SQL Server 2016 开始，定义了一个新的扩展事件，该事件通过数据库级别运行状况检测触发。  事件名称为 sqlserver.availability_replica_database_fault_reporting 

此 XEvent 仅在主要副本上触发。 检测到可用性组中托管的数据库存在数据库级别运行状况问题时会触发此 XEvent。

以下是创建捕获此事件的 XEvent 会话的示例。 如未指定路径，XEvent 输出文件应位于默认的 SQL Server 错误日志路径中。 在可用性组的主要副本上执行此操作：

扩展事件会话脚本示例

```sql
CREATE EVENT SESSION [AlwaysOn_dbfault] ON SERVER
ADD EVENT sqlserver.availability_replica_database_fault_reporting
ADD TARGET package0.event_file(SET filename=N'dbfault.xel',max_file_size=(5),max_rollover_files=(4))
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,
    MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)
GO
ALTER EVENT SESSION AlwaysOn_dbfault ON SERVER STATE=START
GO
```

#### <a name="extended-event-output"></a>扩展事件输出
使用 SQL Server Management Studio 连接到主 SQL Server，展开“管理”节点，然后展开“扩展事件”。 找到会话（上面示例中的名称为 AlwaysOn_dbfault），然后将其展开，查看输出文件。 单击输出文件，随即将在一个新的选项卡中打开事件文件。

字段说明：

|列数据 | 说明|
|---------|---------|
|availability_group_id |可用性组的 ID。|
|availability_group_name |可用性组的名称。|
|availability_replica_id |可用性副本的 ID。|
|availability_replica_name |可用性副本的名称。|
|database_name |报告错误的数据库的名称。|
|database_replica_id |可用性副本数据库的 ID。|
|failover_ready_replicas |同步的自动故障转移次要副本数。|
|fault_type  | 报告的错误 ID。 可能的值：  <br/> 0 - 无 <br/>1 - 未知<br/>2 - 关闭|
|is_critical | 从 SQL Server 2016 开始，对于 XEvent 此值应始终返回 true。|


在此示例输出中，fault_type 显示：由于名为 AutoHa-Sample2 的数据库出错（错误类型为 2- 关闭），SQLSERVER-1 副本上的可用性组 Contoso-ag 发生了严重事件。

|字段  | 值|
|---------|---------|
|availability_group_id | 24E6FE58-5EE8-4C4E-9746-491CFBB208C1|
|availability_group_name | Contoso-ag|
|availability_replica_id | 3EAE74D1-A22F-4D9F-8E9A-DEFF99B1F4D1|
|availability_replica_name | SQLSERVER-1|
|database_name | AutoHa-Sample2|
|database_replica_id | 39971379-8161-4607-82E7-098590E5AE00|
|failover_ready_replicas | 1|
|fault_type | 2|
|is_critical | True|


### <a name="related-references"></a>相关参考

* [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)

* [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)

* [针对可用性组的自动故障转移的灵活的故障转移策略 (SQL Server)](./configure-flexible-automatic-failover-policy.md)

* [Enhance AlwaysOn Failover Policy to Test SQL Server Database Data and Log Drives](/archive/blogs/alwaysonpro/enhance-alwayson-failover-policy-to-test-sql-server-database-data-and-log-drives)（增强 AlwaysOn 故障转移策略以测试 SQL Server 数据库数据和日志驱动器）

* [扩展事件](../../../relational-databases/extended-events/extended-events.md)