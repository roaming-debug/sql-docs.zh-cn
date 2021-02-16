---
title: 教程：为复制做准备
description: 在本教程中，你将了解如何通过创建 Windows 帐户、准备快照文件夹和配置分发来准备发布服务器、分发服务器和订阅服务器以进行复制。
ms.custom: seo-lt-2019
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: ce30a095-2975-4387-9377-94a461ac78ee
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7506b3b57d81187269c26fae3cafc2d3cdbc6628
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348618"
---
# <a name="tutorial-prepare-sql-server-for-replication-publisher-distributor-subscriber"></a>教程：为复制准备 SQL Server（发布服务器、分发服务器、订阅服务器）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
在配置复制拓扑之前，制定安全计划是非常重要的。 本教程演示如何更好地保护复制拓扑的安全。 它还会演示如何配置分发，这是数据复制的第一步。 开始其他教程之前，必须先完成本教程。  
  
> [!NOTE]  
> 若要在服务器之间安全地复制数据，你应该实施[复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)中提出的所有建议。  
  
## <a name="what-you-will-learn"></a>学习内容  
在本教程中，你将了解如何准备服务器以便用最少的特权安全地运行复制。  

在本教程中，您将学习如何执行以下操作：
> [!div class="checklist"]
> * 为复制创建 Windows 帐户。
> * 准备快照文件夹。
> * 配置分发。

## <a name="prerequisites"></a>先决条件
本教程适用于熟悉基本数据库操作，但对复制认识有限的用户。 

若要完成本教程，需要 SQL Server、SQL Server Management Studio (SSMS) 以及 AdventureWorks 数据库：  
  
- 在发布服务器（源）上，安装：  
  
   - 任意版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，但 SQL Server Express 或 SQL Server Compact 除外。 这些版本不能充当复制发布服务器。   
   - [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] 示例数据库。 为了增强安全性，默认情况下不会安装示例数据库。  
  
- 订阅服务器（目标）上，安装任意版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，[!INCLUDE[ssEW](../../includes/ssew-md.md)] 除外。 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 不能充当事务复制中的订阅服务器。  
  
