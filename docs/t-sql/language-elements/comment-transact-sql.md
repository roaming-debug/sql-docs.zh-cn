---
description: --（注释）(Transact-SQL)
title: --（注释）(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- --_TSQL
- Comment
- --
dev_langs:
- TSQL
helpviewer_keywords:
- nonexecuting text strings [SQL Server]
- remarks [SQL Server]
- -- (comment character)
- comments [SQL Server]
ms.assetid: 676ea8c2-52c1-4ef6-9354-320f1a091153
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f6a7fb5590bc4cf7cd9eec459f45aaf1df0e458f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208262"
---
# <a name="---comment-transact-sql"></a>--（注释）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  表示用户提供的文本。 可以将注释插入单独行中，嵌套在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令行的结尾或嵌套在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中。 服务器不对注释进行计算。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- text_of_comment  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 text_of_comment  
 包含注释文本的字符串。  
  
## <a name="remarks"></a>注解  
将两个连字符 (“--”) 用于单行或嵌套的注释。 使用 -- 插入的注释通过一个新行终止，该新行由回车符 (u + 000A)、换行符 (u + 000D) 或二者的组合指定。 注释没有最大长度限制。 下表列出了可用来注释或取消注释文本的键盘快捷键：
  
|操作|标准|  
|------------|--------------|  
|将选定文本设为注释|Ctrl+K、Ctrl+C|  
|取消注释所选文本|Ctrl+K、Ctrl+U|  
  
 有关键盘快捷方式的详细信息，请参阅 [SQL Server Management Studio 键盘快捷方式](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)。  
  
 有关多行注释的信息，请参阅[斜杠星型（块注释）(Transact-SQL)](../../t-sql/language-elements/slash-star-comment-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 下面的示例使用 -- 注释字符。  
  
```sql  
-- Choose the AdventureWorks2012 database.  
USE AdventureWorks2012;  
GO  
-- Choose all columns and all rows from the Address table.  
SELECT *  
FROM Person.Address  
ORDER BY PostalCode ASC; -- We do not have to specify ASC because   
-- that is the default.  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [控制流语言 (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)  
  
  
