---
title: 教程：配置事务复制
description: 本教程介绍如何在两个完全连接的 SQL Server 之间配置事务复制。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- replication [SQL Server], tutorials
- wizards [SQL Server replication]
ms.assetid: 7b18a04a-2c3d-4efe-a0bc-c3f92be72fd0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e2a27d4fccfb24aff29668651ea914e11429910f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347126"
---
# <a name="tutorial-configure-replication-between-two-fully-connected-servers-transactional"></a>教程：在两个完全连接的服务器之间配置复制（事务）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
事务复制是在持续连接的服务器之间实现数据移动的好方法。 使用复制向导可以轻松地配置和管理复制拓扑。 

本教程演示如何为持续连接的服务器配置事务复制拓扑。 若要深入了解事务复制的工作机制，请参阅[事务复制概述](./transactional/transactional-replication.md)。 
  
## <a name="what-you-will-learn"></a>学习内容  
本教程将演示如何使用事务复制将数据从一个数据库发布到另一个数据库。  

在本教程中，您将学习如何执行以下操作：
> [!div class="checklist"]
> * 通过事务复制创建发布服务器。
> * 创建事务发布服务器的订阅服务器。
> * 验证订阅和测量滞后时间。
  
  
## <a name="prerequisites"></a>先决条件  
本教程适用于熟悉数据库基本操作但复制经验不足的用户。 在开始本教程之前，必须完成[教程：准备 SQL Server 以用于复制](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md)。  
  
若要完成本教程，需要 SQL Server、SQL Server Management Studio (SSMS) 以及 AdventureWorks 数据库：  
  
- 在发布服务器（源）上，安装：  
  
   - 任意版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，但 SQL Server Express 或 SQL Server Compact 除外。 这些版本不能充当复制发布服务器。   
   - [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] 示例数据库。 为了增强安全性，默认情况下不会安装示例数据库。  
  
- 订阅服务器（目标）上，安装任意版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，[!INCLUDE[ssEW](../../includes/ssew-md.md)] 除外。 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 不能充当事务复制中的订阅服务器。  
  
