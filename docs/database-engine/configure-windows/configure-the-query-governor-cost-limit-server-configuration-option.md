---
title: 配置“查询调控器开销限制”服务器配置选项 | Microsoft Docs
description: 了解“查询调控器开销限制”选项。 了解如何使用它来限制查询的执行。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- queries [SQL Server], time to execute
- query governor cost limit option [SQL Server]
- time [SQL Server], query run time
ms.assetid: e7b8f084-1052-4133-959b-cebf4add790f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7004a0a7b7322fc939b5df21d40c389dc4a3dc6a
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783635"
---
# <a name="configure-the-query-governor-cost-limit-server-configuration-option"></a>配置查询调控器开销限制服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题说明了如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] “查询调控器开销限制” [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 “开销限制”选项指定运行给定查询所允许的估计开销的上限。 查询开销是一个抽象数字，由查询优化器根据估计的执行要求（如 CPU 时间、内存和磁盘 IO）确定。 它是指在特定硬件配置中完成查询所需的估计占用时间（秒）。 这个抽象数字不等于在正在运行的实例上完成查询所需的时间。 应将其视为一个相对度量值。 此选项的默认值为 0，将设置为关闭查询调控器。 将值设置为 0 将允许所有查询在没有任何时间限制下运行。 如果为该选项指定一个非零、非负的数值，则查询调控器将不允许执行估计开销超过该值的查询。   
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要设置查询调控器开销限制选项，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置查询调控器开销限制选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。  
  
-   若要在每个连接基础上更改查询调控器开销限制值，请使用 [SET QUERY_GOVERNOR_COST_LIMIT](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md) 语句。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-query-governor-cost-limit-option"></a>配置查询调控器开销限制选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“连接”** 页面。  
  
3.  选中或清除“使用查询调控器防止查询长时间运行”复选框。  
  
     如果选中此复选框，请在下面的框中输入一个正值，查询调控器将禁止执行估计开销超过该值的所有查询。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-query-governor-cost-limit-option"></a>配置查询调控器开销限制选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `query governor cost limit` 选项的值设置为估计查询开销的 `120` 分钟上限。
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'query governor cost limit', 120 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-query-governor-cost-limit-option"></a><a name="FollowUp"></a> 跟进：在配置查询调控器开销限制选项之后  
 该设置将立即生效，无需重新启动服务器。  
  
## <a name="see-also"></a>另请参阅  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
