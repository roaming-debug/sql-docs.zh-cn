---
title: SQL Server 2017 的新增功能 | Microsoft Docs
description: 了解 SQL Server 2017 的新增功能，这些新增功能为 Linux 和基于 Linux 的 Docker 容器以及 Windows 带来了SQL Server 的强大功能。
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
ms.assetid: 0b57f375-9242-4bb2-9d4b-c560d5a93524
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-2017'
ms.openlocfilehash: e9cebd9b809cc6033a293c2736f774dda62e14a8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461348"
---
# <a name="whats-new-in-sql-server-2017"></a>SQL Server 2017 的新增功能
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]
SQL Server 2017 跨出了重要的一步，它力求通过将 SQL Server 的强大功能引入 Linux、基于 Linux 的 Docker 容器和 Windows，使用户可以在 SQL Server 平台上选择开发语言、数据类型、本地开发或云端开发，以及操作系统开发。 本主题概括了特定功能区域的新增功能，并包括指向其他详细信息的链接。 有关 Linux 上的 SQL Server 的相关详细信息，请参阅 [Linux 上的 SQL Server 文档](/sql/linux/)

[![从评估中心下载](../includes/media/download2.png)](https://go.microsoft.com/fwlink/?LinkID=829477) **试用：** [下载 SQL Server 2017 版本 - 2017 年 10 月](https://go.microsoft.com/fwlink/?LinkID=829477)。

> [!NOTE]
> 除了以下更改外，在正式版发布后会定期发布累积更新。 这些累积更新提供许多改进和修复。 有关最新 CU 版本的信息，请参阅 [SQL Server 2017 累积更新](https://aka.ms/sql2017cu)。

## <a name="sql-server-2017-database-engine"></a>SQL Server 2017 数据库引擎

SQL Server 2017 包含许多新的数据库引擎功能、增强功能和性能改进。 
- 现可将 CLR 程序集添加到受信任的程序集列表，作为 CTP 2.0 中介绍的 `clr strict security` 功能的变通方法  。 添加 [sp_add_trusted_assembly](../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)、[sp_drop_trusted_assembly](../relational-databases/system-stored-procedures/sys-sp-drop-trusted-assembly-transact-sql.md) 和 [sys.trusted_asssemblies](../relational-databases/system-catalog-views/sys-trusted-assemblies-transact-sql.md) 以支持受信任的程序集列表 (RC1)。  
- 可恢复的联机索引重新生成  可从发生故障（例如到副本的故障转移或磁盘空间不足）后联机索引重新生成操作停止处恢复该操作，或暂停并稍后恢复联机索引重新生成操作。 请参阅 [ALTER INDEX](../t-sql/statements/alter-index-transact-sql.md) 和[联机索引操作准则](../relational-databases/indexes/guidelines-for-online-index-operations.md)。 (CTP 2.0)
- 如果服务器意外重启或故障转移到辅助服务器，ALTER DATABASE SCOPED CONFIGURATION 的“IDENTITY_CACHE”  选项可使用户避免标识列值的差值。 请参阅 [ALTER DATABASE SCOPED CONFIGURATION](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)。 (CTP 2.0)
- 新一代的查询处理改进，将对应用程序工作负荷的运行时状况采用优化策略。 对于这款适应性查询处理功能系列初版，我们进行了 3 项新的改进：批处理模式自适应联接、批处理模式内存授予反馈，以及针对多语句表值函数的交错执行     。  请参阅 [SQL 数据库中的智能查询处理](../relational-databases/performance/intelligent-query-processing.md)。
- 自动数据库优化  提供对潜在查询性能问题的深入了解、提出建议解决方案并自动解决已标识的问题。 请参阅[自动优化](../relational-databases/automatic-tuning/automatic-tuning.md)。 (CTP 2.0)
- 用于建模多对多关系的新图形数据库功能  包括用于创建节点和边界表的新 [CREATE TABLE](../t-sql/statements/create-table-sql-graph.md) 语法和用于查询的关键字 [MATCH](../t-sql/queries/match-sql-graph.md)。 请参阅[使用 SQL Server 2017 进行图形处理](../relational-databases/graphs/sql-graph-overview.md)。 (CTP 2.0)
- 默认情况下，启用名为 `clr strict security` 的 sp_configure 选项，以增强 CLR 程序集的安全性。 请参阅 [CLR 严格安全性](../database-engine/configure-windows/clr-strict-security.md)。 (CTP 2.0)
- 安装程序现在支持最多将每个文件的初始 tempdb 文件大小指定为 256 GB (262,144 MB)/文件  ；如果文件大小设置为大于 1 GB 且未启用 IFI，则会出现警告。 (CTP 2.0)
- [sys.dm_db_file_space_usage](../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md) 中的 **modified_extent_page_count** 列跟踪每个数据库文件中的差异更改，从而启用基于数据库中已更改页面百分比执行差异备份或完全备份的智能备份解决方案。 (CTP 2.0)
- [SELECT INTO](../t-sql/queries/select-into-clause-transact-sql.md) T-SQL 语法现支持使用 ON  关键字将表加载到用户默认文件组以外的文件组。 (CTP 2.0)
- 现在，在属于 AlwaysOn 可用性组  的全部数据库（包括属于同一实例的数据库）中支持跨数据库事务。 请参阅 [事务 - AlwaysOn 可用性组和数据库镜像](../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md) (CTP 2.0)
- 新的“可用性组”  功能包括无群集读取扩展支持、最小副本提交可用性组设置和 Windows-Linux 跨操作系统迁移和测试。 (CTP 1.3)
- 新的动态管理视图：
    - [sys.dm_db_log_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md) 公开摘要级别特性和有关事务日志文件的信息，这对于监视事务日志的运行状况很有用。 (CTP 2.1)
    - [sys.dm_tran_version_store_space_usage](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-space-usage.md) 跟踪每个数据库的版本存储使用情况，有助于根据每个数据库的版本存储使用情况主动规划 tempdb 大小。 (CTP 2.0)
    - [sys.dm_db_log_info](../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md) 公开 VLF 信息以监视、警告和避免潜在的事务日志问题。 (CTP 2.0)
    - [sys.dm_db_stats_histogram](../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) 是新的动态管理视图，用于检查统计信息。 (CTP 1.3)
    - sys.dm_os_host_info  为 Windows 和 Linux 提供操作系统信息。 (CTP 1.0)
- “数据库优化顾问”(DTA)  具有其他选项和改进的性能。 (CTP 1.2)
- 内存中增强功能  包括支持内存优化表中的计算列、完全支持本机编译的模块中的 JSON 函数，以及本机编译的模块中的 CROSS APPLY 运算符。 (CTP 1.1)
- 新的字符串函数  是 CONCAT_WS、TRANSLATE 和 TRIM，而 STRING_AGG 函数现在支持 WITHIN GROUP。 (CTP 1.1)
- 对于 CSV 和 Azure Blob 文件，可使用新的批处理访问选项  BULK INSERT 和 OPENROWSET(BULK...)）。 (CTP 1.1)
- 内存优化对象增强功能  包括 sp_spaceused、消除内存优化表的 8 个索引限制、内存优化表的 sp_rename 和本机编译的 T-SQL 模块，以及适用于本机编译的 T-SQL 模块的 CASE 和 TOP (N) WITH TIES。 现在可在 Azure 存储中存储、备份和还原内存优化文件组文件。 (CTP 1.0)
- “DATABASE SCOPED CREDENTIAL”  是一个新的安全对象类，支持 CONTROL、ALTER、REFERENCES、TAKE OWNERSHIP 和 VIEW DEFINITION 权限。 现在，ADMINISTER DATABASE BULK OPERATIONS 在 sys.fn_builtin_permissions 中可见。 (CTP 1.0)
- 已添加数据库 **“COMPATIBILITY_LEVEL 150”** 。 (CTP 1.0)。  

## <a name="sql-server-2017-integration-services-ssis"></a>SQL Server 2017 Integration Services (SSIS)
- SSIS 中的新 Scale Out  功能具有以下新功能和更改的功能。 有关详细信息，请参阅 [SQL Server 2017 Integration Services 中的新增功能](~/integration-services/what-s-new-in-integration-services-in-sql-server-2017.md)。 (RC1)
    -   Scale Out 主要角色现在支持高可用性。
    -   改进了 Scale Out 辅助角色中执行日志的故障转移处理。
    -   存储过程 [catalog].[create_execution]  的参数 runincluster  重命名为 runinscaleout  ，以保持一致性和可读性。
    -   SSIS 目录具有新的全局属性，用于指定执行 SSIS 包的默认模式。
- 在新“Scale Out for SSIS”  功能中，现在可在触发执行时使用 Use32BitRuntime  参数。 (CTP 2.1)
- SQL Server 2017 Integration Services (SSIS) 现在支持 Linux 适用的 SQL Server  ，并且新包允许在 Linux 上从命令行运行 SSIS 包。 有关详细信息，请参阅[宣布 SSIS 支持 Linux 的博客文章](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/)。 (CTP 2.1)
- 新的“Scale Out for SSIS”  功能使在多台计算机上运行 SSIS 更为轻松。 请参阅 [Integration Services Scale Out](~/integration-services/scale-out/integration-services-ssis-scale-out.md)。(CTP 1.0)
- OData 源和 OData 连接管理器现支持连接到 Microsoft Dynamics AX Online 和 Microsoft Dynamics CRM Online 的 OData 源。 (CTP 1.0)

有关详细信息，请参阅 [SQL Server 2017 Integration Services 中的新增功能](~/integration-services/what-s-new-in-integration-services-in-sql-server-2017.md)。

## <a name="sql-server-2017-master-data-services-mds"></a>SQL Server 2017 Master Data Services (MDS)
- 从 SQL Server 2012、SQL Server 2014 和 SQL Server 2016 升级到 SQL Server 2017 Master Data Services 时，体验和性能有所改进。 
- 现在可以在 Web 应用程序的“资源管理器”  页中查看实体、集合和层次结构的排序列表。
- 提升了使用暂存存储过程暂存数百万条记录时的性能。
- 提升了在“管理组”  页中展开“实体”  文件夹以分配模型权限时的性能。 “管理组”  页位于 Web 应用程序的“安全性”  部分中。 有关性能改进的详细信息，请参阅 [https://support.microsoft.com/help/4023865?preview](https://support.microsoft.com/help/4023865?preview)。 若要详细了解如何分配权限，请参阅[分配模型对象权限 (Master Data Services)](../master-data-services/assign-model-object-permissions-master-data-services.md)。

## <a name="sql-server-2017-analysis-services-ssas"></a>SQL Server 2017 Analysis Services (SSAS) 
SQL Server Analysis Services 2017 引入了许多可用于表格模型的增强功能。 其中包括：
- 作为 Analysis Services 默认安装选项的表格模式。 (CTP 2.0)
- 用于保护表格模型元数据的对象级安全性。 (CTP 2.0)
- 可基于日期字段轻松创建关系的日期关系。 (CTP 2.0)
- 新的“获取数据”  (Power Query) 数据源，以及现有的 DirectQuery 数据源支持 M 查询。 (CTP 2.0) 
- 用于 SSDT 的 DAX 编辑器。 (CTP 2.0)
- 编码提示，一种用于优化大型内存中表格模型的数据刷新的高级功能。 (CTP 1.3)
- 支持针对表格模型的 1400 兼容级别  。 若要新建或将现有表格模型项目升级到 1400 兼容级别，请下载并安装 [SQL Server Data Tools (SSDT) 17.0 RC2](https://go.microsoft.com/fwlink?LinkId=837939)。 (CTP 1.1)
- 1400 兼容级别的表格模型的新式获取数据  体验。 请参阅 [Analysis Services 团队博客](/archive/blogs/analysisservices/introducing-a-modern-get-data-experience-for-sql-server-vnext-on-windows-ctp-1-1-for-analysis-services)。 (CTP 1.1)
- Hide Members  属性可隐藏不规则层次结构中的空白成员。 (CTP 1.1)
- 新的详细信息行  最终用户操作可显示聚合信息的详细信息  。 [SELECTCOLUMNS](/dax/selectcolumns-function-dax) 和 DETAILROWS  函数用于创建详细信息行表达式。 (CTP 1.1)
- DAX IN  运算符可指定多个值。 (CTP 1.1)

有关详细信息，请参阅 [SQL Server Analysis Services 中的新增功能](/analysis-services/what-s-new-in-analysis-services)。

## <a name="sql-server-2017-reporting-services-ssrs"></a>SQL Server 2017 Reporting Services (SSRS)
不可再通过 SQL Server 安装程序安装 SQL Server Reporting Services。 请转到 Microsoft 下载中心以[下载 Microsoft SQL Server 2017 Reporting Services](https://www.microsoft.com/download/details.aspx?id=55252)。 
- 注释现在可用于报表，以增加视角并与他人协作。 还可包含带有批注的附件。
- 在最新版本的报表生成器和 SQL Server Data Tools 中，通过在查询设计器中拖放所需的字段，可针对支持的 SQL Server Analysis Services 表格数据模型创建本机 DAX 查询。 请参阅 [Reporting Services 博客](/archive/blogs/sqlrsteamblog/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools)。
- 为了实现现代应用程序开发以及自定义，SSRS 现在支持完全符合 OpenAPI 规范的 RESTful API。 现在可在 [swaggerhub](https://app.swaggerhub.com/apis/microsoft-rs/SSRS/2.0) 上找到完整的 API 规范和文档。

有关详细信息，请参阅 [SQL Server Reporting Services (SSRS) 中的新增功能](~/reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)。

## <a name="machine-learning-in-sql-server-2017"></a>SQL Server 2017 中的机器学习 

SQL Server R 服务已重命名为 SQL Server 机器学习服务，以反映对除 R 语言外的 Python 的支持  。 可以使用机器学习服务（数据库内）在 SQL Server 中运行 R 或 Python 脚本，或者安装 Microsoft 机器学习服务器（独立）来部署和使用不需要 SQL Server 的 R 和 Python 模型。  

SQL Server 开发人员现在可访问开放源代码生态系统中提供的大量 Python ML 和 AI 库，以及 Microsoft 的最新创新：

- **revoscalepy** - 此等效于 RevoScaleR 的 Python 包含用于线性回归和逻辑回归、决策树、提升树和随机林的并行算法，以及一组丰富的用于数据转换和数据移动、远程计算上下文和数据源的 API。
- **microsoftml** - 这是一个先进的机器学习算法和 Python 绑定转换包，其中包含深层神经网络、快速决策树和决策树以及用于线性回归和逻辑回归的优化算法。 用户还可获得预定型模型，这些模型基于用于图像提取或情感分析的 ResNet 模型。
- **使用 T-SQL 进行 Python 操作** - 使用存储过程 `sp_execute_external_script` 轻松部署 Python 代码。 通过将数据从 SQL 流式传输到 Python 进程并使用 MPI 环并行化来获得出色性能。
- **SQL Server 计算上下文中的 Python** - 数据科学家和开发人员可以从其开发环境远程执行 Python 代码，以便在不移动数据的情况下浏览数据和开发模型。
- **本机计分** - Transact-SQL 中的 PREDICT 函数可用于执行 SQL Server 2017 的任何实例中的计分（即使未安装 R）。 只需使用一个受支持的 RevoScaleR 和 revoscalepy 算法训练该模型，并将该模型保存为全新的二进制紧凑格式。
- **程序包管理** - T-SQL 现在支持 CREATE EXTERNAL LIBRARY 语句，使 DBA 更好地管理 R 程序包。 使用角色控制专用或共享包访问权限，在数据库中存储 R 包并在用户之间进行共享。
- **性能改进** - 存储过程 `sp_execute_external_script` 已经过优化，支持列存储数据的批处理模式执行。


有关详细信息，请参阅 [SQL Server 机器学习服务中的新增功能](~/machine-learning/what-s-new-in-sql-server-machine-learning-services.md)。

## <a name="next-steps"></a>后续步骤
- 请参阅 [SQL Server 2017 发行说明](sql-server-2017-release-notes.md)。
- 了解 [What's new for SQL Server 2017 on Linux](../linux/sql-server-linux-whats-new.md)（Linux 上 SQL Server 2017 的新增功能）。
- 查找 [SQL Server 2016 中的新增功能](what-s-new-in-sql-server-2016.md)。

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png)