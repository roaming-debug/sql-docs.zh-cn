---
description: 创建作业
title: 创建作业
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: b35af2b6-6594-40d1-9861-4d5dd906048c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: fece3364c7d4399f323503f4e7bf58a01f295ba8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338826"
---
# <a name="create-a-job"></a>创建作业
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]、[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 SQL Server 管理对象 (SMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中创建 SQL Server 代理作业。  
  
若要添加可以发送到操作员的作业步骤、计划、警报和通知，请参阅“请参阅”部分中的主题的链接。  
  
-   **开始之前：**  
  
    [限制和局限](#Restrictions)  
  
    [安全性](#Security)  
  
-   **若要创建作业，可使用：**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
    [SQL Server 管理对象](#SMOProcedure)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
  
-   若要创建作业，用户必须是某个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理固定数据库角色或 **sysadmin** 固定服务器角色的成员。 作业只能由其所有者或 **sysadmin** 角色的成员进行编辑。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的固定数据库角色的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
-   将作业指派给另一个登录名并不保证新所有者有足够的权限来成功运行该作业。  
  
-   本地作业是由本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理进行缓存的。 因此，任何修改都会隐式强制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理重新缓存该作业。 由于直到调用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sp_add_jobserver **时，** 代理才缓存作业，因此最后调用 **sp_add_jobserver** 将更为有效。  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
-   您必须是系统管理员才可以更改作业的所有者。  
  
-   为了安全起见，仅作业所有者或 **sysadmin** 角色的成员可以更改作业的定义。 只有 **sysadmin** 固定服务器角色的成员才可以将作业所有权分配给其他用户，并且他们可以运行任何作业，而不管作业所有者是谁。  
  
    > [!NOTE]  
    > 如果将作业所有权重新指派到的用户不是 **sysadmin** 固定服务器角色的成员，而执行作业的步骤需要代理帐户（例如， [!INCLUDE[ssIS](../../includes/ssis_md.md)] 包执行），则请确保该用户可以访问该代理帐户，否则作业将失败。  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-a-sql-server-agent-job"></a>创建 SQL Server 代理作业  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开要创建 SQL Server 代理作业的服务器。  
  
2.  单击加号以展开 **“SQL Server 代理”**。  
  
3.  右键单击“作业”文件夹，然后选择“新建作业…”。  
  
4.  在 **“新建作业”** 对话框的 **“常规”** 页上，修改作业的常规属性。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“常规”页）](../../ssms/agent/job-properties-new-job-general-page.md)  
  
5.  在 **“步骤”** 页上，组织作业步骤。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“步骤”页）](../../ssms/agent/job-properties-new-job-steps-page.md)  
  
6.  在 **“计划”** 页上，组织作业的计划。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“计划”页）](../../ssms/agent/job-properties-new-job-schedules-page.md)  
  
7.  在 **“警报”** 页上，组织作业的警报。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“警报”页）](../../ssms/agent/job-properties-new-job-alerts-page.md)  
  
8.  在“通知”页上，设置在作业完成时 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理要执行的操作。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“通知”页）](../../ssms/agent/job-properties-new-job-notifications-page.md)。  
  
9. 在 **“目标”** 页上，管理作业的目标服务器。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“目标”页）](../../ssms/agent/job-properties-new-job-targets-page.md)。  
  
10. 完成后，单击 **“确定”** 。  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-create-a-sql-server-agent-job"></a>创建 SQL Server 代理作业  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE msdb ;  
    GO  
    EXEC dbo.sp_add_job  
        @job_name = N'Weekly Sales Data Backup' ;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',  
        @freq_type = 1,  
        @active_start_time = 233000 ;  
    USE msdb ;  
    GO  
    EXEC sp_attach_schedule  
       @job_name = N'Weekly Sales Data Backup',  
       @schedule_name = N'RunOnce';  
    GO  
    EXEC dbo.sp_add_jobserver  
        @job_name = N'Weekly Sales Data Backup';  
    GO  
    ```  
  
有关详细信息，请参阅：  
  
-   [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
  
-   [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
-   [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
-   [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)  
  
## <a name="using-sql-server-management-objects"></a><a name="SMOProcedure"></a>使用 SQL Server 管理对象  
**创建 SQL Server 代理作业**  
  
通过使用所选编程语言（如 Visual Basic、Visual C# 或 PowerShell）来调用 **Job** 类的 **Create** 方法。 有关示例代码，请参阅 [在 SQL Server 代理中计划自动管理任务](../../relational-databases/server-management-objects-smo/tasks/scheduling-automatic-administrative-tasks-in-sql-server-agent.md)。  
