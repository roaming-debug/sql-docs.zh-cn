---
description: Change Data Capture Service for Oracle by Attunity 系统体系结构
title: Change Data Capture Service for Oracle by Attunity 系统体系结构 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 1db6c737-3c60-4066-a0a3-3611e1c83e4e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a8d184639991b7e7a0d5e837fabe95535fdc08fa
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765674"
---
# <a name="change-data-capture-service-for-oracle-by-attunity-system-architecture"></a>Change Data Capture Service for Oracle by Attunity 系统体系结构

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Oracle CDC 服务将对一个或多个源 Oracle 数据库中所选表的更改捕获到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC 数据库中。 下面的关系图显示构成 Oracle CDC 服务的各组件。  
  
 ![服务体系结构](../../integration-services/change-data-capture/media/service-architecture.gif "服务体系结构")  
  
 下图阐释使用的四个平台。 在许多情况下，这些平台可以重叠，但此关系图表示一个标准用例。 例如，Oracle 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库分别运行在单独的计算机上并且不与 Oracle CDC 服务平台或用于设计 CDC 服务的平台共享是有意义的。 在此图中阐释的平台是：  
  
-   Oracle CDC 服务：可以是安装和运行了 Oracle CDC 服务的任何支持的 Windows 计算机。 此平台还可以表示 Microsoft 故障转移群集中的某一群集节点（在本文档的后面将论述高可用性配置）。  
  
-   Oracle 数据库：可以是运行支持的 Oracle 数据库版本的任何计算机。 这包括运行 Windows、Linux 或所安装的 Oracle 数据库版本支持的任何其他操作系统的任何计算机。 请注意，该关系图以复数形式显示此平台，因为单个 Oracle CDC 服务可以从多个源 Oracle 数据库捕获更改。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]：可以是运行目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库（ [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]的支持的 SKU）的任何计算机。 一个 Oracle CDC 服务支持用于存储更改表和服务配置的一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 平台还可以表示 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的群集实例或使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] AlwaysOn **功能的** 的镜像实例。  
  
-   Oracle CDC 设计器：可以是能够访问源 Oracle 数据库和目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的任何支持的 Windows 计算机。  
  
 下表描述了在上述四个平台上运行的组件。  
  
|组件/说明|组件由以下项构成：|  
|----------------------------|----------------------------|  
|Oracle CDC 服务：这是发生了变更数据捕获活动的 Windows 服务。|Oracle CDC 实例：Oracle CDC 服务的子进程，用来处理单个源 Oracle 数据库（每个源 Oracle 数据库有一个 Oracle CDC 实例）的变更数据捕获活动。|  
||Oracle 日志读取器：使用 Oracle 客户端读取 Oracle 事务日志。|  
||Oracle 客户端：用于与 Oracle 进行通信的 Oracle 即时客户端。 这是应从 Oracle 获取并在安装 Oracle CDC 服务之前安装的必备组件。|  
||[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更改编写器：它将对捕获的 Oracle 表的提交的更改写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]更改表。 此组件还在目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中维护该捕获状态。|  
||[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ODBC 客户端：用于 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]的 Microsoft Native Client。 这是应从 Microsoft 获取并在安装 Oracle CDC 服务之前安装的必备组件。|  
|Oracle CDC 服务配置：这是一种 Microsoft 管理控制台管理单元，它创建 Windows 服务并且设置其配置。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端：.NET framework 的版本 4 随附的 SQL ADO.NET 客户端。|  
|Oracle 数据库：从其捕获对所选表的更改的源 Oracle 数据库。|日志挖掘器：通过其读取 Oracle 事务日志的一种 Oracle 组件。|  
||事务日志：联机并存档的 Oracle 重做日志，Oracle 使用这些日志来确保数据库可以回滚事务并且从失败中恢复（在该情况下，Oracle 数据库必须在存档日志模式下操作）。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例：承载 CDC 数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 该实例可以是群集 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例（故障转移群集）或镜像数据库 (AlwaysOn)。|MSXDBCDC 数据库：一种数据库，使用此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 CDC 服务的有关信息保存在此数据库中。 它还保存与每个 CDC 服务处理的 Oracle CDC 实例有关的信息。 该数据库作为 CDC 服务创建进程的一部分创建。|  
||CDC 数据库：存储对源 Oracle 数据库之一进行的更改的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 将为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC 启用 CDC 数据库，以便它们具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC 表和函数，从而轻松地使用来自 Oracle 的更改。|  
|Oracle CDC 设计器：可帮助创建 Oracle CDC 实例的一种 Microsoft 管理控制台管理单元。 使用此管理单元可以选择要捕获的表和列，提供 Oracle 连接信息以及管理 CDC 实例的生命周期。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端：.NET framework 的版本 4 随附的 SQL ADO.NET 客户端。|  
||Oracle 客户端：用于与 Oracle 进行通信的 Oracle 即时客户端。 这是应从 Oracle 获取并在安装 Oracle CDC 服务之前安装的必备组件。|  
  
 Oracle CDC 服务及其子 Oracle CDC 实例只能与作为客户端的源 Oracle 数据库和目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例通信。 它们不主动侦听任何网络和其他协议。 Oracle CDC 服务监视 CDC 数据库中是否有配置更改，并且基于更新的配置更新其操作。  
  
  
