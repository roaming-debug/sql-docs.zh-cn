---
description: 设置用于全文筛选器后台程序启动器的服务帐户
title: 设置用于全文筛选器后台程序启动器的服务帐户
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], FDHOST Launcher (MSSQLFDLauncher) service account
- FDHOST Launcher (MSSQLFDLauncher) [SQL Server]
ms.assetid: 3ab1d101-7ae0-488f-9b57-468e2517b737
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 4904a5e907a026f0cd8a6b5a0936b9f9395c2453
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468618"
---
# <a name="set-the-service-account-for-the-full-text-filter-daemon-launcher"></a>设置用于全文筛选器后台程序启动器的服务帐户
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
 本主题介绍如何使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器为 SQL 全文筛选器后台程序启动器服务 (MSSQLFDLauncher) 设置或更改服务帐户。 SQL Server 安装程序使用的默认服务帐户是 `NT Service\MSSQLFDLauncher`。
  
  
## <a name="about-the-sql-full-text-filter-daemon-launcher-service"></a>关于 SQL 全文筛选器后台程序启动器服务。
SQL Server 全文搜索将使用 SQL 全文筛选器后台程序启动器服务来启动筛选器后台程序主机进程，此进程用于处理全文搜索筛选和断字。 必须运行启动器服务才能使用全文搜索。  
  
SQL 全文筛选器后台程序启动器服务是可识别实例的服务，它与特定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例相关联。 SQL 全文筛选器后台程序启动器服务将服务帐户信息传播到所启动的每个筛选器后台程序主机进程。  

##  <a name="set-the-service-account"></a><a name="setting"></a> 设置服务帐户  
  
1.  在“开始”菜单上，指向“所有程序”、展开 [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]，再单击“SQL Server 2016 配置管理器”。  
  
2.  在“SQL Server 配置管理器”中，单击“SQL Server 服务”，右键单击“SQL 全文筛选器后台程序启动器” **（** _实例名称_ **）** ，然后单击“属性”。  
  
3.  单击此对话框的“登录”选项卡，选择或输入一个帐户，该帐户用于运行由 SQL 全文筛选器后台程序启动器服务启动的每个进程。  
  
4.  关闭此对话框之后，单击“重新启动”以重新启动 SQL 全文筛选器后台程序启动器服务。  
  
![SQL 全文筛选器后台程序启动器进程属性](../../relational-databases/search/media/sql-full-text-filter-daemon-launch-process-properties.png)
  
##  <a name="troubleshoot-the-sql-full-text-filter-daemon-launcher-service-if-it-doesnt-start"></a><a name="error"></a> 排查 SQL 全文筛选器后台程序启动器服务未启动的问题  
 如果 SQL 全文筛选器后台程序启动器服务未启动，请查看以下可能原因：  
  
### <a name="permissions-issues"></a>权限问题
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务组没有启动 SQL 全文筛选器后台程序启动器服务的权限。  

     请确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务组拥有对 SQL 全文筛选器后台程序启动器服务帐户的权限。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的安装过程中，向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务组授予了管理、查询和启动 SQL 全文筛选器后台程序启动器服务的默认权限。 如果在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后删除了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务组对 SQL 全文筛选器后台程序启动器服务帐户的权限，则 SQL 全文筛选器后台程序启动器服务将无法启动，并且全文搜索也将被禁用。     

-   用来登录到该服务的帐户没有相应的特权。  
  
     你使用的帐户可能对安装该服务器实例的计算机不具有登录特权。 请验证您用于登录的帐户是否对相应本地计算机具有用户权限。  

### <a name="service-account-and-password-issues"></a>服务帐户和密码问题
-   此服务帐户的用户帐户或密码不正确。  
  
     在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 配置管理器中，确保该服务使用正确的服务帐户和密码。  
  
-   与 SQL 全文筛选器后台程序启动器服务帐户关联的密码已过期。  
  
     如果对 SQL 全文筛选器后台程序启动器服务使用本地用户帐户并且你的密码已过期，则需执行以下操作：  
  
    1.  为此帐户设置一个新的 Windows 密码。  
  
    2.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 配置管理器中，更新 SQL 全文筛选器后台程序启动器服务以便使用新密码。  
  
### <a name="named-pipes-configuration-issues"></a>命名管道配置问题
-   未正确地配置 SQL 全文筛选器后台程序启动器服务。  
  
     如果本地计算机已禁用命名管道功能，或者已将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为使用非默认命名管道，则 SQL 全文筛选器后台程序启动器服务可能无法启动。  
  
-   相同命名管道的另一个实例已在运行。  
  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务充当 SQL 全文筛选器后台程序启动器服务客户端的命名管道服务器。 如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动之前另一进程便已创建该命名管道，则将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和 Windows 事件日志中记录一条错误，并且将无法使用全文搜索。  确定哪个进程或应用程序在试图使用该命名管道并停止相应的应用程序。  
  
## <a name="see-also"></a>另请参阅  
 [管理服务操作指南主题（SQL Server 配置管理器）](../../database-engine/configure-windows/scm-services-connect-to-another-computer.md)   
 [升级全文搜索](../../relational-databases/search/upgrade-full-text-search.md)  
  
