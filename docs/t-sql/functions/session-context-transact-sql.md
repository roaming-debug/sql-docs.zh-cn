---
description: SESSION_CONTEXT (Transact-SQL)
title: SESSION_CONTEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SESSION_CONTEXT
- sys.SESSION_CONTEXT
- SESSION_CONTEXT_TSQL
- sys.SESSION_CONTEXT_TSQL
helpviewer_keywords:
- SESSION_CONTEXT function
ms.assetid: b6bdbc54-331a-43cc-ab3d-3872d6a12100
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: eb971143c7de2bdbf683b24f81162e088ffd2a8e
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91378723"
---
# <a name="session_context-transact-sql"></a>SESSION_CONTEXT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  返回当前会话上下文中指定键的值。 使用 [sp_set_session_context (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md) 步骤设置值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
SESSION_CONTEXT(N'key')  
```  
  
## <a name="arguments"></a>参数
 “key”  
 正在检索的值的键（类型为 sysname）。  
  
## <a name="return-type"></a>返回类型  
 **sql_variant**  
  
## <a name="return-value"></a>返回值  
 如果未为该键设置值，则为与会话上下文中指定值相关联的值，或者为 NULL。  
  
## <a name="permissions"></a>权限  
 任何用户都可以读取其会话的会话上下文。  
  
## <a name="remarks"></a>备注  
 SESSION_CONTEXT 的 MARS 行为类似于 CONTEXT_INFO 的该行为。 如果 MARS 批设置键值对，则新值不会返回到相同连接上的其他 MARS 批，除非它们在设置新值的批完成后开始。 如果连接上有多个活动的 MARS 批，那么值不能设置为“read_only”。 这样可以避免有关哪一个值“胜出”的争用条件和非确定性。  
  
## <a name="examples"></a>示例  
 下面这个简单的示例将键 `user_id` 的会话上下文值设置为 4，然后使用 SESSION_CONTEXT 函数检索值。  
  
```sql  
EXEC sp_set_session_context 'user_id', 4;  
SELECT SESSION_CONTEXT(N'user_id');  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_set_session_context (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)   
 [CURRENT_TRANSACTION_ID (Transact-SQL)](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [行级安全性](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO (Transact-SQL)](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO (Transact-SQL)](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
