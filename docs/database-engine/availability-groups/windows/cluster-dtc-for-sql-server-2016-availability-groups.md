---
title: 为可用性组聚集 DTC 服务
description: '介绍为 AlwaysOn 可用性组群集 Microsoft 分布式事务处理协调器 (DTC) 服务的要求和步骤。 '
ms.custom: seo-lt-2019
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: a47c5005-20e3-4880-945c-9f78d311af7a
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 981ce2dcfadf234383382103a33faf4f0ab9302e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643040"
---
# <a name="how-to-cluster-the-dtc-service-for-an-always-on-availability-group"></a>如何为 AlwaysOn 可用性组群集 DTC 服务

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

本主题介绍为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 群集化 Microsoft 分布式事务处理协调器 (DTC) 服务的要求和步骤。 有关分布式事务和 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]的其他信息，请参阅 [AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)。

 ## <a name="checklist-preliminary-requirements"></a>清单：初步要求

|任务|参考|  
|-----------------|----------|  
|确保已正确配置所有节点、服务和可用性组。|[针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)|
|确保已满足可用性组 DTC 的要求。|[AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)

## <a name="checklist-clustered-dtc-resource-dependencies"></a>清单：群集 DTC 资源的依赖关系

|任务|参考|  
|-----------------|----------|  
|共享存储驱动器。|[配置共享存储驱动器](https://msdn.microsoft.com/library/cc982358(v=bts.10).aspx)。 请考虑使用驱动器盘符 **M**。|
|唯一的 DTC 网络名称资源。  将该名称注册为 Active Directory 中的群集计算机对象。<br /><br />确保以下任一条件成立：<br /><br />• 创建 DTC 网络名称资源的用户对此 DTC 网络名称资源要驻留的 OU 或容器具有创建计算机对象的权限。<br /><br />• 如果用户不具有创建计算机对象的权限，让域管理员为 DTC 网络名称资源预安排群集计算机对象。|[在 Active Directory 域服务中预安排群集计算机对象](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn466519(v=ws.11))|
|有效的可用静态 IP 地址及其相应的子网掩码。||

## <a name="cluster-the-dtc-resource"></a>群集 DTC 资源
创建可用性组资源后，即可创建一个群集 DTC 资源并将其添加到可用性组中。  有关示例脚本，请参阅[为 AlwaysOn 可用性组创建群集 DTC](../../../database-engine/availability-groups/windows/create-clustered-dtc-for-an-always-on-availability-group.md)。


## <a name="checklist-post-clustered-dtc-resource-configurations"></a>清单：公布群集 DTC 资源配置

|任务|参考|  
|-----------------|----------|  
|为群集 DTC 资源安全启用网络访问权限。|[Enable Network Access Securely for MS DTC](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753620(v=ws.10))（为 MS DTC 安全启用网络访问权限）|
|停止并禁用本地 DTC 服务。|[配置服务启动方式](https://technet.microsoft.com/library/cc755249(v=ws.11).aspx)|
|为可用性组中的每个实例循环 SQL Server 服务。  根据需要故障转移可用性组。|[执行可用性组的计划手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)<br /><br />[启动、停止、暂停、继续、重新启动数据库引擎、SQL Server 代理或 SQL Server Browser 服务](../../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)|

- 如果服务器是 Windows Server 2012 R2，则操作系统必须应用 [KB 3030373](https://support.microsoft.com/kb/3090973) 。

- 根据 [针对 Always On 可用性组的先决条件、限制和建议](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)中的清单为可用性组准备服务器。

- 为 [**AlwaysOn 可用性组**](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)配置服务器实例。

### <a name="resources"></a>RESOURCES


[有关在可用性组上测试 DTC 的详细信息：](/archive/blogs/dataplatform/sql-server-2016-dtc-support-in-availability-groups)

[监视 AlwaysOn 可用性组系统视图](monitor-availability-groups-transact-sql.md)

[逐步创建可用性组](create-an-availability-group-transact-sql.md)


[SQL Server 2016 DTC Support in Availability Groups](/archive/blogs/dataplatform/sql-server-2016-dtc-support-in-availability-groups)（可用性组中的 SQL Server 2016 DTC 支持） 

[外部链接：通过 Windows Server 2008 R2 为 SQL Server 的群集实例配置 DTC](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/)