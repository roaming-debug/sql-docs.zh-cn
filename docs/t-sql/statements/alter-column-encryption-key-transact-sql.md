---
description: ALTER COLUMN ENCRYPTION KEY (Transact-SQL)
title: ALTER COLUMN ENCRYPTION KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER COLUMN ENCRYPTION
- ALTER_COLUMN_ENCRYPTION_TSQL
- ALTER COLUMN ENCRYPTION KEY
- ALTER_COLUMN_ENCRYPTION_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column encryption key, alter
- ALTER COLUMN ENCRYPTION KEY statement
ms.assetid: c79a220d-e178-4091-a330-c924cc0f0ae0
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: b9aaf24adad7cc1451b32173bedefc30fbff60eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191051"
---
# <a name="alter-column-encryption-key-transact-sql"></a>ALTER COLUMN ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  在数据库中更改列加密密钥，添加或删除加密的值。 列加密密钥最多可具有两个值，这样使相应的列主密钥能够进行轮换。 使用 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) 或[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md) 加密列时，会使用列加密密钥。 添加列加密密钥值之前，必须使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [CREATE MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) 语句定义用于加密该值的列主密钥。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER COLUMN ENCRYPTION KEY key_name   
    [ ADD | DROP ] VALUE   
    (  
        COLUMN_MASTER_KEY = column_master_key_name   
        [, ALGORITHM = 'algorithm_name' , ENCRYPTED_VALUE =  varbinary_literal ]   
    ) [;]  
```  

## <a name="arguments"></a>参数
 key_name  
 要更改的列加密密钥。  
  
 column_master_key_name  
 指定用于对列加密密钥 (CEK) 进行加密的列主密钥 (CMK) 的名称。  
  
 algorithm_name  
 用于对值进行加密的加密算法的名称。 系统提供程序的算法必须为 RSA_OAEP。 删除列加密密钥值时，此参数无效。  
  
 varbinary_literal  
 使用指定的主加密密钥加密的 CEK BLOB。 删除列加密密钥值时，此参数无效。  
  
> [!WARNING]  
>  切勿在此语句中传递纯文本 CEK 值。 这样做不利于发挥此功能的优点。  

## <a name="remarks"></a>备注
通常情况下，创建列加密密钥时，密钥只具有一个加密值。 列主密钥需要进行轮换（需要将当前的列主密钥替换为新的列主密钥）时，可以为列加密密钥添加一个新值，并使用新的列主密钥进行加密。 此工作流可以确保客户端应用程序能访问使用列加密密钥加密的数据，同时客户端应用程序将能使用新的列主密钥。 通过 Always Encrypted 功能，无权访问新主密钥的客户端应用程序中的驱动程序将能够通过列加密密钥值（使用旧的列主密钥进行加密）来访问敏感数据。 Always Encrypted 支持的加密算法要求纯文本值具有 256 位。 
 
建议使用诸如 SQL Server Management Studio (SSMS) 或 PowerShell 等工具来轮换列主密钥。 请参阅[使用 SQL Server Management Studio 轮换 Always Encrypted 密钥](../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-ssms.md)和[使用 PowerShell 轮换 Always Encrypted 密钥](../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)。

应使用密钥存储提供程序生成加密值，该提供程序封装了存储有列主密钥的密钥存储。  

 轮换列主密钥，原因如下：
- 法规遵从性要求可能要求定期轮换密钥。
- 列主密匙遭到破坏，出于安全考虑，需对其进行轮换。
- 若要启用或禁用与服务器端上的安全 enclave 共享列加密密钥。 例如，如果当前列主密钥不支持 enclave 计算（尚未使用 ENCLAVE_COMPUTATIONS 属性定义）并且你想要在使用列主密钥加密的列加密密钥进行保护的列上启用 enclave 计算，需要将列主密钥替换为具有 ENCLAVE_COMPUTATIONS 属性的新密钥。 [Always Encrypted 密钥管理概述](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)和[管理具有安全 enclave 的 Always Encrypted 的密钥](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)。

可使用 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)、[sys.column_encryption_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) 和 [sys.column_encryption_key_values (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md) 查看有关列加密密钥的相关信息。  
  
## <a name="permissions"></a>权限  
 需要对数据库具有 ALTER ANY COLUMN ENCRYPTION KEY 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-adding-a-column-encryption-key-value"></a>A. 添加列加密密钥值  
 下面的示例更改名为 `MyCEK` 的列加密密钥。  
  
```sql  
ALTER COLUMN ENCRYPTION KEY MyCEK  
ADD VALUE  
(  
    COLUMN_MASTER_KEY = MyCMK2,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E00740075007300650072002F006D0079002F0064006500650063006200660034006100340031003000380034006200350033003200360066003200630062006200350030003600380065003900620061003000320030003600610037003800310066001DDA6134C3B73A90D349C8905782DD819B428162CF5B051639BA46EC69A7C8C8F81591A92C395711493B25DCBCCC57836E5B9F17A0713E840721D098F3F8E023ABCDFE2F6D8CC4339FC8F88630ED9EBADA5CA8EEAFA84164C1095B12AE161EABC1DF778C07F07D413AF1ED900F578FC00894BEE705EAC60F4A5090BBE09885D2EFE1C915F7B4C581D9CE3FDAB78ACF4829F85752E9FC985DEB8773889EE4A1945BD554724803A6F5DC0A2CD5EFE001ABED8D61E8449E4FAA9E4DD392DA8D292ECC6EB149E843E395CDE0F98D04940A28C4B05F747149B34A0BAEC04FFF3E304C84AF1FF81225E615B5F94E334378A0A888EF88F4E79F66CB377E3C21964AACB5049C08435FE84EEEF39D20A665C17E04898914A85B3DE23D56575EBC682D154F4F15C37723E04974DB370180A9A579BC84F6BC9B5E7C223E5CBEE721E57EE07EFDCC0A3257BBEBF9ADFFB00DBF7EF682EC1C4C47451438F90B4CF8DA709940F72CFDC91C6EB4E37B4ED7E2385B1FF71B28A1D2669FBEB18EA89F9D391D2FDDEA0ED362E6A591AC64EF4AE31CA8766C259ECB77D01A7F5C36B8418F91C1BEADDD4491C80F0016B66421B4B788C55127135DA2FA625FB7FD195FB40D90A6C67328602ECAF3EC4F5894BFD84A99EB4753BE0D22E0D4DE6A0ADFEDC80EB1B556749B4A8AD00E73B329C95827AB91C0256347E85E3C5FD6726D0E1FE82C925D3DF4A9  
);  
GO  
  
```  
  
### <a name="b-dropping-a-column-encryption-key-value"></a>B. 删除列加密密钥值  
 下面的示例删除了一个值，对名为 `MyCEK` 的列加密密钥进行了更改。  
  
```sql  
ALTER COLUMN ENCRYPTION KEY MyCEK  
DROP VALUE  
(  
    COLUMN_MASTER_KEY = MyCMK  
);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
 [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/drop-column-encryption-key-transact-sql.md)   
 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/create-column-master-key-transact-sql.md)   
 [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [sys.column_encryption_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)   
 [sys.column_encryption_key_values (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Always Encrypted 密钥管理概述](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
 [管理具有安全 enclave 的 Always Encrypted 的密钥](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
  
  
