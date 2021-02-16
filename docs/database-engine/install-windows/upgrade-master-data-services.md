---
title: 升级 Master Data Services | Microsoft Docs
description: 发现用于升级 Microsoft SQL Server Master Data Services 的 4 种方案。 了解文件位置和升级疑难解答。
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 9c3543f3-3eb9-455d-a9bf-f17e9506ad21
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 89905788ffe1b5f62f2c5a49daeee52261e00e73
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344938"
---
# <a name="upgrade-master-data-services"></a>升级 Master Data Services

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  以下是升级 Microsoft [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Master Data Services 的方案。  
  
-   [升级（不升级数据库引擎）](../../database-engine/install-windows/upgrade-master-data-services.md#noengine)  
  
-   [升级（升级数据库引擎）](../../database-engine/install-windows/upgrade-master-data-services.md#engine)  
  
-   [在两台计算机上执行升级的方案](../../database-engine/install-windows/upgrade-master-data-services.md#twocomputer)  
  
-   [通过从备份还原数据库升级](../../database-engine/install-windows/upgrade-master-data-services.md#restore)  
  
> [!IMPORTANT]  
> -   不支持从 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CTP1 版升级到 CTP2 版。  
> -   在执行任何升级之前备份您的数据库。  
> -   升级过程将重新创建存储过程并对 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]所使用的表进行升级。 您对这些组件中的任何一个所做的任何自定义内容可能会丢失。  
> -   模型部署包只能在创建它们的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中使用。 不能将在 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中创建的模型部署包部署到 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]中。  
> -   在将 Data Quality Services 和 Master Data Services 升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]后，用于 Excel 的 Master Data Services 外接程序的任何早期版本都将不再适用。 可以从[适用于 Microsoft Excel 的 Master Data Services 加载项](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)下载适用于 Excel 的 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Master Data Services 加载项。  
  
##  <a name="file-location"></a><a name="fileLocation"></a> 文件位置  
  
-   在 [!INCLUDE[ss2017](../../includes/sssql17-md.md)] 中，默认情况下这些文件安装在 drive:\Program Files\Microsoft SQL Server\140\Master Data Services中。  

-   在 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 中，默认情况下这些文件安装在驱动器:\Program Files\Microsoft SQL Server\130\Master Data Services 中。  
  
-   在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中，默认情况下这些文件安装在驱动器:\Program Files\Microsoft SQL Server\120\Master Data Services 中。  
  
-   在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中，默认情况下这些文件安装在驱动器:\Program Files\Microsoft SQL Server\110\Master Data Services 中。  
  
-   在 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 中，默认情况下这些文件安装在驱动器:\Program Files\Microsoft SQL Server\Master Data Services 中。  
  
##  <a name="upgrade-without-database-engine-upgrade"></a><a name="noengine"></a> 升级（不升级数据库引擎）  
 在此方案中，继续使用 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 托管 MDS 数据库。 但是，必须升级 MDS 数据库的架构，然后创建最新的 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] Web 应用程序来访问 MDS 数据库。 升级后，无法再通过早期版本的 Web 应用程序访问 MDS 数据库。  
  
 可以在同一台计算机上安装最新的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 和早期版本的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]。 文件安装在不同的位置，如 [文件位置](#fileLocation)中所示。  
  
 **升级（不升级数据库引擎）**  
  
1.  安装 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 和所需的任何其他功能。  
  
    1.  打开 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] 安装向导。  
  
    2.  在左窗格中，单击 **“安装”** 。  
  
    3.  在右窗格中，单击“全新 SQL Server 独立安装或向现有安装添加功能”。  
  
    4.  在 **“功能选择”** 页上，选择 **[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]** 和要安装的任何其他功能。  
  
    5.  完成向导。  
  
2.  升级 MDS 数据库架构。  
  
    1.  打开当前的 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]。  
  
        > [!IMPORTANT]  
        >  若要升级 MDS 数据库架构，您必须以在创建 MDS 数据库时指定的管理员帐户登录。 在 MDS 数据库的 mdm.tblUser 中，此用户的 **ID** 值为 **1**。  
  
    2.  在左窗格中单击 **“数据库配置”** 。  
  
    3.  在右窗格中，单击“选择数据库”并指定 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 数据库实例的信息。  
  
    4.  单击 **“升级数据库”** 以启动 **“升级数据库向导”** 。 有关详细信息，请参阅[升级数据库向导（Master Data Services 配置管理器）](../../master-data-services/upgrade-database-wizard-master-data-services-configuration-manager.md)。  
  
