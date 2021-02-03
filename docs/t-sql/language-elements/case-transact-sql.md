---
title: CASE (Transact-SQL)
description: CASE 表达式的 Transact-SQL 参考。 CASE 评估条件列表以返回特定结果。
ms.date: 01/26/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CASE_TSQL
- CASE
dev_langs:
- TSQL
helpviewer_keywords:
- CASE expression [Transact-SQL]
- simple CASE expression
- comparing expressions
- searched CASE expression
ms.assetid: 658039ec-8dc2-4251-bc82-30ea23708cee
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e0fbd27d6849d5473862f93083c9d42f3d6e453
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183316"
---
# <a name="case-transact-sql"></a>CASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

计算条件列表，并返回多个可能的结果表达式之一。  
  
 CASE 表达式有两种格式：  
  
-   CASE 简单表达式，它通过将表达式与一组简单的表达式进行比较来确定结果。  
  
-   CASE 搜索表达式，它通过计算一组布尔表达式来确定结果。  
  
 这两种格式都支持可选的 ELSE 参数。  
  
 CASE 可用于允许使用有效表达式的任意语句或子句。 例如，可以在 SELECT、UPDATE、DELETE 和 SET 等语句以及 select_list、IN、WHERE、ORDER BY 和 HAVING 等子句中使用 CASE。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and Azure Synapse Analytics
  
--Simple CASE expression:   
CASE input_expression   
     WHEN when_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END   

--Searched CASE expression:  
CASE  
     WHEN Boolean_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CASE  
     WHEN when_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数  
 input_expression   
 使用简单 CASE 格式时计算的表达式。 input_expression 是任何有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)  。  
  
 WHEN when_expression   
 使用简单 CASE 格式时要与 input_expression 进行比较的简单表达式  。 when_expression 是任何有效的表达式  。 input_expression 及每个 when_expression 的数据类型必须相同或必须是隐式转换的数据类型   。  
  
 THEN result_expression   
 当 input_expression = when_expression 的计算结果为 TRUE 时，或 Boolean_expression 的计算结果为 TRUE 时返回的表达式    。 result expression 是任何有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)  。  
  
 ELSE else_result_expression   
 比较运算计算结果不为 TRUE 时返回的表达式。 如果忽略此参数且比较运算计算结果不为 TRUE，则 CASE 返回 NULL。 else_result_expression 是任何有效的表达式  。 else_result_expression 及任何 result_expression 的数据类型必须相同或必须是隐式转换的数据类型   。  
  
 WHEN Boolean_expression   
 使用 CASE 搜索格式时所计算的布尔表达式。 Boolean_expression 是任何有效的布尔表达式  。  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 从 result_expressions 和可选 else_result_expression 的类型集中返回优先级最高的类型   。 有关详细信息，请参阅[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)。  
  
### <a name="return-values"></a>返回值  
 **CASE 简单表达式：**  
  
 CASE 简单表达式的工作方式如下：将第一个表达式与每个 WHEN 子句中的表达式进行比较，以确定它们是否等效。 如果这些表达式等效，将返回 THEN 子句中的表达式。  
  
-   仅用于等同性检查。  
  
-   按指定的顺序计算每个 WHEN 子句的 input_expression = when_expression。  
  
-   返回首个 input_expression = when_expression 的计算结果为 TRUE 的 result_expression    。  
  
-   如果 input_expression = when_expression 的计算结果均不为 TRUE，则在指定了 ELSE 子句的情况下，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将返回 else_result_expression；若没有指定 ELSE 子句，则返回 NULL 值    。  
  
 **CASE 搜索表达式：**  
  
-   按指定顺序对每个 WHEN 子句的 Boolean_expression 进行计算  。  
  
-   返回首个 Boolean_expression 的计算结果为 TRUE 的 result_expression   。  
  
