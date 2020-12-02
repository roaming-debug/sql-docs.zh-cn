---
title: 创建加密的备份 | Microsoft Docs
description: 本文演示如何使用 Transact-SQL 在 SQL Server 中创建加密的备份。 可以备份到磁盘或 Azure 存储。
ms.custom: ''
ms.date: 08/04/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: e29061d3-c2ab-4d98-b9be-8e90a11d17fe
author: cawrites
ms.author: chadam
ms.openlocfilehash: b02f55100110952bce5785130ba330b5c96bce81
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130440"
---
# <a name="create-an-encrypted-backup"></a>创建加密的备份
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍使用 Transact-SQL 创建加密备份所需的步骤。  有关使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的示例，请参阅 [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)。 
  
## <a name="backup-to-disk-with-encryption"></a>备份到磁盘并加密  
 **先决条件：**  
  
-   有权访问本地磁盘或空间充足的存储以创建数据库备份。  
  
-   master 数据库的数据库主密钥以及 SQL Server 实例上提供的证书或非对称密钥。 有关加密要求和权限，请参阅 [Backup Encryption](../../relational-databases/backup-restore/backup-encryption.md)。  
  
 按照以下步骤向本地磁盘创建数据库的加密备份。 本例使用一个名为 MyTestDB 的用户数据库。  
  
1.  **创建 master 数据库的数据库主密钥：** 选择密码来对存储于该数据库中的主密钥副本进行加密。 连接到数据库引擎，启动新查询窗口并复制和粘贴下例，然后单击 **“执行”** 。  
  
    ```  
    -- Creates a database master key.   
    -- The key is encrypted using the password "<master key password>"  
    USE master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master key password>';  
    GO  
  
    ```  
  
2.  **创建备份证书：** 在 master 数据库中创建备份证书。 将以下示例复制并粘贴到查询窗口中，然后单击 **“执行”** 。  
  
    ```  
    Use Master  
    GO  
    CREATE CERTIFICATE MyTestDBBackupEncryptCert  
       WITH SUBJECT = 'MyTestDB Backup Encryption Certificate';  
    GO  
  
    ```  
  
3.  **备份数据库：** 指定要使用的加密算法和证书。 将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  

    ```
    BACKUP DATABASE [MyTestDB]  
    TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MyTestDB.bak'  
    WITH  
      COMPRESSION,  
      ENCRYPTION   
       (  
       ALGORITHM = AES_256,  
       SERVER CERTIFICATE = MyTestDBBackupEncryptCert  
       ),  
      STATS = 10  
    GO
    ```  
  
 有关加密受 EKM 保护的备份的示例，请参阅[使用 Azure 密钥保管库的可扩展密钥管理 (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)。  
  
### <a name="backup-to-azure-storage-with-encryption"></a>备份到 Azure 存储并加密  
 如果使用“SQL Server 备份到 URL”  选项向 Azure 存储创建备份，则加密步骤相同，但必须使用 URL 作为目标，并使用 SQL 凭据向 Azure 存储进行身份验证。 若要为 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 配置加密选项，请参阅 [对 Microsoft Azure 启用 SQL Server 托管备份](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)。  
  
 **先决条件：**  
  
-   Windows 存储帐户和容器。 有关详细信息，请参阅 [第 1 课：创建 Azure 存储对象](/previous-versions/sql/sql-server-2016/jj720557(v=sql.130))。  
  
-   master 数据库的数据库主密钥以及 SQL Server 实例上的证书或非对称密钥。 有关加密要求和权限，请参阅 [Backup Encryption](../../relational-databases/backup-restore/backup-encryption.md)。  
  
1.  **创建 SQL Server 凭据：** 若要创建 SQL Server 凭据，请连接到数据库引擎、打开新查询窗口并复制和粘贴到下例，然后单击“执行”  。  
  
    ```  
    CREATE CREDENTIAL mycredential   
    WITH IDENTITY= 'mystorageaccount' - this is the name of the storage account you specified when creating a storage account    
    , SECRET = '<storage account access key>' - this should be either the Primary or Secondary Access Key for the storage account  
    ```  
  
2.  **创建数据库主密钥：** 选择密码来对存储于该数据库中的主密钥副本进行加密。 连接到数据库引擎，启动新查询窗口并复制和粘贴下例，然后单击 **“执行”** 。  
  
    ```  
    -- Creates a database master key.  
    -- The key is encrypted using the password "<master key password>"  
    USE Master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master key password>';  
    GO  
  
    ```  
  
3.  **创建备份证书：** 在 master 数据库中创建备份证书。 将下例复制并粘贴到查询窗口中，然后单击 **“执行”**  
  
    ```  
    USE Master;  
    GO  
    CREATE CERTIFICATE MyTestDBBackupEncryptCert  
       WITH SUBJECT = 'MyTestDBBackupEncryptCert ';  
    GO  
  
    ```  
  
4.  **备份数据库：** 指定要使用的加密算法和证书。 将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    BACKUP DATABASE [MyTestDB]  
    TO URL = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MyTestDB.bak'  
    WITH  
      CREDENTIAL 'mycredential' - this is the name of the credential created in the first step.  
      ,COMPRESSION  
      ,ENCRYPTION   
       (  
       ALGORITHM = AES_256,  
       SERVER CERTIFICATE = MyTestDBBackupEncryptCert  
       ),  
      STATS = 10  
    GO  
  
    ```  
  
