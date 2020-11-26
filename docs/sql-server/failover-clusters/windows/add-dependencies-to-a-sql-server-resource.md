---
description: 向 SQL Server 资源添加依赖项
title: 向 SQL Server FCI 资源添加依赖项
descriptoin: Describes how to add dependencies to an Always On failover cluster instance (FCI) resource using the Failover Cluster Manager.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- resource dependencies [SQL Server]
- failover clustering [SQL Server], dependencies
- clusters [SQL Server], dependencies
- dependencies [SQL Server], clustering
ms.assetid: 25dbb751-139b-4c8e-ac62-3ec23110611f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 659d9f1b2cb7e8d7d771b260cbeaf6db1a6e1c13
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127605"
---
# <a name="add-dependencies-to-a-sql-server-resource"></a>向 SQL Server 资源添加依赖项
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍了如何使用故障转移群集管理器管理单元向 AlwaysOn 故障转移群集实例 (FCI) 资源添加依赖项。 故障转移群集管理器管理单元是用于 Windows Server 故障转移群集 (WSFC) 服务的群集管理应用程序。  
  
-   **准备工作：**  [限制和局限](#Restrictions)、[先决条件](#Prerequisites)  
  
-   **向 SQL Server 资源添加依赖项，使用：** [Windows 故障转移群集管理器](#WinClusManager)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 需要特别注意的是：如果要将其他任何资源添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组中，这些资源必须始终具有自身的唯一 SQL 网络名称资源以及自身的 SQL IP 地址资源。  
  
 不要将现有的 SQL 网络名称资源和 SQL IP 地址资源用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]以外的任何资源。 如果将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源与其他资源共享，可能会发生下列问题：  
  
-   可能会发生意外停用。  
  
-   Service Pack 安装可能会失败。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序可能会失败。 如果出现此问题，将无法安装更多的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，也不能执行例程维护。  
  
 还需要考虑下列问题：  
  
-   使用 FTP 进行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制：如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例使用 FTP 进行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制，则 FTP 服务必须与设置为使用 FTP 服务的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装位于同一个物理磁盘。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源依赖关系：如果将某个资源添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组中，并且您具有与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源的依赖关系来确保 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可用，则 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 建议您添加与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理资源的依赖关系。 不要添加与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源的依赖关系。 若要确保运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的计算机保持高可用性，请对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理资源进行合理配置以使 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理资源的失败不会对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组带来影响。  
  
-   文件共享和打印机资源：安装文件共享资源或打印机群集资源时，不应将它们置于运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的计算机的同一物理磁盘资源上。 如果将它们放在相同的物理磁盘资源上，可能导致运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的计算机的性能降低或该计算机上的服务丢失。  
  
-   MS DTC 注意事项：安装操作系统和配置 FCI 后，必须使用故障转移群集管理器管理单元将 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 配置为在群集中使用。 群集 MS DTC 失败不会导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序停止运行，但如果未能正确配置 MS DTC，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 应用程序功能可能会受影响。  
  
     如果在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组中安装 MS DTC，并且具有依赖于 MS DTC 的其他资源，则在此组脱机时或者处于故障转移期间的情况下，MS DTC 将不可用。 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 建议您尽可能将 MS DTC 放入单独的组中且占用单独的物理磁盘资源。  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 如果将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装到具有多个磁盘驱动器的 WSFC 资源组，并选择将数据置于其中一个驱动器上，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源将被设置为只依赖于该驱动器。 若要将数据或日志放到另一个磁盘上，必须先为相应磁盘添加与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源的依赖关系。  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WinClusManager"></a> 使用故障转移群集管理器管理单元  
 **为 SQL Server 资源添加依赖关系**  
  
-   打开故障转移群集管理器管理单元。  
  
-   查找包含要添加依赖关系的可用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源的组。  
  
-   如果磁盘资源已经位于此组中，请转到步骤 4。 否则，请查找包含该磁盘的组。 如果该组与包含 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的组不属于同一节点，请将包含磁盘资源的组移到包含 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组的节点中。  
  
-   选择 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源，打开 **“属性”** 对话框，然后使用 **“依赖项”** 选项卡将磁盘添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 依赖项集中。  
  
  
