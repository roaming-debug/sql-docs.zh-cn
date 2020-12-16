---
description: USE (Transact-SQL)
title: USE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/28/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- USE_TSQL
- USE
dev_langs:
- TSQL
helpviewer_keywords:
- USE statement
- database context [SQL Server]
- context changes [SQL Server]
- modifying database context
ms.assetid: c05acac8-c063-4770-8e36-d7f71d500b10
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 27e6f62e7b867fcae8d97a080ead87daf5536b7f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464198"
---
# <a name="use-transact-sql"></a>USE (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  将数据库上下文更改为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的指定数据库或数据库快照。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
USE { database_name }   
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *database_name*  
 用户上下文要切换到的数据库或数据库快照的名称。 数据库和数据库快照名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，数据库参数只能引用当前数据库。 如果提供的数据库不是当前数据库，则 `USE` 语句不会在数据库之间切换，并返回错误代码 40508。 若要更改数据库，您必须直接连接到数据库。 在本页顶部，USE 语句标记为不适用于 SQL 数据库，因为即使可以在批处理中使用 `USE` 语句，它也不会起任何作用。
  
## <a name="remarks"></a>注解  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，该登录将自动连接到它的默认数据库，并获得数据库用户的安全上下文。 如果还没有为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录创建数据库用户，则登录将作为 guest 进行连接。 如果数据库用户在数据库上没有 CONNECT 权限，则 USE 语句将失败。 如果还没有为登录分配默认数据库，则它的默认数据库将设置为 master。  
  
 USE 在编译和执行期间均可执行，并且立即生效。 因此，出现在批处理中 USE 语句之后的语句将在指定数据库中执行。  
  
## <a name="permissions"></a>权限  
 要求对数据库具有 CONNECT 权限。  
  
## <a name="examples"></a>示例  
 以下示例将数据库上下文更改为 `AdventureWorks2012` 数据库。  
  
```sql  
USE AdventureWorks2012;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../statements/create-database-transact-sql.md)   
 [DROP DATABASE (Transact SQL)](../../t-sql/statements/drop-database-transact-sql.md)   
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)  
  
