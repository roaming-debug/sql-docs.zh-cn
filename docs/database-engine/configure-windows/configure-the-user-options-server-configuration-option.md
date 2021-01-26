---
title: 配置 user options 服务器配置选项 | Microsoft Docs
description: 了解“用户选项”选项。 了解它如何更改 SQL Server 为用户工作会话建立的查询处理选项的默认值。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- global default for all users [SQL Server]
- users [SQL Server], global defaults
- user options option [SQL Server]
ms.assetid: cfed8f86-6bcf-4b90-88eb-9656e22d5dc5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 71dcf7e52b1c58c7df868f502bc8de5da9892c6b
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783424"
---
# <a name="configure-the-user-options-server-configuration-option"></a>配置 user options 服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 **或** 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中配置 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] user options [!INCLUDE[tsql](../../includes/tsql-md.md)]服务器配置选项。 **user options** 选项指定了适用于所有用户的全局默认值。 将建立一个用户工作会话期间使用的默认查询处理选项的列表。 **user options** 选项允许您更改 SET 选项的默认值（如果服务器的默认设置不合适）。  
  
 用户可以使用 SET 语句覆盖这些默认值。 可以为新登录名动态配置 **user options** 。 更改 **user options** 的设置后，新的登录名会话将使用新的设置，当前登录名会话受不影响。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要配置 user options 配置选项，可使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [在配置用户选项配置选项之后](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   下表列出并说明了 **user options** 的配置值。 并非所有配置值都是相互兼容的。 例如，不能同时设置 ANSI_NULL_DFLT_ON 和 ANSI_NULL_DFLT_OFF。  
  
    |值|配置|说明|  
    |-----------|-------------------|-----------------|  
    |1|DISABLE_DEF_CNST_CHK|控制执行期间或延迟的约束检查。|  
    |2|IMPLICIT_TRANSACTIONS|对于 DBLIB 网络库连接，控制执行语句时是否隐式启动事务。 IMPLICIT_TRANSACTIONS 设置对 ODBC 或 OLEDB 连接没有影响。|  
    |4|CURSOR_CLOSE_ON_COMMIT|控制执行提交操作后游标的行为。|  
    |8|ANSI_WARNINGS|控制聚合警告中的截断和 NULL。|  
    |16|ANSI_PADDING|控制固定长度变量的填充。|  
    |32|ANSI_NULLS|使用相等运算符时控制 NULL 处理。|  
    |64|ARITHABORT|在查询执行过程中发生溢出或被零除错误时终止查询。|  
    |128|ARITHIGNORE|在查询过程中出现溢出或被零除错误时返回 NULL。|  
    |256|QUOTED_IDENTIFIER|对表达式进行求值时区别单引号和双引号。|  
    |512|NOCOUNT|关闭执行每个语句后返回的报告受影响的行数的消息。|  
    |1024|ANSI_NULL_DFLT_ON|将会话的行为更改为使用 ANSI 兼容的空性。 未显式定义为空性的新列允许使用空值。|  
    |2048|ANSI_NULL_DFLT_OFF|将会话的行为更改为不使用 ANSI 兼容的空性。 未显式定义为空性的新列不允许使用空值。|  
    |4096|CONCAT_NULL_YIELDS_NULL|将 NULL 值与字符串串联时返回 NULL。|  
    |8192|NUMERIC_ROUNDABORT|表达式中出现精度降低时生成错误。|  
    |16384|XACT_ABORT|如果 Transact-SQL 语句引发运行时错误，则回滚事务。|  
  
-   **user options** 中位的位置与 @@OPTIONS 中位的位置相同。 每个连接都有自己的 @@OPTIONS 函数，该函数表示配置环境。 登录到 \ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，用户会收到将当前 **user options** 值指定为 @@OPTIONS 的默认环境。 对 **user options** 执行 SET 语句会影响会话的 @@OPTIONS 函数的相应值。 在此设置更改后创建的所有连接都将收到新值。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 默认情况下，所有用户都具备不带参数或仅带第一个参数的 **sp_configure** 的执行权限。 若要执行带两个参数的 **sp_configure** 以更改配置选项或运行 RECONFIGURE 语句，则用户必须具备 ALTER SETTINGS 服务器级别的权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-the-user-options-configuration-option"></a>配置 user options 配置选项  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“连接”** 节点。  
  
3.  在 **“默认连接选项”** 框中，选择一个或多个属性以便为所有已连接的用户配置默认查询处理选项。  
  
     默认情况下，不配置用户选项。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-the-user-options-configuration-option"></a>配置 user options 配置选项  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例说明如何使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 配置 `user options` 以更改 ANSI_WARNINGS 服务器选项的设置。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'user options', 8 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
##  <a name="follow-up-after-you-configure-the-user-options-configuration-option"></a><a name="FollowUp"></a> 跟进：在配置用户选项配置选项之后  
 该设置将立即生效，无需重新启动服务器。  
  
## <a name="see-also"></a>另请参阅  
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
