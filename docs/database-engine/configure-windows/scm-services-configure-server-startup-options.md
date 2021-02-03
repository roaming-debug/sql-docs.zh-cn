---
title: 配置服务器启动选项（SQL Server 配置管理器）| Microsoft Docs
description: 了解如何设置 SQL Server 数据库引擎启动时使用的选项。 查看对启动参数进行更改的限制。
ms.custom: ''
ms.date: 11/23/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- parameters [SQL Server], startup options
- SQL Server, startup options
- SQL Server, startup parameters
- single-user mode [SQL Server], starting in
- startup options [SQL Server]
- startup parameters [SQL Server]
- SQL Server services, setting startup options
- SQL Server services, setting startup parameters
ms.assetid: 7a94643c-6460-4baf-bb31-0cb99eaf970d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 13240827e460c1bb64f3ae41e648d4dd44842105
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236664"
---
# <a name="scm-services---configure-server-startup-options"></a>SCM 服务 - 配置服务器启动选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 每次启动时使用的启动选项。 有关启动选项列表，请参阅 [数据库引擎服务启动选项](../../database-engine/configure-windows/database-engine-service-startup-options.md)。  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
### <a name="limitations-and-restrictions"></a>限制和局限  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器将启动参数写入注册表。 这些参数将在下次启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)]时生效。  
  
 在群集上，更改必须在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 处于联机状态的情况下，在活动服务器上进行，并且在重新启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 时生效。 在其他节点上，启动选项的注册表更新将在下次故障转移时进行。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 只有可以更改注册表中的相关项的用户才能配置服务器启动选项。 其中包括以下用户。  
  
-   本地管理员组的成员。  
  
-   如果将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]配置为在域帐户下运行，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 使用该域帐户。  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> 使用 SQL Server 配置管理器  
  
#### <a name="to-configure-startup-options"></a>配置启动选项  
  
1.  单击 **“开始”** 按钮，依次指向 **“所有程序”** 、“ [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]”和 **“配置工具”** ，然后单击 **“SQL Server 配置管理器”** 。  
  
    > [!NOTE]  
    >  因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 管理控制台程序的一个管理单元而不是单独的程序，所以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器在新版本的 Windows 中不显示为一个应用程序。  
    >   
    >  -   **Windows 10**：  
    >          要打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器，请在“起始页” 中键入 SQLServerManager13.msc（适用于 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]）。 对于早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请将 13 替换为较小的数字。 单击“SQLServerManager13.msc”可打开配置管理器。 要将配置管理器固定到“起始页”或“任务栏”，请右键单击“SQLServerManager13.msc”，然后单击“打开文件位置” 。 在“Windows 文件资源管理器”中，右键单击“SQLServerManager13.msc”，然后单击“固定到‘开始’屏幕”  或“固定到任务栏” 。  
    >  -   **Windows 8**：  
    >          若要打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器，请在“搜索”超级按钮中的“应用”下，键入 SQLServerManager\<version>.msc（例如 SQLServerManager13.msc），然后按 Enter。  
  
2.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器中，单击 **“SQL Server 服务”** 。  
  
3.  在右侧窗格中，右键单击 **SQL Server (** _<instance_name>_ **)”** ，然后单击“属性”。  
  
4.  在 **“启动参数”** 选项卡上的 **“指定启动参数”** 框中，键入该参数，然后单击 **“添加”** 。  
  
     例如，若要以单用户模式启动，请在“指定启动参数”  框中键入 **-m** ，然后单击“添加” 。 （以单用户模式重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，请停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理。 否则， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理可能会首先连接，并阻止你作为第二个用户连接。）  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  重新启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
    > [!WARNING]  
    >  结束单用户模式的使用之后，在“启动参数”框中选择“现有参数”  框中的 **-m** 参数，然后单击“删除” 。 重新启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还原为典型的多用户模式。  
  
## <a name="see-also"></a>另请参阅  
 [在单用户模式下启动 SQL Server](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)   
 [在系统管理员被锁定时连接到 SQL Server](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)   
 [启动、停止或暂停 SQL Server 代理服务](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
 [数据库引擎服务启动选项](../../database-engine/configure-windows/database-engine-service-startup-options.md) 
  
