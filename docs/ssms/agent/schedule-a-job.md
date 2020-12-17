---
description: Schedule a Job
title: Schedule a Job
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scheduling jobs [SQL Server]
- SQL Server Agent jobs, scheduling
- jobs [SQL Server Agent], scheduling
ms.assetid: f626390a-a3df-4970-b7a7-a0529e4a109c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 18cb7d2e1c6ccf51ed1921d65a0c31fa84c54b5b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478818"
---
# <a name="schedule-a-job"></a>安排作业计划
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍如何安排 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业计划。  
  
-   **开始之前：**  
  
    [安全性](#Security)  
  
-   **若要安排作业计划，可使用：**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server 管理对象](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-and-attach-a-schedule-to-a-job"></a>创建计划并将其附加到作业中  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开“SQL Server 代理”，展开“作业”，右键单击要计划的作业，并单击“属性”。  
  
3.  选择 **“计划”** 页，再单击 **“新建”**。  
  
4.  在 **“名称”** 框中，键入新计划的名称。  
  
5.  如果不希望计划在创建后立即生效，则清除 **“启用”** 复选框。  
  
6.  对于 **“计划类型”**，请选择下列操作之一：  
  
    -   单击 **“SQL Server 代理启动时自动启动”** ，在启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务时启动作业。  
  
    -   单击 **“CPU 空闲时启动”** ，在 CPU 达到空闲条件时启动作业。  
  
    -   如果希望反复运行计划，则单击 **“重复执行”** 。 若要设置重复执行的计划，请完成对话框上的 **“频率”**、 **“每天频率”** 和 **“持续时间”** 组。  
  
    -   如果希望仅运行一次计划，请单击 **“执行一次”** 。 若要设置“执行一次”计划，请完成对话框上的“执行一次”组。  
  
#### <a name="to-attach-a-schedule-to-a-job"></a>将计划附加到作业中  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开“SQL Server 代理”，展开“作业”，右键单击要计划的作业，然后单击“属性”。  
  
3.  选择 **“计划”** 页，再单击 **“选取”**。  
  
4.  选择要附加的计划，然后单击 **“确定”**。  
  
5.  在“作业属性”对话框中，双击附加的计划。  
  
6.  验证是否正确设置了 **“开始日期”** 。 如果该选项的设置不正确，则将日期设置为要让计划启动的日期，然后单击 **“确定”**。  
  
7.  在 **“作业属性”** 对话框中，单击 **“确定”**。  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-schedule-a-job"></a>安排作业计划  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE msdb ;  
    GO  
    -- creates a schedule named NightlyJobs.   
    -- Jobs that use this schedule execute every day when the time on the server is 01:00.   
    EXEC sp_add_schedule  
        @schedule_name = N'NightlyJobs' ,  
        @freq_type = 4,  
        @freq_interval = 1,  
        @active_start_time = 010000 ;  
    GO  
    -- attaches the schedule to the job BackupDatabase  
    EXEC sp_attach_schedule  
       @job_name = N'BackupDatabase',  
       @schedule_name = N'NightlyJobs' ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md) 和 [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)。  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>使用 SQL Server 管理对象  
通过使用所选编程语言（如 Visual Basic、Visual C# 或 PowerShell）来使用 **JobSchedule** 类。 有关详细信息，请参阅[SQL Server 管理对象 (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)。  
