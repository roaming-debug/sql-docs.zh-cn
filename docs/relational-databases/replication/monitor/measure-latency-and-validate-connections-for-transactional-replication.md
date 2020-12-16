---
title: 测量滞后时间和验证连接（事务性）
description: 了解如何使用 SQL Server Management Studio (SSMS)、Transact-SQL (T-SQL) 或复制管理对象 (RMO) 中的复制监视器来度量 SQL Server 中事务发布的滞后时间并验证其连接。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Replication Monitor, performance
- tracer tokens [SQL Server replication]
- latency [SQL Server replication]
- transactional replication, tracer tokens
- monitoring performance [SQL Server replication], tracer tokens
ms.assetid: 4addd426-7523-4067-8d7d-ca6bae4c9e34
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 558363289f212544d71eee6ad2a4d40a9d5716c8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477868"
---
# <a name="measure-latency-and-validate-connections-for-transactional-replication"></a>为事务复制测量滞后时间和验证连接
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用复制监视器、 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中为事务复制测量滞后时间和验证连接。 事务复制提供了跟踪令牌功能，可以方便地测量事务复制拓扑中的滞后时间和验证发布服务器、分发服务器及订阅服务器之间的连接。 将令牌（少量数据）写入发布数据库的事务日志中，就像标记典型的复制事务一样对其进行标记，使用令牌可以执行以下计算：  
  
-   计算在发布服务器上提交事务和在分发服务器上将相应命令插入分发数据库之间所间隔的时间。  
  
-   计算在分发数据库中插入命令和在订阅服务器上提交相应事务之间所间隔的时间。  
  
 通过这些计算可以回答许多问题，包括：  
  
-   哪个订阅服务器在从发布服务器接收更改时所需的时间最长？  
  
-   在应当收到跟踪令牌的订阅服务器中，哪些订阅服务器（如果有）没有收到跟踪令牌？  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
-   **测量滞后时间和验证连接，使用：**  
  
     [SQL Server 复制监视器](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 停止系统时跟踪令牌也很有用，停止系统的过程涉及停止所有活动并验证所有节点均已收到所有尚未完成的更改。 有关详细信息，请参阅[停止复制拓扑（复制 Transact-SQL 编程）](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md)。  
  
 若要使用跟踪令牌，必须使用特定版本的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]：  
  
-   分发服务器必须是 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本。  
  
-   发布服务器必须是 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本，或者是 Oracle 发布服务器。  
  
-   对于推送订阅，如果订阅服务器为 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 7.0 或更高版本，将从发布服务器、分发服务器和订阅服务器收集跟踪令牌统计信息。  
  
-   对于请求订阅，仅当订阅服务器为 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本时才从订阅服务器收集跟踪令牌统计信息。 如果订阅服务器为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 7.0 或 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)]，将仅从发布服务器和分发服务器收集统计信息。  
  
 还有一些需要注意的问题和限制：  
  
-   订阅必须处于活动状态才能接收跟踪令牌。 订阅如果已经初始化就会处于活动状态。  
  
-   重新初始化会删除相关订阅的所有挂起的跟踪令牌。  
  
-   订阅服务器仅接收在其初始同步之后创建的跟踪令牌。  
  
-   重新发布订阅服务器不转发跟踪令牌。  
  
-   故障转移到辅助实例后，复制监视器无法调整 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的发布实例的名称，将继续在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的原始主实例名称下显示复制信息。 在故障转移之后，无法使用复制监视器输入跟踪令牌，但是可以在复制监视器中显示使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]在新发布服务器上输入的跟踪令牌。  
  
##  <a name="using-sql-server-replication-monitor"></a><a name="SSMSProcedure"></a> 使用 SQL Server 复制监视器  
 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
#### <a name="to-insert-a-tracer-token-and-view-information-on-the-token"></a>插入跟踪令牌并查看有关令牌的信息  
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。  
  
2.  单击 **“跟踪令牌”** 选项卡。  
  
3.  单击 **“插入跟踪器”** 。  
  
4.  在以下列中查看跟踪令牌的运行时间： **“发布服务器到分发服务器”** 、 **“分发服务器到订阅服务器”** 、 **“总滞后时间”** 。 值为 **“挂起”** 表示令牌尚未到达指定点。  
  
#### <a name="to-view-information-on-a-tracer-token-inserted-previously"></a>查看有关以前插入的跟踪令牌的信息  
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。  
  
2.  单击 **“跟踪令牌”** 选项卡。  
  
3.  从 **“插入时间”** 下拉列表中选择时间。  
  
