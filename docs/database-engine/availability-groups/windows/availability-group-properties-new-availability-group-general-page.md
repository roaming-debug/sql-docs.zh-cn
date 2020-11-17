---
title: “常规”页面（“新建可用性组和属性”对话框）
titleSuffix: SQL Server
description: 介绍 SQL Server Management Studio (SSMS) 中“新建可用性组”和“可用性组属性”对话框的“常规”页面上提供的各种属性。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: end-user-help
f1_keywords:
- sql13.swb.availabilitygroupproperties.general.f1
ms.assetid: 9af5379f-91b8-4729-9f75-4a80242a30e9
author: cawrites
ms.author: chadam
ms.openlocfilehash: f7e570006908358b1cfe0cb07972113585e58a95
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584674"
---
# <a name="availability-group-properties-new-availability-group-general-page"></a>可用性组属性：新建可用性组（“常规”页）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题同时适用于 **“新建可用性组”** 对话框和 **“可用性组属性”** 对话框的 **“常规”** 选项卡。  **“新建可用性组”** 对话框支持您无需使用 [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]即创建新的可用性组。 **“可用性组属性”** 对话框支持您查看和修改现有的可用性组的配置。  
  
 **查看可用性组属性**  
  
-   [查看可用性组属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="ui-element-list"></a>UI 元素列表  
 **可用性组名称**  
 可用性组的名称。 这是在 Windows Server 故障转移群集 (WSFC) 内必须唯一的用户指定的名称。  
  
## <a name="availability-databases"></a>可用性数据库  
 **Database Name**  
 已添加到可用性组中的数据库的名称。  
  
 **添加**  
 单击此选项可将数据库添加到可用性组。  
  
 **删除**  
 单击此选项可从可用性组中删除所选数据库。  
  
## <a name="availability-replicas"></a>可用性副本  
 **服务器实例**  
 承载此副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的服务器名称；对于非默认实例，则为其实例名称。  
  
 **角色**  
 **主要节点**  
 当前主副本。  
  
 **辅助节点**  
 当前辅助副本。  
  
 **正在解析**  
 当前该副本角色处于正在解析为主角色或辅助角色的进程中。  
  
 **可用性模式**  
 副本的可用性模式，可为下列值之一：  
  
 **异步提交**  
 主副本可以不必等待辅助副本将日志写入磁盘，即可提交事务。  
  
 **同步提交**  
 主副本等待提交给定的事务，直到辅助副本将事务写入磁盘。  
  
 有关详细信息，请参阅 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。  
  
 **故障转移模式**  
 副本的故障转移模式，可为下列值之一：  
  
 **自动**  
 自动故障转移。 副本为自动故障转移的目标。 仅当可用性模式设置为同步提交时，才选择此选项。  
  
 **手动**  
 手动故障转移。 副本仅能由数据库管理员手动进行故障转移。  
  
 **主角色中的连接**  
 当副本拥有主角色时支持的客户端连接类型。  
  
 **允许所有连接**  
 主副本中的数据库允许所有连接。 这是默认设置。  
  
 **允许读/写连接**  
 不允许 Application Intent 连接属性设置为 **ReadOnly** 的连接。 在 Application Intent 属性设置为 **ReadWrite** 或者未设置 Application Intent 连接属性时，将允许连接。 有关 Application Intent 连接属性的详细信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
 **可读取辅助角色**  
 正在履行辅助角色的可用性副本（也就是辅助副本）是否可以接受来自客户端的连接，可为下列值之一：  
  
 **是**  
 不允许与此副本的辅助数据库的直接连接。 它们不可用于读访问。 这是默认设置。  
  
 **仅限读意向**  
 仅允许与此副本的辅助数据库的直接只读连接。 辅助数据库全都可用于读访问。  
  
 **是**  
 允许与此副本的辅助数据库的所有连接，但仅限读访问。 辅助数据库全都可用于读访问。  
  
 **会话超时（秒）**  
 此副本上会话超时期限的秒数。  
  
 **端点 URL**  
 端点的 URL。 有关这些 URL 格式的信息，请参阅[在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)。  
  
 **添加**  
 单击此选项可将辅助副本添加到可用性组。  
  
 **删除**  
 单击此选项可从可用性组中删除辅助副本。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
