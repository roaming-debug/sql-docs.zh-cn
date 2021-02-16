---
title: 验证 SQL Server 安装 | Microsoft Docs
description: SQL Server 发现报告可用于验证 SQL Server 的版本以及计算机上安装的 SQL Server 功能。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- validating installations [SQL Server]
ms.assetid: 1689af50-d2b8-4aa6-8f27-cc7127157fc8
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6a575fd6b5876486a9a1f1a003dab00a881ee1ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353808"
---
# <a name="validate-a-sql-server-installation"></a>验证 SQL Server 安装

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发现报告可用于验证 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本和在计算机上安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。 “已安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能发现报告”显示了在本地服务器上安装的所有 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]、[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]、[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]、[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 和 [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 产品和功能的报告。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心的 **“工具”** 页上提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能发现报告。  
  
 ## <a name="run-ssnoversion-features-discovery-report"></a>运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能发现报告  
  
 使用“开始”菜单，依次指向“所有程序”、“[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] \<Version Name>”、“配置工具”，然后单击“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心”，以启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心    。 若要运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能发现报告，请在“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心”的左侧导航区域中单击“工具”，然后单击“已安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能发现报告”。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发现报告将保存到 %ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\nnn\Setup Bootstrap\Log\\<last Setup Session\>。  
  
 您还可以通过命令行生成发现报告。 从命令提示符运行“Setup.exe /Action=RunDiscovery”。如果将“/q”添加到上述命令行，将不显示 UI，但是仍将在 %ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\nnn\Setup Bootstrap\Log\\<last Setup Session\> 中创建报告。  
  
## <a name="see-also"></a>另请参阅  
 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
