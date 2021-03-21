---
description: EXECUTE AS (Transact-SQL)
title: EXECUTE AS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXECUTE AS
- EXECUTE_AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- WITH NO REVERT clause
- sessions [SQL Server], execution context
- EXECUTE AS
- execution context [SQL Server]
- switching execution context
ms.assetid: 613b8271-7f7d-4378-b7a2-5a7698551dbd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: eaeafb5eb8e64686429cb0c7b8ccac9c1b7785c2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740157"
---
# <a name="execute-as-transact-sql"></a>EXECUTE AS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  设置会话的执行上下文。  
  
 默认情况下，会话在用户登录时开始，在用户注销时结束。 会话过程中的所有操作都受限于对该用户进行的权限检查。 当运行 EXECUTE AS 语句时，会话的执行上下文将切换到指定的登录名或用户名。 上下文切换后，将根据登录名和用户安全令牌检查该帐户（而非调用 EXECUTE AS 语句的用户）的权限。 实际上，在会话或模块的执行期间模拟了用户或登录帐户，或显式恢复了上下文切换。  
  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
{ EXEC | EXECUTE } AS <context_specification>  
[;]  
  
<context_specification>::=  
{ LOGIN | USER } = 'name'  
    [ WITH { NO REVERT | COOKIE INTO @varbinary_variable } ]   
| CALLER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 LOGIN  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定要模拟的执行上下文是一个登录名。 模拟范围处于服务器级别。  
  
> [!NOTE]  
>  此选项在包含的数据库、SQL 数据库或 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中不可用。  
  
 USER  
 指定要模拟的上下文是当前数据库中的用户。 模拟范围只限于当前数据库。 对数据库用户的上下文切换不会继承该用户的服务器级别权限。  
  
> [!IMPORTANT]  
>  当到数据库用户的上下文切换处于活动状态时，任何对数据库以外资源的访问尝试都将导致语句失败。 这包括 USE database 语句、分布式查询以及引用其他数据库（使用由三或四部分构成的标识符）的查询。  
  
 'name' 是有效的用户或登录名。 name 必须是 sysadmin 固定服务器角色的成员，或者分别作为 [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) 或 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 中的主体而存在。  
  
 可以将 name 指定为局部变量。  
  
 name 必须是单一实例帐户，而不能是组、角色、证书、密钥或内置帐户，如 NT AUTHORITY\LocalService、NT AUTHORITY\NetworkService 或 NT AUTHORITY\LocalSystem。  
  
 有关详细信息，请参阅本主题后面的[指定用户名或登录名](#_user)。  
  
 NO REVERT  
 指定上下文切换不能恢复到以前的上下文。 NO REVERT 选项只能在临时级别使用。  
  
 有关恢复到以前上下文的详细信息，请参阅 [REVERT (Transact-SQL)](../../t-sql/statements/revert-transact-sql.md)。  
  
 COOKIE INTO @varbinary_variable  
 指定仅当调用 REVERT WITH COOKIE 语句包含正确的 @varbinary_variable 值时，执行上下文才能恢复回以前的上下文。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将 cookie 传递到 @varbinary_variable。 COOKIE INTO 选项只能在临时级别使用。  
  
 @varbinary_variable 是 varbinary(8000)。  
  
> [!NOTE]  
>  cookie OUTPUT 参数现记载为 varbinary(8000)，这是正确的最大长度 。 但是，当前执行返回 varbinary(100)。 应用程序应保留 varbinary(8000)，以便当 cookie 在将来的版本中返回大小增量时，应用程序可继续正确运行。  
  
 CALLER  
 当在模块内部使用时，指定模块内部的语句在模块调用方的上下文中执行。
当在模块外部使用时，语句没有任何操作。
 > [!NOTE]  
>  此选项在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]中不可用。  
  
## <a name="remarks"></a>备注  
 执行上下文中的更改在下列操作之一发生之前一直有效：  
  
-   运行另一个 EXECUTE AS 语句。  
  
-   运行 REVERT 语句。  
  
-   删除会话。  
  
-   命令已执行退出的存储过程或触发器。  
  
