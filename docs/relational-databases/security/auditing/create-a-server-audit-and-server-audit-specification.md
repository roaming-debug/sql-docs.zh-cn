---
title: 创建服务器审核和服务器审核规范
description: 了解如何使用 SQL Server Management Studio (SSMS) 或 Transact-SQL (T-SQL) 创建 SQL Server 审核和服务器审核规范。
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.SWB.SQLAUDIT.FILTER.F1
- sql13.swb.sqlaudit.general.f1
- sql13.swb.sqlaudit.srvaudit.general.f1
helpviewer_keywords:
- server audit [SQL Server]
- audits [SQL Server], specification
ms.assetid: 6624b1ab-7ec8-44ce-8292-397edf644394
author: DavidTrigano
ms.author: datrigan
ms.openlocfilehash: 3db484c1279546ab76ebf302582dbb984f991fa4
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250114"
---
# <a name="create-a-server-audit-and-server-audit-specification"></a>创建服务器审核和服务器审核规范
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中创建服务器审核和服务器审核规范。 “  审核” [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库涉及到跟踪和记录系统中发生的事件。 *SQL Server Audit* 对象收集单个服务器实例或数据库级操作和操作组以进行监视。 这种审核处于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例级别。 每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例可以具有多个审核。 “服务器审核规范”  对象属于审核。 您可以为每个审核创建一个服务器审核规范，因为它们都是在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例范围内创建的。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要创建服务器审核和服务器审核规范，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   审核必须已存在，才能为它创建服务器审核规范。 服务器审核规范在创建之后处于禁用状态。  
  
-   CREATE SERVER AUDIT 语句位于事务范围内。 如果对事务进行回滚，也将对该语句进行回滚。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
-   若要创建、更改或删除服务器审核，主体需要拥有 ALTER ANY SERVER AUDIT 或 CONTROL SERVER 权限。  
  
-   具有 ALTER ANY SERVER AUDIT 权限的用户可以创建服务器审核规范并将其绑定到任何审核。  
  
-   创建服务器审核规范后，具有 CONTROL SERVER 或 ALTER ANY SERVER AUDIT 权限的主体、sysadmin 帐户或具有对审核的明确访问权的主体即可查看该规范。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-server-audit"></a>创建服务器审核  
  
1.   在对象资源管理器中，展开“安全性”文件夹。  
  
2.  右键单击“审核”文件夹，然后选择“新建审核…”   。  
  
     在 **“创建审核”** 对话框的 **“常规”** 页上提供以下选项。  
  
     **审核名称**  
     审核的名称。 这是在创建新审核时自动生成的，但是您可以对其进行编辑。  
  
     **队列延迟(毫秒)**  
     指定在强制处理审核操作之前可以等待的时间（毫秒）。  值 0 指示同步传递。 默认的最小值为 **1000** （1 秒）。 最大值为 2,147,483,647（2,147,483.647 秒或者 24 天 20 小时 31 分钟 23.647 秒）。  
  
     **在审核日志失败时：**  
     **继续**  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 操作将继续。 审核记录将不会保留。 审核将继续尝试将事件记入日志，并且在故障条件得到解决后将恢复。 选择 **“继续”** 选项可以允许未经审核的活动，这可能违反了您的安全策略。 在 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的继续操作比维护完整审核更重要时，选择此选项。 这是默认选项。  
  
     **关闭服务器**  
     在写入目标的服务器实例无法将数据写入审核目标时，强制关闭服务器。 发出此命令的登录名必须具有 **SHUTDOWN** 权限。 如果该登录名没有此权限，则该函数将失败并将引发错误消息。 将不会发生审核的事件。 在审核失败可能损害系统的安全或完整性时，选择此选项。  
  
     **失败操作**  
     在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit 无法写入审核日志的情况下，如果数据库操作将导致审核的事件，则此选项将导致数据库操作失败。 将不会发生审核的事件。 不会导致审核的事件的操作可以继续。 审核将继续尝试将事件记入日志，并且在故障条件得到解决后将恢复。 在维护完整审核比对 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的完全访问权限更重要时，选择此选项。  
  
    > [!IMPORTANT]  
    >  在审核处于失败状态时，专用管理员连接可继续执行审核的事件。  
  
     “审核目标”  列表  
     指定数据的审核目标。 可用选项包括二进制文件、Windows 应用程序日志或 Windows 安全日志。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 写入到 Windows 安全日志。 有关详细信息，请参阅 [将 SQL Server 审核事件写入安全日志](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md)。  
  
     **文件路径**  
     指定当“审核目标”  是文件时，要将审核数据写入的文件夹所在的位置。  
  
     **省略号 (...)**  
     打开“定位文件夹 - server\_name”对话框，以指定文件路径或创建要写入审核文件的文件夹   。  
  
     **审核文件最大限制：**  
     **最大滚动更新文件数**  
     指定在达到审核文件的最大数目时，新的文件内容将覆盖最旧的审核文件。  
  
     **最大文件数**  
     指定在达到最大审核文件数时，导致生成附加审核事件的任何操作都将失败并报告错误。  
  
     “无限制”  复选框  
     在选中了“最大滚动更新文件数”  下的“无限制”  复选框后，可创建的审核文件数不受任何限制。 **“无限制”** 复选框默认被选中并且适用于 **“最大滚动更新文件数”** 和 **“最大文件数”** 选择。  
  
     “文件数”  框  
     指定要创建的审核文件的数目，最高为 2,147,483,647。 只有取消选中 **“无限制”** 时，此选项才可用。  
  
     **最大文件大小**  
     指定审核文件的最大大小，可以兆字节 (MB)、千兆字节 (GB) 或百万兆字节 (TB) 为单位。 可以指定的数字最大为 2,147,483,647 TB。 选中 **“无限制”** 复选框将不会对文件大小施加限制。 默认情况下， **“无限制”** 复选框为选中状态。  
  
     “保留磁盘空间”  复选框  
     指定在磁盘上预先分配与指定的最大文件大小相等的空间。 只有在 **“最大文件大小”** 下未选中 **“无限制”** 复选框的情况下，才能使用此设置。 默认情况下，不选中此复选框。  
  
3.  或者，在 **“筛选器”** 页上，对服务器审核输入一个谓词或 `WHERE` 子句，以便指定在 **“常规”** 页上未提供的附加选项。 用小括号将该谓词括起来，例如 `(object_name = 'EmployeesTable')`。  
  
4.  在完成选项选择后，请单击 **“确定”** 。  
  
#### <a name="to-create-a-server-audit-specification"></a>创建服务器审核规范  
  
1.  在对象资源管理器中，右键单击加号以展开 **“安全性”** 文件夹。  
  
2.  右键单击“服务器审核规范”文件夹，然后选择“新建服务器审核规范…”   。  
  
     **“创建服务器审核规范”** 对话框中提供了以下选项。  
  
     **名称**  
     服务器审核规范的名称。 这是在创建新服务器审核规范时自动生成的，但是您可以对其进行编辑。  
  
     **审核**  
     现有服务器审核的名称。 或者键入审核的名称，或者从列表中选择一个名称。  
  
     **审核操作类型**  
     指定要捕获的服务器级别审核操作组和审核操作。 有关服务器级别审核操作组和审核操作的列表以及它们所包含事件的说明，请参阅 [SQL Server 审核操作组和操作](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)。  
  
     **对象架构**  
     显示具有指定“对象名称”  的对象的架构。  
  
     **Object Name**  
     要审核的对象的名称。 这仅适用于审核操作，而不适用于审核组。  
  
     **省略号 (...)**  
     打开 **“选择对象”** 对话框，以便基于指定的 **“审核操作类型”** 浏览和选择可用对象。  
  
     **主体名称**  
     对于所审核的对象，要作为审核筛选依据的帐户。  
  
     **省略号 (...)**  
     打开 **“选择对象”** 对话框以基于指定的 **“对象名称”** 浏览和选择可用对象。  
  
3.  完成后，单击“**确定**”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-server-audit"></a>创建服务器审核  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 
  
    ```  
    -- Creates a server audit called "HIPAA_Audit" with a binary file as the target and no options.  
    CREATE SERVER AUDIT HIPAA_Audit  
        TO FILE ( FILEPATH ='E:\SQLAudit\' );  
    ```  
> [!NOTE]
> 即使可以使用 UNC 路径作为审核文件目标，也要谨慎使用。 如果相应文件共享存在网络延迟，SQL Server 可能会出现性能降低，因为线程会先等待审核写入完成，再继续运行。 你可能会在 SQL Server 错误日志中看到各种错误消息，如 17894：
>
>   “2020-02-07 12:21:35.100 来自调度程序池‘XE 引擎主调度程序池’工作器 0x00000058E7300000 的服务器调度程序 (0x7954) 似乎在节点 0 上无法完成”。


#### <a name="to-create-a-server-audit-specification"></a>创建服务器审核规范  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    /*Creates a server audit specification called "HIPAA_Audit_Specification" that audits failed logins for the SQL Server audit "HIPAA_Audit" created above.  
    */  
  
    CREATE SERVER AUDIT SPECIFICATION HIPAA_Audit_Specification  
    FOR SERVER AUDIT HIPAA_Audit  
        ADD (FAILED_LOGIN_GROUP);  
    GO  
    -- Enables the audit.   
  
    ALTER SERVER AUDIT HIPAA_Audit  
    WITH (STATE = ON);  
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE SERVER AUDIT (Transact-SQL)](../../../t-sql/statements/create-server-audit-transact-sql.md) 和 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../../t-sql/statements/create-server-audit-specification-transact-sql.md)。  
  
  
