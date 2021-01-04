---
title: 配置高级数据安全
titleSuffix: Azure Arc
description: 为已启用 Azure Arc 的 SQL Server 实例配置高级数据安全
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: e0c68752d21de5945c06433af8c08f6208322f7c
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103163"
---
# <a name="configure-advanced-data-security-for-azure-arc-enabled-sql-server-instance"></a>为已启用 Azure Arc 的 SQL Server 实例配置高级数据安全

可通过以下步骤为本地 SQL Server 实例启用高级数据安全。

## <a name="prerequisites"></a>必备知识

* 将 SQL Server 实例载入已启用 Arc 的 SQL Server。 按照以下说明[将 SQL Server 实例载入已启用 Arc 的 SQL Server](connect.md)。

* 为用户帐户分配一个[安全中心角色 (RBAC)](/azure/security-center/security-center-permissions)

## <a name="create-a-log-analytics-workspace"></a>创建 Log Analytics 工作区

1. 搜索“Log Analytics 工作区”资源类型，并通过“创建”边栏选项卡添加新资源。

   ![创建新工作区](media/configure-advanced-data-security/create-new-log-analytics-workspace.png)

   > [!NOTE]
   > 你可以使用任何区域中的 Log Analytics 工作区，因此，只要你有一个工作区，就可以使用它。 但建议在创建“计算机 - Azure Arc”资源的相同区域中创建一个工作区。

1. 转到 Log Analytics 工作区资源的概述页面，选择“Windows、Linux 和其他源”。 复制工作区 ID 和主密钥以供稍后使用。

   ![“Log analytics 工作区”边栏选项卡](media/configure-advanced-data-security/log-analytics-workspace-blade.png)

## <a name="install-microsoft-monitoring-agent-mma"></a>安装 Microsoft Monitoring Agent (MMA)

仅当尚未在远程计算机上配置 MMA 代理时，才需要执行下一步。

1. 为安装了 SQL Server 实例的虚拟或物理服务器选择“计算机 - Azure Arc”资源，并使用 Microsoft Monitoring Agent 扩展功能添加“Microsoft Monitoring Agent - Azure Arc”扩展 。 当系统要求配置 Log Analytics 工作区时，请使用上一步中保存的工作区 ID 和主密钥。

   ![安装 MMA](media/configure-advanced-data-security/install-mma-extension.png)

1. 验证成功后，单击“创建”以启动 MMA Arc 扩展部署工作流。 部署完成后，状态将更新为“成功”。

1. 有关详细信息，请参阅 [Azure Arc 的扩展管理](/azure/azure-arc/servers/manage-vm-extensions)

## <a name="enable-azure-defender"></a>启用 Azure Defender

接下来，你需要为 SQL Server 实例启用 Azure Defender。

1. 从侧边栏转到“安全中心”，并打开“定价和设置”页面。

1. 选择为上一步中的 MMA 扩展配置的工作区

1. 选择“启用 Azure Defender”。 请确保已启用“计算机上的 SQL Server”选项。

   ![升级工作区](media/configure-advanced-data-security/enable-azure-defender.png)

 > [!NOTE]
   > 首次扫描将在启用高级数据安全后的 24 小时内进行，以生成漏洞评估。 之后，将在每周的星期日执行自动扫描。

## <a name="explore"></a>浏览

了解 Azure 安全中心的安全异常和威胁。

1. 打开你的 SQL Server - Azure Arc 资源并在左侧菜单中选择“安全”。 查看该实例的建议和警报。

   ![选择安全标题](media/configure-advanced-data-security/security-heading-sql-server-arc.png)

1. 单击任意建议，查看“安全中心”中的漏洞详细信息。

   ![漏洞报告](media/configure-advanced-data-security/vulnerabilities-report.png)

1. 单击任意安全警报，了解完整的详细信息并进一步了解 [Azure Sentinel](/azure/sentinel/overview) 中的攻击。 下图是暴力攻击警报的示例。

   ![暴力攻击警报](media/configure-advanced-data-security/brute-force-alert.png)

1. 单击“采取行动”以缓解警报。

   ![警报缓解措施](media/configure-advanced-data-security/brute-force-alert-mitigation.png)

> [!NOTE]
> 页面顶部的常规“安全中心”链接不会使用预览门户 URL，因此 SQL Server - Azure Arc 资源在这里将不可见 。 建议访问这些链接，了解单独的建议或警报。

## <a name="next-steps"></a>后续步骤

可以使用 [Azure Sentinel](/azure/sentinel/overview) 进一步调查安全警报和攻击。 按照以下说明[载入 Azure Sentinel](/azure/sentinel/connect-data-sources)。