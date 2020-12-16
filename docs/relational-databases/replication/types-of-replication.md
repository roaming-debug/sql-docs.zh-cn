---
title: 复制类型 | Microsoft Docs
description: 了解 SQL Server 提供用于分布式应用程序的不同类型的复制。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], types
ms.assetid: c1655e8d-d14c-455a-a7f9-9d2f43e88ab4
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: a1f72eeaa2ffff2db3e536eec6f1aa317a2cb45c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468698"
---
# <a name="types-of-replication"></a>复制类型
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供以下类型的复制以用于分布式应用程序：  

| 类型 | **说明** |
|:-------- | :-------------- |
| [事务复制](transactional/transactional-replication.md)| 在发布服务器上进行的更改会在发生时（几乎实时）传递给订阅服务器。 数据更改将按照其在发布服务器上发生的顺序和事务边界应用于订阅服务器。 | 
| [合并复制](merge/merge-replication.md) | 可以在发布服务器和订阅服务器上更改数据，并且可以通过触发器进行跟踪。 订阅服务器在连接到网络时将与发布服务器进行同步，并交换自上次同步以来发布服务器和订阅服务器之间发生更改的所有行。 | 
| [快照复制](snapshot-replication.md) | 将来自发布服务器的快照应用于订阅服务器，这完全按照数据在特定时刻的状态分发数据，而不监视数据是否更新。 发生同步时，将生成完整的快照并将其发送到订阅服务器。| 
| [对等](transactional/peer-to-peer-transactional-replication.md) | 对等复制建立在事务复制的基础之上，以事务方式近乎实时地在多个服务器实例之间传播一致的更改。 | 
| [双向](transactional/bidirectional-transactional-replication.md)| 双向事务复制是一种特定的事务复制拓扑，它允许两台服务器相互交换更改：每台服务器均发布数据，然后从另一台服务器订阅包含相同数据的发布。 | 
| [可更新订阅](transactional/updatable-subscriptions-for-transactional-replication.md) | 以事务复制为基础而构建，在订阅服务器上针对可更新订阅更新了数据时，会首先传播到发布服务器，然后传播到其他订阅服务器。 | 
  
 
为应用程序选择的复制类型取决于多种因素，其中包括实际复制环境、要复制的数据类型和数量，以及是否在订阅服务器上更新数据等等。 实际环境包括复制中所涉及的计算机数量和位置，以及这些计算机是客户端（工作站、便携式电脑或手持设备）还是服务器。  
  
每种复制类型通常都开始于发布服务器和订阅服务器之间的已发布对象的初始同步。 此初始同步可以由带有“快照” 的复制执行，该快照为发布所指定的所有对象和数据的副本。 快照在创建之后，便被传递到订阅服务器。 对于某些应用程序而言，只需快照复制即可。 对于另一些类型的应用程序而言，后续数据更改应随着时间而增量式地传递到订阅服务器，这一点很重要。 某些应用程序也需要更改从订阅服务器传递回发布服务器。 事务复制和合并复制为这些类型的应用程序提供了若干选项。  
  
 
## <a name="see-also"></a>另请参阅  
 [复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)
  
  
