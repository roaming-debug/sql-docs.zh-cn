---
title: 计划 SQL Server 安装 | Microsoft Docs
description: 可参考本文，规划 SQL Server 的安装。 本文提供了 SQL Server 安装所需资源的链接。
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: quickstart
helpviewer_keywords:
- installing SQL Server, planning
ms.assetid: b1d56f2f-603f-48f2-b902-c715f14a6db9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 62b5e5e18743d7425a011e00787ca374e8f90507
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765861"
---
# <a name="planning-a-sql-server-installation"></a>计划 SQL Server 安装
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  若要安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，请按下列步骤操作：  
  
-   查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的安装要求、系统配置检查和安全注意事项。  
  
-   运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序以安装或升级到更高版本。 在升级前，请查看 [升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)。  
  
-   使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实用工具配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
 无论使用哪种安装方法，您都需要作为个人或代表实体确认接受软件许可条款，除非您对于软件的使用受单独的协议（如 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 批量许可协议或与 ISV 或 OEM 之间的第三方协议）管辖。  
  
 将在安装程序用户界面中显示许可条款，供您审核审阅和接受。 无人参与的安装（使用 `/Q` 或 `/QS` 参数）必须包含 `/IAcceptSQLServerLicenseTerms` 参数。 在 [Microsoft SQL Server 许可条款和许可证信息](https://www.microsoft.com/Licensing/product-licensing/sql-server.aspx)中下载和查看许可条款。 有关批量许可条款，请参阅[许可条款和文档](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=53)。 有关 SQL Server 的早期版本，请参阅 [Microsoft 软件许可条款](https://go.microsoft.com/fwlink/?LinkID=148209)。  
  
> [!NOTE]  
>  根据您接收软件的方式（例如，通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 批量许可），您对软件的使用可能受其他条款和条件约束。  
  
## <a name="in-this-section"></a>本节内容  
 [SQL Server 安装中的新增功能](../../sql-server/install/what-s-new-in-sql-server-installation.md)  
 本文介绍有关这一版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中新增或改进的安装功能的详细信息。  
  
 安装 [SQL Server 2016 和 2017](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)、[SQL Server 2019](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) 或 [SQL Server on Linux](../../linux/sql-server-linux-setup.md) 的硬件和软件要求。本文列出了安装和运行 SQL Server 实例的最低硬件和软件要求。 .  
  
 [安装 SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 本文介绍安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 前和安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后应考虑的一些最佳安全做法。  
  
 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
 本文介绍此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中服务的默认配置，以及可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中以及安装之后设置的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的配置选项。  
  
 [网络协议和网络库](../../sql-server/install/network-protocols-and-network-libraries.md)  
 本文介绍这一版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中网络协议的默认配置，以及可用的配置选项。  
  
 [使用 SQL Server 的多个版本和实例](../../sql-server/install/work-with-multiple-versions-and-instances-of-sql-server.md)  
 本文介绍安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的多个版本和实例时的注意事项。  
  
 [SQL Server 中的本地语言版本](../../sql-server/install/local-language-versions-in-sql-server.md)  
 本文介绍了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地化版本。  
  
## <a name="related-sections"></a>相关章节  
 [安装 SQL Server](../../database-engine/install-windows/install-sql-server.md)  
 本节概要介绍用于安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的不同安装选项。  
  
 [安装 SQL Server Business Intelligence 功能](../../sql-server/install/install-sql-server-business-intelligence-features.md)  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装文档中的本节说明如何安装属于 Microsoft BI 平台一部分的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。  
  
 [升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 本节简要说明如何对之前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进行升级。  
  
 [卸载 SQL Server](../../sql-server/install/uninstall-sql-server.md)  
 参照本节可以完全卸载 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] 的现有实例，并且对系统进行准备以便您可以重新安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
 [SQL Server 故障转移群集安装](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装文档中的本节介绍了如何安装和配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集。  
  
## <a name="see-also"></a>另请参阅  
 [从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
 [高可用性解决方案 (SQL Server)](../../database-engine/sql-server-business-continuity-dr.md)   
 [安装故障转移群集前的准备工作](../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)   
 [使用安装向导升级 SQL Server（安装程序）](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)