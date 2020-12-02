---
title: 备份加密 | Microsoft Docs
description: 本文介绍 SQL Server 备份的加密选项，包括备份期间加密的用法、优点和推荐做法。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 334b95a8-6061-4fe0-9e34-b32c9f1706ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: b06c0cc9b3b50510282c620ff86aa1a478fae419
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129331"
---
# <a name="backup-encryption"></a>备份加密
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题概述 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份的加密选项。 其中详细介绍备份期间加密的用法、优点和推荐做法。  

## <a name="overview"></a><a name="Overview"></a> 概述  
 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]开始，SQL Server 可在创建备份时加密数据。 通过在创建备份时指定加密算法和加密程序（证书或非对称密钥），可创建加密的备份文件。 所有存储目标：支持本地和 Windows Azure 存储。 此外，可为 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 操作配置加密选项，这是 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]中引入的一项新功能。  
  
 若要在备份期间加密，必须指定加密算法以及用于保护加密密钥的加密程序。 支持以下加密选项：  
  
- **加密算法：** 支持的加密算法包括：AES 128、AES 192、AES 256 和 Triple DES  
  
- **加密程序：** 证书或非对称密钥  
  
> [!CAUTION]  
> 备份证书或非对称密钥很重要，并且最好备份到与用于加密的备份文件不同的位置。 没有证书或非对称密钥，你将无法还原备份，从而使备份文件无法使用。  
  
 **还原加密的备份：** SQL Server 还原不需要在还原期间指定任何加密参数。 但要求在要还原到的实例上有用于加密备份文件的证书或非对称密钥。 执行还原的用户帐户必须对证书或密钥具有 **VIEW DEFINITION** 权限。 如果将加密的备份还原到其他实例，则必须确保该实例上有证书。  
将加密数据库还原到新位置的顺序是：

1. 旧数据库中的 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)
1. 新位置 master 数据库中的 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md)
1. [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)，将导出的证书从旧数据库备份到新服务器上的某个位置
1. [将数据库还原到新位置 (SQL Server)](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)

 如果从经过 TDE 加密的数据库还原备份，则要还原到的实例上应有 TDE 证书。 有关详细信息，请参阅 [将受 TDE 保护的数据库移到其他 SQL Server](../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)。
  
##  <a name="benefits"></a><a name="Benefits"></a> 优势  
  
1. 加密数据库备份有助于保证数据安全：SQL Server 提供用于在创建备份时加密备份数据的选项。  
  
1. 加密还可用于使用 TDE 加密的数据库。  
  
1. 由 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]进行的备份支持加密，这样可提高站点外备份的安全性。  
  
1. 此功能支持多个最高 AES 256 位的加密算法。 这样可选择符合要求的算法。  
  
1. 可将加密密钥与扩展密钥管理 (EKM) 提供程序集成。  
 
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 以下是有关加密备份的先决条件：  
  
1. **创建 master 数据库的数据库主密钥：** 数据库主密钥是一种用于保护数据库中存在的证书私钥和非对称密钥的对称密钥。 有关详细信息，请参阅 [SQL Server 和数据库加密密钥（数据库引擎）](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)。  
  
1. 创建用于备份加密的证书或非对称密钥。 有关创建证书的详细信息，请参阅 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)。 有关创建非对称密钥的详细信息，请参阅[创建非对称密钥 (Transact-SQL)](../../t-sql/statements/create-asymmetric-key-transact-sql.md)。  
  
    > [!IMPORTANT]  
    >  仅支持位于扩展密钥管理 (EKM) 中的非对称密钥。  
  
##  <a name="restrictions"></a><a name="Restrictions"></a> 限制  
 以下限制适用于加密选项：  
  
- 如果使用非对称密钥加密备份数据，则仅支持位于 EKM 提供程序中的非对称密钥。  
  
- SQL Server Express 和 SQL Server Web 不支持在备份期间进行加密。 但是，支持从加密的备份还原到 SQL Server Express 或 SQL Server Web 的实例。  
  
- 旧版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法读取加密的备份。  
  
- 加密的备份不支持追加到现有的备份集选项。  

