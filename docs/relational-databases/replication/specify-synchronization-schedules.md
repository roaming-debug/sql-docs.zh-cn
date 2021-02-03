---
title: 指定同步计划 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或复制管理对象在 SQL Server 中指定同步计划。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [SQL Server replication], synchronizing
- scheduling synchronization [SQL Server replication]
- synchronization [SQL Server replication], schedules
- replication [SQL Server], synchronization
ms.assetid: 97f2535b-ec19-4973-823d-bcf3d5aa0216
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a0dac3922e006d34bf1f93c617ae70d79aff27f6
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250590"
---
# <a name="specify-synchronization-schedules"></a>指定同步计划
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中指定同步计划。 在创建订阅时，可以定义同步计划以控制何时运行订阅的复制代理。 如果没有指定计划参数，则订阅将使用默认计划。  
  
 订阅由分发代理（对于快照复制和事务复制）或合并代理（对于合并复制）进行同步。 代理可以连续运行、按需运行或按计划运行。  
  
 **本主题内容**  
  
-   **指定同步计划，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在新建订阅向导的 **“同步计划”** 页上指定同步计划。 有关访问此向导的详细信息，请参阅 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md) 和 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
 在 **“作业计划属性”** 对话框中修改同步计划，该对话框位于 **的** “作业” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 文件夹和复制监视器的代理详细信息窗口中。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
 如果从 **“作业”** 文件夹指定计划，请使用下表确定代理作业的名称。  
  
|代理|作业名称|  
|-----------|--------------|  
|请求订阅的合并代理|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<integer>**|  
|推送订阅的合并代理|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
|推送订阅的分发代理|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>** <sup>1</sup>|  
|请求订阅的分发代理|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<GUID>** <sup>2</sup>|  
|非 SQL Server 订阅服务器的推送订阅的分发代理|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
  
 <sup>1</sup> 对于 Oracle 发布的推送订阅，它是 **\<Publisher>-\<Publisher**>，而不是 \<Publisher>-\<PublicationDatabase>  
  
 <sup>2</sup> 对于 Oracle 发布的请求订阅，它是 **\<Publisher>-\<DistributionDatabase**>，而不是 \<Publisher>-\<PublicationDatabase>  
  
#### <a name="to-specify-synchronization-schedules"></a>指定同步计划  
  
1.  在新建订阅向导的“同步计划”页上，从“代理计划”下拉列表中为要创建的每个订阅选择以下值之一：   
  
    -   **连续运行**  
  
    -   **仅按需运行**  
  
    -   **\<Define Schedule...>**  
  
2.  如果选择“\<Define Schedule...>”，请在“作业计划属性”对话框中指定一个计划，然后单击“确定”。    
  
3.  完成向导。  

#### <a name="to-modify-a-synchronization-schedule-for-a-push-subscription-in-replication-monitor"></a>在复制监视器中修改推送订阅的同步计划  
  
1.  在复制监视器的左窗格中依次展开发布服务器组、发布服务器，然后单击某个发布。  
  
2.  单击 **“所有订阅”** 选项卡。  
  
3.  右键单击订阅，然后单击 **“查看详细信息”** 。  
  
4.  在“订阅 <SubscriptionName>”窗口中，单击“操作”，再单击“\<AgentName> 作业属性”  。  
  
5.  在“作业属性 - \<JobName>”对话框的“计划”页上，单击“编辑”。    
  
6.  在 **“作业计划属性”** 对话框中，从 **“计划类型”** 下拉列表中选择一个值：  
  
    -   若要指定代理应连续运行，请选择 **“SQL Server 代理启动时自动启动”** 。  
  
    -   若要指定代理应按计划运行，请选择 **“重复执行”** 。  
  
    -   若要指定代理应按需运行，请选择 **“执行一次”** 。  
  
7.  如果选择 **“重复执行”** ，请为代理指定计划。  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-push-subscription-in-management-studio"></a>在 Management Studio 中修改推送订阅的同步计划  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中连接到分发服务器，然后展开服务器节点。  
  
2.  展开 **“SQL Server 代理”** 文件夹，再展开 **“作业”** 文件夹。  
  
3.  右键单击与订阅相关联的分发代理或合并代理的作业，再单击 **“属性”** 。  
  
4.  在“作业属性 - \<JobName>”对话框的“计划”页上，单击“编辑”。    
  
5.  在 **“作业计划属性”** 对话框中，从 **“计划类型”** 下拉列表中选择一个值：  
  
    -   若要指定代理应连续运行，请选择 **“SQL Server 代理启动时自动启动”** 。  
  
    -   若要指定代理应按计划运行，请选择 **“重复执行”** 。  
  
    -   若要指定代理应按需运行，请选择 **“执行一次”** 。  
  
