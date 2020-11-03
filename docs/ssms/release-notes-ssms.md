---
title: (SSMS) 发行说明
description: SQL Server Management Studio (SSMS) 发行说明。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 3dc76cc1-3b4c-4719-8296-f69ec1b476f9
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 10/27/2020
ms.openlocfilehash: ce232d98e441d6ce217a2f97f6b8b1b5e130b7f3
ms.sourcegitcommit: b09f069c6bef0655b47e9953a4385f1b52bada2b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92734642"
---
# <a name="release-notes-for-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) 发行说明

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

本文提供有关 SSMS 的当前和以前版本的更新、改进和 bug 修复的详细信息。

## <a name="current-ssms-release"></a>当前 SSMS 版本

### <a name="1871"></a>18.7.1

![下载](media/download-icon.png) [下载 SSMS 18.7](download-sql-server-management-studio-ssms.md)

- 版本号：18.7.1
- 生成号：15.0.18358.0
- 发行日期：2020 年 10 月 27 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x40a)

SSMS 18.7 是 SSMS 的最新正式发布 (GA) 版本。 如果需要 SSMS 的早期版本，请参阅 [SSMS 的早期版本](release-notes-ssms.md#previous-ssms-releases)。

#### <a name="whats-new-in-1871"></a>18.7.1 中的新增功能

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]


#### <a name="bug-fixes-in-1871"></a>18.7.1 中的缺陷修复

| 新项 | 详细信息 |
|----------|---------|
| 查询存储 | 修复了右键单击查询存储的对象资源管理器节点时引发的错误。 |


#### <a name="known-issues-1871"></a>已知问题 (18.7.1)

| 新项 | 详细信息 | 解决方法 |
|----------|---------|------------|
| Analysis Services | 通过 msmdpump.dll 连接到 SSAS 时出错。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696)。 | 空值 |
| Analysis Services | 在极少数情况下，如果在使用升级设置升级 SSMS 后尝试打开 DAX 编辑器，则会看到“对象未设置为对象实例”错误。 | 若要解决此问题，请卸载并重新安装 SSMS。 |
| 常规 SSMS | “新建服务器审核规范”对话框可能会导致 SSMS 发生故障，并显示访问冲突错误。 | 空值 |
| 常规 SSMS | 应将使用 SMO 的 SSMS 扩展重新编译为面向新的特定于 SSMS 的 SMO v161 包。 可在 https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ 找到预览版本 </br></br> 针对以前 160 个版本的 Microsoft.SqlServer.SqlManagementObjects 包编译的扩展仍起作用。 | 空值 |
| Integration Services | 导入或导出 Integration Services 中的包或导出 Azure-SSIS Integration Runtime 中的包时，包含脚本任务/组件的包的脚本丢失。 解决方法：删除文件夹“C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild”。 | 空值 |
| Integration Services | 在较新的操作系统中，由于“指定的服务未安装”，Integration Services 远程连接 可能会失败。 解决方法：在“Computer\HKEY_CLASSES_ROOT\AppID”和“Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID”下识别与 Integration Services 相关的注册表位置，并在这些配置单元中，将名为“LocalService”的注册表项重命名为“LocalService_A”，以用于我们尝试连接的特定版本的 Integration Services | 不适用 |
| “对象资源管理器” | 由于引擎发生了与 [Azure Synapse Analytics 按需 SQL](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) 相关的更改，低于 18.7 的 SSMS 版本的对象资源管理器有中断性变更。 | 若要继续将 SSMS 中的对象资源管理器与 Azure Synapse Analytics 按需 SQL 结合使用，你需要 SSMS 18.7 或更高版本。 |

可参考 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server)了解其他已知问题，并向产品团队提供反馈。

## <a name="previous-ssms-releases"></a>SSMS 的早期版本

[!INCLUDE[ssms-connect-aazure-ad](../includes/ssms-connect-azure-ad.md)]

通过选择相关部分中的下载链接下载以前的 SSMS 版本。

| SSMS 版本 | 生成号 | 发布日期 |
|--------------|--------------|--------------|
| [18.7](#187) | 15.0.18357.0 | 2020 年 10 月 20 日 |
| [18.6](#186) | 15.0.18338.0 | 2020 年 7 月 22 日 |
| [18.5.1](#1851) | 15.0.18333.0 | 2020 年 6 月 9 日 |
| [18.5](#185) | 15.0.18330.0 | 2020 年 4 月 7 日 |
| [18.4](#184) | 15.0.18206.0 | 2019 年 11 月 4 日 |
| [18.3.1](#1831) | 15.0.18183.0 | 2019 年 10 月 2日 |
| [18.2](#182) | 15.0.18142.0 | 2019 年 7 月 25 日 |
| [18.1](#181) | 15.0.18131.0 | 2019 年 6 月 11 日 |
| [18.0](#180) | 15.0.18118.0 | 2019 年 4 月 24 日 |
| [17.9.1](#1791) | 14.0.17289.0 | 2018 年 11 月 21 日 |
| [16.5.3](#1653) | 13.0.16106.4 | 2017 年 1 月 30 日 |

### <a name="187"></a>18.7

![下载](media/download-icon.png) [下载 SSMS 18.7](download-sql-server-management-studio-ssms.md)

- 版本号：18.7
- 生成号：15.0.18357.0
- 发行日期：2020 年 10 月 20 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x40a)

SSMS 18.7 是 SSMS 的最新正式发布 (GA) 版本。 如果需要 SSMS 的早期版本，请参阅 [SSMS 的早期版本](release-notes-ssms.md#previous-ssms-releases)。

#### <a name="whats-new-in-187"></a>18.7 中的新变化

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

| 新建项 | 详细信息 |
|----------|---------|
| Azure Data Studio 安装集成 | 安装 SSMS 也会安装 Azure Data Studio。 |
| Always Encrypted | 若要识别新的 HSM 终结点，需要升级 SSMS。 这是通过使用新的 AKV 提供程序 NugetPackage 来完成的。 |
| 导入平面文件 | 经过了改进，通过默认学习 300 行数据，以更好地预测数据类型。 |
| 导入平面文件 | 防止应该是 SmallInt 的列被声明为 TinyInt。 |
| 导入平面文件 | 经过了改进，在数据导入失败时，可以正确地清理 DW 表。 |
| Resource Governor | 新增了对十进制值的支持。 |
| 显示计划 | 新增了 PREDICT 运算符。 |
| XEvent UI | 新增了使用 wait_type 名称编写脚本来输出扩展事件的功能。 要求用户在 wait_type 筛选器谓词中使用 map_value 列的值，而不是 map_key，因为密钥值在版本升级期间可能会发生变更。 解决方法：新增了一个复选框，用户可用来选择使用 map_value 还是 map_key 作为 wait_type 筛选器谓词值。 |

#### <a name="bug-fixes-in-187"></a>18.7 中的缺陷修复

| 新项 | 详细信息 |
|----------|---------|
| 可访问性 | 修复了当查询 DW 时弹出的“数据库不支持以下设置”窗口中按钮的 Tab 键顺序。 |
| 辅助功能 | 导入和导出向导：在高 DPI 模式下，页面布局不正确。 |
| 活动监视器 | 修复了以下问题：活动监视器在打开“进程”选项卡时暂停。请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37050118)。 |
| AlwaysOn 可用性组 | 修复了以下问题：读取扩展可用性组故障转移无法正常运行。 |
| Analysis Services | 已将 PowerQuery 组件更新为 2.84.982，以用于 AS 表格模型方案。 |
| Analysis Services | 修复了以下问题：在尝试通过 msmdpump.dll 连接到 SSAS 时出错。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696)。 |
| 备份/还原 | 修复了以下问题：对于 SQL 2016 及更低版本，在 HostDistribution 属性缺失的情况下选择“查看连接属性”生成了 SMO 错误。 |
| 数据库设计器 | 修复了以下问题：在处理十进制数字时 SSMS 发生故障。 |
| 数据库关系图 | 修复了以下问题：在“添加表”对话框没有正确显示的情况下，使用数据库关系图可能会导致 SSMS 发生故障或挂起。 |
| 数据库镜像 | 修复了以下问题：镜像配置失败。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897281)。 |
| 常规 SSMS | 修复了以下问题：在尝试连接到 Azure SQL DB 时，可能需要数秒钟（用户数据库中的 SQL 登录）。 |
| 常规 SSMS | 修复了以下问题：SSMS 不处理/不显示捕获到的死锁（.xdl 文件）。 |
| 常规 SSMS | 修复了以下问题：无法尝试打开 SQL Server 2008 R2 及更低版本的错误日志设置，找不到 ErrorLogSizeKb 属性。 |
| 常规 SSMS | 关于 [Azure Synapse Analytics 按需 SQL](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) 支持的常规修复和改进。 |
| 导入平面文件 | 修复了以下问题：向导没有检测到文件可能被另一个应用程序使用，而是抛出了错误。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/40761574)。 |
| 导入/导出数据层应用程序 | 修复了以下问题：导入 bacpac 时，默认服务层为标准 S0（与 Azure 门户和 SqlPackage.exe 行为相同）。 |
| 导入平面文件 | 修复了以下问题：向导没有检测到文件可能被另一个应用程序使用，而是抛出了错误。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/40761574)。 |
| Integration Services | 修复了以下问题：当不同的订阅有相同的名称时，IR 创建向导和作业迁移向导中的 Azure 订阅组合框项是重复的。 |
| Integration Services | 修复了以下问题：有时，无法在 IR 创建向导中启用“连接”按钮。 |
| Integration Services | 修复了以下问题：“设置参数值”对话框中的“使用环境变量”组合框项不按顺序排列。 |
| Intellisense | 修复了以下问题：在执行查询时，SSMS 可能会发生故障。 |
| Intellisense | 修复了以下问题：当用户连接到配置了只读路由的可用性组时，IntelliSense 无法运行。 |
| 链接服务器 | 修复了以下问题：拥有 CONTROL SERVER 权限（但不是 sysadmin 角色）的用户无法添加链接服务器。 |
| 日志查看器 | 修复了以下问题：拥有 VIEW SERVER STATE 权限的用户无法查看 SQL Server 错误日志。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/32899204)。 |
| “对象资源管理器” | 修复了以下问题：在某些对象资源管理器节点（如“策略管理”、“扩展事件”）上选择“启动 PowerShell”菜单可能会导致 PowerShell 无法正常启动。 |
| 已注册的服务器 | 修复了以下问题：在尝试注册中央管理服务器时 SSMS 发生故障。 |
| 已注册的服务器 | 修复了以下问题：缺少从已注册的服务器启动 Azure Data Studio 的菜单项。 |
| 报表 | 修复了以下问题：在性能仪表板上，无法尝试转到子链接（如“费用高昂的查询”）。 这个问题在大多数非英语版本的 SSMS 中很常见。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/41454499)。 |
| 显示计划 | 修复了以下问题：在使用“查找节点”搜索文本时 SSMS 发生故障。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/40421650)。 |
| 显示计划 | 在“内存授予”工具提示行上添加了 KB 后缀 |
| 漏洞评估 | 修复了以下问题：当尝试在漏洞评估中设置基线时，SSMS 抛出了错误。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/40578565)。 |
| XEvent UI | 修复了以下问题：点击 F1 无法转到 DOCS 上的正确页面。 |
| XEvent UI | 修复了 XEvent Viewer 中的日志，其中工具提示没有正确显示包含使用代理项对编码的文本的文本。 |

#### <a name="known-issues-187"></a>已知问题 (18.7)

| 新项 | 详细信息 | 解决方法 |
|----------|---------|------------|
| Analysis Services | 通过 msmdpump.dll 连接到 SSAS 时出错。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696)。 | 空值 |
| Analysis Services | 在极少数情况下，如果在使用升级设置升级 SSMS 后尝试打开 DAX 编辑器，则会看到“对象未设置为对象实例”错误。 | 若要解决此问题，请卸载并重新安装 SSMS。 |
| 常规 SSMS | “新建服务器审核规范”对话框可能会导致 SSMS 发生故障，并显示访问冲突错误。 | 空值 |
| 常规 SSMS | 应将使用 SMO 的 SSMS 扩展重新编译为面向新的特定于 SSMS 的 SMO v161 包。 可在 https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ 找到预览版本 </br></br> 针对以前 160 个版本的 Microsoft.SqlServer.SqlManagementObjects 包编译的扩展仍起作用。 | 空值 |
| Integration Services | 导入或导出 Integration Services 中的包或导出 Azure-SSIS Integration Runtime 中的包时，包含脚本任务/组件的包的脚本丢失。 解决方法：删除文件夹“C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild”。 | 空值 |
| Integration Services | 在较新的操作系统中，由于“指定的服务未安装”，Integration Services 远程连接 可能会失败。 解决方法：在“Computer\HKEY_CLASSES_ROOT\AppID”和“Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID”下识别与 Integration Services 相关的注册表位置，并在这些配置单元中，将名为“LocalService”的注册表项重命名为“LocalService_A”，以用于我们尝试连接的特定版本的 Integration Services | 不适用 |
| “对象资源管理器” | 由于引擎发生了与 [Azure Synapse Analytics 按需 SQL](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) 相关的更改，低于 18.7 的 SSMS 版本的对象资源管理器有中断性变更。 | 若要继续将 SSMS 中的对象资源管理器与 Azure Synapse Analytics 按需 SQL 结合使用，你需要 SSMS 18.7 或更高版本。 |
| 查询存储 | 右键单击时查询存储的对象资源管理器节点时引发错误。 | 通过展开节点并右键单击各个子选项，直接访问项。 |

### <a name="186"></a>18.6

![下载](media/download-icon.png) [下载 SSMS 18.6](https://go.microsoft.com/fwlink/?linkid=2146265)

- 版本号：18.6
- 生成号：15.0.18338.0
- 发行日期：2020 年 7 月 22 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x40a)

#### <a name="whats-new-in-186"></a>18.6 中的新增功能

| 新建项 | 详细信息 |
|----------|---------|
| Analysis Services | 更新到最新版本的 AS 客户端库。 |
| 审核 | 添加了对 SENSITIVE_BATCH_COMPLETED_GROUP 操作 ID（字符串而非数字）的支持。 |
| 审核 | 将以下字段添加到审核查看器：affected_rows、response_rows、connection_id、duration_milliseconds 和 data_sensitivity_information。 |
| 数据分类 | 更新 SSMS 以支持通过 PowerShell cmdlet 导出/导入导出的策略。 |
| 导入平面文件 | 添加了对固定宽度文件的支持，以及对 .csv/.tsv 文件的文件类型检测的支持，确保它们分别分析为 csv/tsv 文件。 |
| Integration Services | 添加了对 Azure SQL 托管实例代理作业的支持，以便在 Azure-SSIS IR 中从包存储执行 SSIS 包。 |
| SMO/脚本编写 | 添加了对在 [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is)（以前的 SQL Azure DW）上编写动态数据掩码脚本的支持。 |
| SMO/脚本编写 | 添加了对在 [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is)（以前的 SQL DW）上编写安全策略脚本的支持。 |

#### <a name="bug-fixes-in-186"></a>18.6 中的 bug 修复

| 新项 | 详细信息 |
|----------|---------|
| 可访问性 | 调整了数据库常规属性页上辅助功能的边框颜色（网格和名称框周围的边框更暗，对比度设为 > 3:1）。 |
| 可访问性 | 添加了对查询执行的处理以更新讲述人（需要在计算机上安装 NetFx4.8+）。 |
| Always Encrypted | 修复了尽管 CMK 启用了 enclave，“新建列加密密钥”对话框仍表示 CMK 未启用 enclave 这一问题。 |
| Analysis Services | 修复了查看 Analysis Services 分区可能导致未经处理的异常的问题。 |
| **数据库关系图** | 修复了数据库关系图长时间未解决的问题，该问题导致现有关系图损坏和 SSMS 崩溃。 如果使用 SSMS 18.0 到 18.5.1 之间的版本创建或保存关系图，并且该关系图包含文本批注，则在任何版本的 SSMS 中均无法打开该关系图。 经过修复后，SSMS 18.6 可以打开并保存 SSMS 17.9.1 及更早版本创建的关系图。 SSMS 17.9.1 和以前的版本也可以在 SSMS 18.6 保存关系图后打开它。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37992649)。 |
| 数据分类 | 修复了“数据分类”窗格的“建议”面板中未显示列名的问题。 |
| 常规 SSMS |  修复了 SQL Azure DB（超大规模服务层）的数据库属性“大小”和“可用空间”的值不正确的问题。 |
| 常规 SSMS | 修复了 SQL Azure DB 的数据库属性“大小”显示最大值而非数据库实际大小的问题（注意：对于 DW，它仍显示最大大小）。 |
| 常规 SSMS | 解决了 SSMS 中的三个常见挂起源的问题。 |
| 常规 SSMS | 修复了数个与 SSMS 连接对话框忘记条目（服务器/用户/密码）相关的问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/40256401)和 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/40015519)。 |
| 常规 SSMS |   修复了“统计信息属性”对话框中选中“更新这些列的统计信息”复选框并选择“确定”后无反应的问题。 统计信息未更新，并且尝试为操作编写脚本时产生消息：没有要进行脚本编写的操作。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37799992)。 |
| 常规 SSMS | 解决了与 [CVE-2020-1455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2020-1455) 相关的问题。 | 
| 导入/导出数据层应用程序 | 修复了导入 bacpac 文件时 SSMS 引发错误的问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/40229137)。 |
| Integration Services | 修复了使用 SSMS 18.4 或更早版本在 Azure SQL 托管实例中执行 SSIS 包时客户无法编辑 SQL 代理作业步骤的 bug。 |
| Integration Services |  修复了用于在 SQL 代理作业步骤中为本地 SQL Server 执行 SSIS 包的“执行选项”选项卡中缺少“使用 32 位运行时”选项的 bug。 |
| Intellisense/编辑器 | 修复了选择“文件”->“新建”->“数据库引擎查询”时可能弹出错误对话框的问题。 |
| “对象资源管理器” | 修复了在对象资源管理器中右键单击表或索引节点时 Azure SQL 数据库“属性”窗口无法使用的问题。 |
| “对象资源管理器” | 解决了当控制平面中断影响 sys.database_service_objectives 时 SSMS 无法在 Azure 中展开 master 数据库的数据库节点这一问题。 |
| 报表 | 修复了 Linux 上已损坏的几个标准报表 </br></br> 示例：内存消耗报表失败，出现类似于“"/var/opt/mssql/log/log_116.trc\log.trc" 无效...”的错误。 |
| SMO/脚本编写 | 更新了逻辑以在 Azure SQL 数据库中创建新的数据库，将 Gen5_2 用作默认 SLO。 |
| Xevent UI | 修复了长时间未解决的问题（在 SSMS 18.0 中引入）：“保存为 XEL 文件...”引发错误。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37695592)。 |