-   如果 Boolean_expression 的计算结果均不为 TRUE，则在指定了 ELSE 子句的情况下，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 将返回 else_result_expression；若没有指定 ELSE 子句，则返回 NULL 值   。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 仅允许在 CASE 表达式中嵌套 10 个级别。  
  
 CASE 表达式不能用于控制 Transact-SQL 语句、语句块、用户定义函数以及存储过程的执行流。 如需控制流方法的列表，请参阅[控制流语言 (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)。  
  
 CASE 表达式按顺序评估其条件并在满足第一个条件时停止。 在某些情况下，将会先计算表达式，然后 CASE 表达式会将表达式的结果作为其输入接收。 在计算这些表达式时可能会出现错误。 首先计算在 CASE 表达式的 WHEN 参数中出现的聚合表达式，然后将结果提供给 CASE 表达式。 例如，下面的查询将在生成 MAX 聚合的值时生成被零除错误。 在计算 CASE 表达式之前会出现这种情况。  
  
```sql  
WITH Data (value) AS   
(   
SELECT 0   
UNION ALL   
SELECT 1   
)   
SELECT   
   CASE   
      WHEN MIN(value) <= 0 THEN 0   
      WHEN MAX(1/value) >= 100 THEN 1   
   END   
FROM Data ;  
```  
  
 您应该仅依赖于标量表达式（包括返回标量的非相关子查询）的 WHEN 条件的计算顺序，而不应依赖于聚合表达式。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-a-select-statement-with-a-simple-case-expression"></a>A. 使用带有 CASE 简单表达式的 SELECT 语句  
 在 `SELECT` 语句中，`CASE` 简单表达式只能用于等同性检查，而不进行其他比较。 下面的示例使用 `CASE` 表达式更改产品系列类别的显示，以使这些类别更易于理解。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   ProductNumber, Category =  
      CASE ProductLine  
         WHEN 'R' THEN 'Road'  
         WHEN 'M' THEN 'Mountain'  
         WHEN 'T' THEN 'Touring'  
         WHEN 'S' THEN 'Other sale items'  
         ELSE 'Not for sale'  
      END,  
   Name  
FROM Production.Product  
ORDER BY ProductNumber;  
GO  
```  
  
### <a name="b-using-a-select-statement-with-a-searched-case-expression"></a>B. 使用带有 CASE 搜索表达式的 SELECT 语句  
 在 `SELECT` 语句中，`CASE` 搜索表达式允许根据比较值替换结果集中的值。 下面的示例根据产品的价格范围将标价显示为文本注释。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   ProductNumber, Name, "Price Range" =   
      CASE   
         WHEN ListPrice =  0 THEN 'Mfg item - not for resale'  
         WHEN ListPrice < 50 THEN 'Under $50'  
         WHEN ListPrice >= 50 and ListPrice < 250 THEN 'Under $250'  
         WHEN ListPrice >= 250 and ListPrice < 1000 THEN 'Under $1000'  
         ELSE 'Over $1000'  
      END  
FROM Production.Product  
ORDER BY ProductNumber ;  
GO  
```  
  
### <a name="c-using-case-in-an-order-by-clause"></a>C. 在 ORDER BY 子句中使用 CASE  
 下面的示例在 ORDER BY 子句中使用 CASE 表达式，以根据给定的列值确定行的排序顺序。 在第一个示例中，会计算 `SalariedFlag` 表中 `HumanResources.Employee` 列的值。 `SalariedFlag` 设置为 1 的员工将按 `BusinessEntityID` 以降序顺序返回。 `SalariedFlag` 设置为 0 的员工将按 `BusinessEntityID` 以升序顺序返回。 在第二个示例中，当 `TerritoryName` 列等于“United States”时，结果集会按 `CountryRegionName` 列排序，对于所有其他行则按 `CountryRegionName` 排序。  
  
```sql  
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO    
```  
  
```sql  
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
### <a name="d-using-case-in-an-update-statement"></a>D. 在 UPDATE 语句中使用 CASE  
 下面的示例在 UPDATE 语句中使用 CASE 表达式，以确定为 `VacationHours` 设置为 0 的员工的 `SalariedFlag` 列所设置的值。 如果 `VacationHours` 减去 10 小时后会得到一个负值，则 `VacationHours` 将增加 40 小时；否则 `VacationHours` 将增加 20 小时。 OUTPUT 子句用于显示前后的休假时间值。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee  
SET VacationHours =   
    ( CASE  
         WHEN ((VacationHours - 10.00) < 0) THEN VacationHours + 40  
         ELSE (VacationHours + 20.00)  
       END  
    )  
OUTPUT Deleted.BusinessEntityID, Deleted.VacationHours AS BeforeValue,   
       Inserted.VacationHours AS AfterValue  
WHERE SalariedFlag = 0;   
```  
  
### <a name="e-using-case-in-a-set-statement"></a>E. 在 SET 语句中使用 CASE  
 下面的示例在表值函数 `dbo.GetContactInfo` 中的 SET 语句中使用 CASE 表达式。 在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中，与人员有关的所有数据都存储在 `Person.Person` 表中。 例如，该人员可以是员工、供应商代表或消费者。 该函数将返回给定 `BusinessEntityID` 的名字与姓氏以及该人员的联系人类型。SET 语句中的 CASE 表达式将根据该 `BusinessEntityID` 列是存在于 `Employee`、`Vendor` 还是存在于 `Customer` 表中来确定要为 `ContactType` 列显示的值。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE FUNCTION dbo.GetContactInformation(@BusinessEntityID INT)  
    RETURNS @retContactInformation TABLE   
(  
BusinessEntityID INT NOT NULL,  
FirstName NVARCHAR(50) NULL,  
LastName NVARCHAR(50) NULL,  
ContactType NVARCHAR(50) NULL,  
    PRIMARY KEY CLUSTERED (BusinessEntityID ASC)  
)   
AS   
-- Returns the first name, last name and contact type for the specified contact.  
BEGIN  
    DECLARE   
        @FirstName NVARCHAR(50),   
        @LastName NVARCHAR(50),   
        @ContactType NVARCHAR(50);  
  
    -- Get common contact information  
    SELECT   
        @BusinessEntityID = BusinessEntityID,   
@FirstName = FirstName,   
        @LastName = LastName  
    FROM Person.Person   
    WHERE BusinessEntityID = @BusinessEntityID;  
  
    SET @ContactType =   
        CASE   
            -- Check for employee  
            WHEN EXISTS(SELECT * FROM HumanResources.Employee AS e   
                WHERE e.BusinessEntityID = @BusinessEntityID)   
                THEN 'Employee'  
  
            -- Check for vendor  
            WHEN EXISTS(SELECT * FROM Person.BusinessEntityContact AS bec  
                WHERE bec.BusinessEntityID = @BusinessEntityID)   
                THEN 'Vendor'  
  
            -- Check for store  
            WHEN EXISTS(SELECT * FROM Purchasing.Vendor AS v            
                WHERE v.BusinessEntityID = @BusinessEntityID)   
                THEN 'Store Contact'  
  
            -- Check for individual consumer  
            WHEN EXISTS(SELECT * FROM Sales.Customer AS c   
                WHERE c.PersonID = @BusinessEntityID)   
                THEN 'Consumer'  
        END;  
  
    -- Return the information to the caller  
    IF @BusinessEntityID IS NOT NULL   
    BEGIN  
        INSERT @retContactInformation  
        SELECT @BusinessEntityID, @FirstName, @LastName, @ContactType;  
    END;  
  
    RETURN;  
END;  
GO  
  
SELECT BusinessEntityID, FirstName, LastName, ContactType  
FROM dbo.GetContactInformation(2200);  
GO  
SELECT BusinessEntityID, FirstName, LastName, ContactType  
FROM dbo.GetContactInformation(5);  
```  
  
### <a name="f-using-case-in-a-having-clause"></a>F. 在 HAVING 子句中使用 CASE  
 下面的示例在 HAVING 子句中使用 CASE 表达式，以限制由 SELECT 语句返回的行。 该语句将返回 `HumanResources.Employee` 表中针对每个职位的最高每小时薪金。 HAVING 子句将职位限制为两类员工：一是最高每小时薪金超过 40 美元的男性员工，二是最高每小时薪金超过 42 美元的女性员工。  
  
```sql 
USE AdventureWorks2012;  
GO  
SELECT JobTitle, MAX(ph1.Rate)AS MaximumRate  
FROM HumanResources.Employee AS e  
JOIN HumanResources.EmployeePayHistory AS ph1 ON e.BusinessEntityID = ph1.BusinessEntityID  
GROUP BY JobTitle  
HAVING (MAX(CASE WHEN Gender = 'M'   
        THEN ph1.Rate   
        ELSE NULL END) > 40.00  
     OR MAX(CASE WHEN Gender  = 'F'   
        THEN ph1.Rate    
        ELSE NULL END) > 42.00)  
ORDER BY MaximumRate DESC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="g-using-a-select-statement-with-a-case-expression"></a>G. 将 SELECT 语句和 CASE 表达式结合使用  
 在 SELECT 语句中，CASE 表达式允许根据比较值替换结果集中的值。 下面的示例使用 CASE 表达式更改产品系列类别的显示，以使这些类别更易于理解。 不存在任何值时，则显示文本“Not for sale”。  
  
```sql 
-- Uses AdventureWorks  
  
SELECT   ProductAlternateKey, Category =  
      CASE ProductLine  
         WHEN 'R' THEN 'Road'  
         WHEN 'M' THEN 'Mountain'  
         WHEN 'T' THEN 'Touring'  
         WHEN 'S' THEN 'Other sale items'  
         ELSE 'Not for sale'  
      END,  
   EnglishProductName  
FROM dbo.DimProduct  
ORDER BY ProductKey;  
```  
  
### <a name="h-using-case-in-an-update-statement"></a>H. 在 UPDATE 语句中使用 CASE  
 下面的示例在 UPDATE 语句中使用 CASE 表达式，以确定为 `VacationHours` 设置为 0 的员工的 `SalariedFlag` 列所设置的值。 如果 `VacationHours` 减去 10 小时后会得到一个负值，则 `VacationHours` 将增加 40 小时；否则 `VacationHours` 将增加 20 小时。  
  
```sql 
-- Uses AdventureWorks   
  
UPDATE dbo.DimEmployee  
SET VacationHours =   
    ( CASE  
         WHEN ((VacationHours - 10.00) < 0) THEN VacationHours + 40  
         ELSE (VacationHours + 20.00)   
       END  
    )   
WHERE SalariedFlag = 0;  
```  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [COALESCE (Transact-SQL)](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [IIF (Transact-SQL)](../../t-sql/functions/logical-functions-iif-transact-sql.md)   
 [CHOOSE (Transact-SQL)](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  



