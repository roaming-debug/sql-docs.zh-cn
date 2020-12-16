---
title: SQL Server 证书和非对称密钥 | Microsoft Docs
description: 了解 SQL Server 中的证书和非对称密钥，包括外部生成的或 SQL Server 生成的证书、工具及相关任务。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- security [SQL Server], certificates and asymmetric keys
ms.assetid: 8519aa2f-f09c-4c1c-96b5-abc24811e60c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9120af44ce0f1cb8447b34e47a9ce54b9ba17367
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97431885"
---
# <a name="sql-server-certificates-and-asymmetric-keys"></a>SQL Server 证书和非对称密钥
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 公钥加密是一种消息保密方式，在使用这种方式时用户将创建一个“公钥”和一个“私钥” 。 私钥是保密的，而公钥可以分发给其他人。 虽然密钥之间具有数学关系，但要想通过公钥推导出私钥却并不容易。 可使用公钥加密只能通过相应私钥解密的数据。 这可用于加密发送给私钥所有者的消息。 同样，私钥所有者可以加密只能通过公钥解密的数据。 这种用法构成了数字证书的基础，其中证书所含信息由私钥所有者（假定为内容作者）进行加密。 由于加密和解密密钥不同，因此称之为“非对称密钥”。
  
 证书和非对称密钥都属于非对称加密的使用方式。 证书通常用作非对称密钥的容器，因为它们可以包含更多信息，例如过期日期和颁发者。 这两种机制的加密算法之间存在差异，但相同密钥长度的加密强度是相同的。 通常，可以使用证书来加密数据库中其他类型的加密密钥，或者为代码模块签名。  
  
 证书和非对称密钥可以解密其他人加密的数据。 通常，可以使用非对称加密来加密存储在数据库中的对称密钥。  
  
 公钥不像证书那样具有特定格式，并且不能将其导出到文件中。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包含多种功能，可用于创建和管理与服务器和数据库一起使用的证书和密钥。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法用于创建和管理与其他应用程序或操作系统一起使用的证书和密钥。  
  
## <a name="certificates"></a>证书  
 证书是一个数字签名的安全对象，其中包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的公钥（还可以选择包含私钥）。 您可以使用外部生成的证书，也可以由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成证书。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 证书符合 IETF X.509v3 证书标准。  
  
 证书非常有用，因为它具有将密钥导出和导入 X.509 证书文件的选项。 用于创建证书的语法允许为证书使用创建选项，例如过期日期。  
  
### <a name="using-a-certificate-in-sql-server"></a>在 SQL Server 中使用证书  
 证书可用来帮助确保连接的安全性（在数据库镜像中）、为包和其他对象签名或者加密数据或连接。 下表列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中有关证书的其他资源。  
  
|主题|说明|  
|-----------|-----------------|  
|[CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)|介绍用于创建证书的命令。|  
|[使用数字签名标识包的源](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md)|显示有关如何使用证书为软件包签名的信息。|  
|[使用数据库镜像终结点证书 (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)|提供有关如何将证书用于数据库镜像的信息。|  
  
## <a name="asymmetric-keys"></a>非对称密钥  
 非对称密钥用于确保对称密钥的安全性。 它们还可用于有限数据加密以及对数据库对象进行数字签名。 非对称密钥由私钥和对应的公钥组成。 有关非对称密钥的详细信息，请参阅 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)的公钥（还可以选择包含私钥）。  
  
 可以从强名称密钥文件导入非对称密钥，但不能将其导出。 它们也没有过期选项。 非对称密钥不能加密连接。  
  
### <a name="using-an-asymmetric-key-in-sql-server"></a>在 SQL Server 中使用非对称密钥  
 非对称密钥可用来帮助确保数据的安全性或为纯文本签名。 下表列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中有关非对称密钥的其他资源。  
  
|主题|说明|  
|-----------|-----------------|  
|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)|介绍用于创建非对称密钥的命令。|  
|[SIGNBYASYMKEY (Transact-SQL)](../../t-sql/functions/signbyasymkey-transact-sql.md)|显示用于为对象签名的选项。|  
  
## <a name="tools"></a>工具  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 提供了用于生成证书和强名称密钥文件的工具和实用工具。 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法相比，这些工具在密钥生成过程中提供了更加丰富的灵活选择。 您可以使用这些工具创建具有更复杂的密钥长度的 RSA 密钥，然后将其导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 下表介绍了在哪里可以找到这些工具。  
  
| 工具 | 目的 |
| ---- | ------- |
|[New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)|创建自签名证书。|  
|[makecert](/windows/desktop/SecCrypto/makecert)|创建证书。 已弃用以支持 New-SelfSignedCertificate。|  
|[sn](/dotnet/framework/tools/sn-exe-strong-name-tool)|创建对称密钥的强名称。|  
  
## <a name="related-tasks"></a>Related Tasks  
 [选择加密算法](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [sys.certificates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)   
 [透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
  
