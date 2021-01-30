---
description: sp_trace_setevent (Transact-SQL)
title: sp_trace_setevent (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_setevent_TSQL
- sp_trace_setevent
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setevent
ms.assetid: 7662d1d9-6d0f-443a-b011-c901a8b77a44
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fffcbb5ec35fb329fa0ed3dac4782f0f1417ce82
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209724"
---
# <a name="sp_trace_setevent-transact-sql"></a>sp_trace_setevent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在跟踪中添加或删除事件或事件列。 **sp_trace_setevent** 只能在已停止的现有跟踪上执行 () *状态* 为 **0** 。 如果对不存在的或 *状态* 不为 **0** 的跟踪执行此存储过程，则将返回错误。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用扩展事件。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_trace_setevent [ @traceid = ] trace_id   
          , [ @eventid = ] event_id  
          , [ @columnid = ] column_id  
          , [ @on = ] on  
```  
  
## <a name="arguments"></a>参数  
`[ @traceid = ] trace_id` 要修改的跟踪的 ID。 *trace_id* 为 **int**，没有默认值。 用户使用此 *trace_id* 值标识、修改和控制跟踪。  
  
`[ @eventid = ] event_id` 要打开的事件的 ID。 *event_id* 为 **int**，没有默认值。  
  
 下表列出了可以在跟踪中添加或删除的事件。  
  
|事件号|事件名称|说明|  
|------------------|----------------|-----------------|  
|0-9|预留|预留|  
|10|RPC:Completed|在完成了远程过程调用 (RPC) 时发生。|  
|11|RPC:Starting|在启动了 RPC 时发生。|  
|12|SQL:BatchCompleted|在完成了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理时发生。|  
|13|SQL:BatchStarting|在启动了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理时发生。|  
|14|Audit Login|在用户成功登录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时发生。|  
|15|Audit Logout|在用户从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 注销时发生。|  
|16|Attention|在发生需要关注的事件（如客户端中断请求或客户端连接中断）时发生。|  
|17|ExistingConnection|检测在启动跟踪前连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的用户的所有活动。|  
|18|Audit Server Starts and Stops|在修改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务状态时发生。|  
|19|DTCTransaction|跟踪 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 在两个或更多的数据库之间协调的事务。|  
|20|Audit Login Failed|指示试图从客户端登录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 失败。|  
|21|EventLog|指示已将事件记录到 Windows 应用程序日志中。|  
|22|ErrorLog|指示已将错误事件记录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中。|  
|23|Lock:Released|指示已释放某个资源（如页）的锁。|  
|24|Lock:Acquired|指示获取了某个资源（如数据页）的锁。|  
|25|Lock:Deadlock|指示两个并发事务由于试图获得对方事务拥有的资源的不兼容锁而发生了相互死锁。|  
|26|Lock:Cancel|指示已取消获取资源锁（例如，由于死锁）。|  
|27|Lock:Timeout|指示由于其他事务持有所需资源的阻塞锁而使对资源（例如页）锁的请求超时。 超时由 @ @LOCK_TIMEOUT 函数确定，可使用 set LOCK_TIMEOUT 语句进行设置。|  
|28|Degree of Parallelism Event（7.0 插入）|在执行 SELECT、INSERT 或 UPDATE 语句之前发生。|  
|29-31|预留|改用事件 28。|  
|32|预留|预留|  
|33|异常|指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中出现了异常。|  
|34|SP:CacheMiss|指示未在过程缓存中找到某个存储过程。|  
|35|SP:CacheInsert|指示某个项被插入到过程缓存中。|  
|36|SP:CacheRemove|指示从过程缓存中删除了某个项。|  
|37|SP:Recompile|指示已重新编译存储过程。|  
|38|SP:CacheHit|指示在过程缓存中找到了存储过程。|  
|39|不推荐使用|不推荐使用|  
|40|SQL:StmtStarting|在启动了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时发生。|  
|41|SQL:StmtCompleted|在完成了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时发生。|  
|42|SP:Starting|指示启动了存储过程。|  
|43|SP:Completed|指示完成了存储过程。|  
|44|SP:StmtStarting|指示已开始执行存储过程中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|45|SP:StmtCompleted|指示存储过程中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句已执行完毕。|  
|46|Object:Created|指示 CREATE INDEX、CREATE TABLE 和 CREATE DATABASE 这样的语句已创建了一个对象。|  
|47|Object:Deleted|指示已在 DROP INDEX 和 DROP TABLE 这样的语句中删除了对象。|  
|48|预留||  
|49|预留||  
|50|SQL Transaction|跟踪 [!INCLUDE[tsql](../../includes/tsql-md.md)] BEGIN、COMMIT、SAVE 和 ROLLBACK TRANSACTION 语句。|  
|51|Scan:Started|指示启动了表或索引扫描|  
|52|Scan:Stopped|指示停止了表或索引扫描。|  
|53|CursorOpen|指示 ODBC、OLE DB 或 DB-Library 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中打开了一个游标。|  
|54|TransactionLog|将事务写入事务日志时进行跟踪。|  
|55|Hash Warning|指示未在缓冲分区进行的某一哈希操作（例如，哈希联接、哈希聚合、哈希 union 运算、哈希非重复）已恢复为替换计划。 发生此事件的原因可能是递归深度、数据扭曲、跟踪标记或位计数。|  
|56-57|预留||  
|58|Auto Stats|指示发生了自动更新索引统计信息。|  
|59|Lock:Deadlock Chain|为导致死锁的每个事件而生成。|  
|60|Lock:Escalation|指示较细粒度的锁转换成了较粗粒度的锁（例如，页锁升级或转换为 TABLE 或 HoBT 锁）。|  
|61|OLE DB Errors|指示发生了 OLE DB 错误。|  
|62-66|预留||  
|67|Execution Warnings|指示在执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句或存储过程期间发生的任何警告。|  
|68|Showplan Text (Unencoded)|显示所执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的计划树。|  
|69|Sort Warnings|指示不适合内存的排序操作。 不包括与创建索引有关的排序操作；只包括某查询内的排序操作（如 SELECT 语句中使用的 ORDER BY 子句）。|  
|70|CursorPrepare|指示已准备了 ODBC、OLE DB 或 DB-Library 用于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的游标。|  
|71|Prepare SQL|ODBC、OLE DB 或 DB-Library 已准备好了一个或多个要使用的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|72|Exec Prepared SQL|ODBC、OLE DB 或 DB-Library 已执行了一个或多个准备好的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|73|Unprepare SQL|ODBC、OLE DB 或 DB-Library 已撤消（删除）了一个或多个准备好的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|74|CursorExecute|执行了先前由 ODBC、OLE DB 或 DB-Library 为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句准备的游标。|  
|75|CursorRecompile|由 ODBC 或 DB-Library 为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句打开的游标已直接重新编译或由于架构更改而重新编译。<br /><br /> 为 ANSI 和非 ANSI 游标触发。|  
|76|CursorImplicitConversion|[!INCLUDE[tsql](../../includes/tsql-md.md)] 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句的游标从一种类型转换为另一种类型。<br /><br /> 为 ANSI 和非 ANSI 游标触发。|  
|77|CursorUnprepare|ODBC、OLE DB 或 DB-Library 撤消（删除）了准备好的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的游标。|  
|78|CursorClose|关闭了先前由 ODBC、OLE DB 或 DB-Library 为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句打开的游标。|  
|79|Missing Column Statistics|可能曾经对优化器有用的列统计信息不可用。|  
|80|Missing Join Predicate|正在执行没有联接谓词的查询。 这可能导致长时间运行查询。|  
|81|Server Memory Change|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存的使用量已增加或减少了 1 MB 或最大服务器内存的 5%（两者中较大者）。|  
|82-91|User Configurable (0-9)|用户定义的事件数据。|  
|92|Data File Auto Grow|指示服务器已自动扩展了数据文件。|  
|93|Log File Auto Grow|指示服务器已自动扩展了日志文件。|  
|94|Data File Auto Shrink|指示服务器已自动收缩了数据文件。|  
|95|Log File Auto Shrink|指示服务器已自动收缩了日志文件。|  
|96|Showplan Text|显示来自查询优化器的 SQL 语句的查询计划树。 请注意，" **TextData** " 列不包含此事件的显示计划。|  
|97|Showplan All|显示查询计划，并显示已执行的 SQL 语句的完整编译时详细信息。 请注意，" **TextData** " 列不包含此事件的显示计划。|  
|98|Showplan Statistics Profile|显示查询计划，并显示已执行的 SQL 语句的完整运行时详细信息。 请注意，" **TextData** " 列不包含此事件的显示计划。|  
|99|预留||  
|100|RPC Output Parameter|生成每个 RPC 的参数的输出值。|  
|101|预留||  
|102|Audit Database Scope GDR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的任何用户每次对语句权限发出 GRANT、DENY、REVOKE 时发生（仅适用于数据库操作，例如授予对数据库的权限）。|  
|103|Audit Object GDR Event|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的任何用户每次对对象权限发出 GRANT、DENY、REVOKE 时发生。|  
|104|Audit AddLogin Event|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 添加或删除登录名时发生; 对于 **sp_addlogin** 和 **sp_droplogin**，则为。|  
|105|Audit Login GDR Event|在添加或删除 Windows 登录权限时发生;对于 **sp_grantlogin**、 **sp_revokelogin** 和 **sp_denylogin**。|  
|106|Audit Login Change Property Event|当修改登录名（密码除外）的属性时发生。对于 **sp_defaultdb** 和 **sp_defaultlanguage**。|  
|107|Audit Login Change Password Event|在更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录密码时发生。<br /><br /> 不记录密码。|  
|108|Audit Add Login to Server Role Event|当在固定服务器角色中添加或删除登录名时发生;对于 **sp_addsrvrolemember** 和 **sp_dropsrvrolemember**。|  
|109|Audit Add DB User Event|在将登录名作为数据库用户添加到数据库用户或将其删除 (Windows 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) 数据库时出现; 对于 **sp_grantdbaccess**、 **sp_revokedbaccess**、 **sp_adduser** 和 **sp_dropuser**，则为。|  
|110|Audit Add Member to DB Role Event|当将登录名作为数据库用户添加到数据库用户或将其作为数据库用户删除 (数据库用户或用户定义的) 数据库时出现;对于 **sp_addrolemember**、 **sp_droprolemember** 和 **sp_changegroup**。|  
|111|Audit Add Role Event|在将登录名作为数据库用户添加到数据库或从数据库中删除时发生。对于 **sp_addrole** 和 **sp_droprole**。|  
|112|Audit App Role Change Password Event|在更改应用程序角色的密码时发生。|  
|113|Audit Statement Permission Event|在使用语句权限（如 CREATE TABLE）时发生。|  
|114|Audit Schema Object Access Event|在成功或未成功使用了对象权限（如 SELECT）时发生。|  
|115|Audit Backup/Restore Event|在发出 BACKUP 或 RESTORE 命令时发生。|  
|116|Audit DBCC Event|在发出 DBCC 命令时发生。|  
|117|Audit Change Audit Event|在修改审核跟踪时发生。|  
|118|Audit Object Derived Permission Event|在发出 CREATE、ALTER 和 DROP 对象命令时发生。|  
|119|OLEDB Call Event|为分布式查询和远程存储过程调用 OLE DB 访问接口时发生。|  
|120|OLEDB QueryInterface Event|为分布式查询和远程存储过程 OLE DB **QueryInterface** 调用时发生。|  
|121|OLEDB DataRead Event|对 OLE DB 访问接口调用数据请求时发生。|  
|122|Showplan XML|在执行 SQL 语句时发生。 包括该事件可以标识 Showplan 运算符。 每个事件都存储在格式正确的 XML 文档中。 请注意，此事件的 **Binary** 列包含编码的显示计划。 使用 SQL Server Profiler 可打开跟踪并查看显示计划。|  
|123|SQL:FullTextQuery|执行全文查询时发生。|  
|124|Broker:Conversation|报告 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话的进度。|  
|125|Deprecation Announcement|使用将从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中删除的功能时发生。|  
|126|Deprecation Final Support|使用将从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的下一个主版本中删除的功能时发生。|  
|127|Exchange Spill Event|当并行查询计划中的通信缓冲区已暂时写入 **tempdb** 数据库时发生。|  
|128|Audit Database Management Event|创建、更改或删除数据库时发生。|  
|129|Audit Database Object Management Event|对数据库对象（如架构）执行 CREATE、ALTER 或 DROP 语句时发生。|  
|130|Audit Database Principal Management Event|创建、更改或删除数据库的主体（如用户）时发生。|  
|131|Audit Schema Object Management Event|创建、更改或删除服务器对象时发生。|  
|132|Audit Server Principal Impersonation Event|服务器范围中发生模拟（如 EXECUTE AS LOGIN）时发生。|  
|133|Audit Database Principal Impersonation Event|数据库范围中发生模拟（如 EXECUTE AS USER 或 SETUSER）时发生。|  
|134|Audit Server Object Take Ownership Event|服务器范围中的对象的所有者发生更改时发生。|  
|135|Audit Database Object Take Ownership Event|数据库范围中的对象的所有者发生更改时发生。|  
|136|Broker:Conversation Group|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 创建新的会话组或删除现有会话组时发生。|  
|137|Blocked Process Report|进程被阻塞的时间超过了指定的时间时发生。 不包括系统进程或正在等待未发现死锁的资源的进程。 使用 **sp_configure** 配置生成报表的阈值和频率。|  
|138|Broker:Connection|报告 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 管理的传输连接的状态。|  
|139|Broker:Forwarded Message Sent|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 转发消息时发生。|  
|140|Broker:Forwarded Message Dropped|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 删除用于转发的消息时发生。|  
|141|Broker:Message Classify|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 确定消息的路由时发生。|  
|142|Broker:Transmission|指示在 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 传输层中发生了错误。 错误号和状态值指示了错误源。|  
|143|Broker:Queue Disabled|指示检测到有害消息，这是由于在 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 队列中有五个连续的事务回滚。 该事件包含数据库 ID 和包含有害消息的队列的队列 ID。|  
|144-145|预留||  
|146|Showplan XML Statistics Profile|在执行 SQL 语句时发生。 标识 Showplan 运算符，并显示完整的编译时数据。 请注意，此事件的 **Binary** 列包含编码的显示计划。 使用 SQL Server Profiler 可打开跟踪并查看显示计划。|  
|148|Deadlock Graph|取消获取锁的尝试时发生，这是因为该尝试是死锁的一部分，并且被选为死锁牺牲品。 提供死锁的 XML 说明。|  
|149|Broker:Remote Message Acknowledgement|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 发送或收到消息确认时发生。|  
|150|Trace File Close|跟踪文件在回滚期间关闭时发生。|  
|151|预留||  
|152|Audit Change Database Owner|使用 ALTER AUTHORIZATION 更改数据库的所有者，并且检查执行该操作的权限时发生。|  
|153|Audit Schema Object Take Ownership Event|使用 ALTER AUTHORIZATION 来将所有者分配给对象，并且检查执行该操作的权限时发生。|  
|154|预留||  
|155|FT:Crawl Started|全文爬网（填充）开始时发生。 用于检查工作线程任务是否拾取了爬网请求。|  
|156|FT:Crawl Stopped|全文爬网（填充）停止时发生。 爬网成功完成或发生错误时停止。|  
|157|FT:Crawl Aborted|在全文爬网过程中遇到异常时发生。 通常导致全文爬网停止。|  
|158|Audit Broker Conversation|报告与 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对话安全性相关的审核消息。|  
|159|Audit Broker Login|报告与 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 传输安全性相关的审核消息。|  
|160|Broker:Message Undeliverable|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 无法保留收到的消息时发生，该消息应当已传递给某个服务。|  
|161|Broker:Corrupted Message|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 收到损坏的消息时发生。|  
|162|User Error Message|显示出现错误或异常时用户看到的错误消息。|  
|163|Broker:Activation|队列监视器启动激活存储过程时，发送 QUEUE_ACTIVATION 通知时，或者队列监视器启动的激活存储过程退出时发生。|  
|164|Object:Altered|数据库对象更改时发生。|  
|165|Performance statistics|将经过编译的查询计划第一次缓存、重新编译或从计划缓存中删除时发生。|  
|166|SQL:StmtRecompile|发生语句级别的重新编译时发生。|  
|167|Database Mirroring State Change|镜像数据库的状态更改时发生。|  
|168|Showplan XML For Query Compile|编译 SQL 语句时发生。 显示完整的编译时数据。 请注意，此事件的 **Binary** 列包含编码的显示计划。 使用 SQL Server Profiler 可打开跟踪并查看显示计划。|  
|169|Showplan All For Query Compile|编译 SQL 语句时发生。 显示完整的编译时数据。 用于标识 Showplan 运算符。|  
|170|Audit Server Scope GDR Event|指示在服务器范围中发生了权限的授予、拒绝或撤消事件（如创建登录）。|  
|171|Audit Server Object GDR Event|指示发生了对架构对象（如表或函数）的授予、拒绝或撤消事件。|  
|172|Audit Database Object GDR Event|指示发生了对数据库对象（如程序集和架构）的授予、拒绝或撤消事件。|  
|173|Audit Server Operation Event|使用了安全审核操作（如使用了更改设置、资源、外部访问或授权）时发生。|  
|175|Audit Server Alter Trace Event|检查语句的 ALTER TRACE 权限时发生。|  
|176|Audit Server Object Management Event|创建、更改或删除服务器对象时发生。|  
|177|Audit Server Principal Management Event|创建、更改或删除了服务器主体时发生。|  
|178|Audit Database Operation Event|发生数据库操作（如检查或订阅查询通知）时发生。|  
|180|Audit Database Object Access Event|访问数据库对象（如架构）时发生。|  
|181|TM: Begin Tran starting|BEGIN TRANSACTION 请求开始时发生。|  
|182|TM: Begin Tran completed|BEGIN TRANSACTION 请求完成时发生。|  
|183|TM: Promote Tran starting|PROMOTE TRANSACTION 请求开始时发生。|  
|184|TM: Promote Tran completed|PROMOTE TRANSACTION 请求完成时发生。|  
|185|TM: Commit Tran starting|COMMIT TRANSACTION 请求开始时发生。|  
|186|TM: Commit Tran completed|COMMIT TRANSACTION 请求完成时发生。|  
|187|TM: Rollback Tran starting|ROLLBACK TRANSACTION 请求开始时发生。|  
|188|TM: Rollback Tran completed|ROLLBACK TRANSACTION 请求完成时发生。|  
|189|Lock： Timeout (超时 > 0) |对资源（如页）的锁请求超时时发生。|  
|190|Progress Report: Online Index Operation|报告生成进程正在运行时，联机索引生成操作的进度。|  
|191|TM: Save Tran starting|SAVE TRANSACTION 请求开始时发生。|  
|192|TM: Save Tran completed|SAVE TRANSACTION 请求完成时发生。|  
|193|Background Job Error|后台作业不正常终止时发生。|  
|194|OLEDB Provider Information|分布式查询运行并收集对应于提供程序连接的信息时发生。|  
|195|Mount Tape|收到磁带装入请求时发生。|  
|196|Assembly Load|发生加载 CLR 程序集的请求时发生。|  
|197|预留||  
|198|XQuery Static Type|执行 XQuery 表达式时发生。 此事件类提供静态类型的 XQuery 表达式。|  
|199|QN: subscription|无法订阅查询注册时发生。 **TextData** 列包含有关事件的信息。|  
|200|QN: parameter table|有关活动订阅的信息存储在内部参数表中。 在创建或删除参数表时发生该事件类。 通常，重新启动数据库时将创建或删除这些表。 **TextData** 列包含有关事件的信息。|  
|201|QN: template|查询模板代表订阅查询的类。 通常，除参数值以外，相同类中的查询是相同的。 当新的订阅请求针对已存在的类 (Match)、新类 (Create) 或 Drop 类（指示清除没有活动订阅的查询类的模板）时，发生此事件类。 **TextData** 列包含有关事件的信息。|  
|202|QN: dynamics|跟踪查询通知的内部活动。 **TextData** 列包含有关事件的信息。|  
|212|位图警告|指示何时在查询中禁用了位图筛选器。|  
|213|Database Suspect Data Page|指示何时将页添加到 **msdb** 中的 **suspect_pages** 表。|  
|214|CPU threshold exceeded|指示资源调控器检测到查询超过 CPU 阈值 (REQUEST_MAX_CPU_TIME_SEC) 的时间。|  
|215|PreConnect:Starting|指示 LOGON 触发器或资源调控器分类器函数开始执行的时间。|  
|216|PreConnect:Completed|指示 LOGON 触发器或资源调控器分类器函数完成执行的时间。|  
|217|Plan Guide Successful|指示 SQL Server 已成功为计划指南中包含的查询或批处理生成执行计划。|  
|218|Plan Guide Unsuccessful|指示 SQL Server 无法为包含计划指南的查询或批处理生成执行计划。 SQL Server 已尝试在不应用计划指南的情况下为此查询或批生成执行计划。 导致此问题的原因可能是计划指南无效。 您可以通过使用 sys.fn_validate_plan_guide 系统函数验证该计划指南。|  
|235|审核全文||  
  
`[ @columnid = ] column_id` 要为事件添加的列的 ID。 *column_id* 为 **int**，没有默认值。  
  
 下表列出了可以为事件添加的列。  
  
|列号|列名称|说明|  
|-------------------|-----------------|-----------------|  
|1|**TextData**|与跟踪内捕获的事件类相关的文本值。|  
|2|**BinaryData**|与在跟踪中捕获的事件类相关的二进制值。|  
|3|**DatabaseID**|由 USE *database* 语句指定的数据库的 ID; 如果未对给定的连接发出 use *database* 语句，则为默认数据库的 ID。<br /><br /> 可以使用 DB_ID 函数确定数据库的值。|  
|4|**TransactionID**|系统分配的事务 ID。|  
|5|**LineNumber**|包含存在错误的行的行号。 对于涉及 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的事件（如 **SP:StmtStarting**）， **LineNumber** 包含存储过程或批查询中语句的行号。|  
|6|**NTUserName**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户名。|  
|7|**NTDomainName**|用户所属的 Windows 域。|  
|8|**HostName**|发起请求的客户端计算机的名称。|  
|9|**ClientProcessID**|客户端计算机分配给正在运行客户端应用程序的进程的 ID。|  
|10|ApplicationName|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|  
|11|**LoginName**|客户端的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。|  
|12|**SPID**|分配给 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与客户端关联的进程的服务器进程 ID。|  
|13|**持续时间**|事件所花费的实耗时间（以微秒为单位）。 Hash Warning 事件不填充该数据列。|  
|14|**StartTime**|事件开始的时间（如果可用）。|  
|15|**EndTime**|事件结束的时间。 启动事件类（如 **SQL:BatchStarting** 或 **SP:Starting**）不填充此列。 **哈希警告** 事件也不会填充它。|  
|16|**Reads**|服务器代表事件所执行的逻辑磁盘读取次数。 此列不由 **Lock：已发布** 事件填充。|  
|17|**Writes**|服务器代表事件所执行的物理磁盘写入次数。|  
|18|CPU |事件所用的 CPU 时间（毫秒）。|  
|19|**权限**|显示权限的位图；由安全审核使用。|  
|20|**严重性**|异常错误的严重级别。|  
|21|**EventSubClass**|事件子类的类型。 所有事件类都不填充此数据列。|  
|22|**Exchange Spill**|系统分配的对象 ID。|  
|23|**Success**|尝试使用权限的成功情况；审核时使用。<br /><br /> **1** = 成功 **0** = 失败|  
|24|**IndexID**|受事件影响的对象的索引的 ID。 若要确定对象的索引的 ID，请使用 **sysindexes** 系统表的 **indid** 列。|  
|25|**IntegerData**|与在跟踪中捕获的事件类相关的整型值。|  
|26|**ServerName**|正在跟踪的实例的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （ *servername* 或 *servername\instancename*）。|  
|27|**EventClass**|被记录的事件类的类型。|  
|28|**ObjectType**|对象（如表、函数或存储过程）的类型。|  
|29|**NestLevel**|执行此存储过程所处的嵌套级。 请参阅 [@ @NESTLEVEL &#40;transact-sql&#41;](../../t-sql/functions/nestlevel-transact-sql.md)。|  
|30|**状态**|发生错误时的服务器状态。|  
|31|**错误**|错误号。|  
|32|**模式**|获取的锁的锁模式。 此列不由 **Lock：已发布** 事件填充。|  
|33|**Handle**|事件中引用的对象的句柄。|  
|34|**ObjectName**|被访问的对象的名称。|  
|35|**DatabaseName**|USE *database* 语句中指定的数据库的名称。|  
|36|**FileName**|被修改的文件名的逻辑名称。|  
|37|**OwnerName**|被引用对象的所有者名称。|  
|38|**RoleName**|语句针对的数据库范围或服务器范围的角色的名称。|  
|39|**TargetUserName**|某些操作的目标的用户名。|  
|40|**DBUserName**|客户端的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库用户名。|  
|41|**LoginSid**|已登录的用户的安全标识符 (SID)。|  
|42|**TargetLoginName**|某些操作的目标的登录名。|  
|43|**TargetLoginSid**|某些操作的目标登录名的 SID。|  
|44|**ColumnPermissions**|列级别权限状态；由安全审核使用。|  
|45|**LinkedServerName**|链接服务器的名称。|  
|46|**ProviderName**|OLE DB 访问接口的名称。|  
|47|**MethodName**|OLE DB 方法的名称。|  
|48|**RowCounts**|批处理中的行数。|  
|49|**RequestID**|包含该语句的请求的 ID。|  
|50|**XactSequence**|用于说明当前事务的标记。|  
|51|**EventSequence**|此事件的序列号。|  
|52|**BigintData1**|**bigint** 值，它依赖于跟踪中捕获的事件类。|  
|53|**BigintData2**|**bigint** 值，它依赖于跟踪中捕获的事件类。|  
|54|**GUID**|GUID 值，与跟踪中捕获的事件类相关。|  
|55|**IntegerData2**|整数值，与跟踪中捕获的事件类相关。|  
|56|**ObjectID2**|相关的对象或实体的 ID（如果可用）。|  
|57|**Type**|整数值，与跟踪中捕获的事件类相关。|  
|58|**OwnerID**|拥有锁的对象的类型。 仅限于锁事件。|  
|59|**ParentName**|对象所在架构的名称。|  
|60|**IsSystem**|指示事件是发生在系统进程中还是发生在用户进程中。<br /><br /> **1** = 系统<br /><br /> **0** = 用户。|  
|61|**Offset**|存储过程或批查询中的语句的起始偏移量。|  
|62|**SourceDatabaseID**|对象源所在数据库的 ID。|  
|63|**SqlHandle**|基于即席查询文本或 SQL 对象的数据库和对象 ID 的 64 位哈希运算。 可以将该值传递到 **sys.dm_exec_sql_text()** 以检索关联的 SQL 文本。|  
|64|**SessionLoginName**|发起会话的用户的登录名。 例如，如果您使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Login1 **连接到** 并以 **Login2** 身份执行语句，则 **SessionLoginName** 将显示 **Login1**，而 **LoginName** 将显示 **Login2**。 此数据列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|  
  
 **[ @on =]** *on*  
 指定将事件设置为 ON (1) 还是 OFF (0)。 *on* 为 **bit**，无默认值。  
  
 如果 *将设置为* **1**，并且 *column_id* 为 NULL，则将事件设置为 on，并清除所有列。 如果 *column_id* 不为 null，则该事件的列设置为 ON。  
  
 如果 *将设置为* **0**，并且 *column_id* 为 NULL，则将关闭事件，并清除所有列。 如果 *column_id* 不为 null，则将关闭列。  
  
 下表说明了和 **\@ columnid** **\@ 之间的交互**。  
  
|\@基于|\@columnid|结果|  
|---------|---------------|------------|  
|ON (**1**)|Null|事件设置为 ON。<br /><br /> 清除所有列。|  
||NOT NULL|指定事件的列设置为 ON。|  
|OFF (**0**)|Null|事件设置为 OFF。<br /><br /> 清除所有列。|  
||NOT NULL|指定事件的列设置为 OFF。|  
  
## <a name="return-code-values"></a>返回代码值  
 下表说明在存储过程完成后用户可能获得的代码值。  
  
|返回代码|描述|  
|-----------------|-----------------|  
|0|没有错误。|  
|1|未知错误。|  
|2|本跟踪当前正在运行。 此时更改跟踪将导致错误。|  
|3|指定的事件无效。 该事件可能不存在或者它不适用于此存储过程。|  
|4|指定的列无效。|  
|9|指定的跟踪句柄无效。|  
|11|指定的列在内部使用，并且不能删除。|  
|13|内存不足。 在没有足够内存执行指定的操作时返回此代码。|  
|16|该函数对此跟踪无效。|  
  
## <a name="remarks"></a>备注  
 **sp_trace_setevent** 将执行以前版本中提供的扩展存储过程执行的许多操作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 使用 **sp_trace_setevent** 而不是以下内容：  
  
-   **xp_trace_addnewqueue**  
  
-   **xp_trace_eventclassrequired**  
  
-   **xp_trace_seteventclassrequired**  
  
 用户必须对为每个事件添加的每个列执行 **sp_trace_setevent** 。 在每次执行期间，如果 **\@ "打开**" 设置为 **1**， **sp_trace_setevent** 会将指定的事件添加到跟踪的事件列表中。 如果 **\@ 将** 设置为 **0**，则 **sp_trace_setevent** 从列表中删除指定的事件。  
  
 严格类型化)  (**sp_trace_xx** 的所有 SQL 跟踪存储过程的参数。 如果没有用正确的输入参数数据类型（参数说明中指定的类型）来调用这些参数，则存储过程将返回错误。  
  
 有关使用跟踪存储过程的示例，请参阅[创建跟踪 (Transact-SQL)](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 用户必须拥有 ALTER TRACE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.fn_trace_geteventinfo &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getinfo (Transact-SQL)](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sp_trace_generateevent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [SQL Server 事件类参考](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [SQL 跟踪](../../relational-databases/sql-trace/sql-trace.md)  
  
  
