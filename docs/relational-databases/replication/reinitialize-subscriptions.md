---
description: 重新初始化订阅
title: 重新初始化订阅 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- initializing subscriptions [SQL Server replication], reinitializing
- subscriptions [SQL Server replication], reinitializing
- reinitializing subscriptions
ms.assetid: fb13712b-e7ad-4f1f-b605-4554bad0cb60
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 739d977502331d245b78fcd22776a25590c66bc8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479808"
---
# <a name="reinitialize-subscriptions"></a>重新初始化订阅
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  重新初始化订阅包括对一个或多个订阅服务器应用一个或多个项目的新快照：事务复制和快照复制允许对各个项目单独重新初始化；而合并复制需要对所有项目重新初始化。 无法重新初始化对等事务复制拓扑中的节点。 如果需要确保节点有新的数据副本，请在该节点上还原备份。 对于下列一种或两种情况，将进行重新初始化：  
  
-   将订阅显式标记为重新初始化。  
  
-   执行一个需要重新初始化的操作，如属性更改。 有关需要重新初始化的操作的详细信息，请参阅[更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
 在这两种情况中，最新的快照将于下次分发代理或合并代理运行时应用到订阅服务器。 对于快照复制和事务复制，当发生重新初始化时，订阅服务器上所做的但尚未与发布服务器同步的任何更改都将在应用新快照时被覆盖。  
  
 对于合并复制，可以选择在应用快照之前从订阅服务器上载所有数据更改。 来自发布服务器的任何挂起架构更改都将应用到订阅服务器，之后，自上次同步以来订阅服务器上所做的任何更新将在重新应用快照之前传播到发布服务器。 该行为由 **upload_first** 和 **automatic_reinitialization_policy** 属性控制；有关详细信息，请参阅 [Reinitialize a Subscription](../../relational-databases/replication/reinitialize-a-subscription.md)。 如果使用 SQL Server Management Studio 或复制监视器将订阅标记为重新初始化，那么 **“重新初始化订阅”** 对话框中将给出一个选项，用来首先上载更改。  
  
> [!IMPORTANT]  
>  如果在合并发布中添加、删除或更改参数化筛选器，则在重新初始化过程中，订阅服务器上的挂起更改将无法上载到发布服务器。 若要上载挂起的更改，请在更改筛选器前同步所有订阅。  
  
 如果在创建发布时指定了不对订阅服务器应用初始快照，并将订阅标记为要重新初始化，则不应用快照。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
 **重新初始化订阅**  
  
 若要重新初始化订阅中的所有项目，请使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、存储过程或复制管理对象 (RMO)。 若要重新初始化快照发布和事务发布中的单个项目，必须使用存储过程。 有关详细信息，请参阅 [Reinitialize a Subscription](../../relational-databases/replication/reinitialize-a-subscription.md)。  
  
## <a name="see-also"></a>另请参阅  
 [初始化订阅](../../relational-databases/replication/initialize-a-subscription.md)   
 [订阅过期和停用](../../relational-databases/replication/subscription-expiration-and-deactivation.md)  
  
  
