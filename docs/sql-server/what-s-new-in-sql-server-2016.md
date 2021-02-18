---
title: SQL Server 2016 中的新增功能
description: 了解新的 SQL Server 2016 安全功能、查询功能、Hadoop 和云集成、R 分析等。
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
keywords:
- 新建 sql server
helpviewer_keywords:
- new features [SQL Server]
- SQL Server, what's new
- SQL Server 2008 what's new
- what's new [SQL Server]
ms.assetid: 6a428023-e3cc-4626-a88a-4c13ccbd7db0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c9313877cac9c4e22fdaa4234fc40f0701649934
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271734"
---
# <a name="whats-new-in-sql-server-2016"></a>SQL Server 2016 中的新增功能
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]    
 通过 SQL Server 2016，可以使用可缩放的混合数据库平台生成任务关键型智能应用程序。此平台内置了需要的所有功能，包括内存中性能、高级安全性和数据库内分析。 SQL Server 2016 版本新增了安全功能、查询功能、Hadoop 和云集成、R 分析等功能，以及许多改进和增强功能。 

此页面收录了摘要信息和链接，可方便读者详细了解 SQL Server 2016，以及每个 SQL Server 组件的最近更新。 

![SQL Server 2016](../sql-server/media/sql-server-2016.png)

 **立即试用 SQL Server！** 
- 下载免费的 [SQL Server 最新版本！](https://www.microsoft.com/sql-server/sql-server-downloads) 。
- 下载最新版 [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)。 
- 已经拥有 Azure 帐户？ 加速[已安装有 SQL Server 2016 的虚拟机](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)。

## <a name="sql-server-2016-database-engine"></a>SQL Server 2016 数据库引擎
- 现在可以在 SQL Server 安装和设置过程中配置多个 tempDB  数据库文件。
- 新的“查询存储”  在数据库中存储查询文本、执行计划和性能指标，以便于监视和排查性能问题。 仪表板可显示耗时最长、占用内存或 CPU 资源最多的查询。
- 时态表  是记录所有数据更改（包括更改日期和时间）的历史记录表。
- SQL Server 中新增了内置 JSON 支持  ，可以支持 JSON 导入、导出、分析和存储。
- 新的 PolyBase  查询引擎将 SQL Server 与 Hadoop 或 Azure Blob 存储中的外部数据相集成。 可以导入和导出数据，也可以执行查询。
- 借助新增的 Stretch Database  功能，可以将本地 SQL Server 数据库中的数据动态安全地存档到云中的 Azure SQL 数据库。 SQL Server 会自动查询本地数据和链接数据库中的远程数据。 
- **内存中 OLTP：** 
    - 现在支持 FOREIGN KEY、UNIQUE 和 CHECK 约束，以及本地编译存储过程 OR、NOT、SELECT DISTINCT、OUTER JOIN 和 SELECT 中的子查询。
    - 支持最大 2TB 的表（之前为最大 256GB）。 
    - 为了实现排序和 AlwaysOn 可用性组支持，增强了列存储索引。
- 新增安全功能：
    - **Always Encrypted：** 启用后，只有具有加密密钥的应用程序才能访问 SQL Server 2016 数据库中的加密敏感数据。 密钥绝不会传递给 SQL Server。
    - **动态数据屏蔽：** 如果在表定义中指定了，那么大多数用户将看不到已掩码的数据，只有拥有 UNMASK 权限的用户才能看到完整数据。
    - **行级别安全性：** 可以在数据库引擎级别限制数据访问，这样用户就能只看到与他们相关的数据。 

## <a name="sql-server-2016-analysis-services-ssas"></a>SQL Server 2016 Analysis Services (SSAS)
SQL Server 2016 Analysis Services 提升了兼容性级别为 1200  的表格模型数据库的性能，并提供了创建、数据库管理、筛选、处理等功能。
- [SQL Server R Services](~/machine-learning/what-s-new-in-sql-server-machine-learning-services.md)  将用于统计分析的 R 编程语言集成到 SQL Server 中。 
- 新增的数据库一致性检查器 (DBCC)  在内部运行，以检测潜在的数据损坏问题。
-  直接查询是查询实时外部数据，而不是先导入数据。现在支持更多的数据源，包括 Azure SQL、Oracle 和 Teradata。 
- 新增了许多 DAX（数据访问表达式）函数  。
- 新增的 [Microsoft.AnalysisServices.Tabular](/dotnet/api/microsoft.analysisservices.tabular)  命名空间管理表格模式实例和模型。 
- [Analysis Services 管理对象 (AMO)](/dotnet/api/) 经过重新设计，包含另一个程序集，即 Microsoft.AnalysisServices.Core.dll  。

请参阅 [Analysis Services 引擎 (SSAS)](/analysis-services/what-s-new-in-analysis-services)。 

## <a name="sql-server-2016-integration-services-ssis"></a>SQL Server 2016 Integration Services (SSIS)
- 支持 AlwaysOn 可用性组 
- **增量包部署**
- 支持 Always Encrypted 
- 新增了 ssis_logreader  数据库级别角色
- 新增了自定义日志记录级别 
- 数据流中的错误列名称  
- 新增了连接器 
- 支持 Hadoop 文件系统 (HDFS) 

请参阅 [Integration Services (SSIS)](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md)。

## <a name="sql-server-2016-master-data-services-mds"></a>SQL Server 2016 Master Data Services (MDS)
- 改进了派生层次结构  ，包括支持递归和多对多层次结构
- 基于域的属性  筛选
- 用于在模型之间共享实体数据的实体同步  功能
- 通过变更集  实现的批准工作流
- 用于提升查询性能的自定义索引 
- 新增了权限级别  ，以提高安全性
- 重新设计了业务规则管理  体验

请参阅 [Master Data Services (MDS)](../master-data-services/what-s-new-in-master-data-services-mds.md)。

## <a name="sql-server-2016-reporting-services-ssrs"></a>SQL Server 2016 Reporting Services (SSRS)
在这一版中，Microsoft 彻底改进了 Reporting Services。 
- 具有 KPI 功能的新 Web 报表门户 
- 新的移动报表发布服务器 
- 重新设计了支持 HTML5 的报表呈现引擎  
- 新增了树状图和旭日图图表类型  

请参阅 [Reporting Services (SSRS)](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)。

## <a name="next-steps"></a>后续步骤   
- [SQL Server 安装程序](../database-engine/install-windows/install-sql-server.md)   
- [SQL Server 2016 发行说明](../sql-server/sql-server-2016-release-notes.md) 
- [SQL Server 2016 数据表](https://download.microsoft.com/download/C/5/3/C53C3AEF-653C-4598-8721-D522E8AC6A3A/SQL_Server_2016_Everything_Built-In_Datasheet_EN_US.pdf)
- [SQL Server各个版本支持的功能](./editions-and-components-of-sql-server-2016.md)
- [安装 SQL Server 2016 的硬件和软件要求](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
- [使用安装向导安装 SQL Server 2016](../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)
- [安装程序和安装服务](../database-engine/install-windows/install-sql-server-servicing-updates.md)
- [新建 SQL PowerShell 模块](https://blogs.technet.microsoft.com/dataplatforminsider/2016/06/30/sql-powershell-july-2016-update/)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]