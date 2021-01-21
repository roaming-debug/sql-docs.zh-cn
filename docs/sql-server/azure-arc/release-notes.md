---
title: 已启用 Azure Arc 的 SQL Server - 发行说明
description: 最新发行说明
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 372f4bec9acc4d4e170ddbc1a1fa6d66be1541e7
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596552"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>发行说明 - 已启用 Azure Arc 的 SQL Server（预览）

> [!NOTE]
> 作为预览版功能，本文中介绍的技术受制于 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="december-2020"></a>2020 年 12 月

### <a name="breaking-change"></a>重大更改

此版本引入了一个称为 `Microsoft.AzureArcData` 的更新的[资源提供程序](/azure/azure-resource-manager/management/azure-services-resource-providers)。 在继续使用已启用 Azure Arc 的 SQL Server 之前，需要注册此资源提供程序。 请参阅[先决条件](connect.md#prerequisites)部分中的资源提供程序注册说明。

如果有现有的 SQL Server - Azure Arc 资源，请使用以下步骤将它们迁移到 Microsoft.AzureArcData 命名空间。

1. 启动 [Cloud Shell](https://shell.azure.com/)。 有关详细信息，请阅读[有关 Cloud Shell 中的 PowerShell 的详细信息](/azure/cloud-shell/quickstart-powershell)。

2. 使用以下命令将脚本上传到 shell：

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/manage/azure-arc-enabled-sql-server/migrate-to-azure-arc-data.ps1 -o migrate-to-azure-arc-data.ps1
    ```
3. 运行该脚本。  

    ```console
   ./migrate-to-azure-arc-data.ps1
    ```

> [!NOTE]
> - 若要将命令粘贴到 shell，请使用 Windows 上的 `Ctrl-Shift-V` 或 MacOS 上的 `Cmd-v`。
> - 该脚本将直接上传到与 Cloud Shell 会话关联的主文件夹。
> - 该脚本将提示资源组名称，并在迁移完成后输出消息。

### <a name="other-changes"></a>其他更改

* “SQL Server - Azure Arc”资源类型中的 TCPPorts 属性已重命名为“TCPStaticPorts”
* 所需的权限不如以前那样广泛。 有关详细信息，请参阅[必需权限](overview.md#required-permissions)部分。

### <a name="known-issues"></a>已知问题

* CreateTime 属性不会添加到 AzureArcData 命名空间中的任何新创建的资源，包括“SQL Server - Azure Arc”资源。

## <a name="october-2020"></a>2020 年 10 月

10 月版本包含以下改进：

* “注册已启用 Azure Arc 的 SQL Server”边栏选项卡现在包括“标记”选项卡。标记包含在注册脚本中，并反映在“SQL Server - Azure Arc”资源中。 有关详细信息，请参阅[将 SQL Server 连接到 Azure Arc](connect.md)。

* “环境运行状况”条目现在支持在门户中通过部署 CustomScriptExtension 来激活 SQL 评估 。 有关详细信息，请参阅[配置 SQL 评估](assess.md#run-on-demand-sql-assessment)。

### <a name="known-issues"></a>已知问题

10 月版本会出现以下问题：

* 若要将 SQL Server 实例连接到 Azure Arc，需要使用权限广泛的帐户。 有关详细信息，请参阅[所需的权限](overview.md#required-permissions)。

## <a name="september-2020"></a>2020 年 9 月

已启用 Azure Arc 的 SQL Server 已发布公共预览版。 已启用 Azure Arc 的 SQL Server 将 Azure 服务扩展到 SQL Server 实例，这些实例托管在客户的数据中心、边缘或多云环境中的 Azure 外部。

有关详细信息，请参阅[已启用 Azure Arc 的 SQL Server 概述](overview.md)

### <a name="known-issues"></a>已知问题

9 月版本会出现以下问题：

* “注册已启用 Azure Arc 的 SQL Server”边栏选项卡不支持配置自定义标记。 若要添加自定义标记，请在注册后打开“SQL Server - Azure Arc”资源，并在“概述”页中更改标记 。

* 若要将 SQL Server 实例连接到 Azure Arc，需要使用权限广泛的帐户。 有关详细信息，请参阅[所需的权限](overview.md#required-permissions)。

## <a name="next-steps"></a>后续步骤

想尝试一下吗？  可通过[已启用 Azure Arc 的 SQL Server 快速入门](https://aka.ms/AzureArcSqlServerJumpstart)快速上手。