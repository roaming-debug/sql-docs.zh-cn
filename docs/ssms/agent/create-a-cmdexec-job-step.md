---
description: 创建 CmdExec 作业步骤
title: 创建 CmdExec 作业步骤
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- CmdExec jobs
ms.assetid: b48da5b4-6fe7-4eb7-bade-dc7d697c6d5c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e0bc263e814e0b87300159b98ec59441527f34c4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472338"
---
# <a name="create-a-cmdexec-job-step"></a>创建 CmdExec 作业步骤
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍了如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、[!INCLUDE[tsql](../../includes/tsql-md.md)] 或 SQL Server 管理对象在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中创建和定义使用可执行程序或操作系统命令的 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
默认情况下，只有 **sysadmin** 固定服务器角色的成员可以创建 CmdExec 作业步骤。 除非 **sysadmin** 用户创建一个代理帐户，否则这些作业步骤将在 SQL Server 代理服务帐户的上下文中运行。 如果不属于 **sysadmin** 角色成员的用户具有访问 CmdExec 代理帐户的权限，则也可以创建 CmdExec 作业步骤。  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-a-cmdexec-job-step"></a>创建 CmdExec 作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开“SQL Server 代理”  ，创建一个新作业或右键单击一个现有作业，再单击“属性”  。  
  
3.  在 **“作业属性”** 对话框中，单击 **“步骤”** 页，再单击 **“新建”** 。  
  
4.  在 **“新建作业步骤”** 对话框中，键入作业的 **“步骤名称”** 。  
  
5.  在 **“类型”** 列表中，选择 **“操作系统(CmdExec)”** 。  
  
6.  在 **“运行身份”** 列表中，选择具有作业将使用的凭据的代理帐户。 默认情况下，CmdExec 作业步骤在 SQL Server 代理服务帐户的上下文中运行。  
  
7.  在 **“成功命令的进程退出代码”** 框中，输入一个介于 0 到 999999 之间的值。  
  
8.  在 **“命令”** 框中，输入操作系统命令或可执行程序。 请参阅“使用 Transact T-SQL”中的示例。  
  
9. 单击 **“高级”** 页设置以下作业步骤选项，例如：当该作业步骤成功或失败时将执行的操作、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理应该尝试执行该作业步骤的次数，以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将作业步骤输出写入哪个文件。 只有 **sysadmin** 固定服务器角色的成员才可以将作业步骤输出写入到操作系统文件中。  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-create-a-cmdexec-job-step"></a>创建 CmdExec 作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- creates a job step that uses CmdExec  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'CMDEXEC',  
        @command = 'C:\clickme_scripts\SQL11\PostBOLReorg GetHsX.exe',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>使用 SQL Server 管理对象  
**创建 CmdExec 作业步骤**  
  
通过使用所选编程语言（如 Visual Basic、Visual C# 或 PowerShell）来使用 **JobStep** 类。  
