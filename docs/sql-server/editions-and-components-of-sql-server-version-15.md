---
description: " 的版本和受支持的功能[!INCLUDE[sssqlv15-md](../includes/sssqlv15-md.md)]"
title: SQL Server 2019 的各个版本和支持的功能 | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- 32-bit vs. 64-bit editions [SQL Server]
- default components
- Workgroup Edition [SQL Server]
- Internet servers [SQL Server]
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- client/server applications [SQL Server]
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- 64-bit edition [SQL Server]
- IIS [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: e0a8f226602ab41422715368fb12c13809fa6b40
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477148"
---
# <a name="editions-and-supported-features-of-sssqlv15-md"></a> 的版本和受支持的功能[!INCLUDE[sssqlv15-md](../includes/sssqlv15-md.md)]

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx](../includes/applies-to-version/sqlserver.md)]

本主题详细介绍了 [!INCLUDE[sssqlv15-md](../includes/sssqlv15-md.md)] 的不同版本支持的功能。

有关以前版本的信息，请参阅：

* [SQL Server 2017](editions-and-components-of-sql-server-2017.md)
* [SQL Server 2016](editions-and-components-of-sql-server-2016.md)

根据应用程序的需要，安装要求会有所不同。 不同版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 能够满足单位和个人独特的性能、运行时以及价格要求。 安装哪些 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 组件还取决于您的具体需要。 下面各节将帮助您了解如何在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的不同版本和可用组件中做出最佳选择。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Evaluation Edition 有 180 天的试用期。

有关最新的发布说明和新增功能的信息，请参阅以下内容：

* [[!INCLUDE[sssqlv15-md](../includes/sssqlv15-md.md)] 发行说明](../sql-server/sql-server-version-15-release-notes.md)
* [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2019 中的新变化](../sql-server/what-s-new-in-sql-server-ver15.md)

**试用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]！[从评估中心下载 [!INCLUDE[sssqlv15-md](../includes/sssqlv15-md.md)]](https://www.microsoft.com//evalcenter/evaluate-sql-server)**

## <a name="ssnoversion-editions"></a>[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] 版本

下表介绍 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的各个版本。

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本|定义|
|---------------------------------------|----------------|
|Enterprise|作为高级产品/服务，[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition 提供了全面的高端数据中心功能，具有极高的性能和无限虚拟化<sup>1</sup>，还具有端到端商业智能，可以为任务关键工作负载和最终用户访问数据见解提供高服务级别。|
|Standard|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard Edition 提供了基本数据管理和商业智能数据库，供部门和小型组织运行其应用程序，并支持将常用开发工具用于本地和云，有助于以最少的 IT 资源进行有效的数据库管理。|
|Web|对于 Web 主机托管服务提供商和 Web VAP 而言，[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web 版本是一项总拥有成本较低的选择，它可针对从小规模到大规模 Web 资产等内容提供可伸缩性、经济性和可管理性能力。|
|开发人员|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer 版支持开发人员基于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]构建任意类型的应用程序。 它包括 Enterprise 版的所有功能，但有许可限制，只能用作开发和测试系统，而不能用作生产服务器。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer 是构建和测试应用程序的人员的理想之选。|
|Express 版本|Express 版本是入门级的免费数据库，是学习和构建桌面及小型服务器数据驱动应用程序的理想选择。 它是独立软件供应商、开发人员和热衷于构建客户端应用程序的人员的最佳选择。 如果您需要使用更高级的数据库功能，则可以将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express 无缝升级到其他更高端的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]版本。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB 是 Express 的一种轻型版本，该版本具备所有可编程性功能，在用户模式下运行，并且具有快速的零配置安装和必备组件要求较少的特点。|

<sup>1</sup> Enterprise Edition 上提供有面向具有[软件保障](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)的客户的无限虚拟化。 部署必须遵守许可指南。 有关详细信息，请参阅“定价和许可”页。

## <a name="using-ssnoversion-with-clientserver-applications"></a>将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 用于客户端/服务器应用程序

在运行直接连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的客户端/服务器应用程序的计算机上，只能安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]客户端组件。 如果要在数据库服务器上管理 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例，或者打算开发 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 应用程序，那么客户端组件安装也是一个不错的选择。

客户端工具选项安装以下 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 功能：向后兼容性组件、 [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]、连接组件、管理工具、软件开发包和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机丛书组件。 有关详细信息，请参阅[安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/install-sql-server.md)。

