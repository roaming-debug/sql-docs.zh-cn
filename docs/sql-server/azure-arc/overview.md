---
title: 已启用 Azure Arc 的 SQL Server
titleSuffix: ''
description: 使用已启用 Azure Arc 的 SQL Server 管理 SQL Server 实例
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: references_regions
ms.openlocfilehash: c1ba7f7552b5050e3c1fa7bc765acfa431f3df30
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103142"
---
# <a name="azure-arc-enabled-sql-server-preview"></a>已启用 Azure Arc 的 SQL Server（预览）

已启用 Azure Arc 的 SQL Server 是 Azure Arc for servers 的一部分。 它将 Azure 服务扩展到 SQL Server 实例，这些实例托管在客户的数据中心、边缘或多云环境中的 Azure 外部。

若要启用 Azure 服务，必须使用 Azure 门户和注册脚本向 Azure Arc 注册运行的 SQL Server 实例。 注册后，该实例将在 Azure 上表示为“SQL Server - Azure Arc”资源。 此资源的属性反映一部分 SQL Server 配置设置。

可以在运行 Windows 或 Linux 的虚拟机或物理计算机上安装 SQL Server，Windows 或 Linux 通过 Connected Machine 代理连接到 Azure Arc。 只要安装了该代理，计算机就会在 SQL Server 实例注册过程中自动进行注册。 Connected Machine 代理通过 TCP 端口 443 安全地与 Azure Arc 进行出站通信。 如果计算机通过防火墙或 HTTP 代理服务器建立连接以通过 Internet 进行通信，请查看 [Connected Machine 的网络配置要求](/azure/azure-arc/servers/agent-overview#prerequisites)。

已启用 Azure Arc 的 SQL Server 的公共预览版支持一组解决方案，这些解决方案需要安装 Microsoft Monitoring Agent (MMA) 服务器扩展并将其连接到 Azure Log Analytics 工作区进行数据收集和报告。 这些解决方案包括使用 Azure 安全中心和 Azure Sentinel 的高级数据安全，以及使用按需 SQL 评估功能的 SQL 环境运行状况检查。

下图说明了已启用 Azure Arc 的 SQL Server 的体系结构。

![公共预览版体系结构](media/overview/pubic-preview-architecture.png)

## <a name="prerequisites"></a>必备知识

### <a name="supported-sql-versions-and-operating-systems"></a>支持的 SQL 版本和操作系统

已启用 Azure Arc 的 SQL Server 支持在以下版本的 Windows 或 Linux 操作系统之一上运行的 SQL Server 2012 或更高版本：

- Windows Server 2012 R2 和更高版本
- Ubuntu 16.04 和 18.04 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64) 
- SUSE Linux Enterprise Server (SLES) 15 (x64)

### <a name="required-permissions"></a>所需的权限

要将 SQL Server 实例和宿主计算机连接到 Azure Arc，必须有一个帐户且该帐户有权执行以下操作：
   * Microsoft.AzureArcData/sqlServerInstances/read
   * Microsoft.AzureArcData/sqlServerInstances/write
   * Microsoft.HybridCompute/machines/read
   * Microsoft.HybridCompute/machines/write
   * Microsoft.GuestConfiguration/guestConfigurationAssignments/read

为了获得最佳安全性，我们建议在 Azure 中创建自定义角色并列出基础权限。 若要了解如何在 Azure 中创建自定义角色并配置基础权限，请参阅[自定义角色概述](/azure/active-directory/users-groups-roles/roles-custom-overview)。 若要添加角色分配，请参阅[使用 Azure 门户添加或删除角色分配](/azure/role-based-access-control/role-assignments-portal)或[使用 Azure RBAC 和 Azure CLI 添加或删除角色分配](/azure/role-based-access-control/role-assignments-cli)。

### <a name="azure-subscription-and-service-limits"></a>Azure 订阅和服务限制

在为 SQL Server 实例和计算机配置 Azure Arc 之前，应查看 Azure 资源管理器[订阅限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#subscription-limits)和[资源组限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits)，以规划要连接的计算机数。

### <a name="networking-configuration-and-resource-providers"></a>网络配置和资源提供程序

查看 Connected Machine 代理所需的[网络配置、传输层安全性和资源提供程序](/azure/azure-arc/servers/agent-overview#prerequisites)。

将 SQL Server 实例连接到 Azure Arc 需要资源提供程序 `Microsoft.AzureArcData`。请参阅[先决条件](connect.md#prerequisites)部分中的资源提供程序注册说明。

如果已将 SQL Server 实例连接到 Azure Arc，请按照以下步骤将现有“SQL Server - Azure Arc”资源迁移到新的命名空间。

### <a name="supported-azure-regions"></a>支持的 Azure 区域

此公共预览版已在以下区域推出：
- 美国东部
- 美国东部 2
- 美国西部 2
- 澳大利亚东部
- Southeast Asia
- 北欧
- 西欧
- 英国南部

## <a name="next-steps"></a>后续步骤

- [将 SQL Server 连接到 Azure Arc](connect.md)
- [使用按需 SQL 评估配置 SQL Server 实例以定期检查环境运行状况](assess.md)
- [为 SQL Server 实例配置高级数据安全](configure-advanced-data-security.md)