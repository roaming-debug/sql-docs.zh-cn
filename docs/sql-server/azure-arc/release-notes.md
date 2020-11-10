---
title: 已启用 Azure Arc 的 SQL Server - 发行说明
description: 最新发行说明
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 10/29/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 3dcbc6b17d5abe87aabe923d70746d65595b9de6
ms.sourcegitcommit: dc3ea1696b8a4332934568439aed6cce4e9737eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93244649"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>发行说明 - 已启用 Azure Arc 的 SQL Server（预览）

> [!NOTE]
> 作为预览版功能，本文中介绍的技术受制于 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="september-2020"></a>2020 年 9 月

已启用 Azure Arc 的 SQL Server 已发布公共预览版。 已启用 Azure Arc 的 SQL Server 将 Azure 服务扩展到 SQL Server 实例，这些实例托管在客户的数据中心、边缘或多云环境中的 Azure 外部。

有关详细信息，请参阅[已启用 Azure Arc 的 SQL Server 概述](overview.md)

### <a name="known-issues"></a>已知问题

9 月版本会出现以下问题：

* “注册已启用 Azure Arc 的 SQL Server”边栏选项卡不支持配置自定义标记。 若要添加自定义标记，请在注册后打开“SQL Server - Azure Arc”资源，并在“概述”页中更改标记 。

* 若要将 SQL Server 实例连接到 Azure Arc，需要使用权限广泛的帐户。 有关详细信息，请参阅[所需的权限](overview.md#required-permissions)。

## <a name="october-2020"></a>2020 年 10 月

10 月版本包含以下改进：

* “注册已启用 Azure Arc 的 SQL Server”边栏选项卡现在包括“标记”选项卡。标记包含在注册脚本中，并反映在“SQL Server - Azure Arc”资源中。 有关详细信息，请参阅[将 SQL Server 连接到 Azure Arc](connect.md)。

* “环境运行状况”条目现在支持在门户中通过部署 CustomScriptExtension 来激活 SQL 评估 。 有关详细信息，请参阅[配置 SQL 评估](assess.md#run-on-demand-sql-assessment)。

### <a name="known-issues"></a>已知问题

10 月版本会出现以下问题：

* 若要将 SQL Server 实例连接到 Azure Arc，需要使用权限广泛的帐户。 有关详细信息，请参阅[所需的权限](overview.md#required-permissions)。

## <a name="next-steps"></a>后续步骤

想尝试一下吗？  可通过[已启用 Azure Arc 的 SQL Server 快速入门](https://aka.ms/AzureArcSqlServerJumpstart)快速上手。
