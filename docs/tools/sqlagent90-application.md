---
title: sqlagent90 应用程序
description: sqlagent90 应用程序从命令提示符处启动 SQL Server 代理。 诊断 SQL Server 代理时或你的支持提供商指示时，使用该应用程序。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- starting SQL Server Agent
- sqlagent90 application
- SQL Server Agent, starting
- command prompt utilities [SQL Server], sqlagent90
ms.assetid: e8b80e8d-d0c9-4500-a868-0ce08233da08
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 63f185f686d21c5c9a490bee83d2c9afaf449b8f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641883"
---
# <a name="sqlagent90-application"></a>sqlagent90 应用程序
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  **sqlagent90** 应用程序从命令提示符处启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理。 通常，应从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 或在应用程序中使用 SQL-SMO 方法来运行 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 代理。 只有在诊断 **代理时或主要支持提供商指示你使用命令提示符时，才可以从命令提示符处运行** sqlagent90 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlagent90  
-c [-v] [-i instance_name]  
```  
  
## <a name="arguments"></a>参数  
 **-c**  
 指示 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理从命令提示符运行，并且独立于 Microsoft Windows 服务控制管理器。 使用 **-c** 时，无法从“管理工具”的“服务”应用程序或从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器控制 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理。 此参数是必需的。  
  
 **-v**  
 指示 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理以详细模式运行并向命令提示符窗口写入诊断信息。 该诊断信息与写入 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理错误日志中的信息相同。  
  
 **-i** *instance_name*  
 指示 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理连接到由 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] instance_name *所指定的* 命名实例。  
  
## <a name="remarks"></a>备注  
 在显示版权消息后，只有在指定了 **-v** 开关时， **sqlagent90** 才会在命令提示符窗口中显示输出。 若要停止 **sqlagent90**，请在命令提示符处按 CTRL+C。 在停止 **sqlagent90** 之前，请不要关闭命令提示符窗口。  
  
## <a name="see-also"></a>另请参阅  
 [自动执行管理任务（SQL Server 代理）](../ssms/agent/automated-administration-tasks-sql-server-agent.md)  
  