- 安装 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
- 安装 [SQL Server 2017 Developer 版本](https://www.microsoft.com/sql-server/sql-server-downloads)。
- 下载 [AdventureWorks 示例数据库](https://github.com/Microsoft/sql-server-samples/releases)。 有关在 SSMS 中还原数据库的说明，请参阅[还原数据库](../backup-restore/restore-a-database-backup-using-ssms.md)。 
    
>[!NOTE]
> - 在相差两个版本以上的 SQL Server 实例上不支持复制。 有关详细信息，请参阅[复制拓扑中受支持的 SQL Server 版本](replication-backward-compatibility.md)。
> - 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，必须使用属于 sysadmin 固定服务器角色成员的登录名连接到发布服务器和订阅服务器  。 有关此角色的详细信息，请参阅[服务器级别角色](../security/authentication-access/server-level-roles.md)。  


**学完本教程的估计时间：30 分钟**
  
## <a name="create-windows-accounts-for-replication"></a>为复制创建 Windows 帐户
在本部分中，将创建 Windows 帐户以运行复制代理。 你将在本地服务器上为以下代理创建一个单独的 Windows 帐户：  
  
|代理|位置|帐户名|  
|---------|------------|----------------|  
|快照代理|发布者|<*machine_name*>\repl_snapshot|  
|日志读取器代理|发布者|<*machine_name*>\repl_logreader|  
|分发代理|发布服务器和订阅服务器|<*machine_name*>\repl_distribution|  
|合并代理|发布服务器和订阅服务器|<*machine_name*>\repl_merge|  
  
> [!NOTE]  
> 在复制教程中，发布服务器和分发服务器共享同一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例 (NODE1\SQL2016)。 订阅服务器实例 (NODE2\SQL2016) 是远程实例。 发布服务器和订阅服务器可以共享同一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，但这不是必需的。 如果发布服务器和订阅服务器共享同一实例，则在订阅服务器上用于创建帐户的步骤不是必须的。  

### <a name="create-local-windows-accounts-for-replication-agents-at-the-publisher"></a>在发布服务器上为复制代理创建本地 Windows 帐户
  
1. 在发布服务器上，从“控制面板”的“管理工具”  中打开“计算机管理”  。  
  
2. 在“系统工具”  中，展开“本地用户和组”  。  
  
3. 右键单击“用户”，然后选择“新建用户”   。  
     
4. 在“用户名”框中，输入 repl_snapshot，提供密码和其他相关信息，然后选择“创建”来创建 repl_snapshot 帐户    ： 

   ![“新用户”对话框](media/tutorial-preparing-the-server-for-replication/newuser.png)
  
5. 重复上述步骤创建 repl_logreader、repl_distribution 和 repl_merge 帐户：  
 
   ![复制用户列表](media/tutorial-preparing-the-server-for-replication/replusers.png)
  
6. 选择“关闭”  。  
  
### <a name="create-local-windows-accounts-for-replication-agents-at-the-subscriber"></a>在订阅服务器上为复制代理创建本地 Windows 帐户  
  
1. 在订阅服务器上，从“控制面板”的“管理工具”  中打开“计算机管理”  。  
  
2. 在“系统工具”  中，展开“本地用户和组”  。  
  
3. 右键单击“用户”，然后选择“新建用户”   。  
  
4. 在“用户名”框中，输入 repl_distribution，提供密码和其他相关信息，然后选择“创建”来创建 repl_distribution 帐户    。  
  
5. 重复上述步骤创建 repl_merge 帐户。  
  
6. 选择“关闭”  。  

有关详细信息，请参阅[复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)。  

## <a name="prepare-the-snapshot-folder"></a>准备快照文件夹
在本部分中，你可配置用于创建和存储发布快照的快照文件夹。 

### <a name="create-a-share-for-the-snapshot-folder-and-assign-permissions"></a>为快照文件夹创建共享并分配权限  
  
1. 在文件资源管理器，浏览到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件夹。 默认位置为 C:\Program Files\Microsoft SQL Server\MSSQL.X\MSSQL\Data。  
  
2. 创建名为 **repldata** 的新文件夹。  
  
3. 右键单击该文件夹并选择“属性”  。  
  
   a. 在“repldata 属性”对话框的“共享”选项卡上，选择“共享”    。  
  
   b. 在“高级共享”  对话框中，选择“共享此文件夹”  ，然后选择“权限”  。  

   ![用于共享 repldata 文件夹的选择](media/tutorial-preparing-the-server-for-replication/repldata.png)

6. 在“repldata 的权限”对话框中，选择“添加”   。 在“选择用户、计算机、服务帐户或组”  框中，键入以前创建的快照代理帐户的名称，例如 Publisher_Machine_Name  >\repl_snapshot  。 选择“检查名称”，然后选择“确定”   。  

   ![添加共享权限的选择](media/tutorial-preparing-the-server-for-replication/addshareperms.png)

7. 重复步骤 6，添加以前创建的两个帐户：<Publisher_Machine_Name  >\repl_merge  和 <Publisher_Machine_Name  >\repl_distribution  。

8. 添加三个帐户后，会分配以下权限：      
   - repl_distribution：**读取**  
   - repl_merge：**读取**  
   - repl_snapshot：**完全控制**    

   ![每个帐户的共享权限](media/tutorial-preparing-the-server-for-replication/sharedpermissions.png)

9. 共享权限配置正确后，选择“确定”  关闭“repldata 的权限”  对话框。 选择“确定”关闭“高级共享”对话框   。 

10. 选择“repldata 属性”  对话框中的“安全性”  选项卡，然后选择“编辑”  ：  

    ![在“安全性”选项卡上的“编辑”按钮](media/tutorial-preparing-the-server-for-replication/editsecurity.png)   

11. 在“repldata 的权限”对话框中，选择“添加”   。 在“选择用户、计算机、服务帐户或组”  框中，键入以前创建的快照代理帐户的名称，例如 Publisher_Machine_Name  >\repl_snapshot  。 选择“检查名称”，然后选择“确定”   。  

    ![添加安全性权限的选择](media/tutorial-preparing-the-server-for-replication/addsecuritypermissions.png)

  
12. 重复上一步，为分发代理和合并代理添加权限，其格式分别为 <Publisher_Machine_Name  >\repl_distribution  和 <Publisher_Machine_Name  >\repl_merge  。  
    
  
13. 验证是否允许以下权限：  
  
    - repl_distribution：**读取**
    - repl_merge：**读取**
    - repl_snapshot：**完全控制**   
 
    ![用户复制数据权限](media/tutorial-preparing-the-server-for-replication/replpermissions.png) 

14. 再次选择“共享”  选项卡，并记下共享的“网络路径”  。 稍后配置“快照文件夹”时，会需要此路径。  

    ![“共享”选项卡上的网络路径](media/tutorial-replicating-data-between-continuously-connected-servers/networkpath.png)

15. 选择“确定”关闭“repldata 属性”对话框   。 
 
有关详细信息，请参阅[保护快照文件夹的安全](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  
  

## <a name="configure-distribution"></a>配置分发
在本部分中，将在发布服务器中配置分发，并设置所需的发布数据库和分发数据库权限。 如果已经配置了分发服务器，则必须在开始本部分之前禁用发布和分发。 如果必须保留现有复制拓扑，请不要执行该操作，生产中尤其如此。   
  
使用远程分发服务器配置发布服务器不属于本教程讨论的范畴。  

### <a name="configure-distribution-at-the-publisher"></a>在发布服务器中配置分发  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后展开服务器节点。  
  
2. 右键单击“复制”文件夹，然后选择“配置分发”   ：  

   ![快捷菜单上的“配置分发”命令](media/tutorial-preparing-the-server-for-replication/configuredistribution.png)
  
   > [!NOTE]  
   > - 如果连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时使用的是 localhost  ，而不是实际服务器名称，则系统会向你显示一个警告提示，指出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法连接到 localhost 或 IP 地址  。 在警告对话框中选择“确定”  。 在“连接到服务器”对话框中，将“服务器名称”从 localhost 或 IP 地址更改为你的服务器的名称    。 然后选择“连接”  。  
   > - 目前 SQL Server Management Studio (SSMS) 18.0 （以及更高版本）存在一个已知问题，在使用 IP 地址连接到分发服务器时不会显示警告消息，但是这样的操作仍然无效  。 在连接到分发服务器时，应使用实际的服务器名称。 
   
   此时分发配置向导启动。  
  
3. 在“分发服务器”  页上，选择“<'ServerName'”  > “将充当自己的分发服务器；SQL Server 将创建分发数据库和日志”  。 然后，选择“下一步”  。  

   ![使服务器充当自己的分发服务器的选项](media/tutorial-preparing-the-server-for-replication/serverdistributor.png)
  
4. 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理未运行，则在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]“代理启动”页面上，选择“是，将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务配置为自动启动”   。 选择“**下一页**”。  

     
5. 在“快照文件夹”  框中输入路径 \\\\<Publisher_Machine_Name  >\repldata  ，然后选择“下一步”  。 此路径应匹配以前配置共享属性后，在 repldata 属性文件夹的“网络路径”  下看到的路径。 

   ![“repldata 属性”对话框和配置分发向导中的网络路径比较](media/tutorial-preparing-the-server-for-replication/repldatasnapshot.png)
  
6. 接受向导剩余页上的默认值。  
    
   ![向导的最后一页](media/tutorial-preparing-the-server-for-replication/distributionwizarddefaults.png)
  
7. 选择“完成”以启用分发  。 

配置分发服务器时，可能会看到以下错误。 它指示用于启动 SQL Server 代理帐户的帐户不是系统上的管理员。 需要手动启动 SQL Server 代理，向现有帐户授予以下权限，或修改正在使用的帐户。 

![配置 SQL Server 代理的错误消息](media/tutorial-preparing-the-server-for-replication/startingagenterror.png)

如果正在使用管理权限运行 SQL Server Management Studio 实例，可从 SSMS 中手动启动 SQL 代理：  
    
![在快捷菜单上为 SSMS 中的代理选择“启动”](media/tutorial-preparing-the-server-for-replication/ssmsstartagent.png) 

>[!NOTE]
> 如果 SQL 代理不启动，右键单击 SSMS 中的 SQL Server 代理，然后选择“刷新”  。 如果仍处于停止状态，请从 SQL Server 配置管理器中手动启动它。    
  
## <a name="set-database-permissions"></a>设置数据库权限  
  
1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，展开“安全性”，右键单击“登录名”，然后选择“新建登录名”    ：  

   ![快捷菜单上的“新登录”命令](media/tutorial-preparing-the-server-for-replication/newlogin.png)
  
2. 在“常规”  页上，选择“搜索”  。 在“输入要选择的对象名称”  框中输入 <Publisher_Machine_Name  >\repl_snapshot  ，然后依次选择“检查名称”  和“确定”  。  

   ![输入对象名称的选择](media/tutorial-preparing-the-server-for-replication/addsnapshotlogin.png)
  
3. 在“用户映射”  页上，在“映射到此登录名的用户”  列表中选择“distribution”  和“[!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]”数据库。  
  
   在“数据库角色成员身份”列表中，为这两个数据库的登录名选择“db_owner”  角色。  

   ![选择数据库及其角色](media/tutorial-preparing-the-server-for-replication/dbowner.png)
  
4. 选择“确定”创建登录名  。  
  
5. 重复步骤 1-4，为其他本地帐户（repl_distribution、repl_logreader 和 repl_merge）创建登录名。 这些登录名也必须映射到属于“distribution”  数据库和“AdventureWorks”  数据库中“db_owner”  固定数据库角色成员的用户。  

   ![对象资源管理器中所有四个帐户的视图](media/tutorial-preparing-the-server-for-replication/usersinssms.png)
   
  
有关详细信息，请参阅：
- [配置分发](../../relational-databases/replication/configure-distribution.md) 
- [复制代理安全性模式](../../relational-databases/replication/security/replication-agent-security-model.md)  

## <a name="next-steps"></a>后续步骤
现在，你已经为复制成功准备了服务器。 下一篇文章将介绍如何配置事务复制： 

> [!div class="nextstepaction"]
> [教程：在两个完全连接的服务器之间配置复制（事务）](tutorial-replicating-data-between-continuously-connected-servers.md)

  
