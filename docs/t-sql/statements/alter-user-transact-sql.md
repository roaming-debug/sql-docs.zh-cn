---
description: ALTER USER (Transact-SQL)
title: ALTER USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_USER_TSQL
- ALTER USER
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database users
- ALTER USER statement
- renaming databases
- schemas [SQL Server], default
- database user names [SQL Server]
- names [SQL Server], database users
- default schemas
- modifying default schemas
ms.assetid: 344fc6ce-a008-47c8-a02e-47fae66cc590
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 21314c02ddb58aabcee33257fc4d8d44f56b9da7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170569"
---
# <a name="alter-user-transact-sql"></a>ALTER USER (Transact-SQL)

重命名数据库用户或更改它的默认架构。

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for SQL Server

ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
```

## <a name="arguments"></a>参数

 userName 指定在此数据库中用于识别该用户的名称。

 LOGIN **=** _loginName_ 通过将用户的安全标识符 (SID) 更改为另一个登录名的 SID，使用户重新映射到该登录名。

 NAME = newUserName 指定此用户的新名称。 newUserName 不能已存在于当前数据库中。

 DEFAULT_SCHEMA = { schemaName | NULL } 指定服务器在解析此用户的对象名时将搜索的第一个架构。 将默认架构设置为 NULL 将从 Windows 组中删除默认架构。 Windows 用户不能使用 NULL 选项。

 PASSWORD **=** '*password*' **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

 指定正在更改的用户的密码。 密码是区分大小写的。

> [!NOTE]
> 此选项仅适用于包含的用户。 有关详细信息，请参阅[包含的数据库](../../relational-databases/databases/contained-databases.md)和 [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)。

 OLD_PASSWORD **=** _'oldpassword'_ 适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

 将替换为“password”的当前用户密码。 密码是区分大小写的。 除非拥有 ALTER ANY USER 权限，否则需要具有 OLD_PASSWORD 才能更改密码。 需要 OLD_PASSWORD 可防止拥有 IMPERSONATION 权限的用户更改密码。

> [!NOTE]
> 此选项仅适用于包含的用户。

 DEFAULT_LANGUAGE ={ NONE \| \<lcid> \| \<language name> \| \<language alias> } 适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本 。

 指定将指派给用户的默认语言。 如果将此选项设置为 NONE，则默认语言将设置为数据库的当前默认语言。 如果之后更改数据库的默认语言，用户的默认语言将保持不变。 DEFAULT_LANGUAGE 可以为本地 ID (lcid)、语言的名称或语言别名。

> [!NOTE]
> 此选项只能在包含数据库中指定，且只能用于包含的用户。

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 和更高版本 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]。

 取消在大容量复制操作期间对服务器进行加密元数据检查。 这使用户能够在表或数据库之间大容量复制加密数据，而无需对数据进行解密。 默认为 OFF。

> [!WARNING]
> 错误使用此选项可能导致数据损坏。 有关详细信息，请参阅[迁移通过 Always Encrypted 保护的敏感数据](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md)。

## <a name="remarks"></a>备注

 默认架构将是服务器为此数据库用户解析对象名时将搜索的第一个架构。 除非另外指定，否则默认架构将是此数据库用户创建的对象所属的架构。

 如果用户具有默认架构，则将使用默认架构。 如果用户不具有默认架构，但该用户是具有默认架构的组的成员，则将使用该组的默认架构。 如果用户不具有默认架构而且是多个组的成员，则该用户的默认架构将是具有最低 principle_id 的 Windows 组的架构和一个显式设置的默认架构。 如果不能为用户确定默认架构，则将使用 dbo 架构。

 可以将 DEFAULT_SCHEMA 设置为数据库中当前不存在的架构。 因此，可以在创建架构之前将 DEFAULT_SCHEMA 分配给用户。

 不能为映射到证书或非对称密钥的用户指定 DEFAULT_SCHEMA。

> [!IMPORTANT]
> 如果用户是 sysadmin 固定服务器角色的成员，则忽略 DEFAULT_SCHEMA 的值。 sysadmin 固定服务器角色的所有成员都有默认架构 `dbo`。

 仅当新用户名的 SID 与在数据库中记录的 SID 匹配时，才能更改映射到 Windows 登录名或组的用户的名称。 此检查将帮助防止数据库中的 Windows 登录名欺骗。

 使用 WITH LOGIN 子句可以将用户重新映射到一个不同的登录名。 不能使用此子句重新映射以下用户：不具有登录名的用户、映射到证书的用户或映射到非对称密钥的用户。 只能重新映射 SQL 用户和 Windows 用户（或组）。 不能使用 WITH LOGIN 子句更改用户类型，例如将 Windows 帐户更改为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。

 如果满足以下条件，则用户的名称会自动重命名为登录名。

- 用户是一个 Windows 用户。

- 名称是一个 Windows 名称（包含反斜杠）。

- 未指定新名称。

- 当前名称不同于登录名。

 如果不满足上述条件，则不会重命名用户，除非调用方另外调用了 NAME 子句。

被映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名、证书或非对称密钥的用户名不能包含反斜杠字符 (\\)。

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>安全性

> [!NOTE]
> 拥有 ALTER ANY USER 权限的用户可以更改任何用户的默认架构。 更改了架构的用户可能会在不知情的情况下从错误表中选择数据，或者从错误架构中执行代码。

### <a name="permissions"></a>权限

 更改用户名需要具有 ALTER ANY USER 权限。

 更改用户的目标登录名需要对数据库拥有 CONTROL 权限。

 若要更改对数据库拥有 CONTROL 权限的用户名名称，则需要对数据库拥有 CONTROL 权限 。

 更改默认架构或语言需要对用户拥有 ALTER 权限。 用户可更改自己的默认架构或语言。

## <a name="examples"></a>示例

所有示例都在用户数据库中执行。

### <a name="a-changing-the-name-of-a-database-user"></a>A. 更改数据库用户的名称

 以下示例将数据库用户 `Mary5` 的名称更改为 `Mary51`。

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. 更改用户的默认架构

 以下示例将用户 `Mary51` 的默认架构更改为 `Purchasing`。

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. 同时更改几个选项

 以下示例可在一个语句中为一个包含数据库用户更改若干个选项。

**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

## <a name="see-also"></a>另请参阅

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [包含的数据库](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        \* SQL 数据库 \*
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL 数据库

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Azure SQL Database

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = schemaName
| LOGIN = loginName
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
[;]

-- Azure SQL Database Update Syntax
ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- SQL Database syntax when connected to a federation member
ALTER USER userName
 WITH <set_item> [ ,... n ]
[;]

<set_item> ::=
 NAME = newUserName
```

