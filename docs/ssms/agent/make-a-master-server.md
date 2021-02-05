---
description: Make a Master Server
title: Make a Master Server
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.msxwiz.operator.f1
- sql13.ag.msxwiz.login.f1
- sql13.ag.msxwiz.target.f1
- sql13.ag.msxwiz.complete.f1
- sql13.ag.msxwiz.cover.f1
helpviewer_keywords:
- master servers [SQL Server], creating
- wizards [SQL Server Management Studio], Master Server Wizard
- SQL Server Agent jobs, master servers
- Master Server Wizard
ms.assetid: 05739a73-1fdf-4d9d-92a6-70f328380322
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4d1adb0e2c86030fe05b1bba4088f71228a654c1
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251402"
---
# <a name="make-a-master-server"></a>设置主服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题描述如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 设置主服务器 [!INCLUDE[tsql](../../includes/tsql-md.md)]。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
如果分布式作业的步骤与某个代理相关联，则该作业将在目标服务器上该代理帐户的上下文下运行。 请确保满足以下条件，否则与代理关联的作业步骤将不会从主服务器下载到目标服务器上：  
  
-   主服务器注册表子项 **\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<&#42;instance_name&#42;>\SQL Server Agent\AllowDownloadedJobsToMatchProxyName** (REG_DWORD) 设置为 1 (true)。 默认情况下，此子项设置为 0 (False)。  
  
-   目标服务器上已存在与运行作业步骤的主服务器代理帐户同名的代理帐户。  
  
从主服务器将使用代理帐户的作业步骤下载到目标服务器时，如果作业步骤失败，可以检查 **msdb** 数据库中 **sysdownloadlist** 表的 **error_message** 列是否存在以下错误消息：  
  
-   “该作业步骤需要代理帐户，但是目标服务器上禁用了代理匹配功能。”  
  
    若要解决此错误，请将 **AllowDownloadedJobsToMatchProxyName** 注册表子项设置为 1。  
  
-   “找不到代理。”  
  
    若要解决此错误，请确保目标服务器上已存在与运行作业步骤的主服务器代理帐户同名的代理帐户。  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
默认情况下授予 **sysadmin** 固定服务器角色的成员执行此过程的权限。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-make-a-master-server"></a>设置主服务器  
  
1.  在“对象资源管理器”中，连接到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)] 的实例，然后展开该实例。  
  
2.  右键单击“SQL Server 代理”，指向“多服务器管理”，再单击“将其设置为主服务器”。 **主服务器向导** 将引导您完成设置主服务器和添加目标服务器的过程。  
  
3.  从“主服务器操作员”中，配置主服务器的操作员。若要通过电子邮件或寻呼程序向操作员发送通知，必须配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理以发送电子邮件。 若要使用 **net send** 向操作员发送通知，必须在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理所在的服务器上运行 Messenger 服务。  
  
    **电子邮件地址**  
    设置操作员的电子邮件地址。  
  
    **寻呼地址**  
    设置操作员的寻呼电子邮件地址。  
  
    **Net send 地址**  
    设置操作员的 **net send** 地址。  
  
4.  从 **“目标服务器”** 页中，为主服务器选择目标服务器。  
  
    **已注册的服务器**  
    列出已在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中注册但尚未成为目标服务器的服务器。  
  
    **目标服务器**  
    列出已经为目标服务器的服务器。  
  
    **>**  
    将所选服务器移动到目标服务器列表中。  
  
    **>>**  
    将所有服务器移动到目标服务器列表中。  
  
    **<**  
    从目标服务器列表中删除所选服务器。  
  
    **<<**  
    从目标服务器列表中删除所有服务器。  
  
    **添加连接**  
    向目标服务器列表中添加服务器，但不注册该服务器。  
  
    **Connection**  
    更改所选服务器的连接属性。  
  
5.  从 **“主服务器登录凭据”** 页中，指定是否要在必要时为目标服务器创建新的登录名，并为其分配针对主服务器的权限。  
  
    **在必要时创建新登录名，并为其分配针对 MSX 的权限**  
    如果指定的登录名不存在，则在目标服务器上创建新登录名。  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-make-a-master-server"></a>设置主服务器  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 本示例将当前服务器登记到 AdventureWorks1 主服务器中。 当前服务器的位置是 Building 21、Room 309、Rack 5。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_msx_enlist N'AdventureWorks1',   
    N'Building 21, Room 309, Rack 5' ;   
GO;  
```  
  
有关详细信息，请参阅 [sp_msx_enlist (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
[创建多服务器环境](../../ssms/agent/create-a-multiserver-environment.md)  
[企业范围的自动化管理](../../ssms/agent/automated-administration-across-an-enterprise.md)  
