---
title: 配置“两位数年份截止”服务器配置选项 | Microsoft Docs
description: 熟悉“两位数年份截止”选项。 了解它如何确定 SQL Server 将两位数年份转换为四位数年份的方式。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- two digit year cutoff option
- four-digit years [SQL Server]
ms.assetid: d94e81b6-f2e6-47ef-b497-ec3d827a1646
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fcc9082b2700b4818b8742c59fe12cc38879f0f1
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783096"
---
# <a name="configure-the-two-digit-year-cutoff-server-configuration-option"></a>配置两位数年份截止服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明了如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] “两位数年份截止” [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 **“两位数年份截止”** 选项从 1753 到 9999 之间指定一个整数来表示缩略形式的年份，以将两位数的年份解释为四位数的年份。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 默认的时间范围是 1950-2049，表示截止年份为 2049。 这说明 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将两位数年份 49 解释为 2049 年，将两位数年份 50 解释为 1950 年，而将两位数年份 99 解释为 1999 年。 若要维护向后兼容性，请将设置保持为默认值。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要配置两位数年份截止选项，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置“两位数年份截止”选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。  
  
-   OLE 自动化对象使用 2030 作为两位数年份截止。 可以使用 **“两位数年份截止”** 选项使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和客户端应用程序之间的日期值保持一致。 

-   为避免日期含糊歧义，请在数据中使用 4 位数字的年份。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-two-digit-year-cutoff-option"></a>配置两位数年份截止选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“杂项服务器设置”** 节点。  
  
3.  在 **“两位数年份支持”** 下的 **“在输入两位数的年份时**， **将其解释为介于下面范围内的年份”** 框中，键入或选择作为时间范围的结束年份的值。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-two-digit-year-cutoff-option"></a>配置两位数年份截止选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `two digit year cutoff` 选项的值设置为 `2030`。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'two digit year cutoff', 2030 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-two-digit-year-cutoff-option"></a><a name="FollowUp"></a> 跟进：在配置“两位数年份截止”选项之后  
 该设置将立即生效，无需重新启动服务器。  
  
## <a name="see-also"></a>另请参阅  
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)  
  
  