### <a name="running-with-iis"></a>使用 IIS 运行

在 Internet 服务器（如运行 Internet Information Services (IIS) 的服务器）上，通常都会安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 客户端工具。 客户端工具包括连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例的应用程序所使用的客户端连接组件。

>[!NOTE]
>尽管可以在运行 IIS 的计算机上安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例，但这种做法通常只用于仅包含一台服务器的小型网站。 大多数网站都将其中间层 IIS 系统安装在一台服务器上或服务器群集中，将数据库安装在另外一个服务器或服务器联合体上。

## <a name="deciding-among-ssnoversion-components"></a>选择 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 组件

使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 安装向导的“功能选择”页面选择安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]时要安装的组件。 默认情况下未选中树中的任何功能。

可根据下表中给出的信息确定最能满足需要的功能集合。

|服务器组件|说明|
|-----------------------|-----------------|
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 包括 [!INCLUDE[ssDE](../includes/ssde-md.md)]（用于存储、处理和保护数据的核心服务）、复制、全文搜索、管理关系数据和 XML 数据的工具（以数据分析集成和用于访问 Hadoop 与其他异类数据源的 Polybase 集成的方式）以及使用关系数据运行 Python 和 R 脚本的机器学习服务。|
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] 包括一些工具，可用于创建和管理联机分析处理 (OLAP) 以及数据挖掘应用程序。|
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 包括用于创建、管理和部署表格报表、矩阵报表、图形报表以及自由格式报表的服务器和客户端组件。 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 还是一个可用于开发报表应用程序的可扩展平台。|
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 是一组图形工具和可编程对象，用于移动、复制和转换数据。 它还包括 [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] 的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)](DQS) 组件。|
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) 是针对主数据管理的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 解决方案。 可以配置 MDS 来管理任何领域（产品、客户、帐户）；MDS 中可包括层次结构、各种级别的安全性、事务、数据版本控制和业务规则，以及可用于管理数据的 [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] 。|
|机器学习服务（数据库内）|机器学习服务（数据库内）支持使用企业数据源的分布式、可缩放的机器学习解决方案。 在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016 中，支持 R 语言。 [!INCLUDE[sssqlv15-md](../includes/sssqlv15-md.md)] 支持 R 和 Python。|
|机器学习服务器（独立）|机器学习服务器（独立）支持在多个平台上部署分布式、可缩放机器学习解决方案，并可使用多个企业数据源，包括 Linux 和 Hadoop。 在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016 中，支持 R 语言。 [!INCLUDE[sssqlv15-md](../includes/sssqlv15-md.md)] 支持 R 和 Python。|

|管理工具|说明|
|----------------------|-----------------|
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) 是用于访问、配置、管理和开发 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 组件的集成环境。 借助 SSMS，所有技能级别的开发人员和管理员都能使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。 最新版 SSMS 更新 SMO，其中包括 [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md)。<br /><br/> 下载和安装 <br />[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]从[下载 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 服务、服务器协议、客户端协议和客户端别名提供基本配置管理。|
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 提供了一个图形用户界面，用于监视 [!INCLUDE[ssDE](../includes/ssde-md.md)] 实例或 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]实例。|
|[!INCLUDE[ssDE](../includes/ssde-md.md)] 优化顾问|[!INCLUDE[ssDE](../includes/ssde-md.md)] 优化顾问可以协助创建索引、索引视图和分区的最佳组合。|
|数据质量客户端|提供了一个非常简单和直观的图形用户界面，用于连接到 DQS 数据库并执行数据清理操作。 它还允许您集中监视在数据清理操作过程中执行的各项活动。|
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] 提供 IDE 以便为以下商业智能组件生成解决方案： [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]、 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]和 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]。<br /><br /> （以前称作 Business Intelligence Development Studio）。<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] 还包含“数据库项目”，为数据库开发人员提供集成环境，以便在 Visual Studio 内为任何 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 平台（包括本地和外部）执行其所有数据库设计工作。 数据库开发人员可以使用 Visual Studio 中功能增强的服务器资源管理器，轻松创建或编辑数据库对象和数据或执行查询。|
|连接组件|安装用于客户端和服务器之间通信的组件，以及用于 DB-Library、ODBC 和 OLE DB 的网络库。|

|文档|说明|
|-------------------|-----------------|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机丛书|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的核心文档。|

