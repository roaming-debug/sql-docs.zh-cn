---
description: '&lt;AgentName&gt; 代理位置'
title: '&lt;AgentName&gt; 代理位置 | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.agentlocation.f1
ms.assetid: dc664d80-fbe3-4586-aba8-a71fa62d14f0
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0155e73c264c04ce875284489ebaea8f72a78d96
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467368"
---
# <a name="ltagentnamegt-agent-location"></a>&lt;AgentName&gt; 代理位置
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  合并代理（对于合并订阅）和分发代理（对于事务订阅和快照订阅）运行在分发服务器或订阅服务器上。 如果代理运行在分发服务器上，则订阅称为推送订阅；如果代理运行在订阅服务器上，则订阅称为请求订阅。 有关推送订阅和请求订阅的详细信息，请参阅[订阅发布](../../relational-databases/replication/subscribe-to-publications.md)。 通过执行向导创建的所有订阅，其类型均为在向导中所选择的类型。 若要创建两种类型的订阅，必须运行两次向导。  
  
> [!NOTE]  
>  创建订阅之后，不能更改订阅类型。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [ssSDSFull](../../relational-databases/replication/create-a-push-subscription.md)   
 [复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
