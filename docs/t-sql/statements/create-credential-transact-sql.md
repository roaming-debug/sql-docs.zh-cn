---
description: CREATE CREDENTIAL (Transact-SQL)
title: CREATE CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREDENTIAL_TSQL
- SQL13.SWB.CREDENTIAL.GENERAL.F1
- CREATE CREDENTIAL
- CREATE_CREDENTIAL_TSQL
- CREDENTIAL
dev_langs:
- TSQL
helpviewer_keywords:
- SECRET clause
- authentication [SQL Server], credentials
- CREATE CREDENTIAL statement
- credentials [SQL Server], CREATE CREDENTIAL statement
ms.assetid: d5e9ae69-41d9-4e46-b13d-404b88a32d9d
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 14230828089399d943eb4332e7c456c6ad68957f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192771"
---
# <a name="create-credential-transact-sql"></a>CREATE CREDENTIAL (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

创建服务器级别的凭据。 凭据是包含连接到 SQL Server 以外的资源时所需的身份验证信息的记录。 多数凭据包括一个 Windows 用户和一个密码。 例如，将数据库备份保存到某个位置可能需要 SQL Server 提供访问该位置的特殊凭据。 有关详细信息，请参阅[凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)。

> [!NOTE]
> 若要创建数据库级别的凭据，请参阅 [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)。 需要为服务器上的多个数据库使用相同凭据时，请使用服务器级别凭据。 使用数据库范围的凭据以使数据库更易于移植。 数据库移动到新服务器时，数据库范围的凭据将随之移动。 使用 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 上的数据库范围的凭据。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
CREATE CREDENTIAL credential_name
WITH IDENTITY = 'identity_name'
    [ , SECRET = 'secret' ]
        [ FOR CRYPTOGRAPHIC PROVIDER cryptographic_provider_name ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

credential_name 指定要创建的凭据的名称。 credential_name 不能以数字符号 (#) 开头。 系统凭据以 ## 开头。 

> [!IMPORTANT]
> 使用共享访问签名 (SAS) 时，该名称必须与容器路径匹配，以 https 开头并且不能包含正斜杠。 请参见[示例 D](#d-creating-a-credential-using-a-sas-token)。

IDENTITY ='identityname' 指定从服务器外部进行连接时要使用的帐户名称 _\__ 。 当凭据用于访问 Azure Key Vault 时，IDENTITY 是该密钥保管库的名称。 请参阅以下示例 C。 凭据使用共享访问签名 (SAS) 时，IDENTITY 是 SHARED ACCESS SIGNATURE。 请参见下面的示例 D。

> [!IMPORTANT]
> Azure SQL 数据库仅支持 Azure Key Vault 和共享访问签名标识。 不支持 Windows 用户标识。

SECRET ='secret' 指定发送身份验证所需的机密内容。

当该凭据用于访问 Azure Key Vault 时，CREATE CREDENTIAL 的 SECRET 参数要求将 \<Client ID>（无连字符）和 Azure Active Directory 中服务主体的 \<Secret> 一起传递，且二者之间不留空格  。 请参阅以下示例 C。 凭据使用共享访问签名时，SECRET 是共享访问签名令牌。 请参见下面的示例 D。 有关如何在 Azure 容器上创建存储访问策略和共享访问签名的信息，请参阅[第 1 课：在 Azure 容器上创建存储访问策略和共享访问签名](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#1---create-stored-access-policy-and-shared-access-storage)。

对于加密提供程序 cryptographic_provider_name，指定企业密钥管理提供程序 (EKM) 的名称 。 有关密钥管理的详细信息，请参阅[可扩展密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。

## <a name="remarks"></a>备注

当 IDENTITY 为 Windows 用户时，机密内容可以是密码。 机密内容使用服务主密钥进行加密。 如果重新生成服务主密钥，则使用新的服务主密钥重新加密机密内容。

创建凭据后，可以使用 [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) 或 [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)，将该凭据映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名只能映射到一个凭据，但是单个凭据可以映射到多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 有关详细信息，请参阅[凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)。 服务器级别凭据只能映射到登录名，不能映射到数据库用户。 

可以在 [sys.credentials](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md) 目录视图中查看有关凭据的信息。

如果该提供程序没有任何登录名映射的凭据，则使用映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户的凭据。

一个登录名可以有多个映射的凭据，只要它们用于不同的提供程序即可。 每个登录名的每个提供程序只能有一个映射的凭据。 相同的凭据可以映射到其他登录名。

## <a name="permissions"></a>权限

需要 ALTER ANY CREDENTIAL 权限。

## <a name="examples"></a>示例

### <a name="a-creating-a-credential-for-windows-identity"></a>A. 创建用于 Windows 标识的凭据

以下示例创建名为 `AlterEgo` 的凭据。 凭据包含 Windows 用户 `Mary5` 和一个密码。

```sql
CREATE CREDENTIAL AlterEgo WITH IDENTITY = 'Mary5',
    SECRET = '<EnterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-credential-for-ekm"></a>B. 创建用于 EKM 的凭据

下面的示例使用一个名为 `User1OnEKM` 的帐户，它是以前通过 EKM 的管理工具在 EKM 模块中创建的，并带有一个基本帐户类型和密码。 服务器上的 sysadmin 帐户创建用于连接到 EKM 帐户的凭据，并将其分配给 `User1` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户：

```sql
CREATE CREDENTIAL CredentialForEKM
    WITH IDENTITY='User1OnEKM', SECRET='<EnterStrongPasswordHere>'
    FOR CRYPTOGRAPHIC PROVIDER MyEKMProvider;
GO

/* Modify the login to assign the cryptographic provider credential */
ALTER LOGIN User1
ADD CREDENTIAL CredentialForEKM;
```

### <a name="c-creating-a-credential-for-ekm-using-the-azure-key-vault"></a>C. 使用 Azure 密钥保管库创建用于 EKM 的凭据

下面的示例使用了用于 Microsoft Azure Key Vault 的 SQL Server 连接器，创建了供 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 访问 Azure Key Vault 时使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据。 有关使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接器的完整示例，请参阅[使用 Azure Key Vault 的可扩展密钥管理 (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)。

> [!IMPORTANT]
> **CREATE CREDENTIAL** 的 **IDENTITY** 参数需要 key vault 名称。 CREATE CREDENTIAL 的 SECRET 参数要求将 \<Client ID>（无连字符）和 \<Secret> 一起传递，且二者之间不留空格  。

 在下例中， **客户端 ID** (`EF5C8E09-4D2A-4A76-9998-D93440D8115D`) 去掉了连字符，并输入为字符串 `EF5C8E094D2A4A769998D93440D8115D` ，而 **Secret** 则表示为字符串 *SECRET_DBEngine*。

```sql
USE master;
CREATE CREDENTIAL Azure_EKM_TDE_cred
    WITH IDENTITY = 'ContosoKeyVault',
    SECRET = 'EF5C8E094D2A4A769998D93440D8115DSECRET_DBEngine'
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov ;
```

下面的示例使用客户端 ID 和 Secret 字符串的变量创建相同的凭据，然后将其连接在一起形成 SECRET 参数  。 REPLACE 函数用于从客户端 ID 中删除连字符。

```sql
DECLARE @AuthClientId uniqueidentifier = 'EF5C8E09-4D2A-4A76-9998-D93440D8115D';
DECLARE @AuthClientSecret varchar(200) = 'SECRET_DBEngine';
DECLARE @pwd varchar(max) = REPLACE(CONVERT(varchar(36), @AuthClientId) , '-', '') + @AuthClientSecret;

EXEC ('CREATE CREDENTIAL Azure_EKM_TDE_cred
    WITH IDENTITY = 'ContosoKeyVault', SECRET = ''' + @PWD + '''
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov ;');
```

### <a name="d-creating-a-credential-using-a-sas-token"></a>D. 使用 SAS 令牌创建凭据

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 至 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)，以及 Azure SQL 托管实例。

下面的示例使用 SAS 令牌创建共享访问签名凭据。 若要详细了解如何在 Azure 容器上创建存储访问策略和共享访问签名，以及使用共享访问签名创建凭据，请参阅[教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)。

> [!IMPORTANT]
> CREDENTIAL NAME 参数需要名称与容器路径匹配，以 https 开头并且末尾不包含正斜杠。 IDENTITY 参数需要名称 SHARED ACCESS SIGNATURE。 SECRET 参数需要共享访问签名令牌。
>
> 共享访问签名密钥不应具有前导值？。

```sql
USE master
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] -- this name must match the container path, start with https and must not contain a trailing forward slash.
    WITH IDENTITY='SHARED ACCESS SIGNATURE' -- this is a mandatory string and do not change it.
    , SECRET = 'sharedaccesssignature' -- this is the shared access signature token
GO
```

### <a name="e-creating-a-credential-for-managed-identity"></a>E. 创建用于托管标识的凭据

以下示例会创建表示 Azure SQL 或 Azure Synapse 服务的托管标识的凭据。 此情况下，不适合使用密码和机密。

```sql
CREATE CREDENTIAL ServiceIdentity WITH IDENTITY = 'Managed Identity';
GO
```

## <a name="see-also"></a>另请参阅

- [凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [ALTER CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-credential-transact-sql.md)
- [DROP CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-credential-transact-sql.md)
- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)
- [sys.credentials (Transact-SQL)](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)
- [第 2 课：使用共享访问签名创建 SQL Server 凭据](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature)
- [共享访问签名](/azure/storage/common/storage-sas-overview)