## <a name="arguments"></a>参数

 userName 指定在此数据库中用于识别该用户的名称。

 LOGIN **=** _loginName_ 通过将用户的安全标识符 (SID) 更改为另一个登录名的 SID，使用户重新映射到该登录名。

 如果 ALTER USER 语句是 SQL 批处理中唯一的语句，则 Azure SQL 数据库将支持 WITH LOGIN 子句。 如果 ALTER USER 语句不是 SQL 批处理中唯一的语句或在动态 SQL 中执行，则不支持 WITH LOGIN 子句。

 NAME = newUserName 指定此用户的新名称。 newUserName 不能已存在于当前数据库中。

 DEFAULT_SCHEMA = { schemaName | NULL } 指定服务器在解析此用户的对象名时将搜索的第一个架构。 将默认架构设置为 NULL 将从 Windows 组中删除默认架构。 Windows 用户不能使用 NULL 选项。

 PASSWORD **=** '*password*' **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

 指定正在更改的用户的密码。 密码是区分大小写的。

> [!NOTE]
> 此选项仅适用于包含的用户。 有关详细信息，请参阅[包含的数据库](../../relational-databases/databases/contained-databases.md)和 [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)。

 OLD_PASSWORD **=** _'oldpassword'_ 适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

 将替换为“password”的当前用户密码。 密码是区分大小写的。 除非拥有 ALTER ANY USER 权限，否则需要具有 OLD_PASSWORD 才能更改密码。 需要 OLD_PASSWORD 可防止拥有 IMPERSONATION 权限的用户更改密码。

