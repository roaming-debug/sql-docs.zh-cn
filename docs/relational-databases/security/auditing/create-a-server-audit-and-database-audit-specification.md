---
title: 创建服务器审核和数据库审核规范
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL (T-SQL) 创建 SQL Server 审核和数据库审核规范。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlaudit.dbaudit.general.f1
helpviewer_keywords:
- audits [SQL Server], creating database specification
- database audit [SQL Server]
ms.assetid: 26ee85de-6e97-4318-b526-900924d96e62
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d015a1adf0bdd5e5f47b49a8dabb6783b8538b7e
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250076"
---
# <a name="create-a-server-audit-and-database-audit-specification"></a>创建服务器审核和数据库审核规范
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 在 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 中创建服务器审核和数据库审核规范。  
  
 审核 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库涉及到跟踪和记录系统中发生的事件。 SQL Server Audit  对象收集单个服务器级实例或数据库级操作和操作组以进行监视。 这种审核处于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例级别。 每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例可以具有多个审核。 *数据库级别审核规范* 对象属于审核。 针对每个审核，您可以为每个 SQL Server 数据库创建一个数据库审核规范。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
 ##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 准备工作  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 数据库审核规范是驻留在给定数据库中的非安全对象。 数据库审核规范在创建之后处于禁用状态。  
  
 当您在用户数据库中创建或修改数据库审核规范时，不要包括针对服务器范围对象（例如系统视图）的审核操作。 如果包括服务器范围对象，将会创建审核。 但是，服务器范围对象将不包括在内，并且将不返回任何错误。 若要审核服务器范围对象，请使用 master 数据库中的数据库审核规范。  
  
 数据库审核规范位于创建它们的数据库（ TempDB  系统数据库除外）中。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
-   具有 ALTER ANY DATABASE AUDIT 权限的用户可以创建数据库审核规范并将其绑定到任何审核。  
  
-   创建数据库审核规范后，具有 CONTROL SERVER 或 ALTER ANY DATABASE AUDIT 权限的主体即可查看该规范。 sysadmin 帐户也可以查看它。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-server-audit"></a>创建服务器审核  
  
1.   在对象资源管理器中，展开“安全性”文件夹。  
  
2.  右键单击“审核”文件夹，然后选择“新建审核”   。 有关详细信息，请参阅[创建服务器审核和服务器审核规范](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)。  
  
3.  选择选项完成后，选择“确定”。   

#### <a name="to-create-a-database-level-audit-specification"></a>创建数据库级别审核规范  
  
1.  在“对象资源管理器”中，展开要创建审核规范的数据库。  
  
2.  展开 **“安全性”** 文件夹。  
  
3.  右键单击“数据库审核规范”文件夹，然后选择“新建数据库审核规范”   。  
  
     “创建数据库审核规范”  对话框中提供了以下选项：  
  
     **名称**  
     数据库审核规范的名称。 创建服务器审核规范时会自动生成名称。 该字段是可编辑的。  
  
     **审核**  
     现有服务器审核对象的名称。 或者键入审核的名称，或者从列表中选择一个名称。  
  
     **审核操作类型**  
     指定要捕获的数据库级别审核操作组和审核操作。 有关数据库级别审核操作组和审核操作的列表以及它们所包含事件的说明，请参阅 [SQL Server 审核操作组和操作](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)。  
  
     **对象架构**  
     显示具有指定“对象名称”  的对象的架构。  
  
     **Object Name**  
     要审核的对象的名称。 此选项仅适用于审核操作。 它不适用于审核组。  
  
     **省略号 (...)**  
     打开“选择对象”  对话框，以便可以基于指定的“审核操作类型”  浏览和选择可用对象。  
  
     **主体名称**  
     对于所审核的对象，要作为审核筛选依据的帐户。  
  
     **省略号 (...)**  
     打开“选择对象”  对话框，以便可以基于指定的“对象名称”  浏览和选择可用对象。  
  
4.  选择选项完成后，选择“确定”。   
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-server-audit"></a>创建服务器审核  
  
1.  在“对象资源管理器”中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准栏上，选择“新建查询”  。  
  
3.  将以下示例粘贴到查询窗口中，然后选择“执行”  。  
  
    ```  
    USE master ;  
    GO  
    -- Create the server audit.   
    CREATE SERVER AUDIT Payrole_Security_Audit  
        TO FILE ( FILEPATH =   
    'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ;   
    GO  
    -- Enable the server audit.   
    ALTER SERVER AUDIT Payrole_Security_Audit   
    WITH (STATE = ON) ;  
    ```  
  
#### <a name="to-create-a-database-level-audit-specification"></a>创建数据库级别审核规范  
  
1.  在“对象资源管理器”中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准栏上，选择“新建查询”  。  
  
3.  将以下示例粘贴到查询窗口中，然后选择“执行”  。 此示例创建一个称为 `Audit_Pay_Tables` 的数据库审核规范。 它针对 `HumanResources.EmployeePayHistory` 表，基于上一个部分中定义的服务器审核对 `dbo` 用户发出的 SELECT 和 INSERT 语句进行审核。  
  
    ```  
    USE AdventureWorks2012 ;   
    GO  
    -- Create the database audit specification.   
    CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
    FOR SERVER AUDIT Payrole_Security_Audit  
    ADD (SELECT , INSERT  
         ON HumanResources.EmployeePayHistory BY dbo )   
    WITH (STATE = ON) ;   
    GO  
  
    ```  
  
 有关详细信息，请参阅 [CREATE SERVER AUDIT (Transact-SQL)](../../../t-sql/statements/create-server-audit-transact-sql.md) 和 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../../t-sql/statements/create-database-audit-specification-transact-sql.md)。  
  
  
