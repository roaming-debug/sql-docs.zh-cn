---
title: 支持的版本和版本升级 (SQL Server 2019)
description: SQL Server 2019 支持的版本和版本升级。
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server], adding to existing installations
- versions [SQL Server], upgrading
- upgrading SQL Server, upgrades supported
- cross-language support
ms.assetid: 702359c4-6ca9-42a8-860c-a95a802898a1
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: 6b92acc4dbf0b7b9418e382019f48ec1d1c6a36a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463558"
---
# <a name="supported-version--edition-upgrades-sql-server-2019"></a>支持的版本和版本升级 (SQL Server 2019)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  你可以从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]、 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]和 [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]进行升级。 本文列出了支持的从这些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本进行升级的途径以及支持的 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 版本升级。  
  
## <a name="pre-upgrade-checklist"></a>升级前的清单  

- 在从 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 的某个版本升级到另一个版本之前，请确认您当前所用的功能在要移到的版本中受支持。  
- 验证支持的[硬件和软件](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)。
- 升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]之前，请先为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 启用 Windows 身份验证，并验证默认配置： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务帐户是否是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sysadmin 组的成员。
- 若要升级到 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)]，您运行的必须是受支持的操作系统。 有关详细信息，请参阅[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)。  
- 如果有挂起的重新启动操作，则会阻止升级。  
- 如果未运行 Windows Installer 服务，则会阻止升级。

## <a name="unsupported-scenarios"></a>不受支持的方案

- 不支持 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 的跨版本实例。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 组件的版本号在 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 实例中必须相同。  
  
- [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 仅适用于 64 位平台。 不支持跨平台升级。 不能使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 32 位实例升级到本机 64 位。 但是，如果数据库未在复制过程中发布，则可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的 32 位实例中备份或分离数据库，然后再将它们还原或附加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新实例（64 位）。 必须在 master、msdb 和 model 系统数据库中重新创建任何登录名和其他用户对象。  
  
- 不能在升级现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的过程中添加新功能。 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例升级到 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 之后，您可以使用 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 安装程序添加功能。 有关详细信息，请参阅[向 SQL Server 的实例添加功能（安装程序）](./add-features-to-an-instance-of-sql-server-setup.md)。  
 
## <a name="upgrades-from-earlier-versions-to-sssqlv15-md"></a>从早期版本升级到 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)]  
 
[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 支持从下列 SQL Server 版本升级：

- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 或更高版本
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3 或更高版本
- [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] SP2 或更高版本
- [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]

下表列出了从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本升级到 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 的支持方案。  
  
|升级前的版本|支持的升级途径|  
|:------|:------|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Enterprise|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Developer|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/>[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Standard|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Web|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Express |[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Business Intelligence|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Evaluation|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Enterprise|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Developer|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Standard|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Web|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Express |[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Express <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 商业智能|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Evaluation|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer|
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Enterprise|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Developer|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Standard|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Web|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Express |[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Express <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Business Intelligence|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Evaluation|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer|
|[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] Enterprise|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] Developer|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] Standard|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard|  
|[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] Web|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web|  
|[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] Express |[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Express <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer|  
|[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] Business Intelligence|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] Evaluation|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer|
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 候选发布* |[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise |  
|[!INCLUDE[sssqlv15_md](../../includes/sssqlv15-md.md)] Developer |[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise | 

 \* 从候选软件发布升级的 Microsof 支持专门针对参与了早期采用者计划的客户。

## <a name="migrate-to-sql-server-2019"></a>迁移到 SQL Server 2019

可以从较旧的版本迁移数据库。 例如，可以将数据库从 [!INCLUDE [sskilimanjaro-md](../../includes/sskilimanjaro-md.md)] 迁移到 SQL Server 2019。

有关信息，请参阅 [Azure 数据库迁移指南](https://datamigration.microsoft.com/scenario/sql-to-sqlserver)。

以下提示和工具可帮助你计划和实施迁移。

- 迁移工具：支持通过[数据迁移助手 (DMA) ](../../dma/dma-overview.md)进行迁移。
- 备份和还原：SQL Server 2008 或 SQL Server 2008 R2 上执行的备份可以还原到 SQL Server 2019。
- 日志传送：如果主实例运行 SQL Server 2008 SP3 或更高版本，或者 SQL Server 2008 R2 SP2 或更高版本，并且辅助实例运行 SQL Server 2019，则支持日志传送。 

   > [!WARNING]
   > 如果发生自动或手动故障转移并且 SQL Server 2019 实例成为主实例，SQL Server 2008 或 SQL Server 2008 R2 实例将变为辅助实例，并且无法接收主实例的更改。

- 大容量加载:可以将表从 SQL Server 2008 或 SQL Server 2008 R2 大容量复制到 SQL Server 2019。

## <a name="sssqlv15-md-edition-upgrade"></a>[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 版本升级 

下表列出了 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)]中支持的版本升级方案。  

有关如何执行版本升级的分步说明，请参阅[升级到 SQL Server 的不同版本（安装程序）](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md)。  
  
|升级前的版本|升级到的版本|  
|------------------|----------------|  
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 和内核）**|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise |  
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Evaluation Enterprise**|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> 对于独立安装，支持从 Evaluation（免费版本）升级到任何付费版本；但对于群集安装，则不支持此升级。 此限制不适用于安装在参与可用性组的 Windows 故障转移群集上的独立实例。 |  
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard**|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 或内核许可证）|  
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer**|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard|  
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard|  
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Express*|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Web|  
  
 此外，您还可以执行 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 或内核许可证）和 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（内核许可证）之间的版本升级：  
  
|版本升级自|版本升级到|  
|--------------------------|------------------------|  
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 许可证）**|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（内核许可证）|  
|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（内核许可证）|[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Enterprise（服务器+CAL 许可证）|  
  
 \* 同样适用于 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Express with Tools 和 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Express with Advanced Services。  
  
 ** 更改 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 的群集实例版本受限。 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 故障转移群集不支持以下方案：  
  
- [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 从 Enterprise 更改为 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Developer、Standard 或 Evaluation。  
  
- [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 从 Developer 更改为 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard 或 Evaluation。  
  
- 从 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard 更改为 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Evaluation。  
  
- 从 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Evaluation 更改为 [!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] Standard。  
  
## <a name="see-also"></a>另请参阅  

 [[!INCLUDE[sssqlv15-md](../../includes/sssqlv15-md.md)] 的版本和受支持的功能](../../sql-server/editions-and-components-of-sql-server-version-15.md)

 [安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)

 [升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)