6.  如果选择 **“重复执行”** ，请为代理指定计划。  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-pull-subscription-in-management-studio"></a>在 Management Studio 中修改请求订阅的同步计划  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中连接到订阅服务器，然后展开服务器节点。  
  
2.  展开 **“SQL Server 代理”** 文件夹，再展开 **“作业”** 文件夹。  
  
3.  右键单击与订阅相关联的分发代理或合并代理的作业，再单击 **“属性”** 。  
  
4.  在“作业属性 - \<JobName>”对话框的“计划”页上，单击“编辑”。    
  
5.  在 **“作业计划属性”** 对话框中，从 **“计划类型”** 下拉列表中选择一个值：  
  
    -   若要指定代理应连续运行，请选择 **“SQL Server 代理启动时自动启动”** 。  
  
    -   若要指定代理应按计划运行，请选择 **“重复执行”** 。  
  
    -   若要指定代理应按需运行，请选择 **“执行一次”** 。  
  
6.  如果选择 **“重复执行”** ，请为代理指定计划。  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 您可以使用复制存储过程以编程的方式定义同步计划。 所使用的存储过程取决于复制类型和订阅类型（请求或推送）。  
  
 计划由以下计划参数定义，而计划的行为从 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md) 继承：  
  
-   \@frequency_type - 计划代理时所用的频率类型。  
  
-   \@frequency_interval - 星期几运行代理。  
  
-   \@frequency_relative_interval - 计划每月运行代理的给定月份的星期。  
  
-   \@frequency_recurrence_factor - 同步之间发生的频率类型单位数值。  
  
-   \@frequency_subday - 一天内多次运行代理时的频率单位。  
  
-   \@frequency_subday_interval - 一天内多次运行代理时运行之间的频率单位数值。  
  
-   \@active_start_time_of_day - 在给定日将要开始运行代理的最早时间。  
  
-   \@active_end_time_of_day - 在给定日将要开始运行代理的最晚时间。  
  
-   \@active_start_date - 代理计划生效的第一天。  
  
-   \@active_end_date - 代理计划生效的最后一天。  
  
#### <a name="to-define-the-synchronization-schedule-for-a-pull-subscription-to-a-transactional-publication"></a>为事务发布的请求订阅定义同步计划  
  
1.  对事务发布创建一个新的请求订阅。 有关详细信息，请参阅 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
2.  在订阅服务器上，执行 [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)。 指定 \@publisher、\@publisher_db 和 \@publication，并为 \@job_name 和 \@password 指定订阅服务器的分发代理运行时使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 凭据。 指定上面详细说明的同步参数，为同步订阅的分发代理作业定义计划。  
  
#### <a name="to-define-the-synchronization-schedule-for-a-push-subscription-to-a-transactional-publication"></a>为事务发布的推送订阅定义同步计划  
  
1.  对事务发布创建一个新的推送订阅。 有关详细信息，请参阅 [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)。  
  
2.  在订阅服务器上，执行 [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)。 指定 \@subscriber、\@subscriber_db 和 \@publication，并为 \@job_name 和 \@password 指定订阅服务器的分发代理运行时使用的 Windows 凭据。 指定上面详细说明的同步参数，为同步订阅的分发代理作业定义计划。  
  
#### <a name="to-define-the-synchronization-schedule-for-a-pull-subscription-to-a-merge-publication"></a>为合并发布的请求订阅定义同步计划  
  
1.  对合并发布创建一个新的请求订阅。 有关详细信息，请参阅 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
2.  在订阅服务器上，执行 [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)。 指定 \@publisher、\@publisher_db 和 \@publication，并为 \@job_name 和 \@password 指定订阅服务器的合并代理运行时使用的 Windows 凭据。 指定上面详细说明的同步参数，为同步订阅的合并代理作业定义计划。  
  
#### <a name="to-define-the-synchronization-schedule-for-a-push-subscription-to-a-merge-publication"></a>为合并发布的推送订阅定义同步计划  
  
1.  对合并发布创建一个新的推送订阅。 有关详细信息，请参阅 [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)。  
  
2.  在订阅服务器上，执行 [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)。 指定 \@subscriber、\@subscriber_db 和 \@publication，并为 \@job_name 和 \@password 指定订阅服务器的合并代理运行时使用的 Windows 凭据。 指定上面详细说明的同步参数，为同步订阅的合并代理作业定义计划。  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 复制使用 SQL Server 代理为定期发生的活动计划作业（如快照生成和订阅同步）。 可以编程的方式使用复制管理对象 (RMO) 为复制代理作业指定计划。  
  
> [!NOTE]  
>  创建一个订阅并将 **false** （请求订阅的默认行为）的值指定为 **CreateSyncAgentByDefault** 时，不会创建代理作业，并忽略计划属性。 在这种情况下，必须由应用程序确定同步计划。 有关详细信息，请参阅 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) 和 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)。  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-push-subscription-to-a-transactional-publication"></a>在创建事务发布的推送订阅时定义一个复制代理计划  
  
