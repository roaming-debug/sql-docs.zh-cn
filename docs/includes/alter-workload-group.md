  更改现有 Resource Governor 工作负载组配置，并且可以选择将其分配给 Resource Governor 资源池。  
  
 ![主题链接图标](../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER WORKLOAD GROUP { group_name | "default" }  
[ WITH  
    ([ IMPORTANCE = { LOW | MEDIUM | HIGH } ]  
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]  
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]  
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]   
      [ [ , ] MAX_DOP = value ]  
      [ [ , ] GROUP_MAX_REQUESTS = value ] )  
 ]  
[ USING { pool_name | "default" } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>参数

 group_name | "default"         
 是现有用户定义工作负荷组或资源调控器默认工作负荷组的名称。  
  
> [!NOTE]  
> 在安装 [!INCLUDE[ssNoVersion](ssnoversion-md.md)] 时资源调控器创建“默认”和内部组。  
  
 与 ALTER WORKLOAD GROUP 一起使用时，选项 "default" 必须用引号 ("") 引起来或用方括号 ([]) 括起来，以免与系统保留字 DEFAULT 冲突。 有关详细信息，请参阅 [Database Identifiers](../relational-databases/databases/database-identifiers.md)。  
  
> [!NOTE]  
> 预定义工作负荷组和资源池都使用小写名称，例如“default”。 对于使用区分大小写排序规则的服务器，应当注意这一点。 使用不区分大小写排序规则的服务器（例如 SQL_Latin1_General_CP1_CI_AS）会将“default”和“Default”视为相同。  
  
 IMPORTANCE = { LOW | MEDIUM | HIGH }        
 指定工作负荷组中某个请求的相对重要性。 重要性为以下值之一：  
  
-   LOW  
-   MEDIUM（默认值）  
-   HIGH  
  
> [!NOTE]  
> 在内部，每个重要性设置都存储为用于计算的一个数字。  
  
 IMPORTANCE 对资源池而言是局部性的；同一资源池内重要性不同的工作负荷组会相互影响，但不会影响其他资源池中的工作负荷组。  
  
 REQUEST_MAX_MEMORY_GRANT_PERCENT = value      
 指定单个请求可以从池中获取的最大内存量。 value 是相对于 MAX_MEMORY_PERCENT 指定的资源池大小的百分比  。  

value 是一个最大为 [!INCLUDE[ssSQL17](sssql17-md.md)] 的整数，以及一个以 [!INCLUDE[sql-server-2019](sssqlv15-md.md)] 开头的浮点数  。 默认值为 25。 value 的允许范围是 1 到 100  。
  
> [!NOTE]  
> 指定的量指的只是查询执行授予内存。  
  
> [!IMPORTANT]
> 将 value 设置为 0 可阻止在用户定义的工作负荷组中运行具有 SORT 和 HASH JOIN 操作的查询  。     
>
> 建议不要将 value 设置为大于 70，这是因为如果正在运行其他并发查询，则服务器可能无法保留足够的空闲内存  。 可能最终会导致查询超时错误 8645。      
  
> [!NOTE]  
> 如果查询内存要求超过了此参数指定的限制，服务器会执行以下操作：  
>   
> -  对于用户定义的工作负荷组，服务器会尝试降低查询的并行度，直到内存要求降到限制范围以内，或直到并行度等于 1。 如果查询内存要求仍然大于限制值，则会发生错误 8657。  
>   
> -  对于内部和默认工作负荷组，服务器会允许查询获取必需的内存。  
>   
> 请注意，如果服务器没有足够的物理内存，则这两种情况都会出现超时错误 8645。  
  
 REQUEST_MAX_CPU_TIME_SEC = value        
 指定请求可以使用的最长 CPU 时间，以秒为单位。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，也就是说无限制  。  
  
> [!NOTE]  
> 默认情况下，如果超过最长时间，Resource Governor 并不会阻止继续发出请求。 但会生成一个事件。 有关详细信息，请参阅 [CPU Threshold Exceeded 事件类](../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md)。 

> [!IMPORTANT]
> 从 [!INCLUDE[ssSQL15](sssql16-md.md)] SP2 和 [!INCLUDE[ssSQL17](sssql17-md.md)] CU3 开始以及使用[跟踪标志 2422](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 时，Resource Governor 将在超出最大时间时终止请求。
  
 REQUEST_MEMORY_GRANT_TIMEOUT_SEC =value   
 指定查询等待内存授予（工作缓冲区内存）变为可用的最长时间（以秒为单位）。  
  
> [!NOTE]  
> 查询并不总是在达到内存授予超时的时候失败。 仅当有太多并发查询运行时，查询才失败。 否则，查询只能获取最小内存授予，从而导致查询性能下降。  
  
 value 必须是正整数  。 value 的默认设置为 0，表示使用基于查询开销的内部计算来确定最长时间  。  
  
 MAX_DOP = value        
 指定并行请求的最大并行度 (DOP)。 value 必须为 0 或正整数（1 到 255）  。 value 为 0 时，服务器选择最大并行度  。 这是默认设置，也是推荐设置。  
  
> [!NOTE]  
> [!INCLUDE[ssDE](ssde-md.md)]为 MAX_DOP 设置的实际值可能小于指定值。 最终值由公式 min(255, CPU 的数目) 确定  。  
  
> [!CAUTION]  
> 更改 MAX_DOP 可能会对服务器的性能产生不利影响。 如果必须更改 MAX_DOP，我们建议将其设置为小于或等于在单个 NUMA 节点中存在的硬件计划程序的最大数目。 我们建议您不要将 MAX_DOP 设置为大于 8 的值。  
  
 按如下方式处理 MAX_DOP：  
  
-   只要作为查询提示的 MAX_DOP 不超过工作负荷组 MAX_DOP，便遵守作为查询提示的 MAX_DOP。  
  
-   作为查询提示的 MAX_DOP 始终会覆盖 sp_configure 'max degree of parallelism'。  
  
-   工作负荷组 MAX_DOP 覆盖 sp_configure 'max degree of parallelism'。  
  
-   如果查询在编译时标记为串行 (MAX_DOP = 1)，则无论工作负载组或 sp_configure 的设置如何，在运行时该查询都无法改回并行。  
  
 配置 DOP 后，只能在授予内存不足时降低它。 工作负荷组重新配置在授予内存队列中等待时不可见。  
  
 GROUP_MAX_REQUESTS = value       
 指定在工作负荷组中允许执行的同时请求最大数。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，表示允许的请求数不限  。 当达到最大并发请求数时，该组中的用户可以登录但置于等待状态，直至并发请求数降到指定值之下。  
  
 USING { pool_name | "default" }        
 将工作负荷组与由 pool_name 标识的用户定义资源池关联起来，这实际上是将此工作负荷组放入资源池中  。 如果没有提供 pool_name，或如果没有使用 USING 参数，则将工作负荷组放入预定义的资源调控器默认池  。  
  
 与 ALTER WORKLOAD GROUP 一起使用时，选项 "default" 必须用引号 ("") 引起来或用方括号 ([]) 括起来，以免与系统保留字 DEFAULT 冲突。 有关详细信息，请参阅 [Database Identifiers](../relational-databases/databases/database-identifiers.md)。  
  
> [!NOTE]  
> 选项 "default" 区分大小写。  
  
## <a name="remarks"></a>备注  
 允许对默认组使用 ALTER WORKLOAD GROUP。  
  
 对工作负荷组配置的更改直到执行 ALTER RESOURCE GOVERNOR RECONFIGURE 后才会生效。 在更改计划影响到设置时，只有在执行 DBCC FREEPROCCACHE (pool_name  ) 后，  新设置才会在之前已缓存的计划中生效，其中 pool_name 是与工作负载组相关联的 Resource Governor 资源池的名称。  
  
-   如果将 MAX_DOP 更改为 1，则由于并行计划可以在串行模式中运行，因此不需要执行 DBCC FREEPROCCACHE。 但是，它可能不如编译为串行计划的计划那么有效。  
  
-   如果将 MAX_DOP 从 1 更改为 0 或大于 1 的值，则不需要执行 DBCC FREEPROCCACHE。 但串行计划不能并行运行，因此清除相应的缓存将允许使用并行编译新计划。  
  
> [!CAUTION]  
> 从多个工作负载组关联的资源池中清除缓存计划将影响用户定义资源池由 pool_name 标识的所有工作负载  。  
  
 执行 DDL 语句时，建议先熟悉 Resource Governor 状态。 有关详细信息，请参阅 [Resource Governor](../relational-databases/resource-governor/resource-governor.md)。  
  
 REQUEST_MEMORY_GRANT_PERCENT：在 [!INCLUDE[ssVersion2005](ssversion2005-md.md)] 中，允许索引创建操作使用比最初授予的工作区内存多的工作区内存，以便提高性能。 这个特别处理在更高版本中由资源调控器支持，然而，最初授予及任何其他内存授予都受资源池和工作负荷组设置的限制。  
  
 **对已分区表创建索引**  
  
 对非对齐的已分区表创建索引所占用的内存与涉及的分区数成正比。  如果所需的内存总量超过资源调控器工作负荷组设置为每个查询设定的限制 (REQUEST_MAX_MEMORY_GRANT_PERCENT)，则可能无法执行这种索引创建。 由于“默认”工作负荷组允许查询超过每个查询的限制，并在开始时使用所需的最低内存以便与 [!INCLUDE[ssVersion2005](ssversion2005-md.md)] 保持兼容，因此，如果“默认”资源池配置了足够多的内存总量以运行此类查询，则用户或许能够在“默认”工作负荷组中运行相同的索引创建。  
  
## <a name="permissions"></a>权限  
 需要 `CONTROL SERVER` 权限。  
  
## <a name="examples"></a>示例  
 下面的示例说明了如何将默认组中请求的重要性从 `MEDIUM` 更改为 `LOW`。  
  
```sql  
ALTER WORKLOAD GROUP "default"  
WITH (IMPORTANCE = LOW);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 下面的示例说明了如何将一个工作负载组从它所在的池中移到默认池中。  
  
```sql  
ALTER WORKLOAD GROUP adHoc  
USING [default];  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [资源调控器](../relational-databases/resource-governor/resource-governor.md)   
 [CREATE WORKLOAD GROUP (Transact-SQL)](../t-sql/statements/create-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP (Transact-SQL)](../t-sql/statements/drop-workload-group-transact-sql.md)   
 [CREATE RESOURCE POOL (Transact-SQL)](../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL (Transact-SQL)](../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL (Transact-SQL)](../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../t-sql/statements/alter-resource-governor-transact-sql.md)  
