---
description: Set Job Step Success or Failure Flow
title: Set Job Step Success or Failure Flow
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, action flow logic
- successful jobs [SQL Server]
- failed jobs [SQL Server]
- jobs [SQL Server Agent], action flow logic
ms.assetid: 23041ccf-8a07-41d3-85b9-c449a54b7e1e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 994faf31dd5cffac9c69fa996478baf539465b99
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472268"
---
# <a name="set-job-step-success-or-failure-flow"></a>Set Job Step Success or Failure Flow
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

创建 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业时，可以指定如果在作业执行期间发生故障，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应执行什么操作。 根据每个作业步骤的成功或失败确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应执行的操作。 然后通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理使用以下过程配置作业步骤的操作流逻辑。  
  
-   **开始之前：**  
  
    [安全性](#Security)  
  
-   **若要设置作业步骤的成功流或失败流，请使用：**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server 管理对象](#SMO)  
  
## <a name="before-you-begin"></a>开始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>设置作业步骤的成功流或失败流  
  
1.  在 **对象资源管理器** 中，展开 **“SQL Server 代理”**，然后展开 **“作业”**。  
  
2.  右键单击要编辑的作业，然后单击“属性”。  
  
3.  选择 **“步骤”** 页，单击一个步骤，再单击 **“编辑”**。  
  
4.  在 **“作业步骤属性”** 对话框中，选择 **“高级”** 页。  
  
5.  在“成功时要执行的操作”列表中，单击作业步骤成功完成时要执行的操作。  
  
6.  在 **“重试次数”** 框中输入介于 0 到 9999 之间的次数，应将作业步骤重复该次数，然后才能认为其失败。 如果在“重试次数”框中输入的值大于 0，则应在“重试间隔（分钟）”框中输入介于 1 到 9999 之间的分钟数，必须经过该间隔后才能重试作业步骤。  
  
7.  在 **“失败时要执行的操作”** 列表中，单击作业步骤失败时要执行的操作。  
  
8.  如果作业是一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本，则可以从下列选项中进行选择：  
  
    -   在 **“输出文件”** 框中，输入要将脚本输出写入的输出文件名。 默认情况下，每次执行作业步骤时都覆盖该文件。 如果不希望覆盖输出文件，请选中 **“将输出追加到现有文件”**。  
  
    -   如果希望将作业步骤记录到一个数据库表中，请选中 **“记录到表”** 。 默认情况下，每次执行作业步骤时都覆盖该表的内容。 如果不希望覆盖表内容，则请选中 **“将输出追加到表中的现有条目”**。 在执行作业步骤后，您可以通过单击 **“查看”** 来查看此表的内容。  
  
    -   如果希望将输出包括在步骤的历史记录中，请选中 **“在历史记录中包含步骤输出”** 。 仅当没有错误时，才会显示输出结果。 此外，输出可能会被截断。  
  
9. 如果 **“作为以下用户运行”** 列表可用，则选择具有作业要使用的凭据的代理帐户。  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>设置作业步骤的成功流或失败流  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @on_success_action = 1;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)。  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>使用 SQL Server 管理对象  
**设置作业步骤的成功流或失败流**  
  
通过使用所选编程语言（如 Visual Basic、Visual C# 或 PowerShell）来使用 **JobStep** 类。 有关详细信息，请参阅 [SQL Server 管理对象 (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)。  
