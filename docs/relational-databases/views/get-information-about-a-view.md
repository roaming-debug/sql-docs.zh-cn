---
description: 获取有关视图的信息
title: 获取有关视图的信息 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.viewproperties.general.f1
helpviewer_keywords:
- views [SQL Server], status information
- metadata [SQL Server], views
- dependencies [SQL Server], views
- displaying view information
- views [SQL Server], metadata
- viewing view information
- status information [SQL Server], views
- view dependencies
ms.assetid: 05a73e33-8f85-4fb6-80c1-1b659e753403
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9af0931fd8f45bf226a3ed3bcd619c329ed651ef
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484419"
---
# <a name="get-information-about-a-view"></a>获取有关视图的信息
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中，通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 可以获取有关视图的定义或属性的信息。 您可能需要查看视图定义以了解数据从源表中的提取方式，或查看视图所定义的数据。  
  
> [!IMPORTANT]  
>  如果更改视图所引用对象的名称，则必须更改视图，使其文本反映新的名称。 因此，在重命名对象之前，首先显示该对象的依赖关系，以确定即将发生的更改是否会影响任何视图。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **获取有关视图的信息，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 使用 `sp_helptext` 返回视图的定义需要 **public** 角色的成员身份。 使用 `sys.sql_expression_dependencies` 查找视图的所有依赖关系需要对该数据库的 VIEW DEFINITION 权限以及对数据库的 `sys.sql_expression_dependencies` 的 SELECT 权限。 系统对象定义（如 SELECT OBJECT_DEFINITION 中返回的对象定义）是公开可见的。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="get-view-properties-by-using-object-explorer"></a>使用对象资源管理器获取视图属性  
  
1.  在 **“对象资源管理器”** 中，单击包含要查看属性的视图的数据库旁边的加号，然后单击加号以展开 **“视图”** 文件夹。  
  
2.  右键单击要查看其属性的视图，然后选择“属性”。  

     **“视图属性”** 对话框中显示以下属性：  
  
     **Database**  
     包含此视图的数据库的名称。  
  
     **Server**  
     当前服务器实例的名称。  
  
     **用户**  
     此连接的用户名。  
  
     **创建日期**  
     显示视图的创建日期。  
  
     **名称**  
     当前视图的名称。  
  
     **架构**  
     显示视图所属的架构。  
  
     **系统对象**  
     指示视图是否为系统对象。 值为 True 和 False。  
  
     **ANSI NULLs**  
     指示创建对象时是否选择了 ANSI NULLs 选项。  
  
     **已加密**  
     指示视图是否已加密。 值为 True 和 False。  
  
     **带引号的标识符**  
     指示创建对象时是否选择了“带引号的标识符”选项。  
  
     **架构已绑定**  
     指示视图是否绑定到架构。 值为 True 和 False。 有关绑定到架构的视图的信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md) 的 SCHEMABINDING 部分。  
  
#### <a name="getting-view-properties-by-using-the-view-designer-tool"></a>使用视图设计器工具获取视图属性  
  
1.  在 **“对象资源管理器”** 中，展开包含要查看属性的视图的数据库，然后展开 **“视图”** 文件夹。  
  
2.  右键单击要查看其属性的视图，然后选择“设计”。  
  
