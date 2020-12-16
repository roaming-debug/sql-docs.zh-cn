---
description: CREATE USER (Transact-SQL)
title: CREATE USER (Transact-SQL)
ms.custom: ''
ms.date: 11/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WITHOUT_LOGIN_TSQL
- CREATE_USER_TSQL
- SQL13.SWB.DATABASEUSER.OWNEDSCHEMAS.F1
- WITHOUT LOGIN
- CREATE USER
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- adding users
- WITHOUT LOGIN [SQL Server]
- CREATE USER statement
- database user additions [SQL Server]
- USER WITHOUT LOGIN [SQL Server]
- users [SQL Server], adding
- users [SQL Server]
ms.assetid: 01de7476-4b25-4d58-85b7-1118fe64aa80
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c88b7acdabad8ff50b15a1dc840b9acf48e91d89
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464118"
---
# <a name="create-user-transact-sql"></a>CREATE USER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  向当前数据库添加用户。 下面列出了 12 种类型的用户，并给出了最基本的语法示例：  
  
**基于 master 数据库中登录名的用户** - 这是最常见的一种用户类型。  
  
-   基于 Windows Active Directory 帐户的登录名的用户。 `CREATE USER [Contoso\Fritz];`     
-   基于 Windows 组的登录名的用户。 `CREATE USER [Contoso\Sales];`   
-   基于使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录名的用户。 `CREATE USER Mary;`  
  
**在数据库中进行身份验证的用户** - 建议用来帮助提高数据库的可移植性。  
 始终可用于 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。 在 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 中，只能用于包含的数据库。  
  
-   基于无登录名的 Windows 用户的用户。 `CREATE USER [Contoso\Fritz];`    
-   基于无登录名的 Windows 组的用户。 `CREATE USER [Contoso\Sales];`  
-   基于 Azure Active Directory 用户的 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 或 [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] 中的用户。 `CREATE USER [Fritz@contoso.com] FROM EXTERNAL PROVIDER;`     

-   拥有密码的包含数据库用户。 （不可用于 [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]。）`CREATE USER Mary WITH PASSWORD = '********';`   
  
**基于通过 Windows 组登录名连接的 Windows 主体的用户**  
  
-   基于无登录名但可通过 Windows 组中的成员身份连接到[!INCLUDE[ssDE](../../includes/ssde-md.md)]的 Windows 用户的用户。 `CREATE USER [Contoso\Fritz];`  
  
-   基于无登录名但可通过其他 Windows 组中的成员身份连接到[!INCLUDE[ssDE](../../includes/ssde-md.md)]的 Windows 组的用户。 `CREATE USER [Contoso\Fritz];`  
  
**无法进行身份验证的用户** - 这些用户无法登录 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]。  
  
-   没有登录名的用户。 不能登录，但可以被授予权限。 `CREATE USER CustomApp WITHOUT LOGIN;`    
-   基于证书的用户。 不能登录，但可以被授予权限，也可以对模块进行签名。 `CREATE USER TestProcess FOR CERTIFICATE CarnationProduction50;`  
-   基于非对称密钥的用户。 不能登录，但可以被授予权限，也可以对模块进行签名。 `CREATE User TestProcess FROM ASYMMETRIC KEY PacificSales09;`   
 
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure SQL Managed Instance
  
-- Syntax Users based on logins in master  
CREATE USER user_name   
    [   
        { FOR | FROM } LOGIN login_name   
    ]  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
-- Users that authenticate at the database  
CREATE USER   
    {  
      windows_principal [ WITH <options_list> [ ,... ] ]  
  
    | user_name WITH PASSWORD = 'password' [ , <options_list> [ ,... ]   
    | Azure_Active_Directory_principal FROM EXTERNAL PROVIDER   
    }  
  
 [ ; ]  
  
-- Users based on Windows principals that connect through Windows group logins  
CREATE USER   
    {   
          windows_principal [ { FOR | FROM } LOGIN windows_principal ]  
        | user_name { FOR | FROM } LOGIN windows_principal  
}  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
-- Users that cannot authenticate   
CREATE USER user_name   
    {  
         WITHOUT LOGIN [ WITH <limited_options_list> [ ,... ] ]  
       | { FOR | FROM } CERTIFICATE cert_name   
       | { FOR | FROM } ASYMMETRIC KEY asym_key_name   
    }  
 [ ; ]  
  
<options_list> ::=  
      DEFAULT_SCHEMA = schema_name  
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }  
    | SID = sid   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name ]   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
