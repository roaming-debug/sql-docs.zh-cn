---
title: SQL Server 和数据库加密密钥
description: 了解 SQL Server 数据库引擎用于加密和保护数据的服务主密钥和数据库主密钥。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- keys [SQL Server], database encryption
ms.assetid: 15c0a5e8-9177-484c-ae75-8c552dc0dac0
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: ac91abe799eb914d2a544d6f9fad598545a920c4
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251131"
---
# <a name="sql-server-and-database-encryption-keys-database-engine"></a>SQL Server 和数据库加密密钥（数据库引擎）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用加密密钥帮助保护存储在服务器数据库中的数据、凭据和连接信息。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的密钥分为两种：“对称”和“非对称”。 对称密钥使用相同的密码对数据进行加密和解密。 非对称密钥使用一个密码来加密数据（称为公钥），使用另一个密码来解密数据（称为私钥）。  
  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，加密密钥包括一组用来保护敏感数据的公钥、私钥和对称密钥。 当第一次启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例时，将在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 初始化过程中创建对称密钥。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用此密钥来加密存储在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的敏感数据。 公钥和私钥由操作系统创建，用于保护对称密钥。 对于在数据库中存储敏感数据的每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，都要创建一个公钥私钥对。  
  
## <a name="applications-for-sql-server-and-database-keys"></a>SQL Server 和数据库密钥的应用  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 在密钥中的应用主要有两个方面：作为某 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上为该实例生成的 *服务主密钥* (SMK) 和作为用于数据库的 *数据库主密钥* (DMK)。

### <a name="service-master-key"></a>服务主密钥
  
 服务主密钥是 SQL Server 加密层次结构的根。 SMK 在第一次启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例时自动生成，用于对链接的服务器密码、凭据和每个数据库中的数据库主密钥进行加密。 SMK 是通过使用采用 Windows 数据保护 API (DPAPI) 的本地计算机密钥进行加密的。 DPAPI 使用从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户的 Windows 凭据和计算机的凭据派生的密钥。 服务主密钥只能由创建它时所用的服务帐户或可以访问该计算机凭据的主体进行解密。

只有创建服务主密钥的 Windows 服务帐户或有权访问服务帐户名称和密码的主体能够打开服务主密钥。

 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 使用 AES-256 加密算法来保护服务主密钥 (SMK) 和数据库主密钥 (DMK)。 AES 是一种比早期版本中使用的 3DES 更新的加密算法。 在将 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 实例升级到 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 后，应重新生成 SMK 和 DMK 以便将主密钥升级到 AES。 有关重新生成 SMK 的详细信息，请参阅 [ALTER SERVICE MASTER KEY (Transact-SQL)](../../../t-sql/statements/alter-service-master-key-transact-sql.md) 和 [ALTER MASTER KEY (Transact-SQL)](../../../t-sql/statements/alter-master-key-transact-sql.md)。

### <a name="database-master-key"></a>数据库主密钥
  
 数据库主密钥是一种用于保护数据库中存在的证书私钥和非对称密钥的对称密钥。 它还可用于对数据进行加密，但由于它有长度限制，所以用于数据加密时实用性不如对称密钥。 要启用数据库主密钥的自动解密，请使用 SMK 对此密钥的副本进行加密。 此密钥的副本存储在使用它的数据库和 **master** 系统数据库中。  
  
 每当更改 DMK 时，存储在 **master** 系统数据库中的 DMK 副本都将在没有提示的情况下更新。 但是，使用 **DROP ENCRYPTION BY SERVICE MASTER KEY** 语句的 **ALTER MASTER KEY** 选项可以更改此默认设置。 必须使用 **OPEN MASTER KEY** 语句和密码打开未使用服务主密钥进行加密的 DMK。  
  
