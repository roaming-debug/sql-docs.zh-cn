---
title: Azure Data Studio 中使用 Kqlmagic（Kusto 查询语言）的笔记本
description: 本教程演示如何在 Azure Data Studio 中创建和运行 Kqlmagic。
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 10/29/2020
ms.openlocfilehash: e78a8b6bd70724b8dda3d542e9c088e706cbe5ee
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343526"
---
# <a name="kqlmagic-in-azure-data-studio"></a>Azure Data Studio 中的 Kqlmagic

Kqlmagic 是一种命令，可在 [Azure Data Studio 笔记本](./notebooks-guidance.md)中扩展 Python 内核的功能。 可以组合使用 Python 和 [Kusto 查询语言 (KQL)](/azure/data-explorer/kusto/query)，以通过与 `render` 命令集成的丰富 Plot.ly 库查询和可视化数据。 Kqlmagic 使你可以在同一个位置获得笔记本、数据分析和丰富 Python 功能的优势。 具有 Kqlmagic 的受支持数据源包括 [Azure 数据资源管理器](/azure/data-explorer/data-explorer-overview)、[Application Insights](/azure/azure-monitor/app/app-insights-overview)、[Azure Monitor 日志](/azure/azure-monitor/platform/data-platform-logs)。

本文演示对于 Azure 数据资源管理器群集、Application Insights 日志和 Azure Monitor 日志，如何使用 Kqlmagic 扩展在 Azure Data Studio 中创建和运行笔记本。

## <a name="prerequisites"></a>先决条件

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-kqlmagic-in-a-notebook"></a>在笔记本中安装并设置 Kqlmagic

此部分中的步骤全都在 Azure Data Studio 笔记本中运行。

1. 创建新笔记本并将“内核”更改为“Python 3”。

   ![新建笔记本](media/notebooks-kqlmagic/install-new-notebook.png)

2. 当包需要更新时，系统可能会提示你升级 Python 包。

   ![是](media/notebooks-kqlmagic/install-python-yes.png)

3. 安装 Kqlmagic：

   ```python
   import sys
   !{sys.executable} -m pip install Kqlmagic --no-cache-dir --upgrade
   ```

   验证它是否已安装：

   ```python
   import sys
   !{sys.executable} -m pip list
   ```

   ![列出](media/notebooks-kqlmagic/install-list.png)

