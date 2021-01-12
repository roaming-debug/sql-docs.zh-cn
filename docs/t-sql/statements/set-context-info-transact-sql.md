---
description: SET CONTEXT_INFO (Transact-SQL)
title: SET CONTEXT_INFO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_CONTEXT_INFO_TSQL
- SET CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- context information [SQL Server]
- CONTEXT_INFO option
- SET CONTEXT_INFO statement
ms.assetid: a0b7b9f3-dbda-4350-a274-bd9ecd5c0a74
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6420062352983a45cb1f5f81ddcae83e173b5130
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091842"
---
# <a name="set-context_info-transact-sql"></a>SET CONTEXT_INFO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  将最多 128 字节的二进制信息与当前会话或连接关联。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET CONTEXT_INFO { binary_str | @binary_var }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *binary_str*  
 一个 **binary** 常量或可以隐式转换为 **binary** 的常量，用于与当前会话或连接关联。  
  
 **@** *binary_var*  
 **varbinary** 或 **binary** 变量，保存用于与当前会话或连接关联的上下文值。  
  
## <a name="remarks"></a>注解  
 检索当前会话的上下文信息的首选方式是使用 CONTEXT_INFO 函数。 会话上下文信息也存储在以下系统视图的 **context_info** 列中：  
  
-   **sys.dm_exec_requests**  
  
-   **sys.dm_exec_sessions**  
  
-   **sys.sysprocesses**  
  
 不能在用户定义函数中指定 SET CONTEXT_INFO。 不能向 SET CONTEXT_INFO 提供一个 NULL 值，因为保存值的视图不允许使用 NULL 值。  
  
 SET CONTEXT_INFO 不接受常量名或变量名以外的表达式。 若要将上下文信息设置为函数调用的结果，必须先将函数调用的结果包括在 **binary** 或 **varbinary** 变量中。  
  
 与其他 SET 语句不同，在存储过程或触发器中发出 SET CONTEXT_INFO 时，为上下文信息设置的新值在存储过程或触发器完成后会继续存在。  
  
## <a name="examples"></a>示例  
  
### <a name="a-setting-context-information-by-using-a-constant"></a>A. 使用常量设置上下文信息  
 以下示例通过设置值并显示结果来阐释 `SET CONTEXT_INFO`。 请注意，查询 `sys.dm_exec_sessions` 要求 SELECT 和 VIEW SERVER STATE 权限，但使用 CONTEXT_INFO 函数则不需要。  
  
```sql
SET CONTEXT_INFO 0x01010101;  
GO  
SELECT context_info   
FROM sys.dm_exec_sessions  
WHERE session_id = @@SPID;  
GO  
```  
  
### <a name="b-setting-context-information-by-using-a-function"></a>B. 使用函数设置上下文信息  
 以下示例演示使用函数输出设置上下文值，其中函数的结果值必须先放到一个 **binary** 变量中。  
  
```sql
DECLARE @BinVar varbinary(128);  
SET @BinVar = CAST(REPLICATE( 0x20, 128 ) AS varbinary(128) );  
SET CONTEXT_INFO @BinVar;  
  
SELECT CONTEXT_INFO() AS MyContextInfo;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [CONTEXT_INFO (Transact-SQL)](../../t-sql/functions/context-info-transact-sql.md)  
  
  
