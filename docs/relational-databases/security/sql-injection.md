---
title: SQL 注入 | Microsoft Docs
description: 了解 SQL 注入攻击的工作方式。 通过在 SQL Server 中验证 SQL 注入的输入和评审代码，来缓解此类攻击。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: security, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Injection
ms.assetid: eb507065-ac58-4f18-8601-e5b7f44213ab
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ea1d2300b2ff2431d58b8fb2d6139ecad130a965
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432239"
---
# <a name="sql-injection"></a>SQL 注入
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  SQL 注入是一种攻击方式，在这种攻击方式中，在字符串中插入恶意代码，然后将该字符串传递到 SQL Server 的实例以进行分析和执行。 构成 SQL 语句的任何过程都应进行注入漏洞审阅，因为 SQL Server 将执行其接收到的所有语法有效的查询。 一个有经验的、坚定的攻击者甚至可以操作参数化数据。  
  
## <a name="how-sql-injection-works"></a>SQL 注入工作原理  
 SQL 注入的主要形式包括直接将代码插入到与 SQL 命令串联在一起并使其得以执行的用户输入变量。 一种间接的攻击会将恶意代码注入要在表中存储或作为元数据存储的字符串。 在存储的字符串随后串连到一个动态 SQL 命令中时，将执行该恶意代码。  
  
 注入过程的工作方式是提前终止文本字符串，然后追加一个新的命令。 由于插入的命令可能在执行前追加其他字符串，因此攻击者将用注释标记“--”来终止注入的字符串。 执行时，此后的文本将被忽略。  
  
 以下脚本显示了一个简单的 SQL 注入。 此脚本通过串联硬编码字符串和用户输入的字符串而生成一个 SQL 查询：  
  
```csharp
var Shipcity;  
ShipCity = Request.form ("ShipCity");  
var sql = "select * from OrdersTable where ShipCity = '" + ShipCity + "'";  
```  
  
 用户将被提示输入一个市县名称。 如果用户输入 `Redmond`，则查询将由与下面内容相似的脚本组成：  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond'  
```  
  
 但是，假定用户输入以下内容：  
  
```sql
Redmond'; drop table OrdersTable--  
```  
  
 此时，脚本将组成以下查询：  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond';drop table OrdersTable--'  
```  
  
 分号 (;) 表示一个查询的结束和另一个查询的开始。 双连字符 (--) 指示当前行余下的部分是一个注释，应该忽略。 如果修改后的代码语法正确，则服务器将执行该代码。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 处理该语句时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将首先选择 `OrdersTable` 中的所有记录（其中 `ShipCity` 为 `Redmond`）。 然后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将删除 `OrdersTable`。  
  
 只要注入的 SQL 代码语法正确，便无法采用编程方式来检测篡改。 因此，必须验证所有用户输入，并仔细检查在您所用的服务器中执行构造 SQL 命令的代码。 本主题中的以下各部分说明了编写代码的最佳做法。  
  
## <a name="validate-all-input"></a>验证所有输入  
 始终通过测试类型、长度、格式和范围来验证用户输入。 实现对恶意输入的预防时，请注意应用程序的体系结构和部署方案。 请注意，设计为在安全环境中运行的程序可能会被复制到不安全的环境中。 以下建议应被视为最佳做法：  
  
-   对应用程序接收的数据不做任何有关大小、类型或内容的假设。 例如，您应该进行以下评估：  
  
    -   如果一个用户在需要邮政编码的位置无意中或恶意地输入了一个 10 MB 的 MPEG 文件，应用程序会做出什么反应？  
  
    -   如果在文本字段中嵌入了一个 `DROP TABLE` 语句，应用程序会做出什么反应？  
  
-   测试输入的大小和数据类型，强制执行适当的限制。 这有助于防止有意造成的缓冲区溢出。  
  
-   测试字符串变量的内容，只接受所需的值。 拒绝包含二进制数据、转义序列和注释字符的输入内容。 这有助于防止脚本注入，防止某些缓冲区溢出攻击。  
  
-   使用 XML 文档时，根据数据的架构对输入的所有数据进行验证。  
  
-   绝不直接使用用户输入内容来生成 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
-   使用存储过程来验证用户输入。  
  
