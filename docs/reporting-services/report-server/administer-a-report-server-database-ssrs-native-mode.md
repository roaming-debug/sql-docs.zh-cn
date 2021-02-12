---
title: 管理报表服务器数据库（本机模式）| Microsoft Docs
description: 了解如何管理 Reporting Services 部署，包括报表服务器数据库的备份和还原，以及如何管理加密密钥。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], databases
- renaming databases
- report server database
- databases [Reporting Services], administering
- reportservertempdb
- reportserver database
ms.assetid: 97b2e1b5-3869-4766-97b9-9bf206b52262
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2ae57de490fcb516b0014ea2484824028c4c1a38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021999"
---
# <a name="administer-a-report-server-database-ssrs-native-mode"></a>管理报表服务器数据库（SSRS 本机模式）
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 部署将两个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关系数据库用作内部存储。 默认情况下，这两个数据库分别命名为 ReportServer 和 ReportServerTempdb。 ReportServerTempdb 随报表服务器主数据库一同创建，用于存储临时数据、会话信息和缓存的报表。  
  
 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]中，数据库管理任务包括备份和还原报表服务器数据库，以及管理用于加密和解密敏感数据的加密密钥。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了许多用来管理报表服务器数据库的工具。  
  
-   若要备份或还原报表服务器数据库、移动报表服务器数据库或恢复报表服务器数据库，可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]、[!INCLUDE[tsql](../../includes/tsql-md.md)] 命令或数据库命令提示实用工具。 有关说明，请参阅[将报表服务器数据库移至其他计算机（SSRS 本机模式）](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md)。  
  
-   若要将现有数据库内容复制到另一个报表服务器数据库，可以附加报表服务器数据库的一个副本，并将其用于其他报表服务器实例。 或者，可以创建并运行一个使用 SOAP 调用的脚本，以便在新数据库中重新创建报表服务器。 可以使用 **rs** 实用工具来运行该脚本。  
  
-   若要管理报表服务器与报表服务器数据库之间的连接，以及查找用于特定报表服务器实例的数据库，可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]配置工具中的“数据库安装”页。 若要了解有关将报表服务器连接到报表服务器数据库的详细信息，请参阅[配置报表服务器数据库连接（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)。  
  
## <a name="sql-server-login-and-database-permissions"></a>SQL Server 登录名和数据库权限  
 报表服务器数据库由报表服务器在内部使用。 报表服务器服务可建立到任一数据库的连接。 可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具来配置报表服务器与报表服务器数据库的连接。  
  
 报表服务器到数据库的连接凭据可以是服务帐户、Windows 本地或域用户帐户或者 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库用户。 您必须为连接选择现有的帐户，因为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不会为您创建帐户。  
  
 系统会自动为您指定的帐户创建一个用来登录报表服务器数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录。  
  
 数据库权限也是自动配置的。 Reporting Services 配置工具将向帐户或数据库用户授予对报表服务器数据库的 **Public** 和 **RSExecRole** 角色。 **RSExecRole** 提供了用于访问数据库表和执行存储过程的权限。 **RSExecRole** 是在创建报表服务器数据库时在 master 和 msdb 数据库中创建的。 **RSExecRole** 是报表服务器数据库 **db_owner** 角色的成员，它允许报表服务器更新其架构以支持自动升级过程。  
  
## <a name="naming-conventions-for-the-report-server-databases"></a>报表服务器数据库的命名约定  
 创建主数据库时，数据库名称必须遵循为 [数据库标识符](../../relational-databases/databases/database-identifiers.md)指定的规则。 临时数据库名称始终与报表服务器主数据库的名称相同，但是带有 Tempdb 后缀。 您不能为临时数据库选择其他名称。  
  
 由于报表服务器数据库被视为内部组件，因此不支持对其进行重命名。 如果重命名报表服务器数据库，则会出现错误。 具体来说，如果重命名主数据库，则将显示一条错误消息，说明数据库名称不同步。如果重命名 ReportServerTempdb 数据库，则稍后运行报表时将出现以下内部错误：  
  
 “报表服务器上出现内部错误。 有关详细信息，请参阅错误日志。 (rsInternalError)  
  
 对象名‘ReportServerTempDB.dbo.PersistedStream’无效。”  
  
 由于 ReportServerTempdb 名称是在内部存储的，并且由存储过程用来执行内部操作，所以会发生此错误。 重命名临时数据库将使存储过程无法正常工作。  
  
## <a name="enabling-snapshot-isolation-on-the-report-server-database"></a>针对报表服务器数据库启用快照隔离  
 您不能针对报表服务器数据库启用快照隔离。 如果启用了快照隔离，则会遇到以下错误：“所选报表尚不可查看。 报表仍处于呈现状态，或报表快照不可用。”  
  
 如果快照隔离不是有意启用的，则说明属性可能已经由另一个应用程序设置，或者已经针对 **“模型”** 数据库启用了快照隔离，从而导致所有的新数据库都继承该设置。  
  
 若要针对报表服务器数据库关闭快照隔离，请启动 Management Studio，打开一个新的查询窗口，粘贴下面的脚本，然后运行该脚本：  
  
```  
ALTER DATABASE ReportServer  
SET ALLOW_SNAPSHOT_ISOLATION OFF  
ALTER DATABASE ReportServerTempdb  
SET ALLOW_SNAPSHOT_ISOLATION OFF  
ALTER DATABASE ReportServer  
SET READ_COMMITTED_SNAPSHOT OFF  
ALTER DATABASE ReportServerTempDb  
SET READ_COMMITTED_SNAPSHOT OFF  
```  
  
## <a name="about-database-versions"></a>关于数据库版本  
 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]中，未提供有关数据库版本的显式信息。 但是，由于数据库版本始终与产品版本同步，因此可以使用产品版本信息来了解数据库版本的更改时间。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的产品版本信息是通过出现在日志文件中以及所有 SOAP 调用的标头中的文件版本信息指示的；连接到报表服务器 URL（例如，打开浏览器浏览 `https://localhost/reportserver`）时也会指示这些产品版本信息。  
  
## <a name="see-also"></a>另请参阅  

 [创建本机模式报表服务器数据库（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md)   
 [配置报表服务器服务帐户（报表服务器配置管理器）](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [配置报表服务器数据库连接（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
 [创建报表服务器数据库（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)   
 [报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Reporting Services 的备份和还原操作](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md)   
 [报表服务器数据库（SSRS 本机模式）](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
 [Reporting Services 报表服务器（本机模式）](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [存储加密的报表服务器数据（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md)   
 [配置和管理加密密钥（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  
  
  
