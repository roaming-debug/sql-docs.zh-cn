---
title: 使用 EKM 在 SQL Server 上启用 TDE | Microsoft Docs
description: 在 SQL Server 中启用透明数据加密，通过在可扩展密钥管理模块中通过 Transact-SQL 使用非对称密钥来保护数据库密钥。
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], TDE using an EKM
- TDE, EKM how to
- EKM, TDE how to
- Transparent Data Encryption, using EKM
ms.assetid: b892e7a7-95bd-4903-bf54-55ce08e225af
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: e455b8b27537a3f7fedd7ca0c9cad356b9d49df4
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764898"
---
# <a name="enable-tde-on-sql-server-using-ekm"></a>使用 EKM 在 SQL Server 上启用 TDE
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本文介绍如何在 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 中启用透明数据加密 (TDE)，以便通过结合使用存储在可扩展密钥管理 (EKM) 模块中的非对称密钥和 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 来保护数据库加密密钥。  
  
 TDE 使用称为数据库加密密钥的对称密钥来加密整个数据库的存储。 还可以使用受主数据库的数据库主密钥保护的证书来保护数据库加密密钥。 有关使用数据库主密钥保护数据库加密密钥的详细信息，请参阅[透明数据加密 (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md)。 有关当 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 在 Azure VM 上运行时配置 TDE 的信息，请参阅[使用 Azure Key Vault 的可扩展密钥管理 (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)。 有关使用 Azure 密钥保管库中的密钥配置 TDE 的信息，请参阅 [使用具有 SQL 加密功能的 SQL Server 连接器](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)。 

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   必须是高特权用户（如系统管理员）才能创建数据库加密密钥以及加密数据库。 该用户必须能够通过 EKM 模块进行身份验证。  
  
-   启动时，[!INCLUDE[ssDE](../../../includes/ssde-md.md)] 必须打开数据库。 为此，您应创建一个将通过 EKM 进行身份验证的凭据，并将该凭据添加到一个基于非对称密钥的登录名。 用户无法使用该登录名进行登录，但 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 将能够通过 EKM 设备对其自身进行身份验证。  
  
-   如果存储在 EKM 模块中的非对称密钥丢失， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]将无法打开数据库。 如果 EKM 提供程序允许你备份非对称密钥，则应该创建备份并将该备份存储到安全的位置。  
  
-   您的 EKM 提供程序所需的选项和参数可能与下面的代码示例中所提供的选项和参数不同。 有关详细信息，请参阅 EKM 提供程序。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 本文使用了以下权限：  
  
-   若要更改配置选项以及运行 RECONFIGURE 语句，您必须具有 ALTER SETTINGS 服务器级别权限。 ALTER SETTINGS 权限由 **sysadmin** 和 **serveradmin** 固定服务器角色隐式持有。  
  
-   需要 ALTER ANY CREDENTIAL 权限。  
  
-   需要 ALTER ANY LOGIN 权限。  
  
-   需要 CREATE ASYMMETRIC KEY 权限。  
  
-   需要拥有对数据库的 CONTROL 权限才能加密该数据库。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-enable-tde-using-ekm"></a>使用 EKM 启用 TDE  
  
1.  将由 EKM 提供程序提供的文件复制到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 计算机上的相应位置。 在本示例中，我们使用 **C:\EKM** 文件夹。  
  
2.  根据 EKM 提供程序的要求，将证书安装到计算机上。  
  
    > [!NOTE]  
    >  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不提供 EKM 提供程序。 每个 EKM 提供程序可以有不同的安装、配置和授权用户的过程。  请查阅 EKM 提供程序文档，完成此步骤。  
  
3.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
4.  在标准菜单栏上，单击 **“新建查询”** 。  
  
5.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Enable advanced options.  
    sp_configure 'show advanced options', 1 ;  
    GO  
    RECONFIGURE ;  
    GO  
    -- Enable EKM provider  
    sp_configure 'EKM provider enabled', 1 ;  
    GO  
    RECONFIGURE ;  
    GO  
    -- Create a cryptographic provider, which we have chosen to call "EKM_Prov," based on an EKM provider  
  
    CREATE CRYPTOGRAPHIC PROVIDER EKM_Prov   
    FROM FILE = 'C:\EKM_Files\KeyProvFile.dll' ;  
    GO  
  
    -- Create a credential that will be used by system administrators.  
    CREATE CREDENTIAL sa_ekm_tde_cred   
    WITH IDENTITY = 'Identity1',   
    SECRET = 'q*gtev$0u#D1v'   
    FOR CRYPTOGRAPHIC PROVIDER EKM_Prov ;  
    GO  
  
    -- Add the credential to a high privileged user such as your   
    -- own domain login in the format [DOMAIN\login].  
    ALTER LOGIN Contoso\Mary  
    ADD CREDENTIAL sa_ekm_tde_cred ;  
    GO  
    -- create an asymmetric key stored inside the EKM provider  
    USE master ;  
    GO  
    CREATE ASYMMETRIC KEY ekm_login_key   
    FROM PROVIDER [EKM_Prov]  
    WITH ALGORITHM = RSA_512,  
    PROVIDER_KEY_NAME = 'SQL_Server_Key' ;  
    GO  
  
    -- Create a credential that will be used by the Database Engine.  
    CREATE CREDENTIAL ekm_tde_cred   
    WITH IDENTITY = 'Identity2'   
    , SECRET = 'jeksi84&sLksi01@s'   
    FOR CRYPTOGRAPHIC PROVIDER EKM_Prov ;  
  
    -- Add a login used by TDE, and add the new credential to the login.  
    CREATE LOGIN EKM_Login   
    FROM ASYMMETRIC KEY ekm_login_key ;  
    GO  
    ALTER LOGIN EKM_Login   
    ADD CREDENTIAL ekm_tde_cred ;  
    GO  
  
    -- Create the database encryption key that will be used for TDE.  
    USE AdventureWorks2012 ;  
    GO  
    CREATE DATABASE ENCRYPTION KEY  
    WITH ALGORITHM  = AES_128  
    ENCRYPTION BY SERVER ASYMMETRIC KEY ekm_login_key ;  
    GO  
  
    -- Alter the database to enable transparent data encryption.  
    ALTER DATABASE AdventureWorks2012   
    SET ENCRYPTION ON ;  
    GO  
    ```  
  
 有关详细信息，请参阅以下主题：  
  
-   [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
-   [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
-   [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
-   [CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)  
  
-   [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
-   [ALTER DATABASE (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [借助 Azure SQL 数据库实现透明数据加密](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
  
