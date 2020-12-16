---
description: SET DEADLOCK_PRIORITY (Transact-SQL)
title: SET DEADLOCK_PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET DEADLOCK_PRIORITY
- DEADLOCK_PRIORITY_TSQL
- SET_DEADLOCK_PRIORITY_TSQL
- DEADLOCK_PRIORITY
dev_langs:
- TSQL
helpviewer_keywords:
- deadlocks [SQL Server], priority settings
- DEADLOCK_PRIORITY option
- locking [SQL Server], deadlocks
- priority deadlock settings [SQL Server]
- SET DEADLOCK_PRIORITY statement
ms.assetid: 810a3a8e-3da3-4bf9-bb15-7b069685a1b6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d515065546607dc5acece79c57bf5199038c9fee
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465828"
---
# <a name="set-deadlock_priority-transact-sql"></a>SET DEADLOCK_PRIORITY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  指定当前会话与其他会话发生死锁时继续处理的相对重要性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET DEADLOCK_PRIORITY { LOW | NORMAL | HIGH | <numeric-priority> | @deadlock_var | @deadlock_intvar }  
  
<numeric-priority> ::= { -10 | -9 | -8 | ... | 0 | ... | 8 | 9 | 10 }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 LOW  
 指定如果当前会话发生死锁，并且死锁链中涉及的其他会话的死锁优先级设置为 NORMAL 或 HIGH 或大于 -5 的整数值，则当前会话将成为死锁牺牲品。 如果其他会话的死锁优先级设置为小于 -5 的整数值，则当前会话将不会成为死锁牺牲品。 此参数还指定如果其他会话的死锁优先级设置为 LOW 或 -5，则当前会话将可能成为死锁牺牲品。  
  
 NORMAL  
 指定如果死锁链中涉及的其他会话的死锁优先级设置为 HIGH 或大于 0 的整数值，则当前会话将成为死锁牺牲品，但如果其他会话的死锁优先级设置为 LOW 或小于 0 的整数值，则当前会话将不会成为死锁牺牲品。 此参数还指定如果其他会话的死锁优先级设置为 NORMAL 或 0，则当前会话将可能成为死锁牺牲品。 NORMAL 为默认优先级。  
  
 HIGH  
 指定如果死锁链中涉及的其他会话的死锁优先级设置为大于 5 的整数值，则当前会话将成为死锁牺牲品，或者如果其他会话的死锁优先级设置为 HIGH 或 5，则当前会话可能成为死锁牺牲品。  
  
 \<numeric-priority>  
 用以提供 21 个死锁优先级别的整数值范围（-10 到 10）。 它指定如果死锁链中涉及的其他会话以更高的死锁优先级值运行，则当前会话将成为死锁牺牲品，但如果其他会话以低于当前会话的死锁优先级值运行，则当前会话不会成为死锁牺牲品。 它还指定如果其他会话以相同于当前会话的死锁优先级值运行，则当前会话可能成为死锁牺牲品。 LOW 对应于 -5、NORMAL 对应于 0 以及 HIGH 对应于 5。  
  
 **@** *deadlock_var*  
 指定死锁优先级的字符变量。 此变量必须设置为“LOW”、“NORMAL”或“HIGH”中的一个值。 而且必须足够大以保存整个字符串。  
  
 **@** *deadlock_intvar*  
 指定死锁优先级的整数变量。 此变量必须设置为 -10 到 10 范围中的一个整数值。  
  
## <a name="remarks"></a>备注  
 当两个会话同时等待访问由其他会话锁定的资源时，便会发生死锁。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例检测到两个会话发生死锁时，将选择其中一个会话作为死锁牺牲品来解决死锁。 此牺牲品的当前事务将回滚，且死锁错误消息 1205 返回客户端。 这样可释放由该会话所控制的所有锁，从而允许其他会话继续进行。  
  
 将哪个会话选为死锁牺牲品取决于每个会话的死锁优先级：  
  
-   如果两个会话的死锁优先级相同，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将回滚的开销较低的会话选为死锁牺牲品。 例如，如果两个会话都将其死锁优先级设置为 HIGH，则此实例便将它估计回滚的开销较低的会话选为牺牲品。 该成本是通过比较各事务此时已写入的日志字节数来确定的。 （可以在死锁图中将此值看作“已用日志”）。
  
-   如果会话的死锁优先级不同，则将死锁优先级最低的会话选为死锁牺牲品。  
  
 SET DEADLOCK_PRIORITY 是在执行或运行时设置，而不是在分析时设置。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例使用变量将死锁优先级设置为 `LOW`。  
  
```sql
DECLARE @deadlock_var NCHAR(3);  
SET @deadlock_var = N'LOW';  
  
SET DEADLOCK_PRIORITY @deadlock_var;  
GO  
```  
  
 以下示例将死锁优先级设置为 `NORMAL`。  
  
```sql
SET DEADLOCK_PRIORITY NORMAL;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [@@LOCK_TIMEOUT (Transact-SQL)](../../t-sql/functions/lock-timeout-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET LOCK_TIMEOUT (Transact-SQL)](../../t-sql/statements/set-lock-timeout-transact-sql.md)  
  
  
