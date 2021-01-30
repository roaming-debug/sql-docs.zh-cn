---
description: SQLGetInfo 支持
title: SQLGetInfo 支持 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- compatibility [ODBC], SQLGetInfo
- backward compatibility [ODBC], SQLGetInfo
- SQLGetInfo function [ODBC], support
ms.assetid: 57326f57-daba-46b6-b0be-6c97213b9ef1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1dc92c6bd409829da13395b1286a5762e4504ced
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202751"
---
# <a name="sqlgetinfo-support"></a>SQLGetInfo 支持
当使用 ODBC 2 时。*x* 应用程序调用 **SQLGetInfo** ODBC *1.x 驱动程序* 时，必须支持下表中的 *InfoType* 参数。  
  
|*InfoType*|返回|  
|----------------|-------------|  
|SQL_ALTER_TABLE (ODBC 2.0) **注意：**  不推荐使用此信息类型;不推荐使用右侧列中的位掩码。|SQLINTEGER 位掩码，用于枚举数据源支持的 **ALTER TABLE** 语句中的子句。<br /><br /> 以下位掩码用于确定支持的子句：<br /><br /> SQL_AT_DROP_COLUMN = 支持删除列。 此操作是由驱动程序定义的还是限制的行为。  (ODBC 2.0) <br /><br /> SQL_AT_ADD_COLUMN = 支持在单个 ALTER TABLE 语句中添加多个列。 此位不与其他 SQL_AT_ADD_COLUMN_XXX 位或 SQL_AT_CONSTRAINT_XXX 位组合。  (ODBC 2.0) |  
|SQL_FETCH_DIRECTION (ODBC 1.0) <br /><br /> 此信息类型是在 ODBC 1.0 中引入的;每个位掩码都带有引入它的版本标记。|枚举支持的提取方向选项的 SQLINTEGER 位掩码。<br /><br /> 以下位掩码与标志结合使用，以确定支持的选项：<br /><br /> SQL_FD_FETCH_NEXT (ODBC 1.0) SQL_FD_FETCH_FIRST (ODBC 1.0) SQL_FD_FETCH_LAST (ODBC 1.0) SQL_FD_FETCH_PRIOR (ODBC 1.0) SQL_FD_FETCH_ABSOLUTE (ODBC 1.0) SQL_FD_FETCH_RELATIVE (ODBC 1.0) SQL_FD_FETCH_BOOKMARK (ODBC 2.0) |  
|SQL_LOCK_TYPES (ODBC 2.0) |SQLINTEGER 位掩码，枚举 **SQLSetPos** 中 *fLock* 参数支持的锁类型。<br /><br /> 以下位掩码与标志结合使用，以确定支持的锁类型：<br /><br /> SQL_LCK_NO_CHANGE SQL_LCK_EXCLUSIVE SQL_LCK_UNLOCK|  
|SQL_ODBC_API_CONFORMANCE (ODBC 1.0) |指示 ODBC 一致性级别的 SQLSMALLINT 值。<br /><br /> SQL_OAC_NONE = 无<br /><br /> SQL_OAC_LEVEL1 = 支持级别1<br /><br /> SQL_OAC_LEVEL2 = 级别2支持|  
|SQL_ODBC_SQL_CONFORMANCE (ODBC 1.0) |指示驱动程序支持的 SQL 语法的 SQLSMALLINT 值。 有关 SQL 一致性级别的定义，请参阅 [附录 C： Sql 语法](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md) 。<br /><br /> SQL_OSC_MINIMUM = 支持的最低语法<br /><br /> SQL_OSC_CORE = 核心语法支持<br /><br /> SQL_OSC_EXTENDED = 支持扩展语法|  
|SQL_POS_OPERATIONS (ODBC 2.0) |SQLINTEGER 位掩码枚举 **SQLSetPos** 中支持的操作。<br /><br /> 以下位掩码与标志结合使用，以确定支持的选项：<br /><br /> SQL_POS_POSITION (ODBC 2.0) SQL_POS_REFRESH (ODBC 2.0) SQL_POS_UPDATE (ODBC 2.0) SQL_POS_DELETE (ODBC 2.0) SQL_POS_ADD (ODBC 2.0) |  
|SQL_POSITIONED_STATEMENTS (ODBC 2.0) |枚举支持的定位 SQL 语句的 SQLINTEGER 位掩码。<br /><br /> 以下位掩码用于确定支持的语句：<br /><br /> SQL_PS_POSITIONED_DELETE SQL_PS_POSITIONED_UPDATE SQL_PS_SELECT_FOR_UPDATE|  
|SQL_SCROLL_CONCURRENCY (ODBC 1.0) |SQLINTEGER 位掩码，枚举游标支持的并发控制选项。<br /><br /> 以下位掩码用于确定受支持的选项：<br /><br /> SQL_SCCO_READ_ONLY = 游标是只读的。 不允许更新。<br /><br /> SQL_SCCO_LOCK = 游标使用足以确保行可更新的最低锁定级别。<br /><br /> SQL_SCCO_OPT_ROWVER = 游标使用乐观并发控制，比较行版本，如 SQLBase ROWID 或 Sybase TIMESTAMP。<br /><br /> SQL_SCCO_OPT_VALUES = 游标使用乐观并发控制，比较值。|  
|SQL_STATIC_SENSITIVITY (ODBC 2.0) |一个 SQLINTEGER 位掩码，它枚举通过 **SQLSetPos** 或定位的 update 或 delete 语句对应用程序所做的更改是否可由该应用程序检测到。<br /><br /> SQL_SS_ADDITIONS = 添加的行对游标可见;光标可以滚动到这些行。 将这些行添加到游标的位置与驱动程序相关。<br /><br /> SQL_SS_DELETIONS = 删除的行不再可用于游标，并且不会在结果集中保留 "洞";在游标从已删除的行滚动后，它无法返回到该行。<br /><br /> SQL_SS_UPDATES = 行的更新对游标可见;如果游标滚动并返回到更新的行，则游标返回的数据是更新的数据，而不是原始数据。 此选项仅适用于不更新密钥的键集驱动游标上的静态游标或更新。 此选项不适用于动态游标，也不适用于在混合游标中更改密钥的情况。<br /><br /> 应用程序是否可以检测由其他用户（包括同一应用程序中的其他游标）对结果集所做的更改取决于游标类型。|  
  
 使用 ODBC 1.x *驱动程序的 odbc 1.x 应用程序* 不应使用上表中所述的 *InfoType* 参数调用 **SQLGetInfo** ，但应使用下一段中列出的 ODBC 1.x *InfoType* 参数。 在 ODBC 2 中使用的 *InfoType* 参数之间不存在一对一的对应关系。*x* 和 ODBC 2.x 中使用的 *。* 使用 ODBC 2 的 ODBC *1.x 应用程序。* 另一方面， *x* 驱动程序应使用前面所述的 *InfoType* 参数。  
  
 在使用游标属性信息类型时，上表中的某些信息类型已弃用。 这些弃用的信息类型为 SQL_FETCH_DIRECTION、SQL_LOCK_TYPES、SQL_POS_OPERATIONS、SQL_POSITIONED_STATEMENTS、SQL_SCROLL_CONCURRENCY 和 SQL_STATIC_SENSITIVITY。 新的 cursor 属性类型是 SQL_XXX_CURSOR_ATTRIBUTES1and SQL_XXX_CURSOR_ATTRIBUTES2，其中 XXX 等于动态，FORWARD_ONLY，KEYSET_DRIVEN 或静态。 每个新类型都指示单个游标类型的驱动程序功能。 有关这些选项的详细信息，请参阅 [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) 函数说明。