3.  右键单击“关系图”窗格中的空白区域，再单击“属性”。  
  
     **“属性”** 窗格中显示以下属性：  
  
     **(名称)**  
     当前视图的名称。  
  
     **Database Name**  
     包含此视图的数据库的名称。  
  
     **说明**  
     对当前视图的简短说明。  
  
     **架构**  
     显示视图所属的架构。  
  
     **服务器名称**  
     当前服务器实例的名称。  
  
     **绑定到架构**  
     防止用户以会使视图定义失效的任何方式修改影响此视图的基础对象。  
  
     **具有确定性**  
     显示是否可以明确地确定所选列的数据类型。  
  
     **非重复值**  
     指定查询将在视图中筛选出重复值。 当只使用表中的部分列并且这些列可能包含重复值时，或者当联接两个或更多表的过程会在结果集中产生重复行时，此选项非常有用。 选择该选项等效于向 SQL 窗格内的语句中插入关键字 DISTINCT。  
  
     **GROUP BY 扩展**  
     指定对于基于聚合查询的视图，附加选项可用。  
  
     **输出所有列**  
     显示所有列是否都由所选视图返回。 这是在创建视图时设置的。  
  
     **SQL 注释**  
     显示 SQL 语句的说明。 若要查看或编辑完整的说明，请单击相应的说明，再单击属性右侧的省略号 (…)。 您的注释可以包含视图使用者和使用时间等信息。  
  
     **Top 规范**  
     展开此项可显示“Top”、“表达式”、“百分比”和“等同值”属性的属性。  
  
     **(最前面)**  
     指定视图将包括 TOP 子句，该子句只返回结果集中的前 n 行或前百分之 n 行。 默认情况下，视图将在结果集中返回前 10 行。 使用此项可更改返回的行数或指定不同的百分比  
  
     **表达式**  
     显示视图将返回的百分比（如果“百分比”设置为“是”）或记录（如果“百分比”设置为“否”）。  
  
     **百分比**  
     指定查询将包含一个 **TOP** 子句，仅返回结果集中前百分之 n 行  
  
     **With Ties**  
     指定视图将包括 **WITH TIES** 子句。 如果视图包含 **WITH TIES** 子句和基于百分比的 **WITH TIES** 子句， **WITH TIES** 将非常有用。 如果设置了该选项，并且百分比截止位置在一组行的中间，且这些行在 **ORDER BY** 子句中具有相同的值，则视图将会扩展，以包含所有这样的行。  
  
     **更新规范**  
     展开此项可显示“使用视图规则更新”和“Check 选项”属性。  
  
     **(使用视图规则更新)**  
     指示对视图的所有更新和插入将由 Microsoft 数据访问组件 (MDAC) 转换为引用视图的 SQL 语句，而非转换为直接引用视图的基表的 SQL 语句。  
  
     在某些情况下，MDAC 将视图更新和视图插入操作表示为针对视图的基本基表的更新和插入。 通过选择 **“使用视图规则更新”**，您可以确保 MDAC 针对视图本身生成更新和插入操作。  
  
     **Check 选项**  
     指示当打开此视图并修改“结果”窗格时，数据源检查添加或修改的数据是否满足视图定义的 **WHERE** 子句的要求。 如果您的修改不满足 **WHERE** 子句的要求，将看到一个包含详细信息的错误。  
  
#### <a name="to-get-dependencies-on-the-view"></a>获取视图的依赖关系  
  
1.  在 **“对象资源管理器”** 中，展开包含要查看属性的视图的数据库，然后展开 **“视图”** 文件夹。  
  
2.  右键单击要查看其属性的视图，然后选择“查看依赖关系”。  
  
3.  选择“依赖于 [视图名称] 的对象”可以显示引用该视图的对象。  
  
4.  选择“[视图名称] 依赖的对象”可以显示由该视图引用的对象。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-get-the-definition-and-properties-of-a-view"></a>获取视图的定义和属性  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例之一复制并粘贴到查询窗口中，然后单击 **“执行”** 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT definition, uses_ansi_nulls, uses_quoted_identifier, is_schema_bound  
    FROM sys.sql_modules  
    WHERE object_id = OBJECT_ID('HumanResources.vEmployee');   
    GO  
    ```  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT OBJECT_DEFINITION (OBJECT_ID('HumanResources.vEmployee')) AS ObjectDefinition;   
    GO  
    ```  
  
    ```  
    EXEC sp_helptext 'HumanResources.vEmployee';  
    ```  
  
 有关详细信息，请参阅 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)、[OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md) 和 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)。  
  
#### <a name="to-get-the-dependencies-of-a-view"></a>获取视图的依赖关系  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
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
    WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');  
    GO  
    ```  
  
 有关详细信息，请参阅 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) 和 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。  
  
  
