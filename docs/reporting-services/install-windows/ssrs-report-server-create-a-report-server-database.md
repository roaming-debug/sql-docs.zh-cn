---
title: 创建报表服务器数据库，配置管理器 | Microsoft Docs
description: SQL Server Reporting Services 本机模式使用两个 SQL Server 关系数据库来存储报表服务器元数据和对象。 一个数据库用于主存储，另一个数据库用于存储临时数据。
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seodec18
ms.date: 9/2/2020
ms.openlocfilehash: 430e4fe05afff6f0c024bd8947dad625710913b6
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236768"
---
# <a name="create-a-report-server-database-report-server-configuration-manager"></a>创建报表服务器数据库（报表服务器配置管理器）  

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式使用两个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关系数据库存储报表服务器元数据和对象。 一个数据库用于主存储，另一个数据库用于存储临时数据。 

这两个数据库一起创建，并按名称绑定。 默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例下，这两个数据库分别命名为 **reportserver** 和 **reportservertempdb**。 这两个数据库统称为“报表服务器数据库”或“报表服务器目录”   。

::: moniker range="=sql-server-2016"

SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式包含用于数据警报元数据的第三个数据库  。 为每个 SSRS 服务应用程序创建三个数据库。 默认情况下，数据库名称包含表示服务应用程序的 GUID。 

以下是三个 SharePoint 模式数据库的示例名称：

- ReportingService_90a9f37075544f22953c4a62e4a9f370  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370TempDB  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370_Alerting  

::: moniker-end
  
> [!IMPORTANT]  
> 请不要编写对报表服务器数据库运行查询的应用程序。 报表服务器数据库不是公用架构。 从一个版本升级到下一个版本时可能会更改表结构。 如果编写一个需要访问报表服务器数据库的应用程序，则始终使用 SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] API 来访问报表服务器数据库。  
>
> 执行日志视图是此规则的例外情况。 有关详细信息，请参阅 [报表服务器 ExecutionLog 和 ExecutionLog3 视图](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md)。  
  
## <a name="ways-to-create-the-report-server-database"></a>创建报表服务器数据库的方式

 ### <a name="native-mode"></a>本机模式
 可通过以下方式创建本机模式的报表服务器数据库：  
  
- **自动**。 如果选择默认配置安装选项，请使用 SQL Server 安装向导。 在 SQL Server 安装向导中，此选项为“报表服务器安装选项”页中的“安装和配置”   。 如果选择“仅安装”选项，则必须使用 SQL Server 报表服务器配置管理器来创建数据库。  
  
- **手动**。 使用 SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器。 如果使用远程 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 来承载该数据库，则必须手动创建报表服务器数据库。 有关详细信息，请参阅[创建本机模式报表服务器数据库](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md)。  

::: moniker range="=sql-server-2016"
  
### <a name="sharepoint-mode"></a>SharePoint 模式 
“报表服务器安装选项”页中只有一个用于 SharePoint 模式的选项（“仅安装”） 。 此选项安装所有 SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 文件和 SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 共享服务。 下一步是通过以下某个方式至少创建一个 SSRS 服务应用程序：  
  
- 转到 SharePoint Server 的管理中心，创建 SSRS 服务应用程序。 有关详细信息，请参阅[在 SharePoint 模式下安装第一个报表服务器](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_create_serrviceapplication)中的“创建服务应用程序”部分。  
  
- 使用 SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] PowerShell cmdlet 创建服务应用程序和报表服务器数据库。 有关详细信息，请参阅[用于 Reporting Services SharePoint 模式的 PowerShell cmdlet](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md) 主题中创建服务应用程序的示例。  

::: moniker-end
  
## <a name="database-server-version-requirements"></a>数据库服务器版本要求

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用来承载报表服务器数据库。 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例可以是本地或远程实例。 以下是可用于承载报表服务器数据库的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 支持版本：  
::: moniker range=">=sql-server-ver15"

- Azure SQL 托管实例

- SQL Server 2019

::: moniker-end
::: moniker range=">=sql-server-2017"

- SQL Server 2017  
::: moniker-end

- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
  
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

若要在远程计算机上创建报表服务器数据库，请将连接配置为使用域用户帐户或具有网络访问权限的服务帐户。 如果使用远程 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，请考虑报表服务器应使用哪些凭据来连接实例。 有关详细信息，请参阅[配置报表服务器数据库连接（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)。  
  
> [!IMPORTANT]  
> 报表服务器和用于承载报表服务器数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可位于不同的域中。 对于 Internet 部署，通常的做法是使用防火墙后的服务器。 
>
> 如果要配置用于 Internet 访问的报表服务器，请使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据连接到位于防火墙后的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 使用 IPSEC 保护连接的安全。  
  
## <a name="edition-requirements-for-a-database-server"></a>数据库服务器的版本要求 

 创建报表服务器数据库时，并非所有版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 都可以用来承载数据库。 有关详细信息，请参阅 [SQL Server 各个版本支持的 Reporting Services 功能](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)中的[报表服务器数据库的版本要求](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md#edition-requirements-for-the-report-server-database)部分。  

## <a name="next-steps"></a>后续步骤

阅读有关[报表服务器配置管理器](reporting-services-configuration-manager-native-mode.md)的信息。  

更多疑问？ 请访问 [Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)。