-- SQL Database syntax when connected to a federation member  
CREATE USER user_name  
[;]

-- Syntax for users based on Azure AD logins for Azure SQL Managed Instance
CREATE USER user_name   
    [   { FOR | FROM } LOGIN login_name  ]  
    | FROM EXTERNAL PROVIDER
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  

<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name 
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ] 
```

> [!NOTE]
> 在创建后，Azure SQL 托管实例功能的 Azure AD 管理员已更改。 有关详细信息，请参阅[适用于 MI 的新 Azure AD 管理员功能](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi)。

```syntaxsql
-- Syntax for Azure Synapse Analytics  
  
CREATE USER user_name   
    [ { { FOR | FROM } { LOGIN login_name }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]

CREATE USER Azure_Active_Directory_principal FROM EXTERNAL PROVIDER  
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]
``` 
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CREATE USER user_name   
    [ { { FOR | FROM }  
      {   
        LOGIN login_name   
      }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 user_name  
 指定在此数据库中用于识别该用户的名称。 *user_name* 为 **sysname**。 它的长度最多是 128 个字符。 在创建基于 Windows 主体的用户时，除非指定其他用户名，否则 Windows 主体名称将成为用户名。  
  
 LOGIN *login_name*  
 指定要为其创建数据库用户的登录名。 *login_name* 必须是服务器中的有效登录名。 可以是基于 Windows 主体（用户或组）的登录名，也可以是使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录名。 当此 SQL Server 登录名进入数据库时，它将获取正在创建的这个数据库用户的名称和 ID。 在创建从 Windows 主体映射的登录名时，请使用格式 [\<domainName\>\\\<loginName\>]。 有关示例，请参阅[语法摘要](#SyntaxSummary)。  
  
 如果 CREATE USER 语句是 SQL 批处理中唯一的语句，则 Azure SQL 数据库将支持 WITH LOGIN 子句。 如果 CREATE USER 语句不是 SQL 批处理中唯一的语句或在动态 SQL 中执行，则不支持 WITH LOGIN 子句。  
  
 WITH DEFAULT_SCHEMA = schema_name  
 指定服务器为此数据库用户解析对象名时将搜索的第一个架构。  
  
 '*windows_principal*'  
 指定正为其创建数据库用户的 Windows 主体。 *windows_principal* 可以是 Windows 用户或 Windows 组。 即使 *windows_principal* 没有登录名，也会创建该用户。 连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，如果 *windows_principal* 没有登录名，Windows 主体必须通过有登录名的 Windows 组中的成员身份在[!INCLUDE[ssDE](../../includes/ssde-md.md)]中进行身份验证，或者连接字符串必须将包含的数据库指定为初始目录。 在从 Windows 主体创建用户时，请使用格式 [\<domainName\>\\\<loginName\>]。 有关示例，请参阅[语法摘要](#SyntaxSummary)。 基于 Active Directory 用户的用户的名称限制为少于 21 个字符。
  
 '*Azure_Active_Directory_principal*'  
 适用范围：[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]、[!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]。  
  
 指定正为其创建数据库用户的 Azure Active Directory 主体。 Azure_Active_Directory_principal 可以是 Azure Active Directory 用户、Azure Active Directory 组或 Azure Active Directory 应用程序。 （Azure Active Directory 用户不能在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中拥有 Windows 身份验证登录名；只有数据库用户才能拥有。）连接字符串必须将包含的数据库指定为初始目录。

 对于 Azure AD 主体，CREATE USER 语法需要：

- Azure AD 用户的 Azure AD 对象的 UserPrincipalName。

  - `CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;`  
  - `CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;`

- Azure AD 组和 Azure AD 应用程序的 Azure AD 对象的 DisplayName。 如果有 Nurses 安全组，可使用：  
  
  - `CREATE USER [Nurses] FROM EXTERNAL PROVIDER;`  
  
 有关详细信息，请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/azure-sql/database/authentication-aad-overview)。  
  
WITH PASSWORD = 'password'  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 只能在包含数据库中使用。 为正在创建的用户指定密码。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，存储的密码信息使用 SHA-512 加盐密码进行计算。  
  
WITHOUT LOGIN  
 指定不应将用户映射到现有登录名。  
  
CERTIFICATE cert_name  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 指定要为其创建数据库用户的证书。  
  
ASYMMETRIC KEY asym_key_name  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 指定要为其创建数据库用户的非对称密钥。  
  
DEFAULT_LANGUAGE = *{ NONE \| \<lcid> \| \<language name> \| \<language salias> }*  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 为新用户指定默认语言。 如果为用户指定了默认语言并在之后更改数据库的默认语言，则用户的默认语言仍会保留为指定的语言。 如果未指定默认语言，用户的默认语言将为数据库的默认语言。 如果未指定用户的默认语言并在之后更改数据库的默认语言，用户的默认语言将更改为数据库的新默认语言。  
  
> [!IMPORTANT]  
>  *DEFAULT_LANGUAGE* 仅用于包含的数据库用户。  
  
SID = sid  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 仅适用于包含的数据库中具有密码的用户（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证）。 指定新数据库用户的 SID。 如果未选择此选项，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动指派 SID。 使用 SID 参数在具有同一标识 (SID) 的多个数据库中创建用户。 当在多个数据库中创建用户以准备进行 Always On 故障转移时，这非常有用。 若要确定用户的 SID，请查询 sys.database_principals。  
  
ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  
 **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS](../../includes/sssds-md.md)]。  
  
 取消在大容量复制操作期间对服务器进行加密元数据检查。 这使用户能够在表或数据库之间大容量复制加密数据，而无需对数据进行解密。 默认为 OFF。  
  
> [!WARNING]  
>  错误使用此选项可能导致数据损坏。 有关详细信息，请参阅[迁移通过 Always Encrypted 保护的敏感数据](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md)。  
  
## <a name="remarks"></a>备注  
 如果已忽略 FOR LOGIN，则新的数据库用户将被映射到同名的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 默认架构将是服务器为此数据库用户解析对象名时将搜索的第一个架构。 除非另外指定，否则默认架构将是此数据库用户创建的对象所属的架构。  
  
 如果用户具有默认架构，则将使用默认架构。 如果用户不具有默认架构，但该用户是具有默认架构的组的成员，则将使用该组的默认架构。 如果用户不具有默认架构而且是多个组的成员，则该用户的默认架构将是具有最低 principle_id 的 Windows 组的架构和一个显式设置的默认架构。 （不可能将可用的默认架构之一显式选作首选架构。）如果不能为用户确定默认架构，则将使用 dbo 架构。  
  
 DEFAULT_SCHEMA 可在创建它所指向的架构前进行设置。  
  
 在创建映射到证书或非对称密钥的用户时，不能指定 DEFAULT_SCHEMA。  
  
 如果用户是 sysadmin 固定服务器角色的成员，则忽略 DEFAULT_SCHEMA 的值。 sysadmin 固定服务器角色的所有成员都有默认架构 `dbo`。  
  
 WITHOUT LOGIN 子句可创建不映射到 SQL Server 登录名的用户。 它可以作为 guest 连接到其他数据库。 可以将权限分配给这一没有登录名的用户，当安全上下文更改为没有登录名的用户时，原始用户将收到无登录名用户的权限。 请参阅示例 [D. 创建和使用不含登录名的用户](#withoutLogin)。  
  
 只有映射到 Windows 主体的用户才能包含反斜杠字符 ( **\\** )。
  
 不能使用 CREATE USER 创建 guest 用户，因为每个数据库中均已存在 guest 用户。 可通过授予 guest 用户 CONNECT 权限来启用该用户，如下所示：  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
 可以在 [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) 目录视图中查看有关数据库用户的信息。

新的语法扩展 FROM EXTERNAL PROVIDER 可用于在 SQL 托管实例中创建服务器级别的 Azure AD 登录名。 使用 Azure AD 登录名，数据库级别的 Azure AD 主体能够映射到服务器级别的 Azure AD 登录名。 若要通过 Azure AD 登录名创建 Azure AD 用户，请使用以下语法：

`CREATE USER [AAD_principal] FROM LOGIN [Azure AD login]`

在 SQL 托管实例数据库中创建用户时，login_name 必须与现有的 Azure AD 登录名相对应，否则使用 FROM EXTERNAL PROVIDER 子句将只能在 master 数据库中创建没有登录名的 Azure AD 用户。 例如，以下命令将创建容器用户：

`CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER`
  
##  <a name="syntax-summary"></a><a name="SyntaxSummary"></a> 语法摘要  
 **基于 master 数据库中登录名的用户**  
  
 下面的列表显示基于登录名的用户的可能语法。 未列出默认架构选项。  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FOR LOGIN SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FROM LOGIN SQLAUTHLOGIN`  
  