- 安装 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
- 安装 [SQL Server 2017 Developer 版本](https://www.microsoft.com/sql-server/sql-server-downloads)。
- 下载 [AdventureWorks 示例数据库](https://github.com/Microsoft/sql-server-samples/releases)。 有关在 SSMS 中还原数据库的说明，请参阅[还原数据库](../backup-restore/restore-a-database-backup-using-ssms.md)。 
 
>[!NOTE]
> - 在相差两个版本以上的 SQL Server 实例上不支持复制。 有关详细信息，请参阅[复制拓扑中受支持的 SQL Server 版本](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/)。
> - 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，必须使用属于 sysadmin 固定服务器角色成员的登录名连接到发布服务器和订阅服务器  。 有关此角色的详细信息，请参阅[服务器级别角色](../security/authentication-access/server-level-roles.md)。  
  
  
**学完本教程的估计时间：60 分钟**  
  
## <a name="configure-the-publisher-for-transactional-replication"></a>配置事务复制的发布服务器
在本节中，使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 创建一个事务发布，以便在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库中发布“Product”  表的筛选子集。 你还要将分发代理使用的 SQL Server 登录名添加到发布访问列表 (PAL)。


### <a name="create-a-publication-and-define-articles"></a>创建发布并定义项目
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后展开服务器节点。  
  
2. 右键单击“SQL Server 代理”  ，然后选择“开始”  。 应在创建发布之前运行 SQL Server 代理。 如果该步骤未启动代理，则需要从“SQL Server 配置管理器”手动启动。 
3. 展开“复制”文件夹，右键单击“本地发布”文件夹，然后选择“新建发布”    。 此步骤会启动新建发布向导：  

   ![启动新建发布向导的选择](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
  
3. 在“发布数据库”页上，选择 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]，然后选择“下一步”   。  
  
4. 在“发布类型”  页上，选择“事务发布”  ，然后选择“下一步”  ：  

   ![选择发布类型的“发布类型”页](media/tutorial-replicating-data-between-continuously-connected-servers/tranrepl.png)
  
5. 在“项目”  页上，展开“表”  节点，选中“Product”  复选框。 然后展开“Product”  ，并清除“ListPrice”  和“StandardCost”  旁边的复选框。 选择“**下一页**”。  

   ![“项目”页和选中的要发布的项目](media/tutorial-replicating-data-between-continuously-connected-servers/replarticles.png)
  
6. 在“筛选表行”  页上，选择“添加”  。   
  
7. 在“添加筛选器”  对话框中，选择“SafetyStockLevel”  列。 选择向右箭头，将此列添加到筛选查询的 Filter 语句 WHERE 子句。 然后在 WHERE 子句修饰符中手动键入以下内容：  
  
   ```sql  
   WHERE [SafetyStockLevel] < 500  
   ```
  
   ![“筛选器表流”页和“添加筛选器”对话框](media/tutorial-replicating-data-between-continuously-connected-servers/filter.png)
  
8. 选择“确定”  ，然后选择“下一步”  。  
  
9. 选中“立即创建快照并使快照保持可用状态，以初始化订阅”复选框，选择“下一步”   ：  

   ![选中复选框的“快照代理”页](media/tutorial-replicating-data-between-continuously-connected-servers/snapshot.png)
  
10. 在“代理安全性”  页上，清除“使用快照代理的安全设置”  复选框。   
  
    选择快照代理的“安全设置”  。 在“进程帐户”框中输入 <Publisher_Machine_Name>\repl_snapshot，为此帐户提供密码，然后选择“确定”     。  

    ![“代理安全性”页和“快照代理安全性”对话框](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagentsecurity.png)
  
12. 重复上一步，将 <Publisher_Machine_Name  >\repl_logreader  设置为日志读取器代理的进程帐户。 然后选择“确定”。   

    ![“日志读取器代理安全性”对话框和“代理安全性”页](media/tutorial-replicating-data-between-continuously-connected-servers/logreaderagentsecurity.png)   

  
13. 在“完成向导”  页的“发布名称”  框中，键入“AdvWorksProductTrans”  ，然后选择“完成”  ：  

    ![带发布名称的“完成向导”页](media/tutorial-replicating-data-between-continuously-connected-servers/advworksproducttrans.png)
  
14. 创建发布后，选择“关闭”完成该向导  。 

尝试创建发布时，如果未运行 SQL Server 代理，则可能会遇到以下错误。 此错误说明已成功创建发布，但快照代理无法启动。 如果发生这种情况，需要启动 SQL Server 代理，然后手动启动快照代理。 下一部分会提供说明。 

![快照代理未能启动的警告](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagenterror.png)
    
  
### <a name="view-the-status-of-snapshot-generation"></a>查看快照的生成状态  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后依次展开服务器节点和“复制”文件夹  。  
  
2. 在“本地发布”文件夹中，右键单击 AdvWorksProductTrans，然后选择“查看快照代理状态”    ：  
   ![用于查看“快照代理”状态的快捷菜单上的命令](media/tutorial-replicating-data-between-continuously-connected-servers/viewsnapshot.png)
  
3. 将显示该发布的快照代理作业的当前状态。 继续下一节之前，请确保快照作业已成功完成。
          
如果创建发布时未运行 SQL Server 代理，检查发布的“快照代理状态”时，将看到快照代理处于“从未运行”状态。 如果是这种情况，则选择“启动”，启动快照代理  ： 

![“启动”按钮和状态消息中的更改会显示“快照代理”已经运行](media/tutorial-replicating-data-between-continuously-connected-servers/startsnapshotagent.png)
     
如果看到此处的错误，请参阅[对快照代理错误进行故障排除](troubleshoot-tran-repl-errors.md#find-errors-with-the-snapshot-agent)。


  
### <a name="add-the-distribution-agent-login-to-the-pal"></a>将分发代理登录名添加到 PAL  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后依次展开服务器节点和“复制”文件夹  。  
  
2. 在“本地发布”文件夹中，右键单击 AdvWorksProductTrans，然后选择“属性”    。  将显示“发布属性”  对话框。    
  
   a. 选择“发布访问列表”页，选择“添加”   。  
   b. 在“添加发布访问项”  对话框中，选择 <Publisher_Machine_Name  >\repl_distribution  ，然后选择“确定”  。
   
   ![将登录添加到发布访问列表的选择](media/tutorial-replicating-data-between-continuously-connected-servers/tranreplproperties.png)

有关详细信息，请参阅[复制编程概念](../../relational-databases/replication/concepts/replication-programming-concepts.md)。  
  

## <a name="create-a-subscription-to-the-transactional-publication"></a>创建事务发布的订阅
在本部分中，你可将订阅服务器添加到以前创建的发布。 本教程使用远程订阅服务器 (NODE2\SQL2016)，但你也可在本地将订阅添加到发布服务器。 

### <a name="create-the-subscription"></a>创建订阅  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后依次展开服务器节点和“复制”文件夹  。  
  
2. 在“本地发布”文件夹中，右键单击“AdvWorksProductTrans”发布，然后选择“新建订阅”    。 新建订阅向导将启动： 
 
   ![用于启动新建订阅向导的选择](media/tutorial-replicating-data-between-continuously-connected-servers/newsub.png)     
  
3. 在“发布”  页上，选择“AdvWorksProductTrans”  ，然后选择“下一步”  ：  

   ![已选中发布的发布页](media/tutorial-replicating-data-between-continuously-connected-servers/selectpub.png)
  
4. 在“分发代理位置”  页上，选择“在分发服务器上运行所有代理”  ，然后选择“下一步”  。  有关请求订阅和推送订阅的详细信息，请参阅[订阅发布](./subscribe-to-publications.md)。

   ![已选中“在分发服务器上运行所有代理”的“分发代理位置”页](media/tutorial-replicating-data-between-continuously-connected-servers/runagentsatdist.png)
  
5. 在“订阅服务器”  页上，如果未显示订阅服务器实例的名称，选择“添加订阅服务器”  ，然后从下拉列表选择“添加 SQL Server 订阅服务器”  。 此步骤会打开“连接到服务器”  对话框。 输入订阅服务器实例名称，然后选择“连接”  。  
    
   添加订阅服务器后，选中订阅服务器实例名称旁边的复选框。 然后在“订阅数据库”  下选择“新建数据库”  。   

   ![带有用于添加订阅服务器选择的“订阅服务器”页](media/tutorial-replicating-data-between-continuously-connected-servers/addsub.png)

7. 将出现“新建数据库”  对话框。 在“数据库名称”框中输入 ProductReplica，选择“确定”，然后选择“下一步”     ： 
  
   ![输入订阅数据库的名称](media/tutorial-replicating-data-between-continuously-connected-servers/productreplica.png)
  
8. 在“分发代理安全性”页中，选择省略号 (…) 按钮   。 在“进程帐户”  框中输入 <Publisher_Machine_Name  >\repl_distribution  ，输入此帐户的密码，选择“确定”  ，然后选择“下一步”  。

   ![“分发代理安全性”对话框中的分发帐户信息](media/tutorial-replicating-data-between-continuously-connected-servers/adddistaccount.png)
  
9. 选择“完成”以接受其余页中的默认值并完成向导  。  
  
### <a name="set-database-permissions-at-the-subscriber"></a>在订阅服务器上设置数据库权限  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到订阅服务器。 展开“安全性”，右键单击“登录名”，然后选择“新建登录名”    。     
  
   a. 在“常规”  页的“登录名”  下，选择“搜索”  ，并添加 <Subscriber_Machine_Name  >\repl_distribution  的登录名。

   b. 在“用户映射”  页上，将登录名“db_owner”  成员身份授予“ProductReplica”  数据库。 

   ![配置订阅服务器上的登录的选择](media/tutorial-replicating-data-between-continuously-connected-servers/loginforsub.png)

2. 选择“确定”，即可关闭“新建登录名”对话框   。 

  
### <a name="view-the-synchronization-status-of-the-subscription"></a>查看订阅的同步状态  
  
1. 连接到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的发布服务器。 展开服务器节点，然后展开“复制”  文件夹。  
  
2. 在“本地发布”文件夹中，展开“AdvWorksProductTrans”发布，右键单击“ProductReplica”数据库中的订阅，然后选择“查看同步状态”     。 系统将显示订阅的当前同步状态：

   ![打开“查看同步状态”对话框的选择](media/tutorial-replicating-data-between-continuously-connected-servers/viewsyncstatus.png)
3. 如果订阅未在“AdvWorksProductTrans”  下出现，请选择 F5 键刷新列表。  
  
有关详细信息，请参阅：  
- [使用快照初始化订阅](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)  
- [订阅发布](../../relational-databases/replication/subscribe-to-publications.md)  

## <a name="measure-replication-latency"></a>测量复制滞后时间
在本节中，将用跟踪令牌来验证更改是否已复制到订阅服务器并确定滞后时间。 滞后时间是发布服务器处所做的更改显示到订阅服务器所需的时间。
  
1. 连接到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的发布服务器。 展开服务器节点，右键单击“复制”  文件夹，然后选择“启动复制监视器”  ：

   ![快捷菜单上的“启动复制监视器”命令](media/tutorial-replicating-data-between-continuously-connected-servers/launchreplmonitor.png)

2. 在左窗格中展开发布服务器组，展开发布服务器实例，然后选择“AdvWorksProductTrans”  发布。  
  
   a. 选择“跟踪令牌”选项卡  。  
   b. 选择“插入跟踪器”  。    
   c. 在以下各列中查看跟踪令牌的占用时间：“发布服务器到分发服务器”、“分发服务器到订阅服务器”、“总滞后时间”    。 值为“挂起”  表示令牌尚未到达指定点。

   ![跟踪令牌的信息](media/tutorial-replicating-data-between-continuously-connected-servers/tracertoken.png)


有关详细信息，请参阅： 
- [为事务复制测量滞后时间和验证连接](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)
- [查找事务复制代理错误](troubleshoot-tran-repl-errors.md)


## <a name="next-steps"></a>后续步骤
已成功配置用于事务复制的发布服务器和订阅服务器。 现在可以在发布服务器中的“Product”  表中插入、更新或删除数据。 然后可以在订阅服务器上查询“Product”  表，以查看已复制的更改。 

下一篇文章介绍如何配置合并复制：  

> [!div class="nextstepaction"]
> [教程：在服务器和移动客户端之间配置复制（合并）](tutorial-replicating-data-with-mobile-clients.md)