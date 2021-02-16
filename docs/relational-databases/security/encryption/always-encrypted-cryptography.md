---
title: Always Encrypted 加密 | Microsoft Docs
description: 了解在 SQL Server 和 Azure SQL 数据库的 Always Encrypted 功能中派生加密材料的加密算法和机制。
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, cryptography system
ms.assetid: ae8226ff-0853-4716-be7b-673ce77dd370
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0da1e6ba00c86bbc391d098ab7cf8b9b218d60e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344387"
---
# <a name="always-encrypted-cryptography"></a>Always Encrypted 加密
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  本文档介绍加密算法和机制，以派生在 [和](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 中 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 始终加密 [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)]功能中使用的加密材料。  
  
## <a name="keys-key-stores-and-key-encryption-algorithms"></a>密钥、密钥存储和密钥加密算法
 Always Encrypted 使用两种类型的密钥：列主密钥和列加密密钥。  
  
 列主密钥 (CMK) 是对密钥进行加密的密钥（例如，用于加密其他密钥的密钥），它始终处于客户端的控制之下，并且存储在外部密钥存储中。 启用了始终加密的客户端驱动程序通过 CMK 存储提供程序与密钥存储进行交互，提供程序可以是驱动程序库（ [!INCLUDE[msCoName](../../../includes/msconame-md.md)]/system 提供程序）或客户端应用程序（自定义提供程序）的一部分。 客户端驱动程序库目前包括 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 证书存储 [的](/windows/desktop/SecCrypto/using-certificate-stores) 密钥存储提供程序和硬件安全模块 (HSM)。 有关提供程序的当前列表信息，请参阅 [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md)。 应用程序开发人员可以为任意存储提供自定义提供程序。  
  
 列加密密钥 (CEK) 是受 CMK 保护的内容加密密钥（例如，用于保护数据的密钥）。  
  
 所有 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] CMK 存储提供程序都通过将 RSA 与最佳非对称加密填充 (RSA-OAEP) 配合使用来加密 CEK。 支持 Microsoft 加密 API 的密钥存储提供程序：.NET Framework 中的下一代加密技术 (CNG) ([SqlColumnEncryptionCngProvider Class](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider)) 使用 A.2.1 节中 RFC 8017 指定的默认参数。 这些默认参数使用 SHA-1 哈希函数和 SHA-1 附带的 MGF1 掩码生成函数。 其他所有密钥存储提供程序都使用 SHA-256。 

Always Encrypted 在内部使用经过 FIPS 140-2 验证的加密模块。 

## <a name="data-encryption-algorithm"></a>数据加密算法  
 始终加密使用 **AEAD_AES_256_CBC_HMAC_SHA_256** 算法来加密数据库中的数据。  
  
 AEAD_AES_256_CBC_HMAC_SHA_256 派生自 [https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05](https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05) 中的规范草案。 它按照 Encrypt-then-MAC 方法，将经验证加密方案配合关联数据使用。 也就是说，首先加密纯文本，然后基于生成的密码文本生成 MAC。  
  
 为了隐藏模式， **AEAD_AES_256_CBC_HMAC_SHA_256** 将使用操作的密码块链 (CBC) 模式，其中初始值送入了名为初始化向量 (IV) 的系统。 可在 [https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf](https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf) 找到 CBC 模式的完整说明。  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** 将使用下列步骤计算给定纯文本值的密码文本值。  
  
### <a name="step-1-generating-the-initialization-vector-iv"></a>步骤 1：生成初始化矢量 (IV)  
 始终加密支持 **AEAD_AES_256_CBC_HMAC_SHA_256** 的两种变体：  
  
-   具有随机性  
  
-   具有确定性  
  
 对于随机加密，将随机生成 IV。 结果是，每次加密相同的纯文本，将生成不同的密码文本，可以防止任何信息泄露。  
  
```  
When using randomized encryption: IV = Generate cryptographicaly random 128bits  
```  
  
 如果有确定性加密，IV 则不会随机生成，而是改为通过使用以下算法从纯文本值派生：  
  
```  
When using deterministic encryption: IV = HMAC-SHA-256( iv_key, cell_data ) truncated to 128 bits.  
```  
  
 其中 iv_key 按以下方式派生自 CEK：  
  
```  
iv_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell IV key" + algorithm + CEK_length)  
```  
  
 为了适应 IV 所需的 1 个数据块，执行的是 HMAC 值截断。
因此，确定加密始终会为给定纯文本值生成相同的已加密文本，从而能够通过比较其对应的已加密文本值推断两个纯文本值是否相等。 有限的信息泄露允许数据库系统支持对加密的列值的相等性比较。  
  
 与其他同类方法相比（如使用预定义 IV 值），确定加密在隐藏模式下更有效。  
  
