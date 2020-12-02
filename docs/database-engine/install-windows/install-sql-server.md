---
title: SQL Server 安装指南
description: 内容索引，可帮助你使用安装向导、命令提示符或 sysprep 等选项安装 SQL Server 和关联组件。
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- AdventureWorks sample database
- installing SQL Server, preparing to install
- installation [SQL Server]
ms.assetid: 0300e777-d56b-4d10-9c33-c9ebd2489ee5
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 46a83efe3efcab212d3569c5f2f32c232f1962c5
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125862"
---
# <a name="sql-server-installation-guide"></a>SQL Server 安装指南

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

本文是内容索引，提供关于在 Windows 上安装 SQL Server 的指南。

有关其他部署方案，请参阅：

- [Linux](../../linux/sql-server-linux-setup.md)
- [Docker 容器](../../linux/sql-server-linux-docker-container-deployment.md)
- [Kubernetes - 大数据群集](../../big-data-cluster/deploy-get-started.md)

从 [!INCLUDE[sssql15](../../includes/sssql15-md.md)] 开始，[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 仅可用作 64 位应用程序。 以下是有关如何获取 SQL Server 以及如何安装 SQL Server 的重要详细信息。

## <a name="getting-started"></a>入门
  
* **版本和功能**：查看不同版本 SQL Server 支持的功能，以确定最适合你业务需求的功能。 
    - [[!INCLUDE[ss2019](../../includes/sssqlv15-md.md)]](~/sql-server/editions-and-components-of-sql-server-version-15.md)：  
    - [[!INCLUDE[ss2017](../../includes/sssqlv14-md.md)]](~/sql-server/editions-and-components-of-sql-server-2017.md)：  
    - [[!INCLUDE[ss2016](../../includes/sssql15-md.md)]](~/sql-server/editions-and-components-of-sql-server-2016.md)：  
    - [[!INCLUDE[ss2014](../../includes/sssql14-md.md)]](https://docs.microsoft.com/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014)

*  **要求**：查看 [SQL Server 2016 和 2017](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)、[SQL Server 2019](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) 或 [Linux 上的 SQL Server](../../linux/sql-server-linux-setup.md) 的硬件和软件安装要求，以及系统配置检查和 [规划 SQL Server 安装中](../../sql-server/install/planning-a-sql-server-installation.md)的安全注意事项 


  
* **示例数据库和示例代码**： 
    * 默认情况下，它们不作为 SQL Server 安装程序的一部分安装，但可以找到它们 
    * 要为非 Express 版本的 SQL Server 安装它们，请参阅[示例存储库](../../samples/sql-samples-where-are.md)
    

## <a name="installation-media"></a>安装媒体

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的下载位置由版本决定：

* “SQL Server Enterprise、Standard 和 Express Edition”已获授权，可供生产之用。 对于企业版和标准版，请联系软件供应商获取安装媒体。 可以在 [Microsoft 许可页面](https://www.microsoft.com/licensing/product-licensing/sql-server)上找到采购信息和 Microsoft 合作伙伴目录。
* [免费版本 - 最新](https://www.microsoft.com/sql-server/sql-server-downloads)
* [免费版本 - 其他](https://www.microsoft.com/evalcenter/evaluate-sql-server)


可在此处找到其他 SQL Server 组件： 

* [所有累积更新](https://sqlserverbuilds.blogspot.com/)
* [SQL Server Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122)。 
* [SQL Server Management Studio](https://aka.ms/ssmsfullsetup)
* [Azure Data Studio](https://go.microsoft.com/fwlink/?linkid=2109256)


## <a name="considerations"></a>注意事项

-   如果通过远程桌面连接 RDC 客户端上本地资源中的介质来启动安装程序，安装将会失败。 若要执行远程安装，介质必须处于网络共享状态，或是物理计算机或虚拟机的本地介质。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质要么处于网络共享状态，要么是映射的驱动器、本地驱动器，或者是虚拟机的 ISO。  
  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序安装该产品所需的以下软件组件：  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client    
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序支持文件  

## <a name="sql-server-installation"></a>SQL Server 安装


|项目|说明|  
|-----------|-----------------|  
|[安装向导](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)|使用从 setup.exe 安装媒体启动的安装向导 GUI 安装 SQL Server。 |  
|[命令提示符](./install-sql-server-from-the-command-prompt.md)|用于从命令提示符运行 SQL Server 安装的示例语法和安装参数。 | 
|[Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md)|在 Windows Server Core 上安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]。|  
|[系统配置检查器的检查参数](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)|讨论系统配置检查器 (SCC) 的功能。|   
|[配置文件](./install-sql-server-using-a-configuration-file.md)|用于通过配置文件运行安装程序的示例语法和安装参数。|  
|[SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)|用于通过 SysPrep 运行安装程序的示例语法和安装参数。|
|[将功能添加到实例](./add-features-to-an-instance-of-sql-server-setup.md)|更新 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 现有实例的组件。|  
|[SQL Server 故障转移群集安装](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)| 安装 SQL Server 故障转移群集实例。  | 
|[修复失败的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 安装](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)|修复损坏的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 安装。|  
|[使用 SQL Server 重命名计算机](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)|在重命名托管 SQL Server 独立实例的计算机的主机名之后，更新存储在 sys.servers 中的系统元数据。 |  
|[安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 服务更新](../../database-engine/install-windows/install-sql-server-servicing-updates.md)|安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的更新。|  
|[安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)| 查看和读取 SQL Server 安装程序日志文件中的错误。 |  
|[验证安装](../../database-engine/install-windows/validate-a-sql-server-installation.md)|查看 SQL 发现报告的用法，以便确认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本以及在计算机上安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。|  
  
  
## <a name="individual-component-installation"></a>单个组件安装 
  
|项目|说明|  
|-----------|-----------------|  
|[SQL Server 数据库引擎](../../database-engine/install-windows/install-sql-server-database-engine.md)|安装和配置 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]。|  
|[SQL Server 复制](../../database-engine/install-windows/install-sql-server-replication.md)|安装和配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制。|  
|[Distributed Replay](../../tools/distributed-replay/install-distributed-replay-overview.md)|列出了有关安装 Distributed Replay 功能的文章。|  
|[带有 SSMS 的 SQL Server 管理工具](../../ssms/download-sql-server-management-studio-ssms.md)|安装和配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理工具。|  
|[SQL Server PowerShell](../../database-engine/install-windows/install-sql-server-powershell.md)|安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 组件的注意事项。|  
  

## <a name="sql-server-configuration"></a>SQL Server 配置 
  
|项目|说明|  
|-----------|-----------------|  
|[配置 Windows 防火墙 (SQL Server)](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|概述防火墙配置以及如何配置 Windows 防火墙以允许访问 SQL Server。|  
|[配置 Windows 防火墙 (SSAS)](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)|配置端口和防火墙设置，以允许访问 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 或用于 SharePoint 的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]。|  
|[配置多宿主计算机](../../sql-server/install/configure-a-multi-homed-computer-for-sql-server-access.md)|配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和具有高级安全性的 Windows 防火墙，以便在多宿主环境中与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例建立网络连接。|  

 
## <a name="see-also"></a>另请参阅  

[升级 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/upgrade-sql-server.md)   
[卸载 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/install/uninstall-sql-server.md)   
[安装 SQL Server Reporting Services (SSRS)](../../reporting-services/install-windows/install-reporting-services.md)   
[安装 SQL Server Analysis Services (SSAS)](/analysis-services/instances/install-windows/install-analysis-services)   
[安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 商业智能功能](../../sql-server/install/install-sql-server-business-intelligence-features.md)   
[高可用性解决方案 (SQL Server)](../sql-server-business-continuity-dr.md)