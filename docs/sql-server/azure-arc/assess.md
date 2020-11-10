---
title: 在启用了 Azure Arc 的 SQL Server 实例上配置按需 SQL 评估
description: 在启用了 Azure Arc 的 SQL Server 实例上配置按需 SQL 评估
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: c6f2a0989cb13253ef4a6a26e013a6b8c7a84ded
ms.sourcegitcommit: f888ac94c7b5f6b6f138ab75719dadca04e8284a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93294378"
---
# <a name="configure-sql-assessment-on-an-azure-arc-enabled-sql-server-instance"></a>在启用了 Azure Arc 的 SQL Server 实例上配置 SQL 评估

SQL 评估提供了一种机制来评估 SQL Server 的配置。 本文介绍了如何在启用了 Azure Arc 的 SQL Server 实例上使用 SQL 评估。

## <a name="prerequisites"></a>先决条件

* SQL Server 实例必须连接到 Azure Arc。有关说明，请参阅[将 SQL Server 连接到 Azure Arc](connect.md) 一文。

* 必须在计算机上安装并配置 Microsoft Monitoring Agent (MMA) 扩展。 有关说明，请查看[安装 MMA](configure-advanced-data-security.md#install-microsoft-monitoring-agent-mma) 一文。 你还可以在 [Log Analytics 代理](/azure/azure-monitor/platform/log-analytics-agent)一文中了解详细信息。

* SQL Server 实例必须[启用 TCP/IP 协议](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)。

* 若要操作 SQL Server 的命名实例，[SQL Server 浏览器服务](../../tools/configuration-manager/sql-server-browser-service.md)必须处于正在运行状态。

* 请确保你已查阅 SQL Server 文档[服务中心按需评估先决条件](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents)。

## <a name="run-on-demand-sql-assessment"></a>运行按需 SQL 评估

1. 打开“SQL Server - Azure Arc”资源，然后选择左侧窗格中的“环境运行状况”。

   > [!div class="mx-imgBorder"]
   > [ ![显示“SQL Server - Azure Arc”资源的“环境运行状况”屏幕的屏幕截图。](media/assess/sql-assessment-heading-sql-server-arc.png) ](media/assess/sql-assessment-heading-sql-server-arc.png#lightbox)

> [!IMPORTANT]
> 如果未安装 MMA 扩展，则无法启动按需 SQL 评估。

2. 选择帐户类型。 如果有托管服务帐户，则可以使用该帐户直接从门户启动 SQL 评估。 指定帐户名称。

> [!NOTE]
> 指定托管服务帐户将激活“配置 SQL 评估”按钮，从而能在门户中通过部署 CustomScriptExtension 启动评估。 因为一次只能部署一个 CustomScriptExtension，所以在执行后将自动删除 SQL 评估的脚本扩展。 如果已将另一个 CustomScriptExtension 部署到托管计算机，则不会激活“配置 SQL 评估”按钮。

3. 如果要更改默认设置，请指定数据收集计算机上的工作目录。 默认使用 `C:\sql_assessment\work_dir`。 在收集和分析期间，数据临时存储在此文件夹中。 如果此文件夹不存在，则会自动创建它。

4. 如果在门户中通过单击“配置 SQL 评估”来启动 SQL 评估，将显示标准部署气泡。

> [!div class="mx-imgBorder"]
   > [ ![屏幕截图：部署 CustomScriptExtension。](media/assess/sql-assessment-custom-script-deployment.png)](media/assess/sql-assessment-custom-script-deployment.png#lightbox)

5. 若想从目标计算机启动 SQL 评估，请单击“下载配置脚本”，将下载的脚本复制到目标计算机，并在 powershell.exe 的管理员实例中执行以下代码块之一 ：

   * 域帐户：系统将提示你输入用户帐户和密码。

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1'
      ```

   * 托管服务帐户 (MSA)

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1' -ManagedServiceAccountName <MSA account name>
      ```

> [!NOTE]
> 此脚本计划一项名为 SQLAssessment 的任务，这项任务会触发数据收集。 此任务在运行脚本后的一小时内执行。 然后，每七天重复一次。

> [!TIP]
> 你可以将此任务修改为在其他日期和时间运行，甚至可以强制它立即运行。 在任务计划程序库中，依次找到“Microsoft” > “Operations Management Suite” > “AOI” **\*\*\***  > “评估” > “SQLAssessment”。

## <a name="view-sql-assessment-results"></a>查看 SQL 评估结果

* 在“环境运行状况”窗格中，选择“查看 SQL 评估结果”按钮。

   > [!NOTE]
   > 除非 Log Analytics 中的结果已就绪，否则“查看 SQL 评估结果”按钮就会一直处于禁用状态。 在目标计算机上处理数据文件后，此进程最长可能需要两个小时才能完成。

   > [!div class="mx-imgBorder"]
   > [ ![显示 SQL 评估结果的屏幕截图。](media/assess/sql-assessment-results.png) ](media/assess/sql-assessment-results.png#lightbox)

* 通过检查工作文件夹中的文件，可以在收集计算机上查看数据处理状态。 计划的任务完成后，应会看在工作目录中看到几个具有 new. 前缀的文件。

   > [!div class="mx-imgBorder"]
   > [ ![显示“文件管理器”窗口的屏幕截图，其中显示工作文件夹中的新数据文件。](media/assess/sql-assessment-data-files-ready.png) ](media/assess/sql-assessment-data-files-ready.png#lightbox)

* Microsoft Monitoring Agent 每 15 分钟扫描一次工作文件夹。 它查找 new.* 文件，并将数据发送到 Log Analytics 工作区。 在 MMA 上传文件后，它会将前缀从 new. 更改 为 processed.

   > [!div class="mx-imgBorder"]
   > ![显示“文件管理器”窗口的屏幕截图，其中显示已处理的数据文件。](media/assess/sql-assessment-data-files-processed.png)

## <a name="next-steps"></a>后续步骤

* 通过查看先决条件文档[服务中心按需评估](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents)来了解详细信息。

* 若要获得对按需 SQL 评估功能的全面支持，需要订阅顶级支持或统一支持。 有关详细信息，请参阅 [Azure 顶级支持](https://azure.microsoft.com/support/plans/premier)。
