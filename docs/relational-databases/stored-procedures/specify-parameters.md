---
title: 指定参数 | Microsoft Docs
description: 了解如何将值传递给参数以及在过程调用期间如何使用每个参数属性。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- parameters [SQL Server], stored procedures
- stored procedures [SQL Server], parameters
- output parameters [SQL Server]
- input parameters [SQL Server]
ms.assetid: 902314fe-5f9c-4d0d-a0b7-27e67c9c70ec
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 476741da7da2e0e463a5a5a0abf4ebf0a8e70d40
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467078"
---
# <a name="specify-parameters"></a>指定参数
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  通过指定过程参数，调用程序可以将值传递给过程的主体。 在执行过程期间，这些值可以用于各种目的。 如果将参数标记为 OUTPUT 参数，则过程参数还可以将值返回给调用程序。  
  
 一个过程最多可以有 2100 个参数，每个参数都有名称、数据类型和方向。 还可以为参数指定默认值（可选）。  
  
 下面的章节提供有关将值传递给参数以及在过程调用期间如何使用每个参数属性的信息。  
  
## <a name="passing-values-into-parameters"></a>将值传递给参数  
 使用过程调用提供的参数值必须为常量或变量，不能将函数名称作为参数值。 变量可以是用户定义的变量或系统变量（如 \@\@spid）。  
  
 下列示例演示如何将参数值传递给过程 `uspGetWhereUsedProductID`。 它们说明了如何将参数作为常量和变量进行传递，以及如何使用变量传递函数值。  
  
```  
USE AdventureWorks2012;  
GO  
-- Passing values as constants.  
EXEC dbo.uspGetWhereUsedProductID 819, '20050225';  
GO  
-- Passing values as variables.  
DECLARE @ProductID int, @CheckDate datetime;  
SET @ProductID = 819;  
SET @CheckDate = '20050225';  
EXEC dbo.uspGetWhereUsedProductID @ProductID, @CheckDate;  
GO  
-- Try to use a function as a parameter value.  
-- This produces an error message.  
EXEC dbo.uspGetWhereUsedProductID 819, GETDATE();  
GO  
-- Passing the function value as a variable.  
DECLARE @CheckDate datetime;  
SET @CheckDate = GETDATE();  
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;  
GO  
```  
  
## <a name="specifying-parameter-names"></a>指定参数名称  
 创建过程并声明参数名时，参数名必须以一个 \@ 字符开头，并且必须在过程范围内是唯一的。  
  
 显式命名参数并将相应的值赋给过程调用中的每个参数允许按任意顺序提供参数。 例如，如果过程 my_proc 需要使用三个参数，分别名为 \@first、\@second和 \@third，可以将传递到此过程的值赋给参数名，如：`EXECUTE my_proc @second = 2, @first = 1, @third = 3;`  
  
> [!NOTE]  
>  如果以 **\@parameter =** _value_ 格式提供参数值，必须按此格式提供所有后续参数。 如果未按格式 **\@parameter =** _value_ 传递参数值，必须按 CREATE PROCEDURE 语句中所列参数顺序（从左到右）提供值。  
  
> [!WARNING]  
>  任何采用 **\@parameter =** _value_ 格式传入的参数如果拼写错误，就会导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成错误，并阻止过程执行。  
  
## <a name="specifying-parameter-data-types"></a>指定参数数据类型  
 在 CREATE PROCEDURE 语句中声明时，必须使用数据类型定义参数。 参数的数据类型确定了在调用过程时该参数所接受值的类型和范围。 例如，如果用 **tinyint** 数据类型定义参数，则在传入该参数时只接受 0 到 255 之间的数值。 如果用与数据类型不兼容的值执行过程，将返回一个错误。  
  
## <a name="specifying-parameter-default-values"></a>指定参数的默认值  
 如果在声明参数时指定了默认值，则参数被视为可选的。 在过程调用中不需要为可选参数提供值。  
  
 在以下情况下使用参数的默认值：  
  
-   在过程调用中未指定参数值。  
  
-   在过程调用中将 DEFAULT 关键字指定为值。  
  
> [!NOTE]  
>  如果默认值是包含嵌入空格或标点符号的字符串，或者以数字开头（例如，6xxx），那么该默认值必须用直的单引号引起来。  

> [!NOTE] 
> Azure Synapse Analytics 或并行数据仓库中不支持默认参数。 
  
 如果没有合适的值可以指定为参数的默认值，则指定 NULL 为默认值。 如果在未提供参数值的情况下执行过程，最好让过程返回自定义的消息。  
  
 下列示例创建带有一个输入参数 `uspGetSalesYTD` 的 `@SalesPerson`过程。 NULL 被指定为该参数的默认值并在错误处理语句中使用，以便在未指定 `@SalesPerson` 参数值的情况下执行过程时返回自定义错误消息。  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.uspGetSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.uspGetSalesYTD;  
