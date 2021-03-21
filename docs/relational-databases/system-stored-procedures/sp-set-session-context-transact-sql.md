---
description: 'sp_set_session_context (Transact-sql) '
title: sp_set_session_context (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_set_session_context
- sp_set_session_context_TSQL
- sys.sp_set_session_context
- sys.sp_set_session_context_TSQL
helpviewer_keywords:
- sp_set_session_context
ms.assetid: 7a3a3b2a-1408-4767-a376-c690e3c1fc5b
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ff9f50aa953b0a7388156a9793d3ab1cbe819dc7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754627"
---
# <a name="sp_set_session_context-transact-sql"></a>sp_set_session_context (Transact-sql) 
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

在会话上下文中设置键值对。  
  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_set_session_context [ @key= ] N'key', [ @value= ] 'value'  
    [ , [ @read_only = ] { 0 | 1 } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>参数  
 [ @key =] N'key '  
 要设置的键，类型为 **sysname**。 最大密钥大小为128个字节。  
  
 [ @value =] "value"  
 **Sql_variant** 类型的指定键的值。 如果将值设置为 NULL，则释放内存。 最大大小为 8,000 个字节。  
  
 [ @read_only =] {0 | 1}  
 类型 **位** 的标志。 如果为1，则无法在此逻辑连接上再次更改指定键的值。 如果 0 (默认值) ，则可更改该值。  
  
## <a name="permissions"></a>权限  
 任何用户都可以为其会话设置会话上下文。  
  
## <a name="remarks"></a>备注  
 与其他存储过程一样， (不) 表达式或函数调用的文本和变量也可以作为参数传递。  
  
 会话上下文的总大小限制为 1 MB。 如果设置的值导致超过此限制，则语句将失败。 可以监视 [sys.dm_os_memory_objects &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md)中的总体内存使用情况。  
  
 可以通过查询 [sys.dm_os_memory_cache_counters &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md) 来监视总体内存使用情况，如下所示： `SELECT * FROM sys.dm_os_memory_cache_counters WHERE type = 'CACHESTORE_SESSION_CONTEXT';`  
  
## <a name="examples"></a>示例  
A. 下面的示例演示如何设置并返回值为英语的名为 language 的会话上下文键。  
  
```  
EXEC sys.sp_set_session_context @key = N'language', @value = 'English';  
SELECT SESSION_CONTEXT(N'language');  
```  
  
 下面的示例演示如何使用可选的只读标志。  
  
```  
EXEC sys.sp_set_session_context @key = N'user_id', @value = 4, @read_only = 1;  
```  

B. 下面的示例演示如何设置和检索值为12323ad 的名为 client_correlation_id 的会话上下文键。
```
-- set value
EXEC sp_set_session_context 'client_correlation_id', '12323ad'; 

--check value
SELECT SESSION_CONTEXT(N'client_correlation_id');

```

## <a name="see-also"></a>另请参阅  
 [CURRENT_TRANSACTION_ID (Transact-SQL)](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [SESSION_CONTEXT &#40;Transact-sql&#41;](../../t-sql/functions/session-context-transact-sql.md)   
 [行级安全性](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO (Transact-SQL)](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO (Transact-SQL)](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
