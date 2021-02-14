---
title: 下载 SQL Server Data Tools (SSDT)
description: 了解 SQL Server Data Tools (SSDT)。 了解如何随 Visual Studio 2019 和 Visual Studio 2017 安装此数据库开发工具集。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssdt
ms.topic: conceptual
keywords: 安装 ssdt, 下载 ssdt, 最新 ssdt
ms.assetid: b0fc4987-d260-4d0a-9dd1-98099835b361
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 02/20/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: cf088c407a95d32f3cfc7413b55ec41661126e78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077230"
---
# <a name="download-sql-server-data-tools-ssdt-for-visual-studio"></a>下载 SQL Server Data Tools (SSDT) for Visual Studio

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

SQL Server Data Tools (SSDT) 是一款新式开发工具，用于生成 SQL Server 关系数据库、Azure SQL 数据库、Analysis Services (AS) 数据模型、Integration Services (IS) 包和 Reporting Services (RS) 报表。 使用 SSDT，你可以设计和部署任何 SQL Server 内容类型，就像在 Visual Studio 中开发应用程序一样轻松。

## <a name="ssdt-for-visual-studio-2019"></a>SSDT for Visual Studio 2019

### <a name="changes-in-ssdt-for-visual-studio-2019"></a>SSDT for Visual Studio 2019 中的变化

用于创建数据库项目的核心 SSDT 功能一直是 Visual Studio 不可或缺的部分。

随着 Visual Studio 2019 发布，启用 Analysis Services、Integration Services 和 Reporting Services 项目所需的功能仅移至相应 Visual Studio (VSIX) 扩展中。

> [!NOTE]
> 没有适用于 Visual Studio 2019 的 SSDT 独立安装程序。

### <a name="install-ssdt-with-visual-studio-2019"></a>使用 Visual Studio 2019 安装 SSDT

