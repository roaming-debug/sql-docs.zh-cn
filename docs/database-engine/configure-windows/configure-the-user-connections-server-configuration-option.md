---
title: 配置 user connections 服务器配置选项 | Microsoft Docs
description: 了解“用户连接”选项。 了解它如何帮助避免由于过多并发连接而使 SQL Server 实例超载。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- simultaneous connections [SQL Server]
- user connections option [SQL Server]
- users [SQL Server], simultaneous connections
- maximum number of simultaneous user connections
- connections [SQL Server], simultaneous
ms.assetid: 53beee6e-59fe-4276-9abb-8f1cec2a3508
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5a6e6b2a47151a967980b6e75766bdec39e88ae
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673462"
---
# <a name="configure-the-user-connections-server-configuration-option"></a>配置 user connections 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中设置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] user connections [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 **user connections** 选项指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上允许同时建立的最大用户连接数。 实际允许的用户连接数还取决于正使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本以及应用程序和硬件的限制。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 允许的最大用户连接数为 32767。 由于 **user connections** 是动态（自动配置）选项， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将根据需要自动调整最大用户连接数，最大不超过允许的最大值。 例如，如果仅有 10 个用户登录，则要分配 10 个用户连接对象。 在大多数情况下，没有必要更改此选项的值。 默认值为 0，表示允许的最多用户连接数为 (32,767) 。  
  
 若要确定系统允许的最大用户连接数，可以执行 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 或查询 [sys.configuration](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) 目录视图。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **配置 user connections 选项，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置用户连接选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。  
  
-   使用 **user connections** 选项有助于避免由于过多并发连接而使服务器超载。 可以根据系统和用户要求估计连接数。 例如，在很多用户的系统上，每个用户通常不要求唯一的连接。 可以在用户间共享连接。 对于运行 OLE DB 应用程序的用户，每个打开的连接对象需要一个连接；对于运行开放式数据库连接 (ODBC) 应用程序的用户，每个活动连接句柄需要一个连接；对于运行 DB-Library 应用程序的用户，每个调用 DB-Library **dbopen** 函数启动的进程需要一个连接。  
  
    > [!IMPORTANT]  
    >  如果必须使用此选项，请不要将值设置得太高，这是因为不管是否使用连接，每个连接都会产生开销。 如果超过了用户连接的最大允许值，将收到一条错误消息，而且直到出现一个可用连接之后才能建立连接。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-user-connections-option"></a>配置 user connections 选项  
  
1.  在对象资源浏览器中，右键单击某个服务器，然后单击 **“属性”** 。  
  
2.  单击 **“连接”** 节点。  
  
3.  在 **“连接”** 下面的 **“最大并发连接数”** 框中，键入或选择一个介于 0 到 32767 之间的值，以设置允许与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例同时连接的最大用户数量。  
  
4.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-user-connections-option"></a>配置 user connections 选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例显示如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `user connections` 选项的值配置为 `325` 个用户。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'user connections', 325 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-user-connections-option"></a><a name="FollowUp"></a> 跟进：在配置用户连接选项之后  
 必须重启 SQL 实例，设置才会生效。  
  
## <a name="see-also"></a>另请参阅  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
