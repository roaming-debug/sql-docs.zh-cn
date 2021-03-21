---
description: ALTER PROCEDURE (Transact-SQL)
title: ALTER PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_PROCEDURE_TSQL
- ALTER_PROC_TSQL
- ALTER PROC
- ALTER PROCEDURE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PROCEDURE statement
- stored procedure modifications [SQL Server]
- modifying stored procedures
- stored procedures [SQL Server], modifying
ms.assetid: ed9b2f76-11ec-498d-a95e-75b490a75733
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 10491ba478d5aec85f6e213756ebe00e9904b59d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746417"
---
# <a name="alter-procedure-transact-sql"></a>ALTER PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  修改先前在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中通过执行 CREATE PROCEDURE 语句创建的过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name [ ; number ]   
    [ { @parameter [ type_schema_name. ] data_type }   
        [ VARYING ] [ = default ] [ OUT | OUTPUT ] [READONLY]  
    ] [ ,...n ]   
[ WITH <procedure_option> [ ,...n ] ]  
[ FOR REPLICATION ]   
AS { [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }  
[;]  
  
<procedure_option> ::=   
    [ ENCRYPTION ]  
    [ RECOMPILE ]  
    [ EXECUTE AS Clause ]  
```  
  
```syntaxsql  
-- Syntax for SQL Server CLR Stored Procedure  
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name [ ; number ]   
    [ { @parameter [ type_schema_name. ] data_type }   
        [ = default ] [ OUT | OUTPUT ] [READONLY]  
    ] [ ,...n ]   
[ WITH EXECUTE AS Clause ]  
AS { EXTERNAL NAME assembly_name.class_name.method_name }  
[;]  
```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name  
    [ { @parameterdata_type } [= ] ] [ ,...n ]  
AS { [ BEGIN ] sql_statement [ ; ] [ ,...n ] [ END ] }  
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *schema_name*  
 过程所属架构的名称。  
  
 procedure_name  
 要更改的过程的名称。 过程名称必须符合 [标识符](../../relational-databases/databases/database-identifiers.md)规则。  
  
 **;** *number*  
 现有的可选整数，该整数用来对具有同一名称的过程进行分组，以便可以用一个 DROP PROCEDURE 语句全部删除它们。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 **@** *parameter*  
 过程中的参数。 最多可以指定 2,100 个参数。  
  
 [ _type\_schema\_name_ **.** ] _data\_type_  
 参数及其所属架构的数据类型。  
  
 有关数据类型限制的信息，请参阅 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)。  
  
 VARYING  
 指定作为输出参数支持的结果集。 此参数由存储过程动态构造，并且其内容可以不同。 仅适用于游标参数。 该选项对于 CLR 过程无效。  
  
 *default*  
 参数的默认值。  
  
 OUT | OUTPUT  
 指示参数是返回参数。  
  
 READONLY  
 指示不能在过程的主体中更新或修改参数。 如果参数类型为表值类型，则必须指定 READONLY。  
  
 RECOMPILE  
 指示[!INCLUDE[ssDE](../../includes/ssde-md.md)]不会缓存该过程的计划，该过程在运行时重新编译。   
  
 ENCRYPTION  
 **适用对象**：SQL Server（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]。  
  
 指示[!INCLUDE[ssDE](../../includes/ssde-md.md)]会将 ALTER PROCEDURE 语句的原始文本转换为模糊格式。 模糊代码的输出在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的任何目录视图中都不能直接显示。 对系统表或数据库文件没有访问权限的用户不能检索模糊文本。 但是，可以通过 [DAC 端口](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)访问系统表的特权用户或直接访问数据库文件的特权用户可以使用此文本。 此外，能够向服务器进程附加调试器的用户可在运行时从内存中检索原始过程。 有关如何访问系统元数据的详细信息，请参阅[元数据可见性配置](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
 使用此选项创建的过程不能作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制的一部分发布。  
  
 不能为公共语言运行时 (CLR) 存储过程指定此选项。  
  
> [!NOTE]  
>  在升级过程中，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 使用存储在 sys.sql_modules 中的模糊注释来重新创建过程。  
  
 EXECUTE AS  
 指定访问存储过程后执行该存储过程所用的安全上下文。  
  
 有关详细信息，请参阅 [EXECUTE AS 子句 (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)。  
  
 FOR REPLICATION  

  
 指定不能在订阅服务器上执行为复制创建的存储过程。 使用 FOR REPLICATION 选项创建的存储过程可用作存储过程筛选，且只能在复制过程中执行。 如果指定了 FOR REPLICATION，则无法声明参数。 该选项对于 CLR 过程无效。 对于使用 FOR REPLICATION 创建的过程，忽略 RECOMPILE 选项。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 { [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }  
 构成过程主体的一个或多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 您可以使用可选的 BEGIN 和 END 关键字将这些语句括起来。 有关详细信息，请参阅 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md) 中的“最佳做法”、“一般备注”以及“限制和局限”部分。  
  
 EXTERNAL NAME _assembly\_name_ **.** _class\_name_ **.** _method\_name_  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 程序集的方法，以便 CLR 存储过程引用。 class_name 必须是有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符，并且必须作为类存在于程序集中。 如果类具有使用句点 (.) 分隔命名空间部分的命名空间限定名称，则必须使用方括号 ([]) 或引号 ("") 来分隔类名。 指定的方法必须为该类的静态方法。  
  
 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不能执行 CLR 代码。 可以创建、修改和删除引用公共语言运行时模块的数据库对象；不过，只有在启用 [clr enabled 选项](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)之后，才能在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中执行这些引用。 若要启用该选项，请使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)。  
  
> [!NOTE]  
>  包含数据库中不支持 CLR 过程。  
  
## <a name="general-remarks"></a>一般备注  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程修改为 CLR 存储过程，反之亦然。  
  
 ALTER PROCEDURE 不会更改权限，也不影响相关的存储过程或触发器。 但是，当修改存储过程时，QUOTED_IDENTIFIER 和 ANSI_NULLS 的当前会话设置包含在该存储过程中。 如果设置不同于最初创建存储过程时有效的设置，则存储过程的行为可能会更改。  
  
 如果原来的过程定义是使用 WITH ENCRYPTION 或 WITH RECOMPILE 创建的，那么只有在 ALTER PROCEDURE 中也包含这些选项时，这些选项才有效。  
  
 有关存储过程的详细信息，请参阅 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求对过程具有 ALTER 权限，或者要求 db_ddladmin 固定数据库角色中的成员身份 。  
  
## <a name="examples"></a>示例  
 以下示例将创建 `uspVendorAllInfo` 存储过程。 此过程返回提供 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 的所有供应商的名称、所提供的产品、信用等级以及可用性。 创建过程之后，便可修改过程以返回不同的结果集。  
  
```sql
IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE Purchasing.uspVendorAllInfo;  
GO  
CREATE PROCEDURE Purchasing.uspVendorAllInfo  
WITH EXECUTE AS CALLER  
AS  
    SET NOCOUNT ON;  
    SELECT v.Name AS Vendor, p.Name AS 'Product name',   
      v.CreditRating AS 'Rating',   
      v.ActiveFlag AS Availability  
    FROM Purchasing.Vendor v   
    INNER JOIN Purchasing.ProductVendor pv  
      ON v.BusinessEntityID = pv.BusinessEntityID   
    INNER JOIN Production.Product p  
      ON pv.ProductID = p.ProductID   
    ORDER BY v.Name ASC;  
GO    
```  
  
 以下示例将更改 `uspVendorAllInfo` 存储过程。 该示例将删除 EXECUTE AS CALLER 子句并且将过程的主体修改为只返回那些提供指定产品的供应商。 `LEFT` 和 `CASE` 函数自定义结果集的外观。  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER PROCEDURE Purchasing.uspVendorAllInfo  
    @Product VARCHAR(25)   
AS  
    SET NOCOUNT ON;  
    SELECT LEFT(v.Name, 25) AS Vendor, LEFT(p.Name, 25) AS 'Product name',   
    'Rating' = CASE v.CreditRating   
        WHEN 1 THEN 'Superior'  
        WHEN 2 THEN 'Excellent'  
        WHEN 3 THEN 'Above average'  
        WHEN 4 THEN 'Average'  
        WHEN 5 THEN 'Below average'  
        ELSE 'No rating'  
        END  
    , Availability = CASE v.ActiveFlag  
        WHEN 1 THEN 'Yes'  
        ELSE 'No'  
        END  
    FROM Purchasing.Vendor AS v   
    INNER JOIN Purchasing.ProductVendor AS pv  
      ON v.BusinessEntityID = pv.BusinessEntityID   
    INNER JOIN Production.Product AS p   
      ON pv.ProductID = p.ProductID   
    WHERE p.Name LIKE @Product  
    ORDER BY v.Name ASC;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```   
 Vendor               Product name  Rating    Availability  
-------------------- ------------- -------   ------------  
Proseware, Inc.      LL Crankarm   Average   No  
Vision Cycles, Inc.  LL Crankarm   Superior  Yes  
(2 row(s) affected)`  
```  

## <a name="see-also"></a>另请参阅  
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DROP PROCEDURE (Transact-SQL)](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [存储过程（数据库引擎）](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)   
 [sys.procedures (Transact-SQL)](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)  
  
  
