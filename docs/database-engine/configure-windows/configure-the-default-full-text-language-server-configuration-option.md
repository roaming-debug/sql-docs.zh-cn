---
title: 配置“默认全文语言”服务器配置选项 | Microsoft Docs
description: 了解默认全文语言选项。 了解如何配置它来指定 SQL Server 用于全文检索的默认语言。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- default full-text language option
ms.assetid: 0fa8785b-0830-4a52-aff5-fcf8268b72fc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45e05505444d00607cae875b8dfd8bd07bf9190e
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783234"
---
# <a name="configure-the-default-full-text-language-server-configuration-option"></a>配置 default full-text language 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置“默认全文语言”服务器配置选项。 **default full-text language** 选项指定全文索引的默认语言值。 语言分析将对全文索引的所有数据执行，并且取决于数据的语言。 该选项的默认值为服务器的语言。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的本地化版本， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将把 **default full-text language** 选项设置为服务器的语言（如果存在合适的匹配项）。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的非本地化版本， **或** 选项为“英语”。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **配置 default full-text language 选项，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置默认全文语言选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   当没有通过 CREATE FULLTEXT INDEX 或 ALTER FULLTEXT INDEX 语句中的 **language_term** 选项为列指定任何语言时，则在全文索引中使用 **default full-text language** 选项的值。 如果不支持默认全文语言，或者语言分析包不可用，则 CREATE 或 ALTER 操作将失败，并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回说明指定语言无效的错误消息。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   此选项是一个高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改。  
  
-   “默认全文语言”选项需要 LCID 值。 有关支持的 LCID 及其相关语言的列表，请参阅 [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)。 例如，独立的软件供应商还可提供其他语言。 如果找不到特定区域语言，则全文引擎将自动切换到主要语言。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>配置 default full-text language 选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“高级”** 节点。  
  
3.  在“杂项”下，使用 **“默认全文语言”** 指定全文索引列的默认语言值。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>配置 default full-text language 选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 将 `default full-text` 选项的值设置为荷兰语 (`1043`)。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'default full-text language', 1043 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
 有关详细信息，请参阅 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)版本的组合自动配置的最大工作线程数。  
  
##  <a name="follow-up-after-you-configure-the-default-full-text-language-option"></a><a name="FollowUp"></a> 跟进：在配置默认全文语言选项之后  
 该设置将立即生效，无需重新启动服务器。  
  
## <a name="see-also"></a>另请参阅  
 [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
