---
title: SQL Server 2017 的各版本和支持的功能 ~ Linux
description: 本文介绍 Linux 上的 SQL Server 2017 的不同版本支持的功能。 有助于你从可用的版本和组件中进行选择。
ms.date: 01/14/2020
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- default components
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: 11ac66182122cba239634cdf07f4b6683d2479cb
ms.sourcegitcommit: 2bf83972036bdbe6a039fb2d1fc7b5f9ca9589d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674165"
---
# <a name="editions-and-supported-features-of-sql-server-2017-on-linux"></a>Linux 上 SQL Server 2017 的各版本和支持的功能

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文详细介绍 Linux 上的 SQL Server 2017 的不同版本支持的功能。 有关 Windows 上 SQL Server 的版本和支持功能，请参阅 [SQL Server 2017 - Windows](../sql-server/editions-and-components-of-sql-server-2017.md)。  
  
根据应用程序的需要，安装要求会有所不同。 不同版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 能够满足单位和个人独特的性能、运行时以及价格要求。 安装哪些 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 组件还取决于您的具体需要。 下面各节将帮助您了解如何在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的不同版本和可用组件中做出最佳选择。  

有关最新的发布说明和新增功能的信息，请参阅以下内容：
- [Linux 上的 SQL Server 2017 发行说明](sql-server-linux-release-notes.md)
- [Linux 上的 SQL Server 2017 的新增功能](sql-server-linux-whats-new.md)

有关 Linux 上不可用的 SQL Server 功能的列表，请参阅[不支持的功能和服务](#Unsupported)。

### <a name="try-sql-server"></a>试用 SQL Server！    
    
[下载 SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017)

## <a name="ssnoversion-editions"></a>[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] 版本  
 下表介绍 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的各个版本。 
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本|定义|  
|---------------------------------------|----------------|  
|Enterprise|作为高级版本，[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise 版提供了全面的高端数据中心功能，性能极为快捷，可为关键任务工作负荷提供较高服务级别。|  
|Standard|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard 版提供了基本数据管理，使部门和小型组织能够顺利运行其应用程序并支持将常用开发工具用于本地和云部署，有助于以最少的 IT 资源获得高效的数据库管理。|  
|Web|对于为从小规模至大规模 Web 资产提供可伸缩性、经济性和可管理性功能的 Web 宿主和 Web VAP 来说，[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web 版本是一项总拥有成本较低的选择。|  
|开发人员|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer 版支持开发人员基于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]构建任意类型的应用程序。 它包括 Enterprise 版的所有功能，但有许可限制，只能用作开发和测试系统，而不能用作生产服务器。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer 是构建和测试应用程序的人员的理想之选。|  
|Express Edition|Express 版本是入门级的免费数据库，是学习和构建桌面及小型服务器数据驱动应用程序的理想选择。 它是独立软件供应商、开发人员和热衷于构建客户端应用程序的人员的最佳选择。 如果您需要使用更高级的数据库功能，则可以将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express 无缝升级到其他更高端的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]版本。|  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 用于客户端/服务器应用程序  

在运行直接连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的客户端/服务器应用程序的计算机上，只能安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]客户端组件。 如果要在数据库服务器上管理 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例，或者打算开发 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 应用程序，那么客户端组件安装也是一个不错的选择。  
  
## <a name="ssnoversion-components"></a>[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 组件  

Linux 上的 SQL Server 2017 支持 SQL Server 数据库引擎。 下表介绍了数据库引擎中的功能。   
  
|服务器组件|说明|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 包括 [!INCLUDE[ssDE](../includes/ssde-md.md)]（用于存储、处理和保护数据安全的核心服务）、复制、全文搜索、用于管理关系数据和 XML 数据以及数据库分析集成中的工具。|  

**Developer 版、Enterprise Core 版和 Evaluation 版**  
有关 Developer 版、Enterprise Core 版和 Evaluation 版支持的功能，请参阅下表中列出的 SQL Server Enterprise 版的功能。

开发人员版仍然仅支持一个 [SQL Server 分布式重播](../tools/distributed-replay/sql-server-distributed-replay.md)客户端。 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a>规模限制  
  
|Feature|Enterprise|Standard|Web|Express| 
|-------------|----------------|--------------|---------|------------------------|
|单个实例使用的最大计算能力 - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|操作系统支持的最大值|限制为 4 个插槽或 24 核，取二者中的较小值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值| 
|单个实例使用的最大计算能力 - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] 或 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|操作系统支持的最大值|限制为 4 个插槽或 24 核，取二者中的较小值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|
|每个 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例的缓冲池的最大内存|操作系统支持的最大值|128 GB|64 GB|1410 MB|
|每个 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]实例的列存储段缓存的最大内存|不受限制的内存| 32 GB| 16 GB| 352 MB|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 中每个数据库的最大内存优化数据大小|不受限制的内存| 32 GB| 16 GB| 352 MB|
|最大关系数据库大小|524 PB|524 PB|524 PB|10 GB|  
  
