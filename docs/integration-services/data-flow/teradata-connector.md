---
description: Microsoft Connector for Teradata
title: Microsoft Connector for Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5f82833ec5498c107155a5284d7a419ba6289239
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346541"
---
# <a name="microsoft-connector-for-teradata"></a>Microsoft Connector for Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

使用 Microsoft Connector for Teradata，可以从 SSIS 包内的 Teradata 数据源中导出数据以及向其中加载数据。

此新连接器支持包含 1 MB 表的数据库。

## <a name="version-support"></a>版本支持

Microsoft Connector for Teradata 支持以下 Microsoft SQL Server 产品：

- Microsoft SQL Server 2019
- 适用于 Visual Studio 2017 的 Microsoft SQL Server Data Tools (SSDT) 15.8.1 或更高版本
- 适用于 Visual Studio 2019 的 Microsoft SQL Server Data Tools (SSDT)

Microsoft connector for Teradata 使用 Teradata Parallel Transporter 应用程序编程语言接口将数据加载到 Teradata 数据库中以及导出其中的数据。 支持以下版本：

- Teradata Parallel Transporter (Teradata PT) 16.10
- Teradata Parallel Transporter (Teradata PT) 16.20

支持以下 Teradata 数据库版本的数据源：

- Teradata 数据库 16.20
- Teradata 数据库 16.10
- Teradata 数据库 15.10
- Teradata 数据库 15.00

有关 Teradata Parallel Transporter 应用程序编程接口程序员指南的详细信息，请查看 [Teradata 文档](https://docs.teradata.com/)。

## <a name="installation"></a>安装

在 32 位计算机上，从 [Teradata 工具和实用程序 - Windows 安装程序包](https://downloads.teradata.com/download/tools/teradata-tools-and-utilities-windows-installation-package)中安装以下驱动程序：

- Teradata ODBC 驱动程序（32 位）
- Teradata PT API（32 位）

在 64 位计算机上，安装以下驱动程序：

- Teradata ODBC 驱动程序（64 位）
- Teradata PT API（64 位）

要为 Teradata 数据库安装连接器，请从 [最新版 Microsoft Connector for Teradata](https://www.microsoft.com/download/details.aspx?id=100599) 下载并运行该安装程序。 然后按照安装向导中的说明进行操作。

安装连接器后，必须重启 SQL Server Integration Services，才能确保 Teradata 源和目标正常运行。

## <a name="design-and-execute-ssis-packages"></a>设计和执行 SSIS 包

Microsoft Connector for Teradata 与 Attunity Teradata 连接器提供类似的用户体验。 面向 SQL Server 2019，用户可以基于以前的体验设计新包，使用适用于 VS 2017 或 VS 2019 的 SSDT。

Teradata 源和目标位于“通用”类别下。

![Teradata 组件](media/teradata-component.png)

Teradata 连接管理器显示为“TERADATA”。

![Teradata 连接管理器类型](media/teradata-connection-manager-type.png)

使用 Attunity Teradata 连接器设计的现有 SSIS 包将自动升级为使用 Microsoft Connector for Teradata。 图标也会更改。

要执行面向 SQL Server 2017 及更低版本的 SSIS 包，需要从以下链接安装相应版本的 Microsoft Connector for Oracle by Attunity：

- [SQL Server 2017：Microsoft Connector for Teradata by Attunity 版本 5.0](https://www.microsoft.com/download/details.aspx?id=55179)
- [SQL Server 2016：Microsoft Connector for Teradata by Attunity 版本 4.0](https://www.microsoft.com/download/details.aspx?id=52950)
- [SQL Server 2014：Microsoft Connector for Teradata by Attunity 版本 3.0](https://www.microsoft.com/download/details.aspx?id=44582)
- [SQL Server 2012：Microsoft Connector for Teradata by Attunity 版本 2.0](https://www.microsoft.com/download/details.aspx?id=29283)

要在面向 SQL Server 2017 及更低版本的 SSDT 中设计 SSIS 包，需要拥有 Microsoft Connector for Teradata，并安装相应版本的 Microsoft Connector for Teradata by Attunity。 

## <a name="limitations-and-known-issues"></a>限制和已知问题

- Teradata 源/目标编辑器，“默认数据库”属性不生效。  在下拉框中键入数据库名称来筛选表或视图，可暂时绕过此问题。

- Teradata 源/目标编辑器，键入 \<database>.<table/view> 时映射步骤无效。 键入 \<database>.<table/view>，然后单击下拉按钮，可暂时绕过此问题。

- Teradata 源编辑器，当数据访问模式为“表名称 – TPT 导出”时，不能显示视图。 使用 Teradata 源的高级编辑器，可暂时绕过此问题。

- Teradata 目标，不能将特性“PackMaximum”设置为“True”。  否则将会出错。

## <a name="uninstallation"></a>卸载

可以运行卸载向导以删除 Microsoft Connector for Teradata。

## <a name="next-steps"></a>后续步骤

- 配置 [Teradata 连接管理器](teradata-connection-manager.md)
- 配置 [Teradata 源](teradata-source.md)
- 配置 [Teradata 目标](teradata-destination.md)
- 如有任何疑问，请访问[技术社区](https://aka.ms/AA6iwdw)。
