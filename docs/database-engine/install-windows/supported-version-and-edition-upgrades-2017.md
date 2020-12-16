---
title: 支持的版本和版本升级 (SQL Server 2017)
description: SQL Server 2017 支持的版本和版本升级。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
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
ms.openlocfilehash: 5c0d8067de4a045d6884258f88c09947c27c09c0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463578"
---
# <a name="supported-version--edition-upgrades-sql-server-2017"></a>支持的版本和版本升级 (SQL Server 2017)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  可以从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 和 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 升级。 本文列出了支持的从这些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本进行升级的途径以及支持的 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 版本升级。  
  
## <a name="pre-upgrade-checklist"></a>升级前的清单  
  
-   在从 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 的某个版本升级到另一个版本之前，请确认您当前所用的功能在要移到的版本中受支持。  
  
-   升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]之前，请先为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 启用 Windows 身份验证，并验证默认配置： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务帐户是否是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sysadmin 组的成员。  
  
-   若要升级到 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]，您运行的必须是受支持的操作系统。 有关详细信息，请参阅[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。  
  
-   如果有挂起的重新启动操作，则会阻止升级。  
  
-   如果未运行 Windows Installer 服务，则会阻止升级。  
  
## <a name="unsupported-scenarios"></a>不受支持的方案  
  
-   不支持 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 的跨版本实例。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 组件的版本号在 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 实例中必须相同。  
  
-   [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 仅适用于 64 位平台。 不支持跨平台升级。 不能使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 32 位实例升级到本机 64 位。 但是，如果数据库未在复制过程中发布，则可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的 32 位实例中备份或分离数据库，然后再将它们还原或附加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新实例（64 位）。 必须在 master、msdb 和 model 系统数据库中重新创建任何登录名和其他用户对象。  
  
-   不能在升级现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的过程中添加新功能。 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例升级到 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 之后，您可以使用 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 安装程序添加功能。 有关详细信息，请参阅[向 SQL Server 的实例添加功能（安装程序）](./add-features-to-an-instance-of-sql-server-setup.md)。  
 
-   在 WOW 模式下不支持故障转移群集。  
    
## <a name="upgrades-from-earlier-versions-to-sssqlv14-md"></a>从早期版本升级到 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]  
 
[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 支持从下列 SQL Server 版本升级：
 
- SQL Server 2008 SP4 或更高版本
- SQL Server 2008 R2 SP3 或更高版本
- SQL Server 2012 SP2 或更高版本
- SQL Server 2014 或更高版本 
- SQL Server 2016 或更高版本
 

  
> [!NOTE]  
>  若要在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 上升级数据库，请参阅 [2005 版本支持](#SupportFor2005)。  
  
 下表列出了从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本升级到 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 的支持方案。  
  
|升级前的版本|支持的升级途径|  
|:------|:------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Enterprise|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Developer|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Standard|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Small Business|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Workgroup|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Express |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Datacenter|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Enterprise|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Developer|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Small Business|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Standard|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Workgroup|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Express |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Enterprise|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Developer|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Standard|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Express |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 商业智能|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Evaluation|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Developer|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Standard|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Express |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Evaluation|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Enterprise|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Developer|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Standard|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Express |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|  
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Business Intelligence|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Evaluation|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer|
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 候选发布* |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise |  
|[!INCLUDE[sssqlv14_md](../../includes/sssqlv14-md.md)] Developer |[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise | 

 \* 从候选发布升级的 Microsof 支持专门针对参与了技术采用计划 (TAP) 的客户。 

   
###  <a name="sssqlv14-md-support-for-ssversion2005"></a><a name="SupportFor2005"></a> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 支持  
 此节讨论针对 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 的 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]支持。 在 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 中，您能够执行以下操作：  
  
-   将 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库（mdf/ldf 文件）附加到数据库引擎的 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 实例。  
  
-   从备份将 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库还原为数据库引擎的 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 实例。  
  
-   备份 [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] 多维数据集并在 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 上还原。  
  
当 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库升级到 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 时，该数据库兼容级别将从 90 更改为 100。 （在 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 中，数据库兼容级别的有效值为 100、110、120、130 和 140。）[ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 讨论兼容级别更改可能会影响 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应用程序的方式。  
  
不支持以上列表中未指定的任何方案，包括但不限于以下情况：  
  
- 在相同计算机上安装 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]（并行）。  
  
- 使用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例作为涉及 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 实例的复制拓扑的成员。  
  
- 在 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例之间配置数据库镜像。  
  
- 使用日志传送在 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例之间备份事务日志。  
  
- 在 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例之间配置链接服务器。  
  
- 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Management Studio 管理 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 实例。  
  
- 在 [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] Management Studio 中附加 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 多维数据集。  
  
- 从 [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] Management Studio 连接到 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 。  
  
- 从 [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] Management Studio 管理 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 服务。  
  
- 支持 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 第三方自定义 Integration Services 组件，如执行和升级。  
  
## <a name="sssqlv14-md-edition-upgrade"></a>[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 版本升级  
下表列出了 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]中支持的版本升级方案。  
  
有关如何执行版本升级的分步说明，请参阅[升级到 SQL Server 的不同版本（安装程序）](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md)。  
  
|升级前的版本|升级到的版本|  
|------------------|----------------|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 和内核）**|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise |  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation Enterprise**|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> 对于独立安装，支持从 Evaluation（免费版本）升级到任何付费版本；但对于群集安装，则不支持此升级。 此限制不适用于安装在参与可用性组的 Windows 故障转移群集上的独立实例。|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard**|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 或内核许可证）|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer**|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express*|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 或内核许可证） <br/><br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard <br/> <br/> [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Web|  
  
 此外，您还可以执行 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 或内核许可证）和 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（内核许可证）之间的版本升级：  
  
|版本升级自|版本升级到|  
|--------------------------|------------------------|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 许可证）**|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（内核许可证）|  
|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（内核许可证）|[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Enterprise（服务器+CAL 许可证）|  
  
 \* 同样适用于 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express with Tools 和 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Express with Advanced Services。  
  
 ** 对 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 故障转移群集版本的更改受到限制。 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 故障转移群集不支持以下方案：  
  
-   [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 从 Enterprise 更改为 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Developer、Standard 或 Evaluation。  
  
-   [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 从 Developer 更改为 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard 或 Evaluation。  
  
-   从 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard 更改为 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation。  
  
-   从 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Evaluation 更改为 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] Standard。  
  
## <a name="see-also"></a>另请参阅  

 [SQL Server 2017 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)
 
 [安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 
 [升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
  