#### <a name="known-issues-186"></a>已知问题 (18.6)

| 新项 | 详细信息 | 解决方法 |
|----------|---------|------------|
| Analysis Services | 通过 msmdpump.dll 连接到 SSAS 时出错。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696)。 | 空值 |
| 常规 SSMS | “新建服务器审核规范”对话框可能会导致 SSMS 发生故障，并显示访问冲突错误。 | 空值 |
| 常规 SSMS | 应将使用 SMO 的 SSMS 扩展重新编译为面向新的特定于 SSMS 的 SMO v161 包。 可在 https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ 找到预览版本 </br></br> 针对以前 160 个版本的 Microsoft.SqlServer.SqlManagementObjects 包编译的扩展仍起作用。 | 空值 |
| Integration Services | 导入或导出 Integration Services 中的包或导出 Azure-SSIS Integration Runtime 中的包时，包含脚本任务/组件的包的脚本丢失。 解决方法：删除文件夹“C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild”。 | 空值|
| Integration Services | 在较新的操作系统中，由于“指定的服务未安装”，Integration Services 远程连接 可能会失败。 解决方法：在“计算机\HKEY_CLASSES_ROOT\AppID 与计算机\HKEY_CLASSES_ROOT\ WOW6432Node\AppID”下找到与 Integration Services 相关的注册表位置，在这些配置单元中将名为“LocalService”的注册表项重命名为“LocalService_A”，以获取尝试连接的 Integration Services 指定版本 | 空值|

### <a name="1851"></a>18.5.1

![下载](media/download-icon.png) [下载 SSMS 18.5.1](https://go.microsoft.com/fwlink/?linkid=2132606)

- 版本号：18.5.1
- 生成号：15.0.18333.0
- 发行日期：2020 年 6 月 9 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x40a)

#### <a name="bug-fixes-in-1851"></a>18.5.1 中的 bug 修复

| 新项 | 详细信息 |
|----------|---------|
| Analysis Services | 提高了在连接到 AS Azure 或 Power BI 服务器的情况下展开数据库列表时的性能。 |
| Analysis Services | 修复了尝试打开 Analysis Services 服务器的同步数据库向导时出现错误的问题。 |
| Analysis Services | 修复了阻止用户使用单元数据权限查询 SSAS 2017 及更早版本的问题。 |
| 常规 SSMS | [表设计器 - 修复了在表设计器网格中尝试使用 TAB 键时发出嘟嘟声的问题](https://feedback.azure.com/forums/908035/suggestions/40318435) |

#### <a name="known-issues-1851"></a>已知问题 (18.5.1)

| 新项 | 详细信息 | 解决方法 |
|----------|---------|------------|
| 常规 SSMS | Diagram Design 存在一个已知缺陷，导致现有关系图损坏。 例如，使用 SSMS 17.9.1 创建关系图设计，然后使用 SSMS 18.x 更新/保存它，再尝试使用 17.9.1 打开它。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37992649)，了解更多详细信息。 | 空值 |
| 常规 SSMS | “新建服务器审核规范”对话框可能会导致 SSMS 发生故障，并显示访问冲突错误。 | 空值 ||
| SMO/脚本 | 需要针对新的 SMO v160 重新编译使用 SMO 的 SSMS 扩展。 | 空值 |
| Integration Services | 导入或导出 Integration Services 中的包或导出 Azure-SSIS Integration Runtime 中的包时，包含脚本任务/组件的包的脚本丢失。 解决方法： | 删除文件夹“C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild”。 |

### <a name="185"></a>18.5

![下载](media/download-icon.png) [下载 SSMS 18.5](https://go.microsoft.com/fwlink/?linkid=2125901)

- 版本号：18.5
- 生成号：15.0.18330.0
- 发行日期：2020 年 4 月 7 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x40a)

#### <a name="whats-new-in-185"></a>18.5 中的新变化

| 新建项 | 详细信息 |
|----------|---------|
| Analysis Services | 现已开始支持 Analysis Services 中的 Power BI 终结点（Azure Analysis Services 的匹配功能）。 |
| Analysis Services | 探查器：现已开始支持 Analysis Services 跟踪定义 15.1。 |
| 数据分类 | 向 VA 扫描结果视图添加了按钮，单击此但能可转到“数据分类”窗格，从而修正数据分类规则。 |
| 数据分类 | 现已开始支持“数据分类”中的敏感度级别。 |
| 超大规模 | 现已开始支持向 SQL Azure 超大规模导入数据层应用程序 (.bacpac)  。 |
| Integration Services | 支持在 MI 代理作业中通过文件系统执行 SSIS 包。 |
| Integration Services | 在将已启用 Azure 的 DTExec 配置为在 Azure-SSIS Integration Runtime 上调用 SSIS 包执行方面进行了用户友好改进。
| Integration Services | 支持连接 Azure-SSIS Integration Runtime，以及管理或执行包存储中的 SSIS 包。
| Integration Services | 支持将本地 SSIS 代理作业迁移到 ADF 管道和触发器。
| Integration Services | 改进了从 SSIS DB 导出 SSIS 项目的用户体验。 旧版导出加载和升级了 SSIS 项目中的包，相比之下，新版独立导出不会加载和升级 SSIS 项目中的包。 而是将包保留在项目中，就像它们在 SSIS DB 中一样，不同之处在于将保护级别更改为 EncryptSensitiveWithUserKey。 |
| SMO/脚本 | 向 View 对象添加了新属性 DwMaterializedViewDistribution。 |
| SMO/脚本 | 已取消支持功能限制  （此预览功能已从本地 SQL Azure 和 SQL 中删除）。 |
| SMO/脚本 | 添加了“笔记本”  ，可作为“生成脚本”向导的目标。 |
| SMO/脚本 | 添加了对按需 SQL 的支持  。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) -“平台”、“名称”和“engineEdition”字段现在可以包含常规的逗号分隔列表（“平台”  ：\[Windows  , Linux  \]），而不仅仅包含正则表达式（“平台”  ：\/Windows\|Linux\/  ）
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 添加了 13 条评估规则。 如需了解更多详情，请转到 [GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-assessment-api)。 |

#### <a name="bug-fixes-in-185"></a>18.5 中的缺陷修复