-   在多层环境中，所有数据都应该在验证之后才允许进入可信区域。 未通过验证过程的数据应被拒绝，并向前一层返回一个错误。  
  
-   实现多层验证。 对无目的的恶意用户采取的预防措施对坚定的攻击者可能无效。 更好的做法是在用户界面和所有跨信任边界的后续点上验证输入。   
    例如，在客户端应用程序中验证数据可以防止简单的脚本注入。 但是，如果下一层认为其输入已通过验证，则任何可以绕过客户端的恶意用户就可以不受限制地访问系统。  
  
-   绝不串联未验证的用户输入。 字符串串联是脚本注入的主要输入点。  
  
-   不接受以下来自可构造文件名的字段中的字符串：AUX、CLOCK$、COM1 到 COM8、CON、CONFIG$、LPT1 到 LPT8、NUL 和 PRN。  
  
 如果可能，拒绝包含以下字符的输入。  
  
|输入字符|在 Transact-SQL 中的含义|  
|---------------------|------------------------------|  
|**;**|查询分隔符。|  
|**”**|字符数据字符串分隔符。|  
|**--**|字符数据字符串分隔符。<br />。|  
|**/\**_ ... _*\*/**|注释分隔符。 服务器不计位于 /\* 和 \*/ 之间的文本。|  
|**xp_**|用于目录扩展存储过程的名称的开头，如 `xp_cmdshell`。|  
  
### <a name="use-type-safe-sql-parameters"></a>使用类型安全的 SQL 参数  
 **中的** Parameters [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 集合提供了类型检查和长度验证。 如果使用 **Parameters** 集合，则输入将被视为文字值而不是可执行代码。 使用 **Parameters** 集合的另一个好处是可以强制执行类型和长度检查。 范围以外的值将触发异常。 以下代码段显示了如何使用 **Parameters** 集合：  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter("AuthorLogin", conn);  
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",  
     SqlDbType.VarChar, 11);  
parm.Value = Login.Text;  
```  
  
 在此示例中， `@au_id` 参数被视为文字值而不是可执行代码。 将对此值进行类型和长度检查。 如果 `@au_id` 值不符合指定的类型和长度约束，则将引发异常。  
  
### <a name="use-parameterized-input-with-stored-procedures"></a>在存储过程中使用参数化输入  
 存储过程如果使用未筛选的输入，则可能容易受 SQL Injection 攻击。 例如，以下代码容易受到攻击：  
  
```csharp
SqlDataAdapter myCommand =   
new SqlDataAdapter("LoginStoredProcedure '" +   
                               Login.Text + "'", conn);  
```  
  
 如果使用存储过程，则应使用参数作为存储过程的输入。  
  
### <a name="use-the-parameters-collection-with-dynamic-sql"></a>在动态 SQL 中使用参数集合  
 如果不能使用存储过程，你仍可使用参数，如以下代码示例所示。  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter(  
"SELECT au_lname, au_fname FROM Authors WHERE au_id = @au_id", conn);  
SQLParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",   
                        SqlDbType.VarChar, 11);  
Parm.Value = Login.Text;  
```  
  
### <a name="filtering-input"></a>筛选输入  
 筛选输入可以删除转义符，这也可能有助于防止 SQL 注入。 但由于可引起问题的字符数量很大，因此这并不是一种可靠的防护方法。 以下示例可搜索字符串分隔符。  
  
```csharp
private string SafeSqlLiteral(string inputSQL)  
{  
  return inputSQL.Replace("'", "''");  
}  
```  
  
### <a name="like-clauses"></a>LIKE 子句  
 请注意，如果要使用 `LIKE` 子句，还必须对通配符字符进行转义：  
  
```csharp
s = s.Replace("[", "[[]");  
s = s.Replace("%", "[%]");  
s = s.Replace("_", "[_]");  
```  
  
## <a name="reviewing-code-for-sql-injection"></a>在代码中检查 SQL 注入  
 应审阅调用 `EXECUTE`、 `EXEC`或 `sp_executesql`的所有代码。 可以使用类似如下的查询来帮助您标识包含这些语句的过程。 此查询检查单词 `EXECUTE` 或 `EXEC`后是否存在 1 个、2 个、3 个或 4 个空格。  
  