如果已安装 [Visual Studio 2019](/visualstudio/install/install-visual-studio?preserve-view=true&view=vs-2019)，则可以编辑工作负载列表，使其包括 SSDT。 如果尚未安装 Visual Studio 2019，可以下载并安装 [Visual Studio 2019 Community](https://visualstudio.microsoft.com/downloads/)。

若要修改已安装的 Visual Studio 工作负载以包含 SSDT，请使用 Visual Studio 安装程序。

1. 启动 Visual Studio 安装程序。 在 Windows“开始”菜单中，可以搜索“安装程序”。

   ![Windows 2019“开始”菜单中的 Visual Studio 安装程序](../ssdt/media/visual-studio-installer.png)

2. 在安装程序中，选择要向其添加 SSDT 的 Visual Studio 版本，然后选择“修改”。

3. 在工作负载列表中的“数据存储和处理”下选择“SQL Server Data Tools”。

   ![数据存储和处理工作负载 2019](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2019.png)

对于 Analysis Services、Integration Services 或 Reporting Services 项目，可以从 Visual Studio 的“扩展” > “管理扩展”或从[市场](https://marketplace.visualstudio.com/search?term=services&target=VS&category=All%20categories&vsVersion=&sortBy=Relevance)安装相应的[扩展](/visualstudio/ide/finding-and-using-visual-studio-extensions)。

* [Analysis Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftAnalysisServicesModelingProjects)
* [Integration Services](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Reporting Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftReportProjectsforVisualStudio)

## <a name="ssdt-for-visual-studio-2017"></a>SSDT for Visual Studio 2017

### <a name="changes-in-ssdt-for-visual-studio-2017"></a>SSDT for Visual Studio 2017 中的变化

自 Visual Studio 2017 起，创建数据库项目的功能已集成到 Visual Studio 安装中。 无需安装 SSDT 独立安装程序，即可获得核心 SSDT 体验。

现在要创建 Analysis Services、Integration Services 或 Reporting Services 项目，仍需要 SSDT 独立安装程序。

### <a name="install-ssdt-with-visual-studio-2017"></a>使用 Visual Studio 2017 安装 SSDT

若要在 [Visual Studio 安装](/visualstudio/install/install-visual-studio)过程中安装 SSDT，请选择“数据存储和处理”工作负荷，然后选择“SQL Server Data Tools”   。

如果已安装 Visual Studio，请使用 Visual Studio 安装程序修改已安装的工作负载，以包含 SSDT。

1. 启动 Visual Studio 安装程序。 在 Windows“开始”菜单中，可以搜索“安装程序”。

   ![Windows 2017“开始”菜单中的 Visual Studio 安装程序](../ssdt/media/visual-studio-installer.png)

2. 在安装程序中，选择要向其添加 SSDT 的 Visual Studio 版本，然后选择“修改”。

3. 在工作负载列表中的“数据存储和处理”下选择“SQL Server Data Tools”。

   ![数据存储和处理工作负载 2017](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2017.png)

### <a name="install-analysis-services-integration-services-and-reporting-services-tools"></a>安装 Analysis Services、Integration Services 和 Reporting Services 工具

若要安装 Analysis Services、Integration Services 和 Reporting Services 项目支持，请运行[SSDT 独立安装程序](#ssdt-for-vs-2017-standalone-installer)。

此安装程序列出了可用于添加 SSDT 工具的 Visual Studio 实例。 如果尚未安装 Visual Studio，选择“安装新 SQL Server Data Tools 实例”  安装 SSDT 时使用的是最低版本 Visual Studio，但为了获得最佳体验，建议结合使用 SSDT 和[最新版 Visual Studio](https://www.visualstudio.com/downloads)。

![选择 AS、IS、RS](../ssdt/media/download-sql-server-data-tools-ssdt/select-services.png)

## <a name="ssdt-for-vs-2017-standalone-installer"></a>SSDT for VS 2017（独立安装程序）

:::image type="icon" source="media/download.png" border="false"::: [下载 SSDT for Visual Studio 2017 (15.9.7)](https://go.microsoft.com/fwlink/?linkid=2151759)

> [!IMPORTANT]
> * 安装 SSDT for Visual Studio 2017 (15.9.7) 前，请卸载“Analysis Services 项目”和“Reporting Services 项目”扩展（如已安装），并关闭所有 VS 实例。 
> * 删除了收件箱组件 Power Query Source for SQL Server 2017。 目前，我们已宣布将 Power Query Source for SQL Server 2017 和 2019 作为现成组件推出（可以从[此处](https://www.microsoft.com/download/details.aspx?id=100619)下载）。
> * 若要使用 Oracle 和 Teradata 连接器设计定目标到低于 SQL 2019 的旧版 SQL Server 的包，除了 [Microsoft Oracle Connector for SQL 2019](https://www.microsoft.com/download/details.aspx?id=58228) 和 [Microsoft Teradata Connector for SQL 2019](https://www.microsoft.com/download/details.aspx?id=100599) 之外，还需要安装相应版本的 Microsoft Connector for Oracle by Attunity 和 Microsoft Connector for Teradata by Attunity。
>    * [版本 5.0 Microsoft Connector for Oracle by Attunity 和 Microsoft Connector for Teradata by Attunity（定目标到 SQL Server 2017）](https://www.microsoft.com/download/details.aspx?id=55179)
>    * [版本 4.0 Microsoft Connector for Oracle by Attunity 和 Microsoft Connector for Teradata by Attunity（定目标到 SQL Server 2016）](https://www.microsoft.com/download/details.aspx?id=52950)
>    * [版本 3.0 Microsoft Connector for Oracle by Attunity 和 Microsoft Connector for Teradata by Attunity（定目标到 SQL Server 2014）](https://www.microsoft.com/download/details.aspx?id=44582)
>    * [版本 2.0 Microsoft Connector for Oracle by Attunity 和 Microsoft Connector for Teradata by Attunity（定目标到 SQL Server 2012）](https://www.microsoft.com/download/details.aspx?id=29283)

### <a name="release-notes"></a>发行说明

有关更改的完整列表，请参阅 [SQL Server Data Tools (SSDT) 发行说明](release-notes-ssdt.md)。

### <a name="system-requirements"></a>系统要求

SSDT for Visual Studio 2017 具有与 Visual Studio 相同的[系统需求](/visualstudio/productinfo/vs2017-system-requirements-vs)。

### <a name="available-languages---ssdt-for-vs-2017"></a>支持的语言 - SSDT for VS 2017

此版本的 SSDT for VS 2017 可安装以下语言  ：

* [简体中文](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x804)
* [繁体中文](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x404)
* [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x409)
* [法语](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40c)
* [德语](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x407)
* [意大利语](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x410)
* [日语](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x411)
* [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x412)
* [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x416)
* [俄语](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x419)
* [西班牙语](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40a)

### <a name="considerations-and-limitations"></a>注意事项和限制

* 无法脱机安装社区版

* 若要升级 SSDT，需要遵循用于安装 SSDT 的相同路径。 例如，如果使用 VSIX 扩展添加了 SSDT，则必须通过 VSIX 扩展进行升级。 如果通过单独安装来安装了 SSDT，需要使用这种方法进行升级。

## <a name="offline-install"></a>脱机安装

若要在未连接到 Internet 时安装 SSDT，请按照此部分中的步骤操作。 有关详细信息，请参阅 [创建 Visual Studio 2017 的网络安装](/visualstudio/install/create-a-network-installation-of-visual-studio)。

首先，在联机  时完成以下步骤：

1. [下载 SSDT 独立安装程序](#ssdt-for-vs-2017-standalone-installer)。

2. [下载 vs_sql.exe](https://aka.ms/vs/15/release/vs_sql.exe)。

3. 仍联机时，执行以下命令之一以下载脱机安装所需的全部文件。 使用 `--layout` 选项是关键，它用于下载脱机安装的实际文件。 将 `<filepath>` 替换为保存文件的实际布局路径。
   1. 对于某种具体语言，请传递区域设置：`vs_sql.exe --layout c:\<filepath> --lang en-us`（一种语言大约为 1GB）。
   1. 对于所有语言，请省略 `--lang` 参数：`vs_sql.exe --layout c:\<filepath>`（所有语言大约为 3.9GB）。

完成上述步骤后，可以脱机  完成以下步骤：

1. 运行 `vs_setup.exe --NoWeb` 以安装 VS2017 Shell 和 SQL Server 数据项目。

2. 从布局文件夹运行 `SSDT-Setup-ENU.exe /install` 并选择 SSIS/SSRS/SSAS。
   a. 若要执行无人参与安装，请运行 `SSDT-Setup-ENU.exe /INSTALLALL[:vsinstances] /passive`。

有关可用选项，请运行 `SSDT-Setup-ENU.exe /help`

> [!NOTE]
> 如果使用的是完整版 Visual Studio 2017，请仅为 SSDT 创建脱机文件夹，并从这一新建文件夹运行 `SSDT-Setup-ENU.exe`（请勿将 SSDT 添加到另一个 Visual Studio 2017 脱机布局）。 如果将 SSDT 布局添加到现有 Visual Studio 脱机布局，则无法在其中创建必要的运行时 (.exe) 组件。

## <a name="supported-sql-versions"></a>受支持的 SQL 版本

|项目模板|支持的 SQL 平台|
|-------------------|--------------------|
|关系数据库| SQL Server 2005\* - SQL Server 2017<br> （使用适用于 Visual Studio 2017 的 SSDT 17.x 或 SSDT 来连接 [Linux 上的 SQL Server](../linux/sql-server-linux-overview.md)）<br /><br />Azure SQL Database<br /><br />Azure Synapse Analytics（仅支持查询；尚不支持数据库项目）<br /><br /> \* SQL Server 2005 支持已停止提供，<br /><br /> 请迁移到官方支持的 SQL 版本|
|Analysis Services 模型<br /><br />Reporting Services 报表 | SQL Server 2008 - SQL Server 2017|
|Integration Services 包| SQL Server 2012 - SQL Server 2019 |

## <a name="dacfx"></a>DacFx

SSDT for Visual Studio 2015 和 2017 都使用 DacFx 17.4.1：[下载数据层应用程序框架 (DacFx) 17.4.1](https://www.microsoft.com/download/details.aspx?id=56508)。

## <a name="previous-versions"></a>以前的版本

若要下载并安装 SSDT for Visual Studio 2015 或较旧版本的 SSDT，请参阅[以前版本的 SQL Server Data Tools（SSDT 和 SSDT-BI）](previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi.md)。

## <a name="see-also"></a>另请参阅

* [SSDT MSDN 论坛](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=ssdt) 

* [SSDT 团队博客](/archive/blogs/ssdt/)

* [DACFx API 参考](/previous-versions/sql/sql-server-2014/dn645454(v=sql.120))

* [下载 SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)

## <a name="next-steps"></a>后续步骤

安装 SSDT 后，阅读这些教程，了解如何使用 SSDT 创建数据库、包、数据模型和报表。

* [面向项目的脱机数据库开发](project-oriented-offline-database-development.md)

* [SSIS 教程：创建简单的 ETL 包](../integration-services/ssis-how-to-create-an-etl-package.md)

* [Analysis Services 教程](/analysis-services/analysis-services-tutorials-ssas)

* [创建基本表报表（SSRS 教程）](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
