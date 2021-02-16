---
description: Attunity 推出的用于 Oracle 和 Teradata 的 Microsoft 连接器（用于 Integration Services (SSIS)）
title: Attunity 推出的用于 Oracle 和 Teradata 的 Microsoft 连接器 | Microsoft Docs
ms.date: 08/16/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: ''
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 012d95d493709ab07741134340141af66399fb53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353733"
---
# <a name="microsoft-connectors-for-oracle-and-teradata-by-attunity-for-integration-services-ssis"></a>Attunity 推出的用于 Oracle 和 Teradata 的 Microsoft 连接器（用于 Integration Services (SSIS)）

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]

> [!NOTE]
> 适用于 Oracle 和 Teradata 的 Attunity Connector 支持 SQL Server 2017 及更低版本。
>
> 自 SQL Server 2019 起，从此处获取适用于 Oracle 和 Teradata 的最新连接器：
> - [Microsoft Connector for Oracle](data-flow/oracle-connector.md)
> - [Microsoft Connector for Teradata](data-flow/teradata-connector.md)

可下载用于Attunity 提供的 Integration Services 的连接器，当在 SSIS 包中向 Oracle 或 Teradata 加载数据或从中下载数据时，该连接器可优化性能。

## <a name="download-the-latest-attunity-connectors"></a>下载最新的 Attunity 连接器

在此处获取最新版本的连接器：  
[用于 Oracle 和 Teradata 的 Microsoft 连接器 v5.0](https://www.microsoft.com/download/details.aspx?id=55179)

## <a name="issue---the-attunity-connectors-arent-visible-in-the-ssis-toolbox"></a>问题 - Attunity 连接器未显示在 SSIS 工具箱中

若要在 SSIS 工具箱中查看 Attunity 连接器，所安装的连接器版本所针对的 SQL Server 版本应与计算机上安装的 SQL Server Data Tools (SSDT) 版本相同。 （计算机上可同时安装有其他早期版本的连接器。）此要求不受 SSIS 项目和包中所面向的 SQL Server 版本的影响。

例如，如果已安装最新版本的 SSDT，则 SSDT 为 17 版，具有一个以 14 开头的内部版本号。 此版本的 SSDT 添加了对 SQL Server 2017 的支持。 若要在 SSIS 包开发中查看和使用 Attunity 连接器（即使想面向更早的 SQL Server 版本），仍须安装最新版本的 Attunity 连接器（5.0 版）。 此版本的连接器也添加了对 SQL Server 2017 的支持。

从“帮助” | “关于 Microsoft Visual Studio”或控制面板中的“程序和功能”检查 Visual Studio 中安装的 SSDT 版本  。 然后安装下表中相应的 Attunity 连接器版本。

|SSDT 版本|SSDT 内部版本号|目标 SQL Server 版本|连接器的所需版本|
|---------|---------|---------|---------|
|17|开头为 14|SQL Server 2017|[用于 Oracle 和 Teradata 的 Microsoft 连接器 v5.0](https://www.microsoft.com/download/details.aspx?id=55179)|
|16|开头为 13|SQL Server 2016|[用于 Oracle 和 Teradata 的 Microsoft 连接器 v4.0](https://www.microsoft.com/download/details.aspx?id=52950)|
||||

## <a name="download-the-latest-sql-server-data-tools-ssdt"></a>下载最新的 SQL Server Data Tools (SSDT)

在此处获取最新版的 SSDT：  
[下载 SQL Server Data Tools (SSDT)](..//ssdt/download-sql-server-data-tools-ssdt.md)
