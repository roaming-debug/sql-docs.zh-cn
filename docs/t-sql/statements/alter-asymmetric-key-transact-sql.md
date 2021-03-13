---
description: ALTER ASYMMETRIC KEY (Transact-SQL)
title: ALTER ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.date: 04/12/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ASYMMETRIC_KEY_TSQL
- ALTER ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- passwords [SQL Server], asymmetric keys
- encryption [SQL Server], asymmetric keys
- DECRYPTION BY PASSWORD option
- ALTER ASYMMETRIC KEY statement
- asymmetric keys [SQL Server], modifying
ms.assetid: 958e95d6-fbe6-43e8-abbd-ccedbac2dbac
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 4cf01b70a9ebeff0a339b710e506f033da396c40
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421878"
---
# <a name="alter-asymmetric-key-transact-sql"></a>ALTER ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  更改非对称密钥的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER ASYMMETRIC KEY Asym_Key_Name <alter_option>  
  
<alter_option> ::=  
      <password_change_option>   
    | REMOVE PRIVATE KEY   

<password_change_option> ::=  
    WITH PRIVATE KEY ( <password_option> [ , <password_option> ] )  

<password_option> ::=  
      ENCRYPTION BY PASSWORD = 'strongPassword'  
    | DECRYPTION BY PASSWORD = 'oldPassword'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *Asym_Key_Name*  
 非对称密钥在数据库中所使用的名称。  
  
 REMOVE PRIVATE KEY   
 从非对称密钥中删除私钥，但不删除公钥。  
  
 WITH PRIVATE KEY  
 更改私钥的保护。  
  
 ENCRYPTION BY PASSWORD **='** _strongPassword_*_'_*  
 指定用于保护私钥的新密码。 password 必须符合运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的 Windows 密码策略要求。 如果省略该选项，则使用数据库主密钥对私钥进行加密。  
  
 DECRYPTION BY PASSWORD **='** _oldPassword_*_'_*  
 指定当前用于保护私钥的旧密码。 如果私钥使用数据库主密钥进行加密，则不需要指定旧密码。  
  
## <a name="remarks"></a>备注  
 如果没有 ENCRYPTION BY PASSWORD 选项所需的数据库主密钥，并且未提供任何密码，则该操作将失败。 有关如何创建数据库主密钥的信息，请参阅 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md)。  
  
 您可以按下表所示，指定 PRIVATE KEY 选项，然后使用 ALTER ASYMMETRIC KEY 更改私钥的保护。  
  
|更改其保护|ENCRYPTION BY PASSWORD|DECRYPTION BY PASSWORD|  
|----------------------------|----------------------------|----------------------------|  
|旧密码到新密码|必选|必需|  
|密码到主密钥|省略|必选|  
|主密钥到密码|必选|省略|  
  
 必须首先打开数据库主密钥，然后才能使用它来保护私钥。 有关详细信息，请参阅 [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md)。  
  
 若要更改非对称密钥的所有权，请使用 [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 如果要删除私钥，则要求对非对称密钥具有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-the-password-of-the-private-key"></a>A. 更改私钥的密码  
 以下示例更改用于保护非对称密钥 `PacificSales09` 的私钥的密码。 新密码为 `<enterStrongPasswordHere>`。  
  
```sql  
ALTER ASYMMETRIC KEY PacificSales09   
    WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<oldPassword>',  
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>');  
GO  
```  
  
### <a name="b-removing-the-private-key-from-an-asymmetric-key"></a>B. 从非对称密钥中删除私钥  
 以下示例从 `PacificSales19` 中删除私钥，只保留公钥。  
  
```sql  
ALTER ASYMMETRIC KEY PacificSales19 REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="c-removing-password-protection-from-a-private-key"></a>C. 从私钥中删除密码保护  
 以下示例从私钥中删除密码保护，然后使用数据库主密钥来保护该私钥。  
  
```sql  
OPEN MASTER KEY DECRYPTION BY PASSWORD = '<database master key password>';  
ALTER ASYMMETRIC KEY PacificSales09 WITH PRIVATE KEY (  
    DECRYPTION BY PASSWORD = '<enterStrongPasswordHere>' );  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [SQL Server 和数据库加密密钥（数据库引擎）](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md)   
 [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md)   
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
