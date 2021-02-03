---
description: ALTER EVENT SESSION (Transact-SQL)
title: ALTER EVENT SESSION (Transact-SQL) &#124; Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER EVENT SESSION
- ALTER_EVENT_SESSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- event sessions [SQL Server]
- extended events [SQL Server], Transact-SQL
- ALTER EVENT SESSION statement
ms.assetid: da006ac9-f914-4995-a2fb-25b5d971cd90
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a48973fcbd4aa030d41b5bdce3e0c10f37ce8efc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204282"
---
# <a name="alter-event-session-transact-sql"></a>ALTER EVENT SESSION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  启动或停止事件会话，或更改事件会话配置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER EVENT SESSION event_session_name  
ON SERVER  
{  
    [ [ {  <add_drop_event> [ ,...n] }     
       | { <add_drop_event_target> [ ,...n ] } ]   
    [ WITH ( <event_session_options> [ ,...n ] ) ]  
    ]  
    | [ STATE = { START | STOP } ]  
}  
  
<add_drop_event>::=  
{  
    [ ADD EVENT <event_specifier>   
         [ ( {   
                 [ SET { event_customizable_attribute = <value> [ ,...n ] } ]  
                 [ ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n ] } ) ]  
                 [ WHERE <predicate_expression> ]  
        } ) ]  
   ]   
   | DROP EVENT <event_specifier> }  
  
<event_specifier> ::=  
{  
[event_module_guid].event_package_name.event_name  
}  
  
<predicate_expression> ::=   
{  
    [ NOT ] <predicate_factor> | {( <predicate_expression> ) }   
    [ { AND | OR } [ NOT ] { <predicate_factor> | ( <predicate_expression> ) } ]   
    [ ,...n ]  
}  
  
<predicate_factor>::=   
{  
    <predicate_leaf> | ( <predicate_expression> )  
}  
  
<predicate_leaf>::=  
{  
      <predicate_source_declaration> { = | < > | ! = | > | > = | < | < = } <value>   
    | [event_module_guid].event_package_name.predicate_compare_name ( <predicate_source_declaration>, <value> )   
}  
  
<predicate_source_declaration>::=   
{  
    event_field_name | ( [event_module_guid].event_package_name.predicate_source_name )  
}  
  
<value>::=   
{  
    number | 'string'  
}  
  
<add_drop_event_target>::=  
{  
    ADD TARGET <event_target_specifier>  
        [ ( SET { target_parameter_name = <value> [ ,...n] } ) ]  
    | DROP TARGET <event_target_specifier>  
}  
  
<event_target_specifier>::=  
{  
    [event_module_guid].event_package_name.target_name  
}  
  
