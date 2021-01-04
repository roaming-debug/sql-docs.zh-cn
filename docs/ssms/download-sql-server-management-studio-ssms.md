---
title: 下载 SQL Server Management Studio (SSMS)
description: 下载最新版 SQL Server Management Studio (SSMS)。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
keywords:
- 安装 SSMS，下载 SSMS，最新的 SSMS
- SQL Server Management Studio
- ssms.exe
- sql man studio
- sql management studio
- SQL Management Studio 安装
- 下载 SQL Management Studio
- MS SQL Management Studio
- 安装 SQL Management Studio
- ssms download
- sql server ssms
- ssms express
ms.assetid: adafeeef-4255-4924-8042-02f503d599ca
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/17/2020
ms.openlocfilehash: e1af0f83181e6fbd479d2727b7da893b8bc20b59
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639080"
---
# <a name="download-sql-server-management-studio-ssms"></a>下载 SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

SQL Server Management Studio (SSMS) 是一种集成环境，用于管理从 SQL Server 到 Azure SQL 数据库的任何 SQL 基础结构。 SSMS 提供用于配置、监视和管理 SQL Server 和数据库实例的工具。 使用 SSMS 部署、监视和升级应用程序使用的数据层组件，以及生成查询和脚本。

使用 SSMS 在本地计算机或云端查询、设计和管理数据库及数据仓库，无论它们位于何处。

## <a name="download-ssms"></a>下载 SSMS

