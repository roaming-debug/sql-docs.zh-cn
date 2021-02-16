---
title: 计划安排 Azure 上的 SSIS 包 | Microsoft Docs
description: 概述了用于计划已部署到 Azure SQL 数据库的 SSIS 包执行的可用方法。
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 7b7c6d785910425aa6388c23bc51b4690d7782c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345695"
---
# <a name="schedule-the-execution-of-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>计划 Azure 中部署的 SQL Server Integration Services (SSIS) 包的执行

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



可以通过选择本文介绍的方法之一，计划已部署到 Azure SQL 数据库服务器上 SSISDB 目录中的 SSIS 包的执行。 可以直接计划包，或作为 Azure 数据工厂管道的一部分间接地计划包。 有关 Azure 上 SSIS 的概述，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](ssis-azure-lift-shift-ssis-packages-overview.md)。

- 直接计划包

  - [借助 SQL Server Management Studio (SSMS) 中的“计划”选项进行计划](#ssms)

  - [SQL 数据库弹性作业](#elastic)

  - [SQL Server 代理](#agent)

- [作为 Azure 数据工厂管道的一部分间接地计划包](#activity)


## <a name="schedule-a-package-with-ssms"></a><a name="ssms"></a>使用 SSMS 计划包

在 SQL Server Management Studio (SSMS) 中，可以右键单击部署到 SSIS 目录数据库 (SSISDB) 的包，并选择“计划”以打开“新建计划”对话框   。 有关详细信息，请参阅[使用 SSMS 计划 Azure 中的 SSIS 包](ssis-azure-schedule-packages-ssms.md)。

此功能要求 SQL Server Management Studio 17.7 或更高版本。 若要获取 SSMS 最新版本，请参阅[下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。

## <a name="schedule-a-package-with-sql-database-elastic-jobs"></a><a name="elastic"></a> 使用 SQL 数据库弹性作业计划安排一个包

有关 SQL 数据库上的弹性作业的详细信息，请参阅[管理横向扩展的云数据库](/azure/sql-database/sql-database-elastic-jobs-overview)。

### <a name="prerequisites"></a>必备条件

在可以使用弹性作业来计划存储在 Azure SQL 数据库服务器上 SSISDB 目录数据库中的 SSIS 包之前，必须先完成以下操作：

1.  安装并配置弹性数据库作业组件。 有关详细信息，请参阅[安装弹性数据库作业概述](/azure/sql-database/sql-database-elastic-jobs-service-installation)。

2. 创建数据库范围的凭据，作业可使用该凭据将命令发送到 SSIS 目录数据库。 有关详细信息，请参阅 [CREATE DATABASE SCOPED CREDENTIAL（创建数据库范围的凭据）(Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)。

### <a name="create-an-elastic-job"></a>创建弹性作业

使用与下列示例所示的脚本类似的 Transact-SQL 脚本来创建作业：

```sql
-- Create Elastic Jobs target group
EXEC jobs.sp_add_target_group 'TargetGroup'

-- Add Elastic Jobs target group member
EXEC jobs.sp_add_target_group_member @target_group_name='TargetGroup',
    @target_type='SqlDatabase', @server_name='YourSQLDBServer.database.windows.net',
    @database_name='SSISDB' 

-- Add a job to schedule SSIS package execution
EXEC jobs.sp_add_job @job_name='ExecutePackageJob', @description='Description', 
    @schedule_interval_type='Minutes', @schedule_interval_count=60

-- Add a job step to create/start SSIS package execution using SSISDB catalog stored procedures
EXEC jobs.sp_add_jobstep @job_name='ExecutePackageJob', 
    @command=N'DECLARE @exe_id bigint 
        EXEC [SSISDB].[catalog].[create_execution]
            @folder_name=N''folderName'', @project_name=N''projectName'',
            @package_name=N''packageName'', @use32bitruntime=0,
            @runinscaleout=1, @useanyworker=1, 
            @execution_id=@exe_id OUTPUT         
        EXEC [SSISDB].[catalog].[start_execution] @exe_id, @retry_count=0', 
    @credential_name='YourDBScopedCredentials', 
    @target_group_name='TargetGroup' 

-- Enable the job schedule 
EXEC jobs.sp_update_job @job_name='ExecutePackageJob', @enabled=1, 
    @schedule_interval_type='Minutes', @schedule_interval_count=60 
```

## <a name="schedule-a-package-with-sql-server-agent-on-premises"></a><a name="agent"></a>使用 SQL Server 代理在本地计划包

有关 SQL Server 代理的详细信息，请参阅 [包的 SQL Server 代理作业](../packages/sql-server-agent-jobs-for-packages.md)。

### <a name="prerequisite---create-a-linked-server"></a>先决条件 - 创建链接服务器

必须先将 SQL 数据库服务器作为链接服务器添加到本地 SQL Server，才能在本地使用 SQL Server 代理来计划安排存储在 Azure SQL 数据库服务器上的包的执行。

1.  **设置链接服务器**

    ```sql
    -- Add the SSISDB database on your Azure SQL Database as a linked server to your SQL Server on premises
    EXEC sp_addlinkedserver
        @server='myLinkedServer', -- Name your linked server
        @srvproduct='',     
        @provider='sqlncli', -- Use SQL Server native client
        @datasrc='<server_name>.database.windows.net', -- Add your Azure SQL Database server endpoint
        @location='',
        @provstr='',
        @catalog='SSISDB'  -- Add SSISDB as the initial catalog
    ```

2.  **设置链接服务器凭据**

    ```sql
    -- Add your Azure SQL Database server admin credentials
    EXEC sp_addlinkedsrvlogin
        @rmtsrvname = 'myLinkedServer',
        @useself = 'false',
        @rmtuser = 'myUsername', -- Add your server admin username
        @rmtpassword = 'myPassword' -- Add your server admin password
    ```

3.  **设置链接服务器选项**

    ```sql
    EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;
    ```

有关详细信息，请参阅[创建链接服务器](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md)和[链接服务器](../../relational-databases/linked-servers/linked-servers-database-engine.md)。

### <a name="create-a-sql-server-agent-job"></a>创建 SQL Server 代理作业

若要在本地使用 SQL Server 代理来计划安排一个包，请创建一个具有作业步骤的作业，该作业依次调用 SSIS 目录存储过程 `[catalog].[create_execution]` 和 `[catalog].[start_execution]`。 有关详细信息，请参阅[包的 SQL Server 代理作业](../packages/sql-server-agent-jobs-for-packages.md)。

1.  在 SQL Server Management Studio 中，连接到要在其中创建作业的本地 SQL Server 数据库。

2.  右键单击“SQL Server 代理”  节点，选择“新建”  ，然后选择“作业”  以打开“新建作业”  对话框。

3.  在“新建作业”  对话框中，选择“步骤”  页，然后选择“新建”  以打开“新建作业步骤”  对话框。

4.  在“新建作业步骤”  对话框中，选择 `SSISDB` 作为  **数据库。**

5.  在“命令”  字段中，输入与以下示例所示脚本类似的 Transact-SQL 脚本：

    ```sql
    -- T-SQL script to create and start SSIS package execution using SSISDB stored procedures
    DECLARE @return_value int, @exe_id bigint 

    EXEC @return_value = [YourLinkedServer].[SSISDB].[catalog].[create_execution] 
        @folder_name=N'folderName', @project_name=N'projectName', 
        @package_name=N'packageName', @use32bitruntime=0, @runincluster=1, @useanyworker=1,
        @execution_id=@exe_id OUTPUT 

    EXEC [YourLinkedServer].[SSISDB].[catalog].[set_execution_parameter_value] @exe_id,
        @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1

    EXEC [YourLinkedServer].[SSISDB].[catalog].[start_execution] @execution_id=@exe_id
    ```

6.  完成配置并计划作业。

## <a name="schedule-a-package-as-part-of-an-azure-data-factory-pipeline"></a><a name="activity"></a>作为 Azure 数据工厂管道的一部分计划包

可通过使用触发器间接地计划包，从而运行用于运行 SSIS 包的 Azure 数据工厂管道。

若要计划数据工厂管道，请使用以下触发器之一：

- [计划触发器](/azure/data-factory/how-to-create-schedule-trigger)

- [翻转窗口触发器](/azure/data-factory/how-to-create-tumbling-window-trigger)

- [基于事件的触发器](/azure/data-factory/how-to-create-event-trigger)

若要将 SSIS 包作为数据工厂管道的一部分运行，请使用以下活动之一：

- [执行 SSIS 包活动](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)。

- [已存储过程活动](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity)。

## <a name="next-steps"></a>后续步骤

查看用于运行部署到 Azure 的 SSIS 包的选项。 有关详细信息，请参阅[在 Azure 中运行 SSIS 包](ssis-azure-run-packages.md)。