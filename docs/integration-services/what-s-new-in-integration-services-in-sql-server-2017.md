---
description: SQL Server 2017 Integration Services 中的新增功能
title: SQL Server 2017 Integration Services 中的新增功能 | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e26d7884-e772-46fa-bfdc-38567fe976a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b955aa61a4e196fc4c419a9b34e186590006df42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345669"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2017"></a>SQL Server 2017 Integration Services 中的新增功能

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


本主题介绍 [!INCLUDE[ssSQLv14_md](../includes/sssql17-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中新增或更新的功能。

> [!NOTE]
> SQL Server 2017 还包括 SQL Server 2016 的功能以及 SQL Server 2016 更新中新增的功能。 若要了解 SQL Server 2016 中的新 SSIS 功能，请参阅 [SQL Server 2016 Integration Services 中的新增功能](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md)。

## <a name="highlights-of-this-release"></a>此版本的亮点

下面是 SQL Server 2017 Integration Services 最重要的新功能。

-   **Scale Out**。跨多台辅助角色计算机更轻松地分发 SSIS 包执行，并从一台主计算机中管理执行和辅助角色。 有关详细信息，请参阅 [Integration Services Scale Out](../integration-services/scale-out/integration-services-ssis-scale-out.md)。

-   **Linux 上的 Integration Services**。 在 Linux 计算机上运行 SSIS 包。 有关详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../linux/sql-server-linux-migrate-ssis.md)。

-   **连接改进**。 使用更新后的 OData 组件连接到 Microsoft Dynamics AX Online 和 Microsoft Dynamics CRM Online 的 OData 源。 

## <a name="new-in-azure-data-factory"></a>Azure 数据工厂的新增功能

现在使用 2017 年 9 月发布的 Azure 数据工厂版本 2 公共预览版可完成以下操作：
-   将包部署到 Azure SQL 数据库上的 SSIS 目录数据库 (SSISDB)。
-   运行部署到 Azure-SSIS Integration Runtime（Azure 数据工厂版本 2 的一个组件）上 Azure 中的包。

有关详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。

这些新增功能需要 SQL Server Data Tools (SSDT) 版本 17.2 或更高版本，但不需要 SQL Server 2017 或 SQL Server 2016。 将包部署到 Azure 时，包部署向导始终将包升级到最新的包格式。

## <a name="new-in-the-azure-feature-pack"></a>Azure 功能包的新增功能

除 SQL Server 中的连接改进外，用于 Azure 的 Integration Services 功能包还添加了对 Azure Data Lake Store 的支持。 有关详细信息，请参阅博客文章[新版 Azure 功能包增强了 ADLS 连接](https://blogs.msdn.microsoft.com/ssis/2017/08/29/new-azure-feature-pack-release-strengthening-adls-connectivity/)。 另请参阅[用于 Integration Services (SSIS) 的 Azure 功能包](azure-feature-pack-for-integration-services-ssis.md)。

## <a name="new-in-sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT) 的新增功能

现在，可在 Visual Studio 2017 或 Visual Studio 2015 中开发面向 SQL Server 2012 到 SQL Server 2017 的 SSIS 项目和包。 有关详细信息，请参阅 [下载 SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)。

## <a name="new-in-ssis-in-sql-server-2017-rc1"></a>SQL Server 2017 RC1 中新的 SSIS 功能

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>SSIS Scale Out 中的新增功能和更改功能

-   Scale Out 主要角色现在支持高可用性。 可为 SSISDB 启用 Always On 功能并为承载 Scale Out Master 服务的服务器设置 Windows Server 故障转移群集。 通过将此更改应用于 Scale Out Master，即可避免单一故障点并为整个 Scale Out 部署提供高可用性。
-   改进了 Scale Out 辅助角色中执行日志的故障转移处理。 如果 Scale Out Worker 意外停止，执行日志将保存到本地磁盘中。 稍后重启辅助角色时，它会重新加载保存的日志并将其继续保存到 SSISDB。
-   存储过程 **[catalog].[create_execution]** 的参数 *runincluster* 重命名为 *runinscaleout*，以保持一致性和可读性。 更改参数名称会产生以下影响：
    -   如果现有脚本在 Scale Out 中运行包，必须将参数名称从 runincluster 更改为 runinscaleout，以使脚本在 RC1 中正常运行。
    -   SQL Server Management Studio (SSMS) 17.1 及更早版本无法在 RC1 的 Scale Out 中触发包执行。 错误消息为：“@runincluster 不是过程 create_execution 的参数   。” SSMS 的下一个版本，即版本 17.2 中修复了此问题。 SSMS 的版本 17.2 及更高版本支持 Scale Out 中的新参数名称和包执行。解决方法如下：SSMS 版本 17.2 可用之前，可以使用 SSMS 的现有版本生成包执行脚本，然后在脚本中将 runincluster 参数的名称更改为 runinscaleout，并运行该脚本 。
-   SSIS 目录具有新的全局属性，用于指定执行 SSIS 包的默认模式。 此新属性适用于这种情况：调用 [catalog].[create_execution] 存储过程且将 runinscaleout 参数设为 null。 此模式也适用于 SSIS SQL 代理作业。 可以在 SSMS 中 SSISDB 节点的“属性”对话框中设置新的全局属性，或者使用下面的命令进行设置：
    ```sql
    EXEC [catalog].[configure_catalog] @property_name=N'DEFAULT_EXECUTION_MODE', @property_value=1
    ```

## <a name="new-in-ssis-in-sql-server-2017-ctp-21"></a>SQL Server 2017 CTP 2.1 中新的 SSIS 功能

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>SSIS Scale Out 中的新增功能和更改功能

-   现可在 Scale Out 中触发执行时使用 Use32BitRuntime 参数。
-   Scale Out 中用于包执行的 SSISDB 日志记录性能得到了改进。 事件消息和消息上下文日志现批量写入 SSISDB 中，而不是逐一写入。 以下是有关此改进的一些其他说明：        
    - 在当前版本的 SQL Server Management Studio (SSMS) 中，某些报表目前不显示要在 Scale Out 中执行的日志。预计将在下一版本的 SSMS 中支持它们。 受影响的报表包括“所有连接”报表、“错误上下文”报表和 Integration Service 仪表板中的“连接信息”部分。
    - 新增了列 event_message_guid。 在 Scale Out 中查询这些执行日志时，使用 event_message_guid 替代 event_message_id 联接 [catalog].[event_message_context] 和 [catalog].[event_messages] 视图。
-   若要获取用于 SSIS Scale Out 的管理应用程序，请[下载 SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 17.1 或更高版本。

## <a name="new-in-ssis-in-sql-server-2017-ctp-20"></a>SQL Server 2017 CTP 2.0 中新的 SSIS 功能

SQL Server 2017 CTP 2.0 无新的 SSIS 功能。

## <a name="new-in-ssis-in-sql-server-2017-ctp-14"></a>SQL Server 2017 CTP 1.4 中新的 SSIS 功能

SQL Server 2017 CTP 1.4 无新的 SSIS 功能。

## <a name="new-in-ssis-in-sql-server-2017-ctp-13"></a>SQL Server 2017 CTP 1.3 中新的 SSIS 功能

SQL Server 2017 CTP 1.3 无新的 SSIS 功能。

## <a name="new-in-ssis-in-sql-server-2017-ctp-12"></a>SQL Server 2017 CTP 1.2 中新的 SSIS 功能

SQL Server 2017 CTP 1.2 无新的 SSIS 功能。

## <a name="new-in-ssis-in-sql-server-2017-ctp-11"></a>SQL Server 2017 CTP 1.1 中新的 SSIS 功能

SQL Server 2017 CTP 1.1 无新的 SSIS 功能。

## <a name="new-in-ssis-in-sql-server-2017-ctp-10"></a>SQL Server 2017 CTP 1.0 中新的 SSIS 功能

### <a name="scale-out-for-ssis"></a>SSIS 的 Scale Out

Scale Out 功能使在多台计算机上运行 [!INCLUDE[ssIS_md](../includes/ssis-md.md)] 变得更加简单。 
   
安装 Scale Out Master 和 Worker 之后，可分发包以在不同的 Worker 上自动执行。 如果执行意外终止，将自动重试该执行。 此外，可使用 Master 集中管理所有执行和 Worker。
   
有关详细信息，请参阅 [Integration Services Scale Out](../integration-services/scale-out/integration-services-ssis-scale-out.md)。
   
### <a name="support-for-microsoft-dynamics-online-resources"></a>支持 Microsoft Dynamics Online 资源

OData 源和 OData 连接管理器现支持连接到 Microsoft Dynamics AX Online 和 Microsoft Dynamics CRM Online 的 OData 源。