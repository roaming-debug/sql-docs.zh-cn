---
title: 参数 | Microsoft Docs
description: 了解如何使用参数在存储过程和函数与调用存储过程或函数的应用程序或工具之间交换数据。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], parameters
- user-defined functions [SQL Server], parameters
ms.assetid: c1f9bd93-3271-4098-a23b-7bd7a19ab65b
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9fb820d31c071850d2a2fc510368be4f3bb1839
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473078"
---
# <a name="parameters"></a>参数
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
参数用于在存储过程和函数以及调用存储过程或函数的应用程序或工具之间交换数据： 

*  输入参数允许调用方将数据值传递到存储过程或函数。
*  输出参数允许存储过程将数据值或游标变量传递回调用方。 用户定义函数不能指定输出参数。
*  每个存储过程向调用方返回一个整数返回代码。 如果存储过程没有显式设置返回代码的值，则返回代码为 0。

下面的存储过程说明了输入参数、输出参数和返回代码的用法：
```
-- Create a procedure that takes one input parameter and returns one output parameter and a return code.
CREATE PROCEDURE SampleProcedure @EmployeeIDParm INT,
         @MaxTotal INT OUTPUT
AS
-- Declare and initialize a variable to hold @@ERROR.
DECLARE @ErrorSave INT
SET @ErrorSave = 0

-- Do a SELECT using the input parameter.
SELECT FirstName, LastName, JobTitle
FROM HumanResources.vEmployee
WHERE EmployeeID = @EmployeeIDParm

-- Save any nonzero @@ERROR value.
IF (@@ERROR <> 0)
   SET @ErrorSave = @@ERROR

-- Set a value in the output parameter.
SELECT @MaxTotal = MAX(TotalDue)
FROM Sales.SalesOrderHeader;

IF (@@ERROR <> 0)
   SET @ErrorSave = @@ERROR

-- Returns 0 if neither SELECT statement had an error; otherwise, returns the last error.
RETURN @ErrorSave
GO
```

执行存储过程或函数时，输入参数既可以将它们的值设置为常量，也可以使用变量的值。 输出参数和返回代码必须将其值返回变量。 参数和返回代码可以与 Transact-SQL 变量或应用程序变量交换数据值。

如果从批处理或脚本调用存储过程，则参数和返回代码值可以使用在同一个批处理中定义的 Transact-SQL 变量。 下面是执行以前创建的过程的批处理的示例。 输入参数被指定为常量，输出参数和返回代码将它们的值放在 Transact-SQL 变量中：
```
-- Declare the variables for the return code and output parameter.
DECLARE @ReturnCode INT
DECLARE @MaxTotalVariable INT

-- Execute the stored procedure and specify which variables
-- are to receive the output parameter and return code values.
EXEC @ReturnCode = SampleProcedure @EmployeeIDParm = 19,
   @MaxTotal = @MaxTotalVariable OUTPUT

-- Show the values returned.
PRINT ' '
PRINT 'Return code = ' + CAST(@ReturnCode AS CHAR(10))
PRINT 'Maximum Quantity = ' + CAST(@MaxTotalVariable AS CHAR(10))
GO
```

应用程序可以通过绑定到程序变量的参数标记在应用程序变量、参数和返回代码之间交换数据。

## <a name="see-also"></a>另请参阅
[CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [重用参数和执行计划部分](../../relational-databases/query-processing-architecture-guide.md)   
 [变量 (Transact-SQL)](../../t-sql/language-elements/variables-transact-sql.md)
