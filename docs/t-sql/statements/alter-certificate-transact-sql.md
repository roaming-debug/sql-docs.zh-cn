---
description: ALTER CERTIFICATE (Transact-SQL)
title: ALTER CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_CERTIFICATE_TSQL
- ALTER CERTIFICATE
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- encryption [SQL Server], certificates
- modifying certificates
- private keys [SQL Server]
- ALTER CERTIFICATE statement
- certificates [SQL Server], modifying
ms.assetid: da4dc25e-72e0-4036-87ce-22de83160836
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a483870b4cc4d7419d853c5002f06c29b9713120
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348565"
---
# <a name="alter-certificate-transact-sql"></a>ALTER CERTIFICATE (Transact-SQL)

[!INCLUDE [sql-asdb-pdw](../../includes/applies-to-version/sql-asdb-pdw.md)]

  更改用于加密证书私钥的密码，删除私钥或导入私钥（如果不存在）。 更改证书对于 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 的可用性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER CERTIFICATE certificate_name   
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY ( <private_key_spec> )  
    | WITH ACTIVE FOR BEGIN_DIALOG = { ON | OFF }  
  
<private_key_spec> ::=   
      {   
        { FILE = 'path_to_private_key' | BINARY = private_key_bits }  
         [ , DECRYPTION BY PASSWORD = 'current_password' ]  
         [ , ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
    |  
      {  
         [ DECRYPTION BY PASSWORD = 'current_password' ]  
         [ [ , ] ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
``` 
 
 
```syntaxsql  
-- Syntax for Parallel Data Warehouse  
  
ALTER CERTIFICATE certificate_name   
{  
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY (   
        FILE = '<path_to_private_key>',  
        DECRYPTION BY PASSWORD = '<key password>' )
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 certificate_name  
 数据库中标识证书的唯一名称。  
  
 REMOVE PRIVATE KEY   
 指定私钥不应再保留在数据库内。  
  
 WITH PRIVATE KEY 指定将证书的私钥加载到 SQL Server 中。

 FILE ='path_to_private_key'  
 指定私钥的完整路径（包括文件名）。 此参数可以是本地路径或网络位置的 UNC 路径。 将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户的安全上下文中访问此文件。 使用此选项时，请确保服务帐户有权访问指定的文件。
 
 如果仅指定文件名，则该文件将保存在实例的默认用户数据文件夹中。 此文件夹可能是（或可能不是）[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DATA 文件夹。 对于 SQL Server Express LocalDB，实例的默认用户数据文件夹是 `%USERPROFILE%` 环境变量为创建实例的帐户指定的路径。  
  
 BINARY = private_key_bits  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 指定为二进制常量的专有键位。 这些位可采用加密形式。 如果加密，则用户必须提供解密密码。 不会对此密码执行密码策略检查。 私钥位应该采用 PVK 文件格式。  
  
 DECRYPTION BY PASSWORD = current_password  
 指定解密私钥所需的密码。  
  
 ENCRYPTION BY PASSWORD = new_password  
 指定用于对数据库中的证书私钥进行加密的密码。 new_password 必须符合运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的 Windows 密码策略要求。 有关详细信息，请参阅 [Password Policy](../../relational-databases/security/password-policy.md)。  
  
 ACTIVE FOR BEGIN_DIALOG = { ON | OFF }  
 使证书可用于 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对话会话的发起方。  
  
## <a name="remarks"></a>备注  
 私钥必须与 certificate_name 指定的公钥相对应。  
  
 如果文件中的密码受空密码保护，则可省略 DECRYPTION BY PASSWORD 子句。  
  
 在导入数据库中已存在的证书私钥时，该私钥将自动受到数据库主密钥的保护。 若要使用密码保护私钥，请使用 ENCRYPTION BY PASSWORD 子句。  
  
 REMOVE PRIVATE KEY 选项将从数据库中删除证书的私钥。 当使用证书来验证签名或在不需要私钥的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 方案中时，可以删除私钥。 请勿删除保护对称密钥的证书的私钥。 需要还原私钥才能对任何应使用证书验证的其他模块或字符串进行签名，或者解密已使用证书加密的值。   
  
 如果使用数据库主密钥加密私钥，则不必指定解密密码。  
 
 若要更改用于加密私钥的密码，请不要指定 FILE 或 BINARY 子句。
  
> [!IMPORTANT]  
>  从数据库删除私钥前，始终对其建立存档副本。 有关详细信息，请参阅 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md) 和 [CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md)。  
  
 WITH PRIVATE KEY 选项在包含的数据库中不可用。  
  
## <a name="permissions"></a>权限  
 需要对证书具有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-removing-the-private-key-of-a-certificate"></a>A. 删除证书的私钥  
  
```sql  
ALTER CERTIFICATE Shipping04   
    REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="b-changing-the-password-that-is-used-to-encrypt-the-private-key"></a>B. 更改用于加密私钥的密码  
  
```sql  
ALTER CERTIFICATE Shipping11   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hkjdskghFDGGG4%',  
    ENCRYPTION BY PASSWORD = '34958tosdgfkh##38');  
GO  
```  
  
### <a name="c-importing-a-private-key-for-a-certificate-that-is-already-present-in-the-database"></a>C. 为数据库中已存在的证书导入私钥  
  
```sql  
ALTER CERTIFICATE Shipping13   
    WITH PRIVATE KEY (FILE = 'c:\importedkeys\Shipping13',  
    DECRYPTION BY PASSWORD = 'GDFLKl8^^GGG4000%');  
GO  
```  
  
### <a name="d-changing-the-protection-of-the-private-key-from-a-password-to-the-database-master-key"></a>D. 将私钥保护从密码更改为数据库主密钥  
  
```sql  
ALTER CERTIFICATE Shipping15   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hk000eEnvjkjy#F%');  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)  
 [DROP CERTIFICATE (Transact-SQL)](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)  
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
 [CERTENCODED (Transact-SQL)](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID (Transact-SQL)](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY (Transact-SQL)](../../t-sql/functions/certproperty-transact-sql.md)  
  
  

