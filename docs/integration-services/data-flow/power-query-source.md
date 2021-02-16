---
title: Power Query 源 | Microsoft Docs
description: 了解如何在 SQL Server Integration Services (SSIS) 数据流中配置 Power Query 源。
ms.date: 12/27/2019
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.powerqueryconnmgr.f1
- sql13.ssis.designer.powerquerysource.queries.f1
- sql13.ssis.designer.powerquerysource.connmgrs.f1
- sql14.ssis.designer.powerqueryconnmgr.f1
- sql14.ssis.designer.powerquerysource.queries.f1
- sql14.ssis.designer.powerquerysource.connmgrs.f1
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 43e02595cc57413eb18ff3e390606ea94493912a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336350"
---
# <a name="power-query-source-preview"></a>Power Query 源（预览版）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

本文介绍如何配置 SQL Server Integration Services (SSIS) 数据流中的 Power Query 源属性。 借助 Power Query 技术，可以使用 Excel / Power BI Desktop 连接到各种数据源并转换数据。 有关详细信息，请参阅 [Power Query - 概述和学习](https://support.office.com/article/power-query-overview-and-learning-ed614c81-4b00-4291-bd3a-55d80767f81d)一文。 可以将 Power Query 生成的脚本复制并粘贴到 SSIS 数据流的 Power Query 源中来进行配置。
  
> [!NOTE]
> 对于当前预览版本，Power Query 源只能用于 Azure 数据工厂 (ADF) 中的 SQL Server 2017/2019 和 Azure-SSIS Integration Runtime (IR)。 可从[此处](https://www.microsoft.com/download/details.aspx?id=100619)下载并安装 SQL Server 2017/2019 的最新 Power Query 源。 已预安装 Azure-SSIS IR Power Query 源。 若要预配 Azure SSIS IR，请参阅[在 ADF 中预配 SSIS](/azure/data-factory/tutorial-deploy-ssis-packages-azure) 一文。

## <a name="configure-the-power-query-source"></a>配置 Power Query 源

要在 SSDT 中打开 Power Query 源编辑器  ，请将 Power Query 源  从 SSIS 工具箱拖放到数据流设计器上，然后双击它。  

![PQ 源](media/power-query-source/pq-source.png)

左侧显示三个选项卡。 在“查询”  选项卡上，可以从下拉菜单中选择查询模式。
-   使用“单个查询”  模式，可以从 Excel/Power BI Desktop 复制和粘贴单个 Power Query 脚本。
-   使用“变量中的单个查询”  模式，可以指定包含要执行查询的字符串变量。

![PQ 源“查询”选项卡“单个”](media/power-query-source/pq-source-queries-tab-single.png)

在“连接管理器”  选项卡上，可以添加或删除包含数据源访问凭据的 Power Query 连接管理器。 选择“检测数据源”  按钮可标识查询中引用的数据源，并列出它们，以供你分配相应的现有 Power Query 连接管理器或创建新的连接管理器。

![PQ 源“连接管理器”选项卡“检测”](media/power-query-source/pq-source-connection-managers-tab-detect.png)

![PQ 源“连接管理器”选项卡“添加”](media/power-query-source/pq-source-connection-managers-tab-add.png)

最后，在“列”  选项卡上，可以编辑输出列信息。

![PQ 源“列”选项卡](media/power-query-source/pq-source-columns-tab.png)

## <a name="configure-the-power-query-connection-manager"></a>配置 Power Query 连接管理器

在 SSDT 上使用 Power Query 源设计数据流时，可以通过以下方式创建新的 Power Query 连接管理器：
- 在选择“添加  /检测数据源”  按钮并从下拉菜单选择“<New connection...>”  后（如上所述），在 Power Query Source 的“连接管理器”  选项卡上间接创建它。
- 通过右键单击程序包的“连接管理器”  面板，然后从下拉菜单选择“新建连接...”  ，直接创建它。

![PQ 源连接管理器面板“添加”](media/power-query-source/pq-source-connection-managers-panel-add.png)

在“添加 SSIS 连接管理器”  对话框中，从连接管理器类型列表中双击“PowerQuery”  。

![PQ 源连接管理器面板“添加”对话框](media/power-query-source/pq-source-connection-managers-panel-add-dialog.png)

在“Power Query 连接管理器编辑器”  中，需要指定“数据源类型”  、“数据源路径”  和“身份验证类型”  ，以及分配相应的访问凭据。 对于“数据源类型”  ，目前可从下拉菜单的 22 种类型中任选一个。

![PQ 源连接管理器编辑器类型](media/power-query-source/pq-source-connection-manager-editor-kind.png)

部分源（Oracle  、DB2  、MySQL  、PostgreSQL  、Teradata  、Sybase  ）需要额外安装 ADO.NET 驱动程序，此驱动程序可以从 [Power Query 必备组件](/power-bi/desktop-data-source-prerequisites)一文中获取。 可以使用自定义安装界面将其安装在 Azure-SSIS IR 上，具体请参阅[自定义 Azure-SSIS IR](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) 一文。

对于“数据源路径”  ，可以输入数据源特定的属性，从而构成没有身份验证信息的连接字符串。 例如，SQL  数据源路径的格式为 `<Server>;<Database>`。 可以选择“编辑”  按钮，将值分配给构成路径的数据源特定属性。

![PQ 源连接管理器编辑器路径](media/power-query-source/pq-source-connection-manager-editor-path.png)

最后，对于“身份验证类型”  ，可以从下拉菜单中选择“匿名  /Windows 身份验证  /用户名密码  /密钥”  ，输入相应的访问凭据，然后选择“测试连接”  按钮，确保 Power Query 源已正确配置。

![PQ 源连接管理器编辑器身份验证](media/power-query-source/pq-source-connection-manager-editor-authentication.png)

### <a name="current-limitations"></a>当前限制

-   暂无法在 Azure-SSIS IR 上使用“Oracle”  数据源，因为无法在其上安装 Oracle ADO.NET 驱动程序，所以请暂时改为安装 Oracle ODBC 驱动程序，并使用“ODBC”  数据源连接到 Oracle，具体请参阅[自定义 Azure-SSIS IR](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) 一文中的“ORACLE STANDARD ODBC”  示例。

-   暂无法在经过自定义设置的 Azure-SSIS IR 上使用“Web”  数据源，所以请暂时在未经过自定义设置的 Azure-SSIS IR 上使用它。

## <a name="next-steps"></a>后续步骤
了解如何在 Azure-SSIS IR 中作为 ADF 管道中的最优活动运行 SSIS 包。 请参阅[执行 SSIS 包活动运行时](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)一文。