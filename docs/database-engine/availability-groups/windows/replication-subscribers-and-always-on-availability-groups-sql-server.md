---
title: 复制订阅服务器和可用性组 (SQL Server)
description: 了解当包含作为复制订阅服务器的数据库的 AlwaysOn 可用性组在 SQL Server 中发生故障转移时会发生什么。
ms.custom: seo-lt-2019
ms.date: 08/08/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- failover subscribers with AlwaysOn
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 0995f269-0580-43ed-b8bf-02b9ad2d7ee6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 310438f8a5f8bec6fa0b2b3ef5b17eb1b097f423
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344594"
---
# <a name="replication-subscribers-and-always-on-availability-groups-sql-server"></a>复制订阅服务器和 AlwaysOn 可用性组 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  当包含作为复制订阅服务器的数据库的 AlwaysOn 可用性组发生故障转移时，复制订阅可能会失败。 对于事务复制推送订阅服务器，如果订阅是使用 AG 侦听器名称创建的，则在故障转移后，分发代理会自动继续复制。 对于事务复制拉取订阅服务器，如果订阅是使用 AG 侦听器名称创建的，且原始订阅服务器已启动并正在运行，则在故障转移后，分发代理会自动继续复制。 这是因为仅在原始订阅服务器（AG 的主要副本）上创建分发代理作业。 对于合并订阅服务器，复制管理员必须通过重新创建订阅手动重新配置订阅服务器。  
  
## <a name="what-is-supported"></a>支持的操作  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制支持自动对发布服务器和事务订阅服务器进行故障转移。 合并订阅服务器可以属于可用性组，但必须在故障转移后通过手动操作来配置新订阅服务器。 可用性组无法与 Websync 和 SQL Server Compact 方案结合使用。  
  
## <a name="how-to-create-transactional-subscription-in-an-always-on-environment"></a>如何在 AlwaysOn 环境中创建事务订阅  
 对于事务复制，请执行以下步骤配置订阅服务器可用性组并对其进行故障转移：  
  
1.  在创建订阅之前，请将订阅服务器数据库添加到合适的 AlwaysOn 可用性组。  
  
2.  将订阅服务器的可用性组侦听器作为链接服务器添加到可用性组的所有节点。 此步骤可确保所有潜在故障转移伙伴能够知道并连接到侦听器。  
  
3.  使用下列 **创建事务复制推送订阅** 部分中的脚本，并使用订阅服务器的可用性组侦听器的名称创建订阅。 在进行故障转移后，侦听器名称将始终有效，并且订阅服务器的实际服务器名称将取决于成为新的主节点的实际节点。  
  
    > [!NOTE]  
    >  订阅必须通过使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本创建，不能使用 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]创建。  
  
4.  若要创建请求订阅，请执行以下操作：  
  
    1.  通过下面“创建事务复制请求订阅”部分中的示例脚本，使用订阅服务器的可用性组侦听程序的名称来创建订阅。 
   
    2.  故障转移后，使用 sp_addpullsubscription_agent 存储过程，在新的主要副本上创建分发代理作业。 
  
 如果在每次故障转移后使用可用性组中的订阅数据库来创建请求订阅，建议在旧的主要副本上禁用分发代理作业，并在新的主要副本上启用此作业。  
  
## <a name="creating-a-transactional-replication-push-subscription"></a>创建事务复制推送订阅  
  
```  
-- commands to execute at the publisher, in the publisher database:  
use [<publisher database name>]  
EXEC sp_addsubscription @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @destination_db = N'<subscriber database name>',   
       @subscription_type = N'Push',   
       @sync_type = N'automatic', @article = N'all', @update_mode = N'read only', @subscriber_type = 0;  
GO  
  
EXEC sp_addpushsubscription_agent @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @subscriber_db = N'<subscriber database name>',   
       @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO  
```  

## <a name="creating-a-transactional-replication-pull-subscription"></a>创建事务复制请求订阅  
  
```  
-- commands to execute at the subscriber, in the subscriber database:  
use [<subscriber database name>]  
EXEC sp_addpullsubscription @publisher= N'<publisher name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>',
        @subscription_type = N'pull';
Go

EXEC sp_addpullsubscription_agent 
        @publisher =  N'<publisher name>', 
        @subscriber = N'<availability group listener name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>' ;
        @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO
```  
  
## <a name="to-resume-the-merge-agents-after-the-availability-group-of-the-subscriber-fails-over"></a>在订阅服务器可用性组发生故障转移后恢复合并代理  
 进行合并复制时，复制管理员必须手动重新配置订阅服务器，步骤如下：  
  
1.  执行 **sp_subscription_cleanup** 以删除订阅服务器上旧的订阅。 在新的主副本（以前为辅助副本）上执行此操作。  
  
2.  通过从新快照开始创建新订阅来重新创建订阅。  
  
> [!NOTE]  
>  当前过程对于合并复制订阅服务器是不方便的，不过，合并复制的主要应用场景是断开连接的用户（台式机、笔记本电脑、手机设备），它们不使用订阅服务器上的 AlwaysOn 可用性组。  
  
  
