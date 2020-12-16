---
title: 本机编译 T-SQL 模块的功能
description: 了解 Transact-SQL 外围应用以及本机编译 T-SQL 模块主体支持的功能，如存储过程和标量用户定义函数。
ms.custom: seo-dt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 05515013-28b5-4ccf-9a54-ae861448945b
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3624ab326c6712805d934839fe9403cfe14410e8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485129"
---
# <a name="supported-features-for-natively-compiled-t-sql-modules"></a>本机编译的 T-SQL 模块支持的功能
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


  本主题列出了 T-SQL 的外围应用以及本机编译 T-SQL 模块主体支持的功能，如存储过程 ([CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md))、标量用户定义函数、内联表值函数和触发器。  

 有关本机模块定义的支持功能，请参阅 [对于本机编译的 T-SQL 模块支持的 DDL](../../relational-databases/in-memory-oltp/supported-ddl-for-natively-compiled-t-sql-modules.md)。  

 如需了解不支持构造的完整信息以及如何解决本机编译模块中不支持某些功能的问题，请参阅 [Migration Issues for Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md)。 有关不支持的功能的详细信息，请参阅 [内存中 OLTP 不支持的 Transact-SQL 构造](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)。  

##  <a name="query-surface-area-in-native-modules"></a><a name="qsancsp"></a> 本机模块中的查询外围应用  

支持以下查询构造：  

CASE 表达式：CASE 可用于允许使用有效表达式的任意语句或子句。
   - 适用对象：[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]。  
    从 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 开始，本机编译 T-SQL 模块已支持 CASE 语句。

SELECT 子句：  

-   列名和别名（使用 AS 或 = 语法）。  

-   标量子查询
    - 适用对象：[!INCLUDE[sssql15-md](../../includes/sssql15-md.md)]。
      从 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 开始，本机编译模块已支持标量子查询。

-   TOP*  

-   SELECT DISTINCT  
    - 适用对象：[!INCLUDE[sssql15-md](../../includes/sssql15-md.md)]。
      从 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 开始，本机编译模块已支持 DISTINCT 运算符。

        - 不支持 DISTINCT 聚合。  

-   UNION 和 UNION ALL
    - 适用对象：[!INCLUDE[sssql15-md](../../includes/sssql15-md.md)]。
      从 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 开始，本机编译模块已支持 UNION 和 UNION ALL 运算符。

-   表分配  

FROM 子句：  

-   FROM \<memory optimized table or table variable>  

-   FROM \<natively compiled inline TVF>  

-   LEFT OUTER JOIN、RIGHT OUTER JOIN、CROSS JOIN 和 INNER JOIN。
    - 适用对象：[!INCLUDE[sssql15-md](../../includes/sssql15-md.md)]。
      从 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 开始，本机编译模块已支持 JOINS。

-   子查询 `[AS] table_alias`。 有关详细信息，请参阅 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)。 
    - 适用对象：[!INCLUDE[sssql15-md](../../includes/sssql15-md.md)]。
      从 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 开始，本机编译模块已支持子查询。

WHERE 子句：  

-   筛选器谓词 IS [NOT] NULL  

-   AND、BETWEEN  
-   OR、NOT、IN、EXISTS
    - 适用对象：[!INCLUDE[sssql15-md](../../includes/sssql15-md.md)]。
      从 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 开始，本机编译模块已支持 OR/NOT/IN/EXISTS 运算符。


[GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md) 子句：

- AVG、COUNT、COUNT_BIG、MIN、MAX 和 SUM 聚合函数。  

- 类型 nvarchar、char、varchar、varchar、varbinary 和 Binary 不支持 MIN 和 MAX。  

[ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md) 子句：


- **ORDER BY** 子句中不支持 **DISTINCT** 。


- 如果 ORDER BY 列表中的表达式逐字出现在 GROUP BY 列表中，将受 [GROUP BY (Transact-SQL)](../../t-sql/queries/select-group-by-transact-sql.md) 支持。
  - 例如，支持 GROUP BY a + b ORDER BY a + b，但不支持 GROUP BY a、b ORDER BY a + b。  


HAVING 子句：