**Developer 和 Evaluation Edition** 有关 Developer 和 Evaluation Edition 支持的功能，请参阅下表中列出的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition 功能。

Developer Edition 继续只支持一个客户端用于 [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md)。

## <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a>规模限制

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|--------:|------:|---:|-------------------------------:|-----:|
|单个实例使用的最大计算能力 - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|操作系统支持的最大值|限制为 4 个插槽或 24 核，取二者中的较小值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|
|单个实例使用的最大计算能力 - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] 或 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|操作系统支持的最大值|限制为 4 个插槽或 24 核，取二者中的较小值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|
|每个 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例的缓冲池的最大内存|操作系统支持的最大值|128 <nobr/>GB|64 <nobr/>GB|1410 <nobr/>MB|1410<nobr/> MB|
|每个 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例的列存储段缓存的最大内存|不受限制的内存| 32 <nobr/>GB| 16 <nobr/>GB| 352 <nobr/>MB| 352 <nobr/>MB|
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 中每个数据库的最大内存优化数据大小|不受限制的内存| 32 <nobr/>GB| 16 <nobr/>GB| 352 <nobr/>MB| 352 <nobr/>MB|
|每个 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]实例利用的最大内存|操作系统支持的最大值|16 <nobr/>GB<sup>2</sup><br /><br /> 64 <nobr/>GB<sup>3</sup>|不适用|空值|不适用|
|每个 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]实例利用的最大内存|操作系统支持的最大值|64 <nobr/>GB|64 <nobr/>GB|4 <nobr/>GB|不适用|
|最大关系数据库大小|524 <nobr/> PB|524 <nobr/> PB|524 <nobr/> PB|10 <nobr/>GB|10 <nobr/>GB|

<sup>1</sup> 对于 Enterprise Edition 配合基于服务器 + 客户端访问许可证 (CAL) 的许可（对新协议不可用），每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的核心数上限为 20 个。 基于内核的服务器许可模型没有限制。 有关详细信息，请参阅[按 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Edition 划分的计算容量上限](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)。

<sup>2</sup> 表格

<sup>3</sup> MOLAP

## <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a>RDBMS 高可用性

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速|
|-------|:--------:|:------:|:-:|--------------------------------:|:-----:|
|Server Core 支持<sup>1</sup>|是|是|是|是|是|
|日志传送|是|是|是|否|否|
|数据库镜像|是|是<sup>2</sup>|是<sup>3</sup>|是<sup>3</sup>|是<sup>3</sup>|
|备份压缩|是|是|否|否|否|
|数据库快照|是|是|是|是|是|
|AlwaysOn 故障转移群集实例<sup>4</sup>|是|是|否|否|否|
|AlwaysOn 可用性组<sup>5</sup>|是|否|否|否|否|
|基本可用性组<sup>6</sup>|否|是|否|否|否|
|自动读写连接重新路由 |是|否|否|否|否|
|联机页面和文件还原|是|否|否|否|否|
|联机索引创建和重新生成|是|否|否|否|否|
|可恢复的联机索引重新生成|是|否|否|否|否|
|联机架构更改|是|否|否|否|否|
|快速恢复|是|否|否|否|否|
|加速数据库恢复|是|是|是|否|否|
|镜像备份|是|否|否|否|否|
|热插拔内存和 CPU|是|否|否|否|否|
|数据库恢复顾问|是|是|是|是|是|
|加密备份|是|是|否|否|否|
|Microsoft Azure 的混合备份（URL 的备份）|是|是|是|否|否|
|无群集可用性组<sup>5、6</sup>|是|是|否|否|否|
|用于灾难恢复的故障转移服务器<sup>7</sup>|是|是|否|否|否|
|用于实现高可用性的故障转移服务器<sup>7</sup>|是|是|否|否|否|
|用于在 Azure 中进行灾难恢复的故障转移服务器<sup>7</sup>|是|是|否|否|否|

<sup>1</sup> 若要详细了解如何在 Server Core 上安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]，请参阅[在 Server Core 上安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/install-sql-server-on-server-core.md)。

<sup>2</sup> 仅完全安全

<sup>3</sup> 仅见证

<sup>4</sup> 在 Enterprise Edition 中，节点数是操作系统支持的最大值。 Standard 版本中支持两个节点。

<sup>5</sup> Enterprise Edition 支持最多 8 个次要副本，包括 5 个同步的次要副本。

