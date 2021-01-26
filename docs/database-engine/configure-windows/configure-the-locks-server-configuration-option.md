---
title: 配置 locks 服务器配置选项 | Microsoft Docs
description: 了解 locks 选项。 了解如何使用它来限制 SQL Server 数据库引擎为锁分配的内存量。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- locks option [SQL Server]
ms.assetid: b0cf0f86-7652-4574-a9fb-908e10d03973
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a30c2cb2955ef82380bde00714098afec153ed95
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783680"
---
# <a name="configure-the-locks-server-configuration-option"></a>配置 locks 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] locks [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 **locks** 选项设置可用锁的最大数目，以限制 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 为锁分配的内存量。 默认设置为 0，即允许 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 根据不断变化的系统要求动态地分配和释放锁结构。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **配置 locks 选项，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置锁选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。  
  
-   如果服务器启动时 **locks** 设置为 0，锁管理器将从 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中获取足够的内存，用于包含 2,500 个锁结构的初始池。 当锁池用完时，将另外为该池获取内存。  
  
     通常情况下，如果锁池需要的内存比 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 内存池中可用的内存多，而具有更多可用的计算机内存（尚未达到 **max server memory** 的阈值），则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将动态分配内存以满足锁的请求。 但是，如果内存分配导致操作系统级的分页（例如，如果另一个应用程序与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例在同一台计算机上运行并使用该计算机的内存），则不会分配更多的锁空间。 动态锁池获取的内存不会超过分配给 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的内存的 60%。 如果锁池获取的内存达到了 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例所获取内存的 60%，或计算机上没有更多的可用内存，则再发出针对锁的请求将生成错误。  
  
     推荐的配置是允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 动态地使用锁。 但是，可以通过设置 **locks** 来替代 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 动态分配锁资源的能力。 将 **locks** 设置为 0 以外的值后， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 分配的锁的数量不能大于在 **locks** 中指定的值。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 显示消息说明超过了可用锁数，请增大此值。 由于每一个锁都需要消耗内存（每一个锁需 96 字节），增加此值将增加服务器对内存的需求。  
  
-   **locks** 选项也会影响何时进行锁升级。 如果 **locks** 设置为 0，则当前锁结构使用的内存达到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 内存池的 40% 时将进行锁升级。 如果 **locks** 未设置为 0，则当锁的数量达到 **locks** 的指定值的 40% 时将进行锁升级。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-locks-option"></a>配置 locks 选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“高级”** 节点。  
  
3.  在 **“并行”** 之下，键入想要的 **locks** 选项的值。  
  
     使用 **locks** 选项设置可用锁的最大数目，以限制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为锁分配的内存量。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-locks-option"></a>配置 locks 选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 设置 `locks` 选项的值，将所有用户可用的锁数设置为 `20000`。  
  
```sql  
Use AdventureWorks2012 ;  
GO  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'locks', 20000;  
GO  
RECONFIGURE;  
GO  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-locks-option"></a><a name="FollowUp"></a> 跟进：在配置锁选项之后  
 必须重新启动服务器，设置才会生效。  
  
## <a name="see-also"></a>另请参阅  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
