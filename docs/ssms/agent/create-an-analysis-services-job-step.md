---
description: Create an Analysis Services Job Step
title: Create an Analysis Services Job Step
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.asquery.execgeneral.f1
helpviewer_keywords:
- job steps [Analysis Services]
ms.assetid: 03d4bb86-514b-4a55-97b9-c2c0fa08b428
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e65aa07cff708a7112b7a0c4e0bbc0338cb40860
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477108"
---
# <a name="create-an-analysis-services-job-step"></a>Create an Analysis Services Job Step

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中创建和定义通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Server 管理对象执行 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]Analysis Services 命令和查询的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代理作业步骤。  
  
-   **开始之前：**  
  
    [限制和局限](#Restrictions)  
  
    [安全性](#Security)  
  
-   **若要使用 Analysis Services 命令和/或查询创建 SQL Server 作业步骤，请使用：**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server 管理对象](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
  
-   如果作业步骤使用 Analysis Services 命令，则命令语句必须是 XML for Analysis Services **Execute** 方法。 该语句可以不包含完整的简单对象访问协议 (SOAP) 信封和 XML for Analysis **Discover** 方法。 虽然 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 支持完整的 SOAP 信封和 **Discover** 方法，但是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤却不支持。 有关 XML for Analysis Services 的详细信息，请参阅 [XML for Analysis 概述 (XMLA)](/previous-versions/sql/)。  
  
-   如果作业步骤使用 Analysis Services 查询，则查询语句必须是多维表达式 (MDX) 查询。 有关 MDX 的详细信息，请参阅 [MDX 语句基础知识 (MDX)](/analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services?viewFallbackFrom=sql-server-ver15)。  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
  
-   若要运行使用 Analysis Services 子系统的作业步骤，用户必须是 **sysadmin** 固定服务器角色的成员或有权访问为使用该子系统而定义的有效代理帐户。 此外， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户或代理必须是 Analysis Services 管理员或有效的 Windows 域帐户。  
  
-   只有 **sysadmin** 固定服务器角色的成员才可以将作业步骤输出写入到文件中。 如果运行作业步骤的用户是 **msdb** 数据库中 **SQLAgentUserRole** 数据库角色的成员，则输出只能写入表中。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理会将作业步骤输出写入 **msdb** 数据库中的 **sysjobstepslog** 表。  
  
-   有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-an-analysis-services-command-job-step"></a>创建 Analysis Services 命令作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开“SQL Server 代理”  ，创建一个新作业或右键单击一个现有作业，再单击“属性”  。 有关创建作业的详细信息，请参阅 [创建作业](../../ssms/agent/create-jobs.md)。  
  
3.  在 **“作业属性”** 对话框中，单击 **“步骤”** 页面，再单击 **“新建”**。  
  
4.  在 **“新建作业步骤”** 对话框中，键入作业 **“步骤名称”**。  
  
5.  在 **“类型”** 列表中，单击 **“SQL Server Analysis Services 命令”**。  
  
6.  在 **“运行身份”** 列表中，选择为使用 Analysis Services 命令子系统而定义的代理。 如果用户是 **sysadmin** 固定服务器角色的成员，还可以选择 **“SQL Agent 服务帐户”** 运行此作业步骤。  
  
7.  选择用于运行作业步骤的 **“服务器”** 或键入服务器名称。  
  
8.  在 **“命令”** 框中，键入要执行的语句，或者单击 **“打开”** 选择一个语句。  
  
9. 单击 **“高级”** 页可定义此作业步骤的选项，例如在作业步骤成功或失败时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理所应执行的操作、作业步骤的尝试次数以及用于写入作业步骤输出的位置。  
  
#### <a name="to-create-an-analysis-services-query-job-step"></a>创建 Analysis Services 查询作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开“SQL Server 代理”  ，创建一个新作业或右键单击一个现有作业，再单击“属性”  。 有关创建作业的详细信息，请参阅 [创建作业](../../ssms/agent/create-jobs.md)。  
  
3.  在 **“作业属性”** 对话框中，单击 **“步骤”** 页，再单击 **“新建”** 。  
  
4.  在 **“新建作业步骤”** 对话框中，键入作业的 **“步骤名称”** 。  
  
5.  在 **“类型”** 列表中，单击 **“SQL Server Analysis Services 查询”**。  
  
6.  在 **“运行身份”** 列表中，选择为使用 Analysis Services 查询子系统而定义的代理。 如果用户是 **sysadmin** 固定服务器角色的成员，还可以选择 **“SQL Agent 服务帐户”** 运行此作业步骤。  
  
7.  选择用于运行作业步骤的 **“服务器”** 和 **“数据库”** ，或者键入服务器或数据库的名称。  
  
8.  在 **“命令”** 框中，键入要执行的语句，或者单击 **“打开”** 选择一个语句。  
  
9. 单击 **“高级”** 页可定义此作业步骤的选项，例如在作业步骤成功或失败时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理所应执行的操作、作业步骤的尝试次数以及用于写入作业步骤输出的位置。  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-create-an-analysis-services-command-job-step"></a>创建 Analysis Services 命令作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Creates a job step that uses XMLA to create a relational data source that
    -- references the AdventureWorks2012 Microsoft SQL Server database.  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name =
            N'Create a relational data source that references the AdventureWorks2012 Microsoft SQL Server database',  
        @subsystem = N'ANALYSISCOMMAND',  
        @command =
            N' <Create xmlns="https://schemas.microsoft.com/analysisservices/2003/engine">  
        <ParentObject>  
            <DatabaseID>AdventureWorks2012</DatabaseID>  
        </ParentObject>  
        <ObjectDefinition>  
            <DataSource xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:type="RelationalDataSource">  
                <ID>AdventureWorks2012</ID>  
                <Name>Adventure Works 2012</Name>  
                <ConnectionString>Data Source=localhost;Initial Catalog=AdventureWorks2012;Integrated Security=True</ConnectionString>  
                <ImpersonationInfo>  
                    <ImpersonationMode>ImpersonateServiceAccount</ImpersonationMode>  
                </ImpersonationInfo>  
                <ManagedProvider>System.Data.SqlClient</ManagedProvider>  
                <Timeout>PT0S</Timeout>  
            </DataSource>  
        </ObjectDefinition>  
    </Create>', ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)。  
  
#### <a name="to-create-an-analysis-services-query-job-step"></a>创建 Analysis Services 查询作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Creates a job step that uses MDX to return data  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Returns the Internet sales amount by state',  
        @subsystem = N'ANALYSISQUERY',  
        @command = N' SELECT  
       [Measures].[Internet Sales Amount] ON COLUMNS,  
       [Customer].[State-Province].Members ON ROWS  
    FROM [AdventureWorks2012]',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)。  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>使用 SQL Server 管理对象  
**创建 PowerShell 脚本作业步骤**  
  
通过使用所选的编程语言（如 XMLA 或 MDX）来使用 **JobStep** 类。 有关详细信息，请参阅 [SQL Server 管理对象 (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)。  
