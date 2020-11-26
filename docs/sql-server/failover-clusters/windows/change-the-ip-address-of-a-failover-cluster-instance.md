---
title: 更改故障转移群集实例的 IP 地址
description: 了解如何使用故障转移群集管理器更改 SQL Server 故障转移群集实例的 IP 地址。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- modifying IP addresses
- failover clustering [SQL Server], IP addresses
- IP addresses [SQL Server]
- clusters [SQL Server], IP addresses
ms.assetid: b685f400-cbfe-4c5d-a070-227a1123dae4
author: cawrites
ms.author: chadam
ms.openlocfilehash: fce7ec31d8cc3ff8e3c9b0cbbd4a25039c998419
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127698"
---
# <a name="change-the-ip-address-of-a-failover-cluster-instance"></a>更改故障转移群集实例的 IP 地址
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用故障转移群集管理器管理单元更改 AlwaysOn 故障转移群集实例 (FCI) 中的 IP 地址资源。 故障转移群集管理器管理单元是用于 Windows Server 故障转移群集 (WSFC) 服务的群集管理应用程序。  
  
-   **开始之前：** [安全性](#Security)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 在开始之前，请查阅以下 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书主题：[安装故障转移群集前的准备工作](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 若要维护或更新 FCI，您必须在 FCI 的所有节点上是拥有“作为服务登录”权限的本地管理员。  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WSFC"></a> 使用故障转移群集管理器管理单元  
 **更改 FCI 的 IP 地址资源**  
  
1.  打开故障转移群集管理器管理单元。  
  
2.  在左窗格中展开 **“服务和应用程序”** 节点，然后单击 FCI。  
  
3.  在右窗格的  “服务器名称”类别下，右键单击该 SQL Server 实例，然后选择  “属性”选项以打开  “属性”对话框。  
  
4.  在 **“常规”** 选项卡上更改 IP 地址资源。  
  
5.  单击 **“确定”** 关闭对话框。  
  
6.  在右侧窗格中，右键单击“SQL IP Address1”（故障转移群集实例名称）并选择  “脱机”。 将会看到 SQL IP Address1（故障转移群集实例名称）、SQL Network Name（故障转移群集实例名称）和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的状态从联机更改为脱机挂起，然后再更改为脱机。  
  
7.  在右侧窗格中，右键单击 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，然后选择  “联机”。 将会看到 SQL IP Address1（故障转移群集实例名称）、SQL Network Name（故障转移群集实例名称）和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的状态从脱机更改为联机挂起，然后再更改为联机。  
  
8.  关闭故障转移群集管理器管理单元。  
  
  
