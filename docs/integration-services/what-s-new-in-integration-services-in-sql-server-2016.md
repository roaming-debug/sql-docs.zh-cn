---
description: SQL Server 2016 Integration Services 中的新增功能
title: SQL Server 2016 Integration Services 中的新增功能 | Microsoft Docs
ms.custom:
- SQL2016_New_Updated
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, what's new
- what's new [Integration Services]
ms.assetid: da6999c7-e5e3-4a59-a284-1da635995af1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a458d0bcf700341951d79c775fb06fde37585362
ms.sourcegitcommit: 894c1a23e922dc29b82c1d2c34c7b0ff28b38654
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2020
ms.locfileid: "92193725"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2016"></a>SQL Server 2016 Integration Services 中的新增功能

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



本主题介绍 SQL Server 2016 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中新增或更新的功能。 它还包括在 SQL Server 2016 期限期间在 [Integration Services (SSIS) 的 Azure 功能包](../integration-services/azure-feature-pack-for-integration-services-ssis.md)中添加或更新的功能。  

## <a name="new-for-ssis-in-azure-data-factory"></a>SSIS Azure 数据工厂中用于 SSIS 的新增功能

现在使用 2017 年 9 月发布的 Azure 数据工厂版本 2 公共预览版可完成以下操作：
-   将包部署到 Azure SQL 数据库上的 SSIS 目录数据库 (SSISDB)。
-   运行部署到 Azure-SSIS Integration Runtime（Azure 数据工厂版本 2 的一个组件）上 Azure 中的包。

有关详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

这些新增功能需要 SQL Server Data Tools (SSDT) 版本 17.2 或更高版本，但不需要 SQL Server 2017 或 SQL Server 2016。 将包部署到 Azure 时，包部署向导始终将包升级到最新的包格式。

## <a name="2016-improvements-by-category"></a>按类别分组的 2016 改进  
  
