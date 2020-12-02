---
description: 验证已复制的数据
title: 验证已复制的数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Subscribers [SQL Server replication], data validation
- replication [SQL Server], validating data
- transactional replication, validating data
- validating data
- merge replication data validation [SQL Server replication], SQL Server Management Studio
ms.assetid: 215b4c9a-0ce9-4c00-ac0b-43b54151dfa3
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: c83a02c9c2b0c8c22a62f1765c839a1c15534405
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470174"
---
# <a name="validate-replicated-data"></a>验证已复制的数据
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中验证订阅服务器上的数据。  
  
通过事务复制和合并复制，您可以验证订阅服务器中的数据与发布服务器中的数据是否匹配。 可以对特定订阅或某一发布的所有订阅执行验证。 指定下列验证类型之一，分发代理或合并代理便会在下次运行时验证数据：  
  
-   **仅行计数。** 此选项将验证订阅服务器上的表与发布服务器上的表的行数是否相同，但不验证行内容是否匹配。 行计数验证是一种简易验证方法，可帮助您了解数据是否存在问题。   
-   **行计数和二进制校验和**。 除了可在发布服务器和订阅服务器上对行进行计数之外，还可使用校验和算法来计算所有数据的校验和。 如果行计数失败，则不计算校验和。  
  
 除了验证订阅服务器和发布服务器上的数据是否匹配之外，合并复制还提供验证每个订阅服务器上的数据是否正确分区的功能。 有关详细信息，请参阅[验证合并订阅服务器的分区信息](../../relational-databases/replication/validate-partition-information-for-a-merge-subscriber.md)。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
   
## <a name="how-data-validation-works"></a>数据验证的工作机制  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过计算发布服务器上的行计数或校验和，并将这些值与订阅服务器上计算所得的行计数或校验和值比较来验证数据。 为整个发布表和整个订阅表各计算一个值，但计算中不包括 **text**、 **ntext** 或 **image** 列中的数据。  
  
 执行计算时，共享锁临时放置对其执行行计数或校验和的表上，不过计算很快完成，同时共享锁被删除，这一过程通常只需几秒钟。  
  
 使用二进制校验和时，32 位冗余校验 (CRC) 将逐列进行，而不是在数据页的物理行上进行。 这使得表中的列可以按任意顺序在数据页上进行物理排序，但对该行计算的 CRC 值不变。 发布上有行筛选器或列筛选器时，可使用二进制校验和进行验证。  

 验证数据分为三个部分：  
  
1.  将对发布的单个或所有订阅“标记”  为要验证。 可以在“验证单个订阅”、“验证多个订阅”和“验证所有订阅”对话框中，将订阅标记为要验证，这些对话框可以通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“本地发布” **文件夹和“本地订阅”** 文件夹访问。 也可以从 **“所有订阅”** 选项卡、 **“订阅监视列表”** 选项卡和复制监视器中的发布节点中对订阅进行标记。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
2.  下次分发代理（对于事务复制）或合并代理（对于合并复制）同步订阅时，将对订阅进行验证。 分发代理通常是连续运行的，在这种情况下验证会立即进行；合并代理通常是按需运行的，在这种情况下验证在运行代理后进行。  
  
3.  查看验证结果：   
    -   在复制监视器的详细信息窗口中查看：对于事务复制，在 **“分发服务器到订阅服务器的历史记录”** 选项卡中；对于合并复制，在 **“同步历史记录”** 选项卡中。    
    -   在 **的** “查看同步状态” [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]对话框中查看。  
 
## <a name="considerations-and-restrictions"></a>注意事项和限制  
 请在验证数据时考虑下列问题：  
  
