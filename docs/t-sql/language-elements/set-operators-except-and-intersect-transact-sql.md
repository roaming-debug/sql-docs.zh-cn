---
description: SET 运算符 - EXCEPT 和 INTERSECT (Transact-SQL)
title: EXCEPT 和 INTERSECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INTERSECT_TSQL
- EXCEPT_TSQL
- INTERSECT
- EXCEPT
dev_langs:
- TSQL
helpviewer_keywords:
- EXCEPT operator [Transact-SQL]
- queries [SQL Server], comparing
- comparing queries
- INTERSECT operator
ms.assetid: b1019300-171a-4a1a-854f-e1e751de3565
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f0a7ddf707ec6ed5bea3ff32fee215e29497767a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100303"
---
# <a name="set-operators---except-and-intersect-transact-sql"></a>SET 运算符 - EXCEPT 和 INTERSECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

比较两个查询的结果，返回非重复行。  
  
EXCEPT 从左侧输入查询返回非由右侧输入查询输出的非重复行。  
 
INTERSECT 返回由左右双侧输入查询运算符输出的非重复行。  
  
若要合并两个使用 EXCEPT 或 INTERSECT 的查询的结果集，请遵循以下基本规则：  
  
-   所有查询中的列数和列的顺序必须相同。  
  
-   数据类型必须兼容。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ EXCEPT | INTERSECT }  
{ <query_specification> | ( <query_expression> ) }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
\<_query\_specification_> | ( \<_query\_expression_> )  
查询规范或查询表达式返回与来自另一个查询规范或查询表达式的数据相比较的数据。 属于 EXCEPT 或 INTERSECT 运算的列定义不一定要相同。 但必须可通过隐式转换实现可比较。 如果数据类型不同，根据[数据类型优先顺序](../../t-sql/data-types/data-type-precedence-transact-sql.md)规则确定为执行比较而运行的数据类型。  
  
如果类型相同，但精度、确定位数或长度不同，那么结果以相同的表达式合并规则为依据。 有关详细信息，请参阅[精度、小数位数和长度 (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。  
  
查询规范或表达式无法返回 xml、text、ntext、image 或非二进制 CLR 用户定义类型列，因为这些数据类型不可比较。  
  
EXCEPT  
返回由 EXCEPT 运算符左侧的查询返回的所有非重复值。 返回这些值的前提是，右侧查询不返回这些值。  
  
INTERSECT  
返回由 INTERSECT 运算符左侧和右侧的查询都返回的所有非重复值。  
  
## <a name="remarks"></a>备注  
可比较列的数据类型是由 EXCEPT 或 INTERSECT 运算符左侧和右侧的查询返回。 这些数据类型可包括具有不同排序规则的字符数据类型。 如果确实是这样，根据[排序规则优先顺序](../../t-sql/statements/collation-precedence-transact-sql.md)规则运行必需比较。 如果无法运行此转换，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 返回错误。  
  
通过比较列值来确定非重复行时，两个 NULL 值被视为相等。  
  
EXCEPT 和 INTERSECT 返回结果集的列名，这些列名与运算符左侧的查询返回的列名相同。  
  
ORDER BY 子句中的列名或别名必须引用左侧查询返回的列名。  
  
EXCEPT 或 INTERSECT 返回的结果集中任何列的为 Null 性，与运算符左侧的查询返回的相应列的为 Null 性相同。  
  
如果 EXCEPT 或 INTERSECT 与其他运算符一起用于表达式，根据以下优先顺序对表达式求值：  
  
1.  括号中的表达式  
  
2.  INTERSECT 运算符  
  
3.  基于在表达式中的位置从左到右求值的 EXCEPT 和 UNION  
  
可使用 EXCEPT 或 INTERSECT 比较超过两组的查询。 如果这样做，通过一次比较两个查询，并遵循前面提到的表达式求值规则来确定数据类型转换。  
  
EXCEPT 和 INTERSECT 无法用于分布式分区视图定义、查询通知。  
 
EXCEPT 和 INTERSECT 可在分布式查询中使用，但只在本地服务器上执行，不会被推送到链接服务器。 因此，在分布式查询中使用 EXCEPT 和 INTERSECT 可能会影响性能。  
  
如果将快速只进游标和静态游标用于 EXCEPT 或 INTERSECT 运算，也可以在结果集中使用这些游标。 另外，还可以将键集驱动的游标或动态游标用于 EXCEPT 或 INTERSECT 运算。 如果这样做，运算结果集的游标会转换为静态游标。  
  
使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的图形显示计划功能显示 EXCEPT 运算时，该运算显示为 [left anti semi join](../../relational-databases/showplan-logical-and-physical-operators-reference.md)，INTERSECT 运算显示为 [left semi join](../../relational-databases/showplan-logical-and-physical-operators-reference.md)。  
  
## <a name="examples"></a>示例  
以下示例说明如何使用 `INTERSECT` 和 `EXCEPT` 运算符。 第一个查询返回 `Production.Product` 表中的所有值，以便对 `INTERSECT` 和 `EXCEPT` 所返回的结果进行比较。  
  
```sql
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product ;  
--Result: 504 Rows  
```  
  
以下查询返回 `INTERSECT` 运算符左右两侧的两个查询均返回的所有非重复值。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
INTERSECT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 238 Rows (products that have work orders)  
```  
  
下面的查询从 `EXCEPT` 运算符左侧的查询返回右侧查询不返回的所有非重复值。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
EXCEPT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 266 Rows (products without work orders)  
```  
  
下面的查询从 `EXCEPT` 运算符左侧的查询返回右侧查询不返回的所有非重复值。 对上例中的表进行了互换。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.WorkOrder  
EXCEPT  
SELECT ProductID   
FROM Production.Product ;  
--Result: 0 Rows (work orders without products)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
以下示例说明如何使用 `INTERSECT` 和 `EXCEPT` 运算符。 第一个查询返回 `FactInternetSales` 表中的所有值，以便对 `INTERSECT` 和 `EXCEPT` 所返回的结果进行比较。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales;  
--Result: 60398 Rows  
```  
  
以下查询返回 `INTERSECT` 运算符左右两侧的两个查询均返回的所有非重复值。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
INTERSECT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9133 Rows (Sales to customers that are female.)  
```  
  
下面的查询从 `EXCEPT` 运算符左侧的查询返回右侧查询不返回的所有非重复值。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
EXCEPT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9351 Rows (Sales to customers that are not female.)  
```  
