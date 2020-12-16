---
title: 查看存储过程的依赖关系 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 2019 (15.x) 中查看存储过程依赖项。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], dependencies
- displaying stored procedure dependencies
- viewing stored procedure dependencies
ms.assetid: 6ae0a369-1bc7-4ae4-be89-2b483697cd1f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2d0a64ac9fd8ed3f9ac09f269a4026ed797b5af2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466978"
---
# <a name="view-the-dependencies-of-a-stored-procedure"></a>查看存储过程的依赖关系
[!INCLUDE[appliesto-ss-asdb-xxxx-pdw-md](../../includes/appliesto-ss-asdb-xxxx-pdw-md.md)]
  本主题介绍了如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中查看存储过程依赖关系。  
  
##  <a name="Top"></a>   
-   **开始之前：** [限制和局限](#Restrictions)、[安全性](#Security)  
  
-   要查看过程的依赖关系，请使用：[SQL Server Management Studio](#SSMSProcedure)、[Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 系统函数： **sys.dm_sql_referencing_entities**  
 要求对被引用的实体拥有 CONTROL 权限，并且对 sys.dm_sql_referencing_entities 拥有 SELECT 权限。 当被引用的实体是分区函数时，要求对数据库拥有 CONTROL 权限。 默认情况下，SELECT 权限授予 public。  
  
 系统函数： **sys.dm_sql_referenced_entities**  
 要求对 sys.dm_sql_referenced_entities 拥有 SELECT 权限并对引用实体拥有 VIEW DEFINITION 权限。 默认情况下，SELECT 权限授予 public。 要求对数据库拥有 VIEW DEFINITION 权限或 ALTER DATABASE DDL TRIGGER 权限（当引用实体为数据库级 DDL 触发器时）。 当引用实体为服务器级 DDL 触发器时，要求对服务器拥有 VIEW ANY DEFINITION 权限。  
  
 对象目录视图： **sys.sql_expression_dependencies**  
 要求对数据库具有 VIEW DEFINITION 权限，并对数据库的 sys.sql_expression_dependencies 具有 SELECT 权限。 默认情况下，SELECT 权限仅授予 db_owner 固定数据库角色的成员。 将 SELECT 和 VIEW DEFINITION 权限授予其他用户时，被授权者可以查看数据库中的所有依赖关系。  
  
##  <a name="how-to-view-the-dependencies-of-a-stored-procedure"></a><a name="Procedures"></a> 如何查看存储过程的依赖关系  
 您可以使用以下项之一：  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **在对象资源管理器中查看过程的依赖关系**  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** 、过程所属的数据库以及 **“可编程性”** 。  
  
3.  展开“存储过程”，右键单击此过程，再单击“查看依赖关系”。  
  
4.  查看依赖于过程的对象的列表。  
  
5.  查看过程所依赖的对象的列表。  
  
6.  单击“确定”。  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **在查询编辑器中查看过程的依赖关系**  
  
 系统函数： **sys.dm_sql_referencing_entities**  
 此函数用于显示依赖于过程的对象。  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** ，然后展开过程所属的数据库。  
  
3.  在 **“文件”** 菜单下，单击 **“新建查询”** 。  
  
4.  复制以下示例并将其粘贴到查询编辑器中。 第一个示例创建 `uspVendorAllInfo` 过程，该过程返回 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 数据库中所有供应商的名称、所提供的产品、信用等级以及可用性。  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
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
  
5.  创建该过程后，第二个示例使用 sys.dm_sql_referencing_entities 函数来显示依赖于该过程的对象。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
    FROM sys.dm_sql_referencing_entities ('Purchasing.uspVendorAllInfo', 'OBJECT');   
    GO  
  
    ```  
  
 系统函数： **sys.dm_sql_referenced_entities**  
 此函数用于显示过程所依赖的对象。  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** ，然后展开过程所属的数据库。  
  
3.  在 **“文件”** 菜单下，单击 **“新建查询”** 。  
  
4.  复制以下示例并将其粘贴到查询编辑器中。 第一个示例创建 `uspVendorAllInfo` 过程，该过程返回 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 数据库中所有供应商的名称、所提供的产品、信用等级以及可用性。  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
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
  
5.  创建该过程后，第二个示例使用 sys.dm_sql_referenced_entities 函数来显示该过程依赖的对象。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT referenced_schema_name, referenced_entity_name,  
    referenced_minor_name,referenced_minor_id, referenced_class_desc,  
    is_caller_dependent, is_ambiguous  
    FROM sys.dm_sql_referenced_entities ('Purchasing.uspVendorAllInfo', 'OBJECT');  
    GO  
    ```  
  
 对象目录视图： **sys.sql_expression_dependencies**  
 此视图可以用于显示过程所依赖的对象或依赖于过程的对象。  
  
 显示依赖于过程的对象。  
 1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** ，然后展开过程所属的数据库。  
  
3.  在 **“文件”** 菜单下，单击 **“新建查询”** 。  
  
4.  复制以下示例并将其粘贴到查询编辑器中。 第一个示例创建 `uspVendorAllInfo` 过程，该过程返回 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 数据库中所有供应商的名称、所提供的产品、信用等级以及可用性。  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
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
  
5.  创建该过程后，第二个示例使用 sys.sql_expression_dependencies 视图来显示依赖于该过程的对象。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_SCHEMA_NAME ( referencing_id ) AS referencing_schema_name,  
        OBJECT_NAME(referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        referencing_class_desc, referenced_class_desc,  
        referenced_server_name, referenced_database_name, referenced_schema_name,  
        referenced_entity_name,   
        COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
        is_caller_dependent, is_ambiguous  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    WHERE referenced_id = OBJECT_ID(N'Purchasing.uspVendorAllInfo')  
    GO  
    ```  
  
 显示过程所依赖的对象。  
 1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** ，然后展开过程所属的数据库。  
  
3.  在 **“文件”** 菜单下，单击 **“新建查询”** 。  
  
4.  复制以下示例并将其粘贴到查询编辑器中。 第一个示例创建 `uspVendorAllInfo` 过程，该过程返回 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 数据库中所有供应商的名称、所提供的产品、信用等级以及可用性。  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
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
  
5.  创建该过程后，第二个示例使用 sys.sql_expression_dependencies 视图来显示该过程依赖的对象。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        referencing_class_desc, referenced_class_desc,  
        referenced_server_name, referenced_database_name, referenced_schema_name,  
        referenced_entity_name,   
        COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
        is_caller_dependent, is_ambiguous  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    WHERE referencing_id = OBJECT_ID(N'Purchasing.uspVendorAllInfo');  
    GO  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [重命名存储过程](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  