```sql
SELECT object_Name(id) FROM syscomments  
WHERE UPPER(text) LIKE '%EXECUTE (%'  
OR UPPER(text) LIKE '%EXECUTE  (%'  
OR UPPER(text) LIKE '%EXECUTE   (%'  
OR UPPER(text) LIKE '%EXECUTE    (%'  
OR UPPER(text) LIKE '%EXEC (%'  
OR UPPER(text) LIKE '%EXEC  (%'  
OR UPPER(text) LIKE '%EXEC   (%'  
OR UPPER(text) LIKE '%EXEC    (%'  
OR UPPER(text) LIKE '%SP_EXECUTESQL%';  
```  
  
### <a name="wrapping-parameters-with-quotename-and-replace"></a>使用 QUOTENAME() 和 REPLACE() 包装参数  
 在选择的每个存储过程中，验证是否对动态 Transact-SQL 中使用的所有变量都进行了正确处理。 来自存储过程的输入参数的数据或从表中读取的数据应包装在 QUOTENAME() 或 REPLACE() 中。 请记住，传递给 QUOTENAME() 的 @variable 值的数据类型为 sysname，且最大长度为 128 个字符。  
  
|\@variable|建议的包装|  
|---------------|-------------------------|  
|安全对象的名称|`QUOTENAME(@variable)`|  
|字符串 ≤ 128 个字符|`QUOTENAME(@variable, '''')`|  
|字符串 > 128 个字符|`REPLACE(@variable,'''', '''''')`|  
  
 使用此方法时，可对 SET 语句进行如下修改：  
  
```sql
-- Before:  
SET @temp = N'SELECT * FROM authors WHERE au_lname ='''   
 + @au_lname + N'''';  
  
-- After:  
SET @temp = N'SELECT * FROM authors WHERE au_lname = '''   
 + REPLACE(@au_lname,'''','''''') + N'''';  
```  
  
### <a name="injection-enabled-by-data-truncation"></a>由数据截断启用的注入  
 如果分配给变量的任何动态 [!INCLUDE[tsql](../../includes/tsql-md.md)] 比为该变量分配的缓冲区大，那么它将被截断。 如果攻击者能够通过将意外长度的字符串传递给存储过程来强制执行语句截断，则该攻击者可以操作该结果。 例如，以下脚本创建的存储过程容易受到由截断启用的注入攻击。  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
