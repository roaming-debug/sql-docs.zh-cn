创建资源调控器工作负荷组并将工作负荷组与资源调控器资源池关联。 不是 [!INCLUDE[msCoName](msconame-md.md)][!INCLUDE[ssNoVersion](ssnoversion-md.md)] 的所有版本都提供资源调控器。 有关 [!INCLUDE[ssNoVersion](ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。

:::image type="icon" source="../database-engine/configure-windows/media/topic-link.gif"::: [Transact-SQL 语法约定](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

## <a name="syntax"></a>语法

```syntaxsql
CREATE WORKLOAD GROUP group_name
[ WITH
    ( [ IMPORTANCE = { LOW | MEDIUM | HIGH } ]
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]
      [ [ , ] MAX_DOP = value ]
      [ [ , ] GROUP_MAX_REQUESTS = value ] )
 ]
[ USING {
    [ pool_name | "default" ]
    [ [ , ] EXTERNAL external_pool_name | "default" ] ]
    } ]
[ ; ]
```

## <a name="arguments"></a>参数

group_name </br>
是工作负荷组的用户定义名称。 group_name 由字母数字组成，最多可包含 128 个字符，在 [!INCLUDE[ssNoVersion](ssnoversion-md.md)] 实例中必须是唯一的，并且必须符合[标识符](../relational-databases/databases/database-identifiers.md)规则  。

IMPORTANCE = { LOW | MEDIUM | HIGH } </br>
指定工作负荷组中某个请求的相对重要性。 重要性为下列值之一，默认值为 MEDIUM：

- LOW
- MEDIUM（默认值）
- HIGH

> [!NOTE]
> 在内部，每个重要性设置都存储为用于计算的一个数字。

IMPORTANCE 对资源池而言是局部性的；同一资源池内重要性不同的工作负荷组会相互影响，但不会影响其他资源池中的工作负荷组。

REQUEST_MAX_MEMORY_GRANT_PERCENT = value </br>
指定单个请求可以从池中获取的最大内存量。 value 是相对于 MAX_MEMORY_PERCENT 指定的资源池大小的百分比  。

在 Azure SQL 托管实例中，value 包含一个不超过 [!INCLUDE[ssSQL17](sssql17-md.md)] 的整数和一个以 [!INCLUDE[sql-server-2019](sssql19-md.md)] 开头的浮点数。 默认值为 25。 value 的允许范围是 1 到 100  。

> [!IMPORTANT]  
> 指定的量指的只是查询执行授予内存。
>
> 将 value 设置为 0 可阻止在用户定义的工作负荷组中运行具有 SORT 和 HASH JOIN 操作的查询  。
>
> 建议不要将 value 设置为大于 70，这是因为如果正在运行其他并发查询，则服务器可能无法保留足够的空闲内存  。 可能最终会导致查询超时错误 8645。
>
> 如果查询内存要求超过了此参数指定的限制，服务器会执行以下操作：
>
> - 对于用户定义的工作负荷组，服务器会尝试降低查询的并行度，直到内存要求降到限制范围以内，或直到并行度等于 1。 如果查询内存要求仍然大于限制值，则会发生错误 8657。
>
> - 对于内部和默认工作负荷组，服务器会允许查询获取必需的内存。
>
> 请注意，如果服务器没有足够的物理内存，则这两种情况都会出现超时错误 8645。

REQUEST_MAX_CPU_TIME_SEC = value </br>
指定请求可以使用的最长 CPU 时间，以秒为单位。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，也就是说无限制  。

> [!NOTE]
> 默认情况下，如果超过最长时间，Resource Governor 并不会阻止继续发出请求。 但会生成一个事件。 有关详细信息，请参阅 [CPU Threshold Exceeded 事件类](../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md)。
> [!IMPORTANT]
> 从 [!INCLUDE[sssql16-md](sssql16-md.md)] SP2 和 [!INCLUDE[ssSQL17](sssql17-md.md)] CU3 开始以及使用[跟踪标志 2422](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 时，Resource Governor 将在超出最大时间时终止请求。

REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value </br>
指定查询等待内存授予（工作缓冲区内存）变为可用状态的最长时间（以秒为单位）。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，表示使用基于查询开销的内部计算来确定最长时间  。

> [!NOTE]
> 查询并不总是在达到内存授予超时的时候失败。 仅当有太多并发查询运行时，查询才失败。 否则，查询只能获取最小内存授予，从而导致查询性能下降。

MAX_DOP = value </br>
指定并行查询执行的最大并行度 (MAXDOP)。  value 必须为 0 或一个正整数  。 value 的允许范围为 0 到 64  。 value 的默认设置为 0，表示使用全局设置  。 按如下方式处理 MAX_DOP：

> [!NOTE]
> 工作负荷组 MAX_DOP 会覆盖[最大并行度的服务器配置](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)和 MAXDOP [数据库范围的配置](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)  。

> [!TIP]
> 要在查询级别完成此操作，请使用 MAXDOP [查询提示](../t-sql/queries/hints-transact-sql-query.md)  。 将最大并行度设置为查询提示时，在未超出工作负荷组 MAX_DOP 时保持有效。 如果 MAXDOP 查询提示值超出使用资源调控器配置的值，则 [!INCLUDE[ssDEnoversion](ssdenoversion-md.md)] 使用资源调控器 `MAX_DOP` 值。 MAXDOP [查询提示](../t-sql/queries/hints-transact-sql-query.md)始终会覆盖[最大并行度的服务器配置](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。
>
> 要在数据库级别完成此操作，请使用 MAXDOP [数据库范围的配置](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)  。
>
> 要在服务器级别完成此操作，请使用“最大并行度 (MAXDOP)”[服务器配置选项](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)  。

GROUP_MAX_REQUESTS = value </br>
指定在工作负荷组中允许执行的同时请求最大数。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，表示允许的请求数不限  。 当达到最大并发请求数时，该组中的用户可以登录但置于等待状态，直至并发请求数降到指定值之下。

USING { pool_name | "default" }  </br>
将工作负荷组与由 pool_name 标识的用户定义的资源池关联起来  。 这实际上是将工作负荷组放入资源池中。 如果没有提供 pool_name，或如果没有使用 USING 参数，将工作负荷组放入预定义的资源调控器默认池  。

"default" 是保留字，并且在与 USING 一起使用时，必须使用引号 ("") 引起来或用方括号 ([]) 括起来。

> [!NOTE]
> 预定义工作负荷组和资源池都使用小写名称，例如“default”。 对于使用区分大小写排序规则的服务器，应当注意这一点。 使用不区分大小写排序规则的服务器（例如 SQL_Latin1_General_CP1_CI_AS）会将“default”和“Default”视为相同。

EXTERNAL external_pool_name | “default“</br>
适用范围：[!INCLUDE[ssNoVersion](ssnoversion-md.md)]（从 [!INCLUDE[sssql16-md](sssql16-md.md)] 开始）  。

工作负荷组可以指定一个外部资源池。 可定义一个工作负荷组并关联两个池：

- [!INCLUDE[ssNoVersion](ssnoversion-md.md)] 工作负荷和查询的资源池
- 外部进程的外部资源池。 有关详细信息，请参阅 [sp_execute_external_script (Transact-SQL)](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)。

## <a name="remarks"></a>备注

使用 `REQUEST_MEMORY_GRANT_PERCENT` 时，允许索引创建操作使用比最初授予的工作区内存更多的工作区内存以提高性能。 [!INCLUDE[ssCurrent](ssnoversion-md.md)] 中的资源调控器支持这种特殊的处理方法。 然而，最初授予及任何其他内存授予都受资源池和工作负荷组设置的限制。

将按[任务](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)设置 `MAX_DOP` 限制。 它不是按[请求](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)限制或按查询限制。 这意味着，在并行查询期间，单个请求可以生成多个任务，然后将它们分配给[计划程序](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)。 有关详细信息，请参阅[线程和任务体系结构指南](../relational-databases/thread-and-task-architecture-guide.md)。

如果使用 `MAX_DOP` 并在编译时将查询标记为串行，则在运行时无法更改回并行，不论工作负荷组或服务器配置如何设置。 配置 `MAX_DOP` 后，只能在内存不足时降低它。 工作负荷组重新配置在授予内存队列中等待时不可见。

### <a name="index-creation-on-a-partitioned-table"></a>对分区表创建索引

对非对齐的已分区表创建索引所占用的内存与涉及的分区数成正比。 如果所需的内存总量超过 Resource Governor 工作负荷组设置规定的每个查询的限制 `REQUEST_MAX_MEMORY_GRANT_PERCENT`，则可能无法执行此索引创建。 由于 "default" 工作负荷组允许查询超过每个查询的限制，并在开始时使用所需的最低内存，因此，如果 "default" 资源池配置了足够多的内存总量以运行此类查询，用户或许能够在 "default" 工作负荷组中运行相同的索引创建    。

## <a name="permissions"></a>权限

需要 `CONTROL SERVER` 权限。

## <a name="example"></a>示例

创建名称为 `newReports` 的工作负荷组，它使用资源调控器默认设置并位于资源调控器默认池中。 该示例指定了 `default` 池，但这并非必需的。

```sql
CREATE WORKLOAD GROUP newReports
WITH
    (REQUEST_MAX_MEMORY_GRANT_PERCENT = 2.5
      , REQUEST_MAX_CPU_TIME_SEC = 100
      , MAX_DOP = 4)    
USING "default" ;
GO
```

## <a name="see-also"></a>另请参阅

- [ALTER 工作负荷组 (Transact-SQL)](../t-sql/statements/alter-workload-group-transact-sql.md)
- [DROP 工作负荷组 (Transact-SQL)](../t-sql/statements/drop-workload-group-transact-sql.md)
- [创建资源池 (Transact-SQL)](../t-sql/statements/create-resource-pool-transact-sql.md)
- [ALTER RESOURCE POOL (Transact-SQL)](../t-sql/statements/alter-resource-pool-transact-sql.md)
- [DROP RESOURCE POOL (Transact-SQL)](../t-sql/statements/drop-resource-pool-transact-sql.md)
- [ALTER RESOURCE GOVERNOR (Transact-SQL)](../t-sql/statements/alter-resource-governor-transact-sql.md)