-   **可管理性**  
  
    -   改进部署  
  
        -   [SSISDB 升级向导](#ssisdbupgrwiz)  
  
        -   [支持 SSIS 目录中的 AlwaysOn](#AlwaysOn)  
  
        -   [增量包部署](#IncrementalDeployment)  
  
        -   [支持 SSIS 目录中的 Always Encrypted](#encrypted)  
  
    -   优化调试  
  
        -   [SSIS 目录中的全新 ssis_logreader 数据库级别角色](#LogReader)  
  
        -   [SSIS 目录中的全新运行时沿袭日志记录级别](#RuntimeLineage)  
  
        -   [SSIS 目录中的全新自定义日志记录级别](#CustomLogging)  
  
        -   [数据流中错误对应的列名称](#ErrorColumn)  
  
        -   [提供对错误列名称的扩展支持](#getidstring)  
  
        -   [支持服务器范围的默认日志记录级别](#ServerLogLevel)  
  
        -   [API 中的全新 IDTSComponentMetaData130 接口](#CMD130)  
  
    -   改进包管理  
  
        -   [改进了项目升级体验](#ProjectUpgrade)  
  
        -   [AutoAdjustBufferSize 属性自动为数据流计算缓冲区大小](#BufferSize)  
  
        -   [可重用控制流模板](#Templates)  
  
        -   [重命名为部件的新模板](#Parts)  
  
-   **连接**  
  
    -   扩展了本地连接  
  
        -   [支持 OData v4 数据源](#ODatav4)  
  
        -   [显式支持 Excel 2013 数据源](#Excel2013)  
  
        -   [支持 Hadoop 文件系统 (HDFS)](#HDFS)  
  
        -   [提供对 Hadoop 和 HDFS 的扩展支持](#more_hadoop)  
  
        -   [HDFS 文件目标现在支持 ORC 文件格式](#hdfsORC)  
  
        -   [ODBC 组件已针对 SQL Server 2016 进行更新](#odbc2016)  
  
        -   [显式支持 Excel 2016 数据源](#Excel2016)  
  
        -   [Connector for SAP BW for SQL Server 2016 已发布](#SAPBW)
        
        -   [用于 Oracle 和 Teradata 的连接器 v4.0 已发布](#oracleteradata)
        
        -   [用于分析平台系统 (PDW) Appliance Update 5 的连接器已发布](#pdwau5)
  
    -   扩展了到云的连接  
  
        -   Azure 存储空间连接器以及针对 HDInsight 的 Hive 和 Pig 任务 — [适用于 SSIS 的 Azure 功能包已发布（针对 SQL Server 2016）](#AFP2016)
        
        -   [支持在 Service Pack 1 中发布的 Microsoft Dynamics Online 资源](#dynamics)
        
        -   [发布了对 Azure Data Lake Store 的支持](#datalakestore)
        
        -   [发布了对 Azure Synapse Analytics 的支持](#sqldwupload)
  
-   **易用性和工作效率**  
  
    -   改进安装体验  
  
        -   [当 SSISDB 属于某个可用性组时，会阻止升级](#Upgrade)  
  
    -   改进设计体验  
  
        -   [SSIS 设计器为 SQL Server 2016、2014 或 2012 创建和维护包](#OneDesigner)  
  
        -   多项设计器改进和 Bug 修复。  
  
    -   改进了 SQL Server Management Studio 的管理体验
  
        -   [改进了 SSIS 目录视图的性能](#CatViews)  
  
    -   其他增强功能  
  
        -   [平衡数据分发器转换现在已成为 SSIS 的一部分](#BDDinbox)  
  
        -   [数据馈送发布组件现在是 SSIS 的一部分](#ComplexFeedinbox)  
  
        -   [支持在 SQL Server 导入和导出向导中使用 Azure Blob 存储](#AzureBlob)  
  
        -   [Change Data Capture Designer and Service for Oracle for Microsoft SQL Server 2016 已发布](#CDCOracle)  
  
        -   [CDC 组件已针对 SQL Server 2016 进行更新](#cdc2016)  
  
        -   [Analysis Services 执行 DDL 任务已更新](#ASDDL)  
  
        -   [Analysis Services 任务支持表格模型](#ssasrc0)  
  
        -   [支持内置 R Services](#builtinR)  
  
        -   [XML 任务中丰富的 XML 验证输出](#ValidateXML)  
  
## <a name="manageability"></a>可管理性  

### <a name="better-deployment"></a>改进部署

####  <a name="ssisdb-upgrade-wizard"></a><a name="ssisdbupgrwiz"></a> SSISDB 升级向导  
 当数据库早于 SQL Server 实例当前版本时，运行 SSISDB 升级向导来升级 SSIS 目录数据库 SSISDB。 当下列任一条件成立时发生。  
  
-   从旧版 SQL Server 还原数据库。  
  
-   在升级 SQL Server 实例之前，未从 AlwaysOn 可用性组中删除数据库。 这样可以防止数据库自动升级。 有关详细信息，请参阅 [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade)。  
  
 有关详细信息，请参阅 [SSIS 目录 (SSISDB)](../integration-services/catalog/ssis-catalog.md)。 

####  <a name="support-for-always-on-in-the-ssis-catalog"></a><a name="AlwaysOn"></a> 支持 SSIS 目录中的 AlwaysOn  
 AlwaysOn 可用性组功能是一个高可用性和灾难恢复解决方案，可以提供替代数据库镜像的企业级方案。 可用性组支持的故障转移环境适用于一组可以共同进行故障转移的离散用户数据库（称为可用性数据库）。 有关详细信息，请参阅 [AlwaysOn 可用性组](../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)。  
  
 SSIS 在 SQL Server 2016 中引入了新功能，可以轻松地将其部署到集中式 SSIS 目录（即 SSISDB 用户数据库）。 为了确保 SSISDB 数据库及其内容（项目、包、执行日志等）的高可用性，可以将 SSISDB 数据库添加到 AlwaysOn 可用性组（其他用户数据库也是如此）。 当发生故障转移时，其中一个辅助节点将自动成为新的主节点。  
  
 有关如何为 SSISDB 启用 Always On 的详细概述和分步说明，请参阅 [SSIS 目录](../integration-services/catalog/ssis-catalog.md)。  

####  <a name="incremental-package-deployment"></a><a name="IncrementalDeployment"></a> 增量包部署  
增量包部署功能允许你将一个或多个包部署到现有的或新的项目，无需部署整个项目。 可使用下述工具以增量方式部署包。  
  
-   部署向导  
  
-   SQL Server Management Studio（使用部署向导）  
  
-   SQL Server Data Tools (Visual Studio)（也使用部署向导）  
  
-   存储过程  
  
-   管理对象模型 (MOM) API  
  
 有关详细信息，请参阅[部署 Integration Services (SSIS) 项目和包](../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。  

####  <a name="support-for-always-encrypted-in-the-ssis-catalog"></a><a name="encrypted"></a> 支持 SSIS 目录中的 Always Encrypted  
 SSIS 已在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中提供始终加密功能支持。 有关详细信息，请参阅以下博客文章。  
  
-   [SSIS with Always Encrypted](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-always-encrypted/ba-p/388272)  
  
-   [查找转换与始终加密](https://techcommunity.microsoft.com/t5/sql-server-integration-services/lookup-transformation-with-always-encrypted/ba-p/388282)  

### <a name="better-debugging"></a>优化调试

####  <a name="new-ssis_logreader-database-level-role-in-the-ssis-catalog"></a><a name="LogReader"></a> SSIS 目录中的全新 ssis_logreader 数据库级别角色  
 在旧版 SSIS 目录中，仅充当 **ssis_admin** 角色的用户可以访问包含日志记录输出的视图。 现在提供了一个新的 **ssis_logreader** 数据库级别的角色，你可以通过该角色将包含日志记录输出的视图的访问权限授予非管理员用户。  
  
 另外，还提供了一个新的 **ssis_monitor** 角色。 该角色支持 AlwaysOn，仅限 SSIS 目录内部使用。  

####  <a name="new-runtimelineage-logging-level-in-the-ssis-catalog"></a><a name="RuntimeLineage"></a> SSIS 目录中的全新运行时沿袭日志记录级别  
 SSIS 目录中的全新“运行时沿袭”日志记录级别收集在数据流中跟踪沿袭信息所需的数据。  可以分析此沿袭信息以映射任务之间的沿袭关系。 使用此信息，ISV 和开发人员可以构建自定义沿袭映射工具。 

####  <a name="new-custom-logging-level-in-the-ssis-catalog"></a><a name="CustomLogging"></a> SSIS 目录中的全新自定义日志记录级别  
 旧版 SSIS 目录允许你在运行包时从以下四个内置日志记录级别进行选择：“无”、“基本”、“性能”或“详细”。  SQL Server 2016 增加了 RuntimeLineage 日志记录级别。 此外，你现在还可以在 SSIS 目录中创建和保存多个自定义日志记录级别，然后在每次运行包时选取要使用的日志记录级别。 每个自定义日志记录级别只选择要捕获的统计信息和事件。 （可选）包括事件上下文，以便查看变量值、连接字符串和任务属性。 有关详细信息，请参阅 [在 SSIS 服务器上启用包执行的日志记录](../integration-services/performance/integration-services-ssis-logging.md#server_logging)。 

####  <a name="column-names-for-errors-in-the-data-flow"></a><a name="ErrorColumn"></a> 数据流中错误对应的列名称  
 当您将重定向的数据流中包含到错误输出的错误的行时，输出会包括顺序错误发生，但不会显示的列的名称的列的数值标识符。 现在可以通过多种方式来查找和显示发生了错误的列的名称。  
  
-   配置日志记录时，请选择对 **DiagnosticEx** 事件进行记录。 此事件将数据流列映射写入日志。 然后就可以使用由错误输出捕获的列标识符来查找此列映射中的列名称。 有关详细信息，请参阅[数据中的错误处理](../integration-services/data-flow/error-handling-in-data.md)。  
  
-   在高级编辑器中，你可以在查看数据流组件的输入或输出列的属性时，查看上游列的列名称。  
  
-   若要查看发生了错误的列的名称，可将数据查看器附加到错误输出。  数据查看器现在会显示对错误的描述以及发生了错误的列的名称。  
  
-   在脚本组件或自定义数据流组件中，调用 IDTSComponentMetadata100 接口的新 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> 方法。  
  
 有关此方面改进的详细信息，请参阅 SSIS 开发人员 Bo Fan 的下述博客文章： [Error Column Improvements for SSIS Data Flow](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)（SSIS 数据流的错误列改进）。  
  
> [!NOTE]  
>  （后续版本扩展了此支持。 有关详细信息，请参阅 [提供对错误列名称的扩展支持](#getidstring) 和 [API 中的全新 IDTSComponentMetaData130 接口](#CMD130)。）  

####  <a name="expanded-support-for-error-column-names"></a><a name="getidstring"></a> 提供对错误列名称的扩展支持  
 **DiagnosticEx** 事件现在除了记录沿袭列的列信息，还会记录所有输入和输出列的列信息。 因此，我们现在调用管道列映射而非管道沿袭映射的输出。  
  
 GetIdentificationStringByLineageID 方法已重命名为 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A>中新增或更新的功能。 有关详细信息，请参阅 [数据流中错误对应的列名称](#ErrorColumn)。  
  
 有关这方面的更改以及错误列改进的详细信息，请参阅下述已更新的博客文章。 [Error Column Improvements for SSIS Data Flow (Updated for CTP3.3)（SSIS 数据流的错误列改进（已对 CTP3.3 进行更新））](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)  
  
> [!NOTE]  
>  （在 RC0 中，此方法已移至新的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> 接口。 有关详细信息，请参阅 [API 中的全新 IDTSComponentMetaData130 接口](#CMD130)。）  

####  <a name="support-for-server-wide-default-logging-level"></a><a name="ServerLogLevel"></a> 支持服务器范围的默认日志记录级别  
 在 SQL Server“服务器属性”的“服务器日志记录级别”属性下，现在可以选择默认服务器范围的日志记录级别。 可以从内置日志记录级别（基本、无、详细、性能或运行时沿袭）中选择一项，也可以选择现有的自定义日志记录级别。 所选的日志记录级别适用于部署到 SSIS 目录的所有包， 同时也默认适用于运行 SSIS 包的 SQL 代理作业步骤。  

####  <a name="new-idtscomponentmetadata130-interface-in-the-api"></a><a name="CMD130"></a> API 中的全新 IDTSComponentMetaData130 接口  
 SSIS 目录中的全新“运行时沿袭”日志记录级别收集在数据流中跟踪沿袭信息所需的数据。 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> 接口向现有的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 接口添加了新功能，特别是 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> 方法。 （ **GetIdentificationStringByID** 方法已从 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 接口移至新接口。）另外，还有新的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInputColumn130> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn130> 接口，这两个接口均提供 **LineageIdentificationString** 属性。 有关详细信息，请参阅 [数据流中错误对应的列名称](#ErrorColumn)。  

### <a name="better-package-management"></a>改进包管理

####  <a name="improved-experience-for-project-upgrade"></a><a name="ProjectUpgrade"></a> 改进了项目升级体验  
 将 SSIS 项目从旧版升级到最新版时，可以继续使用项目级别的连接管理器，而且包布局和注释也会保留。  

####  <a name="autoadjustbuffersize-property-automatically-calculates-buffer-size-for-data-flow"></a><a name="BufferSize"></a> AutoAdjustBufferSize 属性自动为数据流计算缓冲区大小  
 将新的 **AutoAdjustBufferSize** 属性的值设置为 **true** 时，数据流引擎会自动为数据流计算缓冲区大小。 有关详细信息，请参阅 [Data Flow Performance Features](../integration-services/data-flow/data-flow-performance-features.md)。  

####  <a name="reusable-control-flow-templates"></a><a name="Templates"></a> 可重用控制流模板  
 将常用控制流任务或容器保存到单独的模板文件中，然后即可使用控制流模板在项目的一个或多个包中多次重复使用它。 这种可重用性使得 SSIS 包更容易进行设计和维护。 有关详细信息，请参阅 [通过控制流包部件在包之间重用控制流](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md)。  

####  <a name="new-templates-renamed-as-parts"></a><a name="Parts"></a> 重命名为部件的新模板  
 在 CTP 3.0 中新发布的可重用控制流模板已重命名为控制流部件或包部件。 有关此功能的详细信息，请参阅 [通过控制流包部件在包之间重用控制流](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md)。  

## <a name="connectivity"></a>连接  

### <a name="expanded-connectivity-on-premises"></a>扩展了本地连接

####  <a name="support-for-odata-v4-data-sources"></a><a name="ODatav4"></a> 支持 OData v4 数据源  
 OData 源和 OData 连接管理器现在支持 OData v3 和 v4 协议。  
  
-   对于 OData V3 协议，该组件支持 ATOM 和 JSON 数据格式。  
  
-   对于 OData V4 协议，该组件支持 JSON 数据格式。  
  
 有关详细信息，请参阅 [OData Source](../integration-services/data-flow/odata-source.md)。  

####  <a name="explicit-support-for-excel-2013-data-sources"></a><a name="Excel2013"></a> 显式支持 Excel 2013 数据源  
 Excel 连接管理器、Excel 源和 Excel 目标以及 SQL Server 导入和导出向导现在显式支持 Excel 2013 数据源。 

####  <a name="support-for-the-hadoop-file-system-hdfs"></a><a name="HDFS"></a> 支持 Hadoop 文件系统 (HDFS)  
 HDFS 支持包含连接到 Hadoop 群集的连接管理器，以及用于执行常用 HDFS 操作的任务。 有关详细信息，请参阅 [Integration Services (SSIS) 中的 Hadoop 和 HDFS 支持](../integration-services/hadoop-and-hdfs-support-in-integration-services-ssis.md)。  

####  <a name="expanded-support-for-hadoop-and-hdfs"></a><a name="more_hadoop"></a> 提供对 Hadoop 和 HDFS 的扩展支持  
  
-   Hadoop 连接管理器现在支持基本身份验证和 Kerberos 身份验证。 有关详细信息，请参阅 [Hadoop Connection Manager](../integration-services/connection-manager/hadoop-connection-manager.md)。  
  
-   HDFS 文件源和 HDFS 文件目标现在支持文本格式和 Avro 格式。 有关详细信息，请参阅  [HDFS File Source](../integration-services/data-flow/hdfs-file-source.md) 和  [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md)。  
  
-   除了 CopyToHadoop 和 CopyFromHadoop 选项，Hadoop 文件系统任务现在还支持 CopyWithinHadoop 选项。 有关详细信息，请参阅 [Hadoop File System Task](../integration-services/control-flow/hadoop-file-system-task.md)。  

####  <a name="hdfs-file-destination-now-supports-orc-file-format"></a><a name="hdfsORC"></a> HDFS 文件目标现在支持 ORC 文件格式  
 HDFS 文件目标现在除了支持文本文件格式和 Avro 文件格式，还支持 ORC 文件格式。 （HDFS 文件源仅支持文本文件格式和 Avro 文件格式。）有关此组件的详细信息，请参阅 [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md)。  

####  <a name="odbc-components-updated-for-sql-server-2016"></a><a name="odbc2016"></a> ODBC 组件已针对 SQL Server 2016 进行更新  
 ODBC 源和目标组件在更新后已与 SQL Server 2016 完全兼容。 没有新功能，行为也没有改变。  

####  <a name="explicit-support-for-excel-2016-data-sources"></a><a name="Excel2016"></a> 显式支持 Excel 2016 数据源  
 Excel 连接管理器、Excel 源和 Excel 目标现在显式支持 Excel 2016 数据源。  

####  <a name="connector-for-sap-bw-for-sql-server-2016-released"></a><a name="SAPBW"></a> Connector for SAP BW for SQL Server 2016 已发布  
 用于 Microsoft SQL ServerÂ® 2016 的 MicrosoftÂ® Connector for SAP BW 已作为 SQL Server 2016 功能包的一部分发布。 若要下载功能包的组件，请参阅 [MicrosoftÂ® SQL ServerÂ® 2016 功能包](https://go.microsoft.com/fwlink/?LinkID=746297)。
 
#### <a name="connectors-v40-for-oracle-and-teradata-released"></a><a name="oracleteradata"></a> 用于 Oracle 和 Teradata 的连接器 v4.0 已发布
用于 Oracle 和 Teradata 的 Microsoft 连接器 v4.0 已发布。 若要下载该连接器，请参阅 [用于 Oracle 和 Teradata 的 Microsoft 连接器 v4.0](https://www.microsoft.com/download/details.aspx?id=52950)。

### <a name="connectors-for-analytics-platform-system-pdw-appliance-update-5-released"></a><a name="pdwau5"></a> 用于分析平台系统 (PDW) Appliance Update 5 的连接器已发布
用于将数据加载到 PDW with AU5 的目标适配器已发布。 若要下载该适配器，请参阅 [分析平台系统 Appliance Update 5 文档和客户端工具](https://www.microsoft.com/download/details.aspx?id=51610)。

### <a name="expanded-connectivity-to-the-cloud"></a>扩展了到云的连接

####  <a name="azure-feature-pack-for-ssis-released-for-sql-server-2016"></a><a name="AFP2016"></a> 适用于 SSIS 的 Azure 功能包已发布（针对 SQL Server 2016）  
 用于 Integration Services 的 Azure 功能包已发布（针对 SQL Server 2016）。 功能包包含连接到 Azure 数据源的连接管理器，以及用于执行常用 Azure 操作的任务。 有关详细信息，请参阅[用于 Integration Services 的 Azure 功能包 (SSIS)](../integration-services/azure-feature-pack-for-integration-services-ssis.md)。  

#### <a name="support-for-microsoft-dynamics-online-resources-released-in-service-pack-1"></a><a name="dynamics"></a> 支持在 Service Pack 1 中发布的 Microsoft Dynamics Online 资源

安装 SQL Server 2016 Service Pack 1 后，OData 源和 OData 连接管理器现支持连接到 Microsoft Dynamics AX Online 和 Microsoft Dynamics CRM Online 的 OData 源。

#### <a name="support-for-azure-data-lake-store-released"></a><a name="datalakestore"></a> 发布了对 Azure Data Lake Store 的支持

最新版本的 Azure 功能包包括连接管理器、源和目标，以便在 Azure Data Lake Store 中移出或移入数据。 有关详细信息，请参阅[用于 Integration Services 的 Azure 功能包 (SSIS)](../integration-services/azure-feature-pack-for-integration-services-ssis.md)

#### <a name="support-for-azure-synapse-analytics-released"></a><a name="sqldwupload"></a> 发布了对 Azure Synapse Analytics 的支持

最新版本的 Azure 功能包包括 Azure SQL DW 上传任务，用于为 SQL 数据仓库填充数据。 有关详细信息，请参阅[用于 Integration Services 的 Azure 功能包 (SSIS)](../integration-services/azure-feature-pack-for-integration-services-ssis.md)

## <a name="usability-and-productivity"></a>易用性和工作效率  
 
### <a name="better-install-experience"></a>改进安装体验

####  <a name="upgrade-blocked-when-ssisdb-belongs-to-an-availability-group"></a><a name="Upgrade"></a> 当 SSISDB 属于某个可用性组时，会阻止升级  
 如果 SSIS 目录数据库 (SSISDB) 属于某个 AlwaysOn 可用性组，则必须从该可用性组中删除 SSISDB，升级 SQL Server，然后再将 SSISDB 重新添加到该可用性组。 有关详细信息，请参阅 [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade)。  

### <a name="better-design-experience"></a>改进设计体验

####  <a name="multi-targeting-and-multi-version-support-in-ssis-designer"></a><a name="OneDesigner"></a> SSIS 设计器中的多目标和多版本支持  
 你现在可以在用于 Visual Studio 2015 的 SQL Server Data Tools (SSDT) 中使用 SSIS 设计器来创建、维护和运行面向 SQL Server 2016、SQL Server 2014 或 SQL Server 2012 的包。 要获取 SSDT，请参阅 [下载最新的 SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md)。 

 在解决方案资源管理器中，右键单击 Integration Services 项目并选择“属性”  以打开该项目的属性页。 在“配置属性”  的“常规” 选项卡上，选择“TargetServerVersion”  属性，然后选择 SQL Server 2016、SQL Server 2014 或 SQL Server 2012。  
   
 ![“项目属性”对话框中的 TargetServerVersion 属性](../integration-services/media/targetserverversion2.png "“项目属性”对话框中的 TargetServerVersion 属性")  

> [!IMPORTANT]
> 如果为 SSIS 开发自定义扩展插件，请参阅 [支持自定义组件中的多目标](../integration-services/extending-packages-custom-objects/support-multi-targeting-in-your-custom-components.md) 和 [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/)（使 SSIS 自定义扩展插件获得用于 SQL Server 2016 的 SSDT 2015 多版本支持的支持）。  

### <a name="better-management-experience-in-sql-server-management-studio"></a>改进了 SQL Server Management Studio 的管理体验

####  <a name="improved-performance-for-ssis-catalog-views"></a><a name="CatViews"></a> 改进了 SSIS 目录视图的性能  
 现在，大多数 SSIS 目录视图在由不是 ssis_admin 角色成员的用户运行时，其性能有了改进。  

### <a name="other-enhancements"></a>其他增强功能

####  <a name="balanced-data-distributor-transformation-is-now-part-of-ssis"></a><a name="BDDinbox"></a> 平衡数据分发器转换现在已成为 SSIS 的一部分  
 现在，当你安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]时，会安装平衡数据分发器转换，而在旧版 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]中，需单独下载它。 有关详细信息，请参阅 [Balanced Data Distributor Transformation](../integration-services/data-flow/transformations/balanced-data-distributor-transformation.md)。  
  
####  <a name="data-feed-publishing-components-are-now-part-of-ssis"></a><a name="ComplexFeedinbox"></a> 数据馈送发布组件现在是 SSIS 的一部分  
 现在，当你安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]时，会安装数据馈送发布组件，而在旧版 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]中，需单独下载这些组件。 有关详细信息，请参阅 [Data Streaming Destination](../integration-services/data-flow/data-streaming-destination.md)。  

####  <a name="support-for-azure-blob-storage-in-the-sql-server-import-and-export-wizard"></a><a name="AzureBlob"></a> 支持在 SQL Server 导入和导出向导中使用 Azure Blob 存储  
 SQL Server 导入和导出向导现在可以从 Azure Blob 存储导入数据，以及将数据保存到其中。 有关详细信息，请参阅[选择数据源（SQL Server 导入和导出向导）](../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)和[选择目标（SQL Server 导入和导出向导）](../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)。 

####  <a name="change-data-capture-designer-and-service-for-oracle-for-microsoft-sql-server-2016-released"></a><a name="CDCOracle"></a> Change Data Capture Designer and Service for Oracle for Microsoft SQL Server 2016 已发布  
 用于 Microsoft SQL ServerÂ® 2016 的 MicrosoftÂ® Change Data Capture Designer and Service for Oracle by Attunity 已作为 SQL Server 2016 功能包的一部分发布。  这些组件现在支持采用经典安装的 Oracle 12c。 （不支持多租户安装）若要下载功能包的组件，请参阅 [MicrosoftÂ® SQL ServerÂ® 2016 功能包](https://go.microsoft.com/fwlink/?LinkID=746297)。  
  
####  <a name="cdc-components-updated-for-sql-server-2016"></a><a name="cdc2016"></a> CDC 组件已针对 SQL Server 2016 进行更新  
 CDC（变更数据捕获）的控制任务、源和拆分器转换组件在更新后已与 SQL Server 2016 完全兼容。 没有新功能，行为也没有改变。  
  
####  <a name="analysis-services-execute-ddl-task-updated"></a><a name="ASDDL"></a> Analysis Services 执行 DDL 任务已更新  
 Analysis Services 执行 DDL 任务在更新后接受表格模型脚本语言命令。

####  <a name="analysis-services-tasks-support-tabular-models"></a><a name="ssasrc0"></a> Analysis Services 任务支持表格模型  
 现在，你可以使用所有支持 SQL Server Analysis Services (SSAS)（使用 SQL Server 2016 表格模型）的 SSIS 任务和目标。 SSIS 任务在更新后表示表格对象而非多维对象。 例如，当你选择要处理的对象时，Analysis Services 处理任务将自动检测到它是表格模型，并显示表格对象列表而不是度量值组和维度。 分区处理目标现在也显示表格对象，并支持将对象推送到分区中。  
  
 维度处理目标不适用于 SQL 2016 兼容级别的表格模型。  进行表格处理时，只需使用 Analysis Services 处理任务和分区处理目标。 

####  <a name="support-for-built-in-r-services"></a><a name="builtinR"></a> 支持内置 R Services  
 SSIS 已在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中提供内置 R Services 支持。 可以使用 SSIS 来提取数据和加载分析的输出，以及构建、运行和定期重新定型 R 模型。 有关详细信息，请参阅以下日志文章。 [Operationalize your machine learning project using SQL Server 2016 SSIS and R Services](https://techcommunity.microsoft.com/t5/sql-server-integration-services/operationalize-your-machine-learning-project-using-sql-server/ba-p/388296)（使用 SQL Server 2016 SSIS 和 R Services 实施机器学习项目）。 

####  <a name="rich-xml-validation-output-in-the-xml-task"></a><a name="ValidateXML"></a> XML 任务中丰富的 XML 验证输出  
 通过启用 XML 任务的 **ValidationDetails** 属性，验证 XML 文档并获取丰富的错误输出。 在可以使用 **ValidationDetails** 属性之前，XML 任务的 XML 验证仅返回 true 或 false 结果，而不包含关于错误或其位置的详细信息。 现在，当你将 **ValidationDetails** 设置为 true 时，输出文件将包含关于每个错误的详细信息，包括行号和位置。 此信息可用于了解、查找和修复 XML 文档中的错误。 有关详细信息，请参阅 [Validate XML with the XML Task](../integration-services/control-flow/validate-xml-with-the-xml-task.md)。  
  
 [!INCLUDE[ssIS](../includes/ssis-md.md)] 在 **Service Pack 2 中引入了** ValidationDetails [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 属性。 此时尚未公布或记录这一新属性。 **和** 中也提供 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] ValidationDetails [!INCLUDE[ssSQL15](../includes/sssql15-md.md)]属性。   

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 中的新增功能](../sql-server/what-s-new-in-sql-server-2016.md)   
 [SQL Server 2016 的各版本和支持的功能](../sql-server/editions-and-components-of-sql-server-2016.md)