@loginname sysname,  
@old sysname,  
@new sysname  
AS  
-- Declare variable.  
-- Note that the buffer here is only 200 characters long.   
DECLARE @command varchar(200)  
-- Construct the dynamic Transact-SQL.  
-- In the following statement, we need a total of 154 characters   
-- to set the password of 'sa'.   
-- 26 for UPDATE statement, 16 for WHERE clause, 4 for 'sa', and 2 for  
-- quotation marks surrounded by QUOTENAME(@loginname):  
-- 200 - 26 - 16 - 4 - 2 = 154.  
-- But because @new is declared as a sysname, this variable can only hold  
-- 128 characters.   
-- We can overcome this by passing some single quotation marks in @new.  
SET @command= 'update Users set password='   
    + QUOTENAME(@new, '''') + ' where username='   
    + QUOTENAME(@loginname, '''') + ' AND password = '   
    + QUOTENAME(@old, '''')  
  
-- Execute the command.  
EXEC (@command)  
GO  
```  
  
 通过向 128 个字符的缓冲区传递 154 个字符，攻击者便可以在不知道旧密码的情况下为 sa 设置新密码。  
  
```sql
EXEC sp_MySetPassword 'sa', 'dummy',   
'123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012'''''''''''''''''''''''''''''''''''''''''''''''''''   
```  
  
 因此，应对命令变量使用较大的缓冲区，或直接在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句内执行动态 `EXECUTE` 。  
  
### <a name="truncation-when-quotenamevariable--and-replace-are-used"></a>使用 QUOTENAME(@variable, '''') 和 REPLACE() 时的截断  
 如果 QUOTENAME() 和 REPLACE() 返回的字符串超过了分配的空间，该字符串将被自动截断。 以下示例中创建的存储过程显示了可能出现的情况。  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, the data stored in temp variables  
-- will be truncated because the buffer size of @login, @oldpassword,  
-- and @newpassword is only 128 characters, but QUOTENAME() can return  
-- up to 258 characters.  
    SET @login = QUOTENAME(@loginname, '''')  
    SET @oldpassword = QUOTENAME(@old, '''')  
    SET @newpassword = QUOTENAME(@new, '''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, then @newpassword will be '123... n  
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated,   
-- it can be made to look like the following statement:  
-- UPDATE Users SET password ='1234. . .[127] WHERE username=' -- other stuff here  
    SET @command = 'UPDATE Users set password = ' + @newpassword   
     + ' where username =' + @login + ' AND password = ' + @oldpassword;  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 因此，以下语句将把所有用户的密码都设置为在前面的代码中传递的值  
  
```sql
EXEC sp_MyProc '--', 'dummy', '12345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678'  
```  
  
 使用 REPLACE() 时，可以通过超出分配的缓冲区空间来强迫字符串截断。 以下示例中创建的存储过程显示了可能出现的情况。  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, data will be truncated because   
-- the buffers allocated for @login, @oldpassword and @newpassword   
-- can hold only 128 characters, but QUOTENAME() can return   
-- up to 258 characters.   
    SET @login = REPLACE(@loginname, '''', '''''')  
    SET @oldpassword = REPLACE(@old, '''', '''''')  
    SET @newpassword = REPLACE(@new, '''', '''''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, @newpassword will be '123...n   
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated, it  
-- can be made to look like the following statement:  
-- UPDATE Users SET password='1234...[127] WHERE username=' -- other stuff here   
    SET @command= 'update Users set password = ''' + @newpassword + ''' where username='''   
     + @login + ''' AND password = ''' + @oldpassword + '''';  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 与 QUOTENAME() 一样，可以通过声明对所有情况都足够大的临时变量来避免由 REPLACE() 引起的字符串截断。 应尽可能直接在动态 [!INCLUDE[tsql](../../includes/tsql-md.md)]内调用 QUOTENAME() 或 REPLACE()。 或者，也可以按如下方式计算所需的缓冲区大小。 对于 `@outbuffer = QUOTENAME(@input)`， `@outbuffer` 的大小应为 `2*(len(@input)+1)`。 使用 `REPLACE()` 和双引号时（如上一示例），大小为 `2*len(@input)` 的缓冲区便已足够。  
  
 以下计算涵盖所有情况：  
  
```sql
WHILE LEN(@find_string) > 0, required buffer size =  
ROUND(LEN(@input)/LEN(@find_string),0) * LEN(@new_string)   
 + (LEN(@input) % LEN(@find_string))  
```  
  
### <a name="truncation-when-quotenamevariable--is-used"></a>使用 QUOTENAME(@variable, ']') 时的截断  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全对象的名称被传递给使用 `QUOTENAME(@variable, ']')`形式的语句时，可能发生截断。 下面的示例显示了这种情况。  
  
```sql
CREATE PROCEDURE sp_MyProc  
    @schemaname sysname,  
    @tablename sysname,  
AS  
  
-- Declare a variable as sysname. The variable will be 128 characters.  
-- But @objectname actually must allow for 2*258+1 characters.   
DECLARE @objectname sysname  
SET @objectname = QUOTENAME(@schemaname)+'.'+ QUOTENAME(@tablename)   
-- Do some operations.  
GO  
```  
  
 串联 sysname 类型的值时，应使用足够大的临时变量来保存每个值的最多 128 个字符。 应尽可能直接在动态 `QUOTENAME()` 内调用 [!INCLUDE[tsql](../../includes/tsql-md.md)]。 或者，也可以按上一部分所述来计算所需的缓冲区大小。  
  
## <a name="see-also"></a>另请参阅  
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [REPLACE (Transact SQL)](../../t-sql/functions/replace-transact-sql.md)   
 [QUOTENAME (Transact SQL)](../../t-sql/functions/quotename-transact-sql.md)   
 [sp_executesql (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)   
 [保护 SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
  