4.  在以下列中查看跟踪令牌的运行时间： **“发布服务器到分发服务器”** 、 **“分发服务器到订阅服务器”** 、 **“总滞后时间”** 。 值为 **“挂起”** 表示令牌尚未到达指定点。  
  
    > [!NOTE]  
    >  跟踪令牌信息的保留时间与其他历史数据的保留时间一样长，该时间由分发数据库的历史记录保持期来控制。 若要了解如何更改分发数据库属性，请参阅[查看和修改分发服务器和发布服务器属性](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-post-a-tracer-token-to-a-transactional-publication"></a>若要将跟踪令牌发布到事务发布  
  
1.  （可选）在发布服务器上，对发布数据库执行 [sp_helppublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)。 请验证发布是否存在且状态是否处于活动状态。  
  
2.  （可选）在发布服务器上，对发布数据库执行 [sp_helpsubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)。 请验证订阅是否存在且状态是否处于活动状态。  
  
3.  在发布服务器上，对发布数据库执行 [sp_posttracertoken &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-posttracertoken-transact-sql.md)，同时指定“\@publication”。 记录“\@tracer_token_id”输出参数的值。  
  
#### <a name="to-determine-latency-and-validate-connections-for-a-transactional-publication"></a>确定事务发布的滞后时间并验证连接  
  
1.  使用前一过程将跟踪令牌发送到发布。  
  
2.  在发布服务器上，对发布数据库执行 [sp_helptracertokens &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md)，同时指定“\@publication”。 这会返回发布到此发布的所有跟踪令牌的列表。 请记录结果集中所需的 **tracer_id** 。  
  
3.  在发布服务器上，对发布数据库执行 [sp_helptracertokenhistory &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md)，同时指定“\@publication”，以及第 2 步中“\@tracer_id”对应的跟踪令牌 ID。 这会返回所选跟踪令牌的滞后时间信息。  
  
#### <a name="to-remove-tracer-tokens"></a>删除跟踪令牌  
  
1.  在发布服务器上，对发布数据库执行 [sp_helptracertokens &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md)，同时指定“\@publication”。 这会返回发布到此发布的所有跟踪令牌的列表。 请记录结果集中要删除的跟踪令牌的 **tracer_id** 。  
  
2.  在发布服务器的发布数据库中，执行 [sp_deletetracertokenhistory &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md)，同时指定“publication”\@，并为 `@tracer_id` 指定第 2 步中要删除的跟踪 ID。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 本示例发送一个跟踪令牌记录并使用返回的已发送跟踪令牌的 ID 来查看滞后时间信息。  
  
 [!code-sql[HowTo#sp_tracertokens](../../../relational-databases/replication/codesnippet/tsql/measure-latency-and-vali_1.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
  
#### <a name="to-post-a-tracer-token-to-a-transactional-publication"></a>若要将跟踪令牌发布到事务发布  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例。  
  
3.  设置发布的 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 3 中的发布属性定义不正确，或者此发布不存在。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.TransPublication.PostTracerToken%2A> 方法。 此方法将跟踪令牌插入到发布的事务日志中。  
  
#### <a name="to-determine-latency-and-validate-connections-for-a-transactional-publication"></a>确定事务发布的滞后时间并验证连接  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.PublicationMonitor> 类的实例。  
  
3.  设置 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A>、 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>、 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>和 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A> 属性，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 3 中发布监视器的属性定义错误或此发布不存在。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> 方法。 将返回的 <xref:System.Collections.ArrayList> 对象转换为 <xref:Microsoft.SqlServer.Replication.TracerToken> 对象数组。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokenHistory%2A> 方法。 为步骤 5 中的跟踪令牌传递 <xref:Microsoft.SqlServer.Replication.TracerToken.TracerTokenId%2A> 的值。 这样会将所选跟踪令牌的滞后信息作为 <xref:System.Data.DataSet> 对象返回。 如果返回了所有跟踪令牌信息，则发布服务器与分发服务器之间的连接和分发服务器与订阅服务器之间的连接同时存在，并且复制拓扑工作正常。  
  
#### <a name="to-remove-tracer-tokens"></a>删除跟踪令牌  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.PublicationMonitor> 类的实例。  
  
3.  设置 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A>、 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>、 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>和 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A> 属性，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 3 中发布监视器的属性定义错误或此发布不存在。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> 方法。 将返回的 <xref:System.Collections.ArrayList> 对象转换为 <xref:Microsoft.SqlServer.Replication.TracerToken> 对象数组。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.PublicationMonitor.CleanUpTracerTokenHistory%2A> 方法。 传递以下值之一：  
  
    -   步骤 5 中跟踪令牌的 <xref:Microsoft.SqlServer.Replication.TracerToken.TracerTokenId%2A> 。 这将删除选定令牌的信息。  
  
    -   <xref:System.DateTime> 对象。 这会删除早于指定日期和时间的所有令牌的信息。  
  
  
