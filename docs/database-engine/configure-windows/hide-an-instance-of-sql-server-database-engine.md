---
title: 隐藏 SQL Server 数据库引擎的实例 | Microsoft Docs
description: 了解如何隐藏 SQL Server 数据库引擎实例。 客户端计算机不能使用 SQL Server Browser 服务来定位隐藏实例。
ms.custom: ''
ms.date: 08/19/2015
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], hiding instances
- hiding instances of Database Engine
ms.assetid: 392de21a-57fa-4a69-8237-ced8ca86ed1d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0e4c13745057b1276f06f3bc77ace266921a43d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076269"
---
# <a name="hide-an-instance-of-sql-server-database-engine"></a>隐藏 SQL Server 数据库引擎的实例
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 SQL Server 配置管理器在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中隐藏 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 实例。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器服务来枚举安装在计算机上的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例。 这使客户端应用程序可以浏览服务器，并帮助客户端区别同一台计算机上的多个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例。 您可以使用以下过程防止 SQL Server Browser 服务向尝试通过使用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] “浏览” **按钮来查找实例的客户端计算机公开** 实例。  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> 使用 SQL Server 配置管理器  
  
#### <a name="to-hide-an-instance-of-the-sql-server-database-engine"></a>隐藏 SQL Server 数据库引擎实例  
  
1.  在“SQL Server 配置管理器”中，展开“SQL Server 网络配置”，右键单击“\<server instance> 的协议”，然后选择“属性”。    
  
2.  在 **“标志”** 选项卡的 **“隐藏实例”** 框中，选择 **“是”** ，然后单击 **“确定”** 关闭对话框。 对于新连接，更改会立即生效。  
  
## <a name="remarks"></a>备注  
 如果你隐藏命名实例，你将需要在连接字符串中提供端口号才能连接到隐藏的实例，即使浏览器服务正在运行也是如此。 对于命名隐藏实例，我们建议你使用静态端口而不是动态端口。  
  有关详细信息，请参阅[将服务器配置为侦听特定 TCP 端口（SQL Sever 配置管理器）](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)。  
  
### <a name="clustering"></a>群集  
 如果隐藏群集实例或可用性组名称，则群集服务可能无法连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 这将导致群集实例的“IsAlive”检查失败，并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将进入离线状态。 
 
若要避免此情况发生，建议在群集实例的所有节点或承载可用性组副本的所有实例中创建别名，以反映你为该实例配置的静态端口。  例如，在具有两个副本的可用性组上，在节点 1 上为节点 2 实例创建一个别名，如 `node-two\instancename`。 在节点 2 上，创建一个名为 `node-one\instancename` 的别名。 成功故障转移需要别名。 
 
 有关详细信息，请参阅[创建或删除供客户端使用的服务器别名（SQL Server 配置管理器）](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md)。  
  
 如果隐藏群集命名实例，当 **LastConnect** 注册表项 (**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SNI11.0\LastConnect**) 具有的端口与[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在侦听的端口不同时，群集服务可能无法连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 如果群集服务无法建立与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的连接，则你可能会看到类似于以下内容的错误：  
**事件 ID：1001：事件名称：故障转移群集资源死锁。**  
  
## <a name="see-also"></a>另请参阅  
 [服务器网络配置](../../database-engine/configure-windows/server-network-configuration.md)   
 [SQL 虚拟服务器客户端连接的说明](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/273673)   
 [如何将静态端口分配到 SQL Server 命名实例并避免常见缺陷](https://deep.data.blog/2012/09/08/how-to-assign-a-static-port-to-a-sql-server-named-instance-and-avoid-a-common-pitfall/)  
  
  
