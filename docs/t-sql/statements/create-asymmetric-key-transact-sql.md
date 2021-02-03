---
description: CREATE ASYMMETRIC KEY (Transact-SQL)
title: CREATE ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASYMMETRIC_KEY_TSQL
- CREATE ASYMMETRIC KEY
- CREATE_ASYMMETRIC_KEY_TSQL
- ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], creating
- encryption [SQL Server], asymmetric keys
- CREATE ASYMMETRIC KEY statement
- asymmetric keys [SQL Server]
- cryptography [SQL Server], asymmetric keys
ms.assetid: 141bc976-7631-49f6-82bd-a235028645b1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8945797d67574e1c2b8ffb60b2d74e7b5b9462f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204993"
---
# <a name="create-asymmetric-key-transact-sql"></a>CREATE ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在数据库中创建非对称密钥。  
  
 此功能与使用数据层应用程序框架 (DACFx) 的数据库导出不兼容。 必须在导出之前删除所有非对称密钥。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE ASYMMETRIC KEY asym_key_name   
   [ AUTHORIZATION database_principal_name ]  
   [ FROM <asym_key_source> ]  
   [ WITH <key_option> ] 
   [ ENCRYPTION BY <encrypting_mechanism> ] 
   [ ; ]
  
<asym_key_source>::=  
     FILE = 'path_to_strong-name_file'  
   | EXECUTABLE FILE = 'path_to_executable_file'  
   | ASSEMBLY assembly_name  
   | PROVIDER provider_name  
  
<key_option> ::=  
   ALGORITHM = <algorithm>  
      |  
   PROVIDER_KEY_NAME = 'key_name_in_provider'  
      |  
      CREATION_DISPOSITION = { CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
      { RSA_4096 | RSA_3072 | RSA_2048 | RSA_1024 | RSA_512 }   
  
<encrypting_mechanism> ::=  
    PASSWORD = 'password'   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *asym_key_name*  
 数据库中非对称密钥的名称。 非对称密钥名称必须遵循[标识符](../../relational-databases/databases/database-identifiers.md)相关规则，并且在数据库中必须唯一。  

 AUTHORIZATION database_principal_name  
 指定非对称密钥的所有者。 所有者不能是角色或组。 如果省略该选项，则所有者为当前用户。  
  
 FROM asym_key_source  
 指定从中加载非对称密钥对的源。  
  
 FILE = 'path_to_strong-name_file'  
 指定从中加载密钥对的强名称文件所在的路径。 由 MAX_PATH 根据 Windows API 限制为 260 个字符。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 EXECUTABLE FILE = 'path_to_executable_file'  
 指定要从中加载公钥的程序集文件的路径。 由 MAX_PATH 根据 Windows API 限制为 260 个字符。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 ASSEMBLY assembly_name  
 指定已加载到数据库（要从中加载公钥）中的已签名程序集的名称。  
  
 PROVIDER provider_name  
 指定可扩展密钥管理 (EKM) 提供程序的名称。 必须先使用 CREATE PROVIDER 语句定义此提供程序。 有关外部密钥管理的详细信息，请参阅[可扩展密钥管理 (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)。  
  
 ALGORITHM = \<algorithm>  
 可以提供五种算法；RSA_4096、RSA_3072、RSA_2048、RSA_1024 和 RSA_512。  
  
 RSA_1024 和 RSA_512 已弃用。 若要使用 RSA_1024 和 RSA_512（不推荐），必须将数据库设置为兼容级别 120 或更低。  
  
 PROVIDER_KEY_NAME = 'key_name_in_provider'  
 指定来自外部提供程序的密钥名称。  
  
 CREATION_DISPOSITION = CREATE_NEW  
 在可扩展的密钥管理设备上创建新密钥。 PROVIDER_KEY_NAME 必须用于指定设备上的密钥名称。 如果密钥已存在于设备上，此语句将失败，并显示错误。  
  
 CREATION_DISPOSITION = OPEN_EXISTING  
 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 非对称密钥映射到现有可扩展的密钥管理密钥。 PROVIDER_KEY_NAME 必须用于指定设备上的密钥名称。 如果未提供 CREATION_DISPOSITION = OPEN_EXISTING，则默认值为 CREATE_NEW。  
  
 ENCRYPTION BY PASSWORD = 'password'  
 指定用于对私钥进行加密的密码。 如果未提供该子句，则使用数据库主密钥对私钥进行加密。 password 最多为 128 个字符。 password 必须符合运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的 Windows 密码策略要求。  
  
## <a name="remarks"></a>备注  
 “非对称密钥”是数据库级的安全对象实体。 该实体的默认格式包含公钥和私钥。 当未使用 FROM 子句执行时，CREATE ASYMMETRIC KEY 会生成新的密钥对。 使用 FROM 子句执行时，CREATE ASYMMETRIC KEY 会从文件中导入密钥对，或者从程序集或 DLL 文件中导入公钥。  
  
 默认情况下，私钥受数据库主密钥保护。 如果尚未创建任何数据库主密钥，则需要使用密码保护私钥。  
  
 私钥的长度可以为 512、1024 或 2048 位。  
  
## <a name="permissions"></a>权限  
 需要对数据库拥有 CREATE ASYMMETRIC KEY 权限。 如果指定了 AUTHORIZATION 子句，则需要对数据库主体具有 IMPERSONATE 权限，或者对应用程序角色具有 ALTER 权限。 只有 Windows 登录名、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和应用程序角色能拥有非对称密钥。 其他组和角色不能拥有非对称密钥。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-an-asymmetric-key"></a>A. 创建非对称密钥  
 下面的示例使用 `RSA_2048` 算法创建名为 `PacificSales09` 的非对称密钥，并使用密码保护私钥。  
  
```sql  
CREATE ASYMMETRIC KEY PacificSales09   
    WITH ALGORITHM = RSA_2048   
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';   
GO  
```  
  
### <a name="b-creating-an-asymmetric-key-from-a-file-giving-authorization-to-a-user"></a>B. 通过文件创建非对称密钥，为用户提供授权  
 以下示例通过文件中存储的密钥对创建非对称密钥 `PacificSales19`，然后将非对称密钥的所有权授予用户 `Christina`。 私钥由数据库主密钥保护，而后者必须在创建非对称密钥之前创建。  
  
```sql  
CREATE ASYMMETRIC KEY PacificSales19  
    AUTHORIZATION Christina  
    FROM FILE = 'c:\PacSales\Managers\ChristinaCerts.tmp';  
GO  
```  
  
### <a name="c-creating-an-asymmetric-key-from-an-ekm-provider"></a>C. 通过 EKM 提供程序创建非对称密钥  
 以下示例通过名为 `EKM_Provider1` 的可扩展密钥管理提供程序中存储的密钥对，或者名为 `key10_user1` 的可扩展密钥管理提供程序上的密钥创建非对称密钥 `EKM_askey1`。  
  
```sql  
CREATE ASYMMETRIC KEY EKM_askey1   
    FROM PROVIDER EKM_Provider1  
    WITH   
        ALGORITHM = RSA_2048,   
        CREATION_DISPOSITION = CREATE_NEW  
        , PROVIDER_KEY_NAME  = 'key10_user1' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER ASYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)  
 [DROP ASYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)  
 [ASYMKEYPROPERTY (Transact-SQL)](../../t-sql/functions/asymkeyproperty-transact-sql.md)  
 [ASYMKEY_ID (Transact-SQL)](../../t-sql/functions/asymkey-id-transact-sql.md)  
 [选择加密算法](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [使用 Azure 密钥保管库的可扩展密钥管理 (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
