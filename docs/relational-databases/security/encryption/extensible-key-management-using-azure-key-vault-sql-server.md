---
title: 使用 Azure Key Vault 的可扩展密钥管理
description: 使用 SQL Server 连接器在 SQL Server 中通过 Azure Key Vault 实现可扩展性密钥管理。
ms.custom: seo-lt-2019
ms.date: 07/22/2016
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Extensible Key Management with key vault
- Transparent Data Encryption, using EKM and key vault
- EKM, with key vault
- TDE, EKM and key vault
- Key Management with key vault
- SQL Server Connector, about
ms.assetid: 3efdc48a-8064-4ea6-a828-3fbf758ef97c
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: a70ae767aa0f9ed079b616402f3857e03fc3d9dc
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92679062"
---
# <a name="extensible-key-management-using-azure-key-vault-sql-server"></a>使用 Azure Key Vault 的可扩展密钥管理 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  借助 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Azure 密钥保管库的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 加密可以将 Azure 密钥保管库服务用作[可扩展密钥管理 &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)提供程序，以保护 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 加密密钥。  
  
 本主题介绍了 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器。 有关其他信息，你可以参阅 [使用 Azure 密钥保管库的可扩展密钥管理的设置步骤](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)、 [使用具有 SQL 加密功能的 SQL Server 连接器](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)和 [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)。  
  
##  <a name="what-is-extensible-key-management-ekm-and-why-use-it"></a><a name="Uses"></a> 什么是可扩展密钥管理 (EKM)，为什么要使用它？  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 提供了帮助保护敏感数据的几种加密类型，包括[透明数据加密 (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md)、[列级加密](../../../t-sql/functions/cryptographic-functions-transact-sql.md) (CLE) 和[备份加密](../../../relational-databases/backup-restore/backup-encryption.md)。 在传统的密钥层次结构中，上述三种加密类型均使用对称数据加密密钥 (DEK) 对数据进行加密。 通过使用存储在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的密钥层次结构对对称数据加密密钥进行加密而使其获得进一步的保护。 可替代这种模型的是 EKM 提供程序模型。 使用 EKM 提供程序体系结构， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可通过使用存储在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之外的外部加密提供程序中的非对称密钥来保护数据加密密钥。 该模型额外添加了一个安全层，将密钥和数据分开管理。  
   
 下图对传统服务管理密钥层次结构与 Azure 密钥保管库系统进行比较。  
  
 ![该图将传统服务管理密钥层次结构与 Azure Key Vault 系统进行比较。](../../../relational-databases/security/encryption/media/ekm-key-hierarchy-traditional.png "ekm-key-hierarchy-traditional")  
  
   
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器用作 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和 Azure 密钥保管库之间的桥梁，因此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以利用 Azure 密钥保管库服务的可伸缩性、高性能和高可用性。 下图显示了在使用 Azure 密钥保管库和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器的 EKM 提供程序体系结构中如何使用密钥层次结构。  
  
  密钥保管库服务可用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Azure 虚拟机和本地服务器上的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 安装。 Key Vault 服务还提供一种选择，即使用受到严格控制和监视的硬件安全模块 (HSM) 来实现对非对称加密密钥的更高级别的保护。 有关密钥保管库的详细信息，请参阅 [Azure 密钥保管库](/azure/key-vault/general/basic-concepts)。  
  
 下图总结了使用密钥保管库的 EKM 处理流程。 （图中的处理步骤数与图下的设置步骤数并不一致。）  
  
 ![使用 Azure Key Vault 的 SQL Server EKM](../../../relational-databases/security/encryption/media/ekm-using-azure-key-vault.png "使用 Azure Key Vault 的 SQL Server EKM")  

> [!NOTE]  
>  已替换版本 1.0.0.440 和更早的版本，且生产环境不再支持这些版本。 要升级至版本 1.0.1.0 或更高版本，请访问 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=45344) ，并参照“升级 SQL Server 连接器”下 [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)页面上的指南。
  
 有关下一步的信息，请参阅 [Setup Steps for Extensible Key Management Using the Azure Key Vault（使用 Azure 密钥保管库的可扩展密钥管理的设置步骤）](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)。  
  
 有关使用场景，请参阅 [Use SQL Server Connector with SQL Encryption Features（使用具有 SQL 加密功能的 SQL Server 连接器）](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)  
  
