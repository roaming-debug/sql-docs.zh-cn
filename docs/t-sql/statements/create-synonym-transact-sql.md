---
description: CREATE SYNONYM (Transact-SQL)
title: CREATE SYNONYM (Transact-SQL)
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SYNONYM_TSQL
- SYNONYM_TSQL
- SYNONYM
- CREATE SYNONYM
dev_langs:
- TSQL
helpviewer_keywords:
- alternate names [SQL Server]
- names [SQL Server], synonyms
- CREATE SYNONYM statement
- synonyms [SQL Server], creating
ms.assetid: 41313809-e970-449c-bc35-85da2ef96e48
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 181476c45c78374d359e833bea1a0da7aa924a4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192653"
---
# <a name="create-synonym-transact-sql"></a>CREATE SYNONYM (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  创建新的同义词。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- SQL Server Syntax  
  
CREATE SYNONYM [ schema_name_1. ] synonym_name FOR <object>  
  
<object> :: =  
{  
    [ server_name.[ database_name ] . [ schema_name_2 ]. object_name   
  | database_name . [ schema_name_2 ].| schema_name_2. ] object_name  
}  
```  
  
```  
-- Azure SQL Database Syntax  
  
CREATE SYNONYM [ schema_name_1. ] synonym_name FOR < object >  
  
< object > :: =  
{  
    [database_name. [ schema_name_2 ].| schema_name_2. ] object_name  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 schema_name_1  
 指定创建同义词所使用的架构。 如果未指定 *schema*，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会使用当前用户的默认架构。  
  
 *synonym_name*  
 新同义词的名称。  
  
 server_name  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 基对象所在服务器的名称。  
  
 *database_name*  
 基对象所在数据库的名称。 如果未指定 database_name，则使用当前数据库的名称  。  
  
 schema_name_2  
 基对象的架构的名称。 如果未指定 schema_name，则使用当前用户的默认架构  。  
  
 *object_name*  
 同义词被引用基对象的名称。  
  
 Azure SQL 数据库支持由三部分组成的名称格式 database_name.[schema_name].object_name，其中 database_name 为当前数据库，database_name 为 tempdb，object_name 以 # 开头。  
  
## <a name="remarks"></a>备注  
 创建同义词时不需要基对象存在。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在运行时检查基对象是否存在。  
  
 可以为下列对象类型创建同义词：  
  
- 程序集 (CLR) 存储过程
- 程序集 (CLR) 表值函数
- 程序集 (CLR) 标量函数
- 程序集聚合 (CLR) 聚合函数
- 复制筛选过程
- 扩展存储过程
- SQL 标量函数
- SQL 表值函数
- SQL 内联表值函数
- SQL 存储过程
- 表<sup>1</sup>（用户定义）
- 查看

 <sup>1 包括局部临时表和全局临时表</sup>  
  
 不支持使用函数基对象的四部分名称。  
  
 在动态 SQL 中可以创建、删除和引用同义词。
 
 > [!NOTE]
 > 同义词是特定于数据库的，其他数据库无法访问。
  
## <a name="permissions"></a>权限  
 若要在给定架构中创建同义词，则用户必须具有 CREATE SYNONYM 权限，并拥有架构或具有 ALTER SCHEMA 权限。  
  
 CREATE SYNONYM 权限是可授予的权限。  
  
> [!NOTE]  
>  不需要基对象的权限便可成功编译 CREATE SYNONYM 语句，因为基对象的所有权限检查被延迟到运行时进行。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-synonym-for-a-local-object"></a>A. 为本地对象创建同义词  
 下面的示例首先为 `Product` 数据库中的基对象 `AdventureWorks2012` 创建同义词，然后查询该同义词。  
  
```sql 
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
  
-- Query the Product table by using the synonym.  
SELECT ProductID, Name   
FROM MyProduct  
WHERE ProductID < 5;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ----------------------- 
 ProductID   Name 
 ----------- -------------------------- 
 1           Adjustable Race 
 2           Bearing Ball 
 3           BB Ball Bearing 
 4           Headset Ball Bearings 

 (4 row(s) affected)
``` 
  
### <a name="b-creating-a-synonym-to-remote-object"></a>B. 为远程对象创建同义词  
 在下面的示例中，基对象 `Contact` 驻留在名为 `Server_Remote` 的远程服务器上。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql 
EXEC sp_addlinkedserver Server_Remote;  
GO  
USE tempdb;  
GO  
CREATE SYNONYM MyEmployee FOR Server_Remote.AdventureWorks2012.HumanResources.Employee;  
GO  
```  
  
### <a name="c-creating-a-synonym-for-a-user-defined-function"></a>C. 为用户定义的函数创建同义词  
 以下示例创建一个名为 `dbo.OrderDozen` 的函数，该函数将订单金额增加到整打单位。 然后，该示例将为 `dbo.CorrectOrder` 函数创建同义词 `dbo.OrderDozen`。  
  
```sql  
-- Creating the dbo.OrderDozen function  
CREATE FUNCTION dbo.OrderDozen (@OrderAmt INT)  
RETURNS INT  
WITH EXECUTE AS CALLER  
AS  
BEGIN  
IF @OrderAmt % 12 <> 0  
BEGIN  
    SET @OrderAmt +=  12 - (@OrderAmt % 12)  
END  
RETURN(@OrderAmt);  
END;  
GO  
  
-- Using the dbo.OrderDozen function  
DECLARE @Amt INT;  
SET @Amt = 15;  
SELECT @Amt AS OriginalOrder, dbo.OrderDozen(@Amt) AS ModifiedOrder;  
  
-- Create a synonym dbo.CorrectOrder for the dbo.OrderDozen function.  
CREATE SYNONYM dbo.CorrectOrder  
FOR dbo.OrderDozen;  
GO  
  
-- Using the dbo.CorrectOrder synonym.  
DECLARE @Amt INT;  
SET @Amt = 15;  
SELECT @Amt AS OriginalOrder, dbo.CorrectOrder(@Amt) AS ModifiedOrder;  
```  
  
## <a name="see-also"></a>另请参阅  
 [DROP SYNONYM (Transact-SQL)](../../t-sql/statements/drop-synonym-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
