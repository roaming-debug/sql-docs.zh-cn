---
title: 教程：配置合并复制
description: 本教程介绍如何在 SQL Server 和移动客户端之间配置合并复制。
ms.custom: seo-lt-2019
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: af673514-30c7-403a-9d18-d01e1a095115
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3f29e39d8b66b5adafb4c84de6a8f1df99ce95bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348611"
---
# <a name="tutorial-configure-replication-between-a-server-and-mobile-clients-merge"></a>教程：在服务器和移动客户端之间配置复制（合并）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
合并复制是解决中央服务器和偶尔连接的移动客户端之间的数据移动问题的好方法。 使用复制向导可以轻松地配置和管理合并复制拓扑。 

本教程演示如何为移动客户端配置复制拓扑。 有关合并复制的详细信息，请参阅[合并复制概述](./merge/merge-replication.md)。
  
## <a name="what-you-will-learn"></a>学习内容  
本教程将教你使用合并复制将数据从中央数据库发布到一个或多个移动用户，以便每个用户都能获得唯一筛选的数据子集。 

在本教程中，您将学习如何执行以下操作：
> [!div class="checklist"]
> * 为合并复制配置发布服务器。
> * 为合并发布添加移动订阅服务器。
> * 使订阅与合并发布同步。
  
## <a name="prerequisites"></a>先决条件  
本教程面向熟悉数据库基本数据库操作但复制经验不足的用户。 在开始本教程之前，必须完成[教程：准备 SQL Server 以用于复制](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md)。  
  
若要完成本教程，需要 SQL Server、SQL Server Management Studio (SSMS) 以及 AdventureWorks 数据库： 
  