可以通过对多个主体多次调用 EXECUTE AS 语句来创建执行上下文堆栈。 在调用时，REVERT 语句将把上下文切换为上下文堆栈中上一级的登录帐户或用户。 有关此行为的示例，请参阅[示例 A](#_exampleA)。  
  
##  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a>指定用户名或登录名  
 EXECUTE AS \<context_specification> 中指定的用户或登录名必须分别作为 sys.database_principals 或 sys.server_principals 中的主体存在，否则 EXECUTE AS 语句将失败 。 此外，还必须为该主体授予 IMPERSONATE 权限。 除非调用方是数据库所有者或 sysadmin 固定服务器角色的成员，否则即使用户通过 Windows 组成员身份访问数据库或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，主体也必须存在。 例如，假设条件如下： 
  
-   CompanyDomain\SQLUsers 组拥有 Sales 数据库的访问权限 。  
  
-   CompanyDomain\SqlUser1 是 SQLUsers 的成员，因此具有对 Sales 数据库的隐式访问权限  。  
  
 虽然 CompanyDomain\SqlUser1 可通过 SQLUsers 组的成员身份访问数据库，但语句 `EXECUTE AS USER = 'CompanyDomain\SqlUser1'` 失败，因为 `CompanyDomain\SqlUser1` 未在数据库中作为主体存在 。  
  
如果用户处于孤立状态（关联登录名不再存在），并且该用户不是使用 WITHOUT LOGIN 创建的，EXECUTE AS 将针对此用户失败 。  
  
## <a name="best-practice"></a>最佳做法  
 指定具有执行会话中操作所需的最低特权的登录名或用户。 例如，如果只需要数据库级别的权限，则不要指定具有服务器级别权限的登录名；或者除非需要这些权限，否则不要指定数据库所有者帐户。  
  
> [!CAUTION]  
>  只要[!INCLUDE[ssDE](../../includes/ssde-md.md)]可以解析该名称，就可以成功执行 EXECUTE AS 语句。 如果域用户存在，Windows 可能为[!INCLUDE[ssDE](../../includes/ssde-md.md)]解析用户，即使该 Windows 用户无权访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 这可能导致这样的情况：无权访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的登录名似乎登录了，尽管模拟的登录名仅具有给 public 或 guest 授予的权限。  
  
## <a name="using-with-no-revert"></a>使用 WITH NO REVERT  
 当 EXECUTE AS 语句包含可选的 WITH NO REVERT 子句时，不能通过 REVERT 语句或执行另一个 EXECUTE AS 语句来重置会话的执行上下文。 由该语句设置的上下文在删除会话之前一直保持有效。  
  
 当指定 WITH NO REVERT COOKIE = @varbinary_variable 子句时，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将 cookie 值传递给 @varbinary_variable 。 只有执行调用的 REVERT WITH COOKIE = @varbinary_variable 语句包含相同的 \@varbinary_variable 值，该语句设置的执行上下文才能恢复为以前的上下文 。  
  
 该选项在使用连接池的环境中非常有用。 连接池是指维护一组数据库连接，以使应用程序服务器上的应用程序能够重用它们。 由于传递给 \@varbinary_variable 的值仅对于 EXECUTE AS 语句的调用方是已知的，因此该调用方可以保证它们建立的执行上下文不能被任何其他用户更改。  
  
## <a name="determining-the-original-login"></a>确定原始登录  
 使用 [ORIGINAL_LOGIN](../../t-sql/functions/original-login-transact-sql.md) 函数可以返回连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的登录名。 您可以在具有众多显式或隐式上下文切换的会话中使用该函数返回原始登录的标识。  
  
## <a name="permissions"></a>权限  
 若要对某登录名指定 EXECUTE AS，调用方必须具有对所指定登录名的 IMPERSONATE 权限，并且不得被拒绝 IMPERSONATE ANY LOGIN 权限  。 若要对某数据库用户指定 EXECUTE AS，调用方必须具有对所指定用户名的 IMPERSONATE 权限 。 指定 EXECUTE AS CALLER 时，不需要 IMPERSONATE 权限 。  
  
## <a name="examples"></a>示例  
  
###  <a name="a-using-execute-as-and-revert-to-switch-context"></a><a name="_exampleA"></a> A. 使用 EXECUTE AS 和 REVERT 切换上下文  
 以下示例使用多个主体创建上下文执行堆栈。 然后使用 `REVERT` 语句将执行上下文重置为上一个调用方。 将多次执行 `REVERT` 语句以向上移动堆栈，直到将执行上下文设置为原始调用方为止。  
  

```sql
USE AdventureWorks2012;  
GO  
--Create two temporary principals  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
--Give IMPERSONATE permissions on user2 to user1  
--so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
--Verify the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
--Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1 and login2.  
--The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
--Remove temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>B. 使用 WITH COOKIE 子句  
 下面的示例将会话的执行上下文设置为指定的用户，并指定 WITH COOKIE INTO @varbinary_variable 子句。 `REVERT` 语句必须指定传递给 `@cookie` 语句中的 `EXECUTE AS` 变量的值，否则，无法成功将上下文恢复为该调用方。 若要执行此示例，必须存在示例 A 中所创建的 `login1` 登录名和 `user1` 用户。  
  

```sql 
DECLARE @cookie VARBINARY(8000);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie in a safe location in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(8000);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [REVERT (Transact-SQL)](../../t-sql/statements/revert-transact-sql.md)   
 [EXECUTE AS 子句 (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)  
  
  