-   验证数据之前，必须停止订阅服务器上的一切更新活动（验证进行时不必停止发布服务器上的活动）。  
-   因为校验和与二进制校验和在验证大型数据集时可能需要大量处理器资源，所以应将验证安排在复制中所用服务器上的活动最少时进行。   
-   复制只验证表；它不验证只包含架构的项目（例如存储过程）是否在发布服务器和订阅服务器上相同。   
-   可以对任何发布的表使用二进制校验和。 校验和无法验证带有列筛选器的表或列偏移量不一致的逻辑表结构（由于存在用于删除或添加列的 ALTER TABLE 语句）。   
-   复制验证使用 **checksum** 和 **binary_checksum** 函数。 有关其行为的信息，请参阅[校验和 (Transact-SQL)](../../t-sql/functions/checksum-transact-sql.md) 和 [BINARY_CHECKSUM (Transact-SQL)](../../t-sql/functions/binary-checksum-transact-sql.md)。   
-   如果订阅服务器与发布服务器上的数据类型不同，则使用二进制校验和或校验和进行的验证可能会错误地报告失败。 如果执行以下操作之一，则可能出现上述情况：    
    -   显式设置架构选项从而为早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]映射数据类型。  
    -   将合并发布的发布兼容级别设置为早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，并且已发布表包含一个或多个必须为此版本映射的数据类型。    
    -   手动初始化订阅并且正在订阅服务器上使用不同的数据类型。   
-   事务复制的可转换订阅不支持二进制校验和和校验和验证。   
-   复制到非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器的数据不支持验证。    
-   复制监视器的过程仅适用于推送订阅，因为请求订阅不能在复制监视器中同步。 但是，可以在复制监视器中将订阅标记为要验证并查看请求订阅的验证结果。    
-   验证结果指示验证是成功还是失败，但如果失败，并不指定哪些行验证失败。 若要比较发布服务器和订阅服务器中的数据，请使用 [tablediff Utility](../../tools/tablediff-utility.md)。 若要详细了解如何将此实用工具与已复制数据配合使用，请参阅[比较所复制表的差异（复制编程）](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md)。  


## <a name="data-validation-results"></a>数据验证结果  
 验证完成时，分发代理或合并代理将记录有关成功或失败的消息（复制不报告具体失败的行）。 可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、复制监视器和复制系统表中查看这些消息。 上面所列的操作指南主题说明如何运行验证和查看结果。  
  
 若要处理验证失败，请考虑以下事项：  
  
-   配置名为 **“复制: 订阅服务器未通过数据验证”** 的复制警报，以便您在验证失败时得到通知。 有关详细信息，请参阅[配置预定义的复制警报 (SQL Server Management Studio)](../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md)。  
  
-   验证失败是否已对您的应用程序带来问题？ 如果验证失败带来了问题，请手动更新数据以进行同步，或者重新初始化订阅：  
  
    -   可以使用 [tablediff 实用工具](../../tools/tablediff-utility.md)来更新数据。 有关如何使用此实用工具的详细信息，请参阅[比较所复制表的差异（复制编程）](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md)。  
  
    -   有关重新初始化的详细信息，请参阅[重新初始化订阅](../../relational-databases/replication/reinitialize-subscriptions.md)。  

  
  
## <a name="articles-in-transactional-replication"></a>事务复制中的项目 

### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到发布服务器，然后展开服务器节点。    
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。    
3.  右键单击要验证其订阅的发布，然后单击 **“验证多个订阅”**。    
4.  在 **“验证多个订阅”** 对话框中，选择要验证的订阅：   
    -   选择 **“验证所有 SQL Server 订阅”**。    
    -   选择 **“验证下列订阅”**，再选择一个或多个订阅。    
5.  若要指定要执行的验证类型（行计数，或行计数和校验和），请单击 **“验证选项”**，然后在 **“订阅验证选项”** 对话框中指定选项。  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]   
7.  在复制监视器或 **“查看同步状态”** 对话框中查看验证结果。 对于每个订阅：   
    1.  展开发布，右键单击该订阅，然后单击 **“查看同步状态”**。    
    2.  如果代理未运行，请单击 **“查看同步状态”** 对话框中的 **“启动”** 。 该对话框将显示有关验证的信息性消息。    
     如果未显示有关验证的任何消息，则说明该代理已记录了后续消息。 在这种情况下，请在复制监视器中查看验证结果。 有关详细信息，请参阅本主题中有关复制监视器相关操作的过程。  

### <a name="using-transact-sql"></a>“使用 Transact-SQL”

#### <a name="all-articles"></a>所有项目 
  
