---
description: 使用复制代理配置文件
title: 使用复制代理配置文件 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], agents and profiles
- replication agent profiles [SQL Server]
- agents [SQL Server replication], profiles
- profiles [SQL Server], replication agents
ms.assetid: 9c290a88-4e9f-4a7e-aab5-4442137a9918
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 613d3be8703563967241dda54725eda4c777a712
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765599"
---
# <a name="work-with-replication-agent-profiles"></a>使用复制代理配置文件
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中处理复制代理配置文件。 每个复制代理的行为都受一组参数控制，这些参数是通过代理配置文件设置的。 每个代理都有一个默认配置文件，有些代理还有一些其他的预定义配置文件；每个代理每次只能使用一个配置文件。  
  
 **本主题内容**  
  
-   **处理复制代理配置文件，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
    -   访问“代理配置文件”对话框  
  
    -   指定代理的配置文件  
  
    -   创建配置文件  
  
    -   修改配置文件  
  
    -   删除配置文件  
  
     [Transact-SQL](#TsqlProcedure)  
  
    -   创建配置文件  
  
    -   修改配置文件  
  
    -   删除配置文件  
  
    -   在同步期间使用代理配置文件  
  
    -   Transact-SQL 示例  
  
     [复制管理对象](#RMOProcedure)  
  
    -   创建配置文件  
  
    -   修改配置文件  
  
    -   删除配置文件  
  
-   **跟进：** [在更改代理参数后](#FollowUp)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
###  <a name="to-access-the-agent-profiles-dialog-box-from-sql-server-management-studio"></a><a name="Access_SSMS"></a> 从 SQL Server Management Studio 访问“代理配置文件”对话框  
  
1.  在“分发服务器属性 - \<Distributor>”对话框的“常规”页面上，单击“配置文件默认值”  。  

#### <a name="to-access-the-agent-profiles-dialog-box-from-replication-monitor"></a>从复制监视器访问“代理配置文件”对话框  
  
-   若要为所有代理打开此对话框，请右键单击发布服务器，再单击 **“代理配置文件”** 。  
  
-   为单个代理打开此对话框：  
  
    1.  在复制监视器的左窗格中依次展开发布服务器组、发布服务器，然后单击某个发布。  
  
    2.  对于分发代理配置文件和合并代理配置文件，请右键单击 **“所有订阅”** 选项卡上的订阅，再单击 **“代理配置文件”** 。 对于其他代理，右键单击 **“代理”** 选项卡上的代理，然后单击 **“代理配置文件”** 。  
  
###  <a name="to-specify-a-profile-for-an-agent"></a><a name="Specify_SSMS"></a> 指定代理的配置文件  
  
1.  如果 **“代理配置文件”** 对话框中显示了多个代理的配置文件，请选择一个代理。  
  
2.  在 **“代理配置文件”** 网格的 **“作为新项的默认值”** 列中选择一个配置文件。 默认情况下，该配置文件只应用于新发布和订阅的代理。  
  
3.  若要指定用于现有发布或订阅且属于选定类型的所有代理都使用该配置文件，请单击 **“更改现有代理”** 。  
  
###  <a name="to-view-and-edit-the-parameters-associated-with-a-profile"></a><a name="Modify_SSMS"></a> 查看和编辑与配置文件关联的参数  
  
1.  如果 **“代理配置文件”** 对话框中显示了多个代理的配置文件，请选择一个代理。  
  
2.  单击配置文件旁边的属性按钮 (...)。  
  
3.  查看“\<ProfileName> 配置文件属性”对话框中的参数和值。  
  
    -   用户定义配置文件中的参数可以编辑，但预定义系统配置文件中的参数不可编辑。  
  
    -   若要查看代理的所有参数，请清除 **“仅显示此配置文件中使用的参数”** 复选框。 有关代理参数的信息，请参阅本主题末尾处的链接。  
  
4.  单击“关闭”。  
  
###  <a name="to-create-a-user-defined-profile"></a><a name="Create_SSMS"></a> 创建用户定义的配置文件  
  
1.  如果 **“代理配置文件”** 对话框中显示了多个代理的配置文件，请选择一个代理。  
  
2.  单击 **“新建”** 。  
  
3.  在 **“新建代理配置文件”** 初始化对话框中，选择一个作为新配置文件基础的现有配置文件。  
  
4.  在 **“新建代理配置文件”** 对话框的 **“名称”** 和 **“说明”** 文本框中输入相应值。  
  
5.  修改参数以调整配置文件。 若要查看代理的所有参数，请清除 **“仅显示此配置文件中使用的参数”** 复选框。 有关代理参数的信息，请参阅本主题末尾处的链接。  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
###  <a name="to-delete-a-user-defined-profile"></a><a name="Delete_SSMS"></a> 删除用户定义的配置文件  
  
1.  如果 **“代理配置文件”** 对话框中显示了多个代理的配置文件，请选择一个代理。  
  
2.  如果一个配置文件与一个或多个代理相关联，请更改这些代理的配置文件：  
  
    1.  在 **“代理配置文件”** 网格中选择其他配置文件。  
  
    2.  单击 **“更改现有代理”** 。  
  
        > [!NOTE]  
        >  此操作将更改用于现有发布或订阅且属于选定类型的所有代理的配置文件，而不只更改那些使用要删除的配置文件的代理的配置文件。  
  
3.  选择要删除的配置文件，再单击 **“删除”** 。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
###  <a name="to-create-a-new-agent-profile"></a><a name="Create_tsql"></a> 创建一个新的代理配置文件  
  
1.  在分发服务器上，执行 [sp_add_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md)。 指定 \@name，将 \@profile_type 的值指定为 1，并为 \@agent_type 指定下列值之一   ：  
  
    -   **\@profile_type** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     如果此配置文件将成为此复制代理类型的新默认配置文件，请将 \@default 的值指定为 1 。 使用 \@profile_id 输出参数可返回新配置文件的标识符。 这将会使用基于给定代理类型的默认配置文件的配置文件参数集创建新的配置文件。  
  
2.  在已创建新配置文件后，可添加、删除或修改默认参数以自定义该配置文件。  
  
###  <a name="to-modify-an-existing-agent-profile"></a><a name="Modify_tsql"></a> 修改现有代理配置文件  
  
1.  在分发服务器上，执行 [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)。 为 \@agent_type 指定下列值之一：  
  
    -   **\@profile_type** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     这将返回指定类型的代理的所有配置文件。 请记下要更改的配置文件在结果集中的 **profile_id** 值。  
  
2.  在分发服务器上，执行 [sp_help_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)。 为 \@profile_id 指定步骤 1 中的配置文件标识符。 这将返回该配置文件的所有参数。 请记下要从配置文件中修改或删除的任何参数的名称。  
  
3.  若要更改配置文件中的参数值，请执行 [sp_change_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md)。 为 \@profile_id 指定步骤 1 中的配置文件标识符，为 \@parameter_name 指定要更改的参数的名称，并为 \@parameter_value 指定一个新的参数值  。  
  
    > [!NOTE]  
    >  您不能将现有的代理配置文件更改为代理的默认配置文件。 相反，您必须创建一个新的配置文件以作为默认配置文件，如前面的过程所示。  
  
4.  若要从配置文件中删除参数，请执行 [sp_drop_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md)。 为 \@profile_id 指定步骤 1 中的配置文件标识符并为 \@parameter_name 指定要删除的参数的名称 。  
  
5.  若要向配置文件添加新的参数，必须执行下列操作：  
  
    -   查询分发服务器上的 [MSagentparameterlist &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/msagentparameterlist-transact-sql.md) 表以确定可为每个代理类型设置哪些配置文件参数。  
  
    -   在分发服务器上，执行 [sp_add_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md)。 为 \@profile_id 指定步骤 1 中的配置文件标识符，为 \@parameter_name 指定要添加的有效参数的名称，并为 \@parameter_value 指定参数的值  。  
  
###  <a name="to-delete-an-agent-profile"></a><a name="Delete_tsql"></a> 删除代理配置文件  
  
1.  在分发服务器上，执行 [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)。 为 \@agent_type 指定下列值之一：  
  
    -   **\@profile_type** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     这将返回指定类型的代理的所有配置文件。 请记下要删除的配置文件在结果集中的 **profile_id** 值。  
  
2.  在分发服务器上，执行 [sp_drop_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)。 为 \@profile_id 指定步骤 1 中的配置文件标识符。  
  
###  <a name="to-use-agent-profiles-during-synchronization"></a><a name="Synch_tsql"></a> 在同步期间使用代理配置文件  
  
1.  在分发服务器上，执行 [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)。 为 \@agent_type 指定下列值之一：  
  
    -   **\@profile_type** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     这将返回指定类型的代理的所有配置文件。 请记下要使用的配置文件在结果集中的 **profile_name** 值。  
  
2.  如果是从代理作业启动代理，则编辑用于启动该代理的作业步骤，以在 **-ProfileName** 命令行参数后指定在步骤 1 中获得的 **profile_name** 值。 有关详细信息，请参阅[查看和修改复制代理命令提示符参数 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)。  
  
3.  如果是从命令提示符处启动代理，请在 **-ProfileName** 命令行参数后指定在步骤 1 中获得的 **profile_name** 值。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例将为名为 **custom_merge** 的合并代理创建自定义配置文件，更改 **-UploadReadChangesPerBatch** 参数的值，添加新的 **-ExchangeType** 参数，并返回有关创建的配置文件的信息。  
  
 [!code-sql[HowTo#sp_addagentprofileparam](../../../relational-databases/replication/codesnippet/tsql/work-with-replication-ag_1.sql)]  
  
##  <a name="using-rmo"></a><a name="RMOProcedure"></a> 使用 RMO  
  
###  <a name="to-create-a-new-agent-profile"></a><a name="Create_RMO"></a> 创建一个新的代理配置文件  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类的实例创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.AgentProfile> 类的实例。  
  
3.  设置对象的下列属性：  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> - 配置文件的名称。  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.AgentType%2A> - 一个 <xref:Microsoft.SqlServer.Replication.AgentType> 值，该值指定要为其创建该配置文件的复制代理的类型。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> - 步骤 1 中创建的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。  
  
    -   （可选） <xref:Microsoft.SqlServer.Replication.AgentProfile.Description%2A> - 配置文件的说明。  
  
    -   （可选） <xref:Microsoft.SqlServer.Replication.AgentProfile.Default%2A> - 如果默认情况下此 **T:Microsoft.SqlServer.Replication.AgentType** 的所有新代理作业都将使用此配置文件，则将此属性设置为 <xref:Microsoft.SqlServer.Replication.AgentType> 。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.AgentProfile.Create%2A> 方法以在服务器上创建配置文件。  
  
5.  在服务器上存在此配置文件后，可以通过添加、删除或更改复制代理参数的值来对该配置文件进行自定义。  
  
6.  若要将此配置文件分配给现有的复制代理作业，请调用 <xref:Microsoft.SqlServer.Replication.AgentProfile.AssignToAgent%2A> 方法。 为 *distributionDBName* 传递分发数据库的名称并为 *agentID* 传递作业 ID。  
  
###  <a name="to-modify-an-existing-agent-profile"></a><a name="Modify_RMO"></a> 修改现有代理配置文件  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类的实例创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。 传递步骤 1 中创建的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果此方法返回 **false**，请验证分发服务器是否存在。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationServer.EnumAgentProfiles%2A> 方法。 传递一个 <xref:Microsoft.SqlServer.Replication.AgentType> 值以将返回的配置文件缩小至指定类型的复制代理。  
  
5.  从返回的 <xref:Microsoft.SqlServer.Replication.AgentProfile> 中获取所需的 <xref:System.Collections.ArrayList>对象，其中此对象的 <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> 属性与配置文件名相匹配。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.AgentProfile> 的以下方法之一更改配置文件：  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.AddParameter%2A> - 向此配置文件添加支持的参数，其中 *name* 为复制代理参数的名称， *value* 为指定的值。 若要枚举给定代理类型的所有支持的代理参数，请调用 <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameterInfo%2A> 方法。 此方法返回表示所有支持参数的一个 <xref:System.Collections.ArrayList> 对象的 <xref:Microsoft.SqlServer.Replication.AgentProfileParameterInfo> 。  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.RemoveParameter%2A> - 删除配置文件中的某个现有参数，其中 *name* 为复制代理参数的名称。 若要枚举为配置文件定义的所有当前代理参数，请调用 <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameters%2A> 方法。 此方法返回一个 <xref:System.Collections.ArrayList> 对象的 <xref:Microsoft.SqlServer.Replication.AgentProfileParameter> ，表示此配置文件的现有参数。  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.ChangeParameter%2A> - 更改配置文件中现有参数的设置，其中 *name* 为代理参数的名称， *newValue* 为更改参数时的目标值。 若要枚举为配置文件定义的所有当前代理参数，请调用 <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameters%2A> 方法。 此方法返回一个 <xref:System.Collections.ArrayList> 对象的 <xref:Microsoft.SqlServer.Replication.AgentProfileParameter> ，表示此配置文件的现有参数。 若要枚举所有支持的代理参数设置，请调用 <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameterInfo%2A> 方法。 此方法返回 <xref:System.Collections.ArrayList> 对象的 <xref:Microsoft.SqlServer.Replication.AgentProfileParameterInfo> ，表示所有参数的支持值。  
  
###  <a name="to-delete-an-agent-profile"></a><a name="Delete_RMO"></a> 删除代理配置文件  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类的实例创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.AgentProfile> 类的实例。 将 <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> 设置为配置文件的名称，将 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 设置为从步骤 1 中获得的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果此方法返回 **false**，则指定的名称不正确或服务器上不存在该配置文件。  
  
4.  验证 <xref:Microsoft.SqlServer.Replication.AgentProfile.Type%2A> 属性是否设置为指示某个客户配置文件的 <xref:Microsoft.SqlServer.Replication.AgentProfileTypeOption.User>。 您不应该删除 <xref:Microsoft.SqlServer.Replication.AgentProfileTypeOption.System> 的值为 <xref:Microsoft.SqlServer.Replication.AgentProfile.Type%2A>的配置文件。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.AgentProfile.Remove%2A> 方法从服务器中删除此对象表示的用户定义的配置文件。  
  
##  <a name="follow-up-after-changing-agent-parameters"></a><a name="FollowUp"></a> 跟进：在更改代理参数后  
对代理参数所做的更改在下次启动代理时生效。 如果代理连续运行，则必须停止该代理，然后重新启动。 从 SQL Server 2017 CU3 开始，某些代理参数更改将生效，无需重启代理。 
  
## <a name="see-also"></a>另请参阅  
 [复制代理配置文件](../../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)   
 [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)   
 [复制队列读取器代理](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
  