<sup>1</sup> 对于具有基于服务器 + 客户端访问许可证 (CAL) 的许可的 Enterprise 版本（不适用于新协议），每个 SQL Server 实例的内核数上限为 20。 基于内核的服务器许可模型没有限制。 有关详细信息，请参阅[按 SQL Server 版本划分的计算能力限制](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)。  
 
##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a>RDBMS 高可用性  
  
|Feature|Enterprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------|  
|日志传送|是|是|是|否|  
|备份压缩|是|是|否|否| 
|数据库快照|是|否|否|否|
|Always On 故障转移群集实例<sup>1</sup>|是|是|否|否| 
|Always On 可用性组<sup>2</sup>|是|否|否|否|
|基本可用性组<sup>3</sup>|否|是|否|否|
|最小副本提交可用性组|是|是|否|否|
|无群集的可用性组|是|是|否|否|
|联机页面和文件还原|是|否|否|否|
|联机索引|是|否|否|否|
|可恢复的联机索引重新生成|是|否|否|否|
|联机架构更改|是|否|否|否|
|快速恢复|是|否|否|否|
|镜像备份|是|否|否|否|
|热插拔内存和 CPU|是|否|否|否|
|加密备份|是|是|否|否|
|Azure 的混合备份（URL 的备份）|是|是|否|否|
  
<sup>1</sup> 在 Enterprise 版本中，节点数是操作系统支持的最大值。 Standard 版本中支持两个节点。 

<sup>2</sup> Enterprise 版本支持最多 8 个辅助副本，包括 2 个同步辅助副本。 

<sup>3</sup> Standard 版本支持基本可用性组。 基本可用性组支持两个副本，一个数据库。 有关基本可用性组的详细信息，请参阅 [可用性组](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md)。    

##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a>RDBMS 可伸缩性和性能  
  
|Feature|Enterprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------| 
|列存储 <sup>1</sup>|是|是|是|是|  
|聚集列存储索引中的大型对象二进制文件|是|是|是|是|  
|联机非聚集列存储索引重新生成|是|否|否|否|
|内存中 OLTP <sup>1</sup>|是|是|是|是|
|永久性主内存|是|是|是|是|
|表和索引分区|是|是|是|是|  
|数据压缩|是|是|是|是|
|Resource Governor|是|否|否|否|  
|已分区表并行度|是|否|否|否|
|NUMA 感知、大型页内存和缓冲区数组分配|是|否|否|否|
|IO 资源调控|是|否|否|否|  
|延迟持续性|是|是|是|是|
|自动优化|是|否|否|否|
|批处理模式自适应联接|是|否|否|否|
|批处理模式内存授予反馈|是|否|否|否|
|多语句表值函数的交错执行|是|是|是|是|
|大容量插入改进|是|是|是|是|


<sup>1</sup> 内存中 OLTP 数据大小和列存储段缓存限制为“规模限制”部分中的版本所指定的内存量。 最大并行度是有限的。 对于 Standard 版本，索引生成的进程并行度 (DOP) 限制为 2 DOP，对于 Web 和 Express 版本，索引生成的进程并行度 (DOP) 限制为 1 DOP。 这是指在基于磁盘的表和内存优化表上创建的列存储索引。

##  <a name="rdbms-security"></a><a name="RDBMSS"></a>RDBMS 安全性  
  
|Feature|Enterprise|Standard|Web|Express|
|-------------|----------------|--------------|---------|------------------------------------| 
|行级安全性|是|是|是|是|  
|Always Encrypted|是|是|是|是| 
|动态数据掩码|是|是|是|是|   
|基本审核|是|是|是|是| 
|精细审核|是|是|是|是| 
|透明数据库加密|是|否|否|否|   
|用户定义的角色|是|是|是|是| 
|包含的数据库|是|是|是|是| 
|备份加密|是|是|否|否|  

##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a>RDBMS 可管理性  
  
