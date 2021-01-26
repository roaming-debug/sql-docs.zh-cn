---
title: 配置 scan for startup procs 服务器配置选项 | Microsoft Docs
description: 了解“启动时扫描存储过程”选项。 了解它如何指定 SQL Server 是否在启动时扫描并运行所有自动运行的存储过程。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- scan for startup procs option
ms.assetid: 6bf9d252-e766-458d-9dcd-23d895f032a2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 00426f84d2f8b3c0ca4626ffd8bddc234f27cf4d
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783159"
---
# <a name="configure-the-scan-for-startup-procs-server-configuration-option"></a>配置 scan for startup procs 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] scan for startup procs [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 使用 **scan for startup procs** 选项扫描在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动时自动执行的存储过程。 如果将此选项设置为 1，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将扫描服务器上定义的所有自动运行的存储过程，并运行这些过程。 **scan for startup procs** 的默认值为 0（不扫描）。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要配置 scan for startup procs 选项，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置 scan for startup procs 选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。  
  
-   此选项的值可以使用 **sp_configure** 进行设置；但是，如果使用 **sp_procoption**（用于标记或取消标记自动执行的存储过程），则会自动进行设置。 使用 **sp_procoption** 将第一个存储过程标记为自动执行过程后，此选项的值自动设置为 1。 使用 **sp_procoption** 将最后一个存储过程标记为自动执行过程后，此选项的值自动设置为 0。 如果使用 **sp_procoption** 标记或取消标记自动执行过程，并且始终在删除自动执行过程之前进行取消标记，则无需手动设置此选项。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-scan-for-startup-procs-option"></a>配置 scan for startup procs 选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“高级”** 节点。  
  
3.  在“杂项”下，通过从下拉列表框中选择所需值将“启动时扫描存储过程”选项更改为 True 或 False。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-scan-for-startup-procs-option"></a>配置 scan for startup procs 选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `scan for startup procs` 选项的值设置为 `1`。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'scan for startup procs', 1 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
##  <a name="follow-up-after-you-configure-the-scan-for-startup-procs-option"></a><a name="FollowUp"></a> 跟进：在配置 scan for startup procs 选项之后  
 必须重新启动服务器，设置才会生效。  
  
## <a name="see-also"></a>另请参阅  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [sp_procoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md)  
  
  
