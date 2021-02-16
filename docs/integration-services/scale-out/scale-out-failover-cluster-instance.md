---
title: Scale Out 通过 SQL Server 故障转移群集实例对高可用性的支持 | Microsoft Docs
description: 本文介绍如何通过 SQL Server 故障转移群集实例配置高可用性 SSIS Scale Out
ms.custom: performance
ms.date: 04/10/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 444226f7e0f28a6f587a5f01b1e512f2392e9916
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351964"
---
# <a name="scale-out-support-for-high-availability-via-sql-server-failover-cluster-instance"></a>Scale Out 通过 SQL Server 故障转移群集实例对高可用性的支持

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



要使用 SQL Server 故障转移群集实例设置 Scale Out Master 端的高可用性，请执行以下步骤：

## <a name="1-prerequisites"></a>1.先决条件
设置 Windows 故障转移群集。 有关说明，请参阅博客文章[安装适用于 Windows Server 2012 的故障转移群集功能和工具](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733)。 在所有群集节点上安装功能和工具。

## <a name="2-install-sql-server-failover-cluster"></a>2.安装 SQL Server 故障转移群集
安装 SQL Server 故障转移群集。 有关说明，请参阅 [SQL Server 故障转移群集安装](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)。 安装过程中，在“功能选择”页上选择“数据库引擎服务”。 记录 SQL Server 网络名称，以便进一步配置。

![SQL 网络名称](media/sql-network-name.PNG)

向现有 SQL Server 故障转移群集中添加辅助节点。

## <a name="3-install-scale-out-master-on-the-primary-node"></a>3.在主节点上安装 Scale Out Master
使用非聚集安装的安装向导，在主节点上安装 Integration Services 和 Scale Out Master。 

安装过程中，在 Scale Out Master 证书的 CN 中添加 SQL Server 网络名称。

> [!NOTE]
> 如果要单独故障转移 SSISDB 和 Scale Out Master，请按照 [2.在主节点上安装 Scale Out Master](scale-out-support-for-high-availability.md#2-install-scale-out-master-on-the-primary-node) 进行 Scale Out Master 配置。

## <a name="4-install-scale-out-master-on-the-secondary-node"></a>4.在辅助节点上安装 Scale Out Master
按照 [3.在辅助节点上安装 Scale Out Master](scale-out-support-for-high-availability.md#3-install-scale-out-master-on-the-secondary-node) 操作

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5.更新 Scale Out Master 服务配置文件
更新主节点和辅助节点上的 Scale Out Master 服务配置文件 \<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config。 将 SqlServerName 更新为 [SQL Server 网络名称]//[实例名称] 或针对默认实例更新为 [SQL Server 网络名称]  。

## <a name="6-add-scale-out-master-service-to-sql-server-role-in-windows-failover-cluster"></a>6.向 Windows 故障转移群集中的 SQL Server 角色添加 Scale Out Master
在故障转移群集管理器中，连接到 Scale Out 的群集。在资源管理器中选择“角色”，右键单击 SQL Server 角色，然后依次选择“添加资源”和“通用服务”。 

![通用服务](media/generic-service.PNG)

在“新建资源向导”中，选择“Scale Out Master”服务并完成向导。 

使 Scale Out Master 服务联机。

![联机](media/bring-online.PNG)

> [!NOTE]
> 如果要单独故障转移 SSISDB 和 Scale Out Master，请按照 [7.配置 Windows 故障转移群集的 Scale Out Master 服务角色](scale-out-support-for-high-availability.md#7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster)操作

## <a name="7-install-scale-out-workers"></a>7.安装 Scale Out Worker
在辅助角色节点上安装 Scale Out Worker。 安装过程中，指定 https://[Sql Server 网络名称]:[主端口] 作为主终结点。 

> [!NOTE]
> 如果要单独故障转移 SSISDB 和 Scale Out Master，请指定 Scale Out Master 服务 DNS 主机名，而不是 Sql Server 网络名称。

## <a name="8-install-scale-out-worker-client-certificate"></a>8.安装 Scale Out Worker 客户端证书
在 SQL Server 故障转移群集的所有节点上安装辅助角色证书。 请参阅[安装 Scale Out Worker 客户端证书](walkthrough-set-up-integration-services-scale-out.md#InstallCert)。

> [!NOTE]
> Scale Out Manager 尚不支持 SQL Server 故障转移群集。 如果使用 Scale Out Manager 添加 Scale Out Worker，仍需要将辅助角色证书手动安装到所有主节点。

## <a name="next-steps"></a>后续步骤
有关详细信息，请参阅以下文章：
-   [Integration Services (SSIS) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [Integration Services (SSIS) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
