---
title: 查看计划指南属性 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中查看计划指南的属性。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.planguideprop.general.f1
helpviewer_keywords:
- plan guides [SQL Server], view plan guide properties
- viewing plan guide properties
ms.assetid: 8c0d2f39-59c1-4168-a649-65473f6a771b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 72b34306250bc24018c719f10700a29f4b7d6d0c
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766483"
---
# <a name="view-plan-guide-properties"></a>查看计划指南属性
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  可以使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **查看计划指南的属性，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 目录视图中仅显示用户拥有的安全对象的元数据，或用户对其拥有某些权限的安全对象的元数据。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-the-properties-of-a-plan-guide"></a>查看计划指南的属性  
  
1.  单击加号以便展开您要在其中查看计划指南属性的数据库，然后单击加号以便展开 **“可编程性”** 文件夹。  
  
2.  单击加号以便展开 **“计划指南”** 文件夹。  
  
3.  右键单击要查看其属性的计划指南，然后选择“属性”。  
  
     **“计划指南属性”** 对话框显示以下属性。  
  
     **提示**  
     显示要应用于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的查询提示或查询计划。 将查询计划指定为提示时，会显示该计划的 XML 显示计划输出。  
  
     **已禁用**  
     显示计划指南的状态。 可能的值包括 **True** 和 **False**。  
  
     **名称**  
     显示计划指南的名称。  
  
     **参数**  
     当作用域类型为 SQL 或 TEMPLATE 时，显示嵌入在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中的所有参数的名称和数据类型。  
  
     **作用域批处理**  
     显示其中出现 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的批处理文本。  
  
     **作用域对象名称**  
     当作用域类型为 OBJECT 时，显示其中出现 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程、用户定义标量函数、多语句表值函数或 DML 触发器的名称。  
  
     **作用域架构名称**  
     当作用域类型为 OBJECT 时，显示包含此对象的架构的名称。  
  
     **作用域类型**  
     显示其中出现 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的实体的类型。 这便指定了用于将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句与计划指南匹配的上下文。 可能的值为 **OBJECT**、 **SQL** 和 **TEMPLATE**。  
  
     **Statement**  
     显示应用此计划指南的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
4.  单击“确定”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-view-the-properties-of-a-plan-guide"></a>查看计划指南的属性  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- If a plan guide named "Guide1" already exists in the AdventureWorks2012 database, delete it.  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID(N'Guide1') IS NOT NULL  
       EXEC sp_control_plan_guide N'DROP', N'Guide1';  
    GO  
    -- creates a plan guide named Guide1 based on a SQL statement  
    EXEC sp_create_plan_guide   
        @name = N'Guide1',   
        @stmt = N'SELECT TOP 1 *   
                  FROM Sales.SalesOrderHeader   
                  ORDER BY OrderDate DESC',   
        @type = N'SQL',  
        @module_or_batch = NULL,   
        @params = NULL,   
        @hints = N'OPTION (MAXDOP 1)';  
    GO  
    -- Gets the name, created date, and all other relevant property information on the plan guide created above.   
    SELECT name AS plan_guide_name,  
       create_date,  
       query_text,  
       scope_type_desc,  
       OBJECT_NAME(scope_object_id) AS scope_object_name,  
       scope_batch,  
       parameters,  
       hints,  
       is_disabled  
    FROM sys.plan_guides  
    WHERE name = N'Guide1';  
    GO  
    ```  
  
 有关详细信息，请参阅 [sys.plan_guides (Transact-SQL)](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)。  
  
  
