---
title: 查看存储过程的定义 | Microsoft Docs
description: 了解如何在对象资源管理器中查看过程的定义，以及如何在查询编辑器中使用系统存储过程、系统函数和对象目录视图来查看过程的定义。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], viewing
- definition of stored procedure
- viewing stored procedures
- displaying stored procedures
ms.assetid: 93318587-a0c5-4788-946f-3b5dc8372ea9
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8951647f20a2bf48b8cee26ed946795fbf79eda7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467028"
---
# <a name="view-the-definition-of-a-stored-procedure"></a>查看存储过程的定义
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
    
##  <a name="view-the-definition-of-a-stored-procedure"></a><a name="Top"></a> 查看存储过程的定义 

本主题介绍如何在对象资源管理器中查看过程的定义，以及如何在查询编辑器中使用系统存储过程、系统函数和对象目录视图来查看过程的定义。  
  
-   **开始之前：** [安全性](#Security)  
  
-   要查看过程的定义，请使用：[SQL Server Management Studio](#SSMSProcedure)、[Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 系统存储过程： **sp_helptext**  
 要求 **公共** 角色具有成员身份。 系统对象定义对所有用户可见。 用户对象的定义对于对象所有者或具有下列任一权限的被授权者可见：ALTER、CONTROL、TAKE OWNERSHIP 或 VIEW DEFINITION。  
  
 系统函数：**OBJECT_DEFINITION**  
 系统对象定义对所有用户可见。 用户对象的定义对于对象所有者或具有下列任一权限的被授权者可见：ALTER、CONTROL、TAKE OWNERSHIP 或 VIEW DEFINITION。 **db_owner**、 **db_ddladmin** 和 **db_securityadmin** 固定数据库角色的成员隐式具有这些权限。  
  
 sys.sql_modules 目录视图： **sys.sql_modules**  
 目录视图中仅显示用户拥有的安全对象的元数据，或用户对其拥有某些权限的安全对象的元数据。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
##  <a name="how-to-view-the-definition-of-a-stored-procedure"></a><a name="Procedures"></a> 如何查看存储过程的定义  
 您可以使用以下项之一：  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **在对象资源管理器中查看过程的定义**  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** 、过程所属的数据库以及 **“可编程性”** 。  
  
3.  展开“存储过程”，右键单击该过程，再单击“编写存储过程脚本为”，然后单击下列选项之一： “CREATE 到”、“ALTER 到”或“DROP 和 CREATE 到”。  
  
4.  选择“新建查询编辑器窗口”。 这将显示过程定义。  

###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **在查询编辑器中查看过程的定义**  
  
 系统存储过程： **sp_helptext**  
 1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在工具栏上，单击 **“新建查询”** 。  
  
3.  在查询窗口中，输入以下使用 **sp_helptext** 系统存储过程的语句。 更改数据库名称和存储过程名称以引用所需的数据库和存储过程。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    EXEC sp_helptext N'AdventureWorks2012.dbo.uspLogError';  
    ```  
  
 系统函数：**OBJECT_DEFINITION**  
 1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在工具栏上，单击 **“新建查询”** 。  
  
3.  在查询窗口中，输入以下使用 **OBJECT_DEFINITION** 系统函数的语句。 更改数据库名称和存储过程名称以引用所需的数据库和存储过程。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_DEFINITION (OBJECT_ID(N'AdventureWorks2012.dbo.uspLogError'));  
    ```  
  
 sys.sql_modules 目录视图： **sys.sql_modules**  
 1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在工具栏上，单击 **“新建查询”** 。  
  
3.  在查询窗口中，输入以下使用 **sys.sql_modules** 目录视图的语句。 更改数据库名称和存储过程名称以引用所需的数据库和存储过程。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT definition  
    FROM sys.sql_modules  
    WHERE object_id = (OBJECT_ID(N'AdventureWorks2012.dbo.uspLogError'));  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [创建存储过程](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [修改存储过程](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [删除存储过程](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [重命名存储过程](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [OBJECT_ID (Transact-SQL)](../../t-sql/functions/object-id-transact-sql.md)  
  
  
