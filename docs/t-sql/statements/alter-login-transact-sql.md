---
description: ALTER LOGIN (Transact-SQL)
title: ALTER LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_LOGIN_TSQL
- ALTER LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER LOGIN statement
- change password
- mapping logins [SQL Server]
- logins [SQL Server], modifying
- passwords [SQL Server], modifying
- names [SQL Server], logins
- modifying login accounts
ms.assetid: e247b84e-c99e-4af8-8b50-57586e1cb1c5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cfe89693b365924faabe52254ed09fef075be831
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742707"
---
# <a name="alter-login-transact-sql"></a>ALTER LOGIN (Transact-SQL)

更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的属性。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for SQL Server

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    | <cryptographic_credential_option>
    }
[;]

<status_option> ::=
        ENABLE | DISABLE

<set_option> ::=
    PASSWORD = 'password' | hashed_password HASHED
    [
      OLD_PASSWORD = 'oldpassword'
      | <password_option> [<password_option> ]
    ]
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }
    | CREDENTIAL = credential_name
    | NO CREDENTIAL

<password_option> ::=
    MUST_CHANGE | UNLOCK
  
<cryptographic_credentials_option> ::=
    ADD CREDENTIAL credential_name
  | DROP CREDENTIAL credential_name
```

## <a name="arguments"></a>参数

login_name 指定正在更改的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的名称。 域登录名必须用方括号括起来，其格式为 [domain\user]。

ENABLE | DISABLE 启用或禁用此登录名。 禁用登录名不会影响已连接登录名的行为。 （使用 `KILL` 语句终止现有连接。）禁用的登录名将保留它们的权限，且仍然可以模拟。

PASSWORD ='password' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定正在更改的登录名的密码。 密码是区分大小写的。

PASSWORD = hashed\_password 仅适用于 HASHED 关键字。 指定要创建的登录名的密码的哈希值。

> [!IMPORTANT]
> 当一个登录名（或包含的数据库用户）进行连接并经过身份验证后，该连接缓存有关该登录名的标识信息。 对于 Windows 身份验证登录，此标识信息包含有关 Windows 组中的成员身份的信息。 只要保持连接，该登录名的标识就保持已经过身份验证的状态。 若要在标识中强制进行更改（例如，重置密码或更改 Windows 组成员身份），则必须从身份验证机构（Windows 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）注销，然后再次登录。 **sysadmin** 固定服务器角色的成员或任何使用 **ALTER ANY CONNECTION** 权限的登录名都可以使用 **KILL** 命令结束连接，并强制登录名重新连接。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 可以在打开与对象资源管理器和查询编辑器窗口之间的多个连接时重用连接信息。 关闭所有连接可强制重新连接。

HASHED 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定在 PASSWORD 参数后输入的密码已经过哈希运算。 如果未选择此选项，则在将密码存储到数据库之前，对其进行哈希运算。 此选项只能用于在两台服务器之间同步登录名。 切勿使用 HASHED 选项定期更改密码。

OLD_PASSWORD ='oldpassword' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 要指派新密码的登录的当前密码。 密码是区分大小写的。

MUST_CHANGE 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 如果包括此选项，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在首次使用已更改的登录名时提示输入更新的密码。

DEFAULT_DATABASE =database 指定将指派给登录名的默认数据库。

DEFAULT_LANGUAGE =language 指定将指派给登录名的默认语言。 所有 SQL 数据库登录名的默认语言为英语，并且无法更改。 Linux 上 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 `sa` 登录名的默认语言是英语，但可以更改。

NAME = login_name 正在重命名的登录的新名称。 如果是 Windows 登录，则与新名称对应的 Windows 主体的 SID 必须匹配与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的登录相关联的 SID。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的新名称不能包含反斜杠字符 (\\)。

CHECK_EXPIRATION = { ON | OFF } 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定是否应对此登录帐户强制实施密码过期策略。 默认值为 OFF。

CHECK_POLICY = { ON | OFF } 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定应对此登录名强制实施运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机的 Windows 密码策略。 默认值为 ON。

CREDENTIAL = credential_name 将映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录的凭据的名称。 该凭据必须已存在于服务器中。 有关详细信息，请参阅[凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)。 凭据不能映射到 sa 登录名。

NO CREDENTIAL 删除登录到服务器凭据的当前所有映射。 有关详细信息，请参阅[凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)。

UNLOCK 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定应解锁被锁定的登录名。

ADD CREDENTIAL 将可扩展的密钥管理 (EKM) 提供程序凭据添加到登录名。 有关详细信息，请参阅[可扩展的密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。

DROP CREDENTIAL 从登录名删除可扩展密钥管理 (EKM) 提供程序凭据。 有关详细信息，请参阅 [可扩展的密钥管理 (EKM)] (../.. /relational-databases/security/encryption/extensible-key-management-ekm.md)。

## <a name="remarks"></a>备注

如果 CHECK_POLICY 设置为 ON，则无法使用 HASHED 参数。

如果 CHECK_POLICY 更改为 ON，则将出现以下行为：

- 用当前的密码哈希值初始化密码历史记录。

  如果 CHECK_POLICY 更改为 OFF，则将出现以下行为：

- CHECK_EXPIRATION 也设置为 OFF。
- 清除密码历史记录。
- 重置 lockout_time 的值。

如果指定 MUST_CHANGE，则 CHECK_EXPIRATION 和 CHECK_POLICY 必须设置为 ON。 否则，该语句将失败。

如果 CHECK_POLICY 设置为 OFF，则 CHECK_EXPIRATION 不能设置为 ON。 包含此选项组合的 ALTER LOGIN 语句将失败。

不能使用带 DISABLE 参数的 ALTER LOGIN 来拒绝对 Windows 组的访问。 例如，ALTER LOGIN [domain\group] DISABLE 将返回下列错误信息：

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

这是设计的结果。
  
在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中，对连接和服务器级别的防火墙规则进行身份验证时所需的登录数据会暂时缓存在每个数据库中。 此缓存定期刷新。 若要强制刷新身份验证缓存并确保数据库具有最新版本的登录表，请执行 [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)。

## <a name="permissions"></a>权限

需要 ALTER ANY LOGIN 权限。

如果使用 CREDENTIAL 选项，则还需要 ALTER ANY CREDENTIAL 权限。

如果正在更改的登录名是 sysadmin 固定服务器角色的成员或 CONTROL SERVER 权限的被授权者，则进行以下更改时还需要 CONTROL SERVER 权限：

- 在不提供旧密码的情况下重置密码。
- 启用 MUST_CHANGE、CHECK_POLICY 或 CHECK_EXPIRATION。
- 更改登录名。
- 启用或禁用登录名。
- 将登录名映射到其他凭据。

主体可更改用于自身登录的密码、默认语言以及默认数据库。

## <a name="examples"></a>示例

### <a name="a-enabling-a-disabled-login"></a>A. 启用已禁用的登录名

以下示例将启用 `Mary5` 登录名。

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. 更改登录密码

以下示例将登录名 `Mary5` 的密码更改为强密码。

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-password-of-a-login-when-logged-in-as-the-login"></a>C. 登录时更改登录密码

如果正在尝试更改当前登录所用登录名的密码，但没有 `ALTER ANY LOGIN` 权限，则必须指定 `OLD_PASSWORD` 选项。

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>' OLD_PASSWORD = '<oldWeakPasswordHere>';
```

