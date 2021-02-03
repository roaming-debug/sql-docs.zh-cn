---
description: 删除唯一约束
title: 删除唯一约束 | Microsoft Docs
ms.custom: ''
ms.date: 10/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- removing constraints
- UNIQUE constraints [SQL Server], deleting
- constraints [SQL Server], deleting
- deleting constraints
- constraints [SQL Server], unique
ms.assetid: 71e563fc-f5d7-4c2e-a42f-f0695a831f32
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8cba9e925111bf7939798f80cce5d7eeb442f6d3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179754"
---
# <a name="delete-unique-constraints"></a>删除唯一约束

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  您可以使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 删除 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的唯一约束。 删除唯一约束将删除对在约束表达式所包含的列或列组合中输入的值的唯一性要求，并且会删除相应的唯一索引。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要删除唯一约束，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-delete-a-unique-constraint-using-object-explorer"></a>使用对象资源管理器删除唯一约束  
  
1.  在对象资源管理器中，展开包含唯一约束的表，再展开 **“约束”**。  
  
2.  右键单击该键，然后选择“删除”。  
  
3.  在 **“删除对象”** 对话框中，确认指定了正确的键，然后单击 **“确定”**。  
  
#### <a name="to-delete-a-unique-constraint-using-table-designer"></a>使用表设计器删除唯一约束  
  
1.  在“对象资源管理器”中，右键单击具有唯一约束的表，然后单击“设计”。  
  
2.  在“表设计器”菜单上，单击“索引/键”。  
  
3.  在“索引/键”对话框中，从“选定的主键/唯一键和索引”列表中选择唯一键。  
  
4.  单击 **“删除”** 。  
  
5.  在“文件”菜单上，单击“保存表名称” 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-delete-a-unique-constraint"></a>删除唯一约束  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Return the name of unique constraint.  
    SELECT name  
    FROM sys.objects  
    WHERE type = 'UQ' AND OBJECT_NAME(parent_object_id) = N' DocExc';  
    GO  
    -- Delete the unique constraint.  
    ALTER TABLE dbo.DocExc   
    DROP CONSTRAINT UNQ_ColumnB_DocExc;  
    GO  
    ```  
  
 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md) 和 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。  
  
###  <a name="TsqlExample"></a>  
