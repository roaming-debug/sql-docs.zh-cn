---
description: 手动初始化订阅
title: 手动初始化订阅 | Microsoft Docs
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- manual subscription initialization [SQL Server replication]
- subscriptions [SQL Server replication], initializing
- initializing subscriptions [SQL Server replication], without snapshots
ms.assetid: 27a1bc38-e498-4fff-8082-04b52aa4b22c
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 796953a657cbdafce8e4b2a62aa90ea69a5cec8c
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076329"
---
# <a name="initialize-a-subscription-manually"></a>手动初始化订阅
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中手动初始化订阅。 虽然初始快照通常用于初始化订阅，但如果架构和初始数据已经在订阅服务器上存在，则可以在不使用快照的情况下初始化对发布的订阅。  
  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果在将数据及架构复制到订阅服务器和手动初始化订阅之间的时间段内，在使用事务复制发布的数据库上进行了某活动，则此活动导致的更改可能不会复制到订阅服务器。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可通过将架构（通常为数据）复制到订阅数据库，手动初始化对发布的订阅。 架构和数据应与发布数据库匹配。 然后在新建订阅向导的 **“初始化订阅”** 页上指定订阅不需要架构和数据。 有关访问此向导的详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md) 和 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)中手动初始化订阅。  
  
 首次对订阅进行同步时，复制所需的对象和元数据将复制到订阅数据库。  
  
#### <a name="to-initialize-a-subscription-to-a-publication-manually"></a>手动初始化对发布的订阅  
  
1.  确保架构和数据已复制到订阅数据库。  
  
2.  清除 **“初始化订阅”** 页中的 **“初始化”** 复选框。 对每个只要求复制对象和元数据的订阅执行此操作。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程手动初始化订阅。  
  
#### <a name="to-manually-initialize-a-pull-subscription-to-a-transactional-publication"></a>手动初始化对事务发布的请求订阅  
  
1.  确保订阅数据库中存在架构和数据。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
2.  在发布服务器的发布数据库中，执行 [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)。 指定 \@publication 和 \@subscriber，为 \@destination_db 指定订阅服务器上包含已发布数据的数据库的名称，并将 \@subscription_type 的值指定为 pull，将 \@sync_type 的值指定为 replication support only      。 有关详细信息，请参阅 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
3.  在订阅服务器上，执行 [sp_addpullsubscription](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)。 有关更新订阅的信息，请参阅 [Create an Updatable Subscription to a Transactional Publication](./publish/create-an-updatable-subscription-to-a-transactional-publication.md)。  
  
4.  在订阅服务器上，执行 [sp_addpullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)。 有关详细信息，请参阅 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
5.  启动分发代理以传输复制对象，并从发布服务器下载最新更改。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md)。  
  
#### <a name="to-manually-initialize-a-push-subscription-to-a-transactional-publication"></a>手动初始化对事务发布的推送订阅  
  
1.  确保订阅数据库中存在架构和数据。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
2.  在发布服务器的发布数据库中，执行 [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)。 为 \@destination_db 指定订阅服务器上包含已发布数据的数据库的名称，并将 \@subscription_type 的值指定为 push，将 \@sync_type 的值指定为 replication support only    。 有关更新订阅的信息，请参阅 [Create an Updatable Subscription to a Transactional Publication](./publish/create-an-updatable-subscription-to-a-transactional-publication.md)。  
  
3.  在发布服务器的发布数据库中，执行 [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)。 有关详细信息，请参阅 [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)。  
  
4.  启动分发代理以传输复制对象，并从发布服务器下载最新更改。 有关详细信息，请参阅 [同步推送订阅](../../relational-databases/replication/synchronize-a-push-subscription.md)。  
  
#### <a name="to-manually-initialize-a-pull-subscription-to-a-merge-publication"></a>手动初始化对合并发布的请求订阅  
  
1.  确保订阅数据库中存在架构和数据。 通过在订阅服务器上还原发布数据库的备份，即可完成此操作。  
  
2.  在发布服务器中，执行 [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)。 指定 \@publication、\@subscriber 和 \@subscriber_db，并将 \@subscription_type 的值指定为 pull    。 这样便可注册请求订阅。  
  
3.  在订阅服务器上，对包含已发布数据的数据库执行 [sp_addmergepullsubscription](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)。 将 \@sync_type 的值指定为 none。  
  
4.  在订阅服务器上，执行 [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)。 有关详细信息，请参阅 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
5.  启动合并代理以传输复制对象，并从发布服务器下载最新更改。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md)。  
  
#### <a name="to-manually-initialize-a-push-subscription-to-a-merge-publication"></a>手动初始化对合并发布的推送订阅  
  
1.  确保订阅数据库中存在架构和数据。 通过在订阅服务器上还原发布数据库的备份，即可完成此操作。  
  
2.  在发布服务器的发布数据库中，执行 [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)。 为 \@subscriber_db 指定订阅服务器上包含已发布数据的数据库的名称，并将 \@subscription_type 的值指定为 push，将 \@sync_type 的值指定为 none    。  
  
3.  在发布服务器的发布数据库中，执行 [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)。 有关详细信息，请参阅 [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)。  
  
4.  启动合并代理以传输复制对象，并从发布服务器下载最新更改。 有关详细信息，请参阅 [同步推送订阅](../../relational-databases/replication/synchronize-a-push-subscription.md)。  
  
## <a name="see-also"></a>另请参阅  
 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)   
 [备份和还原复制的数据库](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