### <a name="d-changing-the-name-of-a-login"></a>D. 更改登录名称

以下示例将 `Mary5` 登录名称更改为 `John2`。

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="e-mapping-a-login-to-a-credential"></a>E. 将登录名映射到凭据

以下示例将登录名 `John2` 映射到凭据 `Custodian04`。

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="f-mapping-a-login-to-an-extensible-key-management-credential"></a>F. 将登录名映射到可扩展密钥管理凭据

以下示例将登录名 `Mary5` 映射到 EKM 凭据 `EKMProvider1`。

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. 解除锁定登录名

若要解除锁定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，请执行以下语句，并将 \*\*\*\* 替换为所需帐户密码。

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

若要在不更改密码的情况下解除锁定登录名，请关闭检查策略，然后再打开此检查策略。

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. 使用 HASHED 更改登录名的密码

以下示例将 `TestUser` 登录名的密码更改为已经过哈希运算的值。

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>另请参阅

- [凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [可扩展的密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        \* SQL 数据库 \*
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL 数据库

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Azure SQL Database

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE

<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
    ]
    | NAME = login_name
```

## <a name="arguments"></a>参数

login_name 指定正在更改的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的名称。 域登录名必须用方括号括起来，其格式为 [domain\user]。

ENABLE | DISABLE 启用或禁用此登录名。 禁用登录名不会影响已连接登录名的行为。 （使用 `KILL` 语句终止现有连接。）禁用的登录名将保留它们的权限，且仍然可以模拟。

PASSWORD ='password' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定正在更改的登录名的密码。 密码是区分大小写的。

与 SQL 数据库持续保持活动连接需要至少每隔 10 小时进行重新授权（由数据库引擎执行）。 数据库引擎使用最初提交的密码尝试重新授权，且无需用户输入。 出于性能原因，在 SQL 数据库中重置密码时，连接将不会重新进行身份验证，即使该连接因连接池而重置。 这与本地 SQL Server 的行为不同。 如果自最初授权连接时已更改密码，则必须终止连接，并使用新密码建立新连接。 具有 KILL DATABASE CONNECTION 权限的用户可使用 KILL 命令，显式终止与 SQL 数据库的连接。 有关详细信息，请参阅 [KILL](../../t-sql/language-elements/kill-transact-sql.md)。

> [!IMPORTANT]
> 当一个登录名（或包含的数据库用户）进行连接并经过身份验证后，该连接缓存有关该登录名的标识信息。 对于 Windows 身份验证登录，此标识信息包含有关 Windows 组中的成员身份的信息。 只要保持连接，该登录名的标识就保持已经过身份验证的状态。 若要在标识中强制进行更改（例如，重置密码或更改 Windows 组成员身份），则必须从身份验证机构（Windows 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）注销，然后再次登录。 **sysadmin** 固定服务器角色的成员或任何使用 **ALTER ANY CONNECTION** 权限的登录名都可以使用 **KILL** 命令结束连接，并强制登录名重新连接。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 可以在打开与对象资源管理器和查询编辑器窗口之间的多个连接时重用连接信息。 关闭所有连接可强制重新连接。

OLD_PASSWORD ='oldpassword' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 要指派新密码的登录的当前密码。 密码是区分大小写的。

NAME = login_name 正在重命名的登录的新名称。 如果是 Windows 登录，则与新名称对应的 Windows 主体的 SID 必须匹配与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的登录相关联的 SID。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的新名称不能包含反斜杠字符 (\\)。

## <a name="remarks"></a>备注

在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中，对连接和服务器级别的防火墙规则进行身份验证时所需的登录数据会暂时缓存在每个数据库中。 此缓存定期刷新。 若要强制刷新身份验证缓存并确保数据库具有最新版本的登录表，请执行 [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)。

## <a name="permissions"></a>权限

需要 ALTER ANY LOGIN 权限。

如果正在更改的登录名是 sysadmin 固定服务器角色的成员或 CONTROL SERVER 权限的被授权者，则进行以下更改时还需要 CONTROL SERVER 权限：

- 在不提供旧密码的情况下重置密码。
- 更改登录名。
- 启用或禁用登录名。
- 将登录名映射到其他凭据。

主体可更改自己的登录密码。

## <a name="examples"></a>示例

这些示例还包括使用其他 SQL 产品的示例。 请参阅上述支持的参数。

### <a name="a-enabling-a-disabled-login"></a>A. 启用已禁用的登录名

以下示例将启用 `Mary5` 登录名。

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. 更改登录密码

以下示例将登录名 `Mary5` 的密码更改为强密码。

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. 更改登录名称

以下示例将 `Mary5` 登录名称更改为 `John2`。

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. 将登录名映射到凭据

以下示例将登录名 `John2` 映射到凭据 `Custodian04`。

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. 将登录名映射到可扩展密钥管理凭据

以下示例将登录名 `Mary5` 映射到 EKM 凭据 `EKMProvider1`。


**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. 解除锁定登录名

若要解除锁定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，请执行以下语句，并将 \*\*\*\* 替换为所需帐户密码。

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

若要在不更改密码的情况下解除锁定登录名，请关闭检查策略，然后再打开此检查策略。

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. 使用 HASHED 更改登录名的密码

以下示例将 `TestUser` 登录名的密码更改为已经过哈希运算的值。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>另请参阅

- [凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [可扩展的密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL 托管实例 \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL 托管实例

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for SQL Server and Azure SQL Managed Instance

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    | <cryptographic_credential_option>
    }
[;]

<status_option> ::=
        ENABLE | DISABLE
  
<set_option> ::=
    PASSWORD = 'password' | hashed_password HASHED
    [
      OLD_PASSWORD = 'oldpassword'
      | <password_option> [<password_option> ]
    ]
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }
    | CREDENTIAL = credential_name
    | NO CREDENTIAL

<password_option> ::=
    MUST_CHANGE | UNLOCK

<cryptographic_credentials_option> ::=
    ADD CREDENTIAL credential_name
  | DROP CREDENTIAL credential_name
```

> [!NOTE]
> 创建后托管实例功能的 Azure AD 管理员已更改。 有关详细信息，请参阅[适用于 MI 的新 Azure AD 管理员功能](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi)。

```syntaxsql
-- Syntax for Azure SQL Managed Instance using Azure AD logins

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE

<set_option> ::=
     DEFAULT_DATABASE = database
   | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>参数

### <a name="arguments-applicable-to-sql-and-azure-ad-logins"></a>适用于 SQL 和 Azure AD 登录名的参数

login_name 指定正在更改的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的名称。 Azure AD 登录名必须以 user@domain 的形式指定。 例如，john.smith@contoso.com，或者指定为 Azure AD 组或应用程序名称。 对于 Azure AD 登录名，login_name 必须与 master 数据库中创建的现有 Azure AD 登录名对应。

ENABLE | DISABLE 启用或禁用此登录名。 禁用登录名不会影响已连接登录名的行为。 （使用 `KILL` 语句终止现有连接。）禁用的登录名将保留它们的权限，且仍然可以模拟。

DEFAULT_DATABASE =database 指定将指派给登录名的默认数据库。

DEFAULT_LANGUAGE =language 指定将指派给登录名的默认语言。 所有 SQL 数据库登录名的默认语言为英语，并且无法更改。 Linux 上 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 `sa` 登录名的默认语言是英语，但可以更改。

### <a name="arguments-applicable-only-to-sql-logins"></a>仅适用于 SQL 登录名的参数

PASSWORD ='password' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定正在更改的登录名的密码。 密码是区分大小写的。 使用外部登录名（如 Azure AD 登录名）时，密码也不适用。

与 SQL 数据库持续保持活动连接需要至少每隔 10 小时进行重新授权（由数据库引擎执行）。 数据库引擎使用最初提交的密码尝试重新授权，且无需用户输入。 出于性能原因，在 SQL 数据库中重置密码时，连接将不会重新进行身份验证，即使该连接因连接池而重置。 这与本地 SQL Server 的行为不同。 如果自最初授权连接时已更改密码，则必须终止连接，并使用新密码建立新连接。 具有 KILL DATABASE CONNECTION 权限的用户可使用 KILL 命令，显式终止与 SQL 数据库的连接。 有关详细信息，请参阅 [KILL](../../t-sql/language-elements/kill-transact-sql.md)。

PASSWORD = hashed\_password 仅适用于 HASHED 关键字。 指定要创建的登录名的密码的哈希值。

HASHED 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定在 PASSWORD 参数后输入的密码已经过哈希运算。 如果未选择此选项，则在将密码存储到数据库之前，对其进行哈希运算。 此选项只能用于在两台服务器之间同步登录名。 切勿使用 HASHED 选项定期更改密码。

OLD_PASSWORD ='oldpassword' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 要指派新密码的登录的当前密码。 密码是区分大小写的。

MUST_CHANGE<br>
仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 如果包括此选项，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在首次使用已更改的登录名时提示输入更新的密码。

NAME = login_name 正在重命名的登录的新名称。 如果使用 Windows 登录名，则与新名称对应的 Windows 主体的 SID 必须匹配与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的登录名关联的 SID。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的新名称不能包含反斜杠字符 (\\)。

CHECK_EXPIRATION = { ON | OFF } 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定是否应对此登录帐户强制实施密码过期策略。 默认值为 OFF。

CHECK_POLICY = { ON | OFF } 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定应对此登录名强制实施运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机的 Windows 密码策略。 默认值为 ON。

CREDENTIAL = credential_name 将映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录的凭据的名称。 该凭据必须已存在于服务器中。 有关详细信息，请参阅[凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)。 凭据不能映射到 sa 登录名。

NO CREDENTIAL 删除登录到服务器凭据的当前所有映射。 有关详细信息，请参阅[凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)。

UNLOCK 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定应解锁被锁定的登录名。

ADD CREDENTIAL 将可扩展的密钥管理 (EKM) 提供程序凭据添加到登录名。 有关详细信息，请参阅[可扩展的密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。

DROP CREDENTIAL 从登录名删除可扩展密钥管理 (EKM) 提供程序凭据。 有关详细信息，请参阅[可扩展的密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。

## <a name="remarks"></a>备注

如果 CHECK_POLICY 设置为 ON，则无法使用 HASHED 参数。

如果 CHECK_POLICY 更改为 ON，则将出现以下行为：

- 用当前的密码哈希值初始化密码历史记录。

  如果 CHECK_POLICY 更改为 OFF，则将出现以下行为：

- CHECK_EXPIRATION 也设置为 OFF。
- 清除密码历史记录。
- 重置 lockout_time 的值。

如果指定 MUST_CHANGE，则 CHECK_EXPIRATION 和 CHECK_POLICY 必须设置为 ON。 否则，该语句将失败。

如果 CHECK_POLICY 设置为 OFF，则 CHECK_EXPIRATION 不能设置为 ON。 包含此选项组合的 ALTER LOGIN 语句将失败。

不能使用带 DISABLE 参数的 ALTER_LOGIN 来拒绝对 Windows 组的访问。 这是设计的结果。 例如，ALTER_LOGIN [domain\group] DISABLE 将返回下列错误信息：

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中，对连接和服务器级别的防火墙规则进行身份验证时所需的登录数据会暂时缓存在每个数据库中。 此缓存定期刷新。 若要强制刷新身份验证缓存并确保数据库具有最新版本的登录表，请执行 [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)。

## <a name="permissions"></a>权限

需要 ALTER ANY LOGIN 权限。

如果使用 CREDENTIAL 选项，则还需要 ALTER ANY CREDENTIAL 权限。

如果正在更改的登录名是 sysadmin 固定服务器角色的成员或 CONTROL SERVER 权限的被授权者，则进行以下更改时还需要 CONTROL SERVER 权限：

- 在不提供旧密码的情况下重置密码。
- 启用 MUST_CHANGE、CHECK_POLICY 或 CHECK_EXPIRATION。
- 更改登录名。
- 启用或禁用登录名。
- 将登录名映射到其他凭据。

主体可更改用于自身登录的密码、默认语言以及默认数据库。

只有拥有 `sysadmin` 权限的 SQL 主体才能对 Azure AD 登录名执行 ALTER LOGIN 命令。

## <a name="examples"></a>示例

这些示例还包括使用其他 SQL 产品的示例。 请参阅上述支持的参数。

### <a name="a-enabling-a-disabled-login"></a>A. 启用已禁用的登录名

以下示例将启用 `Mary5` 登录名。

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. 更改登录密码

以下示例将登录名 `Mary5` 的密码更改为强密码。

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. 更改登录名称

以下示例将 `Mary5` 登录名称更改为 `John2`。

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. 将登录名映射到凭据

以下示例将登录名 `John2` 映射到凭据 `Custodian04`。

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. 将登录名映射到可扩展密钥管理凭据

以下示例将登录名 `Mary5` 映射到 EKM 凭据 `EKMProvider1`。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本，以及 Azure SQL 托管实例。

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. 解除锁定登录名

若要解除锁定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，请执行以下语句，并将 \*\*\*\* 替换为所需帐户密码。

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

若要在不更改密码的情况下解除锁定登录名，请关闭检查策略，然后再打开此检查策略。

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. 使用 HASHED 更改登录名的密码

以下示例将 `TestUser` 登录名的密码更改为已经过哈希运算的值。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本，以及 Azure SQL 托管实例。

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

### <a name="h-disabling-the-login-of-an-azure-ad-user"></a>H. 禁用 Azure AD 用户的登录名

以下示例禁用 Azure AD 用户 joe@contoso.com 的登录。

```sql
ALTER LOGIN [joe@contoso.com] DISABLE
```

## <a name="see-also"></a>另请参阅

- [凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [可扩展的密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Azure Synapse

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE
  
<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
    ]
    | NAME = login_name
```

## <a name="arguments"></a>参数

login_name 指定正在更改的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的名称。 域登录名必须用方括号括起来，其格式为 [domain\user]。

ENABLE | DISABLE 启用或禁用此登录名。 禁用登录名不会影响已连接登录名的行为。 （使用 `KILL` 语句终止现有连接。）禁用的登录名将保留它们的权限，且仍然可以模拟。

PASSWORD ='password' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定正在更改的登录名的密码。 密码是区分大小写的。

与 SQL 数据库持续保持活动连接需要至少每隔 10 小时进行重新授权（由数据库引擎执行）。 数据库引擎使用最初提交的密码尝试重新授权，且无需用户输入。 出于性能原因，在 SQL 数据库中重置密码时，连接将不会重新进行身份验证，即使该连接因连接池而重置。 这与本地 SQL Server 的行为不同。 如果自最初授权连接时已更改密码，则必须终止连接，并使用新密码建立新连接。 具有 KILL DATABASE CONNECTION 权限的用户可使用 KILL 命令，显式终止与 SQL 数据库的连接。 有关详细信息，请参阅 [KILL](../../t-sql/language-elements/kill-transact-sql.md)。

> [!IMPORTANT]
> 当一个登录名（或包含的数据库用户）进行连接并经过身份验证后，该连接缓存有关该登录名的标识信息。 对于 Windows 身份验证登录，此标识信息包含有关 Windows 组中的成员身份的信息。 只要保持连接，该登录名的标识就保持已经过身份验证的状态。 若要在标识中强制进行更改（例如，重置密码或更改 Windows 组成员身份），则必须从身份验证机构（Windows 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）注销，然后再次登录。 **sysadmin** 固定服务器角色的成员或任何使用 **ALTER ANY CONNECTION** 权限的登录名都可以使用 **KILL** 命令结束连接，并强制登录名重新连接。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 可以在打开与对象资源管理器和查询编辑器窗口之间的多个连接时重用连接信息。 关闭所有连接可强制重新连接。

OLD_PASSWORD ='oldpassword' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 要指派新密码的登录的当前密码。 密码是区分大小写的。

NAME = login_name 正在重命名的登录的新名称。 如果是 Windows 登录，则与新名称对应的 Windows 主体的 SID 必须匹配与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的登录相关联的 SID。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的新名称不能包含反斜杠字符 (\\)。

## <a name="remarks"></a>备注

在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中，对连接和服务器级别的防火墙规则进行身份验证时所需的登录数据会暂时缓存在每个数据库中。 此缓存定期刷新。 若要强制刷新身份验证缓存并确保数据库具有最新版本的登录表，请执行 [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)。

## <a name="permissions"></a>权限

需要 ALTER ANY LOGIN 权限。

如果正在更改的登录名是 sysadmin 固定服务器角色的成员或 CONTROL SERVER 权限的被授权者，则进行以下更改时还需要 CONTROL SERVER 权限：

- 在不提供旧密码的情况下重置密码。
- 更改登录名。
- 启用或禁用登录名。
- 将登录名映射到其他凭据。

主体可更改自己的登录密码。

## <a name="examples"></a>示例

这些示例还包括使用其他 SQL 产品的示例。 请参阅上述支持的参数。

### <a name="a-enabling-a-disabled-login"></a>A. 启用已禁用的登录名

以下示例将启用 `Mary5` 登录名。

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. 更改登录密码

以下示例将登录名 `Mary5` 的密码更改为强密码。

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. 更改登录名称

以下示例将 `Mary5` 登录名称更改为 `John2`。

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. 将登录名映射到凭据

以下示例将登录名 `John2` 映射到凭据 `Custodian04`。

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. 将登录名映射到可扩展密钥管理凭据

以下示例将登录名 `Mary5` 映射到 EKM 凭据 `EKMProvider1`。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. 解除锁定登录名

若要解除锁定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，请执行以下语句，并将 \*\*\*\* 替换为所需帐户密码。

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

若要在不更改密码的情况下解除锁定登录名，请关闭检查策略，然后再打开此检查策略。

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. 使用 HASHED 更改登录名的密码

以下示例将 `TestUser` 登录名的密码更改为已经过哈希运算的值。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>另请参阅

- [凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [可扩展的密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
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

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    }

<status_option> ::=ENABLE | DISABLE

<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
      | <password_option> [<password_option> ]
    ]
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }

