---
title: 支持的版本和版本升级 (SQL Server 2016)
description: SQL Server 2016 支持的版本和版本升级。
ms.custom: ''
ms.date: 06/27/2016
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
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8bf008b28a130c705eabaebf986b28d7da8cd669
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172229"
---
# <a name="supported-version--edition-upgrades-sql-server-2016"></a>支持的版本和版本升级 (SQL Server 2016)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  你可以从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]和 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]进行升级。 本文列出了支持的从这些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本进行升级的途径以及支持的 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 版本升级。  
  
## <a name="pre-upgrade-checklist"></a>升级前的清单  
  
-   在从 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 的某个版本升级到另一个版本之前，请确认您当前所用的功能在要移到的版本中受支持。  
  
-   升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]之前，请先为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 启用 Windows 身份验证，并验证默认配置： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务帐户是否是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sysadmin 组的成员。  
  
-   若要升级到 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]，您运行的必须是受支持的操作系统。 有关详细信息，请参阅 [安装 SQL Server 2016 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。  
  
-   如果有挂起的重新启动操作，则会阻止升级。  
  
-   如果未运行 Windows Installer 服务，则会阻止升级。  
  
## <a name="unsupported-scenarios"></a>不受支持的方案  
  
-   不支持 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 的跨版本实例。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 组件的版本号在 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]实例中必须相同。  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 仅适用于 64 位平台。 不支持跨平台升级。 不能使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 32 位实例升级到本机 64 位。 但是，如果数据库未在复制过程中发布，则可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的 32 位实例中备份或分离数据库，然后再将它们还原或附加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新实例（64 位）。 必须在 master、msdb 和 model 系统数据库中重新创建任何登录名和其他用户对象。  
  
-   不能在升级现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的过程中添加新功能。 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例升级到 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 之后，您可以使用 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 安装程序添加功能。 有关详细信息，请参阅[向 SQL Server 2016 的实例添加功能（安装程序）](./add-features-to-an-instance-of-sql-server-setup.md)。  
 
-   在 WOW 模式下不支持故障转移群集。  
  
-   不支持从以前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的 Evaluation 版升级。

-   从 RC1 或以前版本的 SQL Server 2016 升级到 RC3 或更高版本时，必须在升级前卸载 PolyBase 并在升级后重新安装。
  
## <a name="upgrades-from-earlier-versions-to-sssql15-md"></a>从早期版本升级到 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]  
 
SQL Server 2016 支持从下列 SQL Server 版本升级：
 
- [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] SP4 或更高版本
- [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP3 或更高版本
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 或更高版本
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或更高版本 
 
> [!NOTE]  
> 若要在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 上升级数据库，请参阅 [2005 版本支持](#SupportFor2005)。  
  
下表列出了从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本升级到 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 的支持方案。  
  
|升级前的版本|支持的升级途径|  
|------------------|----------------------------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Small Business|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Workgroup|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Datacenter|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Small Business|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Workgroup|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 商业智能|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Evaluation|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Evaluation|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 候选发布* |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise |  
|[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] Developer |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise | 

 \* 从候选发布升级的 Microsof 支持专门针对参与了技术采用计划 (TAP) 的客户。 
   
###  <a name="sssql15-md-support-for-ssversion2005"></a><a name="SupportFor2005"></a> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 支持  
此节讨论针对 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 的 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]支持。 在 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 中，您能够执行以下操作：  
  
-   将 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库（mdf/ldf 文件）附加到数据库引擎的 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 实例。  
  
-   从备份将 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库还原为数据库引擎的 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 实例。  
  
-   备份 [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] 多维数据集并在 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]上还原。  
  
> [!NOTE]  
> 当 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库升级到 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 时，该数据库兼容级别将从 90 更改为 100。 在 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 中，数据库兼容性级别的有效值为 100、110、120 和 130。 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 讨论兼容级别更改可能会影响 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应用程序的方式。  
  
不支持以上列表中未指定的任何方案，包括但不限于以下情况：  
  
-   在相同计算机上安装 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]（并行）。  
  
-   使用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例作为涉及 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 实例的复制拓扑的成员。  
  
-   在 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例之间配置数据库镜像。  
  
-   使用日志传送在 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例之间备份事务日志。  
  
-   在 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例之间配置链接服务器。  
  
-   从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Management Studio 管理 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 实例。  
  
-   在 [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] Management Studio 中附加 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 多维数据集。  
  
-   从 [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] Management Studio 连接到 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 。  
  
-   从 [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] Management Studio 管理 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 服务。  
  
-   支持 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 第三方自定义 Integration Services 组件，如执行和升级。  
  
## <a name="sssql15-md-edition-upgrade"></a>[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 版本升级  
下表列出了 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]中支持的版本升级方案。  
  
有关如何执行版本升级的分步说明，请参阅[升级到 SQL Server 2016 的不同版本（安装程序）](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md)。  
  
|升级前的版本|升级到的版本|  
|------------------|----------------|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 和内核）**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise |  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation Enterprise**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> 对于独立安装，支持从 Evaluation（免费版本）升级到任何付费版本；但对于群集安装，则不支持此升级。 此限制不适用于安装在参与可用性组的 Windows 故障转移群集上的独立实例。|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 或内核许可证）|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express*|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
  
此外，您还可以执行 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 或内核许可证）和 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（内核许可证）之间的版本升级：  
  
|版本升级自|版本升级到|  
|--------------------------|------------------------|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 许可证）**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（内核许可证）|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（内核许可证）|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise（服务器+CAL 许可证）|  
  
 \* 同样适用于 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express with Tools 和 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express with Advanced Services。  
  
 ** 对 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 故障转移群集版本的更改受到限制。 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 故障转移群集不支持以下方案：  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 从 Enterprise 更改为 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer、Standard 或 Evaluation。  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 从 Developer 更改为 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard 或 Evaluation。  
  
-   从 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard 更改为 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation。  
  
-   从 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation 更改为 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard。  
  
## <a name="see-also"></a>另请参阅  

[SQL Server 2016 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)     
[安装 SQL Server 2016 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)     
[升级到 SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server.md)    
  
