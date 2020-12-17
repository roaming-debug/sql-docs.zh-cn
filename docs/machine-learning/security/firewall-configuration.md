---
title: 防火墙配置
description: 如何为 SQL Server 机器学习服务中的出站连接配置防火墙。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/17/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 44fa8b361f74fb670dd5671bb9fbeaa9e635faad
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470698"
---
# <a name="firewall-configuration-for-sql-server-machine-learning-services"></a>SQL Server 机器学习服务的防火墙配置
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文列出了管理员或架构师在使用机器学习服务时应牢记的防火墙配置注意事项。

## <a name="default-firewall-rules"></a>默认防火墙规则

默认情况下，SQL Server 安装程序通过创建防火墙规则来禁用出站连接。

在 SQL Server 2016 和 2017 中，这些规则基于本地用户帐户，其中安装程序为 SQLRUserGroup 创建了一个出站规则，该规则拒绝对其成员进行网络访问（每个工作线程帐户均作为受该规则约束的本地原则列出）  。 有关 SQLRUserGroup 的详细信息，请参阅 [SQL Server 机器学习服务中扩展性框架的安全性概述](../../machine-learning/concepts/security.md#sqlrusergroup)。

在 SQL Server 2019 中，作为迁移到 AppContainer 的一部分，有基于 AppContainer SID 的新防火墙规则：每个规则分别针对由 SQL Server 安装程序创建的 20 个 AppContainer。 防火墙规则名称的命名约定是阻止 SQL Server 实例 MSSQLSERVER 中的 AppContainer-00 的网络访问，其中 00 是 AppContainer 的编号（默认情况下为 00-20），MSSQLSERVER 是 SQL Server 实例的名称  。

> [!Note]
> 如果需要网络调用，可以禁用 Windows 防火墙中的出站规则。

## <a name="restrict-network-access"></a>限制网络访问

在默认安装中，将使用 Windows 防火墙规则来阻止外部运行时进程的所有出站网络访问。 应创建防火墙规则来防止外部运行时进程下载程序包或进行其他可能有恶意目的的网络调用。

如果使用的是其他防火墙程序，还可以通过为本地用户帐户或用户帐户池所代表的组设置规则来创建规则，以阻止外部运行时的出站网络连接。

强烈建议开启 Windows 防火墙（或你选择的另一种防火墙），防止 R 或 Python 运行时对网络的无限制访问。

## <a name="next-steps"></a>后续步骤

[为入站连接配置 Windows 防火墙](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)