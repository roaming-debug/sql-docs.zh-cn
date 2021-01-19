---
title: 针对内存中 OLTP 的标量用户定义函数 | Microsoft Docs
description: 了解如何在 SQL Server 中为内存中 OLTP 创建和删除本机编译的标量用户定义函数。 本机编译可提高性能。
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: d2546e40-fdfc-414b-8196-76ed1f124bf5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9fd391acc854c4ae337789149955c331ac9543eb
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172029"
---
# <a name="scalar-user-defined-functions-for-in-memory-oltp"></a>针对内存中 OLTP 的标量用户定义函数
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]中，可以创建和删除本机编译标量用户定义函数。 还可以修改这些用户定义函数。 本机编译可提高 Transact-SQL 中用户定义函数的计算性能。  
  
 如果修改本机编译标量用户定义函数，那么在执行操作以及编译新版本函数的同时，该应用程序仍然可用。  
  
 有关支持的 T-SQL 构造，请参阅 [本机编译的 T-SQL 模块支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。  
  
## <a name="creating-dropping-and-altering-user-defined-functions"></a>创建、删除和修改用户定义函数  
 可以使用 CREATE FUNCTION 创建本机编译标量用户定义函数，使用 DROP FUNCTION 删除用户定义函数，使用 ALTER FUNCTION 修改函数。 BEGIN ATOMIC WITH 对用户定义函数是必需的。  
  
 有关支持的语法和任何限制的信息，请参阅下列主题。  
  
-   [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)  
  
-   [ALTER FUNCTION (Transact-SQL)](../../t-sql/statements/alter-function-transact-sql.md)  
  
-   [DROP FUNCTION (Transact-SQL)](../../t-sql/statements/drop-function-transact-sql.md)  
  
     本机编译标量用户定义函数的 DROP FUNCTION 语法与解释用户定义函数相同。  
  
-   [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)  
  
 [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) 存储过程可用于本机编译标量用户定义函数。 它将导致使用元数据中存在的定义重新编译该函数。  
  
 下面的示例演示 [AdventureWorks2016CTP3](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) 示例数据库中的标量 UDF。  
  
```sql  
CREATE FUNCTION [dbo].[ufnLeadingZeros_native](@Value int)   
RETURNS varchar(8)   
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS   
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'English')  
  
    DECLARE @ReturnValue varchar(8);  
    SET @ReturnValue = CONVERT(varchar(8), @Value);  
       DECLARE @i int = 0, @count int = 8 - LEN(@ReturnValue)  
  
    WHILE @i < @count  
       BEGIN  
            SET @ReturnValue = '0' + @ReturnValue;  
            SET @i += 1  
       END  
  
    RETURN (@ReturnValue);  
  
END  
```  
  
## <a name="calling-user-defined-functions"></a>调用用户定义函数  
 可以在表达式中，在与内置标量函数和解释标量用户定义函数相同的位置使用本机编译标量用户定义函数。 在 Transact-SQL 语句和本机编译存储过程中，本机编译标量用户定义函数还可用于 EXECUTE 语句。  
  
 可以在本机编译存储过程和本机编译用户定义函数中允许使用内置函数的位置使用这些标量用户定义函数。 此外还可以在传统的 Transact-SQL 模块中使用本机编译标量用户定义函数。  
  
 可以在能够使用解释标量用户定义函数的位置以互操作模式使用这些标量用户定义函数。 这一使用要遵守交叉容器事务限制，如 **内存优化表中的事物** 中的 [交叉容器事务的支持隔离级别](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)部分所述。 有关互操作模式的详细信息，请参阅 [使用解释型 Transact-SQL 访问内存优化表](../../relational-databases/in-memory-oltp/accessing-memory-optimized-tables-using-interpreted-transact-sql.md)。  
  
 本机编译标量用户定义函数需要显式的执行上下文。 有关详细信息，请参阅 [EXECUTE AS 子句 (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)。 不支持 EXECUTE AS CALLER。 有关详细信息，请参阅 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)。  
  
 有关本机编译标量用户定义函数的 Transact-SQL Execute 语句所支持的语法，请参阅 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)。 有关在本机编译存储过程中执行用户定义函数时所支持的语法，请参阅 [本机编译的 T-SQL 模块支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。  
  
## <a name="hints-and-parameters"></a>提示和参数  
 本机编译标量用户定义函数与本机编译存储过程一样，都支持表、链接和查询提示。 与解释标量用户定义函数一样，引用本机编译标量用户定义函数的 Transact-SQL 查询中包含的查询提示不会影响此用户定义函数的查询计划。  
  
 本机编译标量用户定义函数支持的参数就是本机编译存储过程支持的所有参数，前提是标量用户定义函数允许使用参数。 其中一个支持的参数示例就是表值参数。  
  
## <a name="schema-bound"></a>架构绑定  
 下列各项适用于本机编译标量用户定义函数。  
  
-   必须是架构绑定，通过在 CREATE FUNCTION 和 ALTER FUNCTION 中使用 WITH SCHEMABINDING 参数实现。  
  
-   如果由架构绑定存储过程或用户定义函数引用，则不能删除或更改。  
  
## <a name="showplan_xml"></a>SHOWPLAN_XML  
 本机编译标量用户定义函数支持 SHOWPLAN_XML。 与本机编译存储过程一样，它遵循常规 SHOWPLAN_XML 架构。 用户定义函数的基元素是 `<UDF>`。  
  
 本机编译标量用户定义函数不支持 STATISTICS XML。 当在启用 STATISTICS XML 的情况下运行引用用户定义函数的查询时，会返回 XML 内容，而不返回用户定义函数部分。  
  
## <a name="permissions"></a>权限  
 与本机编译存储过程一样，创建函数时，将检查从本机编译标量用户定义函数引用的对象的权限。 如果模拟的用户没有正确的权限，CREATE FUNCTION 将失败。 如果权限更改导致模拟的用户不再具有正确的权限，用户定义函数的后续执行将失败。  
  
 当在本机编译存储过程中使用本机编译标量用户定义函数时，如果创建外部过程，则将检查执行用户定义函数的权限。 如果外部过程模拟的用户对用户定义函数没有 EXEC 权限，该存储过程的创建将失败。 如果权限更改导致用户不再具有 EXEC 权限，外部过程的执行将失败。  
  
## <a name="see-also"></a>另请参阅  
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [以 XML 格式保存执行计划](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
