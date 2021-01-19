---
title: SCM 服务 - 更改所用帐户的密码 | Microsoft Docs
description: 了解如何更改数据库引擎和 SQL Server 代理所使用帐户的密码。 了解什么时候一定要更改密码。
ms.custom: ''
ms.date: 01/06/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- expired password [SQL Server], SQL Server Agent
- passwords [SQL Server], SQL Server Agent service
- passwords [SQL Server], changing
- expired password [SQL Server], Database Engine
- passwords [SQL Server], SQL Server service
- Database Engine [SQL Server], passwords
- changing passwords used by SQL Server
- modifying passwords
ms.assetid: 5b6dcc03-6cae-45d3-acef-6f85ca6d615f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d6b639f6bc5a8a5bebdca351a5fce2bd713f0b30
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171019"
---
# <a name="scm-services---change-the-password-of-the-accounts-used"></a>SCM 服务 - 更改所用帐户的密码
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 SQL Server 配置管理器在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 代理使用的帐户的密码。 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作为服务在计算机上运行，并使用最初在设置期间提供的凭据。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例使用域帐户运行但此帐户的密码已更改，则必须将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所用的密码更新为新密码。 如果不更新密码， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能无法访问某些域资源；如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 停止，则在更新密码后才能重新启动该服务。  
  
 若要更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证密码，请参阅 [密码已过期](../../relational-databases/security/choose-an-authentication-mode.md)。  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器是为更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务设置而设计和授权使用的工具。 使用 Windows 服务控制管理器 ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] services.msc **) 应用程序更改** 服务不总是更改所有必要设置，并且可能会阻止服务正常运行。 但是在群集环境中，在使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器更改活动节点上的密码之后，您必须使用服务控制管理器来更改被动节点上的密码。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 您必须是计算机管理员才能更改服务所用的密码。  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> 使用 SQL Server 配置管理器  
  
#### <a name="to-change-the-password-used-by-the-sql-server-database-engine-service"></a>更改 SQL Server（数据库引擎）服务所用的密码  
  
1.  单击 **“开始”** 按钮，依次指向 **“所有程序”** 、“ [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]”和 **“配置工具”** ，然后单击 **“SQL Server 配置管理器”** 。  
  
    > [!NOTE]  
    >  因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 管理控制台程序的一个管理单元而不是单独的程序，所以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器在新版本的 Windows 中不显示为一个应用程序。  
    >   
    >  -   **Windows 10**：  
    >          要打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器，请在“起始页” 中键入 SQLServerManager13.msc（适用于 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]）。 对于早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请将 13 替换为较小的数字。 单击“SQLServerManager13.msc”可打开配置管理器。 要将配置管理器固定到“起始页”或“任务栏”，请右键单击“SQLServerManager13.msc”，然后单击“打开文件位置” 。 在“Windows 文件资源管理器”中，右键单击“SQLServerManager13.msc”，然后单击“固定到‘开始’屏幕”  或“固定到任务栏” 。  
    > -   **Windows 8**：  
    >          若要打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器，请在“搜索”超级按钮中的“应用”下，键入 SQLServerManager\<version>.msc（例如 SQLServerManager13.msc），然后按 Enter    。  
  
2.  在 SQL Server 配置管理器中，单击 **“SQL Server 服务”** 。  
  
3.  在“详细信息”窗格中，右键单击“SQL Server (\<instancename> )”，再单击“属性”。  
  
4.  在“SQL Server (\<instancename>) 属性” 对话框中的“登录”选项卡上，对于“帐户名”框中列出的帐户，在“密码”框和“确认密码”框中键入新密码，然后单击“确定”   。  
  
     密码会立即生效，而不需要重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
#### <a name="to-change-the-password-used-by-the-sql-server-agent-service"></a>更改 SQL Server 代理服务所用的密码  
  
1.  单击 **“开始”** 按钮，依次指向 **“所有程序”** 、“ [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]”和 **“配置工具”** ，然后单击 **“SQL Server 配置管理器”** 。  
  
2.  在 SQL Server 配置管理器中，单击 **“SQL Server 服务”** 。  
  
3.  在“详细信息”窗格中，右键单击“SQL Server 代理(\<instancename> )”，再单击“属性”。  
  
4.  在“SQL Server 代理(\<instancename>)属性”对话框中的“登录”选项卡上，对于“帐户名”框中列出的帐户，在“密码”框和“确认密码”框中键入新密码，然后单击“确定”     。  
  
     在独立的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上，密码会立即生效，无需重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 在群集实例上， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源脱机，并需要重新启动。  
  
## <a name="see-also"></a>另请参阅  
 [管理服务操作指南主题（SQL Server 配置管理器）](./scm-services-connect-to-another-computer.md)  
  
