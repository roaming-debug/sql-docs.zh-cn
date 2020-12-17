---
title: SQL Server Data Tools (SSDT) 发行说明
description: 查看适用于 Visual Studio 2017 和早期 Visual Studio 版本的 SQL Server Data Tools (SSDT) 的所有版本的发行说明。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: b071f8b8-c8e5-44e0-bbb6-04804dd1863a
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/15/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: c59e142ec924a573e127d9caa0cd7d6ac9bf9e8f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440488"
---
# <a name="release-notes-for-sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT) 发行说明

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

这些发行说明适用于 [SQL Server Data Tools (SSDT)](download-sql-server-data-tools-ssdt.md) for Visual Studio (VS)。

<!--
Hello.  We have switched to a newer standardized format for Release Notes articles.
Basically we have switched from bullet lists to a 2-column table.
And we have shortened the H2 titles, to reduce wrapping in the rightNav.
And we have renamed the .md file to the standard format, from 'changelog-for-sql-server-data-tools-ssdt.md' to 'release-notes-ssdt.md'.
The presently latest H2 section (## 15.9.0) has been converted to the new format.
But the older sections are too numerous and long to warrant conversion.

REQUEST_1:  Please use the newer 2-column table format from now onward, for each new release section.

REQUEST_2:  Please consider whether it is time to erase perhaps the oldest 25% of these H2 sections.
Or maybe move all but the latest 9 (of 25) H2 sections to a new file perhaps named 'release-notes-history-ssdt.md', and link to it from the bottom of this file.

For questions, contact CraigG or SStein or GeneMi.

GeneMi , 2019/03/22.

P.S.  there's no need to keep this large HTML comment indefinitely.
-->

## <a name="1596nbsp-ssdt-for-vs-2017"></a>15.9.6,&nbsp; SSDT for VS 2017

发布时间：&nbsp; 2020 年 8 月 31 日  
生成号：&nbsp; 14.0.16222.0  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

| 新项 | 详细信息 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | 修复了连接到 SQL Server Analysis Services (SSAS) 数据源时“预览”按钮在 OLE DB 源上不起作用的问题。 |
| Integration Services (SSIS) | 修复了在删除关联路径之前删除数据流组件的输入或输出可能会引发 COMException 错误的问题。 |
| Integration Services (SSIS) | 修复了 SSAS 处理任务无法连接到 Power BI 工作区并刷新其模型的问题。 |
| Integration Services (SSIS) | 修复了在使用 x64 运行时和面向 SQL Server 2017 时，Visual Studio 在调试脚本任务/组件时挂起的问题。 |
| Integration Services (SSIS) | 修复了在某些环境中选择 MySQL 驱动程序时导入/导出向导崩溃的问题。 |
| Integration Services (SSIS) | 修复了一些与辅助功能和高 DPI 相关的问题。 |
| Integration Services (SSIS) | 允许用户在打开包时跳过验证，从而提高性能。 有关详细信息，请参阅[在 SSDT 中加速打开 SSIS 包](https://techcommunity.microsoft.com/t5/sql-server-integration-services/accelerate-the-opening-of-ssis-package-in-ssdt/ba-p/1607099)。 |
| Integration Services (SSIS) | 当目标服务器版本不为 SQL Server 2017 时，阻止部署到 Azure-SSIS。 |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :---------- | :------ |
| 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 | 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。 |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持 OData v4。 | &nbsp; |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持使用 ODBC 连接到 Oracle。 | &nbsp; |
| 未本地化 Power Query 源。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1595nbsp-ssdt-for-vs-2017"></a>15.9.5,&nbsp; SSDT for VS 2017

发布时间：&nbsp; 2020 年 5 月 27 日  
生成号：&nbsp; 14.0.16218.0  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

| 新项 | 详细信息 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | 通过在 SSIS 工具箱中添加搜索框，添加了对搜索任务和管道组件的支持。 |
| Integration Services (SSIS) | 添加了切换目标服务器版本时的进度栏。 |
| Integration Services (SSIS) | 添加了针对启用了 Azure 的项目的其他云配置，并添加了用于在 Azure 中执行包的 Windows 身份验证支持。 |
| Integration Services (SSIS) | 添加了针对 Azure 中启用了 Azure 的项目中要执行的包的评估。 |
| Integration Services (SSIS) | 修复了以下问题：在某些情况下无法在安装程序中列出 VS 实例 |
| Integration Services (SSIS) | 修复了以下问题：如果已卸载 VS 实例，则无法卸载此生产。 |
| Integration Services (SSIS) | 修复了以下问题：当目标服务器版本低于 SQL Server 2019 时，无法在调试期间正确加载从同一包中的另一个脚本组件复制的脚本组件。 |
| Integration Services (SSIS) | 修复了以下辅助功能问题：包设计器窗口下的组件连接器线条亮度比率小于 3:1。 |
| Integration Services (SSIS) | 修复了以下辅助功能问题：包设计器窗口下显示的“使视图适合窗口大小”控件的亮度比率小于 3:1。 |
| Integration Services (SSIS) | 修复了以下问题：当数据库具有包含 filestream 的文件组时，“传输数据库任务”无法正常运行。 |
| Integration Services (SSIS) | 修复了以下问题：在 Foreach 循环组件中使用 ODBC 组件时，ODBC 组件在包执行过程中将在第二个循环中出现“函数序列错误”。 |
| Integration Services (SSIS) | 修复了以下问题：Rebuild Index Task UI 将在低分辨率模式下中断。 |
| Integration Services (SSIS) | 修复了以下问题：“登录”按钮在高 DPI 模式下不显示。 |
| Integration Services (SSIS) | 修复了以下问题：连接管理器元素在高 DPI 模式下显示过大。 |
| Integration Services (SSIS) | 修复了以下问题：执行结果在高 DPI 模式下彼此堆叠。 |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :---------- | :------ |
| 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 | 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。 |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持 OData v4。 | &nbsp; |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持使用 ODBC 连接到 Oracle。 | &nbsp; |
| 未本地化 Power Query 源 | &nbsp; |
| 将 SQL Server 2017 和 SxS 面向使用 CU19 或更高版本 CU 修补的 SQL Server 2017 时，如果 Run64BitRuntime 设置为 true，包含带断点的脚本任务/组件的调试包会挂起。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1594nbsp-ssdt-for-vs-2017"></a>15.9.4,&nbsp; SSDT for VS 2017

发布时间：  &nbsp; 2020 年 3 月 26 日  
生成号：  &nbsp; 14.0.16214.0  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

| 新项 | 详细信息 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | 修复了在容器内移动控制流约束行时 VS 可能崩溃的问题。 |
| Integration Services (SSIS) | 修复了以下问题：维护计划任务的 UI 无法列出在任务 UI 之外创建的 ADO.NET 连接管理器。 |
| Integration Services (SSIS) | 修复了以下问题：如果一个 SSAS 项目属于同时加载了 SSIS 项目的解决方案，则部署该 SSAS 时不显示 Azure 交互式登录页。 |
| Integration Services (SSIS) | 修复了以下问题：未安装 SQL Server 时，单击 MSOLAP 驱动程序属性按钮会导致 DTS 向导崩溃。 |
| Integration Services (SSIS) | 修复了以下问题：MSOLEDBSQL 驱动程序不支持 DTS 向导中的 AAD 身份验证。 |
| Integration Services (SSIS) | 修复了以下问题：以 SQL Server 2012 为目标时，无法正确持久化 XML 源和 ADO.NET 目标。 |
| Integration Services (SSIS) | 修复了 Web 服务任务编辑器中的“下载 WSDL”按钮可能无法正确显示的问题。 |
| Integration Services (SSIS) | 修复了查找转换编辑器的“连接管理器”页中可能无法选择表的问题。 |
| Integration Services (SSIS) | 修复了缓存转换编辑器布局可能混乱的问题。 |
| Integration Services (SSIS) | 修复了包编辑器中“连接管理器”区域可能无法正确显示的问题。 |
| Integration Services (SSIS) | 修复了“转换为包部署模型”向导中可能无法正确显示状态图标的问题。 |
| Integration Services (SSIS) | 已将安装程序更改为不需要从 Internet 下载有效负载的完整安装程序。 |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :---------- | :------ |
| 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 | 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。 |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持 OData v4。 | &nbsp; |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持使用 ODBC 连接到 Oracle。 | &nbsp; |
| 未本地化 Power Query 源 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1593nbsp-ssdt-for-vs-2017"></a>15.9.3，SSDT&nbsp;for VS 2017

发布时间：  2020&nbsp;年 1 月 3 日  
生成号：  14.0.16203.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

| 新项 | 详细信息 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | 删除了收件箱组件 Power Query Source for SQL Server 2017。 目前，我们已宣布将 Power Query Source for SQL Server 2017 和 2019 作为现成组件推出（可以从[此处](https://www.microsoft.com/en-us/download/details.aspx?id=100619)下载）。 |
| Integration Services (SSIS) | 删除了收件箱组件 Microsoft Oracle Connector for SQL Server 2019。 目前，我们已宣布将 Microsoft Oracle Connector for SQL Server 2019 作为现成组件推出（可以从[此处](https://www.microsoft.com/en-us/download/details.aspx?id=58228)下载）。 |
| Integration Services (SSIS) | 修复了以下问题：当目标服务器版本为 SQL Server 2017 或 2019 时，由于 IDtsHost 接口未注册，SSIS 调试器有时可能无法启动。 |
| Integration Services (SSIS) | 修复了高 DPI 模式下的主要 UI 布局问题。 |
| Integration Services (SSIS) | 当目标服务器版本为 SQL Server 2019 时，用于脚本任务/组件的 .NET Framework 版本已升级为 4.7。 |
| Integration Services (SSIS) | 在 ODBC 连接管理器中新增了 ConnectByProxy 属性，以便支持启用自承载集成运行时作为 ODBC 连接管理器中的代理。 |
| Integration Services (SSIS) | 修复了以下问题：用户无法在包部署模式下添加新的数据源。 |
| Integration Services (SSIS) | 修复了以下问题：如果代码使用了任何在 .NET 4.5 之后引入的新语法，用户无法调试脚本任务/组件。 |
| Integration Services (SSIS) | 修复了以下问题：可能无法通过集成运行时创建向导在 Azure 订阅中创建首个数据工厂，因为数据工厂资源提供程序未注册。 |
| Integration Services (SSIS) | 修复了以下问题：如果订阅中有“仅限文件”存储帐户，ADF 连接向导中的 SSIS 无法正确显示 Azure 存储帐户列表。 |
| Integration Services (SSIS) | 修复了以下问题：如果包内有容器，无法使用“在 Azure 中执行”。 |
| Integration Services (SSIS) | 修复了以下问题：char(n char) 和 varchar2(n char) 在 Oracle 连接器中映射到不正确的 DTS 类型。 |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :---------- | :------ |
| 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 | 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。 |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持 OData v4。 | &nbsp; |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持使用 ODBC 连接到 Oracle。 | &nbsp; |
| 未本地化 Power Query 源 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1592nbsp-ssdt-for-vs-2017"></a>15.9.2，SSDT&nbsp;for VS 2017

发布时间：  2019&nbsp;年 7 月 17 日  
生成号：  14.0.16194.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

| 新项 | 详细信息 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | 添加 AzureEnabled 功能。 启用要在 Azure 数据工厂中的 SSIS 平台即服务 (PaaS) 上运行的项目的包 |
| Integration Services (SSIS) | 解决无法通过变量表达式设置 Oracle 连接器属性的问题 |
| Integration Services (SSIS) | 解决在调试以 SQL Server 2019 之前版本为目标的包时 Oracle 连接器具有 VS_NEEDSNEWMETATDATA 错误的问题 |
| Integration Services (SSIS) | 解决在包/项目使用连接管理器属性的表达式时 Oracle 连接器未能升级/降级包/项目的问题 |
| Integration Services (SSIS) | 修复了以下问题：Web 服务任务编辑器的“下载 WSDL”按钮不支持 TLS 1.1 和 1.2 协议（目标为 SQL Server 2019） |
| Integration Services (SSIS) | 修复了以下问题：保存后无法再次加载包含 DQS 连接管理器的包 |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :---------- | :------ |
| 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 | 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。 |
| 无法在包部署模型中创建或编辑数据源。 | 无法打开数据源向导。 |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持 OData v4。 | &nbsp; |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持使用 ODBC 连接到 Oracle。 | &nbsp; |
| 未本地化 Power Query 源 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1591nbsp-ssdt-for-vs-2017"></a>15.9.1，SSDT&nbsp;for VS 2017

发布时间：  2019&nbsp;年 4 月 27 日  
生成号：  14.0.16191.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

| 新项 | 详细信息 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | 修复了以下问题：在定目标到旧版 SQL Server 时，无法正确暂留包部件。 |
| Integration Services (SSIS) | 修复了以下问题：在使用包部件时，无法将表达式添加到优先约束。 |
| Integration Services (SSIS) | 修复了以下问题：Power Query 源和连接管理器的“帮助”按钮没有链接到正确文档。 |
| Integration Services (SSIS) | 修复了以下问题：VS 帮助窗口中不显示 SSIS 内部版本。 |
| Integration Services (SSIS) | 添加了适用于 Ole DB 和平面文件连接管理器的属性“ConnectByProxy”，此属性可使用 Azure-SSIS IR 中的自承载 IR 启用对本地数据的访问权限。 |
| Integration Services (SSIS) | 修复了以下问题：ODBC 组件错误地映射到 DT_DBDATE 数据类型。 |
| Integration Services (SSIS) | 添加了适用于 ADO.NET 和 OLE DB 连接管理器的属性“ConnectUsingManagedIdentity”，此属性可启用托管标识身份验证，以连接到 Azure-SSIS IR 中的数据源。 |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :---------- | :------ |
| 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 | 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。 |
| 无法在包部署模型中创建或编辑数据源。 | 无法打开数据源向导。 |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持 OData v4。 | &nbsp; |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持使用 ODBC 连接到 Oracle。 | &nbsp; |
| 未本地化 Power Query 源。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1590nbsp-ssdt-for-vs-2017"></a>15.9.0，SSDT&nbsp;for VS 2017

发布时间：  2019&nbsp;年 1 月 28 日  
生成号：  14.0.16186.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

| 新项 | 详细信息 |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Integration Services (SSIS) | 在 ADF 2017 中为 SSIS 添加了 Power Query 源（预览版）。 |
| Integration Services (SSIS) | 重新添加对 SQL Server 2012 的支持。 |
| Integration Services (SSIS) | 为 SQL Server 2019 添加了 Oracle 源和目标。 |
| Integration Services (SSIS) | 定目标到 SQL Server 2019 的 Oracle 源和目标已由 SSDT 安装。 <br/></br> 若要设计定目标到服务器版本 2017 或更低版本的包，请从 Microsoft 下载网站下载相应的 Oracle 连接器版本，并将它安装在 SSDT 计算机上。 <br/></br> [适用于 Oracle 的 Attunity Microsoft Connector 版本 5.0（定目标到 SQL Server 2017）](https://www.microsoft.com/download/details.aspx?id=55179 ) <br/></br> [适用于 Oracle 的 Attunity Microsoft Connector 版本 4.0（定目标到 SQL Server 2016）](https://www.microsoft.com/download/details.aspx?id=52950 )<br/></br> [适用于 Oracle 的 Attunity Microsoft Connector 版本 3.0（定目标到 SQL Server 2014）](https://www.microsoft.com/download/details.aspx?id=44582 )<br/></br> [适用于 Oracle 的 Attunity Microsoft Connector 版本 2.0（定目标到 SQL Server 2012）](https://www.microsoft.com/download/details.aspx?id=29283 ) |
| Integration Services (SSIS) | 修复了以下问题：从早期 SSIS 版本迁移时无法加载脚本任务/组件。 |
| Integration Services (SSIS) | 解决了数据查看器在 Windows 7 SP1 和 Windows 8.1 上无法运行的问题。 |
| Integration Services (SSIS) | 解决了在某些情况下保存包导致 Visual Studio 崩溃的问题。 |
| Integration Services (SSIS) | 修复了以下问题：在某些情况下无法执行包。 |
| Integration Services (SSIS) | 同时满足以下两个条件时，就会出现此问题：< br />< br /> &bull;   保护级别为 EncryptSensitiveWithPassword。< br /> &bull;   目标服务器版本低于 SQL Server 2017。          |
| Integration Services (SSIS) | 修复了 SSDT 中未显示使用默认字体的注释的问题。 |
| Integration Services (SSIS) | ISDeploymentWizard 支持命令行模式下的 SQL 身份验证、Azure Active Directory 集成身份验证和 Azure Active Directory 密码身份验证。 |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :---------- | :------ |
| 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 | 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。 |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持 OData v4。 | &nbsp; |
| 当 SSIS 和 SSAS 安装在同一个 Visual Studio 实例上时，Power Query 源可能不支持使用 ODBC 连接到 Oracle。 | &nbsp; |
| 未本地化 Power Query 源。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="1582nbsp-ssdt-for-vs-2017"></a>15.8.2，SSDT&nbsp;for VS 2017

发布时间：  2018&nbsp;年 11 月 5 日  
生成号：  14.0.16182.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能
**SSIS：**

修复了以下问题：将包内有脚本任务/平面文件目标的 SSIS 项目部署到 Azure-SSIS 会导致包无法在 Azure-SSIS 中执行。 

### <a name="known-issues"></a>已知问题：

- 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。

## <a name="1581nbsp-ssdt-for-vs-2017"></a>15.8.1，SSDT&nbsp;for VS 2017

发布时间：  2018&nbsp;年 9 月 27 日  
生成号：  14.0.16179.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

**SSIS：**

1. 添加对 [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] 的支持。
2. 删除对 SQL Server 2012 的支持。

### <a name="known-issues"></a>已知问题：

- 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。
- 将包含包（包中包含脚本任务/平面文件目标）的 SSIS 项目部署到 Azure-SSIS 可导致该包无法在 Azure-SSIS 中执行。

## <a name="158nbsp-ssdt-for-vs-2017"></a>15.8，SSDT&nbsp;for VS 2017

发布时间：  2018&nbsp;年 9 月 5 日  
生成号：  14.0.16174.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

**SSIS：**

1. VS 15.8 修复了以下回归问题：保存脚本任务/组件会触发编译错误。
1. VS 15.8 修复了以下回归问题：部署向导无法正常运行。
1. 修复了以下问题：ADO.NET 连接管理器不支持第三方 ADO.NET 提供程序。

**安装程序：**

- 在 Windows 10 上安装 SSDT 时实现中途重新启动。


### <a name="known-issues"></a>已知问题：

- 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。

## <a name="1571nbsp-ssdt-for-vs-2017"></a>15.7.1，SSDT&nbsp;for VS 2017

发布时间：  2018&nbsp;年 7 月 2 日  
生成号：  14.0.16167.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

**SSIS：**

- 添加对新的 Azure 政府 AAD 颁发机构 (login.microsoftonline.us) 的支持，以便与 AS 任务一起使用。
- 修复了当目标服务器版本为 SQL Server 2016 时，AS 处理任务 UI 将显示“找不到方法”的问题。
- 修复了以下问题：目标服务器版本为 SQL Server 2012 时，无法执行某些管道组件。

**安装程序：**

- 筛选 VS 实例列表以排除无法安装 SSDT 的实例。

### <a name="known-issues"></a>已知问题：

- 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。
- 在 Windows 10 上安装 SSDT 并选择“为 Visual Studio 2017 实例安装新的 SQL Server Data Tools”时，安装将因“不支持所请求的元文件操作”而失败。 重新启动计算机，并再次启动 SSDT 安装程序，以继续安装。

## <a name="1570nbsp-ssdt-for-vs-2017"></a>15.7.0，SSDT&nbsp;for VS 2017

发布时间：  2018&nbsp;年 6 月 4 日  
生成号：  14.0.16165.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

**SSIS：**

- 修复了以下问题：“选项”对话框中“Integration Services 设计器”页无法正常显示。  
- 修复“排序转换编辑器”编辑器中文本显示的亮度比问题  。  
- 修复尝试编辑组合框时“解决引用”对话框消失的问题  。  
- 修复“Hadoop 连接管理器”的 F1 帮助链接不起作用的问题  。  
- 修复面向 SQL Server 2016 时位于容器中的脚本任务代码丢失的问题。  

**安装程序：**

- 修复了以下问题：在 VS 15.7.2 中安装 SSRS 和 SSIS 之前无法安装 SSAS。

### <a name="known-issues"></a>已知问题：

- 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试   。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。

## <a name="1560nbsp-ssdt-for-vs-2017"></a>15.6.0，SSDT&nbsp;for VS 2017

发布时间：  2018&nbsp;年 4 月 10 日  
生成号：  14.0.16162.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

**SSIS：**

- 修复问题：以 SQLServer2016 和 SQLServer2017 为目标时，AS 处理任务不记录任何处理步骤
- 修复了以下问题：在 SSDT 中打开具有很长非英语任务名称的 dtsx 时，发生访问冲突
- 修复问题：ScriptTask 变量列表有时会在任务 UI 中消失
- 修复问题：当包位置为 SQL Server 时，添加现有包的副本失败
- 修复问题：访问某个编辑器对话框中的组合框时，焦点会卡住。
- 修复问题：切换 VS 主题时背景不更改。
- 修复问题：注释和加载标签在深色主题中不可见。
- 修复问题：SSIS 工具箱禁用项的状态属性定义不正确。
- 修复问题：执行 WebServiceTask 始终失败。
- 修复问题：如果连接字符串设置为某个变量，且其中包含依赖于项目参数的表达式，包部署会失败。

**安装程序：**

- 在隐私免责声明中添加“SQL Server Data Tools 客户体验改善计划”链接。
- 修复了以下问题：选择“安装新 SQL Server Data Tools for Visual Studio 2017 实例”时，会看到 VS 安装程序弹出窗口

### <a name="known-issues"></a>已知问题：
- 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。

## <a name="1552nbsp-ssdt-for-vs-2017"></a>15.5.2，SSDT&nbsp;for VS 2017

生成号：  14.0.16156.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

**SSIS**
- 修复了 SSAS 和 SSIS 安装到同一 VS 2017 实例时，SSIS 2008 项目迁移会失败的问题。
- 修复了以下问题：Rdlc 报表设计器和 SSIS 安装到同一 VS 2017 实例时，无法生成 Rdlc 项目。
- 修复了注释颜色无法更新的问题。
- 修复了使用其他语言时，某些字符串在 Hadoop 连接管理器编辑器中截断的问题。
- 修复了某些字符串在 OData 连接管理器编辑器中截断的问题。
- 修复了某些字符串在 Integration Services 导入项目向导窗口中截断的问题。
- 修复了 SSIS 工具框信息窗口中的标题问题。
- 修复了某些字符串在 Integration Services 部署向导窗口中截断的问题。 

**安装程序**
- 修复了以下问题下载有效负载有时失败，出现错误“系统找不到指定的文件 (0x80070002)”。  

### <a name="known-issues"></a>已知问题
- 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试   。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。

## <a name="1551nbsp-ssdt-for-vs-2017"></a>15.5.1，SSDT&nbsp;for VS 2017

生成号：  14.0.16148.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

除了安装程序的以下 bug 修复以外，Visual Studio 2017 (15.5.1) 与版本 15.5.0 相同：

1. 修复安装程序在 SQL Server Integration Services 安装后停止响应的问题。
2. 修复了安装失败后会显示以下错误消息的问题：“不支持请求的元文件操作(0x800707D3)”。

除了这两个 bug 修复之外，以下有关 15.5.0 的详细信息也仍适用于 15.5.1

## <a name="1550nbsp-ssdt-for-vs-2017"></a>15.5.0，SSDT&nbsp;for VS 2017

生成号：  14.0.16146.0&nbsp;  
SSDT for Visual Studio 2017。 

### <a name="whats-new"></a>新增功能

SSDT for Visual Studio 2017 (15.5.0) 不再提供预览版，改为提供正式版 (GA)。

**安装程序**
1. 安装程序 UI 已本地化。
1. 图标替换为更高质量的版本。

**Integration Services (IS)**
1. 在 ADF 中将包部署到 Azure SSIS IR 时，部署向导中添加了包验证步骤，可发现要在 Azure SSIS IR 中执行的 SSIS 包的潜在兼容性问题。 有关详细信息，请参阅[验证部署到 Azure 的 SSIS 包](../integration-services/lift-shift/ssis-azure-validate-packages.md)。
1. SSIS 扩展已本地化。

### <a name="bug-fixes"></a>Bug 修复

**Integration Services (IS)**
1. 修复了 OLEDB 和 ADO.NET 连接管理器的布局损坏的问题。
2. 修复了尝试编辑维度处理任务时出现程序集未发现已出现错误的问题。

### <a name="known-issues"></a>已知问题

**Integration Services (IS)** 当 ExecuteOutOfProcess 设置为 True 时，SSIS 执行包任务不支持调试。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。

## <a name="173nbsp-ssdt-for-vs-2015"></a>17.3，SSDT&nbsp;for VS 2015

生成号：  14.0.61712.050&nbsp;  
SSDT for Visual Studio 2015。 

### <a name="whats-new"></a>新增功能

**Analysis Services (AS) 项目**
- 向表格项目添加了三个新选项（在“选项”>“Analysis Services 表格”>“数据导入”下）：
  - 启用旧数据源 - 允许用户在较新的兼容性模式下创建较早的“1200 兼容性模式”数据源。
  - 自动检测类型 - 启用此选项后，新式数据源的查询编辑器会在加载非结构化查询后尝试检测该查询的数据类型。 如果检测成功，可能会向查询添加新步骤。
  - 运行背景分析 - 启用此选项后，新式数据源的查询编辑器会在加载查询时对数据源运行查询，以分析查询的输出架构。

**Integration Services (IS)**
- 在 ADF 中将包部署到 Azure SSIS IR 时，部署向导中添加了包验证步骤，可发现要在 Azure SSIS IR 中执行的 SSIS 包的潜在兼容性问题。 有关详细信息，请参阅[验证部署到 Azure 的 SSIS 包](../integration-services/lift-shift/ssis-azure-validate-packages.md)。

### <a name="bug-fixes"></a>Bug 修复

**Analysis Services (AS) 项目：**
- 修复了在将模型更改签入到 TFS 时可能导致未经处理的异常的问题。
- 修复了在将包含复杂 M 表达式的表添加到 1400 模型时可能导致异常的问题。
- 修复了在模型关系图视图中搜索元数据时可能导致 Visual Studio 崩溃的问题。
- 修复了在将更改保存到分区 M 查询时可能导致计算列从表定义中删除的 1400 模型问题。
- 修复了在获取数据\表编辑器 UI 中对 1400 模型使用重命名查询时，UI 可能在验证当前数据模型的兼容性时冻结的问题。
- 修复了在将 1400 模型部署到 Azure Analysis Service 时可能导致缺少 Newtonsoft 程序集引用的问题。
- 修复了某些情况下导致通过 PQ 将数据导入 1400 模型出错的问题。
- 修复了 Windows 缩放集时可能出现的 PowerQuery 用户界面对话框缩放问题。
- 修复了重命名角色时出现的问题。
- 修复了某些情况下可能导致未正确保存\同步更改的项目配置问题。
- 修复了 PowerQuery 编辑器自动添加“更改类型”步骤的问题。
- 修复了切换到集成工作区模式/从集成工作区模式切换后导致打开 BIM 文件出错的问题。
- MaxConnections 属性现对表格模型中的数据源可见。
- 增大了 PowerQuery 编辑器窗口的初始大小。
- 现在，PowerQuery 编辑器中显示的 M 查询关键字（如“源”）已本地化。
- 在使用 1400 模型和结构化数据源时缓存凭据，以避免为每个编辑的表输入相同的凭据。

**RS 项目：**
- 修复了阻止在多报表项目中部署单一报表的问题
- 修复了可能导致部署问题的共享数据源问题
- 修复了在代码视图、设计视图和查询编辑器窗口之间切换时撤消管理器可能出现故障的问题
- 修复了可能导致参数窗格在出现运行时错误后消失的问题
- 修复了可能导致报表项目丢失源代码管理映射的报表项目问题

**Integration Services：**
- 修复了切换 Analysis Services 进程任务上的连接时可能出现的问题
- 修复了某些任务/组件未适当本地化的问题。
- 修复了对 CDC 应用 SQL 修复，添加 \__$command\_id 列后，CDC 组件中断的问题。

## <a name="1540-previewnbsp-ssdt-for-vs-2017"></a>15.4.0（预览版），SSDT&nbsp;for VS 2017

生成号：  14.0.16134.0&nbsp;  
SSDT for Visual Studio 2017。 
  
### <a name="whats-new"></a>新增功能

此版本为 Visual Studio 2017 15.4 或更高版本中的 SQL Server 数据库、Analysis Services、Reporting Services 和 Integration Services 项目提供了独立的 Web 安装程序。

### <a name="installer"></a>安装程序

- 允许用户在安装新的 SSDT for VS 2017 实例时设置昵称。
- 在未选择任何 VS 实例的情况下会隐藏安装程序的功能选择复选框。
- 根据客户反馈优化了安装程序的某些消息。
- 修复了安装程序不支持升级的问题。

### <a name="ssis"></a>SSIS

- 修复了以下问题：安装 Azure 功能包后导入/导出向导无法列出数据源。
- 修复了切换连接时编辑 SSIS Analysis Services 进程任务会引发异常的问题。
- 修复了在应用添加 __$command_id 列的 SQL 修补程序后 CDC 组件会中断的问题。
- 修复了以下问题：定目标到旧版 SQL Server 时，无法编辑和执行第三方包。
- 修复了以下问题：双击 DTSWizard.exe 并选择“平面文件源”时，“平面文件源”配置对话框没有正确显示。
- 修复了面向 SQL Server 2017 时包含 Azure 功能包任务/组件的包无法执行的问题。

**已知问题**

- 安装程序尚未本地化。
- 当 ExecuteOutOfProcess 设置为“True”时，SSIS 执行包任务不支持调试  。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。

## <a name="1730nbsp-ssdt-for-vs-2015"></a>17.30，SSDT&nbsp;for VS 2015

生成号：  14.0.61709.290&nbsp;  
SSDT for Visual Studio 2015。 

### <a name="whats-new"></a>新增功能

**Analysis Services (AS)**

- 在 1400 个模型中启用了 Cosmos DB 和 HDI Spark。
- 表格数据源属性。
- 现在支持“空白查询”选项，用于在查询编辑器中为 1400 兼容级别的模型创建新查询。
- 1400 模式模型的查询编辑器现在支持保存查询，而系统不会自动处理新的表格。

**Reporting Services (RS)**

- 现在打开项目时会提示格式已升级，以支持使用 MSBuild 进行生成和部署。

### <a name="known-issues"></a>已知问题

**Analysis Services (AS)**

- 具有透视的 1400 兼容级别的直接查询模型在查询和发现元数据时失败。

**Reporting Services (RS)**

- 新的报表项目格式不会保留源代码管理绑定，并会引发与以下消息类似的错误：

   项目文件 C:\path 未绑定到源代码管理，但该解决方案包含其中的源代码管理绑定信息。 

   若要解决此问题，每当打开解决方案时，请单击“使用解决方案绑定”  。

- 将项目升级到新的 MSBuild 格式后，保存可能会失败，消息类似于以下内容：

   “参数‘unevaluatedValue’不得为 null。”

   若要解决此问题，请更新“项目配置”  并填写“平台”  属性。

### <a name="bug-fixes"></a>Bug 修复

**Analysis Services (AS)**

- 大幅提升了加载表格模型关系图视图时的性能。
- 修复了大量问题，改进了 1400 兼容级别模型中 PowerQuery 的集成和体验。
   - 修复了阻止授予文件源编辑权限的问题。
   - 修复了不能更改文件源的源的问题。
   - 修复了文件源的 UI 显示错误的问题。
- 修复了在“加入日期”关系处于非活动状态的情况下，导致删除了 "JoinOnDate" 属性的问题。
- 查询生成器中的“新建查询”选项现在允许创建一个新的空白查询。
- 修复了导致对现有数据源查询的编辑不能在 1400 兼容级别下更新表格的模型定义的问题。
- 修复了可能引起异常的自定义上下文表达式的相关问题。
- 在 1400 表格模式中采用重复名称导入新的表格时，用户现在会收到通知，指示名称存在冲突，需要调整为唯一的名称。
- 在导入模式中，当前用户模拟模式已从模型中删除，因为它不是受支持的方案。
- PowerQuery 集成现支持用于其他数据源（OData.Feed、Odbc.DataSource、Access.Database、SapBusinessWarehouse.Cubes）的选项。
- 数据源的 PowerQuery 选项字符串现将根据客户端区域设置正确显示本地化文本。
- 关系图视图现在可以显示 1400 兼容级别模型中从 M 查询编辑器新建的列。
- Power Query 编辑器现提供不导入数据的选项。
- 修复了安装数据胶卷过程中的问题，该数据胶卷用于在 VS2017 的多维模型中从 Oracle 导入表格。
- 修复了在极少数情况下，鼠标游标离开表格公式栏时可能会导致故障的问题。
- 修改了“编辑表格属性”对话框中的一个问题，即更改表名会错误地更改源表名，进而导致发生意外错误。
- 修复了在多维项目中尝试在角色设计器“单元格数据”选项卡设计器中调用测试多维数据集安全性时，VS2017 中可能发生的故障。
- SSDT：对于表格数据源，属性是不可编辑的。
- 修复了可能导致 MSBuild 和 DevEnv 版本在某些情况下无法使用解决方案文件正常工作的问题。
- 大幅提升了在表格模型包含更大的元数据的情况下，提交模型更改（针对度量值、计算列的 DAX 编辑）时的性能
- 修复了在 1400 兼容级别模型中使用 PowerQuery 导入数据的过程中出现的大量问题
   - 单击“导入”后需要很长时间才能完成导入，而 UI 不会显示任何状态
   - 当尝试选择要非常慢地导入的表时，导航器视图上有表的大列表
   - 在查询编辑器视图中使用 35 个查询的列表时，查询编辑器的性能低下（PBI 桌面中也会出现该问题）
   - 导入多个表格会禁用工具栏，在某些情况下可能无法再完成该操作 
   - 使用 PQ 导入表格后，模型设计器处于禁用状态，并且未显示任何数据
   - 在 PQ UI 中取消选中“创建新表”，但仍会导致创建新表
   - 文件夹数据源未提示输入凭据 
   - 尝试对结构化数据源获取更新后的凭据时可能发生的未设置对象引用异常
   - 使用 M 表达式打开分区管理器这一操作非常缓慢
   - 在 PQ 编辑器中选择表格属性，这一操作未显示属性
- 提高了在 Power Query UI 集成中捕获顶级异常并在“输出”窗口中显示的可靠性
- 修复了结构数据源上的 ChangeSource 在上下文表达式中不会持续更改的问题
- 修复了 M 表达式错误可能导致无法更新模型，而不显示错误消息的问题
- 修复了关闭 SSDT 时显示“The build must be stopped before the solution can be closed”错误的问题
- 修复了在 1400 兼容级别模型中设置了错误的模拟模式时，VS 可能停止响应的问题 
- 目前，详细信息行属性不为空时，只会序列化为 JSON（更改了默认设置）
- Oracle OLEDB 驱动程序现可用于表格直接查询模式下的列表中
- 在 1400 兼容表格模型中添加 M 表达式，这一操作现在会在表格模型资源管理器 (TME) 中显示\刷新
- 修复了尝试在预先的 1400 兼容级别模型中使用“其他”数据源进行导入时，会导致 MSOLAP 提供程序不会出现在 VS2017 中的问题
- 修复了通过 TME 添加转换可能会导致出现问题的问题 
- 修复了对象级安全性接口中会导致选项卡在某些情况下不正确地显示\隐藏的问题
- 修复了尝试使用“连接到数据库”对话框打开以前加载的多维模型的过程中可能发生故障的问题
- 修复了将自定义程序集添加到多维模型时，会导致发生错误的问题

**Reporting Services (RS)**

- 修复了在 VS 2017 中编译和生成 RDLC 的过程中出现的问题

## <a name="1530-previewnbsp-ssdt-for-vs-2017"></a>15.3.0（预览版），SSDT&nbsp;for VS 2017

生成号：  14.0.16121.0&nbsp;  
SSDT for Visual Studio 2017。 
  
### <a name="whats-new"></a>新增功能

此预览版是 SSDT for Visual Studio 2017 的第一个版本。 此版本为 Visual Studio 2017 15.3 及以上版本中的 SQL Server 数据库、Analysis Services、Reporting Services 和 Integration Services 项目引入了独立的 Web 安装体验。

**已知问题**

- 安装程序尚未本地化。
- SSIS 尚未本地化。
- 当 ExecuteOutofProcess 设置为 True 时，SSIS 执行包任务不支持调试   。 此问题仅适用于调试。 通过 DTExec.exe 或 SSIS 目录进行保存、部署和执行将不受影响。
- 无法将包含第三方扩展的 SSIS 包切换为定目标到其他服务器版本。


## <a name="172nbsp-ssdt-for-vs-2015"></a>17.2，SSDT&nbsp;for VS 2015

生成号：  14.0.61707.300&nbsp;  
SSDT for Visual Studio 2015。 

### <a name="whats-new"></a>新增功能

**AS 项目：**
- 现可在“角色”  对话框中配置“对象级安全性”，便于在 1400 兼容级别表格模型中实现高级安全性。
- 用于在适用于 VS2017 的 SSDT AS 项目中的 AS Azure 模型中无电子邮件地址用户的新的 AAD 角色成员选择。
- SSDT AS 表格项目中新的 AS Azure“始终提示”项目属性，用于自定义 ADAL 凭据缓存的行为。

### <a name="bug-fixes"></a>Bug 修复

**常规**
- 更新了适用于 SQL Server 2017 的品牌引用。

**AS 项目**
- 为增强提交 DAX 测量更改和其他模型编辑时的体验进行了显著的性能修复。
- 修复了使用 1400-兼容级别表格模型的 Analysis Services 项目中有关 Power Query 集成的一些问题。
- 修复了在 VS2017 多维度项目中“设计聚合”设计器可能无法加载的问题。
- 修复了在 Analysis Services 多维度 DSV 图中拖动项可能导致 VS 2017 崩溃的问题。
- 修复了 AS 项目中“部署”对话框没有始终处于 Visual Studio 前台的问题。
- 删除了从作为数据源的数据市场导入 Analysis Services，因为该服务已停用。
- 修复了通过表格模型资源管理器从现有数据源“导入新表”后导致表设计器禁用的问题。
- 修复了可能导致“模型”菜单项从数据源/添加数据源导入以保持隐藏在错误的上下文中的问题。
- 改进了从表格模型资源管理器创建度量值时的体验，避免焦点切换回用于创建度量值的列。
- 从 AS 表格项目中的集成工作区切换到显式工作区服务器时，现在将清除旧的数据库文件。
- 修复了 AS 表格 1400 模型项目中的一个问题，该问题会导致“行级别安全性”复选框 UI 状态最初显示为未选中，而不管实际的基础对象状态如何。
- 修复了当使用 Power Query 和已引发的未经处理的异常将文本文件或 Excel 文件导入 1400 兼容模式表格模型时可能会发生的崩溃。
- 修复了 AS 表格模型设计器中 DAX 公式编辑控件中的滚动条缩略图可能出现的问题。
- 修复了当包含用户名/密码身份验证时阻止修改 PowerQuery 糅合数据源的问题。
- 修复了在连接字符串中设置其他属性时可能会阻止数据源连接的问题。
- 修复了当多个 AS 表格模型项目加载和关闭次要模型设计器而未与设计器中的任何内容交互时，可能会导致 VS 崩溃的问题。
- 修复了在某些情况下对 KPI 格式的编辑没有持续存在的问题。
- 修复了 PowerQuery UI 在不论公式栏是否显示的情况下都显示错误的菜单已检查状态的问题。
- 修复了具有 PowerQuery 数据源的 AS 表格 1400 兼容级别项目当从表格模型资源管理器中选择“更改数据源”菜单时，可能会导致 VS 崩溃的问题。
- 修复了导致加载 1400 表格模型可能会显示错误“未能加载文件或程序集‘Microsoft.ProBI.MashupLibrary’”的间歇性问题  。

**RS 项目**
- RS“标尺”和“参数”框设置选择状态的用户首选项在会话中会被正确记住。

**IS 项目**
- 修复了 ADO/ADO.NET ForEachLoop 容器未正确显示的问题
- 修复了某些任务/组件/向导未本地化的问题
- 将最新 TargetServerVersion 从“SQL Server vNext”更改为“SQL Server 2017” 

## <a name="1710nbsp-ssdt-for-vs-2015"></a>17.10，SSDT&nbsp;for VS 2015

生成号：  14.0.61705.170&nbsp;  
SSDT for Visual Studio 2015。 

### <a name="whats-new"></a>新增功能
**AS 项目：**
- 用户可以对 1400 模型上用户界面中的列设置编码提示
- 现在可在脱机模式下使用非模型相关 IntelliSense
- 表格模型资源管理器现在包含一个节点，用于表示可跨整个模型（1400 兼容级别表格模型）使用的命名 M 表达式
- Azure Active Directory 人员选取器与 Microsoft Azure 门户的标识和访问管理相似，现可在设置表格模型中的角色成员时使用

**数据库项目：**
- 更新至 DacFx 17.1

### <a name="bug-fixes"></a>Bug 修复
- 修复了 VS2017 Visual Studio 选项中商业智能设计器组名称显示不正确的问题
- 修复了为具有报表项目和 AS 项目的解决方案生成代码图时可能出现崩溃的问题
- 修复了 Analysis Services 1400 兼容级别表格模型的 PowerQuery 集成的大量相关问题
- 修复了新 DAX 编辑器工具窗口中的问题：定义度量值时赋值运算符不能位于单独一行
- 修复了重命名透视中的度量值时阻止表格度量值显示更新的问题
- 更新了 Analysis Services 集成工作区引擎和表格对象模型，表格对象模型修复了导致包含转换的 1200 表格项目无法部署到 SQL Server 2016 Analysis Services 服务器的回归问题
- 修复了导致创建/删除新 1400 表格数据源速度过慢的性能问题
- 修复了在不同 DSV 之间快速切换视图时多维模型中的 DSV 图示可能停止呈现的问题

## <a name="dacfx-171"></a>DacFx 17.1

- 修复了加密某列时内存优化表具有其他标识列的问题
- 提供用于创建数据库的 CATALOG_COLLATION 选项的 SQLDOM 支持

## <a name="dacfx-1701"></a>DacFx 17.0.1

- 修复了数据库具有由 HSM 使用 EKM 提供程序进行保护的非对称密钥的相关问题（[Connect 项目](https://connect.microsoft.com/SQLServer/feedback/details/3132749/sqlpackage-exe-fails-when-extracting-a-database-which-contains-an-asymmetric-key-using-an-ekm-provider)）

## <a name="170nbsp-ssdt-for-vs-2015"></a>17.0，SSDT&nbsp;for VS 2015

生成号：  14.0.61704.140&nbsp;  
SSDT for Visual Studio 2015。   
最高支持 SQL Server 2017。 

### <a name="whats-new"></a>新增功能
**数据库项目：**
- 在视图上修改聚集索引将不再阻止部署
- 与列加密相关的架构比较字符串会使用恰当的名称，而非实例名称。   
- 向 SqlPackage 添加了新的命令行选项：ModelFilePath。  这为高级用户提供了一个选项，可用于指定外部 model.xml 文件来执行导入、发布和脚本编写操作   
- 扩展了 DacFx API，用于支持 Azure AD 通用身份验证和多重身份验证 (MFA)

**IS 项目：**
- SSIS OData 源和 OData 连接管理器现支持连接到 Microsoft Dynamics AX Online 和 Microsoft Dynamics CRM Online 的 OData 源。
- SSIS 项目现在支持“SQL Server 2017”的目标服务器版本 
- 提供针对 SQL Server 2017 的 CDC 控制任务、CDC 拆分器和 CDC 源支持。 

**AS 项目：**
- Analysis Services PowerQuery 集成（1400 兼容级别表格模型）：
    - 如果用户已安装第三方驱动程序，DirectQuery 适用于 SQL、Oracle 和 Teradata
    - 在 PowerQuery 中按示例添加列
    - 1400 模型中的数据访问选项（M 引擎使用的模型级别属性）
        - 启用快速合并（默认为 false - 设置为 true 时，糅合引擎会在合并数据时忽略数据源隐私级别）
        - 启用旧版重定向（默认为 false - 设置为 true 时，糅合引擎会跟随可能不安全的 HTTP 重定向。    例如，从 HTTPS 到 HTTP URI 的重定向）  
        - 返回错误值 Null（默认为 false - 设置为 true 时，单元格级别错误会返回 null。 若为 false，在单元格包含错误时抛出异常）  
    - 使用 PowerQuery 的其他数据源（文件数据源）
        - Excel 
        - Text/CSV 
        - Xml 
        - Json 
        - Folder 
        - Access 数据库 
        - Azure Blob 存储 
    - 本地化的 PowerQuery 用户界面
- DAX 编辑器工具窗口
    - 改进了针对度量值、计算列和详细行表达式的 DAX 编辑体验，可通过 SSDT 中的“视图”和其他 Windows 菜单使用
    - 改进了 DAX 分析程序\Intellisense


**RS 项目：**
- 现在，可使用可嵌入的 RVC 控件以支持 SSRS 2016

### <a name="bug-fixes"></a>Bug 修复
**AS 项目：**
- 解决了 BI 项目的模板优先级问题，使其不在 VS 新项目类别的最上面显示
- 修复了在打开 SSIS、SSAS 或 SSRS 解决方案时，可能发生的罕见 VS 故障
- 表格：针对 DAX 分析和公式栏的多种增强和性能修复。
- 表格：如果没有打开 SSAS 表格项目，则不显示表格模型资源管理器。
- 多维：解决了高 DPI 计算机上处理对话框不可用的问题。
- 表格：解决了在已打开 SSMS 的情况下，打开任何 BI 项目将导致 SSDT 故障的问题。 [连接项](https://connect.microsoft.com/SQLServer/feedback/details/3100900/ssdt-faults-when-opening-any-bi-project-when-ssms-is-already-open)
- 表格：修复了以下问题：在 1103 模式下层次结构未正确保存到 bim 文件。[连接项](https://connect.microsoft.com/SQLServer/feedback/details/3105222/vs-2015-ssdt)
- 表格：解决了尽管不支持，但仍能在 32 位计算机上允许集成工作区模式的问题。
- 表格：解决了在准选择模式下单击任何内容（例如键入 DAX 表达式，但单击一个度量值）可能导致崩溃的问题。
- 表格：解决了部署向导将模型的 .Name 属性重置为“模型”的问题。 [连接项](https://connect.microsoft.com/SQLServer/feedback/details/3107018/ssas-deployment-wizard-resets-modelname-to-model)
- 表格：修复了在 TME 中选择层次结构时，即使未选择关系图视图也应显示属性的问题。
- 表格：解决了从某些应用程序中粘贴时，粘贴到 DAX 公式栏的操作将粘贴图像或其他内容（而非文本）的问题。
- 表格：解决了由于存在具有特定定义的度量值，无法打开 1103 中某些旧模型的问题。
- 表格：解决了无法删除 XEvent 会话的问题。
- 解决了无法使用 devenv.com 生成 AS“smproj”文件的问题
- 解决了在 AS 表格模型工作表标签标题中使用朝鲜语输入法时，文本更改过于频繁的问题
- 解决了 DAX Related() 函数的 IntelliSense 无法正常显示其他表中列的问题
- 通过对 AS 数据库的列表进行排序，改进了从数据库对话框执行的 AS 表格项目导入操作
- 解决了在 AS 表格模型中创建计算表时，表达式中没有将表列为建议对象的问题
- 解决了在查看代码后尝试使用集成工作区服务器打开预览 1400 AS 模型时遇到的问题
- 解决了在某些情况下一些数据源（不支持初始目录）无法正常运行的问题 
- 即使启用了保留分区的选项，部署向导也应向计算表分区应用更改
- 解决了只有在重新选择现有 AS 连接的“高级属性”对话框后，才能看到完整列表的问题
- 修复了某些本地化版本中显示截断的用户界面字符串的一些问题
- 修复了 1400 兼容级别 AS 表格模型中 PowerQuery 集成的大量相关问题
- 修复了报表向导样式模板未正确显示的问题
- 修复了从 SQL 更改为 AS 时报表向导可能导致数据源设置错误的问题
- 修复了导致 Analysis Services（表格）项目在命令行 (devenv.com\exe) 中生成失败的问题
- 修复了 DAX 度量值分析器的问题：在 := 前以字母开头时，其显示突出显示的内容和正确的文本颜色
- 修复了此问题：如果路径过长，尝试在集成工作区模式下对表格项目选择“显示所有文件”时，将触发 ObjectRefException
- 修复了 Compact 4.0 客户端数据提供程序的数据源设计器不可用的问题
- 修复了在 VS2017 中尝试浏览 AS 挖掘模型时导致出错的问题
- 修复了 VS2017 中 AS 多维模型的相关问题：更改视图后，DSV 图示阻止呈现并引发异常
- 修复了在 VS2017 中预览报表时 AS 连接失败的问题
 

**RS 项目：**
- 修复了以下问题：在 SSDT 中设计报表时，参数、数据源和数据集的树状视图会在大多数更改发生时折叠 
- 解决了“保存”应该保存 RDL 的版本而非最新版本的问题。
- 解决了当备份已关闭时，SSDT RS 备份文件的问题以及一些其他问题。
- 解决了在报表生成器中单击“拆分单元格”时将显示错误的问题。 [连接项](https://connect.microsoft.com/SQLServer/feedback/details/3101818/ssdt-2015-ssrs-designer-error-by-matrix-cell-split)
- 解决了缓存可能导致报表数据不正确的问题。 [连接项](https://connect.microsoft.com/SQLServer/feedback/details/3102158/ssdtbi-14-0-60812-report-preview-data-is-frequently-wrong-due-to-bad-caching)

**IS 项目：**
- 解决了 run64bitruntime 设置不粘滞的问题。
- 解决了 DataViewer 不保存所显示的列的问题。
- 解决了包部件隐藏注释的问题。 [连接项](https://connect.microsoft.com/SQLServer/feedback/details/3106624/package-parts-hide-annotations)
- 修复了以下问题：包部件放弃数据流布局和注释。 [连接项](https://connect.microsoft.com/SQLServer/feedback/details/3109241/package-parts-discard-data-flow-layouts-and-annotations)
- 解决了从 SQL 服务器导入项目时 SSDT 崩溃的问题。
- 修复了 Hadoop 文件系统任务 TimeoutInMinutes 的问题：打开已保存的 SSIS 包后和在运行时，其默认值为 10。

**数据库项目：**
- SSDT DACPAC 重新部署并添加 IgnoreColumnOrder 设置 [连接项](https://connect.microsoft.com/SQLServer/feedback/details/1221587/ssdt-dacpac-deploy-add-setting-back-in-for-ignorecolumnorder)
- SSDT 在使用 STRING_SPLIT 的情况下会编译失败 [连接项](https://connect.microsoft.com/SQLServer/feedback/details/2906200/ssdt-failing-to-compile-if-string-split-is-used)
- 修复了以下问题：DeploymentContributors 有权访问公共模型，但支持架构尚未初始化。[GitHub 问题](https://github.com/Microsoft/DACExtensions/issues/8)
- FILEGROUP 位置的 DacFx 临时修补程序
- 修复了外部同义词的“未解析的引用”错误。 
- Always Encrypted：联机加密无法禁用对取消项进行更改跟踪，并且如果在开始加密前尚未清除更改跟踪，联机加密也无法正常运行

## <a name="165nbsp-ssdt-for-vs-2015"></a>16.5，SSDT&nbsp;for VS 2015

发布时间：  2016&nbsp;年 10 月 20 日  
生成号：  14.0.61021.0&nbsp;  
SSDT for Visual Studio 2015。   
最高支持 SQL Server 2016。 

**新增功能**

### <a name="connection-improvements"></a>连接改进

* “浏览”选项卡中的新搜索框可帮助你筛选本地服务器、网络服务器和 Azure SQL 数据库。  如果这些列表中显示了大量的服务器或数据库，此功能将十分有用。
* “历史记录”选项卡中包含右键菜单选项用于固定/取消固定收藏项目，并提供一个新选项用于从历史记录中删除连接。 

### <a name="sqlpackage-and-dacfx-api-improvements"></a>SqlPackage 和 DacFx API 改进

现在，使用 SqlPackage.exe 和 DacFx API 可以通过一项操作来生成部署报告和部署脚本，以及发布到数据库。 对于想要保留有关部署期间发布内容的报告的任何人而言，这项改进可以节省时间。 另一项优势是，在 Azure 方案中，将会针对 master 数据库和部署目标数据库创建单独的脚本。 目前只会创建单个脚本，这对于重复部署不会起到作用。

针对 SqlPackage 的发布和脚本操作，已添加两个新参数。

* DeployScriptPath（短名称：dsp）。 这是要将部署脚本写入到的可选路径。 对于 Azure 部署，如果使用 TSQL 命令创建或修改了数据库，则会将主脚本写入相同的路径，但使用“Filename_Master.sql”作为输出文件名。
* DeployReportPath（短名称：drp）。 这是要将部署报告写入到的可选路径。

对于脚本操作，应使用现有输出路径参数或新的脚本/报表专用参数，但不能两者同时使用。

示例用法：

**发布操作**

```Sqlpackage.exe /a:Publish /tsn:(localdb)\ProjectsV13 /tdn:MyDatabase /deployscriptpath:"My\DeployScript.sql" /deployreportpath:"My\DeployReport.xml"```

**脚本操作**

```Sqlpackage.exe /a:Script /tsn:(localdb)\ProjectsV13 /tdn:MyDatabase /deployscriptpath:"My\DeployScript.sql" /deployreportpath:"My\DeployReport.xml"```

DacFx 中添加了两个新 API：DacServices.Publish() 和 DacServices.Script()。 这些 API 还支持通过单个操作执行发布 + 脚本 + 报告操作。 示例用法：

```
DacServices service = new DacServices(connectionString);
using(DacPackage package = DacPackage.Load(@"C:\My\db.dacpac")) {
var options = new PublishOptions() {
    GenerateDeploymentScript = true, // Should a deployment script be created?
    GenerateDeploymentReport = true, // Should an xml deploy report be created?
    DatabaseScriptPath = @"C:\My\OutputScript.sql", // optional path to save script to
    MasterDbScriptPath = @"C:\My\OutputScript_Master.sql", // optional path to save master script to
    DeployOptions = new DacDeployOptions()
};

// Call publish and receive deployment script & report in the results
PublishResult result = service.Publish(package, "TargetDb", options);
Console.WriteLine(result.DatabaseScript);
Console.WriteLine(result.MasterDbScript);
Console.WriteLine(result.DeploymentReport);

// Call script and receive deployment script & report in results
result = service.Script(package, "TargetDb", options);
Console.WriteLine(result.DatabaseScript);
Console.WriteLine(result.MasterDbScript);
Console.WriteLine(result.DeploymentReport);
```

**Analysis Services 和 Reporting Services**

在处理较大的 DAX 表达式时，SSAS 表格设计器 DAX 分析器的性能已得到改进。
有关详细信息，请阅读 [Analysis Services 博客文章](/archive/blogs/analysisservices/introducing-integrated-workspace-mode-for-sql-server-data-tools-for-analysis-services-tabular-projects-ssdt-tabular)。

### <a name="fixed--improved-this-month"></a>本月修复/改进的功能

**Database Tools**

* [连接 bug 3055711](https://connect.microsoft.com/SQLServer/feedback/details/3055711/columns-can't-be-selected-from-cross-apply-openjson-with-explicit-schema) - 无法从使用显式架构的 CROSS APPLY OPENJSON 中选择列
* 修复了自动生成的历史记录表索引的以下问题：DacFx 在重新部署时删除索引
* 已修复 - DacFx 批处理分析器无法分析转义的括号“]”字符，导致发布失败的问题
* 已改进 - 现在，SqlPackage 将在帮助输出中包含每个操作的说明
* 已修复 - 编辑“高级”选项以及编辑发布、架构比较和其他文件中保存的连接字符串时，不会保留连接对话框中的“记住密码”选项
* 已修复 - 对于“历史记录”选项卡中显示的、IntegratedAuthentication = true 的连接，连接属性中的“身份验证”字段保留空白。 现在会按预期显示“Windows 身份验证”
* 已修复 - 不会保留对“工具”->“选项”->“文本编辑器”下的“SQL Server Tools Intellisense”设置所做的更改
* 已改进 - 连接对话框中“历史记录”选项卡上的“固定”/“取消固定”按钮现在更紧凑，减少了出现滚动条的可能性
* 已修复 - 已修复连接对话框中的多个辅助功能问题。

**Analysis Services 和 Reporting Services**

* 修复了 SSDT AS 表格设计器中的一个问题：在某些情况下，单击数据网格中的滚动条 Thumb 控件会导致崩溃
* 修复了 SSDT AS 表格设计器中未提供以当前用户模拟连接的选项问题
* 修复了以下 SSDT AS 表格设计器问题：过度展开编辑栏可能会导致项目无法重新打开
* 修复了在选择表选项卡后，按键时发生 SSDT AS 表格设计器崩溃的问题
* 修复了 SSDT AS 项目中的一个问题：选择“在 Excel 中分析”不会连接到下层 AS 服务器版本

**集成服务**

* 修复了连接 bug [1608896](https://connect.microsoft.com/SQLServer/feedback/details/1608896/move-multiple-integration-service-package-tasks)：移动多个集成服务包任务

## <a name="164-ssdt-for-vs-2015"></a>16.4，SSDT for VS 2015

发布时间：  2016&nbsp;年 9 月 20 日  
生成号：  14.0.60918&nbsp;  
适用于 SQL Server 2016。 

**新增功能**

现在，SqlPackage.exe 和数据层应用程序框架 (DacFx) API 支持架构比较。 有关详细信息，请参阅 [Schema Compare in SqlPackage and the Data-Tier Application Framework](/archive/blogs/ssdt/schema-compare-in-sqlpackage-and-the-data-tier-application-framework-dacfx)（SqlPackage 和数据层应用程序框架中的架构比较）。

**Analysis Services - 适用于 SSDT Tabular 的集成工作区模式 (SSAS)**

SSDT Tabular 现在包含内部 SSAS 实例，如果启用集成工作区模式，SSDT Tabular 将在后台自动启动该实例，使你能够在模型设计器中添加和查看表、列与数据，而无需提供外部工作区服务器实例。 集成工作区模式不会更改 SSDT 表格与工作区服务器和数据库配合工作的方式。 更改的是 SSDT 表格托管工作区数据库的位置。 若要启用集成工作区模式，请在创建新表格项目时显示的“表格模型设计器”对话框中选择“集成工作区”选项。 对于当前使用显式工作区服务器的现有表格项目，可以通过在“属性”窗口（在解决方案资源管理器中选择 Model.bim 文件时会显示该窗口）中，将“集成工作区模式”参数设置为 True 切换到集成工作区模式。 有关详细信息，请参阅 [Analysis Services 博客文章](/archive/blogs/analysisservices/introducing-integrated-workspace-mode-for-sql-server-data-tools-for-analysis-services-tabular-projects-ssdt-tabular)。

**更新和修复**
**数据库工具：**

- [连接问题 3087775](https://connect.microsoft.com/SQLServer/feedback/details/3087775)：针对 VS Data Tools 中临时表损坏的 7 月更新 14.0.60629.0：“值不能为 null。 参数名称: reportedElement”
- [连接问题 1026648](https://connect.microsoft.com/SQLServer/feedback/details/1026648)：在 SSDT 比较结果中 IsPersistedNullable 显示为有差异
- [连接问题 2054735](https://connect.microsoft.com/SQLServer/feedback/details/2054735)：导入 BACPAC 时标识重置
- [连接问题 2900167](https://connect.microsoft.com/SQLServer/feedback/details/2900167)：运行 SSDT 单元测试会留下临时文件
- [连接问题 1807712](https://connect.microsoft.com/SQLServer/feedback/details/1807712)：破坏向后兼容性 - AppLocal 和 Nugetization

**Analysis Services 和 Reporting Services**

* 修复了 SSDT 中的一个问题：编辑 DirectQuery 的 DAX 计算列时错误提示弹出窗口会妨碍操作。
* 修复了 SSDT AS 表格网格中的一个问题：当 Windows 缩放系数设置为高 DPI 200% 以上时，KPI 图标不会显示在度量网格中。
* 修复了 SSDT AS 中的一个问题：粘贴大型表数据可能导致表格项目无响应。
* 修复了 SSDT AS 表格模型编辑器问题：现在，在重命名连接的易记名称时，会将模型标记为需要保存更改。
* 修复了 SSDT AS 表格项目中的一个问题：无法调整管理关系对话框中的列宽。
* 修复了 SSDT AS 表格 1200 级模型中的一个问题：使用“德语”等区域设置时，从 Excel 粘贴数据无法正确将逗号视为小数分隔符。
* 修复了以下 SSDT AS 项目问题：作为包含一些 KPI 图标集的项目，可能会生成错误“无法检索此视觉对象的数据”。
* 修复了 SSDT AS 项目属性对话框中的一个问题。现在，在以高 DPI 缩放比例调整大小时，可以正确定位。
* 修复了 SSDT AS 项目中一个可能会导致升级包含粘贴表的某些模型时出错的问题。
* 修复了以下 SSDT AS 问题：从 Excel 粘贴所有工作表行时速度非常缓慢，并且会创建许多不需要的列。
* 修复了以下 SSDT AS 问题：大型静态 DataTable 表达式的分析和突出显示速度非常缓慢，似乎已停止响应。
* 修复了 SSDT AS 中的一个问题，现在会将度量值和 KPI 值添加到在编辑器中选择的当前透视图。
* 修复了 SSDT 中的一个问题：从 SQL Azure 导入 AS 项目的数据不支持除“dbo”以外的架构类型。

## <a name="163nbsp-ssdt-for-vs-2015"></a>16.3，SSDT&nbsp;for VS 2015

发布时间：  2016&nbsp;年 8 月 15 日  
生成号：  14.0.60812.0&nbsp;  
适用于 SQL Server 2016。 

**新增功能**

- **发行版本控制和编号：** 现在，发行版按编号顺序而不是按月份进行标记。 这符合新的 SSMS 策略；当某个月份推出多个版本或修补程序时，这种版本控制方式可以简化我们的工作。 此发行版为 16.3，表示自 RTM 发行版推出之后的第三次更新。 任何修补程序的版本从 16.3.1 开始递增，下一次更新（计划在下个月推出）的版本为 16.4。
- **Analysis Services - 表格模型资源管理器：** 使用表格模型资源管理器可以方便地浏览模型中的各种元数据对象，例如数据源、表、度量值和关系。 它是以单独的工具窗口实现的，在 Visual Studio 中打开“视图”菜单，指向“其他窗口”，然后单击“表格模型资源管理器”即可显示。 表格模型资源管理器默认显示在单独选项卡上的“解决方案资源管理器”区域中。表格模型资源管理器在树结构中组织元数据对象，该结构和 1200 表格模型的架构很相似，并提供其他许多新功能。
- **数据库工具 - Always Encrypted**：此发行版提供新的 [Always Encrypted 密钥管理](../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) 对话框，方便你在数据库项目中添加列主密钥或列加密密钥，或者在 SQL Server 对象资源管理器中添加实时数据库。 此发行版支持 Windows 证书存储中的证书。 以后的发行版将支持 Azure Key Vault 和 CNG 提供程序。
    - 创建列主密钥或列加密密钥时，单击“更新数据库”后，更改可能不会立即显示在 SQL Server 对象资源管理器中。 若要解决此问题，请在 SQL Server 对象资源管理器中刷新数据库节点。
    - 如果尝试加密某个表中包含 SQL Server 对象资源管理器中数据的列，该操作可能会失败。 此功能目前仅在 SSDT 数据库项目和 SSMS 中受支持。 以后的发行版将会实现对 SQL Server 对象资源管理器的支持。

**更新和修复**
* **数据库工具：**
    - **SSDT：**
        - 连接 bug 1898001 [修复了列说明存在的 128 字符限制的问题](https://connect.microsoft.com/SQLServer/feedback/details/1898001/column-description-limited-to-128-characters)。
        - 修复了以下问题：从 VS 发布数据库不会应用发布配置文件 xml 中的 DatabaseServiceObjective 属性。
        - 连接 bug 2900167 [修复了运行单元测试时错误地留下临时文件的问题](https://connect.microsoft.com/SQLServer/feedback/details/2900167/running-ssdt-unit-tests-leaves-temp-files-behind)。
        - 修复了以下问题：“数据库设置”中的“保留期”组合框被截断。
        - 修复了更改密码时，不会验证 SQL CLR 项目属性中的空旧密码的问题。
    - **DACFx：**
        - 修复了以下问题：由于发生 SqlAzureV12 错误，DACFx 兼容级别未更新。
        - 修复了以下问题：从模型比较中错误地排除 IsAutoGeneratedHistoryTable 属性。

- **Analysis Services 和 Reporting Services**
    - **SSDT：**
        - 修复了服务器连接断开时无法保存表格模型的问题。
        - 修复了以下问题：由于 AS 中可能存在无限循环问题，SSDT 无响应。
        - 修复了以下 DAX 表达式问题：行为因表达式的提交方式不同而不一致。
        - 修复了创建 KPI 时 VS 崩溃的问题。
        - 修复了针对 SQL Server 2008 R2、2012 和 2014 生成无效报告的问题。
        - 修复了以下层次结构顺序问题：.dwpro 项目发生无限循环错误。
        - 修复了以下 RS RDL 问题：降级 RDL 要求完全重新生成，令用户感到迷惑。
        - 修复了一个 KPI 问题：“从客户端工具中隐藏”不起作用。

## <a name="july-2016nbsp-ssdt-for-vs-2015"></a>2016 年 7 月，SSDT&nbsp;for VS 2015

发布时间：  2016&nbsp;年 6 月 30 日  
生成号：  14.0.60629.0&nbsp;  
适用于 SQL Server 2016。 

**新增功能**  
- **Always Encrypted 支持：** 对于包含 Always Encrypted 列的数据库，此发行版通过核心 API 和命令行工具 (SqlPackage.exe) 添加了对 Always Encrypted 的完全支持。 你可以生成并发布完全支持所有 Always Encrypted 功能的数据库项目。  
- **临时表增强支持：** 可以在修改之前取消链接临时表，在完成修改后重新链接临时表，因此简化了体验。 这意味着，在受支持的操作方面，临时表的功能与其他表类型（标准表、内存中表）完全相同。 
- **SqlPackage.exe 和安装更改：** 从 SQL Server 引擎中隔离 SSDT 的方式发生更改，SSMS 有了更新。 有关详细信息，请参阅 [Changes to SSDT and SqlPackage.exe installation and updates](/archive/blogs/ssdt/changes-to-ssdt-and-sqlpackage-exe-installation-and-updates)（SSDT 和 SqlPackage.exe 安装与更新的更改）。


**更新和修复**
* **数据库工具：**
    * 从现在起，SSDT 永远不会在数据库中禁用透明数据加密 (TDE)。 以前，由于项目数据库设置中的默认加密选项已禁用，因此会关闭加密。 使用此修复程序可以启用加密，但发布期间永远不会禁用加密。 
    * 增大了初始连接期间的重试计数和 Azure SQL 数据库连接的复原能力。
    * 如果默认文件组不是 PRIMARY，导入/发布到 Azure V12 将会失败。 现在，在发布时将忽略此设置。
    * 修复了以下问题：导出包含带引号标识符对象的数据库时，在某些情况下导出验证可能会失败。
    * 修复了以下问题：创建 Hekaton 表时错误地添加 TEXTIMAGE_ON 选项，这是不允许的。
    * 修复了以下问题：导出大量数据时，导出需要花费很长时间，原因是数据阶段完成后写入 model.xml 文件导致重写 .bacpac 文件的内容。
    * 修复了以下问题：用户未显示在 Azure SQL 数据仓库和 APS 连接的“安全”文件夹中。

* **Analysis Services 和 Reporting Services：**
    * 修复了 MSOLAP OLEDB 提供程序的一个 SxS 问题：只安装 32 位提供程序，从而影响 64 位 Excel 2016 连接到 SQL Server 2014（在 Office365 中执行 ClickOnce 安装不会出现此问题，只有执行 MSI Excel 安装时才出现）。
    * 修复了将包含粘贴表的 AS 模型从 1103 升级到 1200 兼容级别时，可能出现错误“关系使用无效的列 ID”的问题，使该极端情况变得更可靠。
    * 修复了一个 SxS 问题：卸载 SSDT 2015（装入共享注册表设置）后，同一台计算机上的 SSDT BI 2013 不再能够导入 AS 模型中的数据。
    * 改进了可靠性，解决了当与 AS 引擎的连接断开（例如，SSDT 保持打开一整夜、AS 服务器被回收，或者暂时断开连接的其他情况）时发生的问题/崩溃。 
    * 修复了在多监视器方案中，对话框在非 VS 的屏幕上打开的问题。 
    * 已修复/已启用从 AS 模型粘贴表中的 HTML 表（网格数据）粘贴的支持。 
    * 修复了无法将空白粘贴表升级到 1200（只用作用于度量的容器表）的问题。 
    * 修复了有关以下操作的问题：将包含粘贴表的 AS 表格模型升级到 1200 来解决 CalcTables（用于 1200 中的粘贴表）的 AS 引擎问题，以便在升级后针对新的计算表执行完整过程。 
    * 修复了以下问题：取消创建包含不完整 DAX 表达式的新 AS 1200 模型计算表可能会崩溃。 
    * 修复了当数据库名称与表名称相同时，将 1200 模型从服务器导入 SSDT AS 项目时出现的问题。 
    * 修复了在 1103 表格模型中编辑 KPI 度量值时出现的问题。 
    * 修复了在网格中为 AS 1200 模型粘贴 KPI 度量值时，对象引用未设置异常命中的问题。 
    * 修复了以下问题：无法从 1200 模型的图示视图中删除计算表中的列。 
    * 修复了在代码视图中查看 model.bim 项目文件属性时，对象引用未设置异常命中的问题。 
    * 修复了以下问题：在使用逗号作为小数分隔符的国际区域设置中，将数据粘贴到 AS 模型网格以创建粘贴表时，会生成错误的值。 
    * 修复了在 SSDT 中打开 2008 RS 项目并选择不升级该项目时出现的问题。 
    * 修复了为列类型使用默认格式，以便能够从 UI 更改格式类型时，1200 兼容级别模型计算表中存在的问题。

## <a name="june-2016nbsp-ssdt-for-vs-2015"></a>2016 年 6 月，SSDT&nbsp;for VS 2015

发布时间：  2016&nbsp;年 6 月 1 日  
生成号：  14.0.60525.0&nbsp;  
适用于 SQL Server 2016。 

SSDT 正式版 (GA) 现已发布。 2016 年 6 月 SSDT GA 更新添加了对 SQL Server 2016 RTM 最新更新的支持，并修复了多个 bug。 有关详细信息，请参阅 [SQL Server Data Tools GA update for June 2016](/archive/blogs/ssdt/sql-server-data-tools-ga-update-for-june-2016)（SQL Server Data Tools GA 2016 年 6 月更新）。

## <a name="additional-resources"></a>其他资源

- [下载 SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [以前版本的 SQL Server Data Tools（SSDT 和 SSDT-BI）](../ssdt/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi.md)
- [数据库引擎中的新增功能](../sql-server/what-s-new-in-sql-server-2016.md)
- [Analysis Services 中的新增功能](/analysis-services/what-s-new-in-analysis-services)
- [Integration Services 中的新增功能](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md)