- 具有与 WHERE 子句相同的表达式限制。


#### <a name="order-by-and-top-are-supported-in-natively-compiled-modules-with-some-restrictions"></a>本机编译的模块中支持 ORDER BY 和 TOP，但具有某些限制


- 在 **WITH TIES** 子句中不支持 **PERCENT** 或 **TOP** 。


- **ORDER BY** 子句中不支持 **DISTINCT** 。  


- 在 **TOP** 子句中使用常量时，结合使用 **ORDER BY** 与 **TOP** 不支持超过 8,192。
  - 当查询包含联接或聚合函数时，该限制数可能降低。 （例如，对于一个联接（两个表），限制为 4,096 行。 对于两个联接（三个表），限制为 2,730 行。）  
  - 可通过将行数存储在变量中来获得多于 8,192 的结果：  

```sql
DECLARE @v INT = 9000;
SELECT TOP (@v) ... FROM ... ORDER BY ...
```

不过，与使用变量相比，在 **TOP** 子句中使用常量将提供更好的性能。  

这些本机编译的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 上的限制不适用于内存优化表上解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问。  


##  <a name="data-modification"></a><a name="dml"></a> 数据修改  

支持以下 DML 语句。  

-   INSERT VALUES（每条语句一行）和 INSERT ...SELECT  

-   UPDATE  

-   DELETE  

-   UPDATE 和 DELETE 语句支持 WHERE。  

##  <a name="control-of-flow-language"></a><a name="cof"></a> 控制流语言  
 支持以下控制流语言构造。  

-   [IF...ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md)  

-   [WHILE (Transact-SQL)](../../t-sql/language-elements/while-transact-sql.md)  

-   [RETURN (Transact-SQL)](../../t-sql/language-elements/return-transact-sql.md)  

-   [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md) 可以使用所有[内存中 OLTP 支持的数据类型](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)以及内存优化表类型。 可将变量声明为 NULL 或 NOT NULL。  

-   [SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)  

-   [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)  

    - 要优化性能，请对整个本机编译的 T-SQL 模块使用单个 TRY/CATCH 块。  

-   [THROW (Transact-SQL)](../../t-sql/language-elements/throw-transact-sql.md)  

-   BEGIN ATOMIC（存储过程的外层）。 有关详细信息，请参阅 [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md)。  

##  <a name="supported-operators"></a><a name="so"></a> 支持的运算符  
 支持下列运算符。  

-   [比较运算符 (Transact SQL)](../../t-sql/language-elements/comparison-operators-transact-sql.md)（例如，>、\<, >= 和 <=）  

-   一元运算符（+、-）。  

-   二元运算符（*、/、+、-、%（取模））。  

    - 数字和字符串都支持加号运算符 (+)。  

-   逻辑运算符（AND、OR、NOT）。  

-   按位运算符 ~、&、| 和 ^  

-   APPLY 运算符
    - 适用对象：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]。  
      从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始，本机编译模块支持 APPLY 运算符。

##  <a name="built-in-functions-in-natively-compiled-modules"></a><a name="bfncsp"></a> 本机编译模块中的内置函数  
 内存优化表的约束中以及本机编译的 T-SQL 模块中支持以下函数。  

-   所有[数学函数 (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)  

-   日期函数：CURRENT_TIMESTAMP、DATEADD、DATEDIFF、DATEFROMPARTS、DATEPART、DATETIME2FROMPARTS、DATETIMEFROMPARTS、DAY、EOMONTH、GETDATE、GETUTCDATE、MONTH、SMALLDATETIMEFROMPARTS、SYSDATETIME、SYSUTCDATETIME 和 YEAR。  

-   字符串函数：LEN、LTRIM、RTRIM 和 SUBSTRING。  
    - 适用对象：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]。  
      从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始，还支持下列内置函数：TRIM、TRANSLATE 和 CONCAT_WS。  

-   标识函数：SCOPE_IDENTITY  

-   NULL 函数：ISNULL  

-   Uniqueidentifier 函数：NEWID 和 NEWSEQUENTIALID  

-   JSON 函数  
    - 适用对象：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]。  
      从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始，本机编译模块支持 JSON 函数。

