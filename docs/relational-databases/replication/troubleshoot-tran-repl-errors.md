---
title: 查找事务复制错误
description: 描述如何查找和识别事务复制错误，以及解决复制问题的故障排除方法。
ms.custom: seo-lt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], tutorials
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d39286c1bc228a9da76a163a39e76dbb2022d807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348646"
---
# <a name="troubleshooter-find-errors-with-sql-server-transactional-replication"></a>排除故障：查找 SQL Server 事务复制错误 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

如果对事务复制的工作机制没有基本的了解，那么排查复制错误可能会遭遇挫败。 创建发布的第一步是使用快照代理创建快照并将其保存到快照文件夹。 接下来，分发代理将该快照应用于订阅服务器。 

此过程可创建发布并将其设为同步状态  。 同步分三个阶段进行：
1. 事务发生在复制对象上，并在事务日志中将其标记为“用于复制”。 
2. 日志读取器代理扫描事务日志并查找标记为“用于复制”的事务。 然后这些事务将保存到分发数据库。 
3. 分发代理使用读取器线程扫描分发数据库。 然后，通过使用编写器线程，此代理连接到订阅服务器，将这些更改应用于订阅服务器。

此过程的任何一步都可能发生错误。 查找这些错误可能是故障排除同步问题最具挑战性的方面。 还好可使用复制监视器简化此过程。 

>[!NOTE]
> - 本疑难解答指南旨在介绍故障排除方法。 本指南无法解决具体错误，但可提供查找复制错误的通用指南。 其中提供了一些具体示例，但实际解决方法因环境而异。 
> - 本指南提供的错误示例基于[配置事务复制](../../relational-databases/replication/tutorial-replicating-data-between-continuously-connected-servers.md)教程。



## <a name="troubleshooting-methodology"></a>故障排除方法 

### <a name="questions-to-ask"></a>提出的问题
1. 同步过程中，复制失败出现在何处？
2. 哪个代理遇到错误？
1. 上次复制成功是在什么时候？ 从那以后有什么变化？  

### <a name="steps-to-take"></a>要执行的步骤
1. 使用复制监视器来确定复制出错的位置（哪个代理？）：
   - 如果在“发布服务器到分发服务器”部分发生错误，则是日志读取器代理出现问题  。 
   - 如果在“分发服务器到订阅服务器”部分发生错误，则是分发代理出现问题  。  
