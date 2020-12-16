---
description: 升级 Analysis Services
title: 升级 Analysis Services | Microsoft Docs
ms.custom: ''
ms.date: 12/09/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- upgrading databases
- databases [Analysis Services], upgrading
- installing Analysis Services, side-by-side installations
- Analysis Services upgrades
- Analysis Services upgrades, about upgrading Analysis Services
- SSAS, database migration
- upgrading Analysis Services
- installing Analysis Services, upgrading
- SSAS, upgrading
ms.assetid: a131d329-386e-4470-aaa9-ffcde4e5ec0c
author: Minewiskan
ms.author: owend
monikerRange: '>=sql-server-2016'
manager: erikre
ms.openlocfilehash: 73258d31f23c52915319aa765dd8d9bcbb7f2e35
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480348"
---
# <a name="upgrade-analysis-services"></a>升级 Analysis Services

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  可以将 Analysis Services 实例升级到具有相同服务器模式的 SQL Server 版本，以充分利用当前版本中引入的功能，如 [Analysis Services 中的新增功能](/analysis-services/what-s-new-in-analysis-services)中所述。  
  
 你可以就地升级独立于在同一硬件上运行的其他实例的每个实例。 但是，大多数管理员选择在将生产工作负荷转移到新服务器上之前安装用于应用程序测试的新版本的新实例。 但对于开发或测试服务器来说，就地升级可能会更方便。  
  
## <a name="server-upgrade"></a>服务器升级  
 有两种升级服务器和数据库的基本方法：  
  
> [!NOTE]
> 除非手动更改附加到给定服务器的数据库兼容级别，否则它们保持不变。
   
  
### <a name="in-place-upgrade"></a>就地升级  
 升级过程自动将现有数据库从旧实例迁移到新实例。 因为元数据和二进制数据在两个版本之间兼容，因此升级之后您可以保留该数据而不必手动迁移该数据。  
  
 若要升级现有实例，请运行安装程序并将现有实例的名称指定为新实例的名称。  
  
### <a name="side-by-side-upgrade"></a>并行升级  
  
-   备份所有数据库，并验证每个可以还原的数据库。 有关详细信息，请参阅[备份和还原 Analysis Services 数据库](/analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases)。  
  
-   确定报表、电子表格或仪表板快照的子集，以在稍后用作确认升级后服务器操作的基础。 如果可能，收集性能测量值，以便你可以对在已升级的服务器上运行的相同的工作负载进行比较。  
  
-   安装 Analysis Services 的新实例，选择相同的服务器模式（表格或多维）作为要替换的服务器。 
  
     执行配置端口和添加服务器管理员的安装后的任务。 有关详细信息，请参阅[安装后配置 (Analysis Services)](/analysis-services/instances/post-install-configuration-analysis-services)。  
  
-   附加或还原每个数据库。  
  
-   运行 DBCC 以检查数据库完整性。 与对整个模型层次结构中的孤立对象的测试相比，表格模型经历的检查更彻底。 对于多维模型而言，只会检查分区索引。 有关详细信息，请参阅[适用于 Analysis Services 表格数据库和多维数据库的数据库一致性检查器 (DBCC)](/analysis-services/instances/database-consistency-checker-dbcc-for-analysis-services)。  
  
-   测试报表、电子表格和仪表板，以确认行为或计算没有任何负面更改。 你应看到针对多维和表格工作负荷更快的性能。  
  
-   测试处理操作，更正任何登录或权限问题。 如果使用默认服务帐户进行连接，则新的服务将在不同的帐户下运行。 有关详细信息，请参阅[配置服务帐户 (Analysis Services)](/analysis-services/instances/configure-service-accounts-analysis-services)。  
  
-   在已升级服务器上测试备份和还原操作，调整脚本以使用新的服务器名称。  
  
## <a name="database-upgrade"></a>数据库升级  
 在之前版本中创建的数据库使用原始兼容性级别设置在升级的服务器上运行。 通常情况下，你可以升级数据库或模型以使用更高的兼容性级别获得访问新功能的权限，但请注意，这样做可能将你绑定到特定的服务器版本。  
  
 若要升级数据库，通常应先升级 SQL Server Data Tools (SSDT) 中的模型，然后再将解决方案部署到已升级的服务器实例。
  
 表格和多维数据库遵循不同的路径版本。 巧合的是多维和表格模型具有编号类似的兼容级别。  如果功能更改只影响一个模式，则其将以不同的速率前进。  
  
 为了提供参考，下表总结了兼容性级别，但还应查看详细信息文章，以了解每个级别提供的内容。  
  
|数据库模型|兼容性级别|兼容版本|  
|-|-|-|  
|表格|1500|SQL Server 2019|
|表格|1400|SQL Server 2017|
|表格|1200|SQL Server 2016|  
|表格|1103|SQL Server 2014|  
|表格|1100|SQL Server 2012|  
|多维|1100|SQL Server 2012 和更高版本|  
|多维|1050|SQL Server 2005、2008、2008 R2|  
  
 有关详细信息，请参阅[多维数据库的兼容级别 (Analysis Services)](/analysis-services/multidimensional-models/compatibility-level-of-a-multidimensional-database-analysis-services) 和 [Analysis Services 中表格模型的兼容级别](/analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services)。  
  
## <a name="see-also"></a>另请参阅  
 [计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)   
 [升级 PowerPivot for SharePoint](../../database-engine/install-windows/upgrade-power-pivot-for-sharepoint.md)   
  
