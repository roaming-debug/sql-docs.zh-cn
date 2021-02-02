---
title: 创建维护计划 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中创建单服务器或多服务器维护计划。
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- maintenance plans [SQL Server], creating
ms.assetid: a945cb65-ba7a-42f4-bbd9-6ec675745523
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0d220ed52edbf86f84071c2194658af1c3306ef5
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049140"
---
# <a name="create-a-maintenance-plan"></a>创建维护计划
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建单服务器或多服务器维护计划。 通过使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]，您可以通过以下两种方式之一创建这些维护计划：使用维护计划向导或设计图面。 向导是创建基本维护计划的最佳方法，而使用设计图面创建计划允许您使用增强的工作流。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
     
     [先决条件](#Prerequisite)  
  
     [安全性](#Security)  
  
-   **若要创建维护计划，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 若要创建多服务器维护计划，必须配置包含一个主服务器和一个（或多个）目标服务器的多服务器环境。 必须在主服务器上创建和维护多服务器维护计划。 在目标服务器上可以查看这些计划，但不能进行维护。 
 
###  <a name="prerequisite"></a><a name="Prerequisite"></a> 先决条件  
必须启用 [“代理 XP”服务器配置选项](../../database-engine/configure-windows/agent-xps-server-configuration-option.md) 。
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 若要创建或管理维护计划，您必须是 **sysadmin** 固定服务器角色的成员。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-maintenance-plan-using-the-maintenance-plan-wizard"></a>使用维护计划向导创建维护计划  
  
1.  在对象资源管理器中，单击加号以便展开您要创建维护计划的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  右键单击“维护计划”文件夹，然后选择“维护计划向导”。  
  
4.  按照向导中显示的步骤创建维护计划。 有关详细信息，请参阅 [Use the Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)。  
  
#### <a name="to-create-a-maintenance-plan-using-the-design-surface"></a>使用设计图面创建维护计划  
  
1.  在对象资源管理器中，单击加号以便展开您要创建维护计划的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  右键单击“维护计划”  文件夹，然后选择“新建维护计划” 。  
  
4.  按照[创建维护计划（维护计划设计图面）](../../relational-databases/maintenance-plans/create-a-maintenance-plan-maintenance-plan-design-surface.md)中的步骤创建维护计划。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-maintenance-plan"></a>创建维护计划  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE msdb;  
    GO  
    --  Adds a new job, executed by the SQL Server Agent service, called "HistoryCleanupTask_1".  
    EXEC dbo.sp_add_job  
       @job_name = N'HistoryCleanupTask_1',   
       @enabled = 1,   
       @description = N'Clean up old task history' ;   
    GO  
    -- Adds a job step for reorganizing all of the indexes in the HumanResources.Employee table to the HistoryCleanupTask_1 job.   
    EXEC dbo.sp_add_jobstep  
        @job_name = N'HistoryCleanupTask_1',   
        @step_name = N'Reorganize all indexes on HumanResources.Employee table',   
        @subsystem = N'TSQL',   
        @command = N'USE AdventureWorks2012  
    GO  
    ALTER INDEX AK_Employee_LoginID ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX AK_Employee_NationalIDNumber ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX AK_Employee_rowguid ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX IX_Employee_OrganizationNode ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX PK_Employee_BusinessEntityID ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    ',   
        @retry_attempts = 5,   
        @retry_interval = 5 ;   
    GO  
    -- Creates a schedule named RunOnce that executes every day when the time on the server is 23:00.   
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',   
        @freq_type = 4,   
        @freq_interval = 1,   
        @active_start_time = 233000 ;   
    GO  
    -- Attaches the RunOnce schedule to the job HistoryCleanupTask_1.   
    EXEC sp_attach_schedule  
       @job_name = N'HistoryCleanupTask_1',  
       @schedule_name = N'RunOnce' ;   
    GO  
  
    ```  
  
 有关详细信息，请参阅：  
  
-   [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
  
-   [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
-   [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  
