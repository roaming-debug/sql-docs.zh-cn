---
description: RAISERROR (Transact-SQL)
title: RAISERROR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RAISERROR
- RAISERROR_TSQL
- RAISEERROR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmessages system table
- errors [SQL Server], RAISERROR statement
- user-defined error messages [SQL Server]
- system flags
- generating errors [SQL Server]
- TRY block [SQL Server]
- recording errors
- ad hoc messages
- RAISERROR statement
- CATCH block
- messages [SQL Server], RAISERROR statement
ms.assetid: 483588bd-021b-4eae-b4ee-216268003e79
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8402340c86a6b8fd966d0ee9b00a010c02a5e46f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753707"
---
# <a name="raiserror-transact-sql"></a>RAISERROR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!NOTE]
> RAISERROR  语句不遵循 SET XACT_ABORT  。 新应用程序应使用 THROW 而不是 RAISERROR   。

  生成错误消息并启动会话的错误处理。 RAISERROR 可以引用 sys.messages 目录视图中存储的用户定义消息，也可以动态建立消息。 该消息作为服务器错误消息返回到调用应用程序，或返回到 TRY…CATCH 构造的关联 CATCH 块。 新应用程序应改用 [THROW](../../t-sql/language-elements/throw-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
RAISERROR ( { msg_id | msg_str | @local_variable }  
    { ,severity ,state }  
    [ ,argument [ ,...n ] ] )  
    [ WITH option [ ,...n ] ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
RAISERROR ( { msg_str | @local_variable }  
    { ,severity ,state }  
    [ ,argument [ ,...n ] ] )  
    [ WITH option [ ,...n ] ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 msg_id   
 使用 sp_addmessage 存储在 sys.messages 目录视图中的用户定义错误消息号。 用户定义的错误消息的错误号应大于 50000。 若未指定 msg_id，RAISERROR 会引发一个错误号为 50000 的错误消息。   
  
 msg_str   
 用户定义消息，格式与 C 标准库中的 printf 函数类似。  该错误消息最长可以有 2,047 个字符。 如果该消息包含的字符数等于或超过 2,048 个，则只能显示前 2,044 个并添加一个省略号以表示该消息已被截断。 请注意，由于内部存储行为的缘故，代替参数使用的字符数比输出所显示的字符数要多。 例如，赋值为 2 的代替参数 %d 实际在消息字符串中生成一个字符，但是还会在内部占用另外三个存储字符。  此存储要求减少了可用于消息输出的字符数。  
  
 若指定了 msg_str，RAISERROR 会引发一个错误号为 50000 的错误消息。   
  
 msg_str 是一个字符串，具有可选的嵌入转换规格。  每个转换规格都会定义参数列表中的值如何格式化并将其置于 msg_str 中转换规格位置上的字段中。  转换规格的格式如下：  
  
 % [[flag] [width] [.   precision] [{h | l}]] type    
  
 可在 msg_str 中使用的参数包括：   
  
 flag   
  
 用于确定被替换值的间距和对齐的代码。  
  
|代码|前缀或对齐|说明|  
|----------|-----------------------------|-----------------|  
|-（减号）|左对齐|在给定字段宽度内左对齐参数值。|  
|+（加号）|符号前缀|如果参数值为有符号类型，则在参数值的前面加上加号（+）或减号（-）。|  
|0（零）|零填充|在达到最小宽度之前在输出前面加上零。 如果出现 0 和减号 (-)，将忽略 0。|  
|#（数字）|对 x 或 X 的十六进制类型使用 0x 前缀|当使用 o、x 或 X 格式时，数字符号 (#) 标志在任何非零值的前面分别加上 0、0x 或 0X。 当 d、i 或 u 的前面有数字符号 (#) 标志时，将忽略该标志。|  
|' '（空白）|空格填充|如果输出值有符号且为正，则在该值前加空格。 如果包含在加号（+）标志中，则忽略该标志。|  
  
 width   
  
 定义放置参数值的字段的最小宽度的整数。 如果参数值的长度等于或大于 width，则打印该值，无需进行填充。  如果该值小于 width，则将该值填充到 width 中指定的长度。    
  
 星号 (*) 表示宽度由参数列表中的相关参数指定，该宽度必须为整数值。  
  
 *精度*  
  
 从字符串值的参数值中得到的最大字符数。 例如，如果一个字符串具有五个字符并且精度为 3，则只使用字符串值的前三个字符。  
  
 对于整数值，precision 是指打印的最小位数。   
  
 星号 (*) 表示精度由参数列表中的相关参数指定，该精度必须为整数值。  
  
 {h | l} type   
  
 与字符类型 d、i、o、s、x、X 或 u 一起使用，用于创建 shortint (h) 值或 longint (l) 值。    
  
|类型规范|表示|  
|------------------------|----------------|  
|d 或 i|带符号的整数|  
|o|无符号的八进制数|  
|s|字符串|  
|u|无符号的整数|  
|x 或 X|无符号的十六进制数|  
  
> [!NOTE]  
>  这些类型规范基于最初为 C 标准库中 printf 函数定义的规范。  RAISERROR 消息字符串中使用的类型规范映射到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 数据类型，而 printf 中使用的规范映射到 C 语言数据类型。  [!INCLUDE[tsql](../../includes/tsql-md.md)] 没有与关联 C 数据类型类似的数据类型时，RAISERROR 不支持 printf 中使用的类型规范。  例如，[!INCLUDE[tsql](../../includes/tsql-md.md)] 没有指针数据类型，因此 RAISERROR 不支持用于指针的 %p 规范。   
  
> [!NOTE]  
>  要将值转换为 [!INCLUDE[tsql](../../includes/tsql-md.md)] bigint 数据类型，请指定 %I64d   。  
  
 *local_variable\@*  
 是一个可以为任何有效字符数据类型的变量，其中包含的字符串的格式化方式与 msg_str 相同  。 \@local_variable 必须为 char 或 varchar，或者能够隐式转换为这些数据类型。     
  
 severity   
 是与此消息关联的用户定义的[严重性级别](../../relational-databases/errors-events/database-engine-error-severities.md)。 使用 msg_id 引发使用 sp_addmessage 创建的用户定义消息时，RAISERROR 上指定的严重性会替代 sp_addmessage 中指定的严重性。   
  
 任何用户都可以指定 0 到 18 之间的严重级别。 只有 sysadmin 固定服务器角色成员或具有 ALTER TRACE 权限的用户才能指定 19 到 25 之间的严重级别。 若要使用 19 到 25 之间的严重级别，必须选择 WITH LOG 选项。 将小于 0 的严重级别解释为 0。 将大于 25 的严重级别解释为 25。  
  
> [!CAUTION]  
>  20 到 25 之间的严重级别被认为是致命的。 如果遇到致命的严重级别，客户端连接将在收到消息后终止，并将错误记录到错误日志和应用程序日志。  
  
 可以指定“-1”来返回与以下示例中所示的错误关联的严重性值。  
  
```sql  
RAISERROR (15600,-1,-1, 'mysp_CreateCustomer');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 15600, Level 15, State 1, Line 1   
 An invalid parameter or option was specified for procedure 'mysp_CreateCustomer'.
 ```  
  
 State   
 0 到 255 之间的整数。 负值默认为 1。 不应使用大于 255 的值。 
  
 如果在多个位置引发相同的用户定义错误，则针对每个位置使用唯一的状态号有助于找到引发错误的代码段。  
  
 argument   
 用于代替 msg_str 或对应于 msg_id 的消息中定义的变量的参数。   可以有 0 个或更多个替换参数，但替换参数的总数不能超过 20。 每个替换参数可以是本地变量或下列任何数据类型：tinyint、smallint、int、char、varchar、nchar、nvarchar、binary 或 varbinary。          不支持其他数据类型。  
  
 *option*  
 错误的自定义选项，可以是下表中的任一值。  
  
|值|说明|  
|-----------|-----------------|  
|LOG|在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的错误日志和应用程序日志中记录错误。 记录到错误日志的错误目前被限定为最多 440 字节。 只有 sysadmin 固定服务器角色成员或具有 ALTER TRACE 权限的用户才能指定 WITH LOG。<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|NOWAIT|将消息立即发送给客户端。<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|SETERROR|将 @@ERROR 值和 ERROR_NUMBER 值设置为 msg_id 或 50000，不用考虑严重级别。 <br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
  
## <a name="remarks"></a>备注  
 RAISERROR 生成的错误与[!INCLUDE[ssDE](../../includes/ssde-md.md)]代码生成的错误的运行方式相同。 RAISERROR 指定的值由 ERROR_LINE、ERROR_MESSAGE、ERROR_NUMBER、ERROR_PROCEDURE、ERROR_SEVERITY、ERROR_STATE 以及 @@ERROR 等系统函数来报告。 当 RAISERROR 在严重级别为 11 或更高的情况下在 TRY 块中运行，它便会将控制传输至关联的 CATCH 块。 如果 RAISERROR 在下列情况下运行，便会将错误返回到调用方：  
  
-   在任何 TRY 块的作用域之外运行。  
  
-   在严重级别为 10 或更低的情况下在 TRY 块中运行。  
  
-   在严重级别为 20 或更高的情况下终止数据库连接。  
  
 CATCH 块可以使用 RAISERROR 来再次引发调用 CATCH 块的错误，方法是使用 ERROR_NUMBER 和 ERROR_MESSAGE 之类的系统函数检索原始错误消息。 默认情况下，将严重性在 1 到 10 之间的消息的 @@ERROR 设置为 0。  
  
 msg_id 指定 sys.messages 目录视图中可用的用户定义消息时，RAISERROR 按照与应用到使用 msg_str 指定的用户定义消息文本的规则相同的规则处理文本列中的消息。   用户定义消息文本可以包含转换规格，并且 RAISERROR 将参数值映射到转换规格。 使用 sp_addmessage 添加用户定义错误消息，使用 sp_dropmessage 删除用户定义错误消息。  
  
 RAISERROR 可以代替 PRINT 将消息返回到调用应用程序。 RAISERROR 支持类似于 C 标准库中 printf 函数功能的字符代替，而 [!INCLUDE[tsql](../../includes/tsql-md.md)] PRINT 语句则不支持。  PRINT 语句不受 TRY 块的影响，而在严重级别为 11 到 19 的情况下在 TRY 块中运行的 RAISERROR 会将控制传输至关联的 CATCH 块。 指定严重级别为 10 或更低以使用 RAISERROR 返回 TRY 块中的消息，而不必调用 CATCH 块。  
  
 通常，连续的参数替换连续的转换规格；第一个参数替换第一个转换规格，第二个参数替换第二个转换规格，以此类推。 例如，在以下 `RAISERROR` 语句中，第一个参数 `N'number'` 替换第一个转换规格 `%s`，第二个参数 `5` 替换第二个转换规格 `%d.`。  
  
```sql  
RAISERROR (N'This is message %s %d.', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'number', -- First argument.  
           5); -- Second argument.  
-- The message text returned is: This is message number 5.  
GO  
```  
  
 如果为转换规格的宽度或精度指定了星号 (*)，则要用于宽度或精度的值被指定为整数参数值。 在这种情况下，一个转换规格最多可以使用三个参数，分别用作宽度、精度和代替值。  
  
 例如，下列两个 `RAISERROR` 语句都返回相同的字符串。 一个指定参数列表中的宽度值和精度值；另一个指定转换规格中的宽度值和精度值。  
  
```sql  
RAISERROR (N'<\<%*.*s>>', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           7, -- First argument used for width.  
           3, -- Second argument used for precision.  
           N'abcde'); -- Third argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
RAISERROR (N'<\<%7.3s>>', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
```  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-error-information-from-a-catch-block"></a>A. 从 CATCH 块返回错误消息  
 以下代码示例显示如何在 `RAISERROR` 块中使用 `TRY` 使执行跳至关联的 `CATCH` 块中。 它还显示如何使用 `RAISERROR` 返回有关调用 `CATCH` 块的错误的信息。  
  
> [!NOTE]  
>  RAISERROR 仅能生成状态为 1 到 127 的错误。 由于[!INCLUDE[ssDE](../../includes/ssde-md.md)]可能引发状态为 0 的错误，因此，建议您先检查由 ERROR_STATE 返回的错误状态，然后将它作为一个值传递给状态参数 RAISERROR。  
  
```sql  
BEGIN TRY  
    -- RAISERROR with severity 11-19 will cause execution to   
    -- jump to the CATCH block.  
    RAISERROR ('Error raised in TRY block.', -- Message text.  
               16, -- Severity.  
               1 -- State.  
               );  
END TRY  
BEGIN CATCH  
    DECLARE @ErrorMessage NVARCHAR(4000);  
    DECLARE @ErrorSeverity INT;  
    DECLARE @ErrorState INT;  
  
    SELECT   
        @ErrorMessage = ERROR_MESSAGE(),  
        @ErrorSeverity = ERROR_SEVERITY(),  
        @ErrorState = ERROR_STATE();  
  
    -- Use RAISERROR inside the CATCH block to return error  
    -- information about the original error that caused  
    -- execution to jump to the CATCH block.  
    RAISERROR (@ErrorMessage, -- Message text.  
               @ErrorSeverity, -- Severity.  
               @ErrorState -- State.  
               );  
END CATCH;  
```  
  
### <a name="b-creating-an-ad-hoc-message-in-sysmessages"></a>B. 在 sys.messages 中创建即席消息  
 以下示例显示如何引发 sys.messages 目录视图中存储的消息。 该消息通过 `sp_addmessage` 系统存储过程，以消息号 `50005` 添加到 sys.messages 目录视图中。  
  
```sql  
sp_addmessage @msgnum = 50005,  
              @severity = 10,  
              @msgtext = N'<\<%7.3s>>';  
GO  
RAISERROR (50005, -- Message id.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
sp_dropmessage @msgnum = 50005;  
GO  
```  
  
### <a name="c-using-a-local-variable-to-supply-the-message-text"></a>C. 使用局部变量提供消息文本  
 以下代码示例显示如何使用局部变量为 `RAISERROR` 语句提供消息文本。  
  
```sql  
DECLARE @StringVariable NVARCHAR(50);  
SET @StringVariable = N'<\<%7.3s>>';  
  
RAISERROR (@StringVariable, -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [PRINT (Transact-SQL)](../../t-sql/language-elements/print-transact-sql.md)   
 [sp_addmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [xp_logevent (Transact-SQL)](../../relational-databases/system-stored-procedures/xp-logevent-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL&)](../../t-sql/functions/error-state-transact-sql.md)   
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)  
  
  

