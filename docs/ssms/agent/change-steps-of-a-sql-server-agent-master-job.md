---
description: Change Steps of a SQL Server Agent Master Job
title: Change Steps of a SQL Server Agent Master Job
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 8f1a0ee6-49ff-4080-94ca-d661daeff2a6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 360da5d93c11403d4da325e9a5d787900ff682e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351703"
---
# <a name="change-steps-of-a-sql-server-agent-master-job"></a>更改 SQL Server 代理主作业的步骤
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中更改 SQL Server 代理主作业的步骤。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理主作业不能同时把本地服务器和远程服务器作为目标。  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
除非您是 **sysadmin** 固定服务器角色的成员，否则您只能修改自己拥有的作业。 有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-make-changes-to-the-steps-of-a-sql-server-agent-master-job"></a>更改 SQL Server 代理主作业的步骤  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开包含要修改步骤的作业的服务器。  
  
2.  单击加号以展开 **“SQL Server 代理”**。  
  
3.  单击加号以便展开 **“作业”** 文件夹。  
  
4.  右键单击要修改步骤的作业，然后选择“属性”。  
  
5.  在“作业属性 - job\_name”对话框中的“选择页”下，选择“步骤 。  
  
6.  单击“编辑”，打开“作业步骤属性 -job \_step\_name”对话框 。 有关此对话框中的可用选项的详细信息，请参阅[作业步骤属性 - 新建作业步骤（“常规”页）](../../ssms/agent/job-step-properties-new-job-step-general-page.md)和[作业步骤属性 - 新建作业步骤（“高级”页）](../../ssms/agent/job-step-properties-new-job-step-advanced-page.md)。  
  
7.  完成后，单击 **“确定”** 。  
  
8.  在“作业属性 - job\_name”对话框中，单击“确定” 。  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-make-changes-to-the-steps-of-a-sql-server-agent-master-job"></a>更改 SQL Server 代理主作业的步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- changes the number of retry attempts for the first step
    -- of the Weekly Sales Data Backup job.   
    -- After running this example, the number of retry attempts is 10   
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_update_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_id = 1,  
        @retry_attempts = 10 ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_update_jobstep (TRANSACT-SQL)](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)。  