<password_option> ::=
    MUST_CHANGE | UNLOCK
```

## <a name="arguments"></a>参数

login_name 指定正在更改的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的名称。 域登录名必须用方括号括起来，其格式为 [domain\user]。

ENABLE | DISABLE 启用或禁用此登录名。 禁用登录名不会影响已连接登录名的行为。 （使用 `KILL` 语句终止现有连接。）禁用的登录名将保留它们的权限，且仍然可以模拟。

PASSWORD ='password' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定正在更改的登录名的密码。 密码是区分大小写的。

> [!IMPORTANT]
> 当一个登录名（或包含的数据库用户）进行连接并经过身份验证后，该连接缓存有关该登录名的标识信息。 对于 Windows 身份验证登录，此标识信息包含有关 Windows 组中的成员身份的信息。 只要保持连接，该登录名的标识就保持已经过身份验证的状态。 若要在标识中强制进行更改（例如，重置密码或更改 Windows 组成员身份），则必须从身份验证机构（Windows 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）注销，然后再次登录。 **sysadmin** 固定服务器角色的成员或任何使用 **ALTER ANY CONNECTION** 权限的登录名都可以使用 **KILL** 命令结束连接，并强制登录名重新连接。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 可以在打开与对象资源管理器和查询编辑器窗口之间的多个连接时重用连接信息。 关闭所有连接可强制重新连接。

OLD_PASSWORD ='oldpassword' 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 要指派新密码的登录的当前密码。 密码是区分大小写的。

MUST_CHANGE 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 如果包括此选项，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在首次使用已更改的登录名时提示输入更新的密码。

NAME = login_name 正在重命名的登录的新名称。 如果使用 Windows 登录名，则与新名称对应的 Windows 主体的 SID 必须匹配与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的登录名关联的 SID。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的新名称不能包含反斜杠字符 (\\)。

CHECK_EXPIRATION = { ON | OFF } 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定是否应对此登录帐户强制实施密码过期策略。 默认值为 OFF。

CHECK_POLICY = { ON | OFF } 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定应对此登录名强制实施运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机的 Windows 密码策略。 默认值为 ON。

UNLOCK 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 指定应解锁被锁定的登录名。

## <a name="remarks"></a>备注

如果 CHECK_POLICY 设置为 ON，则无法使用 HASHED 参数。

如果 CHECK_POLICY 更改为 ON，则将出现以下行为：

- 用当前的密码哈希值初始化密码历史记录。

  如果 CHECK_POLICY 更改为 OFF，则将出现以下行为：

- CHECK_EXPIRATION 也设置为 OFF。
- 清除密码历史记录。
- 重置 lockout_time 的值。

如果指定 MUST_CHANGE，则 CHECK_EXPIRATION 和 CHECK_POLICY 必须设置为 ON。 否则，该语句将失败。

如果 CHECK_POLICY 设置为 OFF，则 CHECK_EXPIRATION 不能设置为 ON。 包含此选项组合的 ALTER LOGIN 语句将失败。

不能使用带 DISABLE 参数的 ALTER_LOGIN 来拒绝对 Windows 组的访问。 这是设计的结果。 例如，ALTER_LOGIN [domain\group] DISABLE 将返回下列错误信息：

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中，对连接和服务器级别的防火墙规则进行身份验证时所需的登录数据会暂时缓存在每个数据库中。 此缓存定期刷新。 若要强制刷新身份验证缓存并确保数据库具有最新版本的登录表，请执行 [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)。

## <a name="permissions"></a>权限

需要 ALTER ANY LOGIN 权限。

如果使用 CREDENTIAL 选项，则还需要 ALTER ANY CREDENTIAL 权限。

如果正在更改的登录名是 sysadmin 固定服务器角色的成员或 CONTROL SERVER 权限的被授权者，则进行以下更改时还需要 CONTROL SERVER 权限：

- 在不提供旧密码的情况下重置密码。
- 启用 MUST_CHANGE、CHECK_POLICY 或 CHECK_EXPIRATION。
- 更改登录名。
- 启用或禁用登录名。
- 将登录名映射到其他凭据。

主体可更改用于自身登录的密码、默认语言以及默认数据库。

## <a name="examples"></a>示例

这些示例还包括使用其他 SQL 产品的示例。 请参阅上述支持的参数。

### <a name="a-enabling-a-disabled-login"></a>A. 启用已禁用的登录名

以下示例将启用 `Mary5` 登录名。

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. 更改登录密码

以下示例将登录名 `Mary5` 的密码更改为强密码。

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. 更改登录名称

以下示例将 `Mary5` 登录名称更改为 `John2`。

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. 将登录名映射到凭据

以下示例将登录名 `John2` 映射到凭据 `Custodian04`。

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. 将登录名映射到可扩展密钥管理凭据

以下示例将登录名 `Mary5` 映射到 EKM 凭据 `EKMProvider1`。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. 解除锁定登录名

若要解除锁定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，请执行以下语句，并将 \*\*\*\* 替换为所需帐户密码。

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

若要在不更改密码的情况下解除锁定登录名，请关闭检查策略，然后再打开此检查策略。

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. 使用 HASHED 更改登录名的密码

以下示例将 `TestUser` 登录名的密码更改为已经过哈希运算的值。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>另请参阅

- [凭据](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [可扩展的密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