<sup>6</sup> Standard Edition 支持基本可用性组。 基本可用性组支持两个副本，一个数据库。 有关基本可用性组的详细信息，请参阅 [可用性组](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md)。

<sup>7</sup> 需要[软件保障](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)。

## <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a>RDBMS 可伸缩性和性能

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速|
|------:|:--------:|:------:|:-:|:-------------------------------:|:------:|
|列存储<sup>1</sup> <sup>2</sup>|是|是|是|是|是|
|聚集列存储索引中的大型对象二进制文件|是|是|是|是|是|
|联机非聚集列存储索引重新生成|是|否|否|否|否|
|内存中数据库：内存中 OLTP<sup>1</sup>|是|是|是|是<sup>3</sup>|是|
|内存中数据库：混合缓冲池|是|是|否|否|否|
|内存中数据库：内存优化 TempDB 元数据|是|否|否|否|否|
|内存中数据库：永久性内存支持|是|是|是|是|是|
|Stretch Database|是|是|是|是|是|
|多实例支持|50|50|50|50|50|
|表和索引分区|是|是|是|是|是|
|数据压缩|是|是|是|是|是|
|Resource Governor|是|否|否|否|否|
|已分区表并行度|是|否|否|否|否|
|多个 Filestream 容器|是|是|是|是|是|
|NUMA 感知、大型页内存和缓冲区数组分配|是|否|否|否|否|
|缓冲池扩展|是|是|否|否|否|
|I/O 资源治理|是|否|否|否|否|
|预读|是|否|否|否|否|
|高级扫描|是|否|否|否|否|
|延迟持续性|是|是|是|是|是|
|智能数据库：自动优化|是|否|否|否|否|
|智能数据库：行存储的批处理模式 <sup>1</sup>|是|否|否|否|否|
|智能数据库：行模式内存授予反馈|是|否|否|否|否|
|智能数据库：非重复近似计数|是|是|是|是|是|
|智能数据库：表变量延迟编译|是|是|是|是|是|
|智能数据库：标量 UDF 内联|是|是|是|是|是|
|批处理模式自适应联接|是|否|否|否|否|
|批处理模式内存授予反馈|是|否|否|否|否|
|多语句表值函数的交错执行|是|是|是|是|是|
|大容量插入改进|是|是|是|是|是|