> [!NOTE]
> 此选项仅适用于包含的用户。

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 和更高版本 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]。

 取消在大容量复制操作期间对服务器进行加密元数据检查。 这使用户能够在表或数据库之间大容量复制加密数据，而无需对数据进行解密。 默认为 OFF。

> [!WARNING]
> 错误使用此选项可能导致数据损坏。 有关详细信息，请参阅[迁移通过 Always Encrypted 保护的敏感数据](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md)。

## <a name="remarks"></a>备注

 默认架构将是服务器为此数据库用户解析对象名时将搜索的第一个架构。 除非另外指定，否则默认架构将是此数据库用户创建的对象所属的架构。

 如果用户具有默认架构，则将使用默认架构。 如果用户不具有默认架构，但该用户是具有默认架构的组的成员，则将使用该组的默认架构。 如果用户不具有默认架构而且是多个组的成员，则该用户的默认架构将是具有最低 principle_id 的 Windows 组的架构和一个显式设置的默认架构。 如果不能为用户确定默认架构，则将使用 dbo 架构。

 可以将 DEFAULT_SCHEMA 设置为数据库中当前不存在的架构。 因此，可以在创建架构之前将 DEFAULT_SCHEMA 分配给用户。

 不能为映射到证书或非对称密钥的用户指定 DEFAULT_SCHEMA。

> [!IMPORTANT]
> 如果用户是 sysadmin 固定服务器角色的成员，则忽略 DEFAULT_SCHEMA 的值。 sysadmin 固定服务器角色的所有成员都有默认架构 `dbo`。

 仅当新用户名的 SID 与在数据库中记录的 SID 匹配时，才能更改映射到 Windows 登录名或组的用户的名称。 此检查将帮助防止数据库中的 Windows 登录名欺骗。

 使用 WITH LOGIN 子句可以将用户重新映射到一个不同的登录名。 不能使用此子句重新映射以下用户：不具有登录名的用户、映射到证书的用户或映射到非对称密钥的用户。 只能重新映射 SQL 用户和 Windows 用户（或组）。 不能使用 WITH LOGIN 子句更改用户类型，例如将 Windows 帐户更改为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。

 如果满足以下条件，则用户的名称会自动重命名为登录名。

- 用户是一个 Windows 用户。

- 名称是一个 Windows 名称（包含反斜杠）。

- 未指定新名称。

- 当前名称不同于登录名。

 如果不满足上述条件，则不会重命名用户，除非调用方另外调用了 NAME 子句。

被映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名、证书或非对称密钥的用户名不能包含反斜杠字符 (\\)。

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>安全性

> [!NOTE]
> 拥有 ALTER ANY USER 权限的用户可以更改任何用户的默认架构。 更改了架构的用户可能会在不知情的情况下从错误表中选择数据，或者从错误架构中执行代码。

### <a name="permissions"></a>权限

 更改用户名需要具有 ALTER ANY USER 权限。

 更改用户的目标登录名需要对数据库拥有 CONTROL 权限。

 若要更改对数据库拥有 CONTROL 权限的用户名名称，则需要对数据库拥有 CONTROL 权限 。

 更改默认架构或语言需要对用户拥有 ALTER 权限。 用户可更改自己的默认架构或语言。

## <a name="examples"></a>示例

所有示例都在用户数据库中执行。

### <a name="a-changing-the-name-of-a-database-user"></a>A. 更改数据库用户的名称

 以下示例将数据库用户 `Mary5` 的名称更改为 `Mary51`。

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. 更改用户的默认架构

 以下示例将用户 `Mary51` 的默认架构更改为 `Purchasing`。

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. 同时更改几个选项

 以下示例可在一个语句中为一个包含数据库用户更改若干个选项。