**在数据库中进行身份验证的用户**  
  
 下面的列表显示只能在包含数据库中使用的用户的可能语法。 创建的用户将不与 **master** 数据库中的任何登录名相关。 未列出默认架构和语言选项。  
  
> [!IMPORTANT]  
>  此语法授予用户对数据库的访问权限，并且还将授予对[!INCLUDE[ssDE](../../includes/ssde-md.md)]的新访问权限。  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER Barry WITH PASSWORD = 'sdjklalie8rew8337!$d'`  
  
**基于在 master 数据库中无登录名的 Windows 主体的用户**  
  
 下面的列表显示可通过 Windows 组访问[!INCLUDE[ssDE](../../includes/ssde-md.md)]但在 **master** 数据库中没有登录名的用户的可能语法。 此语法可用于所有类型的数据库中。 未列出默认架构和语言选项。  
  
 此语法与基于 master 数据库中登录名的用户相似，但此用户类别在 master 中没有登录名。 该用户必须可以通过 Windows 组登录名访问[!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
 此语法类似于基于 Windows 主体的包含数据库用户，但此用户类别未获得对[!INCLUDE[ssDE](../../includes/ssde-md.md)]的新访问权限。  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
  
**不能进行身份验证的用户**  
  
 下面的列表显示无法登录 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的用户的可能语法。  
  
-   `CREATE USER RIGHTSHOLDER WITHOUT LOGIN`  
-   `CREATE USER CERTUSER FOR CERTIFICATE SpecialCert`  
-   `CREATE USER CERTUSER FROM CERTIFICATE SpecialCert`  
-   `CREATE USER KEYUSER FOR ASYMMETRIC KEY SecureKey`  
-   `CREATE USER KEYUSER FROM ASYMMETRIC KEY SecureKey`  
  
## <a name="security"></a>安全性  
 创建用户会授予对数据库的访问权限，但不会自动授予对数据库中对象的任何访问权限。 创建用户后，常见操作是将用户添加到有权访问数据库对象的数据库角色中或向用户授予对象权限。 有关设计权限系统的信息，请参阅 [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。  
  
### <a name="special-considerations-for-contained-databases"></a>包含数据库的特殊注意事项  
 连接包含的数据库时，如果用户在 **master** 数据库中没有登录名，连接字符串必须包括包含的数据库名称作为初始目录。 拥有密码的包含数据库用户始终需要使用初始目录参数。  
  
 在包含数据库中，创建用户有助于将数据库与[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例分离，以便可以轻松地将数据库移动到其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中。 有关详细信息，请参阅[包含的数据库](../../relational-databases/databases/contained-databases.md)和[包含的数据库用户 - 使你的数据库可移植](../../relational-databases/security/contained-database-users-making-your-database-portable.md)。 若要将数据库用户从基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名的用户更改为拥有密码的包含数据库用户，请参阅 [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)。  
  
 在包含的数据库中，用户不必在 **master** 数据库中具有登录名。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]管理员应该了解，可在数据库级别而非[!INCLUDE[ssDE](../../includes/ssde-md.md)]级别授予对包含数据库的访问权限。 有关详细信息，请参阅 [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md)。  
  
 对 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 使用包含的数据库用户时，使用数据库级别防火墙规则（而不服务器级别防火墙规则）配置访问权限。 有关详细信息，请参阅 [sp_set_database_firewall_rule（Azure SQL 数据库）](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)。
 
对于 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]和 [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]的包含数据库用户，SSMS 可以支持多重身份验证。 有关详细信息，请参阅 [SQL 数据库和 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 针对 Azure AD MFA 的 SSMS 支持](https://azure.microsoft.com/documentation/articles/sql-database-ssms-mfa-authentication/)。  
  
### <a name="permissions"></a>权限  
 需要对数据库具有 ALTER ANY USER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-database-user-based-on-a-sql-server-login"></a>A. 基于 SQL Server 登录名创建数据库用户  
 下面的示例首先创建一个名为 `AbolrousHazem` 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，然后在 `AbolrousHazem` 中创建对应的数据库用户 `AdventureWorks2012`。  
  
```sql  
CREATE LOGIN AbolrousHazem   
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
```   
更改为用户数据库。 例如，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中使用 `USE AdventureWorks2012` 语句。 在 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]和[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]中，必须建立到用户数据库的新连接。

```sql   
CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
GO   
```  
  
### <a name="b-creating-a-database-user-with-a-default-schema"></a>B. 创建具有默认架构的数据库用户  
 下面的示例首先创建名为 `WanidaBenshoof` 且具有密码的服务器登录名，然后创建具有默认架构 `Wanida` 的对应数据库用户 `Marketing`。  
  
```sql  
CREATE LOGIN WanidaBenshoof   
    WITH PASSWORD = '8fdKJl3$nlNv3049jsKK';  
