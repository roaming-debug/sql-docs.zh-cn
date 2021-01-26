---
title: 配置 remote access 服务器配置选项 | Microsoft Docs
description: 了解已弃用的远程访问选项的替代方法。 查看用于排查 SQL Server 连接问题的其他源。
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote servers [SQL Server], stored procedure execution
ms.assetid: f5de748d-1c55-4714-9661-38fe62e5095f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: afe6721bef097dfa941b5578b6d4287fc370c3f2
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783120"
---
# <a name="configure-the-remote-access-server-configuration-option"></a>配置远程访问服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题提供有关“远程访问”功能的信息。 此配置选项是一个模糊不清的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的通信功能，该功能已弃用，而且你可能不应当使用它。 如果你由于无法连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]而访问此页，请参阅以下主题之一：  
  
-   [教程：数据库引擎入门](../../relational-databases/tutorial-getting-started-with-the-database-engine.md)  
  
-   [登录到 SQL Server](../../database-engine/configure-windows/logging-in-to-sql-server.md)  
  
-   [在系统管理员被锁定时连接到 SQL Server](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)  
  
-   [连接到已注册的服务器 (SQL Server Management Studio)](../../ssms/register-servers/connect-to-a-registered-server-sql-server-management-studio.md)  
  
-   [从 SQL Server Management Studio 连接到任何 SQL Server 组件](../../ssms/f1-help/connect-to-any-sql-server-component-from-sql-server-management-studio.md)  
  
-   [使用 sqlcmd 连接到数据库引擎](../../ssms/scripting/sqlcmd-connect-to-the-database-engine.md)  
  
-   [如何排除连接到 SQL Server 数据库引擎时的故障](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)  
  
 程序员可能对以下主题感兴趣：  
  
-   [如何：在 ASP.NET 2.0 中使用 SQL 身份验证连接到 SQL Server](/previous-versions/msp-n-p/ff648340(v=pandp.10))  
  
-   [连接到 SQL Server 实例](../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md)  
  
-   [如何：创建到 SQL Server 数据库的连接](/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))  
  
 **本主题的正文从此处开始。**  
  
 本主题说明了如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] “远程访问” [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 **“远程访问”** 选项从运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的本地或远程服务器上控制存储过程的执行。 该选项的默认值为 1。 这将授权允许从远程服务器执行本地存储过程或从本地服务器执行远程存储过程。 若要阻止本地存储过程在远程服务器上执行或远程存储过程在本地服务器上执行，请将此选项设置为 0。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] 改用 [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)。
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要配置远程访问选项，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置远程访问选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   “远程访问”选项仅适用于使用 [sp_addserver](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md) 添加的服务器，包括此选项是为了向后兼容。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-access-option"></a>配置远程访问选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“连接”** 节点。  
  
3.  在 **“远程服务器连接”** 下，选中或清除 **“允许远程连接到此服务器”** 复选框。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-remote-access-option"></a>配置远程访问选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `remote access` 选项的值设置为 `0`。  
  
```sql  
EXEC sp_configure 'remote access', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-remote-access-option"></a><a name="FollowUp"></a> 跟进：在配置远程访问选项之后  
 此设置将在重启 SQL Server 之后生效。  
  
## <a name="see-also"></a>另请参阅  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