```sql
ALTER USER Philip
WITHNAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per';
GO
```

## <a name="see-also"></a>另请参阅

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [包含的数据库](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL 托管实例 \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL 托管实例

## <a name="syntax"></a>语法

> [!IMPORTANT]
> Azure SQL 托管实例应用于具有 Azure AD 登录名的用户时，仅支持以下选项：`DEFAULT_SCHEMA = { schemaName | NULL }` 和 `DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }`
> </br> </br> 添加了新的语法扩展，有助于重新映射已迁移到 Azure SQL 托管实例的数据库中的用户。 ALTER USER 语法有助于将通过 Azure AD 联合并同步的域中的数据库用户映射到 Azure AD 登录名。

```syntaxsql
-- Syntax for SQL Managed Instance
ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- Users or groups that are migrated as federated and synchronized with Azure AD have the following syntax:

/** Applies to Windows users that were migrated and have the following user names:
- Windows user <domain\user>
- Windows group <domain\MyWindowsGroup>
- Windows alias <MyWindowsAlias>
**/

ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
 NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
```

## <a name="arguments"></a>参数

 userName 指定在此数据库中用于识别该用户的名称。

 LOGIN **=** _loginName_ 通过将用户的安全标识符 (SID) 更改为另一个登录名的 SID，使用户重新映射到该登录名。

 如果 ALTER USER 语句是 SQL 批处理中唯一的语句，则 Azure SQL 数据库将支持 WITH LOGIN 子句。 如果 ALTER USER 语句不是 SQL 批处理中唯一的语句或在动态 SQL 中执行，则不支持 WITH LOGIN 子句。

 NAME = newUserName 指定此用户的新名称。 newUserName 不能已存在于当前数据库中。

 DEFAULT_SCHEMA = { schemaName | NULL } 指定服务器在解析此用户的对象名时将搜索的第一个架构。 将默认架构设置为 NULL 将从 Windows 组中删除默认架构。 Windows 用户不能使用 NULL 选项。

 PASSWORD = 'password'

 指定正在更改的用户的密码。 密码是区分大小写的。

> [!NOTE]
> 此选项仅适用于包含的用户。 有关详细信息，请参阅[包含的数据库](../../relational-databases/databases/contained-databases.md)和 [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)。

 OLD_PASSWORD **=** _'oldpassword'_

 将替换为“password”的当前用户密码。 密码是区分大小写的。 除非拥有 ALTER ANY USER 权限，否则需要具有 OLD_PASSWORD 才能更改密码。 需要 OLD_PASSWORD 可防止拥有 IMPERSONATION 权限的用户更改密码。

> [!NOTE]
> 此选项仅适用于包含的用户。

 DEFAULT_LANGUAGE ={ NONE | \<lcid> | \<language name> | \<language alias> }

 指定将指派给用户的默认语言。 如果将此选项设置为 NONE，则默认语言将设置为数据库的当前默认语言。 如果之后更改数据库的默认语言，用户的默认语言将保持不变。 DEFAULT_LANGUAGE 可以为本地 ID (lcid)、语言的名称或语言别名。

> [!NOTE]
> 此选项只能在包含数据库中指定，且只能用于包含的用户。

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]

 取消在大容量复制操作期间对服务器进行加密元数据检查。 这使用户能够在表或数据库之间大容量复制加密数据，而无需对数据进行解密。 默认为 OFF。

> [!WARNING]
> 错误使用此选项可能导致数据损坏。 有关详细信息，请参阅[迁移通过 Always Encrypted 保护的敏感数据](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md)。

## <a name="remarks"></a>备注

 默认架构将是服务器为此数据库用户解析对象名时将搜索的第一个架构。 除非另外指定，否则默认架构将是此数据库用户创建的对象所属的架构。

 如果用户具有默认架构，则将使用默认架构。 如果用户不具有默认架构，但该用户是具有默认架构的组的成员，则将使用该组的默认架构。 如果用户不具有默认架构而且是多个组的成员，则该用户的默认架构将是具有最低 principle_id 的 Windows 组的架构和一个显式设置的默认架构。 如果不能为用户确定默认架构，则将使用 dbo 架构。

 可以将 DEFAULT_SCHEMA 设置为数据库中当前不存在的架构。 因此，可以在创建架构之前将 DEFAULT_SCHEMA 分配给用户。

 不能为映射到证书或非对称密钥的用户指定 DEFAULT_SCHEMA。

