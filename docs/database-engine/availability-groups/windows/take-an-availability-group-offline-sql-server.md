---
title: 使可用性组脱机 (SQL Server) | Microsoft Docs
description: 了解如何使用 SQL Server 中的 Transact-SQL 将 Always On 可用性组从 ONLINE 状态转换为 OFFLINE 状态。
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], take offline
ms.assetid: 50f5aad8-0dff-45ef-8350-f9596d3db898
author: cawrites
ms.author: chadam
ms.openlocfilehash: 49d2d529a511aa41ad69dc2326aa33decf0d48ef
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583779"
---
# <a name="take-an-availability-group-offline-sql-server"></a>使可用性组脱机 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何在 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 和更高版本中通过使用 [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] ，将某一 AlwaysOn 可用性组从 ONLINE 状态转换为 OFFLINE 状态。 对于同步提交数据库没有数据丢失，因为如果任何同步提交副本未同步，OFFLINE 操作将引发错误并且保持可用性组处于 ONLINE 状态。 保持可用性组处于联机状态将保护未同步的同步提交数据库，以防可能的数据丢失。 可用性组脱机后，其数据库将不可用于客户端，并且您无法使可用性组重新联机。 因此，使某一可用性组处于脱机状态只会将该可用性组的资源从一个 WSFC 群集迁移到另一个 WSFC 群集。  
  
 在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]的跨群集迁移过程中，如果任何应用程序直接连接到某一可用性组的主副本，则该可用性组必须置于脱机状态。 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的跨群集迁移支持用最短的可用性组停机时间进行操作系统升级。 典型的情形是将 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的跨群集迁移用于升级到 [!INCLUDE[win8](../../../includes/win8-md.md)] 或 [!INCLUDE[win8srv](../../../includes/win8srv-md.md)]的操作系统升级。 有关详细信息，请参阅 [针对操作系统升级的 AlwaysOn 可用性组的跨群集迁移](/previous-versions/sql/sql-server-2012/jj873730(v=msdn.10))。  
  
  
> [!CAUTION]  
>  使用“脱机”选项，可跨群集迁移可用性组资源，或故障转移读取扩展可用性组。
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   您对其输入 OFFLINE 命令的服务器实例必须正在运行 [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] 或更高版本（Enterprise Edition 或更高）。    
-   可用性组当前必须处于联机状态。  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
 在您使可用性组脱机之前，删除可用性组侦听器。 有关详细信息，请参阅 [删除可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)的操作系统升级。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **使可用性组脱机**  
  
1.  连接到为可用性组承载可用性副本的服务器实例。 此副本可以是主副本或辅助副本。  
  
2.  按如下所示使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) 语句：  
  
     ALTER AVAILABILITY GROUP *group_name* OFFLINE  
  
     其中， *group_name* 是可用性组的名称。  
  
### <a name="example"></a>示例  
 下面的示例将 `AccountsAG` 可用性组脱机。  
  
```  
ALTER AVAILABILITY GROUP AccountsAG OFFLINE;  
```  
  
##  <a name="follow-up-after-the-availability-group-goes-offline"></a><a name="FollowUp"></a> 跟进：在可用性组处于脱机状态后  
  
-   **OFFLINE 操作的日志记录：** 启动了 OFFLINE 操作的 WSFC 节点的标识存储于 WSFC 群集日志和 SQL ERRORLOG 中。  
  
-   **如果在使组脱机之前未删除可用性组侦听程序：** 如果要将可用性组迁移到其他 WSFC 群集，请删除其侦听器的 VNN 和 VIP。 你可以通过使用故障转移群集管理控制台、 [Remove-ClusterResource](https://technet.microsoft.com/library/ee461015\(WS.10\).aspx) PowerShell cmdlet 或 [cluster.exe](https://technet.microsoft.com/library/ee461015\(WS.10\).aspx)删除侦听程序的 VNN 和 VIP。 请注意，在 Windows 8 上不推荐使用 cluster.exe。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [删除可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [更改服务器实例的 HADR 群集上下文 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [SQL Server 2012 技术文章](https://msdn.microsoft.com/library/bb418445\(SQL.10\).aspx)  
  
-   [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
