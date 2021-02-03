---
description: SET SHOWPLAN_ALL (Transact-SQL)
title: SET SHOWPLAN_ALL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET SHOWPLAN_ALL
- SET_SHOWPLAN_ALL_TSQL
- SHOWPLAN_ALL
- SHOWPLAN_ALL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- SET SHOWPLAN_ALL statement
- SHOWPLAN_ALL option
- canceling statement execution
- stopping statement execution
- estimated execution information [SQL Server]
ms.assetid: a500b682-bae4-470f-9e00-47de905b851b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 214827fe76301021faa31ec6a922059b60d088e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206951"
---
# <a name="set-showplan_all-transact-sql"></a>SET SHOWPLAN_ALL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  使 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回有关语句执行情况的详细信息，并估计语句对资源的需求。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET SHOWPLAN_ALL { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
 SET SHOWPLAN_ALL 的设置是在执行或运行时设置，而不是在分析时设置。  
  
 如果 `SET SHOWPLAN_ALL` 为 ON，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回每个语句的执行信息但不执行语句，并且 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句不会执行。 在将此选项设置为 ON 后，将始终返回有关所有后续 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的信息，直到将该选项设置为 OFF 为止。 例如，如果在 `SET SHOWPLAN_ALL` 为 ON 时执行 CREATE TABLE 语句，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将从涉及同一个表的后续 SELECT 语句返回错误信息，告知用户指定的表不存在。 因此，对此表的后续引用将失败。 如果 SET SHOWPLAN_ALL 为 OFF，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将执行语句，但不生成报表。  
  
 `SET SHOWPLAN_ALL` 是供为处理其输出而编写的应用程序使用的。 使用 SET SHOWPLAN_TEXT，可以为 Microsoft Win32 命令提示符应用程序（如 **osql** 实用工具）返回可读取的输出。  
  
 不能在存储过程内指定 SET SHOWPLAN_TEXT 和 SET SHOWPLAN_ALL，它们必须是批处理中的唯一语句。  
  
 SET SHOWPLAN_ALL 将信息作为行集返回，该行集将以层次结构树的形式，表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询处理器在执行每个语句时采取的步骤。 在输出中，每个语句都有一行说明语句的文本，后面紧接着几行介绍执行步骤的详细信息。 下表显示了输出中包含的列。  
  
|列名称|说明|  
|-----------------|-----------------|  
|**StmtText**|对于非 PLAN_ROW 类型的行，此列包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的文本。 对于 PLAN_ROW 类型的行，此列包含对操作的说明。 此列包含物理运算符，也可以选择包含逻辑运算符。 此列还可以跟一则由物理运算符决定的说明。 有关详细信息，请参阅 [Showplan 逻辑运算符和物理运算符参考](../../relational-databases/showplan-logical-and-physical-operators-reference.md)。|  
|**StmtId**|当前批处理中的语句数。|  
|**NodeId**|当前查询中的节点的 ID。|  
|**Parent**|父步骤的节点 ID。|  
|**PhysicalOp**|节点的物理实现算法。 仅限于 PLAN_ROWS 类型的行。|  
|**LogicalOp**|此节点表示的关系代数运算符。 仅限于 PLAN_ROWS 类型的行。|  
|**Argument**|提供有关当前执行的操作的补充信息。 此列的内容取决于物理运算符。|  
|**DefinedValues**|包含一组以逗号分隔的此运算符所引入的值。 这些值可以是出现在当前查询（例如，在 SELECT 列表或 WHERE 子句中）内的计算表达式，也可以是查询处理器为处理该查询引入的内部值。 以后在该查询内的任何其他地方都可以引用这些定义的值。 仅限于 PLAN_ROWS 类型的行。|  
|**EstimateRows**|由此运算符生成的预计输出行数。 仅限于 PLAN_ROWS 类型的行。|  
|**EstimateIO**|此运算符的预计 I/O 开销*。 仅限于 PLAN_ROWS 类型的行。|  
|**EstimateCPU**|此运算符的预计 CPU 开销*。 仅限于 PLAN_ROWS 类型的行。|  
|**AvgRowSize**|通过此运算符传递的行的预计平均行大小（以字节为单位）。|  
|**TotalSubtreeCost**|此操作和所有子操作的预计（累积）开销*。|  
|**OutputList**|包含当前操作正在提取的列的列表，此列表以逗号分隔。|  
|**:::no-loc text="Warnings":::**|包含一组以逗号分隔的与当前操作相关的警告信息。 警告消息可能包含字符串 "NO STATS:()" 和一组列表。 此警告信息表示查询优化器曾尝试根据此列的统计信息做出决策，但没有找到可用的统计信息。 因此，查询优化器不得不进行推测，这可能已导致选择了低效的查询计划。 有关创建或更新列统计信息（这些统计信息有助于查询优化器选择更有效的查询计划）的详细信息，请参阅 [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md)。 此列可能选择包含字符串 MISSING JOIN PREDICATE，表示正在进行的联接（与表有关）未使用联接谓词。 意外丢失联接谓词可能导致查询的运行时间比预期长得多，并返回一个巨大的结果集。 如果出现此警告，请验证是否有意不使用联接谓词。|  
|**:::no-loc text="Type":::**|节点类型。 对于每个查询的父节点，这是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句类型（如 SELECT、INSERT、EXECUTE 等）。 对于表示执行计划的子节点，这是 PLAN_ROW 类型。|  
|**Parallel**|**0** = 运算符没有以并行方式运行。<br /><br /> **1** = 运算符正在以并行方式运行。|  
|**EstimateExecutions**|当前查询运行期间，预计将执行此运算符的次数。|  
|||

 *开销单位是基于内部时间度量，而不是时钟时间。 它们用于确定某个计划与其他相计划相比的相对开销。  
  
