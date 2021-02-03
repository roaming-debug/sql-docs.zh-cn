---
description: Exchange Spill 事件类
title: Exchange Spill 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Exchange Spill event class
ms.assetid: fb876cec-f88d-4975-b3fd-0fb85dc0a7ff
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a60e22ef72b86bb8136ee5ad7709212fe7b394b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160782"
---
# <a name="exchange-spill-event-class"></a>Exchange Spill 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  **Exchange Spill** 事件类指示并行查询计划中的通信缓冲区已暂时写入 **tempdb** 数据库。 这种情况发生的几率很小，仅当查询计划具有多次范围扫描时才会发生。  
  
 一般情况下，生成此类范围扫描的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询包含许多 BETWEEN 运算符，其中每个运算符会从表或索引中选择某一范围的行。 或者，可以使用表达式来获取多个范围，例如 (T.a > 10 AND T.a < 20) OR (T.a > 100 AND T.a < 120)。 此外，查询计划必须要求按顺序扫描这些范围，原因是对 T.a 应用了 ORDER BY 子句，或者计划中的迭代器要求它按排序顺序处理元组。  
  
 如果此类查询的计划中有多个 **Parallelism** 运算符，则 **Parallelism** 运算符所使用的内存通信缓冲区将变满，这可能使查询的执行进度由此停止。 在此情况下，其中一个 **Parallelism** 运算符会将其输出缓冲区写入 **tempdb**（此操作称为“交换溢出”），以便它可以处理某些输出缓冲区中的行。 最终，当使用者准备使用溢出行时，溢出行将返回给使用者。  
  
 非常少见的是，在同一执行计划内可能出现多个交换溢出，这将使得查询的执行速度缓慢。 如果您发现同一个查询计划的执行中有五个以上的溢出，请与支持人员联系。  
  
 交换溢出有时是瞬态的，它们可能会随着数据分布的变化而消失。  
  
 下列几种方法可以避免发生交换溢出事件：  
  
-   如果不需要对结果集进行排序，则省略 ORDER BY 子句。  
  
-   如果需要 ORDER BY，则通过 ORDER BY 子句清除参与多次范围扫描的列（如上面示例中的 T.a）。  
  
-   使用索引提示，强制优化器对相关表使用其他访问路径。  
  
-   重写查询以生成不同的查询执行计划。  
  
-   通过将 MAXDOP = 1 选项添加到查询或索引操作的末尾，强制按顺序执行查询。 有关详细信息，请参阅 [配置最大并行度服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) 和 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
> [!IMPORTANT]  
>  若要确定查询优化器生成执行计划时发生 **Exchange Spill** 事件的位置，还应该在跟踪中收集一个 Showplan 事件类。 可以选择除了 **Showplan Text** 和 **Showplan Text (Unencoded)** 事件类（不会返回节点 ID）以外的任何 Showplan 事件类。 Showplan 中的节点 ID 标识查询优化器在生成查询执行计划时执行的每个运算。 这些运算称为“运算符”，Showplan 中的每个运算符都有一个节点 ID。 **Exchange Spill** 事件的 **ObjectID** 列与 Showplan 中的节点 ID 对应，因此可确定哪个运算符或运算导致错误。  
  
## <a name="exchange-spill-event-class-data-columns"></a>Exchange Spill 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**ClientProcessID**|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|**DatabaseID**|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|**EventClass**|**int**|事件类型 = 127。|27|否|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**EventSubClass**|**int**|事件子类的类型。<br /><br /> 1 = 溢出开始<br /><br /> 2 = 溢出结束|21|是|  
|**GroupID**|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|**HostName**|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LoginName**|**nvarchar**|用户的登录名（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 Windows 登录凭据，格式为 \<DOMAIN>\\<username\>）。|11|是|  
|**LoginSid**|**图像**|登录用户的安全标识号 (SID)。 您可以在 **master** 数据库的 **syslogins** 表中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|Windows 用户名。|6|是|  
|**Exchange Spill**|**int**|系统分配的对象 ID。 与 Showplan 中的节点 ID 对应。|22|是|  
|**RequestID**|**int**|包含该语句的请求的 ID。|49|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
|**XactSequence**|**bigint**|用于说明当前事务的标记。|50|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [设置索引选项](../../relational-databases/indexes/set-index-options.md)   
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)  
  
  