| 新项 | 详细信息 |
|----------|---------|
| 可访问性 | SSIS ADF/新建计划 - 修复了以下问题：在讲述人的扫描模式下，“新建计划”  向导下的焦点顺序不符合逻辑。 |
| 可访问性 | 延伸数据库向导 - 修复了以下问题：屏幕阅读器在提供查询表的相关信息时不提示表名称。 |
| Analysis Services | 修复了使用 Azure AD 连接在 AS 中编写脚本时的缓存连接。 |
| AlwaysOn | 修复了以下问题：第一个添加到 AlwaysOn AG 的数据库无法正确联接。
| AlwaysOn | 修复了以下问题：在连接到大数据群集终结点后，如果尝试显示仪表板，会看到错误消息。 |
| 审核 | 修复了以下问题：如果存储帐户的根文件夹中有名称为空的文件夹，“审核日志合并”窗口发生故障。 |
| 审核 | 修复了以下问题：如果容器的根目录中的项过多，“审核日志合并”窗口不会显示所有服务器。 |
| 数据分类 | 修复了以下问题：对于包含大量表的数据库，“数据分类”  向导不会打开。 |
| 数据分类 | 现在，在验证过程中，我们对每个标签/infoType 强制执行不同的 GUID，并强制执行 GUID 的结构。 |
| 数据分类 | 删除了 SqlServer2019 中的分类流程。 |
| 数据分类 | 更正了旧验证测试（添加了级别、删除了非法属性 InformationTypes  ），并为前两个点添加了新测试。 |
| 数据分类 | “已分类列”表正上方的按钮现在按预期最小化建议面板。 |
| 常规 SSMS | 更新了 MSODBC 和 MSOLEDB 驱动程序的版本。 |
| 常规 SSMS | 解决了 SSMS 中至少两个常见源的挂起和故障问题。 |
| 常规 SSMS | 解决了在选择“浏览”按钮时“还原”  对话框挂起的另一种情况。 |
| 常规 SSMS | 修复了 SQL 按需的“新建数据库 GUI”  。 |
| 常规 SSMS | 修复了 SQL 按需的“新建外部表...”  和“新建外部数据源...”模板  。 |
| 常规 SSMS | 修复了 SQL 按需的数据库属性、连接属性、报表隐藏和重命名。 |
| 常规 SSMS | Always Encrypted：修复了以下问题：在选择启用了 enclave 的新密钥时，密钥名称下拉列表变成只读列表。 |
| 常规 SSMS | 清理了“数据库属性选项”  网格，其中显示两个“其他类别”  。 |
| 常规 SSMS | 修复了以下问题：滚动条从“数据库属性选项”网格的中间开始滚动。 |
| 常规 SSMS | 修复了以下问题：在连接到 Analysis Services 服务器后，如果打开 .sql 文件，会导致 SSMS 发生故障。 |
| 常规 SSMS | “连接”对话框 - 修复了以下问题：无法取消选中“记住密码”。 |
| 常规 SSMS | 修复了以下问题：始终记住与服务器/用户关联的凭据。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37875172)。 |
| 常规 SSMS | 修复了以下问题：“编辑器”窗口有时无法正常刷新。 为此，可以在“工具”>“选项”>“环境”中禁用硬件加速  。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37474042)。 |
| 常规 SSMS | 修复了以下问题：无法通过代理进行 Azure Active Directory 身份验证。 |
| 高 DPI/缩放 | 修复了以下问题：“索引属性”  上的控件可能会错误呈现（按钮重叠网格）。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/36030424)。 |
| 高 DPI/缩放 | 修复了“数据库属性”  对话框中的多个问题，此对话框可能会在 4K 监视器上显示剪裁的控件。 |
| 高 DPI/缩放 | 修复了 4k 显示器上的“发布和订阅”向导。 |
| 高 DPI/缩放 | 在“新建审核服务器规范”页上进行了次要修复。 |
| 高 DPI/缩放 | 修复了“高可用性”向导中的 4k 显示器问题。 |
| 高 DPI/缩放 | 修复了以下问题：当缩放比例为 125% 时，用户无法在“Xevent 会话”向导的“Xevent 新建会话”窗口和“设置会话事件筛选器”中添加目标。 |
| 高 DPI/缩放 | 修复了以下问题：当缩放比例大于 100% 时，“将数据库备份到 URL”  UI 上的控件无法呈现。 |
|导入平面文件 | 更新了“平面文件导入”向导，为“允许 NULL”列提供了全部选中的选项。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/38027137)。 |
| “对象资源管理器” | 修复了以下问题：当连接字符串用于在“连接”对话框进行连接时，对象资源管理器可能会显示不正确的信息。 |
| “对象资源管理器” | 修复了以下问题：对于包含数千个（超过 20,000 个）表的数据库，OE 在展开表时的速度缓慢。 |
| 查询存储 UI | 修复了以下问题：（对于“等待时间”  指标）作为每个单独等待类别的执行计数总和的 TRC 报表计算执行计数不正确。 但对于查询的单个执行，将为查询等待的每个等待类别都注册它。 因此，如果 TRC 只是跨等待类别计算它的总和，则会导致执行计数膨胀。 实际上，它应是跨 wait_category 的最大值。 |
| 查询存储 UI | 修复了以下问题：当结果集按 top x 进行筛选时，TRC 详细视图返回的数据不正确。 之所以会这样是因为，查询使用了多个公用表表达式，这些表达式随后联接在一起，以创建最终结果集。 如果 top x 被推送到 CTE 中，有时可能会筛选掉所需的行。 这有时可能会导致结果集具有不确定性。 解决方法是，不要将 top x 子句推送到 CTE。 |
| 查询存储 UI | 修复了以下问题：网格或图表视图中的“计划摘要”需要上次查询执行等待时间。 缺少此列会破坏查询。 此更改集会将这一列添加到等待统计信息 CTE。 |
| 显示计划 | 改进了 SSMS 如何显示包含多个执行的运算符的估计行计数：(1) 已将 SSMS 中的“估计行数”  修改为“每执行的估计行数”；(2) 新增了属性“所有执行的估计行数”  ；(3) 已将属性“实际行数”  修改为“所有执行的实际行数”  。 |
| SQL 代理 | 修复了以下问题：尝试编辑 SQL 代理作业步骤可能会导致 SSMS UI 冻结。 SSMS 现在允许查看（“查看”  按钮）名称已标记化的 output_file（至少对于SQL 代理支持的且在运行时未确定的简单宏/标记是这样）。 此外，当用户无权访问文件时，SSMS 并没有禁用“查看”按钮（就 SQL 权限而言）。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/39063124)。 |
| SQL 代理 | 修复了“作业步骤”页上的 Tab 键顺序。 |
| SQL 代理 | 反转了“作业步骤”页上的“下一步”和“上一步”按钮的位置，让它们按逻辑顺序排列。 |
| SQL 代理 | 调整了“作业计划”窗口，使其不剪裁 UI。 |
| SMO/脚本 | 修复了 SQL 按需的数据库脚本。 |
| SMO/脚本 | 删除了显式 sqlvariant 强制转换（SqlOnDemand 的非法 T-SQL 语法），这修复了 SqlOnDemand 的脚本。 |
| SMO/脚本 | 修复了以下问题：跳过 SQL Azure 索引上的 FILLFACTOR。 |
| SMO/脚本 | 修复了与为外部对象编写脚本相关的问题。 |
| SMO/脚本 | 修复了以下问题：“生成脚本”不允许针对 SQL 数据库选择扩展属性的脚本选项。 同时，还修复了此类扩展属性的脚本。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - XTPHashAvgChainBuckets 规则中的帮助链接不正确。 |
| XEvent UI | 修复了以下问题：将鼠标悬停在网格中的项之上即选中这些项。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/38262124)和 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server/suggestions/37873921)。 |

#### <a name="known-issues-185"></a>已知问题 (18.5)

| 新项 | 详细信息 | 解决方法 |
|----------|---------|------------|

- 无法从计算机 B 修改通过在计算机 A 上运行的 SSMS 创建的数据库关系图（SSMS 崩溃）。 请参阅 [SQL Server 用户反馈 37992649](https://feedback.azure.com/forums/908035/suggestions/37992649)，了解更多详细信息。

- 导入或导出 Integration Services 中的包或导出 Azure-SSIS Integration Runtime 中的包时，包含脚本任务/组件的包的脚本丢失。 解决方法是，删除文件夹 C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild  。

- “新建服务器审核规范”对话框可能会导致 SSMS 发生故障，并显示访问冲突错误。

- 需要将使用 SMO 的 SSMS 扩展重新编译为，定目标到新的 SMO v160（包将在 SSMS 18.5 发布后立即在 nuget.org 上提供）。

- [在 SSMS 中通过 msmdpump.dll 连接到 SSAS 时出错](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696-error-when-connecting-to-ssas-via-msmdpump-dll-in)。

### <a name="184"></a>18.4

![下载](media/download-icon.png) [下载 SSMS 18.4](https://go.microsoft.com/fwlink/?linkid=2108895)

- 版本号：18.4
- 生成号：15.0.18206.0
- 发行日期：2019 年 11 月 4 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x40a)

| 新建项 | 详细信息 |
|----------|---------|
| 数据分类 | 添加了对用于数据分类的自定义信息保护策略的支持。 |
| 查询存储 | 在对话框属性中添加了“每个查询的最大计划数”值  。 |
| 查询存储 | 添加了对新的自定义捕获策略的支持。 |
| 查询存储 | 向查询存储的“数据库属性”    选项添加了“等待统计信息捕获模式”。 |
| SMO/脚本 | SQL DW 中的具体化视图的支持脚本。 |
| SMO/脚本 | 添加了对按需 SQL 的支持  。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 添加了 50 条评估规则（请参阅 GitHub 上的详细信息）。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 添加了基本数学表达式以及与规则条件的比较。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 添加了对 RegisteredServer 对象的支持。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 更新了以 JSON 格式存储规则的方式，还更新了应用替代项/自定义项的机制。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 更新了规则以支持 Linux 上的 SQL。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 更新了规则集 JSON 格式并添加了架构版本。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 更新了 cmdlet 输出以提高建议的可读性。 |
| XEvent 探查器 | 向 XEvent 探查器会话添加了 error_reported 事件  。 |

#### <a name="bug-fixes-in-184"></a>18.4 中的 bug 修复

| 新项 | 详细信息 |
|----------|---------|
| Analysis Services | 修复了多维数据库的 DAX 脚本编辑器未在 IntelliSense 中显示表的问题。 |
| Analysis Services | 使用 DAX 分析程序转换为引擎字符串。 这适用于国际分隔符、十进制和空白。 |
| Always Encrypted | 修复了声明验证不区分大小写的问题   。 |
| Always Encrypted | 修复了错误/警告报告未正常运行的问题。 |
| 复制数据库向导 | 修复了此对话框呈现中的各种截断和布局问题。 |
| 常规 SSMS | 修复了一个长期未解决的问题，即当还指定了 SQL 文件时，SSMS 不遵从在命令行中传递的连接信息。 |
| 常规 SSMS | 修复了尝试在“复制筛选器”对象上显示安全对象时 SSMS 中的故障。 |
| 常规 SSMS | 通过让 SSMS 查看凭据缓存，减轻了 -P 命令行选项的删除：如果找到所需凭据，则会使用它建立连接。 |
| 导入平面文件 | 修复了导入平面文件  功能未正确处理文本限定符的问题。 |
| “对象资源管理器” | 修复了在对象资源管理器中删除 Azure SQL 数据库会显示错误消息的问题。 |
| 查询结果 | 修复了在 SSMS 18.3.1 中引入的一个问题，即网格会稍微绘制得窄一点，并在每列中最长的字符串末尾显示“...”  。 |
| 复制工具 | 修复了在尝试编辑 SQL 代理作业时导致应用程序引发错误（“无法加载文件或程序集...”）的问题。 |
| SMO/脚本 | 修复了其排序规则是 Japanese_BIN2 的 SQL DW 的“编写表脚本为...”  无法正常运行的问题。|
| SMO/脚本 | 修复了 ScriptAlter() 在服务器上结束语句执行的问题。|
| SQL 代理 | 修复了代理操作员 UI 在操作员名称在 UI 中进行了更改时不更新名称，也不会对它编写脚本的问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/32897647)，了解更多详细信息。|

#### <a name="known-issues-184"></a>已知问题 (18.4)

- 无法从计算机 B 修改通过在计算机 A 上运行的 SSMS 创建的数据库关系图（SSMS 崩溃）。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37992649)，了解更多详细信息。

- 在多个查询窗口之间切换时会出现重绘问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37474042)，了解更多详细信息。 此问题的解决方法是在“工具”>“选项”下禁用硬件加速。

可参考 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server)了解其他已知问题，并向产品团队提供反馈。

### <a name="1831"></a>18.3.1

![下载](media/download-icon.png) [下载 SSMS 18.3.1](https://go.microsoft.com/fwlink/?linkid=2105412)

- 版本号：18.3.1
- 生成号：15.0.18183.0
- 发行日期：2019 年 10 月 2日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x40a)

#### <a name="whats-new-in-1831"></a>18.3 1 的新增功能

