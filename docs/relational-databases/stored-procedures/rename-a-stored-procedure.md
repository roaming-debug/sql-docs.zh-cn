---
title: 重命名存储过程 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 2019 (15.x) 中重命名存储过程。
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], renaming
- renaming stored procedures
ms.assetid: 5d2e4c68-7e0b-4405-8919-f5b203e46770
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 433fa034c85418919aa48886c3a7c06757de890f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347676"
---
# <a name="rename-a-stored-procedure"></a>重命名存储过程

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

本主题介绍如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中重命名存储过程。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要重命名存储过程，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   过程名称必须符合 [标识符](../../relational-databases/databases/database-identifiers.md)规则。  
  
-   重命名存储过程会保留 `object_id` 以及专门分配给该过程的所有权限。 删除并重新创建对象将创建一个新的 `object_id`，并删除专门分配给该过程的所有权限。

-   重命名存储过程将不会更改 sys.sql_modules 目录视图的定义列中相应对象名的名称。 要执行该操作，必须删除存储过程，然后使用新名称重新创建该存储过程。  

-   在未将对象更新为反映已对过程所做的更改时，更改过程的名称或定义可能导致依赖对象失败。 有关详细信息，请参阅 [查看存储过程的依赖关系](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 CREATE PROCEDURE  
 要求数据库中的 CREATE PROCEDURE 权限以及对要在其中创建过程的架构的 ALTER 权限，或者要求 **db_ddladmin** 固定数据库角色中的成员身份。  
  
 ALTER PROCEDURE  
 要求对过程具有 ALTER 权限，或者要求 **db_ddladmin** 固定数据库角色中的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-rename-a-stored-procedure"></a>重命名存储过程  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
2.  展开 **“数据库”** 、过程所属的数据库以及 **“可编程性”** 。  
3.  [确定存储过程的依赖关系](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)。  
4.  展开“存储过程”，右键单击要重命名的过程，再单击“重命名”。  
5.  修改过程名称。  
6.  修改在任何相关对象或脚本中引用的过程名称。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-rename-a-stored-procedure"></a>重命名存储过程  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何通过删除过程并使用新名称重新创建该过程来重命名过程。 第一个示例将创建 `'HumanResources.uspGetAllEmployeesTest`存储过程。 第二个示例将存储过程重命名为 `HumanResources.uspEveryEmployeeTest`。  
  
```sql  
--Create the stored procedure.  
USE AdventureWorks2012;  
GO  

CREATE PROCEDURE HumanResources.uspGetAllEmployeesTest  
AS  
    SET NOCOUNT ON;  
    SELECT LastName, FirstName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory;  
GO  
  
--Rename the stored procedure.  
EXEC sp_rename 'HumanResources.uspGetAllEmployeesTest', 'uspEveryEmployeeTest'; 
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER PROCEDURE (Transact-SQL)](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [创建存储过程](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [修改存储过程](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [删除存储过程](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [查看存储过程的定义](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [查看存储过程的依赖关系](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)  
  
  