USE AdventureWorks2012;  
CREATE USER Wanida FOR LOGIN WanidaBenshoof   
    WITH DEFAULT_SCHEMA = Marketing;  
GO  
```  
  
### <a name="c-creating-a-database-user-from-a-certificate"></a>C. 从证书创建数据库用户  
 下面的示例从证书 `JinghaoLiu` 创建数据库用户 `CarnationProduction50`。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql  
USE AdventureWorks2012;  
CREATE CERTIFICATE CarnationProduction50  
    WITH SUBJECT = 'Carnation Production Facility Supervisors',  
    EXPIRY_DATE = '11/11/2011';  
GO  
CREATE USER JinghaoLiu FOR CERTIFICATE CarnationProduction50;  
GO   
```  
  
###  <a name="d-creating-and-using-a-user-without-a-login"></a><a name="withoutLogin"></a> D. 创建和使用不含登录名的用户  
 以下示例创建一个数据库用户 `CustomApp`，该用户不映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 然后，该示例向用户 `adventure-works\tengiz0` 授予相应的权限以便模拟 `CustomApp` 用户。  
  
```sql  
USE AdventureWorks2012 ;  
CREATE USER CustomApp WITHOUT LOGIN ;  
GRANT IMPERSONATE ON USER::CustomApp TO [adventure-works\tengiz0] ;  
GO   
```  
  
 为了使用 `CustomApp` 凭据，用户 `adventure-works\tengiz0` 执行以下语句。  
  
