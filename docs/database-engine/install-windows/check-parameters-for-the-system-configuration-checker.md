---
title: 检查参数系统配置检查器
description: 在 SQL Server 安装过程中，系统配置检查器用于检查阻止成功安装 SQL Server 的情况。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- installing SQL Server, system configuration checks
- failed system configuration checks [SQL Server]
- verifying configuration before installation
- SCC [SQL Server]
- system configuration checker
- scanning computer before installation [SQL Server]
- checking configuration before installation
- status information [SQL Server], system configuration checker
- parameters [SQL Server], system configuration checker
- configuration checkers [SQL Server]
- Setup [SQL Server], system configuration checker
ms.assetid: 8e712c15-6bfa-4d71-b303-9526101e5594
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 541247d27df04953ca91daa4dcd99386170f9ea0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347849"
---
# <a name="check-parameters-for-the-system-configuration-checker"></a>系统配置检查器的检查参数

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中，系统配置检查器 (SCC) 会对将要安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机进行扫描。 SCC 将检查会使安装无法成功 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的情况。 在安装程序启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导之前，SCC 会检索每个项的状态。 然后，将检索结果与所需条件进行比较并提供如何排除妨碍性问题的指导。  
  
系统配置检查器将会生成一个报告，该报告包含有关每个执行规则的简短说明以及执行状态。 该系统配置检查报告位于 %programfiles%\Microsoft SQL Server\140\Setup Bootstrap\Log\\\<YYYYMMDD_HHMM>\\\.    
  
有关详细信息，请参阅下列链接：

- [安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
- [安装 SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
- [支持的版本和版本升级](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)  
  
  