> [!IMPORTANT]
> 如果用户是 sysadmin 固定服务器角色的成员，则忽略 DEFAULT_SCHEMA 的值。 sysadmin 固定服务器角色的所有成员都有默认架构 `dbo`。

 仅当新用户名的 SID 与在数据库中记录的 SID 匹配时，才能更改映射到 Windows 登录名或组的用户的名称。 此检查将帮助防止数据库中的 Windows 登录名欺骗。

 使用 WITH LOGIN 子句可以将用户重新映射到一个不同的登录名。 不能使用此子句重新映射以下用户：不具有登录名的用户、映射到证书的用户或映射到非对称密钥的用户。 只能重新映射 SQL 用户和 Windows 用户（或组）。 不能使用 WITH LOGIN 子句更改用户类型，例如将 Windows 帐户更改为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 唯一的例外是将 Windows 用户更改为 Azure AD 用户。

> [!NOTE]
> 以下规则不适用于 Azure SQL 托管实例上的 Windows 用户，因为我们不支持在 Azure SQL 托管实例上创建 Windows 登录名。 仅当存在 Azure AD 登录名时，才能使用 WITH LOGIN 选项。

 如果满足以下条件，则用户的名称会自动重命名为登录名。

- 用户是一个 Windows 用户。

- 名称是一个 Windows 名称（包含反斜杠）。

- 未指定新名称。

- 当前名称不同于登录名。

 如果不满足上述条件，则不会重命名用户，除非调用方另外调用了 NAME 子句。

被映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名、证书或非对称密钥的用户名不能包含反斜杠字符 (\\)。

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

### <a name="remarks-for-windows-users-in-sql-on-premises-migrated-to-azure-sql-managed-instance"></a>关于迁移到 Azure SQL 托管实例的 SQL 本地 Windows 用户的备注

这些备注适用于作为已通过 Azure AD 联合并同步的 Windows 用户进行身份验证。

> [!NOTE]
> 在创建后，Azure SQL 托管实例功能的 Azure AD 管理员已更改。 有关详细信息，请参阅[适用于 MI 的新 Azure AD 管理员功能](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi)。

- 默认情况下，通过用于迁移目的的 ALTER USER 语法的所有版本中的图形 API 来验证映射到 Azure AD 的 Windows 用户或组。
- 具有别名的本地用户（使用与原始 Windows 帐户不同的名称）将保留别名。
- 对于 Azure AD 身份验证，LOGIN 参数仅适用于 Azure SQL 托管实例，不能与 SQL 数据库一起使用。
- 若要查看 Azure AD 主体的登录名，请使用以下命令：`select * from sys.server_principals`。
- 检查登录名的指示类型是 `E` 还是 `X`。
- PASSWORD 选项不能用于 Azure AD 用户。
- 在所有迁移情况下，Windows 用户或组的角色和权限将自动转移到新的 Azure AD 用户或组。
- 新的语法扩展 FROM EXTERNAL PROVIDER 可用于将 Windows 用户和组从 SQL 本地更改为 Azure AD 用户和组。 使用此扩展时，Windows 域必须通过 Azure AD 联合，且所有 Windows 域成员必须存在于 Azure AD 中。 FROM EXTERNAL PROVIDER 语法适用于 Azure SQL 托管实例，并且应在以下情况下使用：Windows 用户在原始 SQL 实例上没有登录名，并且需要映射到独立的 Azure AD 数据库用户。
- 在这种情况下，允许的用户名可以是：
- Widows 用户（域\用户）。
- Windows 组（MyWidnowsGroup）。
- Windows 别名（MyWindowsAlias）。
- ALTER 命令的结果会将旧用户名替换为基于旧用户名的原始 SID 在 Azure AD 中找到的相应名称。 更改的名称将被替换并存储在数据库的元数据中：
- （域\用户）将替换为 Azure AD user@domain.com。
- （域\\MyWidnowsGroup）将替换为 Azure AD 组。
- （MyWindowsAlias）将保持不变，但会在 Azure AD 中检查此用户的 SID。