```sql  
EXECUTE AS USER = 'CustomApp' ;  
GO  
```  
  
 为了恢复到 `adventure-works\tengiz0` 凭据，该用户执行以下语句。  
  
```sql  
REVERT ;  
GO  
```  
  
### <a name="e-creating-a-contained-database-user-with-password"></a>E. 创建拥有密码的包含数据库用户  
 下面的示例创建一个拥有密码的包含数据库用户。 该示例只能在包含数据库中执行。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。 如果删除了 DEFAULT_LANGUAGE，则此示例可在 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 中正常运行。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER Carlo  
WITH PASSWORD='RN92piTCh%$!~3K9844 Bl*'  
    , DEFAULT_LANGUAGE=[Brazilian]  
    , DEFAULT_SCHEMA=[dbo]  
GO   
```  
  
### <a name="f-creating-a-contained-database-user-for-a-domain-login"></a>F. 为域登录名创建包含数据库用户  
 下面的示例为 Contoso 域中名为 Fritz 的登录名创建一个包含数据库用户。 该示例只能在包含数据库中执行。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER [Contoso\Fritz] ;  
GO   
```  
  
### <a name="g-creating-a-contained-database-user-with-a-specific-sid"></a>G. 创建具有特定 SID 的包含数据库用户  
 下面的示例创建名为 CarmenW 的 SQL Server 经过身份验证的包含数据库用户。 该示例只能在包含数据库中执行。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER CarmenW WITH PASSWORD = 'a8ea v*(Rd##+'  