## <a name="permissions"></a>权限  
 若要使用 SET SHOWPLAN_ALL，必须对执行 SET SHOWPLAN_ALL 的语句具有足够的执行权限，还必须对所有包含被引用对象的所有数据库具有 SHOWPLAN 权限。  
  
 对于 SELECT、INSERT、UPDATE、DELETE、EXEC stored_procedure 和 EXEC user_defined_function 语句，若要生成显示计划，用户必须   ：  
  
-   具有执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的相应权限。  
  
-   对于包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句引用的对象（如表、视图等）的所有数据库，具有 SHOWPLAN 权限。  
  
 对于其他所有语句，如 DDL、USE database_name、SET、DECLARE、动态 SQL 等，只需要具有执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的相应权限  。  
  
## <a name="examples"></a>示例  
 下面两个语句使用了 SET SHOWPLAN_ALL 设置，以显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在查询中分析和优化索引的方法。  
  
 第一个查询在 WHERE 子句中使用针对索引列的等于比较运算符 (=)。 从而在 **LogicalOp** 列内得到 Clustered Index Seek 值，在 **Argument** 列内生成索引名。  
  
 第二个查询在 WHERE 子句中使用 LIKE 运算符。 这将强制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用聚集索引扫描并查找满足 WHERE 子句条件的数据。 从而在 **LogicalOp** 列内得到 Clustered Index Scan 值，在 **Argument** 列内生成索引名；在 **LogicalOp** 列内得到 Filter 值，在 **Argument** 列内出现 WHERE 子句条件。  
  
 第一个索引查询的 **EstimateRows** 和 **TotalSubtreeCost** 列中的值较小，这表示与非索引查询相比，该查询的处理速度快得多且使用的资源更少。  
  
```sql
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_ALL ON;  
GO  
-- First query.  
SELECT BusinessEntityID   
FROM HumanResources.Employee  
WHERE NationalIDNumber = '509647174';  
GO  
-- Second query.  
SELECT BusinessEntityID, EmergencyContactID   
FROM HumanResources.Employee  
WHERE EmergencyContactID LIKE '1%';  
GO  
SET SHOWPLAN_ALL OFF;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_TEXT (Transact-SQL)](../../t-sql/statements/set-showplan-text-transact-sql.md)   
 [SET SHOWPLAN_XML (Transact-SQL)](../../t-sql/statements/set-showplan-xml-transact-sql.md)  
  
  