> [!NOTE]
> 如果在 Azure AD 中找不到转换为 objectID 的原始用户的 SID，则 ALTER USER 命令将失败。

- 若要查看更改的用户，请使用以下命令：`select * from sys.database_principals`
- 检查用户的指示类型 `E` 还是 `X`。
- 当 NAME 用于将 Windows 用户迁移到 Azure AD 用户时，以下限制适用：
- 必须指定有效的 LOGIN。
- NAME 将在 Azure AD 中检查，并且只能是：
- LOGIN 的名称。
- 别名 - 名称不能存在于 Azure AD 中。
- 在所有其他情况下，语法将失败。

## <a name="security"></a>安全性

> [!NOTE]
> 拥有 ALTER ANY USER 权限的用户可以更改任何用户的默认架构。 更改了架构的用户可能会在不知情的情况下从错误表中选择数据，或者从错误架构中执行代码。

### <a name="permissions"></a>权限

 更改用户名需要具有 ALTER ANY USER 权限。

 更改用户的目标登录名需要对数据库拥有 CONTROL 权限。

 若要更改对数据库拥有 CONTROL 权限的用户名名称，则需要对数据库拥有 CONTROL 权限 。

 更改默认架构或语言需要对用户拥有 ALTER 权限。 用户可更改自己的默认架构或语言。

## <a name="examples"></a>示例

所有示例都在用户数据库中执行。

### <a name="a-changing-the-name-of-a-database-user"></a>A. 更改数据库用户的名称

 以下示例将数据库用户 `Mary5` 的名称更改为 `Mary51`。

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. 更改用户的默认架构

 以下示例将用户 `Mary51` 的默认架构更改为 `Purchasing`。

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. 同时更改几个选项

 以下示例可在一个语句中为一个包含数据库用户更改若干个选项。

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

### <a name="d-map-the-user-in-the-database-to-an-azure-ad-login-after-migration"></a>D. 迁移后将数据库中的用户映射到 Azure AD 登录名

以下示例将用户 `westus/joe` 重新映射到 Azure AD 用户 `joe@westus.com`。 此示例适用于已存在于托管实例中的登录名。 完成到 Azure SQL 托管实例的数据库迁移之后，并且想要使用 Azure AD 登录名进行身份验证时，需要执行此操作。

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com
```

### <a name="e-map-an-old-windows-user-in-the-database-without-a-login-in-azure-sql-managed-instance-to-an-azure-ad-user"></a>E. 将数据库中在 Azure SQL 托管实例中没有登录名的旧 Windows 用户映射到 Azure AD 用户

以下示例将不具有登录名的用户 `westus/joe` 重新映射到 Azure AD 用户 `joe@westus.com`。 联合用户必须存在于 Azure AD 中。

```sql
ALTER USER [westus/joe] FROM EXTERNAL PROVIDER
```

### <a name="f-map-the-user-alias-to-an-existing-azure-ad-login"></a>F. 将用户别名映射到现有 Azure AD 登录名

以下示例将用户名 `westus\joe` 重新映射到 `joe_alias`。 在这种情况下，相应的 Azure AD 登录名为 `joe@westus.com`。

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com, name= joe_alias
```

### <a name="g-map-a-windows-group-that-was-migrated-in-azure-sql-managed-instance-to-an-azure-ad-group"></a>G. 将 Azure SQL 托管实例中已迁移的 Windows 组映射到 Azure AD 组

以下示例将旧的本地组 `westus\mygroup` 重新映射到托管实例中的 Azure AD 组 `mygroup`。 组必须存在于 Azure AD 中。