<event_session_options>::=  
{  
    [    MAX_MEMORY = size [ KB | MB] ]  
    [ [,] EVENT_RETENTION_MODE = { ALLOW_SINGLE_EVENT_LOSS | ALLOW_MULTIPLE_EVENT_LOSS | NO_EVENT_LOSS } ]  
    [ [,] MAX_DISPATCH_LATENCY = { seconds SECONDS | INFINITE } ]  
    [ [,] MAX_EVENT_SIZE = size [ KB | MB ] ]  
    [ [,] MEMORY_PARTITION_MODE = { NONE | PER_NODE | PER_CPU } ]  
    [ [,] TRACK_CAUSALITY = { ON | OFF } ]  
    [ [,] STARTUP_STATE = { ON | OFF } ]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
  
|术语|定义|  
|-|-|
|event_session_name|是现有事件会话的名称。|  
|STATE = START &#124; STOP|启动或停止事件会话。 此参数仅在 ALTER EVENT SESSION 应用到事件会话对象时才有效。|  
|ADD EVENT \<event_specifier>|将由 \<event_specifier> 标识的事件与该事件会话建立关联。|
|[event_module_guid].event_package_name.event_name |是事件包中某个事件的名称，其中：<br /><br /> -   event_module_guid 为包含该事件的模块的 GUID。<br />-   event_package_name 为包含操作对象的包。<br />-   event_name 为事件对象。<br /><br /> 事件在 sys.dm_xe_objects 视图中显示为 object_type 'event'。|  
|SET { event_customizable_attribute= \<value> [ ,...n] }|为该事件指定可自定义的属性。 可自定义属性在 sys.dm_xe_object_columns 视图中显示为 column_type 'customizable' 以及 object_name = event_name。|  
|ACTION ( { [event_module_guid].event_package_name.action_name [ ,...n] } ) |要与事件会话关联的操作，其中：<br /><br /> -   event_module_guid 为包含该事件的模块的 GUID。<br />-   event_package_name 为包含操作对象的包。<br />-   action_name 是操作对象。<br /><br /> 操作在 sys.dm_xe_objects 视图中显示为 object_type 'action'。|  
|WHERE \<predicate_expression>|指定用于确定是否应处理事件的谓词表达式。 如果 \<predicate_expression> 为 true，则由会话的操作和目标对事件做进一步处理。 如果 \<predicate_expression> 为 false，则在会话的操作和目标处理事件之前由会话删除该事件。 谓词表达式限制在 3000 个字符，这限制了字符串参数。|
|event_field_name|表示标识谓词源的事件字段的名称。|  
|[event_module_guid].event_package_name.predicate_source_name|表示全局谓词源的名称，其中：<br /><br /> -   event_module_guid 为包含该事件的模块的 GUID。<br />-   event_package_name 为包含谓词对象的包。<br />-   predicate_source_name 在 sys.dm_xe_objects 视图中定义为 object_type 'pred_source'。|  
|[event_module_guid].event_package_name.predicate_compare_name  |要与事件关联的谓词对象的名称，其中：<br /><br /> -   event_module_guid 为包含该事件的模块的 GUID。<br />-   event_package_name 为包含谓词对象的包。<br />-   predicate_compare_name 为在 sys.dm_xe_objects 视图中定义为 object_type 'pred_compare' 的全局源。|  
|DROP EVENT \<event_specifier>|删除由 \<event_specifier> 标识的事件。 \<event_specifier> 在事件会话中必须有效。|  
|ADD TARGET \<event_target_specifier>|将由 \<event_target_specifier> 标识的目标与事件会话建立关联。|
|[event_module_guid].event_package_name.target_name  |为事件会话中目标的名称，其中：<br /><br /> -   event_module_guid 为包含该事件的模块的 GUID。<br />-   event_package_name 为包含操作对象的包。<br />-   target_name 为操作。 操作在 sys.dm_xe_objects 视图中显示为 object_type 'target'。|  
|SET { target_parameter_name= \<value> [, ...n] }|设置目标参数。 目标参数在 sys.dm_xe_object_columns 视图中显示为 column_type 'customizable' 以及 object_name = target_name。<br /><br /> **注意！！** 如果您在使用环形缓冲区目标，我们建议您将 max_memory 目标参数设置为 2048 KB，以便避免在 XML 输出中可能发生数据截断。 有关何时使用不同目标类型的详细信息，请参阅 [SQL Server 扩展事件目标](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))。|  
|DROP TARGET \<event_target_specifier>|删除由 \<event_target_specifier> 标识的目标。 \<event_target_specifier> 在事件会话中必须有效。|  
|EVENT_RETENTION_MODE = { ALLOW_SINGLE_EVENT_LOSS &#124; ALLOW_MULTIPLE_EVENT_LOSS &#124; NO_EVENT_LOSS }|指定要用于处理事件丢失的事件保留模式。<br /><br /> ALLOW_SINGLE_EVENT_LOSS<br /> 事件可能会从会话中丢失。 只有在所有事件缓冲区均已满时才删除单个事件。 通过在事件缓冲区已满时丢失单个事件，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可实现足以满足要求的性能特征，同时还可使处理的事件流中的数据丢失降到最低。<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS<br /> 包含多个事件的已满事件缓冲区可能会从会话中丢失。 丢失事件的数目取决于分配给会话的内存大小、内存的分区情况以及缓冲区中事件的大小。 在事件缓冲区迅速达到已满状态时，该选项可将对服务器性能的影响降至最低，但可能会有大量的事件从会话中丢失。<br /><br /> NO_EVENT_LOSS<br /> 不允许事件丢失。 此选项可确保所有引发的事件都将得以保留。 使用此选项可强制所有激发事件的任务一直等到事件缓冲区中有可用空间时才执行。 这可能会在事件会话处于活动状态时引发可察觉到的性能问题。 在等待从缓冲区刷新事件时，用户连接可能中断。|  
|MAX_DISPATCH_LATENCY = { seconds SECONDS &#124; INFINITE }|指定在将事件调度至事件会话目标前这些事件在内存中缓冲的时间。 最小滞后时间值为 1 秒。 但是，可以使用 0 来指定 INFINITE 滞后时间。 默认情况下，此值设置为 30 秒。<br /><br /> seconds SECONDS<br /> 在开始将缓冲区刷新到目标前等待的时间（单位为秒）。 seconds 是一个整数。<br /><br /> INFINITE<br /> 仅在缓冲区已满或事件会话关闭时才将缓冲区刷新到目标。<br /><br /> **注意！！** MAX_DISPATCH_LATENCY = 0 SECONDS 等效于 MAX_DISPATCH_LATENCY = INFINITE。|  
|MAX_EVENT_SIZE =size [ KB &#124; MB ]|指定允许的最大事件大小。 MAX_EVENT_SIZE 应仅设置为允许单个事件大于 MAX_MEMORY；将其设置为小于 MAX_MEMORY 将引发错误。 size 是整数，并且其值可以以千字节 (KB) 或兆字节 (MB) 表示。 如果以千字节为单位指定 size，则允许的最小大小为 64 KB。 设置 MAX_EVENT_SIZE 后，除 MAX_MEMORY 之外，还创建了两个大小为 size 的缓冲区。 也就是说，用于事件缓冲的总内存为 MAX_MEMORY + 2 * MAX_EVENT_SIZE。|  
|MEMORY_PARTITION_MODE = { NONE &#124; PER_NODE &#124; PER_CPU }|指定事件缓冲区的创建位置。<br /><br /> **NONE**<br /> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中创建一组缓冲区。<br /><br /> PER NODE - 为每个 Numa 节点创建一组缓冲区。<br /><br /> PER CPU - 为每个 CPU 创建一组缓冲区。|  
|TRACK_CAUSALITY = { ON &#124; OFF }|指定是否跟踪因果关系。 如果已启用，因果关系将允许将不同服务器连接上的相关事件关联在一起。|  
|STARTUP_STATE = { ON &#124; OFF }|指定在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动时是否自动启动此事件会话。<br /><br /> 如果 STARTUP_STATE=ON，则事件会话将只在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 停止并重新启动的情况下才启动。<br /><br /> 启动时 ON= Event 会话会启动。<br /><br /> 启动时 OFF = Event 会话不启动。|  
  
## <a name="remarks"></a>备注  
 `ADD` 和 `DROP` 参数不能用在同一个语句中。  
  
## <a name="permissions"></a>权限  
 需要 `ALTER ANY EVENT SESSION` 权限。  
  
## <a name="examples"></a>示例  
 下例启动一个事件会话，获取一些实时会话统计数据，然后将两个事件添加到现有会话中。  
  
```sql  
-- Start the event session  
ALTER EVENT SESSION test_session ON SERVER  
STATE = start;  
GO  

-- Obtain live session statistics   
SELECT * FROM sys.dm_xe_sessions;  
SELECT * FROM sys.dm_xe_session_events;  
GO  
  
-- Add new events to the session  
ALTER EVENT SESSION test_session ON SERVER  
ADD EVENT sqlserver.database_transaction_begin,  
ADD EVENT sqlserver.database_transaction_end;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)   
 [DROP EVENT SESSION (Transact-SQL)](../../t-sql/statements/drop-event-session-transact-sql.md)   
 [SQL Server 扩展事件目标](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))   
 [sys.server_event_sessions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)   
 [sys.dm_xe_objects (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)   
 [sys.dm_xe_object_columns (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)  
  
