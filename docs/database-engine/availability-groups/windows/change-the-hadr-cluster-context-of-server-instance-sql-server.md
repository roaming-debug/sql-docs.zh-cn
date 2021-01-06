---
title: 更改元数据：跨群集可用性组迁移
description: 执行跨群集迁移时，通过更改 SQL Server 实例的 HADR 群集上下文，更改管理 Always On 可用性组中可用性副本元数据的群集。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- Availability replicas [SQL Server], change WSFC cluster context
ms.assetid: ecd99f91-b9a2-4737-994e-507065a12f80
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: bce85840c2a0b2aa1cabbbbeb6d518b9c9c4acb4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643094"
---
# <a name="change-which-cluster-manages-the-metadata-for-replicas-in-an-always-on-availability-group"></a>更改管理 Always On 可用性组中副本元数据的群集

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  本主题介绍如何通过在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和更高版本中使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] ，切换 [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] 实例的 HADR 群集上下文。 *HADR 群集上下文* 用于确定哪一 Windows Server 故障转移群集 (WSFC) 群集管理服务器实例所承载的可用性副本的元数据。  
  
 仅在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 跨群集迁移到新 WSFC 群集上的 [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] 实例的过程中切换 HADR 群集上下文。 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的跨群集迁移支持用最短的可用性组停机时间将操作系统升级到 [!INCLUDE[win8](../../../includes/win8-md.md)] 或 [!INCLUDE[win8srv](../../../includes/win8srv-md.md)] 。 有关详细信息，请参阅 [针对操作系统升级的 AlwaysOn 可用性组的跨群集迁移](/previous-versions/sql/sql-server-2012/jj873730(v=msdn.10))。  
  
> [!CAUTION]  
>  仅在跨群集迁移 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 部署的过程中切换 HADR 群集上下文。  
  
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   您只能将 HADR 群集上下文从本地 WSFC 群集切换到远程群集，然后再从远程群集切换回本地群集。 不能将 HADR 群集上下文从一个远程群集切换到另一个远程群集。  
  
-   只有在 SQL Server 实例不承载任何可用性副本时，才能将 HADR 群集上下文切换到远程群集。  
  
-   远程 HADR 群集上下文随时可以切换回本地群集。 但是，只要服务器实例承载任何可用性副本，该上下文将无法再进行切换。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   您对其更改 HADR 群集上下文的服务器实例必须正在运行 [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] 或更高版本（Enterprise Edition 或更高）。  
  
-   必须为 AlwaysOn 启用该服务器实例。 有关详细信息，请参阅[启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)。  
  
-   为了符合从本地群集上下文切换到远程群集上下文的条件，服务器实例不能承载任何可用性副本。 [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md) 目录视图不应返回任何行。  
  
     如果在服务器实例上存在任何可用性副本，则您必须首先执行以下操作之一，然后才能更改 HADR 群集上下文：  
  
    |副本角色|操作|链接|  
    |------------------|------------|----------|  
    |主|使可用性组脱机。|[使可用性组脱机 (SQL Server)](../../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)|  
    |辅助副本|从其可用性组中删除副本|[将次要副本从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)|  
  
-   在您可以从远程群集切换到本地群集之前，所有同步提交副本必须都处于 SYNCHRONIZED 状态。  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   我们建议您指定完整的域名。 这是因为为了找到短名称的目标 IP 地址，ALTER SERVER CONFIGURATION 使用 DNS 名称解析。 在某些情况下，根据 DNS 搜索顺序，使用短名称可能会导致混淆。 例如，考虑以下命令，该命令在 `abc` 域 (`node1.abc.com`) 中的节点上执行。 意向的目标群集是 `CLUS01` 域 ( `xyz` ) 中的`clus01.xyz.com`群集。 但是，本地域主机还承载名为 `CLUS01` (`clus01.abc.com`) 的一个群集。  
  
     如果已指定目标群集 `CLUS01`的短名称，则 DNS 名称解析可能会返回错误群集 `clus01.abc.com`的 IP 地址。 为了避免此类混淆，请指定目标群集的完整名称，如下例中所示：  
  
    ```  
    ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com'  
    ```  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
-   **SQL Server 登录名**  
  
     需要 CONTROL SERVER 权限。  
  
-   **SQL Server 服务帐户**  
  
     服务器实例的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户必须具有：  
  
    -   打开目标 WSFC 群集的权限。  
  
    -   远程 WSFC 读写访问权限。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **更改可用性副本的 WSFC 群集上下文**  
  
1.  连接到承载可用性组的主副本或辅助副本的服务器实例。  
  
2.  使用 [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md) 语句的 SET HADR CLUSTER CONTEXT 子句，如下所示：  
  
     ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT **=** { **'** _windows\_cluster_ **'** | LOCAL }  
  
     其中：  
  
     *windows_cluster*  
     WSFC 群集的群集对象名称 (CON)。 您可以指定短名称或者完整的域名称。 我们建议您指定完整的域名。 有关详细信息，请参阅本主题前面的 [建议](#Recommendations)。  
  
     LOCAL  
     本地 WSFC 群集。  
  
### <a name="examples"></a>示例  
 下面的示例将 HADR 群集上下文更改为其他群集。 若要标识目标 WSFC 群集 ( `clus01`)，本示例指定完整的群集对象名称 ( `clus01.xyz.com`)。  
  
```  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com';  
```  
  
 下面的示例将 HADR 群集上下文更改为本地 WSFC 群集。  
  
```  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = LOCAL;  
```  
  
##  <a name="follow-up-after-switching-the-cluster-context-of-an-availability-replica"></a><a name="FollowUp"></a> 跟进：在切换可用性副本的群集上下文后  
 新的 HADR 群集上下文将立即生效，而不重新启动服务器实例。 HADR 群集上下文设置是持久的实例级别设置，在服务器实例重新启动时也保持不变。  
  
 通过查询 [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md) 动态管理视图确认新的 HADR 群集上下文，如下所示：  
  
```  
SELECT cluster_name FROM sys.dm_hadr_cluster  
```  
  
 此查询应返回为其设置 HADR 群集上下文的群集的名称。  
  
 在 HADR 群集上下文切换为新的群集时：  
  
-   对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的实例当前承载的任何可用性副本，将清除元数据。  
  
-   以前属于可用性副本的所有数据库现在都处于 RESTORING 状态。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [删除可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [使可用性组脱机 (SQL Server)](../../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)  
  
-   [将辅助副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [将次要副本从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [SQL Server 2012 技术文章](https://msdn.microsoft.com/library/bb418445\(SQL.10\).aspx)  
  
-   [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Windows Server 故障转移群集 (WSFC) 与 SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [ALTER SERVER CONFIGURATION (Transact-SQL)](../../../t-sql/statements/alter-server-configuration-transact-sql.md)  
  