```sql
ALTER USER [westus\mygroup] WITH LOGIN = mygroup
```

## <a name="see-also"></a>另请参阅

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [包含的数据库](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)
- [教程：使用 T-SQL DDL 语法将 SQL Server 本地 Windows 用户和组迁移到 SQL 托管实例](/azure/sql-database/tutorial-managed-instance-azure-active-directory-migration)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Azure Synapse

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>参数

 userName 指定在此数据库中用于识别该用户的名称。

 LOGIN **=** _loginName_ 通过将用户的安全标识符 (SID) 更改为另一个登录名的 SID，使用户重新映射到该登录名。

 如果 ALTER USER 语句是 SQL 批处理中唯一的语句，则 Azure SQL 数据库将支持 WITH LOGIN 子句。 如果 ALTER USER 语句不是 SQL 批处理中唯一的语句或在动态 SQL 中执行，则不支持 WITH LOGIN 子句。

 NAME = newUserName 指定此用户的新名称。 newUserName 不能已存在于当前数据库中。

 DEFAULT_SCHEMA = { schemaName | NULL } 指定服务器在解析此用户的对象名时将搜索的第一个架构。 将默认架构设置为 NULL 将从 Windows 组中删除默认架构。 Windows 用户不能使用 NULL 选项。

## <a name="remarks"></a>备注

 默认架构将是服务器为此数据库用户解析对象名时将搜索的第一个架构。 除非另外指定，否则默认架构将是此数据库用户创建的对象所属的架构。

 如果用户具有默认架构，则将使用默认架构。 如果用户不具有默认架构，但该用户是具有默认架构的组的成员，则将使用该组的默认架构。 如果用户不具有默认架构而且是多个组的成员，则该用户的默认架构将是具有最低 principle_id 的 Windows 组的架构和一个显式设置的默认架构。 如果不能为用户确定默认架构，则将使用 dbo 架构。

 可以将 DEFAULT_SCHEMA 设置为数据库中当前不存在的架构。 因此，可以在创建架构之前将 DEFAULT_SCHEMA 分配给用户。

 不能为映射到证书或非对称密钥的用户指定 DEFAULT_SCHEMA。

> [!IMPORTANT]
> 如果用户是 sysadmin 固定服务器角色的成员，则忽略 DEFAULT_SCHEMA 的值。 sysadmin 固定服务器角色的所有成员都有默认架构 `dbo`。

 使用 WITH LOGIN 子句可以将用户重新映射到一个不同的登录名。 不能使用此子句重新映射以下用户：不具有登录名的用户、映射到证书的用户或映射到非对称密钥的用户。 只能重新映射 SQL 用户和 Windows 用户（或组）。 不能使用 WITH LOGIN 子句更改用户类型，例如将 Windows 帐户更改为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。

 如果满足以下条件，则用户的名称会自动重命名为登录名。

- 未指定新名称。

- 当前名称不同于登录名。

 如果不满足上述条件，则不会重命名用户，除非调用方另外调用了 NAME 子句。

被映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名、证书或非对称密钥的用户名不能包含反斜杠字符 (\\)。

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>安全性

> [!NOTE]
> 拥有 ALTER ANY USER 权限的用户可以更改任何用户的默认架构。 更改了架构的用户可能会在不知情的情况下从错误表中选择数据，或者从错误架构中执行代码。

### <a name="permissions"></a>权限

 更改用户名需要具有 ALTER ANY USER 权限。

 更改用户的目标登录名需要对数据库拥有 CONTROL 权限。

 若要更改对数据库拥有 CONTROL 权限的用户名名称，则需要对数据库拥有 CONTROL 权限 。

 更改默认架构或语言需要对用户拥有 ALTER 权限。 用户可更改自己的默认架构或语言。

## <a name="examples"></a>示例

所有示例都在用户数据库中执行。

### <a name="a-changing-the-name-of-a-database-user"></a>A. 更改数据库用户的名称

 以下示例将数据库用户 `Mary5` 的名称更改为 `Mary51`。

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. 更改用户的默认架构