<sup>1</sup> 内存中 OLTP 数据大小和列存储段缓存限制为不超过版本在[“缩放限制”](#Cross-BoxScaleLimits)部分中指定的内存量。 [批处理模式](../relational-databases/query-processing-architecture-guide.md#batch-mode-execution)操作的并行度 (DOP) 限制为 2（针对 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard Edition）和 1（针对 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web Edition 和 Express Edition）。 这是指在基于磁盘的表和内存优化表上创建的列存储索引。

<sup>2</sup> 聚合下推、字符串谓词下推和 SIMD 优化是 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition 可伸缩性增强功能。 如需了解更多详情，请参阅[列存储索引 - 新变化](../relational-databases/indexes/columnstore-indexes-what-s-new.md)。

<sup>3</sup> LocalDB 安装选项中不包括此功能。

## <a name="rdbms-security"></a><a name="RDBMSS"></a>RDBMS 安全性

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-----:|:-----------------------:|:-----:|
|行级安全性|是|是|是|是|是|
|Always Encrypted|是|是|是|是|是|
|具有安全 enclave 的 Always Encrypted|是|是|是|是|是|
|动态数据掩码|是|是|是|是|是|
|服务器审核|是|是|是|是|是|
|数据库审核|是|是|是|是|是|
|透明数据库加密|是|是|是|否|否|
|可扩展的密钥管理|是|是|否|否|否|
|用户定义的角色|是|是|是|是|是|
|包含的数据库|是|是|是|是|是|
|备份加密|是|是|否|否|否|
|数据分类和审核|是|是|是|是|是|

## <a name="replication"></a><a name="Replication"></a> Replication

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速|
|-------|:---------:|:-------:|:--:|:--------------------------------:|:------:|
|异类订阅服务器|是|是|否|否|否|
|合并复制|是|是|是<sup>1</sup>|是<sup>1</sup>|是<sup>1</sup>|
|Oracle 发布|是|否|否|否|否|
|对等事务复制|是|否|否|否|否|
|快照复制|是|是|是<sup>1</sup>|是<sup>1</sup>|是<sup>1</sup>|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 更改跟踪|是|是|是|是|是|
|事务复制|是|是|是<sup>1</sup>|是<sup>1</sup>|是<sup>1</sup>|
|事务复制到 Azure|是|是|否|否|否|
|事务复制可更新的订阅|是|是|否|否|否|

<sup>1</sup> 仅订阅者

## <a name="management-tools"></a><a name="SSMS"></a>管理工具

|Feature|Enterprise|Standard|Web|Express with Advanced Services|Express|
|-------|:--------:|:------:|:-:|:----------------------------:|:-----:|
|SQL 管理对象 (SMO)|是|是|是|是|是|
|SQL 评估 API|是|是|是|是|是|
|SQL 漏洞评估 |是|是|是|是|是|
|SQL 配置管理器|是|是|是|是|是|
|SQL CMD（命令提示工具）|是|是|是|是|是|
|Distributed Replay - 管理工具|是|是|是|是|否|
|Distributed Replay - 客户端|是|是|是|否|否|
|分布式重播 - 控制器|是<sup>1</sup>|是<sup>2</sup>|是<sup>2</sup>|否|否|
|SQL Profiler|是|是|否<sup>3</sup>|否<sup>3</sup>|否<sup>3</sup>|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理|是|是|是|否|否|
|Microsoft System Center Operations Manager 管理包|是|是|是|否|否|
|数据库优化顾问 (DTA)|是|是<sup>4</sup>|是<sup>4</sup>|否|否|

<sup>1</sup> 最多 16 个客户端

<sup>2</sup> 1 个客户端

<sup>3</sup> 可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard 和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition 探查 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web、[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express、[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express with Tools 和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express with Advanced Services。

<sup>4</sup> 仅对 Standard Edition 功能启用了优化

## <a name="rdbms-manageability"></a><a name="RDBMSM"></a>RDBMS 可管理性

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|用户实例|否|否|否|是|是|
|LocalDB|否|否|否|是|否|
|专用管理连接|是|是|是|是<sup>1</sup>|是<sup>1</sup>|
|SysPrep 支持<sup>2</sup>|是|是|是|是|是|
|PowerShell 脚本支持<sup>3</sup>|是|是|是|是|是|
|支持数据层应用程序组件操作 - 提取、部署、升级、删除|是|是|是|是|是|
|策略自动执行（检查计划和更改）|是|是|是|否|否|
|性能数据收集器|是|是|是|否|否|
|能够作为多实例管理中的托管实例注册|是|是|是|否|否|
|标准性能报表|是|是|是|否|否|
|计划指南和计划指南的计划冻结|是|是|是|否|否|
|使用 NOEXPAND 提示的索引视图的直接查询|是|是|是|是|是|
|直接查询 SQL Server Analysis Services|是|是|否|否|是|
|自动索引视图维护|是|是|是|否|否|
|分布式分区视图|是|否|否|否|否|
|并行索引操作|是|否|否|否|否|
|查询优化器自动使用索引视图|是|否|否|否|否|
|并行一致性检查|是|否|否|否|否|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具控制点|是|否|否|否|否|
|缓冲池扩展|是|是|否|否|否|
|大数据群集的主实例|是|是|否|否|否|
|兼容性认证|是|是|是|是|是|

<sup>1</sup> 使用跟踪标志

<sup>2</sup> 有关详细信息，请参阅[使用 SysPrep 安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的注意事项](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)。

<sup>3</sup> 在 Linux 上，支持来自定目标到 Linux 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的 Windows 计算机的 PowerShell 脚本。

## <a name="development-tools"></a><a name="DevTools"></a>开发工具

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Microsoft Visual Studio 集成|是|是|是|是|是|
|Intellisense（Transact-SQL 和 MDX）|是|是|是|是|是|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools (SSDT)|是|是|是|是|否|
|MDX 编辑、调试和设计工具|是|是|否|否|否|

## <a name="programmability"></a><a name="Programmability"></a> Programmability

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|基本 R 集成 <sup>1</sup>|是|是|是|是|否|
|高级 R 集成 <sup>2</sup>|是|否|否|否|否|
|基本 Python 集成|是|是|是|是|否|
|高级 Python 集成|是|否|否|否|否|
|机器学习服务器（独立）|是|否|否|否|否|
|PolyBase 计算节点|是|是<sup>3</sup>|是<sup>3</sup>|是<sup>3</sup>|是<sup>3</sup> |
|PolyBase 头节点<sup>4</sup>|是|是|否|否|否|
|JSON|是|是|是|是|是|
|查询存储|是|是|是|是|是|
|临时|是|是|是|是|是|
|公共语言运行时 (CLR) 集成|是|是|是|是|是|
|Java 语言运行时集成|是|是|是|是|是|
|本机 XML 支持|是|是|是|是|是|
|XML 索引|是|是|是|是|是|
|MERGE 和 UPSERT 功能|是|是|是|是|是|
|FILESTREAM 支持|是|是|是|是|是|
|FileTable|是|是|是|是|是|
|日期和时间数据类型|是|是|是|是|是|
|国际化支持|是|是|是|是|是|
|全文和语义搜索|是|是|是|是|否|
|查询中的语言规范|是|是|是|是|否|
|Service Broker（消息传递）|是|是|否<sup>5</sup>|否<sup>5</sup>|否<sup>5</sup>|
|Transact-SQL 端点|是|是|是|否|否|
|图形|是|是|是|是|是|
|UTF-8 支持|是|是|是|是|是|

<sup>1</sup> 基本集成仅限于 2 个核心和内存中数据集。

<sup>2</sup> 高级集成可以使用所有用于并行处理任意大小（受硬件限制）数据集的可用核心。

<sup>3</sup> 具有多个计算节点的横向扩展需要一个头节点。

<sup>4</sup> 在低于 SQL Server 2019 的版本中，PolyBase 头节点需要 Enterprise Edition。

<sup>5</sup> 仅客户端

## <a name="integration-services"></a><a name="IS"></a> Integration Services

若要了解 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 各个版本支持的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Integration Services (SSIS) 功能，请参阅 [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 各个版本支持的 Integration Services 功能](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md)。

## <a name="master-data-services"></a><a name="MDS"></a> Master Data Services

若要了解 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 各个版本支持的 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 和 Data Quality Services 功能，请参阅 [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 各个版本支持的 Master Data Services 和 Data Quality Services 功能](../master-data-services/master-data-services-and-data-quality-services-features-support.md)。

## <a name="data-warehouse"></a><a name="DW"></a>数据仓库

|Feature|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|自动生成临时和数据仓库架构|是|是|否|否|否|
|更改数据捕获|是|是|否|否|否|
|星型联接查询优化|是|否|否|否|否|
|针对已分区表和索引的并行查询处理|是|否|否|否|否|
|全局批处理集成|是|否|否|否|否|

## <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services

若要了解 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 各个版本支持的 Analysis Services 功能，请参阅 [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 各个版本支持的 Analysis Services 功能](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md)。

## <a name="reporting-services"></a><a name="SSRS"></a> Reporting Services

若要了解 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 各个版本支持的 Reporting Services 功能，请参阅 [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 各个版本支持的 Reporting Services 功能](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)。

## <a name="business-intelligence-clients"></a><a name="BIC"></a>商业智能客户端

若要了解 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 各个版本支持的商业智能客户端功能，请参阅 [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 各个版本支持的 Analysis Services 功能](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md)或 [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 各个版本支持的 Reporting Services 功能](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)。

## <a name="spatial-and-location-services"></a><a name="SLS"></a>空间和位置服务

|功能名称|Enterprise|Standard|Web|Express with<br/>Advanced Services|Express|
|-------------|:-------:|:------:|:-:|:-------------------------------:|:-----:|
|空间索引|是|是|是|是|是|
|平面和大地测量数据类型|是|是|是|是|是|
|高级空间库|是|是|是|是|是|
|导入/导出业界标准的空间数据格式|是|是|是|是|是|

## <a name="additional-database-services"></a><a name="ADS"></a>其他数据库服务

|功能名称|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 迁移助手|是|是|是|是|是|
|数据库邮件|是|是|是|否|否|

## <a name="other-components"></a><a name="Other"></a>其他组件

|功能名称|Enterprise|Standard|Web|Express with<br/>Advanced Services|快速|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|否|否|
|StreamInsight HA|StreamInsight Premium Edition|否|否|否|否|

## <a name="next-steps"></a>后续步骤

[[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的产品规格](https://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)

[[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 安装](../database-engine/install-windows/installation-for-sql-server-2016.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
