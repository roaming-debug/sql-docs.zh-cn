---
title: 订阅发布 | Microsoft Docs
description: 了解 SQL Server 复制中的订阅类型、如何选择所需类型，以及如何创建订阅。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.conc.subtopubs.f1
helpviewer_keywords:
- subscriptions [SQL Server replication], about subscriptions
- pull subscriptions [SQL Server replication]
- subscriptions [SQL Server replication]
- push subscriptions [SQL Server replication], about push subscriptions
- merge replication subscribing [SQL Server replication]
- merge replication subscribing [SQL Server replication], about subscribing
- publications [SQL Server replication], subscribing
- push subscriptions [SQL Server replication]
- pull subscriptions [SQL Server replication], about pull subscriptions
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: 088ee30a-05ab-47c4-92ed-316b93e12445
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 3485dfb43f03e8391916f01bfc1cedcad04e57dc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479638"
---
# <a name="subscribe-to-publications"></a>订阅发布
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  订阅是对发布中的数据和数据库对象的副本的请求。 订阅定义将接收哪个发布以及接收的时间和位置。 在计划订阅时，请考虑代理处理发生的位置。 所选择的订阅类型将控制代理运行的位置。 对于推送订阅，合并代理或分发代理在分发服务器上运行；对于请求订阅，代理在订阅服务器上运行。 创建订阅后，将无法更改其类型。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
|订阅|特征|使用时间|  
|------------------|---------------------|--------------|  
|推送订阅|对于推送订阅，发布服务器将更改传播到订阅服务器，而无需订阅服务器发出请求。 更改可以按需、连续地或按照计划推送到订阅服务器。 分发代理或合并代理在分发服务器上运行。|通常，数据将连续同步或按照经常重复执行的计划同步。<br /><br /> 发布要求数据近似实时地移动。<br /><br /> 分发服务器上较高的处理器开销不会影响性能。<br /><br /> 通常与快照和事务复制一起使用。|  
|请求订阅|对于请求订阅，订阅服务器请求在发布服务器上所做的更改。 请求订阅允许订阅服务器上的用户确定同步数据更改的时间。 分发代理或合并代理在订阅服务器上运行。|数据通常按需或按计划同步，而非连续同步。<br /><br /> 发布具有大量订阅服务器，并且/或在分发服务器上运行所有代理会消耗大量资源。<br /><br /> 订阅服务器是自主的、断开连接的和/或移动的。 订阅服务器将确定连接和同步更改的时间。<br /><br /> 通常与合并复制一起使用。|  
  
## <a name="merge-replication-subscription-types"></a>合并复制订阅类型  
 所有复制类型都允许推送订阅和请求订阅。 合并复制使用另外两个术语来区分订阅：客户端订阅和服务器订阅。 客户端订阅和服务器订阅类型都可用于推送订阅和请求订阅。 客户端订阅适合于大多数订阅服务器，而服务器订阅通常用于向其他订阅服务器重新发布数据的订阅服务器。 订阅选择还会影响冲突解决。  
  
## <a name="non-sql-server-subscribers"></a>Non-SQL Server Subscribers  
 Oracle 和 IBM DB2 可以使用推送订阅来订阅快照和事务发布。 有关详细信息，请参阅 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)。  
  
## <a name="creating-subscriptions"></a>创建订阅  
 若要创建订阅，请提供下列信息：  
  
-   发布的名称。  
  
-   订阅服务器和订阅数据库的名称。  
  
-   分发代理或合并代理是在分发服务器上运行还是在订阅服务器上运行。  
  
-   分发代理或合并代理是连续运行、按照计划运行，还是仅按需运行。  
  
-   快照代理是否应为订阅创建初始快照，以及分发代理或合并代理是否应在订阅服务器上应用该快照。  
  
-   将运行分发代理或合并代理的帐户。  
  
-   对于合并复制，还要提供订阅类型：服务器或客户端。  
  
 **创建推送订阅**  
  
 [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)  
  
 **查看或修改推送订阅属性**  
  
 [查看和修改推送订阅属性](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)  
  
 **删除推送订阅**  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]：[删除推送订阅](../../relational-databases/replication/delete-a-push-subscription.md)  
  
> [!NOTE]  
>  删除订阅不会从订阅服务器中删除已发布的对象。  
  
 **创建请求订阅**  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]：[创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)  
  
 **查看或修改请求订阅属性**  
  
 [查看和修改请求订阅属性](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  
  
 **删除请求订阅**  
  
 [删除请求订阅](../../relational-databases/replication/delete-a-pull-subscription.md)  
  
## <a name="see-also"></a>另请参阅  
 [保护订阅服务器](../../relational-databases/replication/security/secure-the-subscriber.md)   
 [订阅过期和停用](../../relational-databases/replication/subscription-expiration-and-deactivation.md)  
  
  
