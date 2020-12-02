---
description: 斜杠星型（块注释）(Transact-SQL)
title: 斜杠星型（块注释）(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- /*...*/_TSQL
- Comment
- /*...*/
dev_langs:
- TSQL
helpviewer_keywords:
- nonexecuting text strings [SQL Server]
- /*...*/ (comment)
- remarks [SQL Server]
- comments [SQL Server]
ms.assetid: 4d9ab1b2-4bbb-4c16-beb1-cafc1af7417c
author: rothja
ms.author: jroth
ms.openlocfilehash: 161c90803b636ff8aacd67f773c739b0d0153150
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195426"
---
# <a name="slash-star-block-comment-transact-sql"></a>斜杠星型（块注释）(Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]


  表示用户提供的文本。 服务器不计位于 /* 和 \*/ 之间的文本。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
/*  
text_of_comment  
*/  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 text_of_comment  
 是注释的文本。 它是一个或多个字符串。  
  
## <a name="remarks"></a>备注  
 注释可以插入单独行中，也可以插入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中。 多行的注释必须用 /* 和 \*/ 指明。 用于多行注释的样式规则是，第一行用 /\* 开始，接下来的注释行用 \*\*，并且用 \*/ 结束注释。  
  
 注释没有最大长度限制。  
  
 支持嵌套注释。 如果在现有注释内的任意位置上出现 /* 字符模式，便会将其视为嵌套注释的开始，因此，需要使用注释的结尾标记 \*/。 如果没有注释的结尾标记，便会生成错误。  
  
 例如，以下代码生成一个错误。  
  
```sql  
DECLARE @comment AS VARCHAR(20);  
GO  
/*  
SELECT @comment = '/*';  
*/   
SELECT @@VERSION;  
GO   
```  
  
 若要解决该错误，请执行以下更改。  
  
```sql  
DECLARE @comment AS VARCHAR(20);  
GO  
/*  
SELECT @comment = '/*';  
*/ */  
SELECT @@VERSION;  
GO  
```  
  
## <a name="examples"></a>示例  
 以下示例使用注释解释将要执行哪个代码段。  
  
```sql  
USE AdventureWorks2012;  
GO  
/*  
This section of the code joins the Person table with the Address table,   
by using the Employee and BusinessEntityAddress tables in the middle to   
get a list of all the employees in the AdventureWorks2012 database   
and their contact information.  
*/  
SELECT p.FirstName, p.LastName, a.AddressLine1, a.AddressLine2, a.City, a.PostalCode  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e ON p.BusinessEntityID = e.BusinessEntityID   
JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
JOIN Person.Address AS a ON ea.AddressID = a.AddressID;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [-- (注释) (Transact-SQL)](../../t-sql/language-elements/comment-transact-sql.md)   
 [控制流语言 (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)  
  
  

