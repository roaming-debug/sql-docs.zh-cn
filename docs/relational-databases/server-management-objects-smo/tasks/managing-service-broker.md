---
description: 管理 Service Broker
title: 管理 Service Broker
ms.custom: seo-dt-2019
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- Service Broker [SMO]
ms.assetid: b29d7432-d1e5-4bb6-b544-57b3a9430f95
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 03dd1f5c0eb1e8e1808d346521732b64cc035171
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475468"
---
# <a name="managing-service-broker"></a>管理 Service Broker

[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  在 SMO 中， [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 对象位于需要引用 Microsoft.SqlServer.Smo.dll 的 "... **管理** " 中。 为支持类信息，还要求对 Microsoft.SqlServer.ServiceBrokerEnum.dll 的引用。  
  
 SMO 提供一组 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 对象，这些对象允许以编程方式管理 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 的实现 (DDL)。 这包括定义消息类型、约定、队列和服务。 因为 SMO 是并不针对数据操作、发送和接收的管理工具，所以 SMO 不支持 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 消息。  
  
 在 SMO 中，<xref:Microsoft.SqlServer.Management.Smo.Database.ServiceBroker%2A> 对象是顶级类，所有 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 功能都位于其下。 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 实现是参与分布式消息处理应用程序的每个数据库所必需的。 因此，<xref:Microsoft.SqlServer.Management.Smo.Broker.ServiceBroker> 对象是 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象的子级。  
  
 <xref:Microsoft.SqlServer.Management.Smo.Broker.ServiceBroker> 对象包含用于定义 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 实现的以下对象的集合：  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Broker.MessageType> 对象表示定义消息内容的消息类型。  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Broker.MessageTypeMapping> 对象表示指定给定转换中消息的方向和类型的约定。  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Broker.ServiceQueue> 对象在发送消息前和收到消息后存储消息。 它们提供服务间的异步通信以及其他好处，例如自动锁定同一会话组中的消息。  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Broker.BrokerService> 对象表示 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 服务，这些服务是可寻址的会话端点。 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 消息从一个服务发送到另一个服务。 服务指定一个队列来保存消息，还指定一些约定，约定指明该服务可作为“目标”。  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Broker.RemoteServiceBinding> 对象表示 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 在与某一远程服务进行通信时用于安全性和身份验证的设置。  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Broker.ServiceRoute> 对象表示 [!INCLUDE[ssSB](../../../includes/sssb-md.md)] 路由，它包含所定义的服务和数据库的位置信息。 路由是消息传递所必需的。 默认情况下，每个数据库都包含一个路由，该路由将位置指定为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的当前实例。  
  
## <a name="see-also"></a>另请参阅  
 <xref:Microsoft.SqlServer.Management.Smo.Broker>   
 [SQL Server Service Broker](../../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
