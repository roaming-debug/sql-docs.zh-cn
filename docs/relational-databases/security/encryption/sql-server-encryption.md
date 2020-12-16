---
title: SQL Server 加密 | Microsoft Docs
description: 通过这些资源了解 SQL Server 如何使用加密来增强数据库的安全性。
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], about encryption
- security [SQL Server], encryption
- cryptography [SQL Server], about cryptography
ms.assetid: ead0150e-4943-4ad5-84c8-36f85c7278f4
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb11e565e4f1623ab0e25f47ee1b3c34bd38059a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475558"
---
# <a name="sql-server-encryption"></a>SQL Server 加密
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  加密是指通过使用密钥或密码对数据进行模糊处理的过程。 这会使数据变得毫无用处，除非使用对应的解密密钥或密码。 加密并不解决访问控制问题。 不过，它可以通过限制数据丢失来增强安全性，即使在访问控制失效的情况下。 例如，如果数据库主机配置有误且黑客获取了敏感数据，则如果数据已加密，那么被盗信息可能会毫无用处。  
  

> [!IMPORTANT]  
>  虽然加密是可帮助确保安全性的有力工具，但它并不适用于所有数据或连接。 在决定是否实现加密时，请考虑用户访问数据的方式。 如果用户通过公共网络访问数据，则可能需要使用数据加密以增强安全性。 但是，如果所有访问都具有某项安全 Intranet 配置，则可能不需要使用加密。 任何时候使用加密时还应包括密码、密钥和证书的维护策略。  
  
> [!NOTE]  
>  有关传输层安全性 (TLS 1.2) 的最新信息，请参阅[针对 Microsoft SQL Server 的 TLS 1.2 支持](https://support.microsoft.com/kb/3135244)。  

您可以在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中为连接、数据和存储过程使用加密。 以下主题包含关于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的加密的详细信息。  

 [加密层次结构](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
 提供有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的加密层次结构的信息。  
  
 [选择加密算法](../../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
 说明如何选择有效的加密算法。  
  
 [透明数据加密 (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
 提供有关如何以透明方式来加密数据的一般信息。  
  
 [SQL Server 和数据库加密密钥（数据库引擎）](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，加密密钥包括一组用来保护敏感数据的公钥、私钥和对称密钥。 该部分介绍如何实现和管理加密密钥。  
  
 [Always Encrypted（数据库引擎）](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
 确保本地数据库管理员、云数据库操作员或其他权限虽高但没有获得此方面授权的用户无法访问加密的数据。  
  
 [动态数据屏蔽](../../../relational-databases/security/dynamic-data-masking.md)  
 通过对非特权用户屏蔽敏感数据来限制敏感数据的公开。  
  
 [SQL Server 证书和非对称密钥](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)  
 有关如何使用公钥加密的信息。  
  
## <a name="related-content"></a>相关内容  
 [保护 SQL Server](../../../relational-databases/security/securing-sql-server.md)  
 简要介绍如何帮助确保 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 平台的安全性以及如何处理用户和安全对象。  

[Azure SQL 数据库安全功能概述](/azure/sql-database/sql-database-security-overview)
</br>对用于保护数据、控制访问及执行主动监视的 Azure SQL 数据库安全功能的概述。
  
 [加密函数 (Transact-SQL)](../../../t-sql/functions/cryptographic-functions-transact-sql.md)  
 说明如何实现加密函数。  
  
 [ENCRYPTBYPASSPHRASE (Transact-SQL)](../../../t-sql/functions/encryptbypassphrase-transact-sql.md)  
 说明如何使用密码来加密数据。  
  
 [ENCRYPTBYKEY (Transact-SQL)](../../../t-sql/functions/encryptbykey-transact-sql.md)  
 说明如何使用对称密钥来加密数据。  
  
 [ENCRYPTBYASYMKEY (Transact-SQL)](../../../t-sql/functions/encryptbyasymkey-transact-sql.md)  
 说明如何使用非对称密钥来加密数据。  
  
 [ENCRYPTBYCERT (Transact-SQL)](../../../t-sql/functions/encryptbycert-transact-sql.md)  
 说明如何使用证书来加密数据。  
  
## <a name="external-resources"></a>外部资源  
 [Microsoft TechNet：SQL Server 技术中心：SQL Server 2012 安全性和保护](https://download.microsoft.com/download/8/F/A/8FABACD7-803E-40FC-ADF8-355E7D218F4C/SQL_Server_2012_Security_Best_Practice_Whitepaper_Apr2012.docx)  
 包含有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安全性的最新信息。  
  
## <a name="see-also"></a>另请参阅  
 [sys.key_encryptions (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)   
 [SQL Server 和数据库加密密钥（数据库引擎）](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [备份和还原 Reporting Services 加密密钥](../../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)     
 [启用数据库引擎的加密连接](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)    
  
