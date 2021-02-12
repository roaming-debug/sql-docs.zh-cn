---
title: 什么是控制器？
titleSuffix: SQL Server big data clusters
description: 本文介绍 SQL Server 大数据群集的控制器。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9e2a7413578a8625231ffa0dad1750e98445f339
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039437"
---
# <a name="what-is-the-controller-on-a-sql-server-big-data-cluster"></a>什么是 SQL Server 大数据群集上的控制器？

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

控制器托管用于部署和管理 SQL Server 大数据群集的核心逻辑。 它负责与 Kubernetes、作为群集一部分的 SQL Server 实例以及 HDFS 和 Spark 等其他组件的所有交互。

控制器服务提供以下核心功能：

- 管理群集生命周期：群集启动和删除、更新配置
- 管理 SQL Server 主实例
- 管理计算池、数据池和存储池
- 公开监视工具以观察群集的状态
- 公开故障排除工具以检测和修复意外问题
- 管理群集安全性：
  - 确保提供安全的群集终结点
  - 管理用户和角色
  - 配置群集内通信的凭据

## <a name="deploying-the-controller-service"></a>部署控制器服务

控制器部署并托管在客户想要在其中生成大数据群集的同一 Kubernetes 命名空间中。 此服务由 Kubernetes 管理员在群集启动期间使用 **azdata** 命令行实用工具安装。 有关详细信息，请参阅 [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 入门](deploy-get-started.md)。

生成工作流将在 Kubernetes 之上布置一个功能齐全的 SQL Server 大数据群集，其中包含[概述](big-data-cluster-overview.md)一文中介绍的所有组件。 启动工作流首先创建控制器服务，一旦部署完成，控制器服务将协调主、计算、数据和存储池的其余服务部分的安装和配置。

## <a name="managing-the-cluster-through-the-controller-service"></a>通过控制器服务管理群集

可以使用任一 **azdata** 命令通过控制器服务管理群集。 如果将其他 Kubernetes 对象（如 pod）部署到同一命名空间中，控制器服务不会管理或监视它们。 还可使用 **kubectl** 命令在 Kubernetes 级别管理群集。 有关详细信息，请参阅[监视 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 并对其进行故障排除](cluster-troubleshooting-commands.md)。

为大数据群集创建的控制器和 Kubernetes 对象（有状态集、pod、机密等）驻留在专用的 Kubernetes 命名空间中。 控制器服务将由 Kubernetes 群集管理员授予管理该命名空间内所有资源的权限。  此方案的 RBAC 策略使用 **azdata** 自动配置为初始群集部署的一部分。

### <a name="azdata"></a>azdata

**azdata** 是使用 Python 编写的命令行实用工具，可让群集管理员通过控制器服务公开的 REST API 启动和管理大数据群集。

## <a name="controller-service-security"></a>控制器服务安全性

与控制器服务的所有通信都通过 HTTPS 上的 REST API 进行。 在启动时，将自动为你生成自签名证书。 

对控制器服务终结点的身份验证可使用 Active Directory 标识，也可基于用户名和密码。 使用环境变量 `AZDATA_USERNAME` 和 `AZDATA_PASSWORD` 的输入在群集启动时预配这些凭据。

> [!NOTE]
> 必须提供符合 [SQL Server 密码复杂性要求](../relational-databases/security/password-policy.md)的密码。

## <a name="next-steps"></a>后续步骤

若要了解有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下资源：

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)
- [Workshop:Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 体系结构](https://github.com/microsoft/sqlworkshops-bdc)