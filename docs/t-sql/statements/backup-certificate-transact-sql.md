---
description: BACKUP CERTIFICATE (Transact-SQL)
title: BACKUP CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DUMP_CERTIFICATE_TSQL
- BACKUP CERTIFICATE
- sql13.swb.exportcertificate.f1
- DUMP CERTIFICATE
- BACKUP_CERTIFICATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- decryption [SQL Server], certificates
- exporting certificates
- certificates [SQL Server], exporting
- BACKUP CERTIFICATE statement
- backing up certificates [SQL Server]
- decryption [SQL Server]
- cryptography [SQL Server], certificates
ms.assetid: 509b9462-819b-4c45-baae-3d2d90d14a1c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 9caae25f91fcddbe5dd5cdaaa941fcfddf55c004
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199411"
---
# <a name="backup-certificate-transact-sql"></a>BACKUP CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-pdw](../../includes/applies-to-version/sql-pdw.md)]

  将证书导出到文件中。  
  
 ![链接图标](../../database-engine/configure-windows/media/topic-link.gif "“链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server  
  
BACKUP CERTIFICATE certname TO FILE = 'path_to_file'  
    [ WITH PRIVATE KEY   
      (   
        FILE = 'path_to_private_key_file' ,  
        ENCRYPTION BY PASSWORD = 'encryption_password'   
        [ , DECRYPTION BY PASSWORD = 'decryption_password' ]   
      )   
    ]  
```  
  
   
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
BACKUP CERTIFICATE certname TO FILE ='path_to_file'  
      WITH PRIVATE KEY   
      (   
        FILE ='path_to_private_key_file',  
        ENCRYPTION BY PASSWORD ='encryption_password'   
      )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 certname  
 要备份的证书的名称。

 TO FILE = path_to_file  
 指定要保存证书的文件的完整路径（包括文件名）。 此路径可以是本地路径，也可以是网络位置的 UNC 路径。 如果仅指定了文件名，则该文件将保存在实例的默认用户数据文件夹中（可能是也可能不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DATA 文件夹）。 对于 SQL Server Express LocalDB，实例的默认用户数据文件夹是 `%USERPROFILE%` 环境变量为创建实例的帐户指定的路径。  

 WITH PRIVATE KEY 指定将证书的私钥保存到文件中。 此子句为可选项。

 FILE = path_to_private_key_file  
 指定要保存私钥的文件的完整路径（包括文件名）。 此路径可以是本地路径，也可以是网络位置的 UNC 路径。 如果仅指定了文件名，则该文件将保存在实例的默认用户数据文件夹中（可能是也可能不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DATA 文件夹）。 对于 SQL Server Express LocalDB，实例的默认用户数据文件夹是 `%USERPROFILE%` 环境变量为创建实例的帐户指定的路径。  

 ENCRYPTION BY PASSWORD = encryption_password  
 用于在将密钥写入备份文件之前对私钥进行加密的密码。 该密码需要进行复杂性检查。  
  
 DECRYPTION BY PASSWORD = decryption_password  
 用于在备份密钥之前对私钥进行解密的密码。 如果证书是用主密钥加密，便无需使用此参数。 
  
## <a name="remarks"></a>备注  
 如果在数据库中使用密码对私钥进行加密，则必须指定解密密码。  
  
 将私钥备份到文件时，需要进行加密。 用于保护文件中私钥的密码和用于加密数据库中证书私钥的密码不是同一个密码。  

 私钥以 PVK 文件格式保存。

 若要使用或不使用私钥还原备份证书，请使用 [CREATE CERTIFICATE](../../t-sql/statements/create-certificate-transact-sql.md) 语句。
 
 若要将私钥还原到数据库中的现有证书，请使用 [ALTER CERTIFICATE](../../t-sql/statements/alter-certificate-transact-sql.md) 语句。
 
 在你执行备份后，这些文件就会通过 ACL 备份到 SQL Server 实例的服务帐户。 如果需要将证书还原到在不同帐户下运行的服务器，必须将文件权限调整为文件可供新帐户读取。 
  
## <a name="permissions"></a>权限  
 要求对证书具有 CONTROL 权限，并且了解用于对私钥进行加密的密码的相关信息。 如果你只备份证书的公共部分，此命令必须拥有对证书的某种权限，并且调用方对证书的 VIEW 权限尚未遭拒绝。  
  
## <a name="examples"></a>示例  
  
### <a name="a-exporting-a-certificate-to-a-file"></a>A. 将证书导出到文件中  
 以下示例将证书导出到文件中。  
  
```sql
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert';  
GO  
```  
  
### <a name="b-exporting-a-certificate-and-a-private-key"></a>B. 导出证书和私钥  
 在以下示例中，已备份的证书的私钥将使用密码 `997jkhUbhk$w4ez0876hKHJH5gh` 进行加密。  
  
```sql
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert'  
    WITH PRIVATE KEY ( FILE = 'c:\storedkeys\sales05key' ,   
    ENCRYPTION BY PASSWORD = '997jkhUbhk$w4ez0876hKHJH5gh' );  
GO  
```  
  
### <a name="c-exporting-a-certificate-that-has-an-encrypted-private-key"></a>C. 导出具有加密私钥的证书  
 在以下示例中，证书的私钥在数据库中进行加密。 必须使用密码 `9875t6#6rfid7vble7r` 对私钥进行解密。 将证书存储到备份文件中时，私钥将使用密码 `9n34khUbhk$w4ecJH5gh` 进行加密。  
  
```sql
BACKUP CERTIFICATE sales09 TO FILE = 'c:\storedcerts\sales09cert'   
    WITH PRIVATE KEY ( DECRYPTION BY PASSWORD = '9875t6#6rfid7vble7r' ,  
    FILE = 'c:\storedkeys\sales09key' ,   
    ENCRYPTION BY PASSWORD = '9n34khUbhk$w4ecJH5gh' );  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE (Transact-SQL)](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE (Transact-SQL)](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [CERTENCODED (Transact-SQL)](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID (Transact-SQL)](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY (Transact-SQL)](../../t-sql/functions/certproperty-transact-sql.md)  
  
  