-   错误函数：ERROR_LINE、ERROR_MESSAGE、ERROR_NUMBER、ERROR_PROCEDURE、ERROR_SEVERITY 和 ERROR_STATE  

-   系统函数：@@rowcount。 本机编译存储过程中的语句会更新 @@rowcount，因此，可使用本机编译存储过程中的 @@rowcount 来确定受在该本机编译存储过程中执行的上条语句影响的行数。 但是，@@rowcount 在本机编译存储过程执行开始和结束时会重置为 0。  

-   安全性函数：IS_MEMBER({'group' | 'role'})、IS_ROLEMEMBER ('role' [, 'database_principal'])、IS_SRVROLEMEMBER ('role' [, 'login'])、ORIGINAL_LOGIN()、SESSION_USER、CURRENT_USER、SUSER_ID(['login'])、SUSER_SID(['login'] [, Param2])、SUSER_SNAME([server_user_sid])、SYSTEM_USER、SUSER_NAME、USER、USER_ID(['user'])、USER_NAME([id])、CONTEXT_INFO()。

-   可以嵌套本机模块的执行。

##  <a name="auditing"></a><a name="auditing"></a> 审核  
 在本机编译存储过程中支持过程级审核。  

 有关审核的详细信息，请参阅 [创建服务器审核规范和数据库审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md)。  

##  <a name="table-and-query-hints"></a><a name="tqh"></a> 表提示和查询提示  
 支持以下各项：  

-   INDEX、FORCESCAN 和 FORCESEEK 提示，位于表提示语法或查询的 [OPTION Clause (Transact-SQL)](../../t-sql/queries/option-clause-transact-sql.md) 中。 有关详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  

-   FORCE ORDER  

-   LOOP 联接提示  

-   OPTIMIZE FOR  

 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  

##  <a name="limitations-on-sorting"></a><a name="los"></a>排序限制  
 可以在使用 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md) 和 [ORDER BY 子句 (Transact-SQL)](../../t-sql/queries/select-order-by-clause-transact-sql.md) 的查询中对 8,000 多行进行排序。 但是，如果没有 [ORDER BY 子句 (Transact-SQL)](../../t-sql/queries/select-order-by-clause-transact-sql.md)，[TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md) 最多可对 8,000 行进行排序（如果存在联接，则更少）。  

 如果查询同时使用 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md) 运算符和 [ORDER BY 子句 (Transact-SQL)](../../t-sql/queries/select-order-by-clause-transact-sql.md)，则可以对 TOP 运算符指定多达 8192 行。 如果指定超过 8192 行，则将收到错误消息：消息 41398、级别 16、状态 1、程序 \<procedureName>行 \<lineNumber> TOP 运算符可返回最多 8192 行；但请求了 \<number> 行。    

 如果您没有 TOP 子句，则可以使用 ORDER BY 对任何数目的行进行排序。  

 如果您没有使用 ORDER BY 子句，则可以将任何整数值用于 TOP 运算符。  

 使用 TOP N = 8192 的示例：编译  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    SELECT TOP 8192 ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 使用 TOP N > 8192 的示例：无法编译。  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    SELECT TOP 8193 ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 该 8192 行限制仅适用于 `TOP N` ，其中 `N` 是常量，如前面的示例中所示。  如果您需要 `N` 大于 8192，则可以将该值分配给一个变量并且将该变量用于 `TOP`。  

 使用变量的示例：编译  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    DECLARE @v int = 8193   
    SELECT TOP (@v) ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 **对返回的行数的限制：** 有两种情形可减少可由 TOP 运算符返回的行数：  

-   在查询中使用 JOIN。  JOIN 对该限制的影响依赖于查询计划。  

-   在 ORDER BY 子句中使用聚合函数或对聚合函数的引用。  

 用于在 TOP N 中计算最差情形下支持的最大 N 的公式为： `N = floor ( 65536 / number_of_tables * 8 + total_size+of+aggs )`。  

## <a name="see-also"></a>另请参阅  
 [本机编译存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [本机编译存储过程的迁移问题](./a-guide-to-query-processing-for-memory-optimized-tables.md)