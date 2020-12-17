---
title: 配置并行或并发 R 和 Python 脚本执行
description: 在用户帐户池中配置并行或并发 R 和 Python 脚本执行以扩展 SQL Server 机器学习服务。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/25/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 833f89a2a39fa95999ac98f718769eb425d9d21f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471338"
---
# <a name="scale-concurrent-execution-of-external-scripts-in-sql-server-machine-learning-services"></a>在 SQL Server 机器学习服务中缩放外部脚本的并发执行
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

了解 SQL Server 机器学习服务的辅助角色帐户，以及如何更改默认配置以扩展外部脚本的并发执行数。

在安装机器学习服务的过程中，会创建一个新的 Windows 用户帐户池来支持  *服务的任务执行*[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]。 这些辅助角色帐户的用途是将不同 SQL Server 用户的外部脚本并发执行隔离开来。

> [!Note]
> 在 SQL Server 2019 中，SQLRUserGroup 现在只有一个成员，即一个 SQL Server Launchpad 服务帐户，而不是多个工作人员帐户  。 本文介绍 SQL Server 2016 和 2017 的辅助角色帐户。

## <a name="worker-account-group"></a>辅助角色帐户组

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将为安装和启用了机器学习的每个实例创建一个 Windows 帐户组。

- 在默认实例中，该组的名称为 **SQLRUserGroup**。 无论你使用的是 Python 或 R，还是两者同时使用，此名称都是相同的。
- 在命名的实例中，默认的组名称以实例名称作为后缀：例如 **SQLRUserGroupMyInstanceName**。

用户帐户池默认包含 20 个用户帐户。 在大多数情况下，20 个帐户足以支持机器学习任务，但你也可以更改帐户数量。 最多允许有 100 个帐户。

- 在默认实例中，各个帐户命名为 **MSSQLSERVER01** 到 **MSSQLSERVER20**。
- 对于命名的实例，各个帐户根据实例名称命名：例如，**MyInstanceName01** 到 **MyInstanceName20**。

如果有多个实例使用机器学习，计算机则会有多个用户组。 无法跨实例共享组。

<a name = "HowToChangeGroup"> </a>

## <a name="number-of-worker-accounts"></a>辅助角色帐户的数量

若要修改帐户池中的用户数目，必须按如下所述编辑 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 服务的属性。

与每个用户帐户关联的密码是随机生成的，但在创建帐户后，你可以更改这些密码。

1. 打开 SQL Server 配置管理器并选择“SQL Server 服务”。 
2. 双击 SQL Server Launchpad 服务并停止该服务（如果正在运行）。
3.  在“服务”选项卡上，确保“启动模式”设置为“自动”。  Launchpad 未运行时无法启动外部脚本。
4.  单击“高级”选项卡，然后根据需要编辑“外部用户计数”的值。   此设置控制有多少个不同的 SQL 用户可同时运行外部脚本会话。 默认为 20 个帐户。 用户的最大数量为 100。
5. （可选）如果组织中的策略要求定期更改密码，可将选项“重置外部用户密码”设置为“是”。   这样，便会重新生成 Launchpad 为用户帐户维护的已加密密码。 有关详细信息，请参阅[实施密码策略](../security/sql-server-launchpad-service-account.md#bkmk_EnforcePolicy)。
6.  重启 Launchpad 服务。

## <a name="managing-workloads"></a>管理工作负荷

此池中的帐户数量决定了有多少外部脚本会话可同时处于活动状态。  默认情况下，会创建 20 个帐户，这意味着允许 20 个不同的用户同时使用活动的 Python 或 R 会话。 如果你希望运行超过 20 个并发脚本，则可以增加辅助角色帐户的数量。

当同一用户同时执行多个外部脚本时，此用户运行的所有会话将使用同一个辅助角色帐户。 例如，只要资源允许，单个用户可以同时运行 100 个不同的 Python 或 R 脚本，但所有脚本都是使用单个辅助角色帐户运行的。

可支持的辅助角色帐户数以及任一用户可运行的并行会话数仅受服务器资源的限制。 通常，在使用 Python 或 R 运行时时，内存是你将遭遇的第一个瓶颈。

Python 或 R 脚本可以使用的资源受 SQL Server 的控制。 我们建议使用 SQL Server DMV 监视资源用量，或者查看关联 Windows 作业对象中的性能计数器，并相应地调整服务器内存用量。 如果你在使用 SQL Server Enterprise Edition，可以通过配置一个[外部资源池](create-external-resource-pool.md)来分配用于运行外部脚本的资源。

## <a name="next-steps"></a>后续步骤

- [使用 SQL Server Management Studio 中的自定义报表监视 Python 和 R 脚本执行](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)
- [使用动态管理视图 (DMV) 监视 SQL Server 机器学习服务](../../machine-learning/administration/monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)