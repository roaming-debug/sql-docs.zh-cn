---
description: Give Others Ownership of a Job
title: Give Others Ownership of a Job
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], owners
- owners [SQL Server], jobs
- SQL Server Agent jobs, owners
ms.assetid: 2ded5e9c-4251-4fb1-a047-99f13d150b61
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 9a1dc39795646c704d8168858475095cd549cb0e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461358"
---
# <a name="give-others-ownership-of-a-job"></a>将作业所有权授予其他人
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍如何将 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的所有权重新指派给另一用户。  
  
-   **开始之前：** [限制和局限](#Restrictions)、[安全性](#Security)  
  
-   **若要将作业所有权授予其他人，请使用：**  
  
    [SQL Server Management Studio](#SSMSProc2)  
  
    [Transact-SQL](#TsqlProc2)  
  
    [SQL Server 管理对象](#SMOProc2)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
若要创建作业，用户必须是某个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理固定数据库角色或 **sysadmin** 固定服务器角色的成员。 作业只能由其所有者或 **sysadmin** 角色的成员进行编辑。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的固定数据库角色的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
您必须是系统管理员才可以更改作业的所有者。  
  
将作业指派给另一个登录名并不保证新所有者有足够的权限来成功运行该作业。  
  
### <a name="security"></a><a name="Security"></a>安全性  
为了安全起见，仅作业所有者或 **sysadmin** 角色的成员可以更改作业的定义。 只有 **sysadmin** 固定服务器角色的成员才可以将作业所有权分配给其他用户，并且他们可以运行任何作业，而不管作业所有者是谁。  
  
> [!NOTE]  
> 如果将作业所有权重新指派到的用户不是 **sysadmin** 固定服务器角色的成员，而执行作业的步骤需要代理帐户（例如， [!INCLUDE[ssIS](../../includes/ssis_md.md)] 包执行），则请确保该用户可以访问该代理帐户，否则作业将失败。  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProc2"></a>使用 SQL Server Management Studio  
**将作业所有权授予其他人**  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开“SQL Server 代理”，再展开“作业”，右键单击相应作业，然后单击“属性”。  
  
3.  在 **“所有者”** 列表中，选择一个登录名。 您必须是系统管理员才可以更改作业的所有者。  
  
    将作业指派给另一个登录名并不保证新所有者有足够的权限来成功运行该作业。  
  
## <a name="using-transact-sql"></a><a name="TsqlProc2"></a>使用 Transact-SQL  
**将作业所有权授予其他人**  
  
1.  在对象资源管理器中，连接到数据库引擎实例，然后展开该实例。  
  
2.  在工具栏上，单击 **“新建查询”** 。  
  
3.  在查询窗口中，输入以下使用 [sp_manage_jobs_by_login (TRANSACT-SQL)](../../relational-databases/system-stored-procedures/sp-manage-jobs-by-login-transact-sql.md) 系统存储过程的语句。 以下示例将所有作业从 `danw` 重新分配给 `françoisa`。  
  
    ```  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_manage_jobs_by_login  
        @action = N'REASSIGN',  
        @current_owner_login_name = N'danw',  
        @new_owner_login_name = N'françoisa' ;  
    GO  
    ```  
  
## <a name="using-sql-server-management-objects"></a><a name="SMOProc2"></a>使用 SQL Server 管理对象  
**将作业所有权授予其他人**  
  
1.  通过使用所选编程语言（如 Visual Basic、Visual C# 或 PowerShell）来调用 **Job** 类。 有关示例代码，请参阅 [在 SQL Server 代理中计划自动管理任务](../../relational-databases/server-management-objects-smo/tasks/scheduling-automatic-administrative-tasks-in-sql-server-agent.md)。  
  
## <a name="see-also"></a>另请参阅  
[执行作业](../../ssms/agent/implement-jobs.md)  
[创建作业](../../ssms/agent/create-jobs.md)  