##  <a name="permissions"></a><a name="Permissions"></a> 权限  

对加密数据库执行备份操作的帐户需要具有特定权限。 

- 要备份的数据库上的 db_backupoperator 数据库级别角色。 无论是否加密，都需要此角色。 
- 对 `master` 数据库中的证书具有 VIEW DEFINITION 权限。

   下面的示例将为证书授予适当的权限。 
   
   ```tsql
   USE [master]
   GO
   GRANT VIEW DEFINITION ON CERTIFICATE::[<SERVER_CERT>] TO [<db_account>]
   GO
   ```

> [!NOTE]  
> 不需要访问 TDE 证书即可备份或还原受 TDE 保护的数据库。  
  
## <a name="backup-encryption-methods"></a><a name="Methods"></a> 备份加密方法  
 以下各节简要介绍在备份期间加密数据的步骤。 有关使用 Transact-SQL 加密备份的不同步骤的完整演练，请参阅 [创建加密的备份](../../relational-databases/backup-restore/create-an-encrypted-backup.md)。  
  
### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio  
 在以下任何对话框中，可在创建数据库的备份时加密备份。  
  
1. [备份数据库（“备份选项”页）](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)在“备份选项”页上，可以选择“加密”，并指定加密算法和证书或非对称密钥以用于加密。  
  
1. [使用维护计划向导](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure)选择某个备份任务后，可以在“定义备份()任务”页的“选项”选项卡上，选择“备份加密”，并指定加密算法和证书或密钥以用于加密。  
  
### <a name="using-transact-sql"></a>“使用 Transact-SQL”  
 下面是用于加密备份文件的示例 TSQL 语句：  
  
```sql  
BACKUP DATABASE [MYTestDB]  
TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MyTestDB.bak'  
WITH  
  COMPRESSION,  
  ENCRYPTION   
   (  
   ALGORITHM = AES_256,  
   SERVER CERTIFICATE = BackupEncryptCert  
   ),  
  STATS = 10  
GO  
```  
  
 有关完整 Transact-SQL 语法，请参阅 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)。  
  
### <a name="using-powershell"></a>使用 PowerShell  
 此示例创建加密选项并在 **Backup-SqlDatabase** cmdlet 中将它作为参数值以创建加密的备份。  
  
```powershell
$encryptionOption = New-SqlBackupEncryptionOption -Algorithm Aes256 -EncryptorType ServerCertificate -EncryptorName "BackupCert"  

Backup-SqlDatabase -ServerInstance . -Database "<myDatabase>" -BackupFile "<myDatabase>.bak" -CompressionOption On -EncryptionOption $encryptionOption  
```  
  
##  <a name="recommended-practices"></a><a name="RecommendedPractices"></a> 推荐做法  
 将加密证书和密钥备份到安装实例的本地计算机以外的位置。 若要考虑灾难恢复情况，请考虑将证书和密钥存储到站点外位置。 没有用于加密备份的证书即无法还原加密的备份。  
  
 若要还原加密的备份，要还原到的实例上应有进行备份时使用的原始证书及匹配的指纹。 因此，不应在到期时续订或以任何方式更改证书。 续订可导致更改证书并触发指纹更改，从而使证书对于备份文件变为无效。 执行还原的帐户应对备份期间用于加密的证书或非对称密钥具有 VIEW DEFINITION 权限。  
  
 可用性组数据库备份通常在首选备份副本上执行。  如果在不是备份来源的其他副本上还原备份，请确保用于备份的原始证书在还原到的副本上可用。  
  
 如果数据库启用了 TDE，则选择其他证书或非对称密钥用于加密数据库和备份以提高安全性。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
|主题/任务|说明|  
|-----------------|-----------------|  
|[创建加密的备份](../../relational-databases/backup-restore/create-an-encrypted-backup.md)|介绍创建加密的备份所需的基本步骤|  
|[使用 Azure 密钥保管库的可扩展密钥管理 (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)|提供了创建受 Azure 密钥保管库中的密钥保护的加密备份的示例。|  
  
## <a name="see-also"></a>另请参阅  
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)  