GO  
CREATE PROCEDURE Sales.uspGetSalesYTD  
@SalesPerson nvarchar(50) = NULL  -- NULL default value  
AS   
    SET NOCOUNT ON;   
  
-- Validate the @SalesPerson parameter.  
IF @SalesPerson IS NULL  
BEGIN  
   PRINT 'ERROR: You must specify the last name of the sales person.'  
   RETURN  
END  
-- Get the sales for the specified sales person and   
-- assign it to the output parameter.  
SELECT SalesYTD  
FROM Sales.SalesPerson AS sp  
JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
WHERE LastName = @SalesPerson;  
RETURN  
GO  
  
```  
  
 下列示例执行过程。 第一个语句执行过程，而未指定输入值。 这将导致过程中的错误处理语句返回自定义错误消息。 第二个语句提供了输入值，所以返回了所需的结果集。  
  
```  
-- Run the procedure without specifying an input value.  
EXEC Sales.uspGetSalesYTD;  
GO  
-- Run the procedure with an input value.  
EXEC Sales.uspGetSalesYTD N'Blythe';  
GO  
```  
  
 虽然可以省略已提供默认值的参数，但只能截断参数列表。 例如，如果过程有 5 个参数，可以省略第 4 个和第 5 个参数。 不过，只要有第 5 个参数，就不能跳过第 4 个参数，除非采用 **\@parameter =** _value_ 格式提供参数。  
  
## <a name="specifying-parameter-direction"></a>指定参数方向  
 参数的方向可以为输入（表明将值传递给过程的主体），也可以为输出（表明过程将值返回给调用程序）。 默认为输入参数。  
  
 若要指定输出参数，必须在 CREATE PROCEDURE 语句的参数定义中指定 OUTPUT 关键字。 当过程退出时，它向调用程序返回输出参数的当前值。 执行过程时，调用程序也必须使用 OUTPUT 关键字，才能将该参数值保存到可以在调用程序中使用的变量中。  
  
 下例创建 `Production.usp_GetList` 过程，该过程返回价格不超过指定金额的产品的列表。 此示例显示如何使用多个 SELECT 语句和多个 OUTPUT 参数。 使用 OUTPUT 参数，外部过程、批或多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可以访问在过程执行期间设置的值。  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'Production.uspGetList', 'P' ) IS NOT NULL   
    DROP PROCEDURE Production.uspGetList;  
GO  
CREATE PROCEDURE Production.uspGetList @Product varchar(40)   
    , @MaxPrice money   
    , @ComparePrice money OUTPUT  
    , @ListPrice money OUT  
AS  
    SET NOCOUNT ON;  
    SELECT p.[Name] AS Product, p.ListPrice AS 'List Price'  
    FROM Production.Product AS p  
    JOIN Production.ProductSubcategory AS s   
      ON p.ProductSubcategoryID = s.ProductSubcategoryID  
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice;  
-- Populate the output variable @ListPprice.  
SET @ListPrice = (SELECT MAX(p.ListPrice)  
        FROM Production.Product AS p  
        JOIN  Production.ProductSubcategory AS s   
          ON p.ProductSubcategoryID = s.ProductSubcategoryID  
        WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice);  
-- Populate the output variable @compareprice.  
SET @ComparePrice = @MaxPrice;  
GO  
  
```  
  
 执行 `usp_GetList` 以返回价格低于 $700 的 [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] 产品（自行车）的列表。 输出参数 \@cost 和 \@compareprices 用于控制流语言，以便在“消息”窗口中返回消息。  
  
> [!NOTE]  
>  OUTPUT 变量必须在过程创建和变量使用期间进行定义。 参数名称和变量名称不一定要匹配。 不过，数据类型和参数定位必须匹配（除非使用的是 \@listprice= _variable_）。  
  
```  
DECLARE @ComparePrice money, @Cost money ;  
EXECUTE Production.uspGetList '%Bikes%', 700,   
    @ComparePrice OUT,   
    @Cost OUTPUT  
IF @Cost <= @ComparePrice   
BEGIN  
    PRINT 'These products can be purchased for less than   
    $'+RTRIM(CAST(@ComparePrice AS varchar(20)))+'.'  
END  
ELSE  
    PRINT 'The prices for all products in this category exceed   
    $'+ RTRIM(CAST(@ComparePrice AS varchar(20)))+'.';  
  
```  
  
 下面是部分结果集：  
  
```  
Product                                            List Price  
-------------------------------------------------- ------------------  
Road-750 Black, 58                                 539.99  
Mountain-500 Silver, 40                            564.99  
Mountain-500 Silver, 42                            564.99  
...  
Road-750 Black, 48                                 539.99  
Road-750 Black, 52                                 539.99  
  
(14 row(s) affected)  
  
These items can be purchased for less than $700.00.  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)  
  
  
