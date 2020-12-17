---
description: 为 SQL Server 代理服务设置 SQL Server 别名
title: 为 SQL Server 代理服务设置 SQL Server 别名
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- aliases [SQL Server], creating
- SQL Server Agent, aliases
ms.assetid: 02d6295d-ab52-44f0-8f1b-f3910a507d8f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4530aebde58448903015b8f948ad8f005d9134ee
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472238"
---
# <a name="set-a-sql-server-alias-for-the-sql-server-agent-service"></a>为 SQL Server 代理服务设置 SQL Server 别名

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明了如何通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 设置 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 别名以供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理用来连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务将通过命名管道，使用无需额外客户端配置的动态服务器名称连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。 如果当前使用的不是默认网络传输或连接的是侦听备用命名管道的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，则需要配置服务器连接别名。  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须选择引用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]本地实例的别名，代理才能正常工作。  
  
-   “对象资源管理器”仅在您拥有使用权限时才显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理节点。  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，必须将 **代理配置为使用** sysadmin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]固定服务器角色的成员帐户的凭据，才能执行其功能。 该帐户必须拥有以下 Windows 权限：  
  
-   以服务身份登录 (SeServiceLogonRight)  
  
-   替换进程级别标记 (SeAssignPrimaryTokenPrivilege)  
  
-   跳过遍历检查 (SeChangeNotifyPrivilege)  
  
-   调整进程的内存配额 (SeIncreaseQuotaPrivilege)  
  
有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户所需的 Windows 权限的详细信息，请参阅 [为 SQL Server 代理服务选择帐户](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) 和 [设置 Windows 服务帐户](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-set-a-sql-server-alias-for-the-sql-server-agent-service"></a>为 SQL Server 代理服务设置 SQL Server 别名  
  
1.  在 **对象资源管理器** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)] 实例，再展开该实例。  
  
2.  右键单击“SQL Server 代理”，然后单击“属性”。  
  
3.  在“SQL Server 代理属性 _server\_name_”对话框的“选择页”下，选择“连接”，然后  
  
4.  在 **“本地主机服务器别名”** 框中，键入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理应连接到的服务器别名的类型。  
  
5.  单击“确定”。  
