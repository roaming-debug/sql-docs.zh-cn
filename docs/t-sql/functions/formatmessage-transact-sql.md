---
description: FORMATMESSAGE (Transact-SQL)
title: FORMATMESSAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FORMATMESSAGE
- FORMATMESSAGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmessages system table
- sys.messages catalog view
- FORMATMESSAGE function
- messages [SQL Server], formats
- errors [SQL Server], formats
ms.assetid: 83f18102-2035-4a87-acd0-8d96d03efad5
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4af8d17ee5c75fb5df6690add3c290cef5c52075
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237487"
---
# <a name="formatmessage-transact-sql"></a>FORMATMESSAGE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  根据 sys.messages 中现有的消息或提供的字符串构造一条消息。 FORMATMESSAGE 的功能与 RAISERROR 语句的功能类似。 但是，RAISERROR 会立即打印消息，而 FORMATMESSAGE 则返回供进一步处理的格式化消息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
FORMATMESSAGE ( { msg_number  | ' msg_string ' | @msg_variable} , [ param_value [ ,...n ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 msg_number  
 存储在 sys.messages 中的消息的 ID。 如果 msg_number <= 13000，或者此消息不在 sys.messages 中，则返回 NULL。  
  
 msg_string  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 到[当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。  
  
 用单引号括起来的字符串，其中包含参数值占位符。 该错误消息最长可以有 2,047 个字符。 如果该消息包含的字符数等于或超过 2,048 个，则只能显示前 2,044 个并添加一个省略号以表示该消息已被截断。 请注意，由于内部存储行为的缘故，代替参数使用的字符数比输出所显示的字符数要多。  有关消息字符串结构及在字符串中使用参数的信息，请参阅 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md) 中 msg_str 参数的说明。  

 *@msg_variable*  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 到[当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。  
  
 一个 nvarchar 或 varchar 变量，其中包含符合上述 msg_string 条件的字符串。  
  
 param_value  
 在消息中使用的参数值。 可以是多个参数值。 值的顺序必须与占位符变量在消息中出现的次序相同。 值的最大数目为 20。  
  
## <a name="return-types"></a>返回类型  
 **nvarchar**  
  
## <a name="remarks"></a>备注  
 与 RAISERROR 语句相似，FORMATMESSAGE 用提供的参数值替换消息中的占位符变量来编辑消息。 有关错误消息中允许使用的占位符和编辑过程的详细信息，请参阅 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)。  
  
 FORMATMESSAGE 查找使用用户当前语言的消息。 对于系统消息 (msg_number <=50000)，如果没有本地化的消息版本，则使用操作系统语言版本。 对于用户消息 (msg_number >50000)，如果消息没有本地化版本，则使用英语版本。
  
 对于本地化的消息，提供的参数值必须与美国英语版本中的参数占位符对应。 也就是说，本地化版本的参数 1 必须对应于美国英语版本的参数 1，本地化版本的参数 2 必须对应于美国英语版本的参数 2，依此类推。  
  
## <a name="examples"></a>示例  
  
### <a name="a-example-with-a-message-number"></a>A. 带有消息号的示例  
 下面的示例使用在 sys.messages 中存储的复制消息 `20009`：“项目“%s”无法添加到发布“%s””。FORMATMESSAGE 将为参数占位符替换值 `First Variable` 和 `Second Variable`。 所得到的字符串“项目“First Variable”无法添加到发布“Second Variable””存储在局部变量 `@var1` 中。  
  
```sql
SELECT text FROM sys.messages WHERE message_id = 20009 AND language_id = 1033;  
DECLARE @var1 VARCHAR(200);   
SELECT @var1 = FORMATMESSAGE(20009, 'First Variable', 'Second Variable');   
SELECT @var1;  
```  
  
### <a name="b-example-with-a-message-string"></a>B. 带有消息字符串的示例  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 到[当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。  
  
 以下示例采用字符串作为输入。  
  
```sql
SELECT FORMATMESSAGE('This is the %s and this is the %s.', 'first variable', 'second variable') AS Result;  
```  
  
 返回：`This is the first variable and this is the second variable.`  
  
### <a name="c-additional-message-string-formatting-examples"></a>C. 其他消息字符串格式设置示例  
 以下示例演示了各种格式设置选项。  
  
```sql
SELECT FORMATMESSAGE('Signed int %i, %d %i, %d, %+i, %+d, %+i, %+d', 5, -5, 50, -50, -11, -11, 11, 11);
SELECT FORMATMESSAGE('Signed int with up to 3 leading zeros %03i', 5);  
SELECT FORMATMESSAGE('Signed int with up to 20 leading zeros %020i', 5);  
SELECT FORMATMESSAGE('Signed int with leading zero 0 %020i', -55);  
SELECT FORMATMESSAGE('Bigint %I64d', 3000000000);
SELECT FORMATMESSAGE('Unsigned int %u, %u', 50, -50);  
SELECT FORMATMESSAGE('Unsigned octal %o, %o', 50, -50);  
SELECT FORMATMESSAGE('Unsigned hexadecimal %x, %X, %X, %X, %x', 11, 11, -11, 50, -50);  
SELECT FORMATMESSAGE('Unsigned octal with prefix: %#o, %#o', 50, -50);  
SELECT FORMATMESSAGE('Unsigned hexadecimal with prefix: %#x, %#X, %#X, %X, %x', 11, 11, -11, 50, -50);  
SELECT FORMATMESSAGE('Hello %s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %20s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %-20s!', 'TEST');  
```  
  
## <a name="see-also"></a>另请参阅  
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)  
 [THROW (Transact-SQL)](../../t-sql/language-elements/throw-transact-sql.md)   
 [sp_addmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS (Transact-SQL)](../../t-sql/functions/concat-ws-transact-sql.md)  
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG (Transact-SQL)](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE (Transact-SQL)](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF (Transact-SQL)](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
 [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