|Feature|Enterprise|Standard|Web|Express|   
|-------------|----------------|--------------|---------|------------------------|  
|专用管理连接|是|是|是|支持（使用跟踪标志）|   
|PowerShell 脚本支持|是|是|是|是| 
|支持数据层应用程序组件操作 - 提取、部署、升级、删除|是|是|是|是| 
|策略自动执行（检查计划和更改）|是|是|是|否|  
|性能数据收集器|是|是|是|否|
|标准性能报表|是|是|是|否|
|计划指南和计划指南的计划冻结|是|是|是|否| 
|使用 NOEXPAND 提示的索引视图的直接查询|是|是|是|是| 
|自动索引视图维护|是|是|是|否|
|分布式分区视图|是|否|否|否| 
|并行索引操作|是|否|否|否|  
|查询优化器自动使用索引视图|是|否|否|否| 
|并行一致性检查|是|否|否|否| 
|SQL Server 实用工具控制点|是|否|否|否|    

##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Feature|Enterprise|Standard|Web|Express 
|-------------|----------------|--------------|---------|------------------------|  
|JSON|是|是|是|是|   
|查询存储|是|是|是|是|   
|临时|是|是|是|是|   
|本机 XML 支持|是|是|是|是| 
|XML 索引|是|是|是|是| 
|MERGE 和 UPSERT 功能|是|是|是|是|   
|日期和时间数据类型|是|是|是|是|  
|国际化支持|是|是|是|是| 
|全文和语义搜索|是|是|是|是|
|查询中的语言规范|是|是|是|是|
|Service Broker（消息传递）|是|是|不支持（仅客户端）|不支持（仅客户端）|
|Transact-SQL 端点|是|是|是|否|
|图形|是|是|是|是|  


<sup>1</sup> 具有多个计算节点的 Scale out 需要一个头节点。

## <a name="integration-services"></a><a name="IS"></a> Integration Services

有关 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 各个版本支持的 Integration Services (SSIS) 功能的信息，请参阅 [SQL Server 各个版本支持的 Integration Services 功能](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md)。

##  <a name="spatial-and-location-services"></a><a name="SLS"></a>空间和位置服务  
  
|功能名称|Enterprise|Standard|Web|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|空间索引|是|是|是|是|   
|平面和大地测量数据类型|是|是|是|是| 
|高级空间库|是|是|是|是|   
|导入/导出业界标准的空间数据格式|是|是|是|是|   

## <a name="unsupported-features--services"></a><a name="Unsupported"></a> 不支持的功能和服务

Linux 上的 SQL Server 2017 目前不支持以下功能和服务。 随着时间的推移，对这些功能的支持将越来越多。

| 区域 | 不支持的功能或服务 |
|-----|-----|
| **数据库引擎** | 合并复制 |
| &nbsp; | Stretch DB |
| &nbsp; | PolyBase |
| &nbsp; | 具有第三方连接的分布式查询 |
| &nbsp; | 除 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 之外的数据源的链接服务器  |
| &nbsp; | 系统扩展存储过程（XP_CMDSHELL 等） |
| &nbsp; | FileTable 和文件流 |
| &nbsp; | 带有 EXTERNAL_ACCESS 或 UNSAFE 权限集的 CLR 程序集 |
| &nbsp; | 缓冲池扩展 |
| &nbsp; | 备份到 URL - 页 blob <sup>1</sup>|
| **SQL Server 代理** |  子系统：CmdExec、PowerShell、队列读取器、SSIS、SSAS、SSRS |
| &nbsp; | 警报 |
| &nbsp; | 日志读取器代理 |
| &nbsp; | 托管备份 |
| **高可用性** | 数据库镜像  |
| **安全性** | 可扩展的密钥管理 |
| &nbsp; | 链接服务器的 AD 身份验证 | 
| &nbsp; | 可用性组 (AG) 的 AD 身份验证 | 
| **服务** | SQL Server Browser |
| &nbsp; | SQL Server R Services |
| &nbsp; | StreamInsight |
| &nbsp; | Analysis Services |
| &nbsp; | Reporting Services |
| &nbsp; | “数据库引擎服务” |
| &nbsp; | Master Data Services |
  
<sup>1</sup> 备份到 URL 支持块 blob，通过[共享访问签名](../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS)实现。

## <a name="next-steps"></a>后续步骤
 [SQL Server 2019 的各版本和支持的功能 - Windows](../sql-server/editions-and-components-of-sql-server-version-15.md)  
 [SQL Server 2017 的各版本和支持的功能 - Windows](../sql-server/editions-and-components-of-sql-server-2017.md)  
 [SQL Server 2016 的各版本和支持的功能 - Windows](../sql-server/editions-and-components-of-sql-server-2016.md)  
 [安装 SQL Server](../database-engine/install-windows/install-sql-server.md)  
 [SQL Server 的产品规格](../sql-server/index.yml)