4. 加载 Kqlmagic：

   ```python
   %reload_ext Kqlmagic
   ```

   > [!Note]
   > 如果此步骤失败，则关闭文件并重新打开。

   ![加载 Kqlmagic 扩展](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

5. 可以通过浏览帮助文档或检查版本来测试是否已正确加载 Kqlmagic。

   ```python
   %kql --help "help"
   ```

   > [!Note]
   > 如果 `Samples@help` 要求提供密码，则可以将它留空，然后按 Enter。

   ![帮助](media/notebooks-kqlmagic/install-help.png)

   若要查看安装的 Kqlmagic 版本，请运行以下命令。

   ```python
   %kql --version
   ```

## <a name="kqlmagic-with-an-azure-data-explorer-cluster"></a>Kqlmagic 与 Azure 数据资源管理器群集

此部分介绍如何通过将 Kqlmagic 与 Azure 数据资源管理器群集结合使用来运行数据分析。

### <a name="load-and-authenticate-kqlmagic-for-azure-data-explorer"></a><a name="ade-load-auth"></a> 会针对 Azure 数据资源管理器加载 Kqlmagic 并进行身份验证

   > [!Note]
   > 每次在 Azure Data Studio 中创建新笔记本时，都必须加载 Kqlmagic 扩展。

1. 验证“内核”设置为“Python3”。

   ![内核更改](media/notebooks-kqlmagic/change-kernel.png)

2. 加载 Kqlmagic：

   ```python
   %reload_ext Kqlmagic
   ```

   ![加载 Kqlmagic 扩展](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

3. 连接到群集并进行身份验证：

   ```python
   %kql azureDataExplorer://code;cluster='help';database='Samples'
   ```

    > [!Note]
    > 如果使用自己的 ADX 群集，则必须在连接字符串中包含区域，如下所示：   
    ```%kql azuredataexplorer://code;cluster='mycluster.westus';database='mykustodb'```

   使用设备登录进行身份验证。 从输出中复制代码，然后选择“身份验证”，这会打开需要在其中粘贴代码的浏览器。 成功进行身份验证后，可以返回到 Azure Data Studio 以继续执行脚本的其余部分。

   ![Azure 数据资源管理器身份验证](media/notebooks-kqlmagic/ade-auth.png)

### <a name="query-and-visualize-for-azure-data-explorer"></a>对 Azure 数据资源管理器进行查询和可视化

查询数据使用 [render 运算符](/azure/data-explorer/kusto/query/renderoperator)，而可视化数据使用 ploy.ly 库。 此查询和可视化操作提供了使用本机 KQL 的集成体验。

1. 按状态和频率分析前 10 个暴风雨事件：

   ```python
   %kql StormEvents | summarize count() by State | sort by count_ | limit 10
   ```

   如果熟悉 Kusto 查询语言 (KQL)，则可以在 `%kql` 后键入查询。

   ![分析暴风雨事件](media/notebooks-kqlmagic/ade-analyze-storm-events.png)

2. 可视化时间线图表：

   ```python
   %kql StormEvents \
   | summarize event_count=count() by bin(StartTime, 1d) \
   | render timechart title= 'Daily Storm Events'
   ```

   ![可视化时间表](media/notebooks-kqlmagic/ade-visualize-timechart.png)

3. 使用 `%%kql` 的多行查询示例。

   ```python
   %%kql
   StormEvents
   | summarize count() by State
   | sort by count_
   | limit 10
   | render columnchart title='Top 10 States by Storm Event count'
   ```

   ![多行查询示例](media/notebooks-kqlmagic/ade-multiline-query-sample.png)

## <a name="kqlmagic-with-application-insights"></a>Kqlmagic 与 Application Insights

### <a name="load-and-authenticate-kqlmagic-for-application-insights"></a><a name="appin-load-auth"></a> 会针对 Application Insights 加载 Kqlmagic 并进行身份验证

1. 验证“内核”设置为“Python3”。

   ![内核](media/notebooks-kqlmagic/change-kernel.png)

2. 加载 Kqlmagic：

   ```python
   %reload_ext Kqlmagic
   ```

   ![加载 Kqlmagic 扩展](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

   > [!Note]
   > 每次在 Azure Data Studio 中创建新笔记本时，都必须加载 Kqlmagic 扩展。

3. 连接并进行身份验证。

   首先，必须为 Application Insights 资源生成 API 密钥。 然后，使用应用程序 ID 和 API 密钥从笔记本连接到 Application Insights：

   ```python
   %kql appinsights://appid='DEMO_APP';appkey='DEMO_KEY'
   ```

### <a name="query-and-visualize-for-application-insights"></a>对 Application Insights 进行查询和可视化

查询数据使用 [render 运算符](/azure/data-explorer/kusto/query/renderoperator)，而可视化数据使用 ploy.ly 库。 此查询和可视化操作提供了使用本机 KQL 的集成体验。

1. 显示页面视图：

   ```python
   %%kql
   pageViews
   | limit 10
   ```

   ![页面视图](media/notebooks-kqlmagic/appin-page-views.png)

   > [!Note]
   > 使用鼠标在图表区域上拖动以放大到特定日期。

2. 在时间线图表中显示页面视图：

   ```python
   %%kql
   pageViews
   | summarize event_count=count() by name, bin(timestamp, 1d)
   | render timechart title= 'Daily Page Views'
   ```

   ![时间线图表](media/notebooks-kqlmagic/appin-timechart.png)

## <a name="kqlmagic-with-azure-monitor-logs"></a>Kqlmagic 与 Azure Monitor logs

### <a name="load-and-authenticate-kqlmagic-for-azure-monitor-logs"></a><a name="aml-load-auth"></a> 会针对 Azure Monitor 日志加载 Kqlmagic 并进行身份验证

1. 验证“内核”设置为“Python3”。

   ![更改](media/notebooks-kqlmagic/change-kernel.png)

2. 加载 Kqlmagic：

   ```python
   %reload_ext Kqlmagic
   ```

   ![加载 Kqlmagic 扩展](media/notebooks-kqlmagic/install-load-kql-magic-ext.png)

   > [!Note]
   > 每次在 Azure Data Studio 中创建新笔记本时，都必须加载 Kqlmagic 扩展。

3. 连接并进行身份验证：

   ```python
   %kql loganalytics://workspace='DEMO_WORKSPACE';appkey='DEMO_KEY';alias='myworkspace'
   ```

   ![Log Analytics 身份验证](media/notebooks-kqlmagic/aml-auth.png)

### <a name="query-and-visualize-for-azure-monitor-logs"></a>对 Azure Monitor 日志进行查询和可视化

查询数据使用 [render 运算符](/azure/data-explorer/kusto/query/renderoperator)，而可视化数据使用 ploy.ly 库。 此查询和可视化操作提供了使用本机 KQL 的集成体验。

1. 查看时间线图表：

   ```python
   %%kql
   KubeNodeInventory
   | summarize event_count=count() by Status, bin(TimeGenerated, 1d)
   | render timechart title= 'Daily Kubernetes Nodes'
   ```

   ![Log Analytics 每日 Kubernetes 节点时间表](media/notebooks-kqlmagic/aml-timechart-daily-kubernetes-nodes.png)

## <a name="next-steps"></a>后续步骤

了解有关笔记本和 Kqlmagic 的详细信息：

- [Azure Data Studio 的 Kusto (KQL) 扩展（预览版）](../extensions/kusto-extension.md)
- [创建并运行 Kusto (KQL) 笔记本（预览）](./notebooks-kusto-kernel.md)
- [使用 Jupyter Notebook 和 Kqlmagic 扩展分析 Azure 数据资源管理器中的数据](/azure/data-explorer/Kqlmagic)
- [Jupyter Notebook 和 Jupyter 实验室的扩展 (Magic)](https://github.com/Microsoft/jupyter-Kqlmagic)，让笔记本体验适用于 Kusto、Application Insights 和 LogAnalytics 数据。
- [Kqlmagic](https://pypi.org/project/Kqlmagic/)
- [如何使用 Azure Data Studio 中的笔记本](./notebooks-guidance.md)