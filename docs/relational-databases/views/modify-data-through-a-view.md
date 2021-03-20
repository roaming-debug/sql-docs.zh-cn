---
description: 通过视图修改数据
title: 通过视图修改数据 | Microsoft Docs
ms.custom: ''
ms.date: 10/05/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- data modifications [SQL Server], views
- views [SQL Server], modifying data through
- modifying data [SQL Server], views
ms.assetid: 410e2812-4ebe-48b2-b95f-c7784f1c4336
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6707fcb9068b5d42c8fc4651a27ed00779c057e2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744917"
---
# <a name="modify-data-through-a-view"></a>通过视图修改数据
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  可使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 SQL Server 中修改基础基表的数据。  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md) 中的“可更新的视图”一节。  
  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对目标表的 UPDATE、INSERT 或 DELETE 权限（取决于执行的操作）。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-modify-table-data-through-a-view"></a>通过视图修改表数据  
  
1.  在 **“对象资源管理器”** 中，展开包含视图的数据库，然后展开 **“视图”** 。  
  
2.  右键单击该视图，然后选择“编辑前 200 行”  。  
  
3.  可能需要在 **SQL** 窗格中修改 SELECT 语句以返回要修改的行。  
  
4.  在 **“结果”** 窗格中，找到要更改或删除的行。 若要删除行，请右键单击该行，然后选择“删除”  。 若要更改一个或多个列中的数据，请修改列中的数据。  
  
    > **重要说明!!** 如果视图引用多个基表，则不能删除行。 只能更新属于单个基表的列。  
  
5.  若要插入行，请向下滚动到行的结尾并插入新值。  

    > **重要说明！** 如果视图引用多个基表，则不能插入行。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-update-table-data-through-a-view"></a>通过视图更新表数据  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 此示例通过引用视图 `StartDate` 中的列为特定雇员更改 `EndDate` 和 `HumanResources.vEmployeeDepartmentHistory`列中的值。 此视图从两个表返回值。 此语句会成功，因为修改的列都来自一个基表。  
  
    ```  
    USE AdventureWorks2012 ;   
    GO  
    UPDATE HumanResources.vEmployeeDepartmentHistory  
    SET StartDate = '20110203', EndDate = GETDATE()   
    WHERE LastName = N'Smith' AND FirstName = 'Samantha';   
    GO  
    ```  
  
 有关详细信息，请参阅 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)。  
  
#### <a name="to-insert-table-data-through-a-view"></a>通过视图插入表数据  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 此示例通过指定视图 `HumanResouces.Department` 中的相关列，将一个新行插入到基表 `HumanResources.vEmployeeDepartmentHistory`。 该语句会成功，因为只指定了一个基表中的列，基表中的其他列具有默认值。  
  
    ```  
    USE AdventureWorks2012 ;  
    GO  
    INSERT INTO HumanResources.vEmployeeDepartmentHistory (Department, GroupName)   
    VALUES ('MyDepartment', 'MyGroup');   
    GO  
    ```  
  
 有关详细信息，请参阅 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)。  
  
  