| 新建项 | 详细信息 |
|----------|---------|
| 数据分类 | 向列属性 UI 添加数据分类信息（信息类型  、信息类型 ID  、敏感度标签  和敏感度标签 ID  未在 SSMS UI 中公开）。 |
| Intellisense/编辑器 | 更新了对最近添加到 SQL Server 2019 中的功能（例如，“ALTER SERVER CONFIGURATION”  ）的支持。 |
| Integration Services | 添加新的选择菜单项 `Tools > Migrate to Azure > Configure Azure-enabled DTExec`，该菜单项将 Azure-SSIS Integration Runtime 上的 SSIS 包执行作为 ADF 管道中的“执行 SSIS 包”活动调用。 |
| SMO/脚本 | 添加了对 Azure SQL DW 唯一约束的支持脚本的支持。 |
| SMO/脚本 | 数据分类 </br> 添加了对 SQL 版本 10 (SQL 2008) 及更高版本的支持。 </br> - 为 SQL 版本 15 (SQL 2019) 和更高版本以及 Azure SQL 数据库添加了新的敏感度属性“rank”。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 向规则集格式添加了版本控制。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 添加了新的检查。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 添加了对 Azure SQL 托管实例的支持。 |
| SMO/脚本 | [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md) - 更新了 cmdlet 的默认视图，将结果作为表格显示。 |

#### <a name="bug-fixes-in-1831"></a>18.3.1 中的 bug 修复

| 新项 | 详细信息 |
|----------|---------|
| Analysis Services | 修复了 MDX 查询编辑器中的缩放问题。|
| Analysis Services | 修复了 XEvent UI 中阻止户创建新会话的问题。 |
| 到 SQL Azure 的数据库部署 | 修复了导致此功能无法正常运行的问题（在 DacFx 中）。|
| 常规 SSMS | 修复了导致 SSMS 在 XEvent 查看器使用排序功能时崩溃的问题。 |
| 常规 SSMS | 修复了 SSMS 还原数据库可能无限期停止响应的长期未解决的问题。 </br></br> 请参阅 SQL Server 用户反馈项，了解更多详细信息： </br> [还原数据库 -“选择备份设备”加载缓慢](https://feedback.azure.com/forums/908035/suggestions/32899099/)。  </br> [SSMS 2016 在数据库还原对话框中运行缓慢](https://feedback.azure.com/forums/908035/suggestions/32900767/)。 </br> [还原数据库速度缓慢](https://feedback.azure.com/forums/908035/suggestions/32900224/)。  </br> [单击“...”还原数据库时设备挂起](https://feedback.azure.com/forums/908035/suggestions/34281658/)。  |
| 常规 SSMS | 修复了所有登录名的默认语言显示为阿拉伯语的问题。 </br></br> 请参阅 SQL Server 用户反馈项，了解更多详细信息：[SSMS 18.2 默认语言显示错误](https://feedback.azure.com/forums/908035/suggestions/38236363)。 |
| 常规 SSMS | 修复了难以看到“查询选项”对话框（当用户右键单击 T-SQL 编辑器窗口时）的问题  。|
| 常规 SSMS | 现在可以在“工具”>“选项”>“查询执行”>“SQL Server”>“高级”>“显示完成时间”下配置“结果网格/文件”（在 SSMS 18.2 中引入）中显示的“完成时间”消息  。 |
| 常规 SSMS | 在连接对话框中，将“Active Directory - 密码”和“Active Directory - 集成”分别替换为“Azure Active Directory - 密码”和“Azure Active Directory - 集成”     。 |
| 常规 SSMS | 修复了以下问题：当用户位于 UTC 偏移量为负的 TZ 中时，用户无法使用 SSMS 配置对 SQL Azure 托管实例的审核。 |
| 常规 SSMS | 修复了 XEvent UI 中将鼠标悬停在网格上导致行被选中的问题。 </br></br> 请参阅 SQL Server 用户反馈项，了解更多详细信息：[SSMS 扩展事件 UI 在鼠标悬停时选择操作](https://feedback.azure.com/forums/908035/suggestions/38262124)。 |
| 导入平面文件 | 通过让用户在简单或丰富数据类型检测之间进行选择，修复了“导入平面文件”未导入所有数据的问题。</br></br> 请参阅 SQL Server 用户反馈项，了解更多详细信息：[SSMS“导入平面文件”未能导入所有数据](https://feedback.azure.com/forums/908035/suggestions/38096989)。 |
| Integration Services | 为 SSIS 操作报表添加新的操作类型 *StartNonCatalogExecution* 。|
| Integration Services | 修复了 Azure 数据工厂管道中由 Azure 启用的实用工具 `DTExec` 导致的问题，以使用正确参数类型。 （18.3.1 明确了这一点） |
| SMO/脚本 | 修复了使用 **SMO.Server.SetDefaultInitFields(true)** 时导致 SMO 在提取属性时引发错误的问题。|
| 查询存储 UI | 修复了在“跟踪查询”视图中选择“执行计数”指标时 Y 轴无法缩放的问题   。 |
| 漏洞评估 | 禁用了 Azure SQL 数据库的清除和批准基线。|

#### <a name="known-issues-1831"></a>已知问题 (18.3.1)

- 无法从计算机 B 修改通过在计算机 A 上运行的 SSMS 创建的数据库关系图（SSMS 崩溃）。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37992649)，了解更多详细信息。

- 在多个查询窗口之间切换时会出现重绘问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37474042)，了解更多详细信息。 此问题的解决方法是在“工具”>“选项”下禁用硬件加速。

可参考 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server)了解其他已知问题，并向产品团队提供反馈。

### <a name="182"></a>18.2

![下载](media/download-icon.png) [下载 SSMS 18.2](https://go.microsoft.com/fwlink/?linkid=2099720)

- 版本号：18.2
- 生成号：15.0.18142.0
- 发行日期：2019 年 7 月 25 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x40a)

#### <a name="whats-new-in-182"></a>18.2 的新增功能

| 新项 | 详细信息 |
|----------|---------|
| Integration Services (SSIS) | Azure 中 SSIS 包计划程序的性能优化。 |
| Intellisense/编辑器 | 添加了对数据分类的支持。 |
| OPTIMIZE_FOR_SEQUENTIAL_KEY | 添加了 IntelliSense 支持。 |
| OPTIMIZE_FOR_SEQUENTIAL_KEY | 可以在数据库引擎内启用优化，有助于提高索引中高并发插入的吞吐量。 此选项旨在用于易发生最后一页插入争用的索引，常见于有顺序键（如标识列、序列或日期/时间列）的索引。 有关更多详细信息，请参阅 [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md#sequential-keys)。 |
| 查询执行或结果 | 在消息中添加了一个完成时间，以跟踪给定查询执行完毕的时间  。 |
| 查询执行或结果 | 允许显示更多数据（结果转换为文本）以及将其存储在单元中（结果转换为网格）。 对于这两种情况，SSMS 当前最多支持 200 万个字符（之前分别为 25.6 万和 6.4 万）。 这还解决了用户无法从网格单元中获取超过 43680 个字符的问题。 |
| 显示计划 | 在启用了[内联标量 UDF 特性](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining) (ContainsInlineScalarTsqludfs) 的情况下，在 QueryPlan 中添加了新的属性。 |
| SMO | 添加了对 SQL 评估 API  的支持。 有关详细信息，请参阅 [SQL 评估 API](../tools/sql-assessment-api/sql-assessment-api-overview.md)。 |
|  |  |

#### <a name="bug-fixes-in-182"></a>18.2 中的 bug 修复

| 新项 | 详细信息 |
|------------|-------|
| 可访问性 | 更新了按 F3 时可排序的 XEvent UI（网格）。 |
| AlwaysOn | 修复了在尝试删除可用性组 (AG) 时 SSMS 引发错误的问题 |
| AlwaysOn | 修复了将副本配置为“同步”、使用读取扩展 AG（群集类型 = NONE）时，SSMS 显示错误的故障转移向导的问题。 现在，SSMS 显示针对 Force_Failover_Allow_Data_Loss 选项的向导，这是群集类型 NONE 可用性唯一允许的向导 |
| AlwaysOn | 修复了向导将允许的同步数限制为三个的问题 |
| 数据分类 | 修复了尝试查看 CompatLevel 级别低于 150 的数据库的数据分类报告时，SSMS 引发“索引（从零开始）必须大于或等于零”错误  。 |
| 常规 SSMS | 修复了用户无法通过鼠标滚轮水平滚动结果窗格的问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/34145641)，了解更多详细信息。 |
| 常规 SSMS | 更新了“活动监视器”，使其忽略良性等待类型 SQLTRACE_WAIT_ENTRIES  |
| 常规 SSMS | 修复了某些颜色选项（“文本编辑器”>“编辑器”选项卡和状态栏）未持久化的问题  。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37924165)
| 常规 SSMS | 在连接对话框中，将“Azure Active Directory - 含 MFA 支持的通用身份验证”替换为了“Azure Active Directory - 含 MFA 的通用身份验证”（功能相同，但希望能够减少混淆）   。 |
| 常规 SSMS | 更新了 SSMS，使其在创建 Azure SQL 数据库时使用正确的默认值。 |
| 常规 SSMS | 修复了当服务器为 [SQL Linux 容器](../linux/quickstart-install-connect-docker.md)时，用户无法从[注册服务器](register-servers/register-servers.md)中的节点启动 PowerShell 的问题  。 |
| 导入平面文件 | 修复了从 SSMS 18.0 升级到 18.1 后，“导入平面文件”无法正常运行的问题  。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37912636) |
| 导入平面文件 | 修复了标头包含 Unicode 字符的 .csv 文件中“导入平面文件向导报告重复或无效的列”的问题  。 |
| “对象资源管理器” | 修复了在连接到 SQL Express 时，某些菜单项（例如 SQL Server“导入和导出向导”）缺失或禁用的问题  。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37500016)，了解更多详细信息。 |
| “对象资源管理器” | 修复了将对象从对象资源管理器拖动到编辑器时导致 SSMS 故障的问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37887988)，了解更多详细信息。 |
| “对象资源管理器” | 修复了重命名数据库导致对象资源管理器中显示错误数据库名称的问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37638472)，了解更多详细信息。 |
| “对象资源管理器” | 修复了一个长期未解决的问题，即对于设置为使用 Windows 不再支持的排序规则的数据库，尝试展开其对象资源管理器中的“表”节点时会触发错误（并且用户无法展开其表）  。 此类排序规则的一个示例是 Korean_Wansung_Unicode_CI_AS。 |
| [注册服务器](register-servers/register-servers.md) | 修复了在注册服务器使用“Active Directory - 集成”或“Azure Active Directory - 含 MFA 的通用身份验证”的情况下，尝试针对多个服务器发出查询（在注册服务器的“组”下）因 SSMS 无法连接而无法正常工作的问题    。 |
| [注册服务器](register-servers/register-servers.md) | 修复了在注册服务器使用“Active Directory - 密码”或“SQL 身份验证”，且用户选择不记住密码的情况下，尝试针对多个服务器发出查询（在注册服务器的“组”下）可能导致 SSMS 故障的问题    。 |
| 报表 | 修复了“磁盘使用情况”报告中的问题，即当数据文件有大量盘区时，报告失败  。 |
| 复制工具 | 修复了复制监视器无法与 AG 中的发布服务器数据库和分发服务器一起使用的问题（以前曾在 SSMS 17.x 中修复过此问题） |
| SQL 代理 | 修复了在添加、插入、编辑或删除作业步骤时，焦点重置到第一行而不是活动行的问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/38070892)，了解更多详细信息。 |
| SMO/脚本 | 修复了“CREATE OR ALTER”不对包含扩展属性的对象编写脚本的问题  。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server/suggestions/37236748)，了解更多详细信息。 |
| SMO/脚本 | 修复了 SSMS 无法正确编写 CREATE EXTERNAL LIBRARY 的脚本的问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37868089)，了解更多详细信息。 |
| SMO/脚本 | 修复了尝试对包含几千个表的数据库运行“生成脚本”导致进度对话框似乎卡滞的问题  。 |
| SMO/脚本 | 修复了 SQL 2019 中编写外部表的脚本不起作用的问题  。 |
| SMO/脚本 | 修复了 SQL 2019 中编写外部数据源的脚本不起作用的问题  。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/34295080)，了解更多详细信息。 |
| SMO/脚本 | 修复了面向 Azure SQL 数据库时，未编写列*扩展属性*脚本的问题。 有关更多详细信息，请参阅 [stackoverflow](https://stackoverflow.com/questions/56952337/how-can-i-script-the-descriptions-of-columns-in-ms-sql-server-management-studio)。 |
| SMO/脚本 | 最后一页插入：SMO - 添加属性“Index.IsOptimizedForSequentialKey”  |
|**SSMS 安装程序**| **缓解了 SSMS 安装程序错误地阻止安装 SSMS 报告不匹配语言的问题。在某些异常情况下（例如中止的安装程序或错误地卸载以前的 SSMS 版本），这可能是一个问题。请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37483594/)，了解更多详细信息。** |
| XEvent 探查器 | 修复了关闭查看器时出现的故障。 |

#### <a name="known-issues-182"></a>已知问题 (18.2)

- 无从在计算机 B 中修改从在计算机 A 上运行的 SSMS 创建的数据库关系图（这会导致 SSMS 出现故障）。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37992649)，了解更多详细信息。

- 在多个查询窗口之间切换时会出现重绘问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37474042)。 此问题的解决方法是在“工具”   > “选项”  下禁用硬件加速。