## <a name="managing-sql-server-and-database-keys"></a>管理 SQL Server 和数据库密钥  
 对加密密钥的管理包括创建新数据库密钥，创建服务器和数据库密钥的备份以及了解还原、删除或更改密钥的条件和方式。  
  
 若要管理对称密钥，可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中包括的工具执行下列操作：  
  
-   备份服务器和数据库密钥的副本，以便可以使用这些密钥来恢复服务器安装，或作为计划迁移的一部分。  
  
-   将以前保存的密钥还原到数据库。 这样，新服务器实例就可以访问最初不是由其加密的现有数据。  
  
-   当不能再访问加密数据时删除数据库中的加密数据，这种情况极少出现。  
  
-   当密钥的安全性受到威胁时，重新创建密钥并重新对数据进行加密，这种情况极少出现。 作为安全性方面的最佳做法，您应定期（例如，每隔几个月）重新创建密钥以保护服务器，使其能够抵御试图解开密钥的攻击。  
  
-   在服务器扩展部署（多个服务器同时共享单个数据库以及为该数据库提供可逆加密的密钥）中添加或删除服务器实例。  
  
## <a name="important-security-information"></a>重要的安全信息  
 访问由服务主密钥保护的对象需要使用用来创建该密钥的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户或计算机帐户。 即，计算机与创建密钥的系统绑定在一起。 可以更改 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户或计算机帐户，而不会失去对密钥的访问权限。 但是，如果同时更改两者，则将失去对服务主密钥的访问权限。 如果在不具有这两个元素中的任何一个的情况下失去了服务主密钥的访问权限，则将无法对使用原始密钥加密的数据和对象进行解密。  
  
 如果没有服务主密钥，则将无法还原受服务主密钥保护的连接。  
  
 访问受数据库主密钥保护的对象和数据只需要有用于帮助保护密钥的密码。  
  
> [!CAUTION]  
>  如果失去了对前述密钥的所有访问权限，则将无法访问由这些密钥保护的对象、连接和数据。 可按照此处显示的链接中说明的方法还原服务主密钥，也可以返回原始加密系统来恢复访问权限。 没有用来恢复访问权限的“后门”。  
  
## <a name="in-this-section"></a>本节内容  
 [服务主密钥]()  
 简要介绍服务主密钥及其最佳用法。  
  
 [可扩展密钥管理 &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
 说明如何在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中使用第三方密钥管理系统。  
  
## <a name="related-tasks"></a>Related Tasks  
 [备份服务主密钥](../../../relational-databases/security/encryption/back-up-the-service-master-key.md)  
  
 [还原服务主密钥](../../../relational-databases/security/encryption/restore-the-service-master-key.md)  
  
 [创建数据库主密钥](../../../relational-databases/security/encryption/create-a-database-master-key.md)  
  
 [备份数据库主密钥](../../../relational-databases/security/encryption/back-up-a-database-master-key.md)  
  
 [还原数据库主密钥](../../../relational-databases/security/encryption/restore-a-database-master-key.md)  
  
 [在两个服务器上创建相同的对称密钥](../../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
 [使用 EKM 在 SQL Server 上启用 TDE](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
 [使用 Azure 密钥保管库的可扩展密钥管理 (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
 [加密数据列](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)  
  
## <a name="related-content"></a>相关内容  
 [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
 [ALTER SERVICE MASTER KEY (Transact-SQL)](../../../t-sql/statements/alter-service-master-key-transact-sql.md)  
  
 [还原数据库主密钥](../../../relational-databases/security/encryption/restore-a-database-master-key.md)  
  
## <a name="see-also"></a>另请参阅  
 [备份和还原 Reporting Services 加密密钥](../../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)   
 [删除和重新创建加密密钥（SSRS 配置管理器）](../../../reporting-services/install-windows/ssrs-encryption-keys-delete-and-re-create-encryption-keys.md)   
 [添加和删除扩展部署的加密密钥（SSRS 配置管理器）](../../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md)   
 [透明数据加密 (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