1.  为要创建的订阅创建一个 <xref:Microsoft.SqlServer.Replication.TransSubscription> 类的实例。 有关详细信息，请参阅 [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)。  
  
2.  在调用 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>之前，设置 <xref:Microsoft.SqlServer.Replication.Subscription.AgentSchedule%2A> 属性的下列一个或多个字段：  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> - 计划代理时所使用的频率类型（如每天或每周）。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> - 星期几运行代理。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> - 计划每月运行代理的给定月份的星期。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> - 同步之间发生的频率类型单位数值。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> - 一天内多次运行代理时的频率单位。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> - 一天内多次运行代理时运行之间的频率单位数值。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> - 在给定日期内代理运行开始的最早时间。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> - 在给定日期内代理运行开始的最晚时间。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> - 代理计划有效的起始日期。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> - 代理计划有效的结束日期。  
  
    > [!NOTE]  
    >  如果未指定上述某个属性，将设置为默认值。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法创建订阅。  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-pull-subscription-to-a-transactional-publication"></a>在创建事务发布的请求订阅时定义一个复制代理计划  
  
1.  为要创建的订阅创建一个 <xref:Microsoft.SqlServer.Replication.TransPullSubscription> 类的实例。 有关详细信息，请参阅 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
2.  在调用 <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A>之前，设置 <xref:Microsoft.SqlServer.Replication.PullSubscription.AgentSchedule%2A> 属性的下列一个或多个字段：  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> - 计划代理时所使用的频率类型（如每天或每周）。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> - 星期几运行代理。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> - 计划每月运行代理的给定月份的星期。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> - 同步之间发生的频率类型单位数值。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> - 一天内多次运行代理时的频率单位。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> - 一天内多次运行代理时运行之间的频率单位数值。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> - 在给定日期内代理运行开始的最早时间。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> - 在给定日期内代理运行开始的最晚时间。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> - 代理计划有效的起始日期。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> - 代理计划有效的结束日期。  
  
    > [!NOTE]  
    >  如果未指定上述某个属性，将设置为默认值。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> 方法创建订阅。  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-pull-subscription-to-a-merge-publication"></a>在创建合并发布的请求订阅时定义一个复制代理计划  
  
1.  为要创建的订阅创建一个 <xref:Microsoft.SqlServer.Replication.MergePullSubscription> 类的实例。 有关详细信息，请参阅 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
2.  在调用 <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A>之前，设置 <xref:Microsoft.SqlServer.Replication.PullSubscription.AgentSchedule%2A> 属性的下列一个或多个字段：  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> - 计划代理时所使用的频率类型（如每天或每周）。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> - 星期几运行代理。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> - 计划每月运行代理的给定月份的星期。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> - 同步之间发生的频率类型单位数值。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> - 一天内多次运行代理时的频率单位。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> - 一天内多次运行代理时运行之间的频率单位数值。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> - 在给定日期内代理运行开始的最早时间。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> - 在给定日期内代理运行开始的最晚时间。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> - 代理计划有效的起始日期。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> - 代理计划有效的结束日期。  
  
    > [!NOTE]  
    >  如果未指定上述某个属性，将设置为默认值。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> 方法创建订阅。  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-push-subscription-to-a-merge-publication"></a>在创建合并发布的推送订阅时定义一个复制代理计划  
  
1.  为要创建的订阅创建一个 <xref:Microsoft.SqlServer.Replication.MergeSubscription> 类的实例。 有关详细信息，请参阅 [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)。  
  
2.  在调用 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>之前，设置 <xref:Microsoft.SqlServer.Replication.Subscription.AgentSchedule%2A> 属性的下列一个或多个字段：  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> - 计划代理时所使用的频率类型（如每天或每周）。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> - 星期几运行代理。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> - 计划每月运行代理的给定月份的星期。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> - 同步之间发生的频率类型单位数值。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> - 一天内多次运行代理时的频率单位。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> - 一天内多次运行代理时运行之间的频率单位数值。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> - 在给定日期内代理运行开始的最早时间。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> - 在给定日期内代理运行开始的最晚时间。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> - 代理计划有效的起始日期。  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> - 代理计划有效的结束日期。  
  
    > [!NOTE]  
    >  如果未指定上述某个属性，将设置为默认值。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法创建订阅。  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 此示例创建一个合并发布的推送订阅并指定同步订阅计划。  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>另请参阅  
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [同步推送订阅](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [同步请求订阅](../../relational-databases/replication/synchronize-a-pull-subscription.md)   
 [同步数据](../../relational-databases/replication/synchronize-data.md)  
  
  
