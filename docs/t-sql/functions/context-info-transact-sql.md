---
description: CONTEXT_INFO (Transact-SQL)
title: CONTEXT_INFO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONTEXT_INFO_TSQL
- CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- CONTEXT_INFO function
- Multiple Active Result Sets
- context information [SQL Server]
- MARS [SQL Server]
- session context information [SQL Server]
ms.assetid: 571320f5-7228-4b0e-9d01-ab732d2d1eab
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6539ff370b5a229156c504e9203f23f63d1d1228
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186255"
---
# <a name="context_info--transact-sql"></a>CONTEXT_INFO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数返回通过使用 [SET CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md) 语句为当前会话或批处理设置的“context_info”值。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CONTEXT_INFO()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-value"></a>返回值
“context_info”值。
  
如果尚未设置 context_info：
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回 NULL。  
-   [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 返回特定于会话的唯一 GUID。  
  
## <a name="remarks"></a>备注  
多个活动的结果集 (MARS) 功能将使应用程序能够使用相同连接同时运行多个批处理或请求。 当 MARS 连接中的一个批处理运行 SET CONTEXT_INFO 时，如果 `CONTEXT_INFO` 函数作为 SET 语句运行在同一批处理中，则该 `CONTEXT_INFO` 函数将返回新的上下文值。 如果 `CONTEXT_INFO` 函数运行一个或多个其他连接批处理，则 `CONTEXT_FUNCTION` 不返回新值，除非这些批处理在完成运行 SET 语句的批处理后启动。
  
## <a name="permissions"></a>权限  
不要求具有特殊权限。 下面的系统视图会存储上下文信息，但直接查询这些视图需要 SELECT 和 VIEW SERVER STATE 权限：
- **sys.dm_exec_requests**
- **sys.dm_exec_sessions**
- **sys.sysprocesses**
  
## <a name="examples"></a>示例  
以下简单示例将“context_info”值设置为 `0x1256698456`，然后使用 `CONTEXT_INFO` 函数检索该值。
  
```sql
SET CONTEXT_INFO 0x1256698456;  
GO  
SELECT CONTEXT_INFO();  
GO  
```  
  
## <a name="see-also"></a>另请参阅
[SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)
[SESSION_CONTEXT  &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)  
[sp_set_session_context  &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)  
  

