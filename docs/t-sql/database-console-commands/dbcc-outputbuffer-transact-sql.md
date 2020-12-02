---
description: DBCC OUTPUTBUFFER (Transact-SQL)
title: DBCC OUTPUTBUFFER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC OUTPUTBUFFER
- OUTPUTBUFFER_TSQL
- OUTPUTBUFFER
- DBCC_OUTPUTBUFFER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC OUTPUTBUFFER statement
- output buffers
- current output buffer
ms.assetid: e912a06d-9fde-4e26-b057-801255d79504
author: pmasl
ms.author: umajay
ms.openlocfilehash: dfc426af5cc72e072cab1908930d265dc1cf91c6
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96119622"
---
# <a name="dbcc-outputbuffer-transact-sql"></a>DBCC OUTPUTBUFFER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

以十六进制和 ASCII 格式返回指定 session_id 的当前输出缓冲区。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
```syntaxsql
DBCC OUTPUTBUFFER ( session_id [ , request_id ])  
[ WITH NO_INFOMSGS ]  
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
 允许指定其他选项。  
  
 NO_INFOMSGS  
 取消严重级别从 0 到 10 的所有信息性消息。  
  
## <a name="remarks"></a>备注  
DBCC OUTPUTBUFFER 显示发送到指定客户端 (session_id) 的结果。 对于不包含输出流的进程，则返回错误消息。
  
若要显示所执行的、其返回的结果由 DBCC OUTPUTBUFFER 所显示的语句，请执行 DBCC INPUTBUFFER。
  
## <a name="result-sets"></a>结果集  
DBCC OUTPUTBUFFER 返回以下结果集（值可能会有变化）：
  
```
Output Buffer                                                              
------------------------------------------------------------------------   
01fb8028:  04 00 01 5f 00 00 00 00 e3 1b 00 01 06 6d 00 61  ..._.........m.a  
01fb8038:  00 73 00 74 00 65 00 72 00 06 6d 00 61 00 73 00  .s.t.e.r..m.a.s.  
'...'  
01fb8218:  04 17 00 00 00 00 00 d1 04 18 00 00 00 00 00 d1  ................  
01fb8228:   .  
  
(33 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>权限  
要求具有 **sysadmin** 固定服务器角色的成员身份。
  
## <a name="examples"></a>示例  
以下示例为假定为 `52` 的会话 ID 返回当前输出缓冲区信息。
  
```sql
DBCC OUTPUTBUFFER (52);  
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
  
  
