---
description: PERCENTILE_CONT (Transact-SQL)
title: PERCENTILE_CONT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERCENTILE_CONT_TSQL
- PERCENTILE_CONT
dev_langs:
- TSQL
helpviewer_keywords:
- analytic functions, PERCENTILE_CONT
- PERCENTILE_CONT function
ms.assetid: d019419e-5297-4994-97d5-e9c8fc61bbf4
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 074f2e7e972dd9795cb1f84b98d0dd73feb25102
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753037"
---
# <a name="percentile_cont-transact-sql"></a>PERCENTILE_CONT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 列值的连续分布计算百分位数。 将内插结果，且结果可能不等于列中的任何特定值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
PERCENTILE_CONT ( numeric_literal )   
    WITHIN GROUP ( ORDER BY order_by_expression [ ASC | DESC ] )  
    OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *numeric_literal*  
 要计算的百分位数。 该值必须介于 0.0 和 1.0 之间。  
  
 WITHIN GROUP ( ORDER BY order_by_expression [ ASC | DESC ])  
 指定要排序的数值列表，并计算百分位数。 仅允许一个 order_by_expression  。 表达式必须计算得出精确的或近似数值类型，并且不允许使用其他数据类型。 精确数值类型为 int、bigint、smallint、tinyint、numeric、bit、decimal、smallmoney 和 money          。 近似数值类型为 float 和 real   。 默认的排序顺序为升序。  
  
 OVER ( \<partition_by_clause> )   
 将 FROM 子句生成的结果集划分为数个应用百分位数函数的分区。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。 无法在 PERCENTILE_CONT 函数中指定 OVER 语法的 \<ORDER BY clause> 和 \<rows or range clause>。  
  
## <a name="return-types"></a>返回类型  
 **float(53)**  
  
## <a name="compatibility-support"></a>兼容性支持  
 在兼容级别 110 和更高级别中，WITHIN GROUP 是保留关键字。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
## <a name="general-remarks"></a>一般备注  
 数据集中的任何 null 都将被忽略。  
  
 PERCENTILE_CONT 具有不确定性。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-basic-syntax-example"></a>A. 基本语法示例  
 下面的示例使用 PERCENTILE_CONT 和 PERCENTILE_DISC 函数找出每个部门内雇员的薪金中值。 这些函数可能返回不同的值。 PERCENTILE_CONT 内插适当的值，它在数据集中可能存在，也可能不存在，而 PERCENTILE_DISC 始终从数据集中返回实际值。  
  
```sql  
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

### <a name="b-basic-syntax-example"></a>B. 基本语法示例  
 下面的示例使用 PERCENTILE_CONT 和 PERCENTILE_DISC 函数找出每个部门内雇员的薪金中值。 这些函数可能返回不同的值。 PERCENTILE_CONT 内插适当的值，它在数据集中可能存在，也可能不存在，而 PERCENTILE_DISC 始终从数据集中返回实际值。  
  
```sql  
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
Shipping and Receiving 9.250000      9.0000
```  
  
## <a name="see-also"></a>另请参阅  
 [PERCENTILE_DISC (Transact-SQL)](../../t-sql/functions/percentile-disc-transact-sql.md)  
  
 