- 对于在网格、文本或文件中显示的 SSMS 结果中可以查看的数据大小，存在限制。

- 在对象资源管理器中删除 Azure SQL 数据库时接收错误出现问题，但实际上会成功接收。

- 不管登录名的实际默认语言设置如何，SQL 登录名的默认语言在“登录属性”对话框中都可能显示为阿拉伯语。 若要查看给定登录名的实际默认语言，请使用 T-SQL 从 master.sys.server_principles 选择登录名的 default_language_name   。

### <a name="181"></a>18.1

![下载](media/download-icon.png) [下载 SSMS 18.1](https://go.microsoft.com/fwlink/?linkid=2094583)

- 版本号：18.1
- 生成号：15.0.18131.0
- 发行日期：2019 年 6 月 11 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x40a)

#### <a name="whats-new-in-181"></a>18.1 的新增功能

| 新建项 | 详细信息 |
| :-------| :------|
| 数据库关系图 | [SSMS 中再现数据库关系图](https://feedback.azure.com/forums/908035/suggestions/37507828)。
| SSBDIAGNOSE.EXE |SQL Server 诊断（命令行工具）已添加回 SSMS 包。|
| Integration Services (SSIS) | 支持 Azure 中的计划 SSIS 包，该包位于 Azure 中的 SSIS 目录或系统文件中。 启动新建计划对话框中有三个项，新计划...  菜单项（右键单击 Azure 中 SSIS 目录中的 SSIS 包时会显示）、“在 Azure 中安排执行 SSIS 包”菜单项（位于“工具”菜单项下的“迁移到 Azure”菜单项下）以及“在 Azure 中安排执行 SSIS”（右键单击 Azure SQL 托管实例中的 SQL Server 代理下的作业文件夹时显示）。|

#### <a name="bug-fixes-in-181"></a>18.1 中的 bug 修复

| 新项 | 详细信息 |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 可访问性 | 改进了代理作业 UI 的辅助功能。 |'
| 可访问性 | 通过添加“自动刷新”按钮的可访问名称，同时添加智能可访问名称，该名称会帮助用户了解其按的是何按钮以及按下该按钮所产生的影响，从而改进了“Stretch 监视器”页面上的辅助功能  。 |
| ADS 集成| 修复了尝试使用 ADS 注册服务器时 SSMS 可能出现崩溃的问题。|
| 数据库设计器 | 添加了对 Latin1_General_100_BIN2_UTF8 排序规则的支持（在 SQL Server 2019 CTP3.0 中可用） |
| 数据分类 | 防止将敏感度标签添加到历史记录表中的列，这是不受支持的。 |
| 数据分类 | 解决了有关错误处理兼容级别（服务器与数据库）的问题。 |
| 数据库设计器 | 添加了对 Latin1_General_100_BIN2_UTF8 排序规则的支持（在 SQL2019 CTP3.0 中可用）。 |
| 常规 SSMS | 修复了以下问题：索引中伪列脚本不正确。 |
| 常规 SSMS | 修复了“登录属性”页面中的问题：单击“添加凭据”按钮可能会引发空引用异常  。 |
| 常规 SSMS | 修复了“索引属性”页面中显示的 money 列大小。 |
| 常规 SSMS | 修复了以下问题：在 SQL 编辑器窗口中，SSMS 未遵循“工具/选项”中的 Intellisense 设置  。 |
| 常规 SSMS | 修复了以下问题：SSMS 未遵循“帮助”设置（联机与脱机）。 |
| 高 DPI | 修复了不受支持的查询选项的错误对话框中的控件布局。 |
| 高 DPI | 修复了 SSMS 的某些本地化版本上“新建可用性组”页面中控件的布局  。 |
| 高 DPI | 修复了“新建作业计划”页面的布局  。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37632094)，了解更多详细信息。 |
| 导入平面文件 | 修复了以下问题：在导入期间，行可能会在无任何通知的情况下丢失。|
| Intellisense/编辑器 | 减少了流向 IntelliSense 的 Azure SQL 数据库的基于 SMO 的查询流量。 |
| Intellisense/编辑器 | 修复了在键入 T-SQL 以创建用户时显示的工具提示中的语法错误。 此外，修复了错误消息以区分用户和登录名。 |
| 日志查看器 | 修复了以下问题：即使双击对象资源管理器中旧的存档符号，SSMS 也始终打开当前服务器（或代理）日志。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37633648)，了解更多详细信息。 |
| SSMS 安装 | 修复了以下问题：当安装日志路径包含空格时导致 SSMS 安装失败。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37496110)，了解更多详细信息。 |
| SSMS 安装 | 修复了以下问题：SSMS 在显示初始屏幕后立即退出。 </br> 有关更多详细信息，请参阅以下站点：[SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37502512)、[SSMS 拒绝启动](https://dba.stackexchange.com/questions/238609/ssms-refuses-to-start)和[数据库管理员](https://dba.stackexchange.com/questions/237086/sql-server-management-studio-18-wont-open-only-splash-screen-pops-up)。 |
| 对象资源管理器 | 解除了在 Linux 上连接到 SQL 时对“启用 PowerShell”的启用限制  。 |
| 对象资源管理器 | 修复了以下问题：在尝试展开 Polybase/横向扩展组节点时（连接到计算节点时）导致 SSMS 崩溃。 |
| 对象资源管理器 | 修复了以下问题：即使在禁用给定索引后，“禁用”菜单项仍然处于启用状态  。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37735375)，了解更多详细信息。 |
| 报表 | 将报表更正为以 KB 为单位显示 GrantedQueryMemory（SQL 性能仪表板报表）。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37167289)，了解更多详细信息。 |
| 报表 | 改进了对 Always-On 方案中日志块的跟踪。 |
| 显示计划 | 在显示计划架构中添加了新的显示计划元素 SpillOccurred  。 |
| 显示计划 | 向显示计划架构添加了远程读取（ActualPageServerReads、ActualPageServerReadAheads、ActualLobPageServerReads、ActualLobPageServerReadAheads）     。 |
| SMO/脚本 | 避免在非图形表的脚本编写期间查询边缘约束。 |
| SMO/脚本 | 删除了在使用数据分类编写列的脚本时，对敏感度分类的约束  。 |
| SMO/脚本 | 修复了以下问题：生成数据时图形表上的“生成脚本”失败。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898466)，了解更多详细信息。 |
| SMO/脚本 | 修复了以下问题：EnumObjects() 方法无法获取同义词的架构名称。 |
| SMO/脚本 | 修复了以下问题：在 UIConnectionInfo.LoadFromStream() 中未读取 AdvancedOptions 部分（未指定密码时）  。 |
| SQL 代理 | 修复了以下问题：在使用“作业属性”窗口时导致 SSMS 崩溃。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37164244)，了解更多详细信息。 |
| SQL 代理 | 修复了以下问题：“作业步骤属性”中的“查看”按钮不会始终处于启用状态，从而阻止查看给定作业步骤的输出  。 |
| XEvent UI | 在 XEvents 列表中添加了“Package”列，以区分具有相同名称的事件。 |
| XEvent UI | 向 XEventUI 添加了缺少的“EXTERNAL LIBRARY”类类型映射。 |

#### <a name="known-issues-181"></a>已知问题 (18.1)

- 用户在将对象资源管理器中的表对象拖动到查询编辑器时可能收到错误。 我们已注意到此问题并计划在下一版本中提供修补程序。

- 关闭 SSMS 18.1 之后，“选项”->“文本编辑器”->“编辑器选项卡”和“状态栏”->“状态栏布局和颜色”下的“组连接”和“单服务器连接”颜色选项将消失   。 重新打开 SSMS 后，“状态栏布局和颜色”选项会还原为默认设置（白色）。

- 对于在网格、文本或文件中显示的 SSMS 结果中可以查看的数据大小，存在限制。

- 无法从计算机 B 修改通过在计算机 A 上运行的 SSMS 创建的数据库关系图（SSMS 崩溃）。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37992649)，了解更多详细信息。

### <a name="180"></a>18.0

![下载](media/download-icon.png) [下载 SSMS 18.0](https://go.microsoft.com/fwlink/?linkid=2088649)

- 版本号：18.0  
- 生成号：15.0.18118.0  
- 发行日期：2019 年 4 月 24 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x804)| [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x404)| [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x409)| [法语](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x40c)| [德语](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x407)| [意大利语](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x410)| [日语](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x411)| [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x412)| [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x416)| [俄语](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x419)| [西班牙语](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x40a)

#### <a name="whats-new-in-180"></a>18.0 的新增功能