:::image type="icon" source="media/download-icon.png" border="false"::: **[下载 SQL Server Management Studio (SSMS)](https://aka.ms/ssmsfullsetup)**

SSMS 18.8 是 SSMS 的最新正式发布 (GA) 版本。 如果安装的是旧 GA 版本 SSMS 18，请安装 SSMS 18.8 将它升级到 18.8。

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

- 版本号：18.8
- 生成号：15.0.18369.0
- 发行日期：2020 年 12 月 17 日

如果你有任何意见或建议，或想报告问题，最好是通过 [SQL Server 用户反馈](https://aka.ms/sqlfeedback)与 SSMS 团队联系。

SSMS 18.x 安装不会升级或替换 SSMS 17.x 或更早版本。 SSMS 18.x 与以前的版本并行安装，因此，这两个版本均可供使用。 不过，如果安装的是 SSMS 18.x 预览版，则必须在安装 SSMS 18.8 前卸载它。 转到“帮助”>“关于”  窗口，找到“预览版”，即可查看。

如果计算机包含 SSMS 的并行安装，请验证你是否针对特定需求启动相应的版本。 最新版本标记为 Microsoft SQL Server Management Studio 18

> [!Note]
> 如果你正从一个非英语的语言版本访问此页面并想要查看最新内容，请访问[此网站的英语（美国）版本](https://aka.ms/downloadssmsusenglish)。 可以通过选择[可用语言](#available-languages)从英语（美国）版本站点下载不同的语言。

## <a name="available-languages"></a>可用语言

此版本的 SSMS 可以安装在以下语言中：

SQL Server Management Studio 18.8：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40a)

> [!NOTE]
> SQL Server PowerShell 模块可通过 PowerShell 库单独安装。 有关详细信息，请参阅[下载 SQL Server PowerShell 模块](download-sql-server-ps-module.md)。

## <a name="whats-new"></a>新增功能

有关此版本中新增功能的详细信息，请参阅 [SSMS 发行说明](release-notes-ssms.md)。

此版本存在一些[已知问题](release-notes-ssms.md#known-issues-188)。

## <a name="previous-versions"></a>以前的版本

本文仅适用于最新版本的 SSMS。 若要下载 SSMS 的早期版本，请访问 [SSMS 的早期版本](../ssms/release-notes-ssms.md#previous-ssms-releases)。

[!INCLUDE[ssms-connect-azure-ad](../includes/ssms-connect-azure-ad.md)]

## <a name="unattended-install"></a>无人参与安装

还可以使用命令提示符脚本安装 SSMS。

如果要在没有 GUI 提示的情况下在后台安装 SSMS，请执行以下步骤。

1. 使用提升的权限启动命令提示符。

2. 在命令提示符下键入以下命令。

    ```console
    start "" /w <path where SSMS-ENU.exe file is located> /Quiet SSMSInstallRoot=<path where you want to install SSMS>
    ```

    示例：

    ```console
    start "" /w %systemdrive%\SSMSfrom\SSMS-Setup-ENU.exe /Quiet SSMSInstallRoot=%systemdrive%\SSMSto
    ```

    你也可以传递 /Passive 而不是 /Quiet 来查看设置 UI   。

3. 如果一切正常，你可以看到 SSMS 安装在 %systemdrive%\SSMSto\Common7\IDE\Ssms.exe 中（具体视示例而定）。 如果出现故障，可以检查返回的错误代码，并在 %TEMP%\SSMSSetup 处查看日志文件。

## <a name="installation-with-azure-data-studio"></a>使用 Azure Data Studio 安装

- 从 SSMS 18.7 开始，默认情况下，SSMS 会安装 Azure Data Studio 的系统版本。  如果与随附的 Azure Data Studio 版本相比，不低于该版本的 Azure Data Studio 稳定版或预览体验版本的系统版本已存在于工作站上，则会跳过使用 SSMS 安装 Azure Data Studio 的步骤。 可以在发行说明中找到 Azure Data Studio 版本。
- Azure Data Studio 系统安装程序需要与 SSMS 安装程序相同的安全权限。
- Azure Data Studio 安装是使用默认 Azure Data Studio 安装选项完成的。 它们用于创建“开始”菜单文件夹并将 Azure Data Studio 添加到路径。 不会创建桌面快捷方式，也不会将 Azure Data Studio 注册为任何文件类型的默认编辑器。
- Azure Data Studio 的本地化通过语言包扩展来实现。 若要本地化 Azure Data Studio，请从[扩展市场](../azure-data-studio/extensions/add-extensions.md)下载相应的语言包。
- 此时，可以通过使用命令行标记 `DoNotInstallAzureDataStudio=1` 启动 SSMS 安装程序来跳过 Azure Data Studio 的安装。

## <a name="uninstall"></a>卸载

卸载 SSMS 后，仍会保留安装一些共享组件。

保留安装的共享组件包括：

- Azure Data Studio
- Microsoft .NET Framework 4.7.2
- 适用于 SQL Server 的 Microsoft OLE DB 驱动程序
- Microsoft ODBC Driver 17 for SQL Server
- Microsoft Visual C++ 2013 Redistributable (x86)
- Microsoft Visual C++ 2017 Redistributable (x86)
- Microsoft Visual C++ 2017 Redistributable (x64)
- Microsoft Visual Studio Tools for Applications 2017

由于这些组件可能与其他产品共享，因此不会卸载。 如果卸载了这些组件，则可能会遇到其他产品被禁用的风险。

## <a name="supported-sql-offerings"></a>支持的 SQL 产品/服务

- 此版本的 SSMS 适用于所有[受支持 SQL Server 2008 版本 - [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]](https://support.microsoft.com/lifecycle?C2=1044)，并且在最大程度上支持与 Azure SQL 数据库和 Azure Synapse Analytics 中的最新云功能配合使用。
- 此外，SSMS 18.x 可与 SSMS 17.x、SSMS 16.x 或 SQL Server 2014 SSMS 及早期版本并行安装。
- SQL Server Integration Services (SSIS) - SSMS 版本 17.x 或更高版本不支持连接到旧版 SQL Server Integration Services 服务。 要连接到早期版本的 Integration Services，请使用与 SQL Server 版本一致的 SSMS 版本。 例如，使用 SSMS 16.x 连接到旧版 SQL Server 2016 Integration Services 服务。 可以在同一台计算机上并行安装 SSMS 17.x 和 SSMS 16.x。 由于 SQL Server 2012 的发布，建议使用 SSIS 目录数据库 (SSISDB) 来存储、管理、运行和监视 Integration Services 包。 有关详细信息，请参阅 [SSIS 目录](../integration-services/catalog/ssis-catalog.md)。

## <a name="ssms-system-requirements"></a>SSMS 系统要求

当与最新可用的服务包一起使用时，SSMS 的当前版本支持以下 64 位平台：

支持的操作系统：

- Windows 10（64 位）1607 (10.0.14393) 或更高版本
- Windows 8.1（64 位）
- Windows Server 2019（64 位）
- Windows Server 2016（64 位）
- Windows Server 2012 R2（64 位）
- Windows Server 2012（64 位）
- Windows Server 2008 R2（64 位）

支持的硬件：

- 1.8 GHz 或更快的 x86（Intel、AMD）处理器。 推荐使用双核或更好的内核
- 2 GB RAM；建议 4 GB RAM（如果在虚拟机上运行，则最低 2.5 GB）
- 硬盘空间：2-10 GB 可用空间

> [!NOTE]
> 对于 Windows，SSMS 只能作为 32 位应用程序使用。 如果需要在 Windows 以外的操作系统上运行的工具，我们建议使用 Azure Data Studio。 Azure Data Studio 是一个跨平台工具，可在 macOS、Linux 以及 Windows 上运行。 有关详细信息，请参阅 [Azure Data Studio](../azure-data-studio/what-is.md)。

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

## <a name="next-steps"></a>后续步骤

- [SQL 工具](../tools/overview-sql-tools.md)
- [SQL Server Management Studio 文档](sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is.md)
- [下载 SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [最新更新](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)
- [Azure 数据体系结构指南](https://docs.microsoft.com/azure/architecture/data-guide/)
- [SQL Server 博客](https://cloudblogs.microsoft.com/sqlserver/)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
