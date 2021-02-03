---
description: PERCENTILE_DISC (Transact-SQL)
title: PERCENTILE_DISC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERCENTILE_DISC
- PERCENTILE_DISC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PERCENTILE_DISC function
- analytic functions,PERCENTILE_DISC
ms.assetid: b545413d-c4f7-4c8e-8617-607599a26680
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c141e0a1bca165af7a770be3823c878476b853a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211346"
---
# <a name="percentile_disc-transact-sql"></a>PERCENTILE_DISC (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  计算 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中整个行集内或行集的非重复分区内已排序值的特定百分位数。 对于给定的百分位数值 P  ，PERCENTILE_DISC 在 ORDER BY 子句中对表达式值进行排序。 然后，它返回值，其中给定的最小 CUME_DIST 值（相对于相同的排序规范）大于或等于 P  。例如，PERCENTILE_DISC (0.5) 将计算表达式的第 50 百分位数（也即中值）。 PERCENTILE_DISC 根据列值的离散分布来计算百分位数。 结果等于特定列值。  
  
 ![项目链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
PERCENTILE_DISC ( numeric_literal ) WITHIN GROUP ( ORDER BY order_by_expression [ ASC | DESC ] )  
    OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 literal  
 要计算的百分位数。 该值必须介于 0.0 和 1.0 之间。  
  
 WITHIN GROUP (  ORDER BY order_by_expression  [ ASC  | DESC)**  
 指定要排序的一系列值，并计算百分位数。 仅允许一个 order_by_expression  。 默认的排序顺序为升序。 这一系列值可属于可有效进行排序操作的任何数据类型。  
  
 OVER ( \<partition_by_clause>)**  
 将 FROM 子句的结果集分为多个分区。 百分位数函数应用于这些分区。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。 不能在 PERCENTILE_DISC 函数中指定 \<ORDER BY clause> 和 \<rows or range clause>。  
  
## <a name="return-types"></a>返回类型  
 返回类型由 order_by_expression 类型决定  。  
  
## <a name="compatibility-support"></a>兼容性支持  
 在兼容级别 110 和更高级别中，WITHIN GROUP 是保留关键字。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
## <a name="general-remarks"></a>一般备注  
 数据集中的任何 null 都将被忽略。  
  
 PERCENTILE_DISC 具有不确定性。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="basic-syntax-example"></a>基本语法示例  

 下面的示例使用 PERCENTILE_CONT 和 PERCENTILE_DISC 函数查找每个部门的员工薪金中值。 它们可能不会返回相同的值：
* PERCENTILE_CONT 返回适当的值，即使它在数据集中不存在，也不例外。
* PERCENTILE_DISC 返回实际集值。  
  
```  
USE AdventureWorks2012;  
  
SELECT DISTINCT Name AS DepartmentName  
      ,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianCont  
      ,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianDisc  
FROM HumanResources.Department AS d  
INNER JOIN HumanResources.EmployeeDepartmentHistory AS dh   
    ON dh.DepartmentID = d.DepartmentID  
INNER JOIN HumanResources.EmployeePayHistory AS ph  
    ON ph.BusinessEntityID = dh.BusinessEntityID  
WHERE dh.EndDate IS NULL;  
```  
  
 以下为部分结果集。  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.8269      16.8269
Engineering            34.375       32.6923
Executive              54.32695     48.5577
Human Resources        17.427850    16.5865
```
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="basic-syntax-example"></a>基本语法示例  

 下面的示例使用 PERCENTILE_CONT 和 PERCENTILE_DISC 函数查找每个部门的员工薪金中值。 它们可能不会返回相同的值：
* PERCENTILE_CONT 返回适当的值，即使它在数据集中不存在，也不例外。 
* PERCENTILE_DISC 返回实际集值。  
  
```  
-- Uses AdventureWorks  
  
SELECT DISTINCT DepartmentName  
       ,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY BaseRate)  
        OVER (PARTITION BY DepartmentName) AS MedianCont  
       ,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY BaseRate)  
        OVER (PARTITION BY DepartmentName) AS MedianDisc  
FROM dbo.DimEmployee;  
  
```  
  
 以下为部分结果集。  
  
 ```
DepartmentName        MedianCont    MedianDisc  
--------------------   ----------   ----------  
Document Control       16.826900    16.8269  
Engineering            34.375000    32.6923  
Human Resources        17.427850    16.5865  
Shipping and Receiving  9.250000     9.0000
```  
  
## <a name="see-also"></a>另请参阅  
 [PERCENTILE_CONT (Transact-SQL)](../../t-sql/functions/percentile-cont-transact-sql.md)  
  
  