3.  创建 Web 应用程序。  
  
    1.  打开当前的 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]。  
  
    2.  在左窗格中单击 **“Web 配置”** 。  
  
    3.  在右窗格中，从 **“网站”** 列表选择以下选项之一：  
  
        -   **“默认网站”** ，然后单击 **“创建应用程序”** 。  
  
        -   **“创建新站点”** 。 创建网站时，将自动创建新的 Web 应用程序。  
  
        > [!IMPORTANT]  
        >  在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 版 Master Data Services 配置管理器中，可以选择 SQL Server 早期版本（[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]）的现有 MDS Web 应用程序。 您不能选择现有 Web 应用程序，而是必须为 MDS 创建一个 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Web 应用程序。 否则，在您尝试将 Web 应用程序与升级的 MDS 数据库关联时，您会收到错误，指出无法访问请求的页面，因为该页的相关配置数据无效。  
        >   
        >  如果要为 MDS Web 应用程序使用与现有（[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]）Web 应用程序相同的名字（别名），则必须首先从 IIS 中删除该 Web 应用程序和关联的应用程序池，然后使用 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 版 Master Data Services 配置管理器创建同名的 Web 应用程序。 有关从 IIS 删除 Web 应用程序和应用程序池的信息，请参阅 [删除应用程序 (IIS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771205(v=ws.10)) 和 [删除应用程序池 (IIS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772406(v=ws.10))。  
  
4.  将新 Web 应用程序与已升级的 MDS 数据库关联。  
  
    1.  在 **“将应用程序与数据库相关联”** 部分中，单击 **“选择”** 。  
  
    2.  选择 MDS 数据库。  
  
    3.  单击“应用”。  
  
##  <a name="upgrade-with-database-engine-upgrade"></a><a name="engine"></a> 升级（升级数据库引擎）  
 此方案将数据库引擎和 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 应用程序均从早期版本升级到 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 或 [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)]。  
  
 **升级（要升级数据库引擎）**  
  
1.  仅适用于 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]：打开“控制面板” > “程序和功能”，然后卸载 Microsoft [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)][!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 。  
  
2.  将数据引擎升级到 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 或 [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)]。 有关详细信息，请参阅 [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)。  
  
3.  完成 [升级（不升级数据库引擎）](#noengine) 中的所有步骤。  
  
##  <a name="upgrade-in-two-computer-scenario"></a><a name="twocomputer"></a> 在两台计算机上执行升级的方案  
 此方案涉及升级一个系统，在该系统中要在两台计算机上安装 SQL Server：一台计算机安装 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 或 [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)]，另一台计算机安装早期版本的 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]。  
  
 如果安装早期版本的 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]，可继续使用早期版本在一台计算机上托管 MDS 数据库。 但是，必须升级 MDS 数据库的架构，然后分别使用 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 或 [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] Web 应用程序访问 MDS 数据库。 无法再通过早期版本的 Web 应用程序访问 MDS 数据库。  
  
 **在两台计算机中升级的方案**  
  
-   完成 [升级（不升级数据库引擎）](#noengine)中的所有步骤。  
  
##  <a name="upgrade-with-restoring-a-database-from-backup"></a><a name="restore"></a> 通过从备份还原数据库升级  
 在此方案中，[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 或 [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] 将随同一台计算机或两台不同计算机上的早期版本一起安装。 在低于 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 或 [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] 的版本上备份数据库，升级之前，必须还原该数据库。  
  
 **通过从备份还原数据库升级**  
  
1.  安装 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 和所需的任何其他功能。  
  
    1.  打开 [!INCLUDE[sssnoversion](../../includes/ssnoversion-md.md)] 安装向导。  
  
    2.  在左窗格中，单击 **“安装”** 。  
  
    3.  在右窗格中，单击“全新 SQL Server 独立安装或向现有安装添加功能”。  
  
    4.  在 **“功能选择”** 页上，选择 **[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]** 和要安装的任何其他功能。  
  
    5.  完成向导。  
  
2.  还原已备份的数据库。  
  
3.  升级 MDS 数据库架构、创建 Web 应用程序，并将新的 Web 应用程序与已升级的 MDS 数据库相关联。 有关说明，请参阅 [升级（不升级数据库引擎）](#noengine)中的步骤 2 - 4  
  
## <a name="troubleshooting"></a>疑难解答  
 **问题：** 打开 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Web 应用时，出现“客户端版本与数据库版本不兼容”的错误消息。  
  
 **解决方案：** 当 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 主数据管理器 Web 应用尝试访问已升级到 [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] Master Data Services 的数据库时，会发生此问题。 必须改用 [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] Web 应用程序。  
  
 如果您在升级 MDS 数据库架构时没有在 IIS 中停止并重新启动 **“MDS 应用程序池”** ，则也可能出现此问题。 重新启动 **“MDS 应用程序池”** 可解决此问题。  
  
## <a name="see-also"></a>另请参阅  
 [安装 Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
