---
description: CREATE COLUMN MASTER KEY (Transact-SQL)
title: CREATE COLUMN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL13.SWB.NEWCOLUMNMASTERKEYDEF.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEYDEF.GENERAL.F1
- CREATE COLUMN MASTER KEY
- COLUMN MASTER KEY
- CREATE_COLUMN_MASTER_KEY_TSQL
- COLUMN_MASTER_KEY_TSQL
- SQL13.SWB.NEWCOLUMNMASTERKEY.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEY.GENERAL.F1
dev_langs:
- TSQL
helpviewer_keywords:
- column master key definition
- column master key, create
- CREATE COLUMN MASTER KEY statement
- Always Encrypted, create column master key
ms.assetid: f8926b95-e146-4e3f-b56b-add0c0d0a30e
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 6e18a1e0721428dcbfdb649be46ca83fd0d42972
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204936"
---
# <a name="create-column-master-key-transact-sql"></a>CREATE COLUMN MASTER KEY (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


在数据库中创建列主密钥元数据对象。 列主密钥元数据条目表示存储在外部密钥存储中的密钥。 使用 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) 或[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md) 时，密钥会保护（加密）列加密密钥。 多列主密钥允许定期密钥轮换，以增强安全性。 使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的对象资源管理器或 PowerShell 在密钥存储中创建列主密钥，并在数据库中创建其相关元数据对象。 有关详细信息，请参阅 [Always Encrypted 密钥管理概述](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 

> [!IMPORTANT]
> 创建已启用 enclave 的密钥（带 ENCLAVE_COMPUTATIONS) 需要[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)。

## <a name="syntax"></a>语法  

```syntaxsql
CREATE COLUMN MASTER KEY key_name   
    WITH (  
        KEY_STORE_PROVIDER_NAME = 'key_store_provider_name',  
        KEY_PATH = 'key_path'   
        [,ENCLAVE_COMPUTATIONS (SIGNATURE = signature)]
         )   
[;]  
```  

## <a name="arguments"></a>参数
key_name  
数据库中的列主密钥的名称。  
  
key_store_provider_name  
指定密钥存储提供程序的名称。 密钥存储提供程序是一个客户端软件组件，用于保存包含列主密钥的密钥存储。 

启用了 Always Encrypted 的客户端驱动程序：

- 使用密钥存储提供程序名称 
- 在密钥存储提供程序的驱动程序注册表中搜索密钥存储提供程序 

驱动程序随后使用该提供程序解密列加密密钥。 列加密密钥受列主密钥保护。 列主密匙存储在基础密钥存储中。 随后使用列加密密钥的明文值来加密与加密数据库列相对应的查询参数。 或者，列加密密钥解密来自加密列的查询结果。  
  
启用了 Always Encrypted 的客户端驱动程序库包括热门密钥存储的密钥存储提供程序。   
  
可用的提供程序集取决于客户端驱动程序的类型和版本。 有关特定驱动程序的信息，请参阅 Always Encrypted 文档：[使用 Always Encrypted 开发应用程序](../../relational-databases/security/encryption/always-encrypted-client-development.md)。


下表显示系统提供程序的名称：  
  
|密钥存储提供程序名称|基础密钥存储|  
    |-----------------------------|--------------------------|
    |'MSSQL_CERTIFICATE_STORE'|Windows 证书存储| 
    |'MSSQL_CSP_PROVIDER'|支持 Microsoft CryptoAPI 的存储，如硬件安全模块 (HSM)。|
    |'MSSQL_CNG_STORE'|支持下一代加密技术 API 的存储，如硬件安全模块 (HSM)。|  
    |'AZURE_KEY_VAULT'|请参阅 [Azure Key Vault 入门](/azure/key-vault/general/overview)|  
    |'MSSQL_JAVA_KEYSTORE'| Java 密钥存储。}
  

在启用了 Always Encrypted 的客户端驱动程序中，可以设置自定义密钥存储提供程序，存储没有任何内置密钥存储提供程序的列主密钥。 自定义密钥存储提供程序的名称不能以“MSSQL_”开头，因为它是为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 密钥存储提供程序保留的前缀。 

  
key_path  
列主密钥存储中的密钥路径。 密钥路径必须对于预期要加密或解密数据的每个客户端应用程序都有效。 数据存储在受到引用列主密钥（间接）保护的列中。 客户端应用程序必须具有密钥访问权限。 密钥路径的格式特定于密钥存储提供程序。 以下列表描述了特定的 Microsoft 系统密钥存储提供程序的密钥路径的格式。  
  
-   提供程序名称：MSSQL_CERTIFICATE_STORE  
  
    密钥路径格式：CertificateStoreName/CertificateStoreLocation/CertificateThumbprint   
  
     其中：  
  
    CertificateStoreLocation  
    证书存储位置，必须为当前用户或本地计算机。 有关详细信息，请参阅 [Local Machine and Current User Certificate Stores](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores)（本地计算机和当前用户证书存储）。  
  
    CertificateStore  
    证书存储名称，例如 My。  
  
    CertificateThumbprint  
    证书指纹。  
  
    **示例：**  
  
    ```  
    N'CurrentUser/My/BBF037EC4A133ADCA89FFAEC16CA5BFA8878FB94'  
  
    N'LocalMachine/My/CA5BFA8878FB94BBF037EC4A133ADCA89FFAEC16'  
    ```  
  