以下示例将用户 `Mary51` 的默认架构更改为 `Purchasing`。

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>另请参阅

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [包含的数据库](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
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

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>参数

 userName 指定在此数据库中用于识别该用户的名称。

 LOGIN **=** _loginName_ 通过将用户的安全标识符 (SID) 更改为另一个登录名的 SID，使用户重新映射到该登录名。

 如果 ALTER USER 语句是 SQL 批处理中唯一的语句，则 Azure SQL 数据库将支持 WITH LOGIN 子句。 如果 ALTER USER 语句不是 SQL 批处理中唯一的语句或在动态 SQL 中执行，则不支持 WITH LOGIN 子句。

 NAME = newUserName 指定此用户的新名称。 newUserName 不能已存在于当前数据库中。

 DEFAULT_SCHEMA = { schemaName | NULL } 指定服务器在解析此用户的对象名时将搜索的第一个架构。 将默认架构设置为 NULL 将从 Windows 组中删除默认架构。 Windows 用户不能使用 NULL 选项。

## <a name="remarks"></a>备注

 默认架构将是服务器为此数据库用户解析对象名时将搜索的第一个架构。 除非另外指定，否则默认架构将是此数据库用户创建的对象所属的架构。

 如果用户具有默认架构，则将使用默认架构。 如果用户不具有默认架构，但该用户是具有默认架构的组的成员，则将使用该组的默认架构。 如果用户不具有默认架构而且是多个组的成员，则该用户的默认架构将是具有最低 principle_id 的 Windows 组的架构和一个显式设置的默认架构。 如果不能为用户确定默认架构，则将使用 dbo 架构。

 可以将 DEFAULT_SCHEMA 设置为数据库中当前不存在的架构。 因此，可以在创建架构之前将 DEFAULT_SCHEMA 分配给用户。

 不能为映射到证书或非对称密钥的用户指定 DEFAULT_SCHEMA。

> [!IMPORTANT]
> 如果用户是 sysadmin 固定服务器角色的成员，则忽略 DEFAULT_SCHEMA 的值。 sysadmin 固定服务器角色的所有成员都有默认架构 `dbo`。

 使用 WITH LOGIN 子句可以将用户重新映射到一个不同的登录名。 不能使用此子句重新映射以下用户：不具有登录名的用户、映射到证书的用户或映射到非对称密钥的用户。 只能重新映射 SQL 用户和 Windows 用户（或组）。 不能使用 WITH LOGIN 子句更改用户类型，例如将 Windows 帐户更改为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。

 如果满足以下条件，则用户的名称会自动重命名为登录名。

- 未指定新名称。

- 当前名称不同于登录名。

 如果不满足上述条件，则不会重命名用户，除非调用方另外调用了 NAME 子句。

被映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名、证书或非对称密钥的用户名不能包含反斜杠字符 (\\)。

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>安全性

> [!NOTE]
> 拥有 ALTER ANY USER 权限的用户可以更改任何用户的默认架构。 更改了架构的用户可能会在不知情的情况下从错误表中选择数据，或者从错误架构中执行代码。

### <a name="permissions"></a>权限

 更改用户名需要具有 ALTER ANY USER 权限。

 更改用户的目标登录名需要对数据库拥有 CONTROL 权限。

 若要更改对数据库拥有 CONTROL 权限的用户名名称，则需要对数据库拥有 CONTROL 权限 。

 更改默认架构或语言需要对用户拥有 ALTER 权限。 用户可更改自己的默认架构或语言。

## <a name="examples"></a>示例

所有示例都在用户数据库中执行。

### <a name="a-changing-the-name-of-a-database-user"></a>A. 更改数据库用户的名称

 以下示例将数据库用户 `Mary5` 的名称更改为 `Mary51`。

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. 更改用户的默认架构
 以下示例将用户 `Mary51` 的默认架构更改为 `Purchasing`。

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>另请参阅

- [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)
- [包含的数据库](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
