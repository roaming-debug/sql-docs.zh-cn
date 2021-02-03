---
description: 删除视图
title: 删除视图 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- dropping views
- deleting views
- views [SQL Server], deleting
- removing views
ms.assetid: 6823c7f8-06ca-4bda-8482-7092f03d52a0
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7ee87077178a9d52d7af83f891a3f9d81f949c20
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212298"
---
# <a name="delete-views"></a>删除视图
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  可以使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或，在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中删除视图 [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **从数据库中删除视图，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   删除视图时，将从系统目录中删除视图的定义和有关视图的其他信息。 还将删除视图的所有权限。  
  
-   使用 `DROP TABLE` 删除的表上的任何视图都必须使用 `DROP VIEW`显式删除。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对 SCHEMA 的 ALTER 权限或对 OBJECT 的 CONTROL 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-delete-a-view-from-a-database"></a>从数据库中删除视图  
  
1.  在 **“对象资源管理器”** 中，展开包含要删除的视图的数据库，然后展开 **“视图”** 文件夹。  
  
2.  右键单击要删除的视图，然后单击“删除”。  
  
3.  在 **“删除对象”** 对话框中，单击 **“确定”** 。  
  
    > [!IMPORTANT]  
    >  单击“删除对象”对话框中的“显示依赖关系”，打开“_view\_name_**依赖关系**”对话框。 这将显示依赖于该视图的所有对象和该视图依赖的所有对象。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-delete-a-view-from-a-database"></a>从数据库中删除视图  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 仅在视图存在时，该示例才删除指定的视图。  
  
    ```  
    USE AdventureWorks2012 ;  
    GO  
    IF OBJECT_ID ('HumanResources.EmployeeHireDate', 'V') IS NOT NULL  
    DROP VIEW HumanResources.EmployeeHireDate;  
    GO  
    ```  
  
 有关详细信息，请参阅 [DROP VIEW (Transact-SQL)](../../t-sql/statements/drop-view-transact-sql.md)。  
  
  