1.  在发布服务器上，对发布数据库执行 [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)。 指定 `@publication`并为 `@rowcount_only` 指定以下值之一：  
  
    -   **1** - 只检查行计数（默认值）    
    -   **2** - 行计数和二进制校验和。  
  
    > [!NOTE]  
    >  执行 [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md) 时，将会对发布中的每个项目执行 [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)。 若要成功地执行 [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)，必须对发布的基表中的所有列拥有 SELECT 权限。    
2.  （可选）如果分发代理尚未运行，请为每个订阅都启动分发代理。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 和 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。    
3.  检查代理输出以获取验证结果。 
  
#### <a name="single-article"></a>单个项目  
  
1.  在发布服务器上，对发布数据库执行 [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)。 指定 `@publication`，为 `@article` 指定项目名称，并为 `@rowcount_only` 指定以下值之一：  
  
    -   **1** - 只检查行计数（默认值）    
    -   **2** - 行计数和二进制校验和。  
  
    > [!NOTE]  
    >  若要成功地执行 [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)，必须对发布的基表中的所有列拥有 SELECT 权限。  
  
2.  （可选）如果分发代理尚未运行，请为每个订阅都启动分发代理。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 和 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。    
3.  检查代理输出以获取验证结果。
  
#### <a name="single-subscriber"></a>单个订阅服务器 
  
1.  在发布服务器的发布数据库中，使用 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md) 打开一个显式事务。    
2.  在发布服务器上，对发布数据库执行 [sp_marksubscriptionvalidation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md)。 为 `@publication` 指定发布，为 `@subscriber` 指定订阅服务器的名称，并为 `@destination_db` 指定订阅数据库的名称。    
3.  （可选）对每个要验证的订阅都重复步骤 2。    
4.  在发布服务器上，对发布数据库执行 [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)。 指定 `@publication`，为 `@article` 指定项目名称，并为 `@rowcount_only` 指定以下值之一：    
    -   **1** - 只检查行计数（默认值）    
    -   **2** - 行计数和二进制校验和。  
  
    > [!NOTE]  
    >  若要成功地执行 [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)，必须对发布的基表中的所有列拥有 SELECT 权限。  
  
5.  在发布服务器的发布数据库中，使用 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md) 提交事务。    
6.  （可选）为每个要验证的项目都重复步骤 1 到步骤 5。   
7.  （可选）如果分发代理尚未运行，请启动分发代理。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 和 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。    
8.  检查代理输出以获取验证结果。 有关详细信息，请参阅 [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md)。  

## <a name="all-push-subscriptions-to-a-transactional-publication"></a>事务发布的所有推送订阅 

### <a name="using-replication-monitor"></a>使用复制监视器
  
1.  在复制监视器的左窗格中依次展开发布服务器组、发布服务器。   
2.  右键单击要验证其订阅的发布，然后单击 **“验证多个订阅”**。   
3.  在 **“验证多个订阅”** 对话框中，选择要验证的订阅：  
  
    -   选择 **“验证所有 SQL Server 订阅”**。    
    -   选择 **“验证下列订阅”**，再选择一个或多个订阅。    
4.  若要指定要执行的验证类型（行计数，或行计数和校验和），请单击 **“验证选项”**，然后在 **“订阅验证选项”** 对话框中指定选项。    
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  单击 **“所有订阅”** 选项卡。  
7.  查看验证结果。 对于每个推送订阅：    
    1.  如果代理未运行，请右键单击该订阅，然后单击 **“开始同步”**。    
    2.  右键单击该订阅，然后单击 **“查看详细信息”**。   
    3.  在 **“所选会话中的操作”** 文本区域中的 **“分发服务器到订阅服务器的历史记录”** 选项卡中，查看信息。  
  
## <a name="for-a-single-subscription-to-a-merge-publication"></a>对于合并发布的单个订阅

### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到发布服务器，然后展开服务器节点。    
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。   
3.  展开要验证其订阅的发布，右键单击该订阅，然后单击 **“验证单个订阅”**。    
4.  在 **“验证单个订阅”** 对话框中，选择 **“验证此订阅”**。    
5.  若要指定要执行的验证类型（行计数，或行计数和校验和），请单击 **“选项”**，然后在 **“订阅验证选项”** 对话框中指定选项。    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  在复制监视器或 **“查看同步状态”** 对话框中查看验证结果：  
  
    1.  展开发布，右键单击该订阅，然后单击 **“查看同步状态”**。    
    2.  如果代理未运行，请单击 **“查看同步状态”** 对话框中的 **“启动”** 。 该对话框将显示有关验证的信息性消息。  
  
     如果未显示有关验证的任何消息，则说明该代理已记录了后续消息。 在这种情况下，请在复制监视器中查看验证结果。 有关详细信息，请参阅本主题中有关复制监视器相关操作的过程。  
  
## <a name="for-all-subscriptions-to-a-merge-publication"></a>对于合并发布的所有订阅

### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到发布服务器，然后展开服务器节点。    
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。   
3.  右键单击要验证其订阅的发布，然后单击 **“验证所有订阅”**。    
4.  在 **“验证所有订阅”** 对话框中，指定要执行的验证类型（行计数，或行计数和校验和）。   
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  在复制监视器或 **“查看同步状态”** 对话框中查看验证结果。 对于每个订阅：    
    1.  展开发布，右键单击该订阅，然后单击 **“查看同步状态”**。    
    2.  如果代理未运行，请单击 **“查看同步状态”** 对话框中的 **“启动”** 。 该对话框将显示有关验证的信息性消息。  
  
     如果未显示有关验证的任何消息，则说明该代理已记录了后续消息。 在这种情况下，请在复制监视器中查看验证结果。 有关详细信息，请参阅本主题中有关复制监视器相关操作的过程。  
  
 
## <a name="for-a-single-push-subscription-to-a-merge-publication"></a>对于合并发布的单个推送订阅 

### <a name="using-replication-monitor"></a>使用复制监视器  
1.  在复制监视器的左窗格中依次展开发布服务器组、发布服务器，再单击一个发布。    
2.  单击 **“所有订阅”** 选项卡。    
3.  右键单击要验证的订阅，然后单击 **“验证单个订阅”**。    
4.  在 **“验证单个订阅”** 对话框中，选择 **“验证此订阅”**。    
5.  若要指定要执行的验证类型（行计数，或行计数和校验和），请单击 **“选项”**，然后在 **“订阅验证选项”** 对话框中指定选项。    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  单击 **“所有订阅”** 选项卡。    
8.  查看验证结果：    
    1.  如果代理未运行，请右键单击该订阅，然后单击 **“开始同步”**。    
    2.  右键单击该订阅，然后单击 **“查看详细信息”**。    
    3.  查看 **“同步历史记录”** 选项卡 **“所选会话的最后消息”** 文本区域中的信息。  

### <a name="using-transact-sql"></a>“使用 Transact-SQL”
1.  在发布服务器上，对发布数据库执行 [sp_validatemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergesubscription-transact-sql.md)。 指定 `@publication`，为 `@subscriber` 指定订阅服务器的名称，为 `@subscriber_db` 指定订阅数据库的名称，并为 `@level` 指定以下值之一：   
    -   **1** - 只验证行计数。    
    -   **3** - 行计数二进制校验和验证。  
  
     这样会标记所选订阅以便验证。  
  
2.  启动每个订阅的合并代理。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 和 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。  
3.  检查代理输出以获取验证结果。   
4.  为每个要验证的订阅重复步骤 1 到步骤 3。  
  
> [!NOTE]  
>  通过在运行 **Replication Merge Agent** 时指定 [-Validate](../../relational-databases/replication/agents/replication-merge-agent.md)参数还可以在同步结束时验证对合并发布的订阅。  
  
## <a name="for-all-push-subscriptions-to-a-merge-publication"></a>对于合并发布的所有推送订阅
  
### <a name="using-replication-monitor"></a>使用复制监视器    
1.  在复制监视器的左窗格中依次展开发布服务器组、发布服务器。    
2.  右键单击要验证其订阅的发布，然后单击 **“验证所有订阅”**。    
3.  在 **“验证所有订阅”** 对话框中，指定要执行的验证类型（行计数，或行计数和校验和）。    
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
5.  单击 **“所有订阅”** 选项卡。    
6.  查看验证结果。 对于每个推送订阅：    
    1.  如果代理未运行，请右键单击该订阅，然后单击 **“开始同步”**。    
    2.  右键单击该订阅，然后单击 **“查看详细信息”**。    
    3.  查看 **“同步历史记录”** 选项卡 **“所选会话的最后消息”** 文本区域中的信息。 
  
