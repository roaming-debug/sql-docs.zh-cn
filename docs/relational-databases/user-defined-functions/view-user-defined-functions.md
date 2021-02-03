---
description: 查看用户定义函数
title: 查看用户定义函数 | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.udfproperties.general.f1
- sql13.swb.functionproperties.general.f1
helpviewer_keywords:
- displaying user-defined functions
- viewing user-defined functions
- user-defined functions [SQL Server], viewing
- status information [SQL Server], user-defined functions
ms.assetid: a45dfab5-6384-4311-b935-2e23a70c5c10
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ae8eb805b886edbe9c470ee3b10d01679b2f2362
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210721"
---
# <a name="view-user-defined-functions"></a>查看用户定义函数
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中，通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)]可以获取有关用户定义函数的定义或属性的信息。 您可能需要查看函数的定义，以理解其数据从源表中派生的方式或查看函数所定义的数据。  
  
> [!IMPORTANT]  
>  如果您更改函数所引用的对象的名称，则必须修改函数，使其文本反映新名称。 因此，在重命名对象前，首先显示该对象的依赖关系，以确定所建议的更改是否影响任何函数。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要获取有关函数的信息，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 使用 **sys.sql_expression_dependencies** 查找函数的依赖关系要求对该数据库具有 VIEW DEFINITION 权限，以及对数据库具有 **sys.sql_expression_dependencies** 的 SELECT 权限。 系统对象定义（如 OBJECT_DEFINITION 中返回的对象定义）是公开可见的。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-show-a-user-defined-functions-properties"></a>显示用户定义函数的属性  
  
1.  在 **“对象资源管理器”** 中，单击包含要查看属性的函数的数据库旁边的加号，然后单击加号以展开 **“可编程性”** 文件夹。  
  
2.  单击加号以便展开 **“函数”** 文件夹。  
  
3.  单击加号以展开包含您要查看属性的函数的文件夹：  
  
    -   Table-valued Function  
  
    -   标量值函数  
  
    -   Aggregate 函数  
  
4.  右键单击要查看其属性的函数，然后选择“属性”。  

     “函数属性 - function_name”对话框中将显示以下属性 。  
  
     **Database**  
     包含此函数的数据库的名称。  
  
     **Server**  
     当前服务器实例的名称。  
  
     **用户**  
     此连接的用户名。  
  
     **创建日期**  
     显示函数的创建日期。  
  
     **执行身份**  
     执行该函数的上下文。  
  
     **名称**  
     当前函数的名称。  
  
     **架构**  
     显示函数所属的架构。  
  
     **系统对象**  
     指示该函数是否为系统对象。 值为 True 和 False。  
  
     **ANSI NULLs**  
     指示创建对象时是否选择了 ANSI NULLs 选项。  
  
     **已加密**  
     指示该函数是否已加密。 值为 True 和 False。  
  
     **函数类型**  
     用户定义函数的类型。  
  
     **带引号的标识符**  
     指示创建对象时是否选择了“带引号的标识符”选项。  
  
     **架构已绑定**  
     指示该函数是否已绑定到架构。 值为 True 和 False。 有关绑定到架构的函数的信息，请参阅 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md) 的 SCHEMABINDING 部分。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-get-the-definition-and-properties-of-a-function"></a>获取函数的定义和属性  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例之一复制并粘贴到查询窗口中，然后单击 **“执行”** 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get the function name, definition, and relevant properties  
    SELECT sm.object_id,   
       OBJECT_NAME(sm.object_id) AS object_name,   
       o.type,   
       o.type_desc,   
       sm.definition,  
       sm.uses_ansi_nulls,  
       sm.uses_quoted_identifier,  
       sm.is_schema_bound,  
       sm.execute_as_principal_id  
    -- using the two system tables sys.sql_modules and sys.objects  
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id  
    -- from the function 'dbo.ufnGetProductDealerPrice'  
    WHERE sm.object_id = OBJECT_ID('dbo.ufnGetProductDealerPrice')  
    ORDER BY o.type;  
    GO  
  
    ```  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get the definition of the function dbo.ufnGetProductDealerPrice  
    SELECT OBJECT_DEFINITION (OBJECT_ID('dbo.ufnGetProductDealerPrice')) AS ObjectDefinition;  
    GO  
    ```  
  
 有关详细信息，请参阅 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) 和 [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)。  
  
#### <a name="to-get-the-dependencies-of-a-function"></a>获取函数的依赖关系  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Get all of the dependency information  
    SELECT OBJECT_NAME(sed.referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(sed.referencing_id, sed.referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        sed.referencing_class_desc, sed.referenced_class_desc,  
        sed.referenced_server_name, sed.referenced_database_name, sed.referenced_schema_name,  
        sed.referenced_entity_name,   
        COALESCE(COL_NAME(sed.referenced_id, sed.referenced_minor_id), '(n/a)') AS referenced_column_name,  
        sed.is_caller_dependent, sed.is_ambiguous  
    -- from the two system tables sys.sql_expression_dependencies and sys.object  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    -- on the function dbo.ufnGetProductDealerPrice  
    WHERE sed.referencing_id = OBJECT_ID('dbo.ufnGetProductDealerPrice');  
    GO  
    ```  
  
 有关详细信息，请参阅 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) 和 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。  
  
  
