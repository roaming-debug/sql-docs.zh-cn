---
title: 安装 SQL Server Integration Services | Microsoft Docs
description: 了解如何安装 Microsoft SQL Server Integration Services (SSIS) 以及如何获取 SSIS 的其他下载
ms.custom: ''
ms.date: 09/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, installing
- SSIS, installing
- installing Integration Services, about installing Integration Services
- SQL Server Integration Services, installing
- Setup [Integration Services], about installing Integration Services
- installing Integration Services
- Setup [Integration Services]
ms.assetid: bd20fd3a-414b-4581-959d-ebba4ddf5a55
author: chugugrace
ms.author: chugu
ms.openlocfilehash: abb62aec3a712ace96d6db82ea8eb0085d8a9e74
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049268"
---
# <a name="install-integration-services-ssis"></a>安装 Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了单个安装程序来安装其包括 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 在内的任一组件或所有组件。 使用安装程序，可以在单台计算机上安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 或将其与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件一起安装。

 本文重点介绍了在安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 之前应了解的重要注意事项。 本文中的信息有助于评估安装选项，以便用户做出适当选择，使安装成功完成。

![安装 Integration Services](install-integration-services/install-integration-services-sql-setup.png)

## <a name="get-ready-to-install-integration-services"></a>准备安装 Integration Services

安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 之前，请首先查看以下信息：

- [安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)

- [安装 SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)

## <a name="install-standalone-or-side-by-side"></a>独立安装或并行安装

可以按下列配置安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]：

- 可以在没有旧 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]。

- 可并行安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的现有实例。

在已安装了 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 早期版本的计算机上升级到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的最新版本时，当前版本与该早期版本并行安装。

有关升级 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的详细信息，请参阅[升级 Integration Services](../../integration-services/install-windows/upgrade-integration-services.md)。

## <a name="get-sql-server-with-integration-services"></a>在 SQL Server 上安装 Integration Services

如果尚未安装 Microsoft SQL Server，请从 [SQL Server 下载](https://www.microsoft.com/sql-server/sql-server-downloads)中下载免费评估版，或下载免费开发人员版。 SSIS 不包含在速成版 SQL Server 中。

## <a name="install-integration-services"></a>安装集成服务

 安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 之前，请查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的安装要求并确保计算机满足这些要求。

如果使用安装向导安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，则会使用一系列页面来指定组件和选项。

- 在“功能选择”页的“共享功能”下，选择“Integration Services”    。

- 在“实例功能”下，可根据需要选择“数据库引擎服务”，以托管 SSIS 目录数据库 `SSISDB`，并存储、管理、运行和监视 SSIS 包   。

- 要安装用于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 编程的托管程序集，仍需在“共享功能”下选中“客户端工具 SDK”   。

> [!NOTE]
> 如果选择了可在安装向导的“功能选择”页上选择进行安装的某些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件，则会安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件的部分子集  。 这些组件可用于特定任务，但 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的功能会受到限制。 例如， **“数据库引擎服务”** 选项将安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 导入和导出向导所需的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件。 为确保完整安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，必须在 **“功能选择”** 页上选择“ **集成服务** ”。

### <a name="installing-a-dedicated-server-for-etl-processes"></a>为 ETL 进程安装专用服务器

若要对提取、转换和加载 (ETL) 过程使用专用服务器，请在安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 时安装 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的本地实例。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 通常将包存储在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例中，并使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理对这些包进行计划。 如果 ETL 服务器上没有 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，则必须通过具有 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的服务器计划或运行包。 这导致这些包不会在 ETL 服务器上运行，而是在其启动时所在的服务器上运行。 因此，专用 ETL 服务器的资源不会按预期方式使用。 而且，其他服务器的资源可能会受到 ETL 进程运行的影响

### <a name="configuring-ssis-event-logging"></a>配置 SSIS 事件日志记录

默认情况下，在全新安装中， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 配置为不将与运行包相关的事件记录到应用程序事件日志中。 使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]的数据收集器功能时，此设置可防止生成太多事件日志项。 未记录的事件是，EventID 12288“包已启动”和 EventID 12289“包已成功完成”。 若要将这些事件记录到应用程序事件日志中，请打开注册表以进行编辑。 然后在注册表中，找到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS 节点，并将 LogPackageExecutionToEventLog 设置的 DWORD 值从 0 更改为 1。

## <a name="install-additional-components-for-integration-services"></a>安装 Integration Services 的其他组件

要完整安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，请从以下列表中选择所需组件：

- **Integration Services (SSIS)** 。 使用 SQL Server 安装向导安装 SSIS。 选择 SSIS 会安装以下各项：

  - 对 SQL Server 数据库引擎上 SSIS 目录的支持。

  - （可选）[横向扩展功能](../scale-out/walkthrough-set-up-integration-services-scale-out.md)，包括 Master 和 Worker。

  - 32 位和 64 位 SSIS 组件。

  - 安装 SSIS 时不会安装设计和开发 SSIS 包所需的工具  。

- **SQL Server 数据库引擎**。 使用 SQL Server 安装向导安装数据库引擎。 通过选择“数据库引擎”，可创建并托管 SSIS 目录数据库 `SSISDB`，并存储、管理、运行和监视 SSIS 包。

- **SQL Server Data Tools (SSDT)** 。 要下载并安装 SSDT，请参阅[下载 SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md)。 安装 SSDT 后，可设计和部署 SSIS 包。 SSDT 安装以下各项：

  - SSIS 包设计和开发工具，包括 SSIS 设计器。

  - 仅 32 位 SSIS 组件。

  - Visual Studio 的受限制版本（如果尚未安装 Visual Studio 版本）。

  - SSIS 脚本任务和脚本组件所使用的脚本编辑器 Tools for Applications (VSTA)。

  - SSIS 向导，包括部署向导和包升级向导。

  - SQL Server 导入和导出向导。

- **SQL Server Data Tools (SSDT)** 。 我们已不再使用适用于 Visual Studio 2019 的 SSDT 独立安装程序。 对于 Visual Studio 2019，现可从 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects&ssr=false#overview) 获取 SSIS 设计器扩展。

- **用于 Azure 的 Integration Services 功能包**。 要下载并安装功能包，请参阅[用于 Azure 的 Microsoft SQL Server Integration Services 功能包](../azure-feature-pack-for-integration-services-ssis.md)。 通过安装功能包，可将包连接到 Azure 云中的存储和分析服务，包括以下服务：

  - Azure Blob 存储。

  - Azure HDInsight。

  - Azure Data Lake Store。

  - Azure Synapse Analytics。

  - Azure Data Lake Storage (Gen2).

- **其他可选组件**。 根据需要，可以从 SQL Server 功能包下载其他第三方组件。

  - 用于 Microsoft SQL Server® 的 Microsoft® Connector for SAP BW。 要获取这些组件，请参阅 [Microsoft® SQL Server® 2017 功能包](https://www.microsoft.com/download/details.aspx?id=55992)。

  - Attunity 推出的用于 Oracle 和 Teradata 的 Microsoft Connector。 若要获取这些组件，请参阅 [Attunity 连接器](../attunity-connectors.md)。

## <a name="next-steps"></a>后续步骤

- [并行安装 Integration Services 版本](installing-integration-services-versions-side-by-side.md)

- [SQL Server 2016 Integration Services 中的新增功能](../what-s-new-in-integration-services-in-sql-server-2016.md)

- [SQL Server 2017 Integration Services 中的新增功能](../what-s-new-in-integration-services-in-sql-server-2017.md)