| 新建项| 详细信息|
| :-------| :------|
|支持 SQL Server 2019|SSMS 18.0 是第一个可以完全识别 SQL Server 2019 (compatLevel 150) 的版本  。|
|支持 SQL Server 2019|支持 SQL Server 2019 和 SQL 托管实例中的“BATCH_STARTED_GROUP”和“BATCH_COMPLETED_GROUP”。|
|支持 SQL Server 2019|SMO：添加了对 UDF 内联的支持。|
|支持 SQL Server 2019|GraphDB：在 Graph TC 序列的显示计划中添加标志。|
|支持 SQL Server 2019|Always Encrypted：添加了对 AEv2/Enclave 的支持。|
|支持 SQL Server 2019|Always Encrypted：用户单击“选项”按钮以启用/配置 Enclave 支持时，连接对话框将显示新的“Always Encrypted”选项卡。|
|减小了 SSMS 下载大小| 当前大小约为 500 MB，约为 SSMS 17.x 捆绑包的一半。
|SSMS 基于 Visual Studio 2017 独立 Shell|新的 shell（SSMS 基于 Visual Studio 2017 15.9.11）解锁了 SSMS 和 Visual Studio 中的所有辅助功能修复程序，并包含最新的安全修复程序。|
|SSMS 辅助功能改进| 我们进行了大量的工作以解决所有工具（SSMS、DTA 和探查器）中的辅助功能问题|
|SSMS 现可安装在自定义文件夹中| 此选项在命令行（适用于无人参与安装）和设置 UI 中均可用。 从命令行中，将此额外参数传递给 SSMS-Setup-ENU.exe： SSMSInstallRoot=C:\MySSMS18 默认情况下，SSMS 的新安装位置为：%ProgramFiles(x86)%\Microsoft SQL Server Management Studio 18\Common7\IDE\ssms.exe。  这并不意味着 SSMS 为多实例。|
|SSMS 允许使用 OS 语言以外的其他语言进行安装|已解除阻止混合语言设置。 例如，可以在法语版 Windows 上安装 SSMS 德语版。 如果 OS 语言与 SSMS 语言不匹配，则用户需要通过“工具” > “选项” > “国际设置”更改语言，否则 SSMS 会显示英语 UI    。|
|SSMS 与 SQL 引擎不再共享组件|我们为避免与 SQL 引擎共享组件付出了大量努力，共享组件通常会导致可维护性问题（一方强制改写另一方安装的文件）。|
|SSMS 需要 NetFx 4.7.2 或更高版本|我们将最低要求从 NetFx4.6.1 升级到 NetFx4.7.2：这让我们能够利用新框架公开的新功能。|
|迁移 SSMS 设置的功能| 当 SSMS 18 首次启动时，系统会提示用户迁移 17.x 设置。 用户设置文件现存储为纯 XML 文件，从而提高了可移植性并且可能允许编辑。|
|支持高 DPI| 现默认启用高 DPI。|
|SSMS 附带 Microsoft OLE DB 驱动程序| 有关详细信息，请参阅[下载适用于 SQL Server 的 Microsoft OLE DB 驱动程序](../connect/oledb/download-oledb-driver-for-sql-server.md)。|
|Windows 8 不支持 SSMS。 Windows 10 和 Windows Server 2016 需要 1607 (10.0.14393) 或更高版本|由于 NetFx 4.7.2 上新的依赖项，SSMS 18.0 不会安装在 Windows 8、较旧版本的 Windows 10 和 Windows Server 2016 上。 SSMS 安装程序会阻止这些系统。 仍然支持 Windows 8.1。|
|SSMS 不再添加到 PATH 环境变量|SSMS.EXE 的路径（以及常规工具）不再添加到路径中。 用户可以手动添加它，也可以在新式 Windows 计算机上使用“开始”菜单添加。|
|不再需要包 ID 来开发 SSMS 扩展| 以前 SSMS 仅选择性地加载已知包，因此需要开发人员注册其自己的包。 这种情况不会再出现。|
|常规 SSMS|在 SSMS 中公开文件组的 AUTOGROW_ALL_FILES 配置选项。|
|常规 SSMS|从 SSMS GUI 中删除了有风险的“轻型池”和“优先级提升”选项。 有关详细信息，请参阅[优先级提升详细信息 - 以及不建议这样做的原因](https://deep.data.blog/2010/01/26/priority-boost-details-and-why-its-not-recommended/)。
|常规 SSMS|用于创建文件的新建菜单和键绑定：Ctrl+Alt+N  。 CTRL + N 继续创建一个新查询  。|
|常规 SSMS|用户现在可以使用“新防火墙规则”对话框指定规则名称，而不是代表用户自动生成一个规则名称  。|
|常规 SSMS|特别针对 v140+ T-SQL 改进了编辑器中的 IntelliSense。|
|常规 SSMS|在 SSMS UI 中的排序规则对话框上为 UTF-8 添加了支持。|
|常规 SSMS|为连接对话框 MRU 密码切换到了“Windows 凭据管理器”。 这将解决一个长期未解决的问题，即密码的持久性并非始终可靠。|
|常规 SSMS|通过确保在预期监视器上弹出越来越多的对话框和窗口，改进了对多监视器系统的支持。|
|常规 SSMS|在“服务器属性”对话框的“新建数据库设置”页中公开了“备份校验和默认”服务器配置。 有关详细信息，请参阅 [https://feedback.azure.com/forums/08035-sql-server/suggestions/34634974](https://feedback.azure.com/forums/08035-sql-server/suggestions/34634974)。|
|常规 SSMS|在“配置 SQL Server 错误日志”下公开了“错误日志文件的最大大小”。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/33624115](https://feedback.azure.com/forums/908035/suggestions/33624115)。|
|常规 SSMS|在“工具”菜单下添加了“迁移到 Azure”– 我们集成了数据库迁移助手和 Azure 数据库迁移服务，提供快速便捷的访问，帮助加快迁移到 Azure 的过程。|
|常规 SSMS|添加了逻辑，提示用户在使用“更改连接”时提交打开的事务。|
|Azure Data Studio 集成|添加了启动/下载 Azure Data Studio 的菜单项。|
|Azure Data Studio 集成|向对象资源管理器添加了“启动 Azure Data Studio”菜单项。|
|Azure Data Studio 集成|右键单击 OE 中的数据库节点时，将向用户显示上下文菜单，以在 Azure Data Studio 中运行查询或创建新笔记本。|
|Azure SQL 支持| SLO/Edition/MaxSize 数据库属性现在接受自定义名称，从而更容易支持 Azure SQL 数据库的未来版本。|
|Azure SQL 支持| 添加了对 vCore SKU（常规用途和业务关键）的支持：Gen4_24 和所有 Gen5。|
|Azure SQL 托管实例|连接到 Azure SQL 托管实例时，在 SMO 和 SSMS 中添加了新的“Azure AD 登录”作为新登录类型。|
|AlwaysOn|在 SSMS Always On 仪表板中重新处理 RTO（估计恢复时间）和 RPO（估计的数据丢失）。 请参阅 [https://docs.microsoft.com/sql/database-engine/availability-groups/windows/monitor-performance-for-always-on-availability-groups](../database-engine/availability-groups/windows/monitor-performance-for-always-on-availability-groups.md) 中更新后的文档。|
|Always Encrypted| “连接到服务器”对话框中新“Always Encrypted”选项卡的“启用 Always Encrypted”复选框现在提供为数据库连接启用/禁用 Always Encrypted 的简便方法。|
|具有安全 Enclave 的 Always Encrypted| 已在 SQL Server 2019 中进行多项增强来支持具有安全 Enclave 的 Always Encrypted：“连接到服务器”对话框（新的“Always Encrypted”选项卡）中指定 Enclave 证明 URL 的文本字段。  “新列主密钥”对话框中用于控制新列主密钥是否允许 Enclave 计算的新复选框。  其他 Always Encrypted 密钥管理对话框现在可公开列主密钥允许 Enclave 计算的信息。|
|审核文件|已将身份验证方法从基于存储帐户密钥更改为基于 Azure AD 的身份验证。|
|数据分类| 重新组织了数据分类任务菜单：将子菜单添加到了数据库任务菜单中，并添加了从菜单中打开报表而无需先打开分类数据窗口的选项。|
|数据分类|向 SMO 添加了新功能“数据分类”。 列对象公开新属性：SensitivityLabelName、SensitivityLabelId、SensitivityInformationTypeName、SensitivityInformationTypeId 和 IsClassified（只读）。 有关详细信息，请参阅 [ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../t-sql/statements/add-sensitivity-classification-transact-sql.md)|
|数据分类|向“数据分类”浮出控件添加了新的“分类报表”菜单项。|
|数据分类| 更新了建议。|
|数据库兼容性级别升级|在“数据库名称”>“任务”>“数据库升级”下添加了一个新选项 ***。此选项将启动新的查询优化助手 (QTA)，引导用户完成以下过程*** ：在升级数据库兼容性级别之前，收集性能基线。 升级到所需数据库兼容性级别。  对同一工作负荷收集第二轮性能数据。 检测工作负荷回归并提供测试建议，以提高工作负荷性能。  这类似于在[查询存储使用方案](../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade)中记录的数据库升级过程，最后一步（QTA 不依赖之前已知的良好状态来生成建议）除外。|
|数据层应用程序向导|添加了对使用图形表导入/导出数据层应用程序的支持。|
|平面文件导入向导|添加了用于通知用户导入可能已导致列重命名的逻辑。|
|Integration Services (SSIS)|添加了支持，使客户能够在 Azure 政府云中的 Azure-SSIS IR 上安排 SSIS 包。|
|Integration Services (SSIS)|通过 SSMS 使用 Azure SQL 托管实例的 SQL 代理时，可以在 SSIS 代理作业步骤中配置参数和连接管理器。|
|Integration Services (SSIS)|连接到 Azure SQL 数据库/Azure SQL 托管实例时，可以使用“default”作为初始数据库与它进行连接。|
|Integration Services (SSIS)|在“Integration Services 目录”节点下添加了“在 Azure 数据工厂中尝试 SSIS”的新条目项，此条目项可用于启动“Integration Runtime 创建向导”并快速创建“Azure-SSIS Integration Runtime”  。
|Integration Services (SSIS)|在目录创建向导中添加了“创建 SSIS IR”按钮，该按钮可用于启动“Integration Runtime 创建向导”并快速创建“Azure-SSIS Integration Runtime”  。|
|Integration Services (SSIS)|ISDeploymentWizard 现支持命令行模式下的 SQL 身份验证、Azure Active Directory 集成身份验证和 Azure Active Directory 密码身份验证。|
|Integration Services (SSIS)|部署向导现支持创建和部署到 Azure 数据工厂 SSIS Integration Runtime。|
|对象脚本|编写对象脚本时，为“CREATE OR ALTER”添加了新的菜单项。|
|查询存储|向图表 Y 轴上显示的数字添加千位分隔符，改善了部分报表（资源总消耗）的可用性。|
|查询存储|添加了新的查询等待统计信息报表。|
|查询存储|在“跟踪查询”视图中添加了“执行计数”指标。|
|复制工具|在复制监视器和 SSMS 中添加了对非默认端口规范功能的支持。|
|显示计划|如果可用，则在显示计划运算符节点下添加实际运行时间、实际行与估计行。 这使实际计划看起来与实时查询统计信息计划一致。|
|显示计划|为显示计划单击“编辑查询”按钮时修改了工具提示并添加了注释，以向用户表明如果查询超过 4000 个字符，则 SQL 引擎可能会截断该显示计划。|
|显示计划|添加了显示“Materializer 运算符（外部选择）”的逻辑。|
|显示计划|添加新的显示计划属性 BatchModeOnRowStoreUsed 以轻松识别正在使用“行存储的批处理模式扫描”功能的查询。 每当查询执行行存储的批处理模式扫描时，都会将新属性 (BatchModeOnRowStoreUsed="true") 添加到 StmtSimple 元素。|
|显示计划|为 DW ROLLUP 和 CUBE 添加了对 LocalCube RelOp 的显示计划支持。|
|显示计划|适用于 Azure Synapse Analyticss 中新的 ROLLUP 和 CUBE 聚合功能的新 LocalCube 运算符。|
|SMO| 为可恢复索引创建扩展 SMO 支持。|
|SMO| 在 SMO 对象（“PropertyMissing”）上添加了新事件以帮助应用程序作者更快地检测 SMO 性能问题。|
|SMO| 公开了 Configuration 对象上新的 DefaultBackupChecksum 属性，该属性映射到“备份校验和默认值”服务器配置。|
|SMO| 公开了 Server 对象上新的 ProductUpdateLevel 属性，该属性映射到正在使用的 SQL 版本（例如 CU12 和 RTM）的服务级别上。|
|SMO| 公开了 Database 对象上新的 LastGoodCheckDbTime 属性，该属性映射到“lastgoodcheckdbtime”数据库属性上。 如果该属性不可用，则返回默认值“1900/1/1 凌晨 12:00:00”。|
|SMO|将 RegSrvr.xml 文件（注册服务器配置文件）的位置移动到了“%AppData%\Microsoft\SQL Server Management Studio”（未进行版本控制，因此可以跨 SSMS 版本共享）。|
|SMO|添加了“云见证”作为新的仲裁类型，以及作为新的资源类型。|
|SMO|在 SMO 和 SSMS 中添加了对“边缘约束”的支持。|
|SMO|同时在 SMO 和 SSMS 中为“边缘约束”添加了级联删除。|
|SMO|添加了对数据分类“读写”权限的支持。|
|漏洞评估| 在 Azure SQL DW 上启用了漏洞评估任务菜单。|
|漏洞评估|更改了在 Azure SQL 托管实例上运行的漏洞评估规则集，以便“漏洞评估”扫描结果与 Azure SQL DB 中的保持一致。|
|漏洞评估| “漏洞评估”现在支持 Azure SQL DW。|
|漏洞评估|添加了新的导出功能，以将漏洞评估扫描结果导出为 Excel。|
|XEvent 查看器|XEvent 查看器：启用了显示计划窗口以获取更多 XEvent。|

#### <a name="bug-fixes-in-180"></a>18.0 中的 bug 修复

| 新建项 | 详细信息|
|----------|--------|
|故障与冻结|修复了与 GDI 对象相关的常见 SSMS 故障源。|
|故障与冻结|修复了选择"脚本作为创建/更新/删除"（已删除 SMO 对象的不必要提取）时挂起和性能不佳的一个常见源。|
|故障与冻结|修复了在启用 ADAL 跟踪期间使用 MFA 连接到 Azure SQL 数据库时系统停止响应的问题。|
|故障与冻结|修复了从活动监视器中调用时实时查询统计信息中系统停止响应（或感知到的挂起）的问题（在使用 SQL Server 身份验证但未设置“持久性安全信息”时显示的问题）。|
|故障与冻结|修复了在对象资源管理器中选择“报表”时系统停止响应的问题，可能表现为高延迟连接或资源的临时不可访问性。|
|故障与冻结|修复了尝试使用中央管理服务器和 Azure SQL Server 时 SSMS 中出现的故障。 有关详细信息，请参阅[使用中央管理服务器时 SMSS 17.5 应用程序出现错误和故障](https://feedback.azure.com/forums/908035/suggestions/33374884)。|
|故障与冻结|通过优化检索 IsFullTextEnabled 属性的方式，修复了对象资源管理器中系统停止响应的问题。|
|故障与冻结|通过避免生成检索数据库属性的不必要查询，修复了“复制数据库向导”中系统停止响应的问题。|
|故障与冻结|修复了编辑 T-SQL 时导致 SSMS 停止响应/故障的问题。|
|故障与冻结|缓解了编辑大型 T-SQL 脚本时 SSMS 变得无响应的问题。|
|故障与冻结|修复了处理查询返回的大数据集时 SSMS 内存不足的问题。|
|常规 SSMS|修复了“已注册服务器”的连接中未传递“ApplicationIntent”的问题。|
|常规 SSMS|修复了在高 DPI 监视器上未正确呈现“新 XEvent 会话向导 UI”表单的问题。|
|常规 SSMS|修复了尝试导入 bacpac 文件的问题。|
|常规 SSMS|修复了尝试显示数据库 (FILEGROWTH > 2048 GB) 属性时引发算术溢出错误的问题。|
|常规 SSMS|解决了性能仪表板报表报告在子报表中找不到 PAGELATCH 和 PAGEIOLATCH 等待的问题。|
|常规 SSMS|另一轮修复，通过在正确的监视器中打开对话框，使 SSMS 能更好地识别多监视器。|
|Analysis Services (AS)|修复了 AS Xevent UI 的“高级设置”被剪辑的问题。|
|Analysis Services (AS)|修复了 DAX 分析引发“找不到文件”异常这一问题。|
|Azure SQL Database|修复了连接到 Azure SQL 数据库中的用户数据库而不是主数据库时，Azure SQL 数据库查询窗口中未正确填充数据库列表的问题。|
|Azure SQL Database|修复了无法向 Azure SQL 数据库添加“时态表”的问题。|
|Azure SQL Database|在 Azure 的菜单统计信息下启用了“统计信息属性”子菜单选项，因为它已受到相当长一段时间的完全支持。|
|Azure SQL - 常规支持|修复了阻止用户显示 Azure 订阅（如果存在超出 50 个）的常用 Azure UI 控件中的问题。 此外，排序已更改为按名称而不是按订阅 ID。 例如，尝试从 URL 还原备份时，用户可能会遇到此情况。|
|Azure SQL - 常规支持|修复了在常用 Azure UI 控件中枚举订阅时可能会引发“索引已超出范围”的问题。 必须为非负数且小于集合大小。” 错误的订阅时常用 Azure UI 控件中的问题。 例如，尝试从 URL 还原备份时，用户可能会遇到此情况。|
|Azure SQL - 常规支持|解决了服务级别目标被硬编码使得 SSMS 难以支持更新版 Azure SQL SLO 的问题。 现在，用户可以登录 Azure 并允许 SSMS 检索所有适用的 SLO 数据（版本和最大大小）|
|Azure SQL 托管实例支持|改进/优化了对托管实例的支持：已禁用 UI 中不支持的选项和用于处理 URL 审核目标的“查看审核日志”选项的修复。|
|Azure SQL 托管实例支持|“生成和发布脚本”向导脚本不支持的 CREATE DATABASE 子句。|
|Azure SQL 托管实例支持|为托管实例启用了实时查询统计信息。|
|Azure SQL 托管实例支持|数据库属性 -> 文件错误编写了 ALTER DB ADD FILE 脚本。|
|Azure SQL 托管实例支持|修复了即使选择某些其他计划类型时仍选择 ONIDLE 计划的 SQL 代理计划程序的回归。|
|Azure SQL 托管实例支持|调整 MAXTRANSFERRATE、MAXBLOCKSIZE 以在 Azure 存储上进行备份。|
|Azure SQL 托管实例支持|在还原操作之前编写结尾日志备份的问题（这在 CL 上不受支持）。|
|Azure SQL 托管实例支持|创建数据库向导未正确编写 CREATE DATABASE 语句。|
|Azure SQL 托管实例支持|连接到托管实例时，对 SSMS 中的 SSIS 包进行特殊处理。|
|Azure SQL 托管实例支持|修复了连接到托管实例时尝试使用“活动监视器”显示错误的问题。|
|Azure SQL 托管实例支持|改进了对 Azure AD 登录（在 SSMS 资源管理器中）的支持。|
|Azure SQL 托管实例支持|改进了 SMO 文件组对象的脚本编写。|
|Azure SQL 托管实例支持|改进了凭据的 UI。|
|Azure SQL 托管实例支持|添加了对逻辑复制的支持。|
|Azure SQL 托管实例支持|修复了导致右键单击数据库并选择“导入数据层应用程序”失败的问题。|
|Azure SQL 托管实例支持|修复了导致右键单击“TempDB”会显示错误的问题。|
|Azure SQL 托管实例支持|修复了对 SMO 中的 ALTER DB ADD FILE 语句编写脚本导致生成的 T-SQL 脚本为空的问题。|
|Azure SQL 托管实例支持|改进了托管实例服务器特定属性（硬件生成、服务层、使用和保留的存储）的显示。|
|Azure SQL 托管实例支持|修复了编写数据库脚本（“脚本作为创建...”）没有编写额外文件组和文件脚本的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/37326799](https://feedback.azure.com/forums/908035/suggestions/37326799)。 |
|备份/还原/附加/分离数据库|修复了 .mdf 文件的物理文件名与原始文件名不匹配时用户无法附加数据库的问题。|
|备份/还原/附加/分离数据库|修复了 SSMS 可能找不到有效的还原计划或可能找到的还原计划并非最优的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897752](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897752)。 |
|备份/还原/附加/分离数据库|修复了“附加数据库”向导不显示已重命名的辅助文件的问题。 现在，会显示文件并添加了相关注释（例如“找不到”）。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/32897434](https://feedback.azure.com/forums/908035/suggestions/32897434)。 |
|复制数据库向导|生成脚本/传输/复制数据库向导尝试创建一个表，其内存中表不强制将 ansi_padding 设置为 on。|
|复制数据库向导|SQL Server 2017 和 SQL Server 2019 上的传输数据库任务/复制数据库向导中断。|""
|复制数据库向导|先创建生成脚本/传输/复制数据库向导脚本表，再创建关联的外部数据源。|
|连接对话框|启用了通过按 DEL 键从以前的用户名列表中删除用户名。 有关详细信息，请参阅[允许从 SSMS 登录窗口删除用户](https://feedback.azure.com/forums/908035/suggestions/32897632)。|
|DAC 导入向导|修复了使用 Azure Active Directory (Azure AD) 连接时 DAC 导入向导失效的问题。|
|数据分类|修复了在数据分类窗格中保存分类时，其他数据分类窗格在其他数据库中打开的问题。|
|数据层应用程序向导|解决了由于对服务器的访问受限（例如，无法访问同一服务器上的所有数据库），用户无法导入数据层应用程序 (.dacpac) 的问题。|
|数据层应用程序向导|解决了当许多数据库碰巧托管在同一 Azure SQL 服务器上时导致导入速度非常缓慢的问题。|
|外部表|在模板、SMO、IntelliSense 和属性网格中添加了对 Rejected_Row_Location 的支持。|
|平面文件导入向导|修复了“导入平面文件向导”未正确处理双引号（转义）的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/32897998](https://feedback.azure.com/forums/908035/suggestions/32897998)。 |
|平面文件导入向导|修复了（在使用不同浮点分隔符的区域设置上）错误处理浮点类型的问题。|
|平面文件导入向导|修复了与值为 0 或 1 时的导入位数相关的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898535](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898535)。 |
|平面文件导入向导|修复了浮点  数据类型输入为 null  问题。|
|平面文件导入向导|修复了导入向导无法处理负十进制值的问题。|
|平面文件导入向导|修复了向导无法从单列 CSV 文件导入的问题。|
|平面文件导入向导|修复了已存在目标表时平面文件导入不允许更改目标表的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32896186](https://feedback.azure.com/forums/908035-sql-server/suggestions/32896186)。 |
|帮助查看器|改进了有关使用联机/脱机模式的逻辑（仍然有一些问题需要解决）。|
|帮助查看器|修复了使用联机/脱机设置的“查看帮助”。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897791](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897791)。 |
|高可用性和灾难恢复 (HADR)<BR> 可用性组 (AG)|修复了“故障转移可用性组”向导中角色始终显示为“正在解析”的问题。|
|高可用性和灾难恢复 (HADR)<BR> 可用性组 (AG)|修复了 SSMS 在“可用性组仪表板”中显示的警告遭截断的问题。|
|Integration Services (IS)|修复了同一台计算机上安装有 SQL Server 2019 和 SSMS 18.0 时部署向导无法连接到 SQL Server 的 SxS 问题。|
|Integration Services (IS)|修复了在设计维护计划时，不能编辑维护计划任务的问题。|
|Integration Services (IS)|修复了重命名部署下的项目时部署向导卡住的问题。|
|Integration Services (IS)|启用了 Azure-SSIS IR 计划功能中的环境设置。|
|Integration Services (IS)|修复了当客户帐户属于多个租户时 SSIS Integration Runtime 创建向导停止响应的问题。|
|作业活动监视器|修复了使用作业活动监视器（通过筛选器）时的故障。|
|“对象资源管理器”|修复了 SSMS 在 OE 中尝试展开“管理”节点（错误配置 DataCollector）时引发“对象无法从 DBNull 转换为其他类型”异常的问题。|
|“对象资源管理器”|修复了在调用“Edit Top N…”之前 OE 未转义引号会导致设计混淆的问题。|
|“对象资源管理器”|修复了无法从 Azure 存储树中启动“导入数据层应用程序”向导的问题。|
|“对象资源管理器”|修复了 TLS/SSL 复选框的状态不持久的“数据库邮件配置”问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32895541](https://feedback.azure.com/forums/908035-sql-server/suggestions/32895541)。 |
|“对象资源管理器”|修复了尝试使用 is_auto_update_stats_async_on 还原数据库时 SSMS 灰显选项并关闭现有连接的问题。|
|“对象资源管理器”|修复了在 OE 中的节点上右键单击的问题（例如，右键单击“表”并希望执行某个操作，比如通过转到“筛选器”>“筛选器设置”筛选表，但筛选器设置窗体可能未显示在 SSMS 当前处于活动状态屏幕上）。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/34284106](https://feedback.azure.com/forums/908035-sql-server/suggestions/34284106)。 |
|“对象资源管理器”|修复了尝试重命名对象时 Delete 键在对象资源管理器中不工作这一长期未解决的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/33073510](https://feedback.azure.com/forums/908035-sql-server/suggestions/33073510)、[https://feedback.azure.com/forums/908035/suggestions/32910247](https://feedback.azure.com/forums/908035/suggestions/32910247) 和其他重复项。|
|“对象资源管理器”|显示现有数据库文件的属性时，大小显示在列“大小 (MB)”而不是“初始大小 (MB)”下，这是创建新数据库时显示的内容。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32629024](https://feedback.azure.com/forums/908035-sql-server/suggestions/32629024)。 |
|“对象资源管理器”|由于当前版本的 SSMS 中不支持此类表，已禁用“图形表”的“设计”上下文菜单项。|
|“对象资源管理器”|修复了高 DPI 监视器不呈现“新建作业计划”对话框的问题。 有关详细信息，请参阅 [https://feedback.azure.com/admin/v3/suggestions/35541262](https://feedback.azure.com/admin/v3/suggestions/35541262)。 |
|“对象资源管理器”|修复了在对象资源管理器详细信息中显示数据库大小（“大小 (MB)”）的问题并改进了显示方式：仅 2 位小数并使用千位分隔符格式。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/34379308](https://feedback.azure.com/forums/908035/suggestions/34379308)。|
|“对象资源管理器”|修复了创建“空间索引”因“要完成此操作，请设置 PartitionScheme 属性”等错误而失败的问题。|
|“对象资源管理器”|在对象资源管理器中进行了一些小的性能改进，以尽可能避免发出额外的查询。|
|“对象资源管理器”|扩展了重命名所有架构对象的数据库时请求确认的逻辑（可配置该设置）。|
|“对象资源管理器”|在对象资源管理器筛选中添加了正确的转义。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/36678803](https://feedback.azure.com/forums/908035/suggestions/36678803)。 |
|“对象资源管理器”|修复/改进了对象资源管理器详细信息中的视图，以显示带适当分隔符的数字。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/32900944](https://feedback.azure.com/forums/908035/suggestions/32900944)。 |
|“对象资源管理器”|修复了连接到 SQL Express 时“表”节点上的上下文菜单（以前缺少“新建”浮出控件、图形表未正确列出，并且缺少由系统控制版本的表）。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/37245529](https://feedback.azure.com/forums/908035/suggestions/37245529)。 |
|对象脚本|整体性能改进 - 生成 WideWorldImporters 的脚本与 SSMS 17.7 相比需要一半时间。|
|对象脚本|当编写对象脚本时，将省略具有默认值的 DB 作用域配置。|
|对象脚本|编写脚本时不生成动态 T-SQL。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898391](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898391)。 |
|对象脚本|在 SQL Server 2016 及更早版本中对表编写脚本时，省略图形语法“as edge”和“as node”。|
|对象脚本|修复了使用 Azure AD 和 MFA 连接到 Azure SQL 数据库时数据库对象脚本编写失败的问题。|
|对象脚本|修复了在 Azure SQL 数据库上使用 GEOMETRY_AUTO_GRID/GEOGRAPHY_AUTO_GRID 编写空间索引脚本时引发错误的问题。|
|对象脚本|修复了即使“对象资源管理器”脚本设置已设为与源匹配也仍然导致（Azure SQL 数据库的）数据库脚本始终以本地 SQL 为目标的问题。|
|对象脚本|修复了尝试对 SQL DW 数据库中的表（包含生成错误 T-SQL 语句的聚集索引和非聚集索引）编写脚本时出现的问题。|
|对象脚本|修复了尝试对 SQL DW 数据库中的表（包含生成错误 T-SQL 语句[重复语句]的“聚集列存储索引”和“聚集索引”）编写脚本时出现的问题。|
|对象脚本|修复了没有范围值（SQL DW 数据库）的分区表脚本的编写问题。|
|对象脚本|修复了用户无法编写审核/审核规范 SERVER_PERMISSION_CHANGE_GROUP 脚本的问题。|
|对象脚本|修复了用户无法对 SQL DW 中的统计信息编写脚本的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897296](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897296)。 |
|对象脚本|修复了在“出错时继续编写脚本”设置为 false 时，“生成脚本向导”显示具有脚本编辑错误的错误表的问题。|
|对象脚本|改进了 SQL Server 2019 上的脚本生成。|
|探查器|将“聚合表重写查询”事件添加到了探查器事件。|
|查询数据存储|修复了可能引发“DocumentFrame (SQLEditors)”异常的问题。|
|查询数据存储|修复了尝试在内置查询存储报表中设置自定义时间间隔时，用户无法对开始/结束时间间隔选择 AM 或 PM 的问题。|
|结果网格|修复了导致高对比度模式（所选行号不可见）的问题。|
|结果网格|修复了单击网格时导致出现“索引超出范围”异常的问题。|
|结果网格|解决了网格结果背景色被忽略的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/32895916](https://feedback.azure.com/forums/908035/suggestions/32895916)。 |
|显示计划|有多个线程时，新的内存授予运算符属性显示错误。|
|显示计划|在实际执行 xml 计划的 RunTimeCountersPerThread 中添加以下 4 个属性：HpcRowCount（由 hpc  设备处理的行数）、HpcKernelElapsedUs（正在使用的内核执行的运行时间等待）、HpcHostToDeviceBytes（已从主机传输到设备的字节数）和 HpcDeviceToHostBytes （已从设备传输到主机的字节数）。|
|显示计划|修复了类似计划节点在错误位置突出显示的问题。|
|SMO|修复了 SMO/ServerConnection 未正确处理基于 SqlCredential 的连接的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/33698941](https://feedback.azure.com/forums/908035-sql-server/suggestions/33698941)。 |
|SMO|修复了使用 SMO 编写的应用程序在多个线程上试图枚举来自同一服务器的数据库时遇到错误（即使在每个线程上使用单独的 SqlConnection 实例）的问题。|
|SMO|修复了从外部表传输的性能回归问题。|
|SMO|修复了导致 SMO 在定位 Azure 时泄露 SqlConnection 实例的 ServerConnection 线程安全问题。|
|SMO|修复了在尝试还原名称中带有大括号的数据库时导致 StringBuilder.FormatError 的问题。|
|SMO|修复了 SMO 中的 Azure 数据库默认为所有字符串比较应用不区分大小写的排序规则，而不是针对数据库使用指定的排序规则的问题。|
|SSMS 编辑器|修复了还原默认颜色的“SQL 系统表”意外将颜色设置为暗黄绿色，而不是默认的绿色，使其难以在白色背景上读取的问题。 有关详细信息，请参阅[还原 SQL 系统表的错误默认颜色](https://feedback.azure.com/forums/908035-sql-server/suggestions/32896906)。 此问题在 SSMS 的非英文版本上仍然存在。|
|SSMS 编辑器|修复了在使用 Azure Active Directory (Azure AD) 身份验证连接到 Azure SQL DW 时 IntelliSense 不工作的问题。|
|SSMS 编辑器|修复了用户缺少对 master 数据库的访问权限时，Azure 中的 IntelliSense 问题  。|
|SSMS 编辑器|修复了目标数据库排序规则区分大小写时损坏的用于创建“时态表”的代码片段。|
|SSMS 编辑器|新 TRANSLATE 函数当前由 intellisense 识别。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898430](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898430)。 |
|SSMS 编辑器|针对 FORMAT 内置函数改进了 IntelliSense。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898676](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898676)。 |
|SSMS 编辑器|LAG 和 LEAD 当前被识别为内置函数。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898757](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898757)。 |
|SSMS 编辑器|修复了使用“ALTER TABLE...ADD CONSTRAINT...WITH(ONLINE=ON)”时 IntelliSense 发出警告的问题。|
|SSMS 编辑器|修复了多个系统视图和表值函数未正确着色的问题。|
|SSMS 编辑器|修复了单击“编辑器”选项卡可能导致该选项卡关闭而非获得焦点的问题。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/37291114](https://feedback.azure.com/forums/908035/suggestions/37291114)。 |
|SSMS 选项|修复了“工具” > “选项” > “SQL Server 对象资源管理器” > “命令”页未正确调整大小的问题     。|
|SSMS 选项|SSMS 现在默认禁用在 XMLA 编辑器中自动下载 DTD - XMLA 脚本编辑器（使用 xml 语言服务）现在因可能存在恶意 xmla 文件而默认阻止自动下载 DTD。 这是通过禁用“工具”   > “选项”   > “环境”   > “文本编辑器”   > “XML”   > “其他”  中的“自动下载 DTD 和架构”设置来控制的。|
|SSMS 选项|将 Ctrl+D 还原成了 SSMS 早期版本中那样的快捷方式  。 有关详细信息，请参阅 [https://feedback.azure.com/forums/908035/suggestions/35544754](https://feedback.azure.com/forums/908035/suggestions/35544754)。 |
|表设计器|修复了“编辑 200 行”中的故障。|
|表设计器|修复了在连接到 Azure SQL 数据库时设计器允许添加表的问题。|
|漏洞评估|修复了扫描结果加载不正确的问题。|
|XEvent|已添加两个列“action_name”和“class_type_desc”，将操作 ID 和类类型字段显示为可读字符串。|
|XEvent|已删除 1000000 个事件的事件 XEvent 查看器上限。|
|XEvent 探查器|修复了连接到 96 核 SQL Server 时无法启动 XEvent 探查器的问题。|
|XEvent 查看器|修复了使用“扩展事件工具栏选项”对事件分组时 XEvent 查看器出现故障的问题。|

#### <a name="deprecated-and-removed-features-in-180"></a>18.0 中已弃用和已删除的功能

下面列出了 SSMS 版本 18.0 中弃用和删除的功能。

- T-SQL 调试程序
- 数据库关系图
- SSMS 不再安装以下工具：
  - OSQL.EXE
  - DReplay.exe
  - SQLdiag.exe
  - SSBDiagnose.exe
  - bcp.exe
  - sqlcmd.exe
- Configuration Manager 工具：
  - SQL Server 配置管理器和报表服务器配置管理器不再是 SSMS 安装程序的一部分。
- DMF 标准策略
  - 策略不再随 SSMS 安装。 已将它们迁移到 Git。 如果用户需要策略，则能够参与并下载/安装它们。
- SSMS 命令行选项 - P 已删除
  - 出于安全考虑，已删除命令行上指定明文密码的选项。
- 删除了生成脚本并发布到 Web 服务的功能
  - 从 SSMS UI 中删除了此已弃用的功能。
- 在对象资源管理器中删除了“维护 > 旧版”节点。
  - 将不再能够访问陈旧的“数据库维护计划”和“SQL Mail”节点。 新式“数据库邮件”和“维护计划”节点继续照常工作。

#### <a name="known-issues-180"></a>已知问题 (18.0)

- 安装版本 18.0 时可能会遇到以下问题：无法运行 SQL Server Management Studio。 如果遇到此问题，请按照 [SSMS2018 - 已安装，但不运行](https://feedback.azure.com/forums/908035-sql-server/suggestions/37502512-ssms2018-installed-but-will-not-run)一文中的步骤进行操作。

- 对于在网格、文本或文件中显示的 SSMS 结果中可以查看的数据大小，存在限制

- 在多个查询窗口之间切换时会出现重绘问题。 请参阅 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/37474042)，了解更多详细信息。 此问题的解决方法是在“工具”>“选项”下禁用硬件加速。

### <a name="1791"></a>17.9.1

![下载](media/download-icon.png) [下载 SSMS 17.9.1](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)

- 版本号：17.9.1
- 生成号：14.0.17289.0
- 发行日期：2018 年 11 月 21 日

#### <a name="bug-fixes-in-1791"></a>17.9.1 中的 bug 修复

- 修复了在使用带有 SQL 查询编辑器的“Azure Active Directory - 通用且具有 MFA 支持”身份验证时，用户可能会遇到关闭连接并在每次查询调用时重新打开连接的问题。 此类关闭连接的副作用包括意外删除全局临时表，有时还会向连接提供新的 SPID。
- 修复了长期未解决的问题，即还原计划无法查找还原计划，或在某些情况下生成低效的还原计划。
- 修复了“导入数据层应用程序”向导中的问题，该问题在连接到 Azure SQL 数据库时可能出现错误。

> [!NOTE]
> 如果安装在以下平台中，非英语本地化版本的 SSMS 17.x 需要 [KB 2862966 安全更新程序包](https://support.microsoft.com/kb/2862966)：Windows 8、Windows 7、Windows Server 2012 和 Windows Server 2008 R2。

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x804)| [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x404)| [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)| [法语](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40c)| [德语](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x407)| [意大利语](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x410)| [日语](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x411)| [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x412)| [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x416)| [俄语](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x419)| [西班牙语](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40a)

#### <a name="uninstall-and-reinstall-ssms-17x"></a>卸载并重装 SSMS 17.x

如果安装 SSMS 时遇到问题，并且标准的卸载和重装无法解决问题，可以先尝试下[修复](https://support.microsoft.com/help/4028054/windows-10-repair-or-remove-programs) Visual Studio 2015 IsoShell。 如果修复 Visual Studio 2015 IsoShell 不能解决此问题，可以尝试下面的步骤来解决许多偶然的问题：

1. 像卸载任意应用程序那样卸载 SSMS（使用“应用和功能”、“程序和功能”，具体取决于 Windows 版本）   。

2. 通过提升的 cmd 提示符卸载 Visual Studio 2015 IsoShell  ：

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```

    ```vs_isoshell.exe /Uninstall /Force /PromptRestart```

3. 像卸载任何应用程序那样卸载 Microsoft Visual C++ 2015 可再发行组件。 如果计算机上有 x86 和 x64，则卸载它们。

4. 通过提升的 cmd 提示符重装 Visual Studio 2015 IsoShell  ：  

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```  

    ```vs_isoshell.exe /PromptRestart```

5. 重装 SSMS。

6. 如果当前不是最新版本的话，则升级到[最新版本的 Visual C++ 2015 可再发行组件](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)。

### <a name="1653"></a>16.5.3

![下载](media/download-icon.png) [下载 SSMS 16.5.3](https://go.microsoft.com/fwlink/?LinkID=840946)

- 版本号：16.5.3  
- 生成号：13.0.16106.4  
- 发行日期：2017 年 1 月 30 日

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x804)| [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x404)| [英语（美国）](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x409)| [法语](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40c)| [德语](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x407)| [意大利语](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x410)| [日语](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x411)| [朝鲜语](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x412)| [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x416)| [俄语](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x419)| [西班牙语](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40a)

#### <a name="bug-fixes-in-1653"></a>16.5.3 中的 bug 修复

- 修复了 SSMS 16.5.2 中引入的问题，即当表具有多个稀疏列时，导致“表”节点扩展。

- 用户可以部署包含 OData 连接管理器的 SSIS 包，这些包连接到 SSIS 目录的 Microsoft Dynamics AX/CRM Online 资源。 有关详细信息，请参阅 [OData 连接管理器](../integration-services/connection-manager/odata-connection-manager.md)。

- 为现有表配置 Always Encrypted 功能失败，在不相关的对象上出错。 [连接 ID 3103181](https://connect.microsoft.com/SQLServer/feedback/details/3103181/setting-up-always-encrypted-on-an-existing-table-fails-with-errors-on-unrelated-objects)

- 为现有数据库配置 Always Encrypted 功能时，多个架构无法正常运行。 [连接 ID 3109591](https://connect.microsoft.com/SQLServer/feedback/details/3109591/sql-server-2016-always-encrypted-against-existing-database-with-multiple-schemas-doesnt-work)

- 由于数据库包含引用系统视图的视图，Always Encrypted、“已加密列”向导失败。 [连接 ID 3111925](https://connect.microsoft.com/SQLServer/feedback/details/3111925/sql-server-2016-always-encrypted-encrypted-column-wizard-failed-task-failed-due-to-following-error-cannot-save-package-to-file-the-model-has-build-blocking-errors)

- 使用 Always Encrypted 功能进行加密时，错误处理加密后刷新模块出现错误。

- “打开最近的文件”菜单不显示最近保存的文件。  [连接 ID 3113288](https://connect.microsoft.com/SQLServer/feedback/details/3113288/ssms-2016-open-recent-menu-doesnt-show-recently-saved-files)

- 右键单击表的索引（通过远程 (Internet) 连接）时，SSMS 运行缓慢。 [连接 ID 3114074](https://connect.microsoft.com/SQLServer/feedback/details/3114074/ssms-slow-when-right-clicking-an-index-for-a-table-over-a-remote-internet-connection)

- 解决了 SQL 设计器滚动条的问题。 [连接 ID 3114856](https://connect.microsoft.com/SQLServer/feedback/details/3114856/bug-in-scrollbar-on-sql-desginer-in-ssms-2016)

- 表的上下文菜单暂时停止响应

- SSMS 偶尔在活动监视器中引发异常和崩溃。 [连接 ID 697527](https://connect.microsoft.com/SQLServer/feedback/details/697527/)

- SSMS 2016 崩溃，显示错误“由于在 IP 71AF8579 (71AE0000) 的 .NET 运行时出现内部错误，进程终止，退出代码 80131506”

## <a name="additional-downloads"></a>其他下载

有关所有 SQL Server Management Studio 下载的列表，请搜索 [Microsoft 下载中心](https://www.microsoft.com/download/search.aspx?q=sql%20server%20management%20studio&p=0&r=10&t=&s=Relevancy~Descending)。  
  
有关最新版本的 SQL Server Management Studio，请参阅 [下载 SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)。