---
title: 从故障转移群集实例故障中恢复
description: 了解如何在 SQL Server 中发生故障转移后使用故障转移群集管理器管理单元从故障转移群集实例的故障转移中恢复。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], recovery from failure
- failover clustering [SQL Server], recovery from failure
- hardware failures [SQL Server]
- recovering failover cluster failures [SQL Server]
ms.assetid: 3d151d0c-e841-4325-8606-c094de37d7d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 45b917246037b9c333ba07bece6f6630e0ae2464
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251219"
---
# <a name="recover-from-failover-cluster-instance-failure"></a>从故障转移群集实例故障中恢复
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明在 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]中发生故障转移后如何使用故障转移群集管理器管理单元从群集故障中恢复。 故障转移群集管理器管理单元是用于 Windows Server 故障转移群集 (WSFC) 服务的群集管理应用程序。  
  
-   [从无法修复的故障中恢复](#Scenario1)  
  
-   [从软件故障中恢复](#Scenario2)  
  
##  <a name="recover-from-an-irreparable-failure"></a><a name="Scenario1"></a> 从无法修复的故障中恢复  
 使用以下步骤从无法修复的故障中恢复。 例如，该故障可能是由磁盘控制器或操作系统的故障引起的。 在此情况中，故障是由两节点的群集中节点 1 的硬件故障造成的。  
  
1.  节点 1 失败后， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 故障转移到节点 2。  
  
2.  从 FCI 中逐出节点 1。 为此，从节点 2 打开“故障转移群集管理”器管理单元，右键单击节点 1，单击“移动操作”  ，然后单击“逐出节点”  。  
  
3.  验证节点 1 是否已从群集定义中逐出。  
  
4.  安装新的硬件，以替换节点 1 中发生故障的硬件。  
  
5.  使用故障转移群集管理器管理单元将节点 1 添加到现有群集。 有关详细信息，请参阅 [安装故障转移群集前的准备工作](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)。  
  
6.  确保所有群集节点上的管理员帐户都相同。  
  
7.  运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序以将节点 1 添加到 FCI。 有关详细信息，请参阅[在 SQL Server 故障转移群集中添加或删除节点（安装程序）](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。  
  
##  <a name="recover-from-a-reparable-failure"></a><a name="Scenario2"></a> 从可修复的故障中恢复  
 使用以下步骤从可修复的故障中恢复。 在此情况中，故障由节点 1 关闭或脱机引起，但并非无法挽回。 这可能由操作系统故障、硬件故障或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例本身的故障引起。  
  
1.  节点 1 失败后，FCI 故障转移到节点 2。  
  
2.  解决节点 1 的问题。  
  
3.  确保所有节点处于联机状态且 WSFC 服务正在运行。  
  
4.  将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移到恢复的节点。  
  
  
