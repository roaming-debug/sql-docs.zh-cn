---
description: sys.sysprocesses (Transact-SQL)
title: sys.sys进程 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysprocesses_TSQL
- sys.sysprocesses_TSQL
- sys.sysprocesses
- sysprocesses
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysprocesses compatibility view
- sysprocesses system table
ms.assetid: 60a36d36-54b3-4bd6-9cac-702205a21b16
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d8272d6759508697512506bcab8df81d65b90b83
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095376"
---
# <a name="syssysprocesses-transact-sql"></a>sys.sysprocesses (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含正在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上运行的进程的相关信息。 这些进程可以是客户端进程或系统进程。 若要访问 sysprocesses，您必须位于 master 数据库上下文中，或者必须使用由三部分构成的名称 master.dbo.sysprocesses。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|spid|**smallint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会话 ID。|  
|kpid|**smallint**|Windows 线程 ID。|  
|blocked|**smallint**|正在阻塞请求的会话的 ID。 如果此列为 NULL，则表示请求未被阻塞，或锁定会话的会话信息不可用（或无法进行标识）。<br /><br /> -2 = 阻塞资源由孤立的分布式事务拥有。<br /><br /> -3 = 阻塞资源由延迟的恢复事务拥有。<br /><br /> -4 = 由于内部闩锁状态转换而无法确定阻塞闩锁所有者的会话 ID。|  
|waittype|**binary(2)**|保留。|  
|waittime|**bigint**|当前等待时间（毫秒）。<br /><br /> 0 = 进程不等待。|  
|lastwaittype|**nchar(32)**|指示上次或当前等待类型名称的字符串。|  
|waitresource|**nchar(256)**|锁资源的文本化表示法。|  
|dbid|**smallint**|当前正由进程使用的数据库 ID。|  
|uid|**smallint**|执行命令的用户 ID。 如果用户数和角色数超过 32,767，则发生溢出或返回 NULL。|  
|cpu|**int**|进程的累计 CPU 时间。 无论 SET STATISTICS TIME 选项是 ON 还是 OFF，都为所有进程更新该项。|  
|physical_io|**bigint**|进程的累计磁盘读取和写入。|  
|memusage|**int**|当前为此进程分配的过程缓存中的页数。 一个负数，表示进程正在释放由另一个进程分配的内存。|  
|login_time|**datetime**|客户端进程登录到服务器的时间。|  
|last_batch|**datetime**|客户端进程上次执行远程存储过程调用或 EXECUTE 语句的时间。|  
|ecid|**smallint**|用于唯一标识代表单个进程进行操作的子线程的执行上下文 ID。|  
|open_tran|**smallint**|进程的打开事务数。|  
|status|**nchar(30)**|进程 ID 状态。 可能的值为：<br /><br />   =  睡眠 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]正在重置会话。<br /><br /> **正在运行** = 会话正在运行一个或多个批处理。 多个活动的结果集 (MARS) 启用后，会话可以运行多个批。 有关详细信息，请参阅[使用多个活动的结果集 (MARS)](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)。<br /><br /> **背景** = 会话正在运行一个后台任务，例如死锁检测。<br /><br /> **rollback** = 会话具有正在处理的事务回滚。<br /><br /> **挂起** = 会话正在等待工作线程变为可用。<br /><br /> 可 **运行**= 会话中的任务在等待获取时间量程时位于计划程序的可运行队列中。<br /><br /> **spinloop** = 会话中的任务正在等待旋转锁可用。<br /><br /> 已 **挂起**= 会话正在等待事件（如 i/o）完成。|  
|sid|**binary(86)**|用户的全局唯一标识符 (GUID)。|  
|hostname|**nchar(128)**|工作站的名称。|  
|program_name|**nchar(128)**|应用程序的名称。|  
|hostprocess|**nchar(10)**|工作站进程 ID 号。|  
|cmd|**nchar (52)**|当前正在执行的命令。|  
|nt_domain|**nchar(128)**|客户端的 Windows 域（如果使用 Windows 身份验证）或可信连接的 Windows 域。|  
|nt_username|**nchar(128)**|进程的 Windows 用户名（如果使用 Windows 身份验证）或可信连接的 Windows 用户名。|  
|net_address|**nchar(12)**|为每个用户工作站上的网络适配器分配的唯一标识符。 当用户登录时，该标识符插入 net_address 列。|  
|net_library|**nchar(12)**|用于存储客户端网络库的列。 每个客户端进程都在网络连接上进入。 网络连接有一个与这些进程关联的网络库，该网络库使得这些进程可以建立连接。|  
|loginame|**nchar(128)**|登录名。|  
|context_info|**binary(128)**|使用 SET CONTEXT_INFO 语句存储在批中的数据。|  
|sql_handle|**binary(20)**|表示当前正在执行的批或对象。<br /><br /> **注意** 此值是从对象的批或内存地址派生的。 通过使用基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 哈希的算法无法计算此值。|  
|stmt_start|**int**|为指定 sql_handle 运行当前 SQL 语句的起始偏移量。|  
|stmt_end|**int**|所指定 sql_handle 的当前 SQL 语句的结束偏移量。<br /><br /> -1 指出当前语句为指定的 sql_handle 运行到 fn_get_sql 函数返回结果的结尾。|  
|request_id|**int**|请求 ID。 用于标识在特定会话中运行的请求。|
|page_resource |**二进制 (8)** |适用于：[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] <br /><br /> 如果列包含页，则为页资源的8字节的十六进制表示形式 `waitresource` 。 |  
  
## <a name="remarks"></a>备注  
 如果用户对服务器具有 VIEW SERVER STATE 权限，则该用户可查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中所有正在执行的会话；否则，该用户只能查看当前会话。  
  
## <a name="see-also"></a>另请参阅  
 [与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
