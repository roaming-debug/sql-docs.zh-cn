---
description: DBCC INPUTBUFFER (Transact-SQL)
title: DBCC INPUTBUFFER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC INPUTBUFFER
- INPUTBUFFER
- DBCC_INPUTBUFFER_TSQL
- INPUTBUFFER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- input buffers [SQL Server]
- last statement from client
- displaying last statement sent
- statements [SQL Server], last statement
- DBCC INPUTBUFFER statement
ms.assetid: a44d702b-b3fb-4950-8c8f-1adcf3f514ba
author: pmasl
ms.author: umajay
ms.openlocfilehash: a76fab0c7e0b7e15beb0eb094de4aa66e1644b2e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96119594"
---
# <a name="dbcc-inputbuffer-transact-sql"></a>DBCC INPUTBUFFER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

显示从客户端发送到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的最后一个语句。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC INPUTBUFFER ( session_id [ , request_id ])  
[WITH NO_INFOMSGS ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*session_id*  
与各活动主连接关联的会话 ID。  
  
request_id  
要在当前会话中搜索的精确请求（批）。  

下面的查询返回 request_id：  
```sql
SELECT request_id   
FROM sys.dm_exec_requests   
WHERE session_id = @@spid;  
```  
WITH  
启用要指定的选项。  
  
NO_INFOMSGS  
取消严重级别从 0 到 10 的所有信息性消息。  
  
## <a name="result-sets"></a>结果集  
DBCC INPUTBUFFER 返回包含如下列的行集。
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**EventType**|**nvarchar(30)**|事件类型。 这可能是 RPC 事件或 Language 事件。 检测不到上一个事件时，输出为 No Event。|  
|**参数**|**smallint**|0 = 文本<br /><br /> 1- n = Parameters|  
|**EventInfo**|**nvarchar(4000)**|对于 RPC 的 EventType，EventInfo 仅包含过程名。 对于 Language 的 EventType，仅显示事件的前 4000 个字符。|  
  
例如，当缓冲区中的最后一个事件是 DBCC INPUTBUFFER(11) 时，DBCC INPUTBUFFER 将返回以下结果集。
  
```
EventType      Parameters EventInfo               
-------------- ---------- ---------------------   
Language Event 0          DBCC INPUTBUFFER (11)  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  

> [!NOTE]
> 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 开始，使用 [sys.dm_exec_input_buffer](../../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md) 返回有关提交到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的语句的信息。

## <a name="permissions"></a>权限  
对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，要求具有下列权限之一：
-   用户必须是 sysadmin 固定服务器角色的成员。  
-   用户必须具有 VIEW SERVER STATE 权限。  
-   session_id 必须与正在运行该命令的会话 ID 相同。 要确定会话 ID，请执行以下查询：  
  
```sql
SELECT @@spid;  
```
  
对于 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 高级层和业务关键层，需要数据库的 VIEW DATABASE STATE 权限。 对于 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 标准层、基本层和常规用途层，需要 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 管理员帐户。
  
## <a name="examples"></a>示例  
以下示例在一个连接上运行一个时间较长的事务，而与此同时在另一个连接上运行 `DBCC INPUTBUFFER`。
  
```sql
CREATE TABLE dbo.T1 (Col1 INT, Col2 CHAR(3));  
GO  
DECLARE @i INT = 0;  
BEGIN TRAN  
SET @i = 0;  
WHILE (@i < 100000)  
BEGIN  
INSERT INTO dbo.T1 VALUES (@i, CAST(@i AS CHAR(3)));  
SET @i += 1;  
END;  
COMMIT TRAN;  
--Start new connection #2.  
DBCC INPUTBUFFER (52);  
```  

## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
[sys.dm_exec_input_buffer (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md)
  
  
