---
title: 配置 remote query timeout 服务器配置选项 | Microsoft Docs
description: 了解“remote query timeout”选项。 了解它如何确定 SQL Server 超时之前远程操作花费的秒数。
ms.custom: ''
ms.date: 03/08/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- time limit for remote queries [SQL Server]
- remote query timeout option
ms.assetid: 888c8448-933b-41e3-8aa1-c206bc0cdb78
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ddf0184fd7a1bd95290fa2a041d2fede09f90bf
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783162"
---
# <a name="configure-the-remote-query-timeout-server-configuration-option"></a>配置 remote query timeout 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] remote query timeout [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 **remote query timeout** 选项指定在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 超时之前远程操作可以持续的时间（秒）。此选项的默认值是 600，即允许等待 10 分钟。 此值将应用到由作为远程查询的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 发起的发送连接。 此值不会对 [!INCLUDE[ssDE](../../includes/ssde-md.md)]接收的查询产生任何影响。 若要禁用该超时，请将此值设置为 0。 查询将一直等待，直到完成。  
  
 对于异类查询， **remote query timeout** 指定远程访问接口在查询超时前应等待结果集的秒数（由命令对象使用 DBPROP_COMMANDTIMEOUT 行集属性进行初始化）。如果远程提供程序支持该值，则该值还用于设置 DBPROP_GENERALTIMEOUT。 这将导致任何其他操作在指定的秒数后超时。  
  
 对于远程存储过程， **remote query timeout** 指定在发送一个远程 `EXEC` 语句之后，在远程存储过程超时前必须等待的秒数。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [先决条件](#Prerequisites)  
  
     [安全性](#Security)  
  
-   **配置 remote query timeout 选项，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置 remote query timeout 选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   在设定此值前，必须允许远程服务器连接。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-query-timeout-option"></a>配置 remote query timeout 选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“连接”** 节点。  
  
3.  在 **“远程服务器连接”** 下的 **“远程查询超时值”** 框中，键入或选择介于 0 到 2,147,483,647 之间的值以设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在超时之前等待的最多秒数。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-remote-query-timeout-option"></a>配置 remote query timeout 选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `remote query timeout` 选项的值设置为 `0` 以禁用超时。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'remote query timeout', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-remote-query-timeout-option"></a><a name="FollowUp"></a> 跟进：在配置 remote query timeout 选项之后  
 该设置将立即生效，无需重新启动服务器。  
  
## <a name="see-also"></a>另请参阅  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [行集属性和行为](../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
