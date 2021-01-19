---
description: DROP PROCEDURE (Transact-SQL)
title: DROP PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP PROCEDURE
- DROP_PROCEDURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- removing stored procedures
- dropping procedure groups
- deleting stored procedures
- deleting procedure groups
- DROP PROCEDURE statement
- dropping stored procedures
- stored procedures [SQL Server], removing
- removing procedure groups
ms.assetid: 1c2d7235-7b9b-4336-8f17-429e7d82c2c3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c810d1e57b3e2cfb092e1091da0d95b082eb8c2d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172469"
---
# <a name="drop-procedure-transact-sql"></a>DROP PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的当前数据库中删除一个或多个存储过程或过程组。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP { PROC | PROCEDURE } [ IF EXISTS ] { [ schema_name. ] procedure } [ ,...n ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP { PROC | PROCEDURE } { [ schema_name. ] procedure_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 IF EXISTS  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 到[当前版本](https://go.microsoft.com/fwlink/p/?LinkId=299658)）。  
  
 有条件地删除过程（仅当其已存在时）。  
  
 *schema_name*  
 过程所属架构的名称。 不能指定服务器名称或数据库名称。  
  
 procedure  
 要删除的存储过程或存储过程组的名称。 不能删除编号过程组内的单个过程；但可删除整个过程组。  
  
## <a name="best-practices"></a>最佳实践  
 在删除任何存储过程之前，请检查依赖对象，并且相应地修改这些对象。 如果没有更新这些对象，则删除存储过程可能会导致依赖对象和脚本失败。 有关详细信息，请参阅[查看存储过程的依赖关系](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)  
  
## <a name="metadata"></a>Metadata  
 若要显示现有过程的列表，请查询 sys.objects 目录视图。 若要显示过程定义，请查询 sys.sql_modules 目录视图。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 需要拥有该过程的 CONTROL 权限，或该过程所属架构的 ALTER 权限，或 db_ddladmin 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例将删除当前数据库中的 `dbo.uspMyProc` 存储过程。  
  
```sql  
DROP PROCEDURE dbo.uspMyProc;  
GO  
```  
  
 下面的示例将删除当前数据库中的多个存储过程。  
  
```sql  
DROP PROCEDURE dbo.uspGetSalesbyMonth, dbo.uspUpdateSalesQuotes, dbo.uspGetSalesByYear;  
```  
  
 以下示例删除 `dbo.uspMyProc` 存储过程（如果存在），但不会在该过程不存在时引发错误。 此语法是 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 中的新语法。  
  
```sql  
DROP PROCEDURE IF EXISTS dbo.uspMyProc;  
GO  
```  
  
  
## <a name="see-also"></a>另请参阅  
 [ALTER PROCEDURE (Transact-SQL)](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [删除存储过程](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)  
  
  