2. 在作业活动监视器中查看该代理的作业历史记录，确定错误的详细信息。 如果作业历史记录显示的信息不够详尽，可在该特定代理上[启用详细日志记录](#enable-verbose-logging-on-any-agent)。
3. 尝试确定错误的解决方案。


## <a name="find-errors-with-the-snapshot-agent"></a>查找快照代理错误
快照代理生成快照并将其写入指定的快照文件夹。 

1. 查看快照代理的状态：

    a. 在对象资源管理器中，展开“复制”下的“本地发布”节点   。

    b. 右键单击发布“AdvWorksProductTrans” > “查看快照代理状态”   。 

    ![快捷菜单上的“查看快照代理状态”命令](media/troubleshooting-tran-repl-errors/view-snapshot-agent-status.png)

1. 如果快照代理状态中报告了错误，可在快照代理作业历史记录中了解更多详细信息：

    a. 在对象资源管理器中展开“SQL Server 代理”，并打开作业活动监视器  。 

    b. 按“类别”排序，并根据类别“REPL-Snapshot”找到快照代理   。

    c. 右键单击快照代理，然后选择“查看历史记录”  。 

   ![用于打开快照代理历史记录的选择](media/troubleshooting-tran-repl-errors/snapshot-agent-history.png)
    
1. 在快照代理历史记录中，选择相关的日志项目。 这通常位于报告错误的项目前一两行  。 （红色的 X 表示错误。）查看日志下方框中的消息文本： 

    ![拒绝访问的快照代理错误](media/troubleshooting-tran-repl-errors/snapshot-access-denied.png)

    ```console
    The replication agent had encountered an exception.
    Exception Message: Access to path '\\node1\repldata.....' is denied.
    ```

如果未正确配置快照文件夹的 Windows 权限，将看到快照代理出现“访问遭拒绝”的错误。 需验证是否具备存储快照文件夹的权限，并确保用于运行快照代理的帐户有权访问共享。  

## <a name="find-errors-with-the-log-reader-agent"></a>查找日志读取器代理错误
日志读取器代理会连接到发布服务器数据库，并扫描事务日志，查找标记为“用于复制”的任何事务。 然后将这些事务添加到分发数据库。 

1.  连接到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的发布服务器。 展开服务器节点，右键单击“复制”  文件夹，然后选择“启动复制监视器”  ：  

    ![快捷菜单上的“启动复制监视器”命令](media/troubleshooting-tran-repl-errors/launch-repl-monitor.png)
  
    此时，复制监视器打开：![复制监视器](media/troubleshooting-tran-repl-errors/repl-monitor.png) 
   
2. 红色 X 表示发布不同步。 展开左侧的“我的发布服务器”，然后展开相关的发布服务器  。  
  
3.  选择左侧的“AdvWorksProductTrans”发布，然后在其中一个选项卡上查找红色 X，确定问题的位置  。 在本例中，红色 X 位于“代理”选项卡上，表明其中一个代理遇到错误  ： 

    ![“代理”选项卡上的红色 X](media/troubleshooting-tran-repl-errors/agent-error.png)

4. 选择“代理”选项卡，确定遇到错误的代理  ： 

    ![故障日志读取器代理的红色 X](media/troubleshooting-tran-repl-errors/log-reader-agent-failure.png)


5. 此视图显示两种代理：快照代理和日志读取器代理。 遇到错误的代理会出现红色的 X。在本例中，是日志读取器代理。 

    双击报告错误的行，打开日志读取器代理的代理历史记录。 此历史记录提供有关该错误的详细信息： 
    
    ![日志读取器代理的错误详细信息](media/troubleshooting-tran-repl-errors/log-reader-error.png)

    ```console
    Status: 0, code: 20011, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.
    The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.
    Status: 0, code: 15517, text: 'Cannot execute as the database principal because the principal "dbo" does not exist, this type of principal cannot be impersonated, or you do not have permission.'.
    Status: 0, code: 22037, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.        
    ```

6. 若未正确设置发布服务器数据库的所有者，通常便会发生该错误。 这可能会在还原数据库时发生。 要验证这一点：

    a. 在对象资源管理器中展开“数据库”  。

    b. 右键单击“AdventureWorks2012” > “属性”   。 

    c. 确认一名所有者存在于“文件”页下  。 如果此框为空，则这可能是产生该问题的原因所在。 

   ![数据库属性中的“文件”页，其上的“所有者”框为空](media/troubleshooting-tran-repl-errors/db-properties.png)

7. 如果“文件”页上的所有者为空，请在 AdventureWorks2012 数据库的上下文中打开“新建查询”窗口   。 运行以下 T-SQL 代码：

    ```sql
    -- set the owner of the database to 'sa' or a specific user account, without the brackets. 
    EXECUTE sp_changedbowner '<useraccount>'
    -- example for sa: exec sp_changedbowner 'sa'
    -- example for user account: exec sp_changedbowner 'sqlrepro\administrator' 
    ```

8. 可能需要重启日志读取器代理：

    a. 在对象资源管理器中展开“SQL Server 代理”节点，并打开作业活动监视器  。

    b. 按“类别”排序，并根据“REPL-LogReader”类别找到日志读取器代理   。 

    c. 右键单击日志读取器代理作业，并选择“启动作业于步骤”   。 

    ![用于重启日志读取器代理的选择](media/troubleshooting-tran-repl-errors/start-job-at-step.png)

9. 通过再次打开“复制监视器”，确认发布现已同步。 如果尚未打开，通过右键单击对象资源管理器中的“复制”，可找到该监视器  。 
10. 选择“AdvWorksProductTrans”发布，接着选择“代理”选项卡，然后双击日志读取器代理，打开代理历史记录   。 现应看到日志读取器代理正在运行，并且正在复制命令或具有“未复制事务”：

    ![日志读取器代理正在运行，且没有复制事务](media/troubleshooting-tran-repl-errors/log-reader-running.png)

## <a name="find-errors-with-the-distribution-agent"></a>查找分发代理错误
分发代理在分发数据库中查找数据，然后将其应用到订阅服务器。 

1. 连接到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的发布服务器。 展开服务器节点，右键单击“复制”文件夹，然后选择“启动复制监视器”   。  
2. 在“复制监视器”中，选择“AdvWorksProductTrans”发布，然后选择“所有订阅”选项卡    。右键单击订阅并选择“查看详细信息”  ：

    ![快捷菜单上的“查看详细信息”命令](media/troubleshooting-tran-repl-errors/view-details.png)

2. “分发服务器到订阅服务器历史记录”对话框随即打开，其中阐明了代理遇到的错误  ： 

     ![分发代理的错误详细信息](media/troubleshooting-tran-repl-errors/dist-history-error.png)

    ```console
    Error messages:
    Agent 'NODE1\SQL2016-AdventureWorks2012-AdvWorksProductTrans-NODE2\SQL2016-7' is retrying after an error. 89 retries attempted. See agent job history in the Jobs folder for more details.
    ```

3. 错误指示分发代理正在重试。 若要查找更多详细信息，请检查分发代理的作业历史记录： 

    a. 在对象资源管理器中展开“SQL Server 代理”>“作业活动监视器”   。 
    
    b. 按照“类别”对作业进行排序  。 

    c. 根据类别“REPL-Distribution”找到分发代理  。 右键单击该代理并选择“查看历史记录”  。

    ![用于查看分发代理历史记录的选择](media/troubleshooting-tran-repl-errors/view-dist-agent-history.png)

5. 选择其中一个错误条目，并在窗口底部查看错误文本：  

    ![表示分发代理密码错误的错误文本](media/troubleshooting-tran-repl-errors/dist-pw-wrong.png)

    ```console
    Message:
    Unable to start execution of step 2 (reason: Error authenticating proxy NODE1\repl_distribution, system error: The user name or password is incorrect.)
    ```

6. 此错误表示分发代理使用的密码不正确。 解决方法：

    a. 展开对象资源管理器中的“复制”节点  。
    
    b. 右键单击“订阅”>“属性面板”  。
    
    c. 选择“代理进程帐户”旁边的省略号 (...)，并修改密码  。

    ![用于修改分发代理密码的选择](media/troubleshooting-tran-repl-errors/dist-agent-pw-change.png)

7. 通过右键单击对象资源管理器中的“复制”再次检查“复制监视器”  。 “所有订阅”下的红色 X 表示分发代理仍遇到错误  。 

    通过右键单击“复制监视器”中的订阅 > “查看详细信息”，打开“分发到订阅服务器”历史记录    。 此处的错误是不同的： 

    ![表示分发代理无法连接的错误](media/troubleshooting-tran-repl-errors/dist-agent-cant-connect.png)

    ```console
    Connecting to Subscriber 'NODE2\SQL2016'        
    Agent message code 20084. The process could not connect to Subscriber 'NODE2\SQL2016'.
    Number:  18456
    Message: Login failed for user 'NODE2\repl_distribution'.
    ```

8. 此错误表示分发代理无法连接到订阅服务器，因为用户 NODE2\repl_distribution 登录失败  。 若要进一步调查，请连接到订阅服务器，并在对象资源管理器的“管理”节点下，打开当前的 SQL Server 错误日志   ： 

    ![表示订阅服务器登录失败的错误](media/troubleshooting-tran-repl-errors/login-failed.png)
    
    如果看到此错误，则表明登录在订阅服务器上丢失 若要解决此错误，请参阅[复制权限](../../relational-databases/replication/security/security-role-requirements-for-replication.md)。

9. 解决登录错误后，再次检查复制监视器。 如果所有问题均得以解决，可在“所有订阅”下的“发布名称”旁边看到一个绿色箭头，并且状态为“正在运行”    。 

    右键单击订阅，即可再次打开“分发服务器到订阅服务器”历史记录，从而验证是否成功  。 如果是首次运行分发代理，将看到快照已批量复制到订阅服务器： 

     ![状态为“正在运行”的分发代理以及有关大容量复制的消息](media/troubleshooting-tran-repl-errors/dist-agent-success.png)   


## <a name="enable-verbose-logging-on-any-agent"></a>启用任何代理上的详细日志记录
可使用详细日志记录来查看有关复制拓扑中任何代理发生的错误的更多详细信息。 其步骤对每个代理均相同。 只要确保在作业活动监视器中选择了正确的代理即可。 

   >[!NOTE]   
   > 代理可位于发布服务器或订阅服务器上，具体取决于它是请求订阅还是推送订阅。 如果在所查看的服务器上找不到要查找的代理，请尝试检查其他服务器。  

1. 确定要保存详细日志记录的位置，并确保该文件夹存在。 此示例使用 c:\temp。 
2. 在对象资源管理器中展开“SQL Server 代理”节点，并打开作业活动监视器  。 

    ![作业活动监视器快捷菜单上的“查看作业活动”命令](media/troubleshooting-tran-repl-errors/job-activity-monitor.png)    

1. 按“类别”排序，并确定感兴趣的代理  。 本示例使用日志读取器代理。 右键单击感兴趣的代理 >“属性”  。

    ![用于打开代理属性的选择](media/troubleshooting-tran-repl-errors/log-agent-properties.png)

1. 选择“步骤”页，然后突出显示“运行代理”步骤   。 选择“编辑”  。 

    ![用于编辑“运行代理”步骤的选择](media/troubleshooting-tran-repl-errors/edit-steps.png)

1. 在“命令”框中，开始新行，输入以下文本并选择“确定”   ： 

    ```console
    -Output C:\Temp\OUTPUTFILE.txt -Outputverboselevel 3
    ```

    可根据你的偏好修改位置和详细级别。

    ![作业步骤属性中的详细输出](media/troubleshooting-tran-repl-errors/verbose.png)

   > [!NOTE]
   > 当添加详细输出参数时，这些内容可能会导致代理失败，或者输出文件丢失：
   > - 存在短划线变成连字符的格式问题。 
   > - 该位置在磁盘上不存在，或者运行代理的帐户没有权限写入指定的位置。 
   > - 最后一个参数与 `-Output` 参数之间缺少一个空格。 
   > - 不同的代理支持不同的详细级别。 如果启用详细日志记录但无法启动代理，请尝试将指定的详细级别降低 1 级。 

1. 右键单击“代理”>“停止作业于步骤”来重启“日志读取器代理”  。 从工具栏中选择“刷新”图标来刷新  。 右键单击“代理”>“启动作业于步骤”  。
2. 查看磁盘上的输出。 

    ![输出文本文件](media/troubleshooting-tran-repl-errors/output.png)

    
1. 若要禁用详细日志记录，请按照之前的相同步骤来删除先前添加的整个 `-Output` 行。 

有关详细信息，请参阅[为复制代理启用详细日志记录](https://support.microsoft.com/help/312292/how-to-enable-replication-agents-for-logging-to-output-files-in-sql-se)。 


## <a name="see-also"></a>另请参阅
<br>[事务复制概述](../../relational-databases/replication/transactional/transactional-replication.md)
<br>[复制教程](../../relational-databases/replication/replication-tutorials.md)
<br>[ReplTalk 博客](https://blogs.msdn.microsoft.com/repltalk)

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]