### <a name="step-2-computing-aes_256_cbc-ciphertext"></a>步骤 2：计算 AES_256_CBC 已加密文本  
 计算 IV 后，将生成 **AES_256_CBC** 已加密文本：  
  
```  
aes_256_cbc_ciphertext = AES-CBC-256(enc_key, IV, cell_data) with PKCS7 padding.  
```  
  
 其中加密密钥 (enc_key) 派生自 CEK，如下所示。  
  
```  
enc_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell encryption key" + algorithm + CEK_length )  
```  
  
### <a name="step-3-computing-mac"></a>步骤 3：计算 MAC  
 随后，使用以下算法计算 MAC：  
  
```  
MAC = HMAC-SHA-256(mac_key, versionbyte + IV + Ciphertext + versionbyte_length)  
```  
  
 其中：  
  
```  
versionbyte = 0x01 and versionbyte_length = 1
mac_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell MAC key" + algorithm + CEK_length)  
```  
  
### <a name="step-4-concatenation"></a>步骤 4：串联  
 最后，串联算法版本字节、MAC、IV 和 AES_256_CBC 已密码文本，以生成加密值：  
  
```  
aead_aes_256_cbc_hmac_sha_256 = versionbyte + MAC + IV + aes_256_cbc_ciphertext  
```  
  
## <a name="ciphertext-length"></a>密码文本长度  
 **AEAD_AES_256_CBC_HMAC_SHA_256** 已加密文本的特定组件的长度是（以字节为单位）：  
  
-   versionbyte：1  
  
-   MAC：32  
  
-   IV：16  
  
-   aes_256_cbc_ciphertext： `(FLOOR (DATALENGTH(cell_data)/ block_size) + 1)* block_size`，其中：  
  
    -   block_size 为 16 字节  
  
    -   cell_data 是纯文本值  
  
     因此，aes_256_cbc_ciphertext 最小大小为 1 个块，即 16 个字节。  
  
 因此，可以使用以下公式计算加密给定纯文本值 (cell_data) 生成的密码文本长度：  
  
```  
1 + 32 + 16 + (FLOOR(DATALENGTH(cell_data)/16) + 1) * 16  
```  
  
 例如：  
  
-   4 个字节长度的 **int** 纯文本值加密后变成 65 字节长度的二进制值。  
  
-   2,000 个字节长度的 **nchar(1000)** 纯文本值加密后变成 2,065 字节长度的二进制值。  
  
 下表包含数据类型和每种类型的密码文本长度的完整列表。  
  
|数据类型|密码文本长度 [字节]|  
|---------------|---------------------------------|  
|**bigint**|65|  
|**binary**|不定。 使用上面的公式。|  
|**bit**|65|  
|**char**|不定。 使用上面的公式。|  
|**date**|65|  
|**datetime**|65|  
|**datetime2**|65|  
|**datetimeoffset**|65|  
|**decimal**|81|  
|**float**|65|  
|**地理**|N/A（不支持）|  
|**geometry**|N/A（不支持）|  
|**hierarchyid**|N/A（不支持）|  
|**图像**|N/A（不支持）|  
|**int**|65|  
|**money**|65|  
|**nchar**|不定。 使用上面的公式。|  
|**ntext**|N/A（不支持）|  
|**numeric**|81|  
|**nvarchar**|不定。 使用上面的公式。|  
|**real**|65|  
|**smalldatetime**|65|  
|**smallint**|65|  
|**smallmoney**|65|  
|**sql_variant**|N/A（不支持）|  
|**sysname**|N/A（不支持）|  
|**text**|N/A（不支持）|  
|**time**|65|  
|**timestamp**<br /><br /> (**rowversion**)|N/A（不支持）|  
|**tinyint**|65|  
|**uniqueidentifier**|81|  
|**varbinary**|不定。 使用上面的公式。|  
|**varchar**|不定。 使用上面的公式。|  
|**xml**|N/A（不支持）|  
  
## <a name="net-reference"></a>.NET 参考  
 有关本文档中讨论的算法的详细信息，请参阅 [.NET 参考](https://referencesource.microsoft.com/)中的 SqlAeadAes256CbcHmac256Algorithm.cs、SqlColumnEncryptionCertificateStoreProvider.cs 和 SqlColumnEncryptionCertificateStoreProvider.cs 文件  。  
  
## <a name="see-also"></a>另请参阅  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 - [使用 Always Encrypted 开发应用程序](../../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