- 在发布服务器（源）上，安装：  
  
   - 任何版本的 SQL Server，SQL Server Express 或 SQL Server Compact 除外。 这些版本不能充当复制发布服务器。   
   - [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库。 为了增强安全性，默认情况下不会安装示例数据库。  
  
- 在订阅服务器（目标）上，安装任意版本的 SQL Server，SQL Server Express 或 SQL Server Compact 除外。 本教程中创建的发布不支持 SQL Server Express 或 SQL Server Compact。 

- 安装 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
- 安装 [SQL Server 2017 Developer 版本](https://www.microsoft.com/sql-server/sql-server-downloads)。
- 下载 [AdventureWorks 示例数据库](https://github.com/Microsoft/sql-server-samples/releases)。 有关在 SSMS 中还原数据库的说明，请参阅[还原数据库](../backup-restore/restore-a-database-backup-using-ssms.md)。  
 
  
>[!NOTE]
> - 在相差两个版本以上的 SQL Server 实例上不支持复制。 有关详细信息，请参阅[复制拓扑中受支持的 SQL Server 版本](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/)。
> - 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，必须使用属于 sysadmin 固定服务器角色成员的登录名连接到发布服务器和订阅服务器  。 有关此角色的详细信息，请参阅[服务器级别角色](../security/authentication-access/server-level-roles.md)。  
  
  
**学完本教程的估计时间：60 分钟**  
  
## <a name="configure-a-publisher-for-merge-replication"></a>为合并复制配置发布服务器
在本部分中，将使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 创建合并发布，用于在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库中发布 Employee、SalesOrderHeader 和 SalesOrderDetail 表的子集    。 这些表用参数化行筛选器进行筛选，以便每个订阅都包含唯一的数据分区。 你还要将合并代理使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名添加到发布访问列表 (PAL) 中。  
  
### <a name="create-merge-publication-and-define-articles"></a>创建合并发布并定义项目  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后展开服务器节点。  
  
2. 通过在对象资源管理器中右键单击“SQL Server 代理”并选择“启动”，来启动“SQL Server 代理”  。 如果此步骤未启动代理，则需要从“SQL Server 配置管理器”手动启动。  
3. 展开“复制”文件夹，右键单击“本地发布”，再选择“新建发布”    。 新建发布向导将启动：  

   ![用于启动新建发布向导的选择](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
3. 在“发布数据库”页上，选择 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]，然后选择“下一步”   。 

      
4. 在“发布类型”页上，选择“合并发布”，然后选择“下一步”    。  
   
5. 在“订阅服务器类型”页上，确保仅选中了 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或更高版本，然后选择“下一步”   ： 

    ![“发布类型”和“订阅服务器类型”页](media/tutorial-replicating-data-with-mobile-clients/mergerpl.png)
  
   
6. 在“项目”页上，展开“表”节点   。 请选择以下三个表：“Employee”、“SalesOrderHeader”和“SalesOrderDetail”    。 选择“**下一页**”。  

   ![“项目”页上的表选择](media/tutorial-replicating-data-with-mobile-clients/mergearticles.png)

   >[!NOTE]
   > “Employee”表包含数据类型为 hierarchyid 的列 (OrganizationNode)    。 仅 SQL Server 2017 中支持复制该数据类型。 
   >
   > 如果使用低于 SQL Server 2017 的内部版本，屏幕底部会显示一条消息，通知在双向复制中使用此列可能出现数据丢失。 对于此教程，可以忽略此消息。 但是，除非使用受支持的内部版本，否则不应在生产坏境中复制此数据类型。
   > 
   > 有关如何复制 hierarchyid 数据类型的详细信息，请参阅[在复制中使用 Hierarchyid 列](../../t-sql/data-types/hierarchyid-data-type-method-reference.md#using-hierarchyid-columns-in-replicated-tables)  。
    
  
7. 在“筛选表行”页上，选择“添加”，然后选择“添加筛选器”    。  
  
8. 在“添加筛选器”对话框中，在“选择要筛选的表”中选择“Employee (HumanResources)”    。 选择 LoginID 列，再选择向右键以将此列添加到筛选查询的 WHERE 子句，并将 WHERE 子句修改如下  ：  
  
   ```sql 
    WHERE [LoginID] = HOST_NAME()  
   ```  
  
   选择“此表中的行将仅转到一个订阅”，再选择“确定”   。  
 
   ![用于添加筛选器的选择](media/tutorial-replicating-data-with-mobile-clients/mergeaddfilter.png)

    
  
10. 在“筛选表行”页上，选择“Employee (Human Resources)”，选择“添加”，然后选择“添加联接以扩展所选筛选器”     。  
  
    a. 在“添加联接”对话框的“联接的表”下，选择“Sales.SalesOrderHeader”    。 在“手动编写联接语句”框中，按如下所示完成联接语句  ：  
  
    ```sql  
    ON [Employee].[BusinessEntityID] =  [SalesOrderHeader].[SalesPersonID] 
    ```  
  
    b. 在“指定联接选项”中，选择“唯一键”，然后选择“确定”    。

    ![用于将联接添加到筛选器的选择](media/tutorial-replicating-data-with-mobile-clients/mergeaddjoin.png)

  
13. 在“筛选表行”页上，选择“SalesOrderHeader”，选择“添加”，然后选择“添加联接以扩展所选筛选器”     。  
  
    a. 在“添加联接”  对话框的“联接的表”  下，选择“Sales.SalesOrderDetail”  。    
    b. 选择“使用生成器创建语句”  。  
    c. 在“预览”框中，确认联接语句如下所示  ：  
  
    ```sql  
    ON [SalesOrderHeader].[SalesOrderID] = [SalesOrderDetail].[SalesOrderID] 
    ```  
  
    d. 在“指定联接选项”中，选择“唯一键”，然后选择“确定”    。 选择“**下一页**”。 

    ![用于为销售订单添加其他联接的选择](media/tutorial-replicating-data-with-mobile-clients/joinsalestables.png)
  
21. 选中“立即创建快照”，清除“计划在以下时间运行快照代理”，然后选择“下一步”    ：  

    ![用于立即创建快照的选择](media/tutorial-replicating-data-with-mobile-clients/snapshotagent.png)
  
22. 在“代理安全性”页上，选择“安全设置”   。 在“进程帐户”框中输入 <Publisher_Machine_Name>\repl_snapshot，为此帐户提供密码，然后选择“确定”     。 选择“**下一页**”。  

    ![用于设置快照代理安全性的选择](media/tutorial-replicating-data-with-mobile-clients/snapshotagentsecurity.png)
  
23. 在“完成该向导”页的“发布名称”框中，输入 AdvWorksSalesOrdersMerge，然后选择“完成”     ：  

    ![带发布名称的“完成向导”页](media/tutorial-replicating-data-with-mobile-clients/namemergerepl.png)
  
24. 创建发布后，选择“关闭”  。 在“对象资源管理器”中的“复制”节点下，右键单击“本地复制”并选择“刷新”，以查看新的合并复制     。  
  
### <a name="view-the-status-of-snapshot-generation"></a>查看快照的生成状态  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后依次展开服务器节点和“复制”文件夹  。  
  
2. 在“本地发布”文件夹中，右键单击 AdvWorksSalesOrdersMerge，再选择“查看快照代理状态”    ：  

   ![用于查看快照代理状态的选择](media/tutorial-replicating-data-with-mobile-clients/viewsnapshotagentstatus.png)
  
3. 将显示该发布的快照代理作业的当前状态。 继续下一课之前，请确保快照作业已成功完成。  
  
### <a name="add-the-merge-agent-login-to-the-pal"></a>将合并代理登录名添加到 PAL  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后依次展开服务器节点和“复制”文件夹  。  
  
2. 在“本地发布”文件夹中，右键单击 AdvWorksSalesOrdersMerge，再选择“属性”    。  
  
   a. 选择“发布访问列表”页，选择“添加”   。 
  
   b. 在“添加发布访问项”对话框中，选择“<Publisher_Machine_Name>\repl_merge”，然后选择“确定”     。 再一次选择“确定”  。 

   ![用于添加合并代理登录名的选择](media/tutorial-replicating-data-with-mobile-clients/mergepal.png) 

  
有关详细信息，请参阅：  
- [筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md) 
- [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
- [定义项目](../../relational-databases/replication/publish/define-an-article.md)  
  
  
## <a name="create-a-subscription-to-the-merge-publication"></a>创建合并发布的订阅
在此节中，介绍如何将订阅添加到先前创建的合并发布中。 本教程将使用远程订阅服务器 (NODE2\SQL2016)。 然后，为订阅数据库设置权限，并手动生成新订阅的筛选数据快照。   
  
### <a name="add-a-subscriber-for-merge-publication"></a>添加合并发布订阅服务器
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到订阅服务器，并展开服务器节点。 展开“复制”文件夹，右键单击“本地订阅”文件夹，然后选择“新建订阅”    。 新建订阅向导将启动：

   ![用于启动新建订阅向导的选择](media/tutorial-replicating-data-with-mobile-clients/newsub.png)
  
2. 在“发布”页上，选择“发布服务器”列表中的“查找 SQL Server 发布服务器”    。  
  
   在“连接到服务器”对话框的“服务器名称”框中，输入发布服务器实例的名称，然后选择“连接”    。 

   ![用于添加发布服务器的选择](media/tutorial-replicating-data-with-mobile-clients/publication.png)
  
4. 选择“AdvWorksSalesOrdersMerge”，然后选择“下一步”   。  
  
5. 在“合并代理位置”页上，选择“在其订阅服务器上运行每个代理”，然后选择“下一步”    ：  

   ![“在其订阅服务器上运行每个代理”选项](media/tutorial-replicating-data-with-mobile-clients/pullsub.png)
  
6. 在“订阅服务器”页上，选择订阅服务器实例名称  。 然后在“订阅数据库”下，从列表中选择“新建数据库”   。  
  
   在“新建数据库”对话框的“数据库名称”框中输入 SalesOrdersReplica    。 选择“确定”  ，然后选择“下一步”  。 

   ![用于将数据库添加到订阅服务器的选择](media/tutorial-replicating-data-with-mobile-clients/addsubdb.png)
  
8. 在“合并代理安全性”页中，选择省略号 (…) 按钮   。 在“进程帐户”框中输入 <Subscriber_Machine_Name>\repl_merge，并提供此帐户的密码    。 选择“确定”，选择“下一步”，然后再一次选择“下一步”    。  

   ![用于合并代理安全性的选择](media/tutorial-replicating-data-with-mobile-clients/mergeagentsecurity.png)

9. 在“同步计划”页上，将“代理计划”设置为“仅按需运行”    。 选择“**下一页**”。  

   ![用于代理的“仅按需运行”选择](media/tutorial-replicating-data-with-mobile-clients/mergesyncschedule.png)
  
9. 在“初始化订阅”页上，从“初始化时间”列表中选择“首次同步时”，单击“下一步”    。 选择“下一步”，继续进入到“订阅类型”页，然后选择适当的订阅类型   。 本教程使用客户端  。 选择订阅类型后，再一次选择“下一步”  。 

   ![用于首次同步时初始化订阅的选择](media/tutorial-replicating-data-with-mobile-clients/firstsync.png)

10. 在“HOST_NAME 值”页的“HOST_NAME 值”框中，输入值 adventure-works\pamela0    。 然后选择“完成”  。  

    ![“HOST_NAME 值”页](media/tutorial-replicating-data-with-mobile-clients/hostname.png)
  
11. 再一次选择“完成”  。 创建订阅后，选择“关闭”  。  

### <a name="set-server-permissions-at-the-subscriber"></a>在订阅服务器上设置服务器权限  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到订阅服务器。 展开“安全性”，右键单击“登录名”，然后选择“新建登录名”    。  
  
   在“常规”页上，选择“搜索”并在“输入对象名称”框中输入 <Subscriber_ Machine_Name>\repl_merge      。 选择“检查名称”，然后选择“确定”   。 
    
   ![用于设置登录名的选择](media/tutorial-replicating-data-with-mobile-clients/sublogin.png)
  
1. 在“用户映射”页上，依次选择“SalesOrdersReplica”数据库和“db_owner”角色    。 在“安全对象”页上，向“更改跟踪”授予“显式”权限    。 选择“确定”  。

   ![“用户映射”和“安全对象”页](media/tutorial-replicating-data-with-mobile-clients/setdbo.png)
  
### <a name="create-the-filtered-data-snapshot-for-the-subscription"></a>创建订阅的筛选数据快照  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后依次展开服务器节点和“复制”文件夹  。  
  
2. 在“本地发布”文件夹中，右键单击“AdvWorksSalesOrdersMerge”发布，然后选择“属性”    。  
   
   a. 选择“数据分区”页，然后选择“添加”   。   
   b. 在“添加数据分区”对话框的“HOST_NAME 值”框中，输入 adventure-works\pamela0，然后选择“确定”     。  
   c. 选择新添加的分区，选择“立即生成所选快照”，然后选择“确定”   。 

   ![用于添加分区的选择](media/tutorial-replicating-data-with-mobile-clients/partition.png)
  
  
有关详细信息，请参阅：  
- [订阅发布](../../relational-databases/replication/subscribe-to-publications.md)  
- [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)  
- [带有参数化筛选器的合并发布的快照](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  

## <a name="synchronize-the-subscription-to-the-merge-publication"></a>使订阅与合并发布同步

在本部分中，你将使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 启动合并代理以初始化订阅。 还将使用此过程与发布服务器同步。   
  
### <a name="start-synchronization-and-initialize-the-subscription"></a>启动同步并初始化订阅  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到订阅服务器。  
2. 确保 SQL Server 代理正在运行。 如果未运行，在对象资源管理器中右键单击 SQL Server 代理并选择“启动”  。 如果此步骤未启动代理，则需要使用 SQL Server 配置管理器手动启动。 
  
2. 展开“复制”节点  。 在“本地订阅”文件夹中，右键单击 SalesOrdersReplica 数据库中的订阅，再选择“查看同步状态”    。  
  
   选择“启动”以初始化订阅  。 

   ![使用“启动”按钮同步状态](media/tutorial-replicating-data-with-mobile-clients/mergesyncstatus.png)
    
  
  
## <a name="next-steps"></a>后续步骤  
已成功配置用于合并复制的发布服务器和订阅服务器。 你还可以：

1. 在发布服务器或订阅服务器中插入、更新或删除 SalesOrderHeader 或 SalesOrderDetail 表中的数据   。
2. 当网络连接可用于同步发布服务器和订阅服务器之间的数据时，重复此过程。
3. 在其他服务器上查询 SalesOrderHeader 或 SalesOrderDetail 表，查看已复制的更改   。  
  
有关详细信息，请参阅：   
- [使用快照初始化订阅](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [同步数据](../../relational-databases/replication/synchronize-data.md)  
- [同步请求订阅](../../relational-databases/replication/synchronize-a-pull-subscription.md)  
  
  
  
