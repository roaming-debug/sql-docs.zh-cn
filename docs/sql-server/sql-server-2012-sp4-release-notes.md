---
title: SQL Server 2012 Service Pack 发行说明 | Microsoft Docs
description: 本文包含有关 SQL Server 2012 的四个服务包的聚合发行说明。 每个服务包都是之前服务包的累积。
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 67cb8b3e-3d82-47f4-840d-0f12a3bff565
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 83a7828c93d6863e99d88d3cce45654e8f73abbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341126"
---
# <a name="sql-server-2012-service-pack-release-notes"></a>SQL Server 2012 Service Pack 发行说明
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
本主题包含有关 SQL Server 2012 的四个服务包的聚合发行说明。 每个服务包都是之前服务包的累积。

服务包仅在线提供，安装介质中不包含，可下载为：
- [SQL Server 2012 SP4](https://go.microsoft.com/fwlink/?linkid=846937)
- [SQL Server 2012 SP3](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information)
- [SQL Server 2012 SP2](https://support.microsoft.com/KB/2958429)
- [SQL Server 2012 SP1](https://go.microsoft.com/fwlink/p/?LinkID=268158)

## <a name="service-pack-4-release-notes"></a>Service Pack 4 发行说明

### <a name="download-pages"></a>下载页面

- [SQL Server 2012 SP4 功能包](https://www.microsoft.com/download/details.aspx?id=56041)
- [SQL Server 2012 SP4 修补程序安装](https://www.microsoft.com/download/details.aspx?id=56040)
- [SQL Server 2012 SP4 Express](https://www.microsoft.com/download/details.aspx?id=56042)


### <a name="performance-and-scale-improvements"></a>性能和缩放性方面的改进
- 改进了分发代理清除过程  - 过大的分发数据库会导致发生阻塞和死锁情况。 改进的清理过程旨在消除其中某些阻塞或死锁情况的发生。 
- 动态内存对象缩放  - 基于大量的节点和内核对内存对象进行动态分区，从而缩放现代硬件。 动态升级旨在防止潜在的瓶颈并对线程安全内存对象进行自动分区。 未分区的内存对象可进行动态升级，以便按节点分区。 分区数等于 NUMA 节点数。 可进一步提升按节点分区的内存对象，使其按 CPU 分区，其中分区数等于 CPU 数。
- 为缓冲池启用 8 TB 以上的内存  - 为缓冲池的使用启用了 128-TB 的虚拟地址空间
- **更改跟踪清除** - 改进了更改跟踪端表的更改跟踪清除性能和效率。 

### <a name="supportability-and-diagnostics-improvements"></a>可支持性和诊断改进
- 对复制代理的完全转储支持  - 如果复制代理现在遇到未经处理的异常，则会默认创建异常现象的微型转储。 此默认行为要求对未经处理的异常执行复杂的故障排除步骤。 SP4 引入了一个新的注册表项，支持为复制代理创建完全转储。
- 增强了 showplan XML 中的诊断  - Showplan XML 已得到增强，公开了以下相关信息：已启用的跟踪标志、用于优化的嵌套循环联接的内存部分、CPU 时间和运行时间。 
- 加强了 XE 和 DMV 诊断之间的相关性  - 仅 Query_hash 和 query_plan_hash 字段用于标识查询。 DMV 将二者定义为 varbinary(8)，而 XEvent 将其定义为 UINT64。 由于 SQL Server 不具有“unsigned bigint”，因此强制转换并非始终有效。 这一改进引入了等效于 query_hash 和 query_plan_hash 的新 XEvent 操作/筛选器列，除非二者定义为有助于关联 XE 和 DMV 之间的查询的 INT64。 
- 增强了内存授予/使用诊断  - 新的 query_memory_grant_usage XEvent（移植自 Server 2016 SP1）
- 将协议跟踪添加到 SSL 协商步骤  - 添加了成功/失败的协商的一些跟踪信息，包括协议等。在部署 TLS 1.2（以此为例）的情况下，对连接方案进行故障排除时很有用
- 为分发数据库设置正确的兼容性级别  - 安装服务包后，分发数据库兼容级别会更改为 90。 由于 sp_vupgrade_replication 存储过程中的问题，因此对级别进行了更改。 SP 现已改为设置分发数据库的正确兼容级别。 
- 用于克隆数据库的新的 DBCC 命令  - 克隆数据库是新添加的 DBCC 命令，允许 CSS 等强大的用户通过克隆架构和元数据（不克隆数据）对现有生产数据库进行故障排除。 该调用通过 DBCC clonedatabase (‘source_database_name’, ‘clone_database_name’) 执行。 克隆的数据库不应用于生产环境。 若要查看在克隆数据库的调用过程中是否已生成数据库，可以使用以下命令：select DATABASEPROPERTYEX('clonedb', 'isClone')。返回值 1 表示 true，0 表示 false。 
- SQL 错误日志中的 TempDB 文件和文件大小信息  - 如果在启动期间，TempDB 数据文件的大小和自动增长不同，则输出文件数并触发警告。
- IFI 支持 SQL Server 错误日志中的消息  - 在错误日志中指示数据库即时文件初始化处于启用/禁用状态
- 替换 DBCC INPUTBUFFER 的新 DMF  - 已引入将 session_id 作为参数的新动态管理函数 sys.dm_input_buffer，用于替换 DBCC INPUTBUFFER
- 针对可用性组的读取路由问题的 Xevent 增强功能  - 当前，如果存在路由列表，但路由列表中的任何服务器都不可用于连接，则只会激发 read_only_rout_fail XEvent。 这一改进包括用于帮助进行故障排除的其他信息，还会在用于激发 XEvent 的码位中展开。 
- 通过可用性组故障转移，改进了 Service Broker 的处理  - 当前，如果在可用性组故障转移上启用了 Service Broker，则 AG 故障转移期间，主要副本发起的所有 Service Broker 连接均保持打开状态。 AG 故障转移期间，这一改进会关闭所有此类打开的连接。
- **自动 Soft-NUMA 分区** - 在服务级别启动了跟踪标志 8079 后，会借助 SQL 2014 SP2 引入自动 [Soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md) 技术。 如果在启动期间启用了跟踪标志 8079，SQL Server 2014 SP2 会询问硬件布局，并在报告每个 NUMA 节点 8 个或多个 CPU 的系统上自动配置 Soft NUMA。 自动 Soft NUMA 行为是超线程（HT/逻辑处理器）感知。 通过提高侦听器数、缩放和网络与加密功能，其他节点的分区和创建会缩放后台处理。 建议首先测试自动 Soft NUMA 的工作负荷性能，然后再在生产中启用该技术。

## <a name="service-pack-3-release-notes"></a>Service Pack 3 发行说明

### <a name="download-pages"></a>下载页面
- [SQL Server 2012 SP3 功能包](https://go.microsoft.com/fwlink/?linkid=615935)
- [SQL Server 2012 SP3 Express](https://go.microsoft.com/fwlink/?linkid=692144)

若要详细了解如何基于当前安装版本确定要安装的文件的位置和名称，请参阅 [SQL Server 2012 Service Pack 3 版本信息](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information)中的“选择正确的文件进行下载”部分。

## <a name="service-pack-2-release-notes"></a>Service Pack 2 发行说明
  
### <a name="download-pages"></a>下载页面 
- [SQL Server 2012 SP2 功能包](https://www.microsoft.com/download/details.aspx?id=43340)
- [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)

使用下表，基于您当前安装的版本识别要下载的文件的位置和名称。 下载页包含系统要求和基本安装说明。  

|如果您目前已经安装的版本是...|而您需要...|请下载和安装...|  
|---|---|---|   
|32 位安装：|||  
|SQL Server 2012 的任何版本的 32 位版|升级到 SQL Server 2012 SP2 的 32 位版|**SQLServer2012SP2-KB2958429-** <arch> **-** <lang id> **.exe** ，来自 [SQL Server 2012 SP2 下载页](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|SQL Server 2012 RTM Express 的 32 位版|升级到 SQL Server 2012 Express SP2 的 32 位版|**SQLEXPR_** <arch> **_** <lang> **.msi** ，来自 [SQL Server 2012 SP2 Express 下载页](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|仅 SQL Server 2012 的客户端和可管理性工具（包括 SQL Server 2012 Management Studio）的 32 位版|将客户端和可管理性工具升级到 SQL Server 2012 SP2 的 32 位版|**SQLEXPRWT_** <arch> **_** <lang> **.msi** ，来自 [SQL Server 2012 SP2 Express 下载页](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|SQL Server 2012 Management Studio Express 的 32 位版|升级到 SQL Server 2012 SP2 Management Studio Express 的 32 位版|**SQLManagementStudio_** <arch> **_** <lang> **.msi** ，来自 [SQL Server 2012 SP2 Express 下载页](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|SQL Server 2012 的任何版本的 32 位版以及客户端和管理工具（包括 SQL Server 2012 RTM Management Studio）的 32 位版|将所有产品都升级到 SQL Server 2012 SP2 的 32 位版|**SQLEXPRADV_** <arch> **_** <lang> **.msi** ，来自 [SQL Server 2012 SP2 Express 下载页](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|[Microsoft SQL Server 2012 RTM 功能包](https://www.microsoft.com/download/details.aspx?id=56041) (#microsoft-sql-server-2012-rtm-功能包) 或 [Microsoft SQL Server 2012 SP1 功能包](https://www.microsoft.com/download/details.aspx?id=35575)(#microsoft-sql-server-2012-sp1-功能包) 中一种或多种工具的 32 位版|将工具升级到 Microsoft SQL Server 2012 SP2 功能包的 32 位版|Microsoft [SQL Server 2012 SP2 功能包下载页](https://www.microsoft.com/download/details.aspx?id=43340)中的一种或多种工具|  
|64 位安装：|||  
|SQL Server 2012 的任何版本的 64 位版|升级到 SQL Server 2012 SP2 的 64 位版|SQLServer2012SP2-KB2958429-<arch>-<langid>.exe，来自 [SQL Server 2012 SP2 下载页](https://go.microsoft.com/fwlink/?LinkID=401006)(#sql-server-2012-sp2-下载页)|  
|SQL Server 2012 RTM Express 的 64 位版|升级到 SQL Server 2012 SP2 的 64 位版|**SQLEXPR_** <arch> **_** <lang> **.msi** ，来自 [SQL Server 2012 SP2 Express 下载页](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|仅 SQL Server 2012 的客户端和可管理性工具（包括 SQL Server 2012 R2 Management Studio）的 32 位版|将客户端和可管理性工具升级到 SQL Server 2012 SP2 的 64 位版|**SQLEXPRWT_** <arch> **_** <lang> **.msi** ，来自 [SQL Server 2012 SP2 Express 下载页](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|SQL Server 2012 Management Studio Express 的 64 位版|升级到 SQL Server 2012 SP2 Management Studio Express 的 64 位版|**SQLManagementStudio_** <arch> **_** <lang> **.msi** ，来自 [SQL Server 2012 SP2 Express 下载页](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|[Microsoft SQL Server 2012 RTM 功能包](https://www.microsoft.com/download/details.aspx?id=56041) (#microsoft-sql-server-2012-rtm-功能包) 或 [Microsoft SQL Server 2012 SP1 功能包](https://www.microsoft.com/download/details.aspx?id=35575)(#microsoft-sql-server-2012-sp1-功能包) 中一种或多种工具的 64 位版|将工具升级到 Microsoft SQL Server 2012 SP2 功能包的 64 位版|Microsoft [SQL Server 2012 SP2 功能包下载页](https://www.microsoft.com/download/details.aspx?id=43340)中的一种或多种工具|   


## <a name="service-pack-1-release-notes"></a>Service Pack 1 发行说明

### <a name="download-pages"></a>下载页面  
- [SQL Server 2012 SP1 功能包](https://go.microsoft.com/fwlink/p/?LinkID=268158)
- [SQL Server 2012 SP1 Express](https://www.microsoft.com/download/details.aspx?id=35579)


请使用下表来确定要下载和安装的文件。 安装 Service Pack 之前请验证您的系统是符合要求的。 表中链接的下载页上提供了系统要求。  

|如果您目前已经安装的版本是...|而您需要...|请下载和安装...|  
|---|---|---|  
|**32 位安装：**|||  
|SQL Server 2012 的任何版本的 32 位版|升级到 SQL Server 2012 SP1 的 32 位版|SQLServer2012SP1-KB2674319-x86-ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|SQL Server 2012 RTM Express 的 32 位版|升级到 SQL Server 2012 Express SP1 的 32 位版|SQLServer2012SP1-KB2674319-x86-ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|仅 SQL Server 2012 的客户端和可管理性工具（包括 SQL Server 2012 Management Studio）的 32 位版|将客户端和可管理性工具升级到 SQL Server 2012 SP1 的 32 位版|SQLManagementStudio_x86_ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=267905)(#此处)）|  
|SQL Server 2012 Management Studio Express 的 32 位版|升级到 SQL Server 2012 SP1 Management Studio Express 的 32 位版|SQLManagementStudio_x86_ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=267905)(#此处)）|  
|SQL Server 2012 的任何版本的 32 位版 **和** 客户端和可管理性工具（包括 SQL Server 2012 RTM Management Studio）的 32 位版|将所有产品都升级到 SQL Server 2012 SP1 的 32 位版|SQLServer2012SP1-KB2674319-x86-ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|[Microsoft SQL Server 2012 RTM 功能包](https://www.microsoft.com/download/details.aspx?id=44272)(#microsoft-sql-server-2012-rtm-功能包) 中一种或多种工具的 32 位版|将工具升级到 Microsoft SQL Server 2012 SP1 功能包的 32 位版|[Microsoft SQL Server 2012 SP1 功能包](https://www.microsoft.com/download/details.aspx?id=35575)中的一个或多个文件|  
|没有安装 SQL Server 2012 的 32 位版|安装包括 SP1 的 32 位 Server 2012（预装了 SP1 的新实例）|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **和** SQLServer2012SP1-FullSlipstream-x86-ENU.box（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|没有安装 SQL Server 2012 Management Studio 的 32 位版|安装 32 位 SQL Server 2012 Management Studio（包括 SP1）|SQLManagementStudio_x86_ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkId=267905)(#此处)）|  
|无 32 位版 SQL Server 2012 RTM Express|安装 32 位 SQL Server 2012 Express（包括 SP1）|SQLEXPR32_x86_ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkId=267905)(#此处)）|  
|**SQL Server 2008** 或 **SQL Server 2008 R2** 的 32 位安装|**就地升级** 到 32 位 SQL Server 2012（包括 SP1）|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **和** SQLServer2012SP1-FullSlipstream-x86-ENU.box（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|**64 位安装：**|||  
|SQL Server 2012 的任何版本的 64 位版|升级到 SQL Server 2012 SP1 的 64 位版|SQLServer2012SP1-KB2674319-x64-ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|SQL Server 2012 RTM Express 的 64 位版|升级到 SQL Server 2012 SP1 的 64 位版|SQLServer2012SP1-KB2674319-x64-ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|仅 SQL Server 2012 的客户端和可管理性工具（包括 SQL Server 2012 R2 Management Studio）的 32 位版|将客户端和可管理性工具升级到 SQL Server 2012 SP1 的 64 位版|SQLManagementStudio_x64_ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=267905)(#此处)）|  
|SQL Server 2012 Management Studio Express 的 64 位版|升级到 SQL Server 2012 SP1 Management Studio Express 的 64 位版|SQLManagementStudio_x64_ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=267905)(#此处)）|  
|SQL Server 2012 的任何版本的 64 位版 **和** 客户端和可管理性工具（包括 SQL Server 2012 RTM Management Studio）的 64 位版|将所有产品都升级到 SQL Server 2012 SP1 的 64 位版|SQLServer2012SP1-KB2674319-x64-ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|[Microsoft SQL Server 2012 RTM 功能包](https://www.microsoft.com/download/details.aspx?id=44272)(#microsoft-sql-server-2012-rtm-功能包) 中一种或多种工具的 64 位版|将工具升级到 Microsoft SQL Server 2012 SP1 功能包的 64 位版|[Microsoft SQL Server 2012 SP1 功能包](https://www.microsoft.com/download/details.aspx?id=35575)中的一个或多个文件|  
|没有安装 SQL Server 2012 的 64 位版|安装包括 SP1 的 64 位 Server 2012（预装了 SP1 的新实例）|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **和** SQLServer2012SP1-FullSlipstream-x64-ENU.box（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  
|没有安装 SQL Server 2012 Management Studio 的 64 位版|安装 64 位 SQL Server 2012 Management Studio（包括 SP1）|SQLManagementStudio_x64_ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=267905)(#此处)）|  
|无 64 位版 SQL Server 2012 RTM Express|安装 64 位 SQL Server 2012 Express（包括 SP1）|SQLEXPR_x64_ENU.exe（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=267905)(#此处)）|  
|**SQL Server 2008** 或 **SQL Server 2008 R2** 的 64 位安装|**就地升级** 到 64 位 SQL Server 2012（包括 SP1）|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **和** SQLServer2012SP1-FullSlipstream-x64-ENU.box（从 [此处](https://go.microsoft.com/fwlink/p/?LinkID=268158)(#此处)）|  

### <a name="known-issues-fixed-in-this-service-pack"></a>此 Service Pack 中已修复的已知问题  
有关此 Service Pack 中已修复的 Bug 和已知问题的完整列表，请参阅 [此知识库文章](https://support.microsoft.com/kb/2674319)。   

### <a name="reinstalling--instances-of-sql-server-failover-cluster-fails-if-you-use-the-same-ip-address"></a>如果使用同一 IP 地址，则重新安装 SQL Server 故障转移群集实例失败  
**问题：** 如果在安装 SQL Server 故障转移群集实例期间指定不正确的 IP 地址，安装将失败。 卸载失败的实例后，如果尝试使用同一实例名称和正确的 IP 地址重新安装 SQL Server 故障转移群集实例，安装将失败。 安装失败是由于上次安装留下的重复资源组造成的。  
  
**解决方法：** 要解决此问题，请在重新安装时使用不同的实例名称，或在重新安装前手动删除该资源组。 有关详细信息，请参阅 [在 SQL Server 故障转移群集中添加或删除节点](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。 
  
### <a name="analysis-services-and-powerpivot"></a>Analysis Services 和 PowerPivot  
  
##### <a name="powerpivot-configuration-tool-does-not-create-the-powerpivot-gallery"></a>PowerPivot 配置工具未创建 PowerPivot 库  
**问题：** PowerPivot 配置工具设置团队网站，因此不创建 PowerPivot 库。  
  
**解决方法：** 创建一个新应用（库）。  
  
1.  确认网站集功能 **“针对网站集的 PowerPivot 功能集成”** 处于活动状态。  
  
2.  从现有网站的 **“网站内容”** 页上，单击 **“添加应用程序”** 。  
  
3.  单击 **“PowerPivot 库”** 。  
  
#### <a name="to-use-powerpivot-for-excel-with-excel-2013-you-must-use-the-add-in-that-is-installed-with-excel"></a>若要将 PowerPivot for Excel 与 Excel 2013 一起使用，您必须使用与 Excel 一起安装的外接程序  
**问题：** 在 Office 2010 中，PowerPivot for Excel 是一种可从 [https://www.microsoft.com/bi/powerpivot.aspx](https://www.microsoft.com/bi/powerpivot.aspx) 下载的独立加载项。 也可以从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=29074)下载它。 请注意有两个版本的 PowerPivot 加载项可下载：一个随 SQL Server 2008 R2 提供，而另一个随 SQL Server 2012 提供。 但对于 Office 2013，PowerPivot for Excel 随 Office 一起提供并且在您安装 Excel 时安装。 尽管 PowerPivot for Excel 2010 的 SQL Server 2008 R2 和 SQL Server 2012 版本与 Excel 2013 不兼容，但是，如果您想要将 Excel 2010 与 Excel 2013 并行运行，仍可以在您的客户端计算机上安装 PowerPivot for Excel 2010。 换言之，Excel 的两个版本可以共存，因此可以使用相应的 PowerPivot 外接程序。  
  
**解决方法：** 若要使用 PowerPivot for Excel 2013，必须启用 COM 加载项。 从 Excel 2013，选择“**文件**” | “**选项**” | “**外接程序**”。从“ **管理** ”下拉框中，选择“ **COM 外接程序** ”，然后单击“ **执行**”。 从“ **COM 外接程序**”中，选择 **Microsoft Office PowerPivot for Excel 2013** ，然后单击“ **确定**”。  
  
### <a name="reporting-services"></a>Reporting Services  
  
#### <a name="install-and-configure-sharepoint-server-2013-prior-to-installing-reporting-services"></a>在安装 Reporting Services 前安装和配置 SharePoint Server 2013  
**问题：** 在安装 SQL Server Reporting Services (SSRS) 之前，请满足以下要求  。  
  
1.  运行 SharePoint 2013 产品准备工具。  
  
2.  安装 SharePoint Server 2013。  
  
3.  运行 SharePoint 2013 产品配置向导或完成等效的配置步骤来配置 SharePoint 场。  
  
**解决方法：** 如果在配置 SharePoint 场之前安装了 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint 模式，则所需的解决方法由所安装的其他组件而定。  
  
#### <a name="power-view-in-sharepoint-server-2013-requires-microsoftanalysisservicesspclientdll"></a>SharePoint Server 2013 中的 Power View 需要 Microsoft.AnalysisServices.SPClient.dll  
**问题：** [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 未安装必需的组件 **Microsoft.AnalysisServices.SPClient.dll**。 如果在 SharePoint 模式下安装 SharePoint Server 2013 Preview 和 [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)][!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] ，但是未下载和安装 PowerPivot for SharePoint 2013 安装程序包 **spPowerPivot.msi** ，Power View 将不起作用且出现以下症状：  
  
**症状**：尝试创建 Power View 报表时，会看到一条类似于以下内容的错误消息：  
  
-   “无法与数据源建立连接...”  
  
内部错误详细信息将包含如下消息：  
  
-   “连接字符串属性‘用户标识’不支持值‘SharePoint 主体’。”  
  
**解决方法：** 在 SharePoint Server 2013 上安装 PowerPivot for SharePoint 2013 安装程序包 (spPowerPivot.msi)  。 该安装程序包作为 [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] 功能包的一部分提供。 可以从 [!INCLUDE[msCoName](../includes/msconame-md.md)] 下载中心的 [SQL Server 2012 SP1 功能包](https://www.microsoft.com/download/details.aspx?id=35575) 下载此功能包。  
  
#### <a name="power-view-sheets-in-a-powerpivot-workbook-are-deleted-after-a-scheduled-data-refresh"></a>在执行预定的数据刷新后删除 PowerPivot 工作簿中的 Power View 工作表  
**问题**：在 PowerPivot for SharePoint 加载项中，如果对带有 Power View 的工作簿使用“计划的数据刷新”，则将删除所有 Power View 工作表  。  
  
**解决方法**：要将“计划的数据刷新”用于 Power View 工作簿，请创建仅用作数据模型的 PowerPivot 工作簿  。 使用您的 Excel 工作表和 Power View 工作表创建单独的工作簿，将它链接到包含数据模型的 PowerPivot 工作簿。 应只对包含数据模型的 PowerPivot 工作簿安排执行数据刷新。  
  
### <a name="data-quality-services"></a>数据库引擎服务  
  
#### <a name="dqs-available-in-the-incorrect-edition-of-sql-server-2012"></a>错误的 SQL Server 2012 版本中提供 DQS  
**问题：** 在 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] RTM 版本中，Data Quality Services (DQS) 功能在 Enterprise、Business Intelligence 和 Developer 版本以外的 SQL Server 版本中提供。 在安装 SQL Server 2012 SP1 后，DQS 将在除 Enterprise、Business Intelligence 和 Developer 版本之外的所有版本中不可用。  
  
**解决方法**：如果你在不支持的版本中使用 DQS，请升级到支持的版本，或者从你的应用程序中删除对此功能的依赖项。  
  
### <a name="sql-server-express"></a>SQL Server Express  
  
#### <a name="full-version-of-sql-server-management-studio-available-in-sql-server-2012-express-sp1"></a>在 SQL Server 2012 Express SP1 中提供 SQL Server Management Studio 的完整版本  
SQL Server 2012 Express Service Pack 1 (SP1) 版本包括 SQL Server 2012 Management Studio 的完整版本（以前仅在 SQL Server 2012 DVD 上提供），而非 SQL Server 2012 Management Studio Express。 若要下载和安装 SQL Server 2012 Express SP1，请参阅 [SQL Server 2012 Express Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=267905)。  
  
### <a name="change-data-capture-service-and-designer-for-oracle-by-attunity"></a>Change Data Capture Service 和 Designer for Oracle by Attunity  
  
#### <a name="upgrading-the-cdc-service-and-designer"></a>升级 CDC 服务和设计器  
**问题：** 如果在你安装 SQL Server 2012 SP1 时，适用于 Oracle 的更改数据捕获设计器和 Attunity 推出的适用于 Oracle 的更改数据捕获服务安装在你的计算机上，则通过安装 SP1 将不会升级这些组件。  
  
**解决方法：** 将 CDC 组件升级到最新版本：  
  
1.  从 [SQL Server 2012 SP1 功能包下载页](https://www.microsoft.com/download/details.aspx?id=35575)下载用于 Change Data Capture Service for Oracle by Attunity 的 .msi 文件。  
  
2.  运行该 .msi 文件。  
  
### <a name="sql-server-data-tier-application-framework-dacfx"></a>SQL Server 数据层应用程序框架 (DACFx)  
**就地升级支持**  
  
此版本的数据层应用程序框架 (DACFx) 支持从以前版本就地升级，因此在升级到此版本前，不需要删除以前的 DACFx 安装。 您可以在 [此处](/previous-versions/sql/sql-server-2016/dn702988(v=sql.130))找到 DACFx 的将来版本。  
  
**对选择性 XML 索引的支持**  
  
SQL Server 2012 SP1 包括对 [选择性 XML 索引 (SXI)](../relational-databases/xml/selective-xml-indexes-sxi.md)(#选择性-xml-索引-(sxi)) 这个新 SQL Server 功能的支持，该功能为 XML 列数据提供新的索引编制方式，提高了性能和效率。  
  
DACFx 现在支持所有 DAC 方案和客户端工具中的 SXI 索引。 SXI 仅在最新版本的 SSDT 中受支持。 SSDT RTM 和 2012 年 9 月版本不支持 SXI。  
  
**对本机 BCP 数据格式的支持**  
  
以前，用于存储 DACPAC 和 BACPAC 包中的表数据的数据格式为 JSON。 应用此更新后，本机 BCP 现在为数据永久格式。 这个变化为 DACFx 带来改进的 SQL Server 数据类型精确度，包括对 SQL_Variant 类型的支持以及针对大型数据库的增强数据部署性能。  
  
**创建/部署包时保存 CHECK 约束状态**  
  
以前，DACFx 不在数据库架构中保存对表定义的 CHECK 约束状态 (WITH CHECK/NOCHECK)，也不在 DACPAC 内存储此信息。 当现有表数据违反 CHECK 约束时，此行为可能导致潜在的包部署问题。 应用此更新后，DACFx 现在在从数据库提取时在 DACPAC 内存储 CHECK 约束的当前状态并在包部署时相应还原此状态。  
  
**对 SqlPackage.exe（DACFx 命令行工具）的更新**  
  
-   带数据提取 DACPAC - 从一个活动 SQL Server 或 Azure SQL 数据库创建数据库快照文件 (.dacpac)，该文件除了包含数据库架构之外还包含用户表的数据。 可以使用 SqlPackage.exe“发布”操作将这些包发布到新的或现有 SQL Server 或 Azure SQL 数据库。 包中包含的数据将替代目标数据库中的现有数据。  
  
-   导出 BACPAC - 创建包含数据库架构和用户数据的活动 SQL Server 或 Azure SQL 数据库的逻辑备份文件 (.bacpac)，这些架构和数据可用于将数据库从本地 SQL Server 迁移到 Azure SQL 数据库。 可以在支持的 SQL Server 版本间导出与 Azure 兼容的数据库，之后再导入。  
  
-   导入 BACPAC - 导入 .bacpac 文件以新建或填充空的 SQL Server 或 Azure SQL 数据库。  
  
MSDN 上的完整 SqlPackage.exe 文档可以在 [此处](../tools/sqlpackage/sqlpackage.md)找到。  
  
**包兼容性**  
  
本版本介绍用于 DAC 包的几个向前兼容方案。  
  
-   本版本创建的不包含 SXI 元素或表数据的 DAC 包可能由以前版本的 DACFx（SQL Server 2012 RTM、SQL Server 2012 CU1 和 DACFx 2012 年 9 月版）使用。  
  
-   以前版本的 DACFx 创建的所有 DAC 包可以由本版本使用。  
  
## <a name="see-also"></a>另请参阅
- [安装 SQL Server 2012 服务更新](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [如何识别 SQL Server 的版本](https://support.microsoft.com/help/321185)
- [安装 SQL Server 2012 服务更新](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [如何识别 SQL Server 的版本](https://support.microsoft.com/help/321185) 
- [如何确定 SQL Server 的版本和版本类别](https://support.microsoft.com/kb/321185)  
- [SQL Server 2014 各个版本支持的功能](./editions-and-components-of-sql-server-2016.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
