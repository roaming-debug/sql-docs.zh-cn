---
description: CREATE LOGIN (Transact-SQL)
title: CREATE LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_LOGIN_TSQL
- CREATE LOGIN
- LOGIN_TSQL
- LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- passwords [SQL Server], logins
- mapping logins [SQL Server]
- logins [SQL Server], creating
- CREATE LOGIN statement
- permissions [SQL Server], logins
- Windows domain accounts [SQL Server]
- re-hashing passwords
- certificates [SQL Server], logins
ms.assetid: eb737149-7c92-4552-946b-91085d8b1b01
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a6a695e5bc2fe0c02a097f66317ea2f3f1161cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192637"
---
# <a name="create-login-transact-sql"></a>CREATE LOGIN (Transact-SQL)

为 SQL Server、SQL 数据库、Azure Synapse Analytics 或 Analytics Platform System 数据库创建登录名。 单击以下选项卡之一，了解特定版本的语法、参数、注解、权限和示例。

CREATE LOGIN 参与事务。 如果在事务内执行 CREATE LOGIN 并且该事务回滚，则登录名创建也会回滚。 如果在事务内执行，则在事务提交之前无法使用创建的登录名。

有关语法约定的详细信息，请参阅 [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure SQL 数据库](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL<br />托管实例](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for SQL Server
CREATE LOGIN login_name { WITH <option_list1> | FROM <sources> }

<option_list1> ::=
    PASSWORD = { 'password' | hashed_password HASHED } [ MUST_CHANGE ]
    [ , <option_list2> [ ,... ] ]

<option_list2> ::=
    SID = sid
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | CHECK_EXPIRATION = { ON | OFF}
    | CHECK_POLICY = { ON | OFF}
    | CREDENTIAL = credential_name

<sources> ::=
    WINDOWS [ WITH <windows_options>[ ,... ] ]
    | CERTIFICATE certname
    | ASYMMETRIC KEY asym_key_name
  
<windows_options> ::=
    DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>参数

login_name 指定创建的登录名。 有四种类型的登录名：SQL Server 登录名、Windows 登录名、证书映射登录名以及非对称密钥映射登录名。 在创建从 Windows 域帐户映射的登录名时，必须以 [\<domainName>\\<login_name>] 格式使用 Windows 2000 之前的用户登录名。 不能使用 login_name@DomainName 格式的 UPN。 有关示例，请参阅本文后面的示例 D。 身份验证登录的类型为 sysname，它必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则，且不能包含“\\” 。 Windows 登录名可以包含“\\”。 基于 Active Directory 用户的登录名的名称限制为少于 21 个字符。

PASSWORD =“password”仅适用于 SQL Server 登录名。 指定正在创建的登录名的密码。 请使用强密码。 有关详细信息，请参阅[强密码](../../relational-databases/security/strong-passwords.md)和[密码策略](../../relational-databases/security/password-policy.md)。 从 SQL Server 2012 (11.x) 开始，存储的密码信息使用 SHA-512 加盐密码进行计算。

密码是区分大小写的。 密码应始终至少包含八个字符，并且不能超过 128 个字符。 密码可以包含 a-z、A-Z、0-9 和大多数非字母数字字符。 密码不能包含单引号或 login_name。

PASSWORD **=** *hashed\_password* 仅适用于 HASHED 关键字。 指定要创建的登录名的密码的哈希值。

HASHED 仅适用于 SQL Server 登录。 指定在 PASSWORD 参数后输入的密码已经过哈希运算。 如果未选择此选项，则在将作为密码输入的字符串存储到数据库中之前，对其进行哈希运算。 此选项应仅用于在服务器之间迁移数据库。 切勿使用 HASHED 选项创建新的登录名。 HASHED 选项不能用于 SQL 7 或更早版本创建的哈希。

MUST_CHANGE 仅适用于 SQL Server 登录。 如果包括此选项，则 SQL Server 将在首次使用新登录时提示用户输入新密码。

CREDENTIAL =credential\_name 将映射到新 SQL Server 登录名的凭据名称。 该凭据必须已存在于服务器中。 当前此选项只将凭据链接到登录名。 凭据不能映射到系统管理员 (sa) 登录名。

SID = sid 用于重新创建登录名。 仅适用于 SQL Server 身份验证登录，不适用于 Windows 身份验证登录。 指定新 SQL Server 身份验证登录的 SID。 如果未使用此选项，SQL Server 将自动分配 SID。 SID 结构取决于 SQL Server 版本。 SQL Server 登录 SID：基于 GUID 的 16 字节 (binary(16)) 文本值。 例如，`SID = 0x14585E90117152449347750164BA00A7`。

DEFAULT_DATABASE =database 指定将指派给登录名的默认数据库。 如果未包括此选项，则默认数据库将设置为 master。

DEFAULT_LANGUAGE =language 指定将指派给登录名的默认语言。 如果未包括此选项，则默认语言将设置为服务器的当前默认语言。 即使将来服务器的默认语言发生更改，登录名的默认语言也仍保持不变。

CHECK_EXPIRATION = { ON | OFF } 仅适用于 SQL Server 登录名。 指定是否应对此登录帐户强制实施密码过期策略。 默认值为 OFF。

CHECK_POLICY = { ON | OFF } 仅适用于 SQL Server 登录名。 指定应对此登录强制实施运行 SQL Server 的计算机的 Windows 密码策略。 默认值为 ON。

如果 Windows 策略要求强密码，密码必须至少包含以下四个特点中的三个：

- 大写字符 (A-Z)。
- 小写字符 (a-z)。
- 数字 (0-9)。
- 一个非字母数字字符，如空格、_、@、*、^、%、!、$、# 或 &。

WINDOWS 指定将登录名映射到 Windows 登录名。

CERTIFICATE certname 指定将与此登录名关联的证书名称。 此证书必须已存在于 master 数据库中。

ASYMMETRIC KEY asym_key_name 指定将与此登录名关联的非对称密钥的名称。 此密钥必须已存在于 master 数据库中。

## <a name="remarks"></a>备注

- 密码是区分大小写的。
- 只有创建 SQL Server 登录时，才支持对密码预先进行哈希运算。
- 如果指定 MUST_CHANGE，则 CHECK_EXPIRATION 和 CHECK_POLICY 必须设置为 ON。 否则，该语句将失败。
- 不支持 CHECK_POLICY = OFF 和 CHECK_EXPIRATION = ON 的组合。
- 如果 CHECK_POLICY 设置为 OFF，将对 lockout_time 进行重置，并将 CHECK_EXPIRATION 设置为 OFF。

> [!IMPORTANT]
> 只有在 Windows Server 2003 及更高版本上才会强制执行 CHECK_EXPIRATION 和 CHECK_POLICY。 有关详细信息，请参阅 [Password Policy](../../relational-databases/security/password-policy.md)。

- 从证书或非对称密钥创建的登录名仅用于代码签名。 不能用于连接到 SQL Server。 仅当 master 中已存在证书或非对称密钥时，才能从证书或非对称密钥创建登录名。
- 有关用于传输登录名的脚本，请参阅[如何在 SQL Server 2005 和 SQL Server 2008 的实例之间传输登录名和密码](https://support.microsoft.com/kb/918992)。
- 自动创建登录名将启用新的登录名，并授予它服务器级 CONNECT SQL 权限。
- 服务器的[身份验证模式](../../relational-databases/security/choose-an-authentication-mode.md)必须匹配登录名类型才能允许访问。
- 有关设计权限系统的信息，请参阅 [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。

## <a name="permissions"></a>权限

- 只有具有针对服务器的 ALTER ANY LOGIN 权限或 securityadmin 固定服务器角色的成员身份的用户才可创建登录 。 有关详细信息，请参阅[服务器级别角色](/azure/sql-database/sql-database-manage-logins#groups-and-roles)和 [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md)。
- 如果使用 CREDENTIAL 选项，则还需要对此服务器的 ALTER ANY CREDENTIAL 权限 。

## <a name="after-creating-a-login"></a>创建登录后

创建登录后，该登录可以连接到 SQL Server，但是只具有授予 public 角色的权限。 考虑执行以下部分活动。

- 要连接到数据库，请创建登录名对应的数据库用户。 有关详细信息，请参阅 [CREATE USER](../../t-sql/statements/create-user-transact-sql.md)。
- 使用 [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md) 创建用户定义的服务器角色。 使用 ALTER SERVER ROLE ...ADD MEMBER 将新登录名添加到用户定义的服务器角色。 有关详细信息，请参阅 [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md) 和 [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md)。
- 使用 sp_addsrvrolemember 将登录名添加到固定服务器角色。 有关详细信息，请参阅[服务器级别角色](../../relational-databases/security/authentication-access/server-level-roles.md)和 [sp_addsrvrolemember](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)。
- 使用 GRANT 语句将服务器级别权限授予新的登录名或包含该登录名的角色。 有关详细信息，请参阅 [GRANT](../../t-sql/statements/grant-transact-sql.md)。

## <a name="examples"></a>示例

### <a name="a-creating-a-login-with-a-password"></a>A. 创建带密码的登录名

以下示例为特定用户创建登录名并分配密码。

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-with-a-password-that-must-be-changed"></a>B. 创建带必须更改的密码的登录名

以下示例为特定用户创建登录名并分配密码。 `MUST_CHANGE` 选项要求用户在首次连接服务器时更改此密码。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'
    MUST_CHANGE, CHECK_EXPIRATION = ON;
GO
```

> [!NOTE]
> 当 CHECK_EXPIRATION 设为 OFF (关)时，不能使用 MUST_CHANGE 选项。

### <a name="c-creating-a-login-mapped-to-a-credential"></a>C. 创建映射到凭据的登录名

以下示例使用该用户为特定用户创建登录名。 此登录名映射到凭据。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>',
    CREDENTIAL = <credentialName>;
GO
```

### <a name="d-creating-a-login-from-a-certificate"></a>D. 从证书创建登录名

下例用 master 中的证书为特定用户创建登录名。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
USE MASTER;
CREATE CERTIFICATE <certificateName>
    WITH SUBJECT = '<login_name> certificate in master database',
    EXPIRY_DATE = '12/05/2025';
GO
CREATE LOGIN <login_name> FROM CERTIFICATE <certificateName>;
GO
```

### <a name="e-creating-a-login-from-a-windows-domain-account"></a>E. 从 Windows 域帐户创建登录名

以下示例使用 Windows 域帐户创建一个登录名。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
CREATE LOGIN [<domainName>\<login_name>] FROM WINDOWS;
GO
```

### <a name="f-creating-a-login-from-a-sid"></a>F. 从 SID 创建登录名

以下示例首先创建 SQL Server 身份验证登录，并确定该登录的 SID。

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';
SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

我的查询返回 0x241C11948AEEB749B0D22646DB1A19F2 作为 SID。 你的查询将返回不同的值。 以下语句将删除登录名，然后重新创建登录名。 使用前面的查询中的 SID。

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

### <a name="g-creating-a-login-with-multiple-arguments"></a>G. 创建具有多个参数的登录名

下面的示例演示如何在每个参数之间使用逗号将多个参数串在一起。

```sql
CREATE LOGIN [MyUser]
WITH PASSWORD = 'MyPassword',
DEFAULT_DATABASE = MyDatabase,
CHECK_POLICY = OFF,
CHECK_EXPIRATION = OFF ;
```

## <a name="see-also"></a>另请参阅

- [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [主体](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [密码策略](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [创建登录名](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        \* Azure SQL 数据库 \*
    :::column-end:::
    :::column:::
        [Azure SQL<br />托管实例](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL 数据库

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Azure SQL Database
CREATE LOGIN login_name
 { WITH <option_list> }

<option_list> ::=
    PASSWORD = { 'password' }
    [ , SID = sid ]
```

## <a name="arguments"></a>参数

login_name 指定创建的登录名。 Azure SQL 数据库中的单一数据库和共用数据库以及 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的数据库仅支持 SQL 登录名。 若要为 Azure Active Directory 用户创建帐户或创建未与登录关联的用户帐户，请使用 [CREATE USER](create-user-transact-sql.md) 语句。 有关详细信息，请参阅[在 Azure SQL 数据库中管理登录名](/azure/sql-database/sql-database-manage-logins)。

PASSWORD ='password*' 指定正在创建的 SQL 登录名的密码。 请使用强密码。 有关详细信息，请参阅[强密码](../../relational-databases/security/strong-passwords.md)和[密码策略](../../relational-databases/security/password-policy.md)。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，存储的密码信息使用 SHA-512 加盐密码进行计算。

密码是区分大小写的。 密码应始终至少包含八个字符，并且不能超过 128 个字符。 密码可以包含 a-z、A-Z、0-9 和大多数非字母数字字符。 密码不能包含单引号或 login_name。

SID = sid 用于重新创建登录名。 仅适用于 SQL Server 身份验证登录，不适用于 Windows 身份验证登录。 指定新 SQL Server 身份验证登录的 SID。 如果未使用此选项，SQL Server 将自动分配 SID。 SID 结构取决于 SQL Server 版本。 对于 SQL 数据库，这是包含 `0x01060000000000640000000000000000` 的 32 字节 (binary(32)) 文本以及表示 GUID 的 16 个字节。 例如，`SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`。

## <a name="remarks"></a>备注

- 密码是区分大小写的。
- 自动创建登录名将启用新的登录名，并授予它服务器级 CONNECT SQL 权限。

> [!IMPORTANT]
> 有关在 Azure SQL 数据库中使用登录名和用户的信息，请参阅[在 Azure SQL 数据库中管理登录名](/azure/sql-database/sql-database-manage-logins)。

## <a name="login"></a>登录

### <a name="sql-database-logins"></a>SQL 数据库登录名

CREATE LOGIN 语句必须是批中的唯一语句。

在连接到 SQL 数据库的一些方法（如 sqlcmd）中，必须使用 \<login>@\<server>> 符号将 SQL 数据库服务器名称追加到连接字符串中的登录名之后 。 例如，如果登录为 `login1`，SQL 数据库服务器的完全限定名称是 `servername.database.windows.net`，则连接字符串的 username 参数应是 `login1@servername`。 由于 username 参数的总长度为 128 个字符，因此，login_name 被限定为 127 个字符减去服务器名称的长度 。 在示例中，`login_name` 只能包含 117 个字符，因为 `servername` 包含 10 个字符。

在 SQL 数据库中，必须使用适当的权限连接到 master 数据库才能创建登录。 有关详细信息，请参阅[创建其他登录名和具有管理权限的用户](/azure/sql-database/sql-database-manage-logins#create-additional-logins-and-users-having-administrative-permissions)。

SQL Server 规则允许你创建 \<loginname>@\<servername> 格式的 SQL Server 身份验证登录。 如果你的 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]服务器是 myazureserver 并且登录名是 myemail@live.com，则必须提供 myemail@live.com@myazureserver 格式的登录名  。

在 SQL 数据库中，对连接和服务器级别的防火墙规则进行身份验证时所需的登录数据会暂时缓存在每个数据库中。 此缓存定期刷新。 若要强制刷新身份验证缓存并确保数据库具有最新版本的登录表，请执行 [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)。

## <a name="permissions"></a>权限

只有服务器级别主体登录（由预配过程创建）或 master 数据库中的 `loginmanager` 数据库角色成员可以创建新的登录。 有关详细信息，请参阅[创建其他登录名和具有管理权限的用户](/azure/sql-database/sql-database-manage-logins#create-additional-logins-and-users-having-administrative-permissions)。

## <a name="examples"></a>示例

### <a name="a-creating-a-login-with-a-password"></a>A. 创建带密码的登录名

以下示例为特定用户创建登录名并分配密码。

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-from-a-sid"></a>B. 从 SID 创建登录名

以下示例首先创建 SQL Server 身份验证登录，并确定该登录的 SID。

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

我的查询返回 0x241C11948AEEB749B0D22646DB1A19F2 作为 SID。 你的查询将返回不同的值。 以下语句将删除登录名，然后重新创建登录名。 使用前面的查询中的 SID。

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

## <a name="see-also"></a>另请参阅

- [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [主体](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [密码策略](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [创建登录名](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL 数据库](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        \* Azure SQL<br />托管实例 \*
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL 托管实例

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Azure SQL Managed Instance
CREATE LOGIN login_name [FROM EXTERNAL PROVIDER] { WITH <option_list> [,..]}

<option_list> ::=
    PASSWORD = {'password'}
    | SID = sid
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>参数

login_name 与 FROM EXTERNAL PROVIDER 子句一起使用时，登录名指定 Azure Active Directory (AD) 主体，可以是 Azure AD 用户、组或应用程序。 否则，登录名表示所创建 SQL 登录名的名称。

FROM EXTERNAL PROVIDER </br>
指定登录名用于 Azure AD 身份验证。

PASSWORD = 'password' 指定正在创建的 SQL 登录名的密码。 请使用强密码。 有关详细信息，请参阅[强密码](../../relational-databases/security/strong-passwords.md)和[密码策略](../../relational-databases/security/password-policy.md)。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，存储的密码信息使用 SHA-512 加盐密码进行计算。

密码是区分大小写的。 密码应始终至少包含十个字符，并且不能超过 128 个字符。 密码可以包含 a-z、A-Z、0-9 和大多数非字母数字字符。 密码不能包含单引号或 login_name。

SID **=** *sid* 用于重新创建登录名。 仅适用于 SQL Server 身份验证登录名。 指定新 SQL Server 身份验证登录的 SID。 如果未使用此选项，SQL Server 将自动分配 SID。 SID 结构取决于 SQL Server 版本。 对于 SQL 数据库，这是包含 `0x01060000000000640000000000000000` 的 32 字节 (binary(32)) 文本以及表示 GUID 的 16 个字节。 例如，`SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`。

## <a name="remarks"></a>备注

- 密码是区分大小写的。
- 创建映射到 Azure AD 帐户的服务器级主体 (FROM EXTERNAL PROVIDER) 时引入了新语法
- 指定 FROM EXTERNAL PROVIDER 时：

  - login_name 必须表示现有 Azure AD 帐户（用户、组或应用程序），当前 Azure SQL 托管实例可在 Azure AD 中访问该帐户。 对于 Azure AD 主体，CREATE LOGIN 语法需要：
    - Azure AD 用户的 Azure AD 对象的 UserPrincipalName。
    - Azure AD 组和 Azure AD 应用程序的 Azure AD 对象的 DisplayName。
  - 不能使用 PASSWORD 选项。
- 默认情况下，忽略 FROM EXTERNAL PROVIDER 子句时，会创建一个常规 SQL 登录名。
- Azure AD 登录名会在 sys.server_principals 中显示，同时，对于映射到 Azure AD 用户的登录名，类型列值设置为“E”，type_desc 设置为“EXTERNAL_LOGIN”；对于映射到 Azure AD 组的登录名，类型列值设置为“X”，type_desc 设置为“EXTERNAL_GROUP”   。
- 有关用于传输登录名的脚本，请参阅[如何在 SQL Server 2005 和 SQL Server 2008 的实例之间传输登录名和密码](https://support.microsoft.com/kb/918992)。
- 自动创建登录名将启用新的登录名，并授予它服务器级 CONNECT SQL 权限。

> [!IMPORTANT]
> 有关在 Azure SQL 数据库中使用登录名和用户的信息，请参阅[在 Azure SQL 数据库中管理登录名](/azure/sql-database/sql-database-manage-logins)。

## <a name="logins-and-permissions"></a>登录名和权限

只有服务器级别主体登录（由预配过程创建）或 master 数据库中的 `securityadmin` 或 `sysadmin` 数据库角色成员可以创建新的登录名。 有关详细信息，请参阅[服务器级别角色](/azure/sql-database/sql-database-manage-logins#groups-and-roles)和 [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md)。

默认情况下，在 master 中授予新建 Azure AD 登录名的标准权限为：**CONNECT SQL** 和 **VIEW ANY DATABASE**。

### <a name="sql-managed-instance-logins"></a>SQL 托管实例登录名

- 必须具有对服务器的 ALTER ANY LOGIN 权限或固定服务器角色（`securityadmin` 和 `sysadmin` 中的一个）的成员身份。 只有具有针对服务器的 ALTER ANY LOGIN 权限或其中一个角色的成员身份的 Azure Active Directory (Azure AD) 帐户可以执行创建命令。
- 如果登录名是 SQL 主体，则只有属于 `sysadmin` 角色的登录名才能使用 create 命令来为 Azure AD 帐户创建登录名。
- 必须是用于 Azure SQL 托管实例的同一目录中的 Azure AD 成员。

## <a name="after-creating-a-login"></a>创建登录后

> [!NOTE]
> 在创建后，Azure SQL 托管实例功能的 Azure AD 管理员已更改。 有关详细信息，请参阅[适用于 MI 的新 Azure AD 管理员功能](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi)。

创建登录名后，该登录名可以连接到托管实例，但只具有授予 public 角色的权限。 考虑执行以下部分活动。

- 若要通过 Azure AD 登录名创建 Azure AD 用户，请参阅 [CREATE USER](../../t-sql/statements/create-user-transact-sql.md)。
- 若要向数据库中的用户授予权限，请使用 ALTER SERVER ROLE ...ADD MEMBER 语句将用户添加到其中一个内置数据库角色或自定义角色中，或者使用 [GRANT](../../t-sql/statements/grant-transact-sql.md) 语句直接向用户授予权限。 有关详细信息，请参阅[非管理员角色](/azure/sql-database/sql-database-manage-logins#non-administrator-users)、[其他服务器级管理角色](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles)、[ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) 和 [GRANT](grant-transact-sql.md) 语句。
- 若要授予服务器范围内的权限，请在 master 数据库中创建数据库用户，并使用 ALTER SERVER ROLE ...ADD MEMBER 语句将用户添加到其中一个管理服务器角色。 有关详细信息，请参阅[服务器级别角色](/azure/sql-database/sql-database-manage-logins#groups-and-roles)、[ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) 和[服务器角色](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles)。
  - 使用以下命令将 `sysadmin` 角色添加到 Azure AD 登录名：`ALTER SERVER ROLE sysadmin ADD MEMBER [AzureAD_Login_name]`
- 使用 GRANT 语句将服务器级别权限授予新的登录名或包含该登录名的角色。 有关详细信息，请参阅 [GRANT](../../t-sql/statements/grant-transact-sql.md)。

## <a name="limitations"></a>限制

- 不支持将映射到 Azure AD 组的 Azure AD 登录名设置为数据库所有者。
- 支持使用其他 Azure AD 主体模拟 Azure AD 服务器级主体，例如 [EXECUTE AS](execute-as-transact-sql.md) 子句。
- 只有属于 `sysadmin` 角色的 SQL 服务器级主体（登录名）可以针对 Azure AD 主体执行以下操作：
  - EXECUTE AS USER
  - EXECUTE AS LOGIN
- 无法通过 Azure 门户将从另一个 Azure AD 目录导入的外部（来宾）用户直接配置为 SQL 托管实例的 Azure AD 管理员。 而是需将外部用户加入 Azure AD 启用安全机制的组，并将该组配置为实例管理员。 可使用 PowerShell 或 Azure CLI 将各来宾用户设置为实例管理员。
- 登录名不会复制到故障转移组中的辅助实例。 登录名保存在 master 数据库中，它是系统数据库，因而不进行异地复制。 若要解决此问题，用户必须在辅助实例上创建具有相同 SID 的登录名。

```SQL
-- Code to create login on the secondary instance
CREATE LOGIN foo WITH PASSWORD = '<enterStrongPasswordHere>', SID = <login_sid>;
```

## <a name="examples"></a>示例

### <a name="a-creating-a-login-with-a-password"></a>A. 创建带密码的登录名

以下示例为特定用户创建登录名并分配密码。

 ```sql
 CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
 GO
 ```

### <a name="b-creating-a-login-from-a-sid"></a>B. 从 SID 创建登录名

 以下示例首先创建 SQL Server 身份验证登录，并确定该登录的 SID。

 ```sql
 CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

 SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
 GO
 ```

我的查询返回 0x241C11948AEEB749B0D22646DB1A19F2 作为 SID。 你的查询将返回不同的值。 以下语句将删除登录名，然后重新创建登录名。 使用前面的查询中的 SID。

 ```sql
 DROP LOGIN TestLogin;
 GO

 CREATE LOGIN TestLogin
 WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

 SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
 GO
 ```

### <a name="c-creating-a-login-for-a-local-azure-ad-account"></a>C. 为本地 Azure AD 帐户创建登录名

 以下示例为 Azure AD 帐户 joe@myaad.onmicrosoft.com 创建一个登录名，该帐户位于 myaad 的 Azure AD 中。

```sql
CREATE LOGIN [joe@myaad.onmicrosoft.com] FROM EXTERNAL PROVIDER
GO
```

### <a name="d-creating-a-login-for-a-federated-azure-ad-account"></a>D. 为联合 Azure AD 帐户创建登录

 以下示例为联合 Azure AD 帐户 bob@contoso.com 创建一个登录名，该帐户位于名为 contoso 的 Azure AD 中。 用户 bob 也可以是来宾用户。

```sql
CREATE LOGIN [bob@contoso.com] FROM EXTERNAL PROVIDER
GO
```

### <a name="e-creating-a-login-for-an-azure-ad-group"></a>E. 为 Azure AD 组创建登录

 以下示例为 Azure AD 组 mygroup 创建一个登录名，该组位于 myaad 的 Azure AD 中

```sql
CREATE LOGIN [mygroup] FROM EXTERNAL PROVIDER
GO
```

### <a name="f-creating-a-login-for-an-azure-ad-application"></a>F. 为 Azure AD 应用程序创建登录名

以下示例为 Azure AD 应用程序 myapp 创建一个登录名，该应用程序位于 myaad 的 Azure AD 中

```sql
CREATE LOGIN [myapp] FROM EXTERNAL PROVIDER
```

### <a name="g-check-newly-added-logins"></a>G. 检查新添加的登录名

要检查新添加的登录名，请执行以下 T-SQL 命令：

```sql
SELECT *
FROM sys.server_principals;
GO
```

## <a name="see-also"></a>另请参阅

- [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [主体](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [密码策略](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [创建登录名](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL 数据库](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL<br />托管实例](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Azure Synapse Analytics
CREATE LOGIN login_name
 { WITH <option_list> }

<option_list> ::=
    PASSWORD = { 'password' }
    [ , SID = sid ]
```

## <a name="arguments"></a>参数

login_name 指定创建的登录名。 Azure Synapse 中的 SQL Analytics 仅支持 SQL 登录名。 若要为 Azure Active Directory 用户创建帐户，请使用 [CREATE USER](create-user-transact-sql.md) 语句。

PASSWORD ='password*' 指定正在创建的 SQL 登录名的密码。 请使用强密码。 有关详细信息，请参阅[强密码](../../relational-databases/security/strong-passwords.md)和[密码策略](../../relational-databases/security/password-policy.md)。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，存储的密码信息使用 SHA-512 加盐密码进行计算。

密码是区分大小写的。 密码应始终至少包含八个字符，并且不能超过 128 个字符。 密码可以包含 a-z、A-Z、0-9 和大多数非字母数字字符。 密码不能包含单引号或 login_name。

 SID = sid 用于重新创建登录名。 仅适用于 SQL Server 身份验证登录，不适用于 Windows 身份验证登录。 指定新 SQL Server 身份验证登录的 SID。 如果未使用此选项，SQL Server 将自动分配 SID。 SID 结构取决于 SQL Server 版本。 对于 SQL Analytics，这是包含 `0x01060000000000640000000000000000` 的 32 字节 (binary(32)) 文本以及表示 GUID 的 16 个字节。 例如，`SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`。

## <a name="remarks"></a>备注

- 密码是区分大小写的。
- 有关用于传输登录名的脚本，请参阅[如何在 SQL Server 2005 和 SQL Server 2008 的实例之间传输登录名和密码](https://support.microsoft.com/kb/918992)。
- 自动创建登录名将启用新的登录名，并授予它服务器级 CONNECT SQL 权限。
- 服务器的[身份验证模式](../../relational-databases/security/choose-an-authentication-mode.md)必须匹配登录名类型才能允许访问。
- 有关设计权限系统的信息，请参阅 [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。

## <a name="logins"></a>登录名

CREATE LOGIN 语句必须是批中的唯一语句。

在使用工具（如 sqlcmd）连接到 Azure Synapse 中时，必须使用 \<login>@\<server> 符号将 SQL Analytics 服务器名称追加到连接字符串中的登录名之后。 例如，如果登录为 `login1`，SQL Analytics 服务器的完全限定名称是 `servername.database.windows.net`，则连接字符串的 username 参数应是 `login1@servername`。 由于 username 参数的总长度为 128 个字符，因此，login_name 被限定为 127 个字符减去服务器名称的长度 。 在示例中，`login_name` 只能包含 117 个字符，因为 `servername` 包含 10 个字符。

若要创建登录名，必须连接到 master 数据库。

SQL Server 规则允许你创建 \<loginname>@\<servername> 格式的 SQL Server 身份验证登录。 如果你的 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]服务器是 myazureserver 并且登录名是 myemail@live.com，则必须提供 myemail@live.com@myazureserver 格式的登录名  。

对连接和服务器级别的防火墙规则进行身份验证时所需的登录数据会暂时缓存在每个数据库中。 此缓存定期刷新。 若要强制刷新身份验证缓存并确保数据库具有最新版本的登录表，请执行 [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)。

有关登录名的详细信息，请参阅[管理数据库和登录名](/azure/sql-database/sql-database-manage-logins)。

## <a name="permissions"></a>权限

只有服务器级别主体登录（由预配过程创建）或 master 数据库中的 `loginmanager` 数据库角色成员可以创建新的登录。 有关详细信息，请参阅[服务器级别角色](/azure/sql-database/sql-database-manage-logins#groups-and-roles)和 [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md)。

## <a name="after-creating-a-login"></a>创建登录后

创建登录后，该登录可以连接到 Azure Synapse，但是只具有授予 public 角色的权限。 考虑执行以下部分活动。

- 要连接到数据库，请创建登录名对应的数据库用户。 有关详细信息，请参阅 [CREATE USER](../../t-sql/statements/create-user-transact-sql.md)。
- 若要向数据库中的用户授予权限，请使用 ALTER SERVER ROLE ...ADD MEMBER 语句将用户添加到其中一个内置数据库角色或自定义角色中，或者使用 [GRANT](grant-transact-sql.md) 语句直接向用户授予权限。 有关详细信息，请参阅[非管理员角色](/azure/sql-database/sql-database-manage-logins#non-administrator-users)、[其他服务器级管理角色](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles)、[ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) 和 [GRANT](grant-transact-sql.md) 语句。
- 若要授予服务器范围内的权限，请在 master 数据库中创建数据库用户，并使用 ALTER SERVER ROLE ...ADD MEMBER 语句将用户添加到其中一个管理服务器角色。 有关详细信息，请参阅[服务器级别角色](/azure/sql-database/sql-database-manage-logins#groups-and-roles)、[ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) 和[服务器角色](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles)。

- 使用 GRANT 语句将服务器级别权限授予新的登录名或包含该登录名的角色。 有关详细信息，请参阅 [GRANT](../../t-sql/statements/grant-transact-sql.md)。

## <a name="examples"></a>示例

### <a name="a-creating-a-login-with-a-password"></a>A. 创建带密码的登录名

以下示例为特定用户创建登录名并分配密码。

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-from-a-sid"></a>B. 从 SID 创建登录名

 以下示例首先创建 SQL Server 身份验证登录，并确定该登录的 SID。

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

我的查询返回 0x241C11948AEEB749B0D22646DB1A19F2 作为 SID。 你的查询将返回不同的值。 以下语句将删除登录名，然后重新创建登录名。 使用前面的查询中的 SID。

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

## <a name="see-also"></a>另请参阅

- [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [主体](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [密码策略](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [创建登录名](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL 数据库](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL<br />托管实例](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>分析平台系统

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Analytics Platform System
CREATE LOGIN loginName { WITH <option_list1> | FROM WINDOWS }

<option_list1> ::=
    PASSWORD = { 'password' } [ MUST_CHANGE ]
    [ , <option_list> [ ,... ] ]
  
<option_list> ::=
      CHECK_EXPIRATION = { ON | OFF}
    | CHECK_POLICY = { ON | OFF}
```

## <a name="arguments"></a>参数

login_name 指定创建的登录名。 有四种类型的登录名：SQL Server 登录名、Windows 登录名、证书映射登录名以及非对称密钥映射登录名。 在创建从 Windows 域帐户映射的登录名时，必须以 [\<domainName>\\<login_name>] 格式使用 Windows 2000 之前的用户登录名。 不能使用 login_name@DomainName 格式的 UPN。 有关示例，请参阅本文后面的示例 D。 身份验证登录的类型为 sysname，它必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则，且不能包含“\\” 。 Windows 登录名可以包含“\\”。 基于 Active Directory 用户的登录名的名称限制为少于 21 个字符。

PASSWORD ='password' 仅适用于 SQL Server 登录名。 指定正在创建的登录名的密码。 请使用强密码。 有关详细信息，请参阅[强密码](../../relational-databases/security/strong-passwords.md)和[密码策略](../../relational-databases/security/password-policy.md)。 从 SQL Server 2012 (11.x) 开始，存储的密码信息使用 SHA-512 加盐密码进行计算。

密码是区分大小写的。 密码应始终至少包含八个字符，并且不能超过 128 个字符。 密码可以包含 a-z、A-Z、0-9 和大多数非字母数字字符。 密码不能包含单引号或 login_name。

MUST_CHANGE 仅适用于 SQL Server 登录。 如果包括此选项，则 SQL Server 将在首次使用新登录时提示用户输入新密码。

CHECK_EXPIRATION = { ON | OFF } 仅适用于 SQL Server 登录名。 指定是否应对此登录帐户强制实施密码过期策略。 默认值为 OFF。

CHECK_POLICY = { ON | OFF } 仅适用于 SQL Server 登录名。 指定应对此登录强制实施运行 SQL Server 的计算机的 Windows 密码策略。 默认值为 ON。

如果 Windows 策略要求强密码，密码必须至少包含以下四个特点中的三个：

- 大写字符 (A-Z)。
- 小写字符 (a-z)。
- 数字 (0-9)。
- 一个非字母数字字符，如空格、_、@、*、^、%、!、$、# 或 &。

WINDOWS 指定将登录名映射到 Windows 登录名。

## <a name="remarks"></a>备注

- 密码是区分大小写的。
- 如果指定 MUST_CHANGE，则 CHECK_EXPIRATION 和 CHECK_POLICY 必须设置为 ON。 否则，该语句将失败。
- 不支持 CHECK_POLICY = OFF 和 CHECK_EXPIRATION = ON 的组合。
- 如果 CHECK_POLICY 设置为 OFF，将对 lockout_time 进行重置，并将 CHECK_EXPIRATION 设置为 OFF。

> [!IMPORTANT]
> 只有在 Windows Server 2003 及更高版本上才会强制执行 CHECK_EXPIRATION 和 CHECK_POLICY。 有关详细信息，请参阅 [Password Policy](../../relational-databases/security/password-policy.md)。

- 有关用于传输登录名的脚本，请参阅[如何在 SQL Server 2005 和 SQL Server 2008 的实例之间传输登录名和密码](https://support.microsoft.com/kb/918992)。
- 自动创建登录名将启用新的登录名，并授予它服务器级 CONNECT SQL 权限。
- 有关设计权限系统的信息，请参阅 [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。

## <a name="permissions"></a>权限

只有具有针对服务器的 ALTER ANY LOGIN 权限或 securityadmin 固定服务器角色的成员身份的用户才可创建登录 。 有关详细信息，请参阅[服务器级别角色](/azure/sql-database/sql-database-manage-logins#groups-and-roles)和 [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md)。

## <a name="after-creating-a-login"></a>创建登录后

创建登录后，该登录可以连接到 Azure Synapse Analytics，但是只具有授予 public 角色的权限。 考虑执行以下部分活动。

- 要连接到数据库，请创建登录名对应的数据库用户。 有关详细信息，请参阅 [CREATE USER](../../t-sql/statements/create-user-transact-sql.md)。
- 使用 [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md) 创建用户定义的服务器角色。 使用 ALTER SERVER ROLE ...ADD MEMBER 将新登录名添加到用户定义的服务器角色。 有关详细信息，请参阅 [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md) 和 [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md)。
- 使用 sp_addsrvrolemember 将登录名添加到固定服务器角色。 有关详细信息，请参阅[服务器级别角色](../../relational-databases/security/authentication-access/server-level-roles.md)和 [sp_addsrvrolemember](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)。
- 使用 GRANT 语句将服务器级别权限授予新的登录名或包含该登录名的角色。 有关详细信息，请参阅 [GRANT](../../t-sql/statements/grant-transact-sql.md)。

## <a name="examples"></a>示例

### <a name="g-creating-a-sql-server-authentication-login-with-a-password"></a>G. 创建具有密码的 SQL Server 身份验证登录名

下面的示例创建密码为 `A2c3456` 的登录名 `Mary7`。

```sql
CREATE LOGIN Mary7 WITH PASSWORD = 'A2c3456$#' ;
```

### <a name="h-using-options"></a>H. 使用选项

下面的示例创建具有密码和一些可选参数的登录名 `Mary8`。

```sql
CREATE LOGIN Mary8 WITH PASSWORD = 'A2c3456$#' MUST_CHANGE,
CHECK_EXPIRATION = ON,
CHECK_POLICY = ON;
```

### <a name="i-creating-a-login-from-a-windows-domain-account"></a>I. 从 Windows 域帐户创建登录名

下面的示例在 `Contoso` 域中使用 Windows 域帐户创建名为 `Mary` 的登录名。

```sql
CREATE LOGIN [Contoso\Mary] FROM WINDOWS;
GO
```

## <a name="see-also"></a>另请参阅

- [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [主体](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [密码策略](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [创建登录名](../../relational-databases/security/authentication-access/create-a-login.md)

---

::: moniker-end