-   提供程序名称：MSSQL_CSP_PROVIDER  
  
    密钥路径格式：ProviderName/KeyIdentifier   
  
    其中：  
  
    *ProviderName*  
    列主密钥存储的加密服务提供程序（CSP，用于实现 CAPI）的名称。 如果使用 HSM 作为密钥存储，则提供程序名称必须是 HSM 供应商提供的 CSP 的名称。 提供程序必须安装在客户端计算机上。  
  
    *KeyIdentifier*  
    密钥存储中的密钥标识符，用作列主密钥。  
  
    **示例：**  
  
    ```  
    N'My HSM CSP Provider/AlwaysEncryptedKey1'  
    ```  
  
-   提供程序名称：MSSQL_CNG_STORE  
  
    密钥路径格式：ProviderName/KeyIdentifier   
  
    其中：  
  
    *ProviderName*  
    列主密钥存储的密钥存储提供程序（KSP，用于实现下一代加密技术 (CNG) API）的名称。 如果使用 HSM 作为密钥存储，则提供程序名称必须是 HSM 供应商提供的 KSP 的名称。 提供程序必须安装在客户端计算机上。  
  
    *KeyIdentifier*  
    密钥存储中的密钥标识符，用作列主密钥。  
  
    **示例：**  
  
    ```  
    N'My HSM CNG Provider/AlwaysEncryptedKey1'  
    ```  

-   提供程序名称：AZURE_KEY_STORE  
  
    密钥路径格式：KeyUrl   
  
    其中：  
  
    KeyUrl  
    Azure Key Vault 中密钥的 URL

ENCLAVE_COMPUTATIONS  
指定列主密匙已启用 enclave。 可以与服务器端安全 enclave 共享使用列主密钥加密的所有列加密密钥，并将其用于 enclave 内的计算。 有关详细信息，请参阅[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)。

*签名*  
通过对 *密钥路径* 进行数字签名产生的二进制文本以及包含列主密钥的 ENCLAVE_COMPUTATIONS 设置。 签名反映是否指定了 ENCLAVE_COMPUTATIONS。 该签名可防止未经授权的用户更改签名的值。 启用了 Always Encrypted 的客户端驱动程序会验证签名，如果签名无效，则向应用程序返回错误。 必须使用客户端工具生成签名。 有关详细信息，请参阅[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)。

## <a name="remarks"></a>备注

在数据库中创建列加密密钥元数据条目前，请先创建列主密钥元数据条目，然后才能使用 Always Encrypted 加密数据库中的任何列。 元数据中的列主密钥条目不包含实际列主密钥。 列主密钥必须存储在外部列密钥存储中（SQL Server 外部）。 元数据中的密钥存储提供程序名称和列主密钥路径对于客户端应用程序必须有效。 客户端应用程序需要使用列主密钥来解密列加密密钥。 列加密密钥使用列主密钥进行加密。 客户端应用程序还需要查询加密的列。

建议使用诸如 SQL Server Management Studio (SSMS) 或 PowerShell 等工具来管理列主密钥。 此类工具会生成签名（在使用具有安全 enclave 的 Always Encrypted 的情况下）并自动发出 `CREATE COLUMN MASTER KEY` 语句来创建列加密密钥元数据对象。 请参阅[使用 SQL Server Management Studio 预配 Always Encrypted 密钥](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-ssms.md)和[使用 PowerShell 预配 Always Encrypted 密钥](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md)。 

  
## <a name="permissions"></a>权限  
需要 ALTER ANY COLUMN MASTER KEY 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-column-master-key"></a>A. 创建列主密钥  
以下示例为列主密钥创建列主密钥元数据条目。 对于使用 MSSQL_CERTIFICATE_STORE 提供程序访问列主密钥的客户端应用程序，列主密钥存储在证书存储中：  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
```  
  
为列主密钥创建列主密钥元数据条目。 客户端应用程序使用 MSSQL_CNG_STORE 提供程序访问列主密钥：  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CNG_STORE',    
    KEY_PATH = N'My HSM CNG Provider/AlwaysEncryptedKey'  
);  
```  
  
为列主密钥创建列主密钥元数据条目。 对于使用 AZURE_KEY_VAULT 提供程序访问列主密钥的客户端应用程序，列主密钥存储在 Azure Key Vault 中。  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/  
        MyCMK/4c05f1a41b12488f9cba2ea964b6a700');  
```  
  
为列主密钥创建列主密钥元数据条目。 列主密钥存储在自定义列主密钥存储中：  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = 'CUSTOM_KEY_STORE',    
    KEY_PATH = 'https://contoso.vault/sales_db_tce_key'  
);  
```  
### <a name="b-creating-an-enclave-enabled-column-master-key"></a>B. 创建已启用 enclave 的列主密钥  
以下示例为已启用 enclave 的列主密钥创建列主密钥元数据条目。 对于使用 MSSQL_CERTIFICATE_STORE 提供程序访问列主密钥的客户端应用程序，已启用 enclave 的列主密钥存储在证书存储中：  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
     ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020542419990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );  
```  
  
为已启用 enclave 的列主密匙创建列主密钥元数据条目。 对于使用 AZURE_KEY_VAULT 提供程序访问列主密钥的客户端应用程序，已启用 enclave 的列主密钥存储在 Azure Key Vault 中。  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/MyCMK/4c05f1a41b12488f9cba2ea964b6a700');
    ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020582413990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );
```  
  
## <a name="see-also"></a>另请参阅
 
* [DROP COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/drop-column-master-key-transact-sql.md)   
* [创建列加密密钥 (Transact-SQL)](../../t-sql/statements/create-column-encryption-key-transact-sql.md)
* [sys.column_master_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
* [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
* [具有安全 Enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)   
* [Always Encrypted 密钥管理概述](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
* [管理具有安全 enclave 的 Always Encrypted 的密钥](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
