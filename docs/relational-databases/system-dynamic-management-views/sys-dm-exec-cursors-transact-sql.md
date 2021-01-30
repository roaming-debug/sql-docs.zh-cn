---
description: sys.dm_exec_cursors (Transact-SQL)
title: sys.dm_exec_cursors (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_cursors_TSQL
- dm_exec_cursors
- dm_exec_cursors_TSQL
- sys.dm_exec_cursors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cursors dynamic management function
ms.assetid: f520b63c-36af-40f1-bf71-6901d6331d3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 30e47feb0650e5e345c78f431f27bd0a953b31bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99106403"
---
# <a name="sysdm_exec_cursors-transact-sql"></a>sys.dm_exec_cursors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关在各种数据库中打开的游标的信息。  
  
## <a name="syntax"></a>语法  
  
```  
  
dm_exec_cursors (session_id | 0 )  
```  
  
## <a name="arguments"></a>参数  
 *session_id* |0  
 会话的 ID。 如果指定了 *session_id* ，则此函数将返回有关指定会话中的游标的信息。  
  
 如果指定了 0，则此函数返回有关所有会话的所有游标的信息。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|持有此游标的会话 ID。|  
|**cursor_id**|**int**|游标对象的 ID。|  
|name |**nvarchar(256)**|用户定义的游标名称。|  
|**properties**|**nvarchar(256)**|指定游标的属性。 下列属性的值连在一起可构成此列的值：<br />声明接口<br />游标类型 <br />游标并发<br />游标范围<br />游标嵌套级别<br /><br /> 例如，在此列中返回的值可能是 "TSQL &#124; 动态 &#124; 乐观 &#124; 全局 (0) "。|  
|**sql_handle**|**varbinary(64)**|声明游标的批处理的文本句柄。|  
|**statement_start_offset**|**int**|在当前正在执行的批处理或存储过程中，指示当前正在执行的语句开始位置的字符数。 可以与 **sql_handle**、 **statement_end_offset** 和 [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) 动态管理函数一起使用，以检索请求的当前正在执行的语句。|  
|**statement_end_offset**|**int**|在当前正在执行的批处理或存储过程中，指示当前正在执行的语句结束位置的字符数。 可以与 **sql_handle**、 **statement_start_offset** 和 **sys.dm_exec_sql_text** 动态管理函数一起使用，以检索请求的当前正在执行的语句。|  
|**plan_generation_num**|**bigint**|可用于在重新编译后区分不同计划实例的序列号。|  
|**creation_time**|**datetime**|创建此游标时的时间戳。|  
|**is_open**|**bit**|指定游标是否处于打开状态。|  
|**is_async_population**|**bit**|指定后台线程是否仍异步填充 KEYSET 或 STATIC 游标。|  
|**is_close_on_commit**|**bit**|指定是否使用 CURSOR_CLOSE_ON_COMMIT 声明游标。<br /><br /> 1 = 事务结束时将关闭游标。|  
|**fetch_status**|**int**|返回游标的上一提取状态。 这是最后返回的 @ @FETCH_STATUS 值。|  
|**fetch_buffer_size**|**int**|返回有关提取缓冲区大小的信息。<br /><br /> 1 = Transact-SQL 游标。 对于 API 游标，可以将该参数设置为更高的值。|  
|**fetch_buffer_start**|**int**|对于 FAST_FORWARD 和 DYNAMIC 游标，如果游标未打开或被放在第一行之前，则该参数返回 0。 否则，返回 -1。<br /><br /> 对于 STATIC 和 KEYSET 游标，如果游标未打开，则该参数返回 0；如果游标放在最后一行之外，则该参数返回 -1。<br /><br /> 在其他情况下，该参数返回游标所在的行号。|  
|**ansi_position**|**int**|游标在提取缓冲区中的位置。|  
|**worker_time**|**bigint**|辅助线程执行此游标所用的时间（毫秒）。|  
|**内容**|**bigint**|游标所执行的读取次数。|  
|**写**|**bigint**|游标所执行的写入次数。|  
|**dormant_duration**|**bigint**|自上次对此游标启动查询（打开或提取）以来所经过的时间（毫秒）。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="remarks"></a>备注  
 下表提供了有关游标声明接口的信息，并列出了这些属性列的可能值。  
  
|properties|说明|  
|--------------|-----------------|  
|API|使用一个数据访问 API（ODBC、OLEDB）声明游标。|  
|TSQL|使用 Transact-SQL DECLARE CURSOR 语法声明游标。|  
  
 下表提供了有关游标类型的信息，并列出了这些属性列的可能值。  
  
|类型|说明|  
|----------|-----------------|  
|Keyset|将游标声明为键集。|  
|动态|将游标声明为动态。|  
|快照|将游标声明为快照或静态。|  
|Fast_Forward|将游标声明为快进。|  
  
 下表提供了有关游标并发的信息，并列出了这些属性列的可能值。  
  
|并发|说明|  
|-----------------|-----------------|  
|只读|将游标声明为只读。|  
|Scroll Locks|游标使用滚动锁。|  
|乐观|游标使用乐观并发控制。|  
  
 下表提供了有关游标范围的信息，并列出了这些属性列的可能值。  
  
|范围|说明|  
|-----------|-----------------|  
|本地|指定该游标的范围对在其中创建它的批处理、存储过程或触发器是局部的。|  
|Global|指定该游标范围对连接是全局的。|  
  
## <a name="examples"></a>示例  
  
### <a name="a-detecting-old-cursors"></a>A. 检测旧游标  
 以下示例返回有关在服务器上打开时间超过指定时间（36 小时）的游标的信息。  
  
```  
SELECT creation_time, cursor_id, name, c.session_id, login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s ON c.session_id = s.session_id   
WHERE DATEDIFF(hh, c.creation_time, GETDATE()) > 36;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
  

