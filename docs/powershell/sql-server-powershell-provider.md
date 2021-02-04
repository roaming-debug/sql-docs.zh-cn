---
title: SQL Server PowerShell 提供程序 | Microsoft Docs
description: 了解用于 Windows PowerShell 的 SQL Server 提供程序，该提供程序通过类似于文件系统路径的路径提供对 SQL Server 对象的访问。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- PowerShell [SQL Server], provider
- PowerShell [SQL Server], SQL Server PowerShell Provider
- Providers [PowerShell]
- SMO [SQL Server], PowerShell
- PowerShell [SQL Server], SMO
- SQL Server Management Objects, PowerShell
ms.assetid: b97acc43-fcd2-4ae5-b218-e183bab916f9
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 07/31/2019
ms.openlocfilehash: 67ad8b940787f50f36ea6798282b651c290d4fca
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076199"
---
# <a name="sql-server-powershell-provider"></a>SQL Server PowerShell 提供程序

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

用于 Windows PowerShell 的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供程序用类似于文件系统路径的路径公开 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 对象的层次结构。 可以使用路径来查找对象，然后使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 管理对象 (SMO) 模型中的方法来针对对象执行操作。  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="benefits-of-the-sql-server-powershell-provider"></a>SQL Server PowerShell 提供程序的优点

通过 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供程序实现的路径有助于轻松地以交互方式查看 SQL Server 实例中的所有对象。 您可以使用与您通常用于导航文件系统路径的命令相似的 Windows PowerShell 别名来导航路径。  
  
## <a name="the-sql-server-powershell-hierarchy"></a>SQL Server PowerShell 层次结构

可以用层次结构表示其数据或对象模型的产品使用 Windows PowerShell 提供程序来公开层次结构。 该层次结构是使用与 Windows 文件系统所用结构相似的驱动器和路径结构公开的。  
  
 每个 Windows PowerShell 提供程序都实现一个或多个驱动器。 每个驱动器都是相关对象的层次结构的根节点。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供程序实现一个 SQLSERVER: 驱动器。 该提供程序还为 SQLSERVER: 驱动器定义了一组主文件夹。 每个文件夹及其子文件夹表示一组可通过使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 管理对象模型访问的对象。 当关注某个以这些主文件夹之一开始的路径中的子文件夹时，可以使用相关对象模型中的方法对该节点所表示的对象执行操作。 下表列出了由 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] 提供程序实现的 Windows PowerShell 文件夹：  
  
|Folder|SQL Server 对象模型命名空间|对象|  
|------------|---------------------------------------|-------------|  
|`SQLSERVER:\SQL`|<xref:Microsoft.SqlServer.Management.Smo><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Agent><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Broker><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Mail>|数据库对象，如表、视图和存储过程。|  
|`SQLSERVER:\SQLPolicy`|<xref:Microsoft.SqlServer.Management.Dmf><br /><br /> <xref:Microsoft.SqlServer.Management.Facets>|基于策略的管理对象，如策略和方面。|  
|`SQLSERVER:\SQLRegistration`|<xref:Microsoft.SqlServer.Management.RegisteredServers><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.RegSvrEnum>|已注册的服务器对象，如服务器组和已注册服务器。|  
|`SQLSERVER:\Utility`|<xref:Microsoft.SqlServer.Management.Utility>|实用工具对象，例如， [!INCLUDE[ssDE](../includes/ssde-md.md)]的托管实例|  
|`SQLSERVER:\DAC`|[Microsoft.SqlServer.Management.Dac](/previous-versions/sql/sql-server-2012/ee212127(v=sql.110))|数据层应用程序对象（如 DAC 包）和操作（如部署 DAC）。|  
|`SQLSERVER:\DataCollection`|<xref:Microsoft.SqlServer.Management.Collector>|数据收集器对象，如收集组和配置存储区。|  
|`SQLSERVER:\SSIS`|<xref:Microsoft.SqlServer.Management.IntegrationServices>|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 对象，如项目、包和环境。|  
|`SQLSERVER:\XEvent`|<xref:Microsoft.SqlServer.Management.XEvent>|SQL Server 扩展事件|
|`SQLSERVER:\DatabaseXEvent`|[Microsoft.SqlServer.Management.XEventDbScoped](/dotnet/api/microsoft.sqlserver.management.xeventdbscoped)|SQL Server 扩展事件|
|`SQLSERVER:\SQLAS`|<xref:Microsoft.AnalysisServices>|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] 对象，例如多维数据集、聚合和维度。|  
  
 例如，可以使用 SQLSERVER:\SQL 文件夹作为路径的开头，该路径可以表示 SMO 对象模型支持的任何对象。 SQLSERVER:\SQL 路径的前导部分是 SQLSERVER:\SQL\\*计算机名称*\\*实例名称*。 实例名称后面的节点在对象集合（如 *数据库* 或 *视图*）和对象名称（如 AdventureWorks2012）之间交替变化。 架构不用对象类表示。 在为架构中的顶层对象（如表或视图）指定节点时，必须以 *SchemaName.ObjectName* 格式指定对象名称。  
  
 以下示例显示 AdventureWorks2012 数据库的 Purchasing 架构中的 Vendor 表的路径，该数据库位于本地计算机上的 [!INCLUDE[ssDE](../includes/ssde-md.md)] 默认实例中：  
  
```powershell
SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```
  
 有关 SMO 对象模型层次结构的详细信息，请参阅 [SMO Object Model Diagram](../relational-databases/server-management-objects-smo/smo-object-model-diagram.md)。  
  
 路径中的集合节点与相关对象模型中的集合类相关联。 对象名节点与相关对象模型中的对象类相关联，如下表中所示：  
  
|路径|SMO 类|  
|----------|---------------|  
|`SQLSERVER:\SQL\MyComputer\DEFAULT\Databases`|<xref:Microsoft.SqlServer.Management.Smo.DatabaseCollection>|  
|`SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012`|<xref:Microsoft.SqlServer.Management.Smo.Database>|  
  
## <a name="sql-server-provider-tasks"></a>SQL Server 提供程序任务  
  
|任务说明|项目|  
|----------------------|-----------|  
|介绍如何使用 Windows PowerShell cmdlet 导航路径中的节点，以及如何在每个节点获取该节点上对象的列表。|[导航 SQL Server PowerShell 路径](navigate-sql-server-powershell-paths.md)|  
|介绍如何使用 SMO 方法和属性对路径中的节点表示的对象进行报告和执行任务。 还介绍如何获取该节点的 SMO 方法和属性的列表。|[使用 SQL Server PowerShell 路径](work-with-sql-server-powershell-paths.md)|  
|介绍如何将 SMO 统一资源名称 (URN) 转换为 SQL Server 提供程序路径。|[将 URN 转换为 SQL Server 提供程序路径](/powershell/module/sqlserver/Convert-UrnToPath)|  
|介绍如何使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供程序打开 SQL Server 身份验证连接。 默认情况下，提供程序使用通过运行 Windows PowerShell 会话的 Windows 帐户的凭据生成的 Windows 身份验证连接。|[在数据库引擎 PowerShell 中管理身份验证](manage-authentication-in-database-engine-powershell.md)|  
  
## <a name="next-steps"></a>后续步骤

[SQL Server PowerShell](sql-server-powershell.md)