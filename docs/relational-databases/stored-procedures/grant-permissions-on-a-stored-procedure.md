---
title: 授予对存储过程的权限 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 2019 (15.x) 中授予针对存储过程的权限。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], permissions
ms.assetid: a7d15816-a788-4099-ad91-dc4b26618299
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b41f60b80dcb13d9e61b854666e3298b2b295cf
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250270"
---
# <a name="grant-permissions-on-a-stored-procedure"></a>授予对存储过程的权限
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  本主题说明如何在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)]授予对存储过程的权限。 可以为数据库中的现有用户、数据库角色或应用程序角色授予权限。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要授予对存储过程的权限，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   不能使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 授予对系统过程或系统函数的权限。 改为使用 [GRANT 对象权限](../../t-sql/statements/grant-object-permissions-transact-sql.md) 。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 授权者（或用 AS 选项指定的主体）必须具有带 GRANT OPTION 的相同权限，或具有隐含所授予权限的更高权限。 需要拥有对该过程所属架构的 ALTER 权限，或对该过程的 CONTROL 权限。 有关详细信息，请参阅 [GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)授予对存储过程的权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-grant-permissions-on-a-stored-procedure"></a>授予对存储过程的权限  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** 、过程所属的数据库以及 **“可编程性”** 。  
  
3.  展开“存储过程”，右键单击要针对其授予权限的过程，再单击“属性”。  
  
4.  在 **“存储过程属性”** 中，选择 **“权限”** 页。  
  
5.  若要为用户、数据库角色或应用程序角色授予权限，请单击 **“搜索”** 。  
  
6.  在 **“选择用户或角色”** 中，单击 **“对象类型”** 以添加或清除所需的用户和角色。  
  
7.  单击 **”浏览“** 以显示用户或角色列表。 选择应对其授予权限的用户或角色。  
  
8.  在 **“显式权限”** 网格中，选择要为指定的用户或角色授予的权限。 有关权限的说明，请参阅[权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)。  

 选择 **“授予”** 指示要为被授权者授予指定的权限。 选择 **“具有授予权限”** 指示被授权者还可以将指定权限授予其他主体。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-grant-permissions-on-a-stored-procedure"></a>授予对存储过程的权限  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 该示例授予名为 `EXECUTE` 的应用程序角色对存储过程 `HumanResources.uspUpdateEmployeeHireInfo` 的 `Recruiting11`权限。  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [创建存储过程](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [修改存储过程](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [删除存储过程](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [重命名存储过程](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)  
  
  