, SID = 0x01050000000000090300000063FF0451A9E7664BA705B10E37DDC4B7;
```  
  
### <a name="h-creating-a-user-to-copy-encrypted-data"></a>H. 创建用户以复制加密数据  
 以下示例会创建一个用户，该用户可以将受 Always Encrypted 功能保护的数据从一组包含加密列的表复制到另一组具有加密列的表中（在相同或不同的数据库中）。  有关详细信息，请参阅[迁移通过 Always Encrypted 保护的敏感数据](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md)。  
  
**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS](../../includes/sssds-md.md)]。  
  
```sql  
CREATE USER [Chin]   
WITH   
      DEFAULT_SCHEMA = dbo  
    , ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = ON ;  
```

### <a name="i-create-an-azure-ad-user-from-an-azure-ad-login-in-sql-managed-instance"></a>I. 在 SQL 托管实例中通过 Azure AD 登录名创建 Azure AD 用户

 若要通过 Azure AD 登录名创建 Azure AD 用户，请使用以下语法。

 使用授予 `sysadmin` 角色的 Azure AD 登录名登录托管实例。 以下代码可通过登录名 bob@contoso.com 创建 Azure AD 用户 bob@contoso.com。 此登录名是在 [CREATE LOGIN](./create-login-transact-sql.md#examples) 示例中创建的。

```sql
CREATE USER [bob@contoso.com] FROM LOGIN [bob@contoso.com];
GO
```

> [!IMPORTANT]
> 通过 Azure AD 登录名创建 USER 时，请指定 user_name 作为 LOGIN 的相同 login_name 。

支持创建 Azure AD 用户，作为属于组的 Azure AD 登录名中的组。

```sql
CREATE USER [AAD group] FROM LOGIN [AAD group];
GO
```

此外，还可通过属于组的 Azure AD 登录名创建 Azure AD 用户。

```sql
CREATE USER [bob@contoso.com] FROM LOGIN [AAD group];
GO
```

### <a name="j-create-an-azure-ad-user-without-an-aad-login-for-the-database"></a>J. 为数据库创建没有 AAD 登录名的 Azure AD 用户

可使用以下语法在 SQL 托管实例数据库中创建 Azure AD 用户 bob@contoso.com（包含的用户）：

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
GO
```

## <a name="next-steps"></a>后续步骤  
创建用户后，便可考虑使用 [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md) 语句将用户添加到某个数据库角色。  
你可能还想对该角色 [GRANT 对象权限](../../t-sql/statements/grant-object-permissions-transact-sql.md)，以便它能访问表。 有关 SQL Server 安全模型的常规信息，请参阅[权限](../../relational-databases/security/permissions-database-engine.md)。   
  
## <a name="see-also"></a>另请参阅  
 [创建数据库用户](../../relational-databases/security/authentication-access/create-a-database-user.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [ALTER USER (Transact-SQL)](../../t-sql/statements/alter-user-transact-sql.md)   
 [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [包含的数据库](../../relational-databases/databases/contained-databases.md)   
 [使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/azure-sql/database/authentication-aad-overview)   
 [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)