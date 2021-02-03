---
description: 创建视图
title: 创建视图 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- views [SQL Server], creating
ms.assetid: 0b7bd2a1-544c-42ba-8e7b-4822f34d7b64
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d7c7e339afa580f81a834021670cc81fc61ab8ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161911"
---
# <a name="create-views"></a>创建视图
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  您可以使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建视图。 可以将视图用于以下用途：  
  
-   集中、简化和自定义每个用户对数据库的认识。  
  
-   用作安全机制，方法是允许用户通过视图访问数据，而不授予用户直接访问底层基表的权限。  
  
-   提供向后兼容接口来模拟架构已更改的表。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **创建视图，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 只能在当前数据库中创建视图。  
  
 视图最多可以包含 1,024 列。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求在数据库中具有 CREATE VIEW 权限，并具有在其中创建视图的架构的 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-view-by-using-the-query-and-view-designer"></a>使用查询和视图设计器创建视图  
  
1.  在 **“对象资源管理器”** 中，展开要创建新视图的数据库。  
  
2.  右键单击“视图”文件夹，然后单击“新建视图...” 。  
  
3.  在 **“添加表”** 对话框中，从以下选项卡之一选择要在新视图中包含的元素：“表”、“视图”、“函数”和“同义词”。  
  
4.  单击 **“添加”**，再单击 **“关闭”**。  
  
5.  在 **“关系图窗格”** 中，选择要在新视图中包含的列或其他元素。  
  
6.  在 **“条件窗格”** 中，选择列的其他排序或筛选条件。  
  
7.  在“文件”菜单上，单击“保存”以保存 _视图名称_。  
  
8.  在 **“选择名称”** 对话框中，输入新视图的名称并单击 **“确定”**。  

     有关查询和视图设计器的详细信息，请参阅[查询和视图设计器工具（可视化数据库工具）](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-view"></a>创建视图  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE AdventureWorks2012 ;   
    GO  
    CREATE VIEW HumanResources.EmployeeHireDate  
    AS  
    SELECT p.FirstName, p.LastName, e.HireDate  
    FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
    ON e.BusinessEntityID = p.BusinessEntityID ;   
    GO  
    -- Query the view  
    SELECT FirstName, LastName, HireDate  
    FROM HumanResources.EmployeeHireDate  
    ORDER BY LastName;  
  
    ```  
  
 有关详细信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)。  
  