### <a name="using-transact-sql"></a>“使用 Transact-SQL”
1.  在发布服务器上，对发布数据库执行 [sp_validatemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergepublication-transact-sql.md)。 指定 `@publication`并为 `@level` 指定以下值之一：    
    -   **1** - 只验证行计数。   
    -   **3** - 行计数二进制校验和验证。  
  
     这样会标记所有订阅以便验证。   
2.  启动每个订阅的合并代理。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 和 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。  
  
3.  检查代理输出以获取验证结果。 有关详细信息，请参阅 [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md)。  

  
## <a name="validate-data-using-merge-agent-parameters"></a>使用合并代理参数验证数据  
  
1.  通过以下方式之一从命令提示符下启动订阅服务器（请求订阅）或分发服务器（推送订阅）上的合并代理。   
    -   为 **-Validate** 参数指定值 **1** （行计数）或 **3** （行计数和二进制校验和）。    
    -   为 **-ProfileName** 参数指定 **rowcount validation** 或 **rowcount and checksum validation** 。  
  
     有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 或 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 通过使用复制，您可以使用复制管理对象 (RMO) 以编程方式验证订阅服务器上的数据是否与发布服务器上的数据匹配。 使用的对象取决于复制拓扑的类型。 事务复制要求验证发布的所有订阅。  
  
> [!NOTE]  
>  有关示例，请参阅本节后面的 [示例 (RMO)](#RMOExample)。  
  
#### <a name="to-validate-data-for-all-articles-in-a-transactional-publication"></a>验证事务发布中所有项目的数据  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例。 设置发布的 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性。 将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的其余属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.TransPublication.ValidatePublication%2A> 方法。 传递以下参数：  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationOption>  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationMethod>  
  
    -   一个布尔值，指示是否在验证完成后停止分发代理。  
  
     这将把项目标记为等待验证。  
  
5.  如果尚未运行，则启动分发代理以同步每个订阅。 有关详细信息，请参阅 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) 或 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md)。 验证操作的结果将写入代理历史记录。 有关详细信息，请参阅 [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md)。  
  
#### <a name="to-validate-data-in-all-subscriptions-to-a-merge-publication"></a>验证合并发布的所有订阅中的数据  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例。 设置发布的 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性。 将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的其余属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.MergePublication.ValidatePublication%2A> 方法。 传递所需的 <xref:Microsoft.SqlServer.Replication.ValidationOption>。  
  
5.  对每个订阅运行合并代理以开始验证，或者等待下一个计划的代理运行。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 和 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。 验证操作的结果将写入代理的历史记录，您可以使用复制监视器查看。 有关详细信息，请参阅 [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md)。  
  
#### <a name="to-validate-data-in-a-single-subscription-to-a-merge-publication"></a>验证合并发布的单个订阅中的数据  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例。 设置发布的 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性。 将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的其余属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.MergePublication.ValidateSubscription%2A> 方法。 传递要验证的订阅服务器和订阅数据库的名称以及所需的 <xref:Microsoft.SqlServer.Replication.ValidationOption>。  
  
5.  对订阅运行合并代理以开始验证，或者等待下一个计划的代理运行。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 和 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。 验证操作的结果将写入代理的历史记录，您可以使用复制监视器查看。 有关详细信息，请参阅 [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md)。  
  
###  <a name="example-rmo"></a><a name="RMOExample"></a> 示例 (RMO)  
 此示例将对事务发布的所有订阅标记为行计数验证。  
  
 [!code-cs[HowTo#rmo_ValidateTranPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatetranpub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateTranPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatetranpub)]  
  
 此示例将对合并发布的特定订阅标记为行计数验证。  
  
 [!code-cs[HowTo#rmo_ValidateMergeSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatemergesub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateMergeSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatemergesub)]  
  
 ## <a name="see-also"></a>另请参阅  
[Best Practices for Replication Administration](../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  
