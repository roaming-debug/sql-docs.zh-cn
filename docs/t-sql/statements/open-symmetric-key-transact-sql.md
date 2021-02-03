---
description: OPEN SYMMETRIC KEY (Transact-SQL)
title: OPEN SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN SYMMETRIC KEY
- OPEN_SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], opening
- OPEN SYMMETRIC KEY statement
ms.assetid: ff019a7c-c373-46c7-ac43-ffb7e2ee60b3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 939ad2351cfa4bf6340d0451af3ba71656fe5b85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204636"
---
# <a name="open-symmetric-key-transact-sql"></a>OPEN SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  解密对称密钥并使其可供使用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
OPEN SYMMETRIC KEY Key_name DECRYPTION BY <decryption_mechanism>  
  
<decryption_mechanism> ::=  
    CERTIFICATE certificate_name [ WITH PASSWORD = 'password' ]  
    |  
    ASYMMETRIC KEY asym_key_name [ WITH PASSWORD = 'password' ]  
    |  
    SYMMETRIC KEY decrypting_Key_name  
    |  
    PASSWORD = 'decryption_password'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 Key_name  
 要打开的对称密钥的名称。  
  
 CERTIFICATE certificate_name  
 证书的名称，该证书的私钥将用于解密对称密钥。  
  
 ASYMMETRIC KEY asym_key_name  
 非对称密钥的名称，该密钥的私钥将用于解密对称密钥。  
  
 WITH PASSWORD ='password'  
 用于解密证书或非对称密钥的私钥的密码。  
  
 SYMMETRIC KEY *decrypting_key_name*  
 对称密钥的名称，该密钥将用于解密正在打开的对称密钥。  
  
 PASSWORD ='password'  
 用于保护对称密钥的密码。  
  
## <a name="remarks"></a>注解  
 打开的对称密钥将绑定到会话而不是安全上下文。 打开的密钥将持续有效，直到它显式关闭或会话终止。 如果您打开一个对称密钥然后切换上下文，则该密钥将保持打开状态并在模拟的上下文中可用。 可以在 [sys.openkeys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md) 目录视图中查看有关打开的对称密钥的信息。  
  
 如果对称密钥由另一个密钥加密，则必须首先打开该密钥。  
  
 如果对称密钥已打开，则查询为 **NO_OP**。  
  
 如果所提供的用于解密对称密钥的密码、证书或密钥不正确，则查询将失败。  
  
 无法打开使用加密提供程序创建的对称密钥。 没有 **OPEN** 语句，使用此类对称密钥的加密和解密操作也会成功，因为加密提供程序会打开和关闭密钥。  
  
## <a name="permissions"></a>权限  
 调用方必须具有对密钥的某些权限，并且必须未被拒绝授予对密钥的 VIEW DEFINITION 权限。 其他权限要求因解密机制的而异：  
  
-   DECRYPTION BY CERTIFICATE：具有对证书的 CONTROL 权限并且知道加密其私钥的密码。  
  
-   DECRYPTION BY ASYMMETRIC KEY：具有对非对称密钥的 CONTROL 权限并知道加密其私钥的密码。  
  
-   DECRYPTION BY PASSWORD：知道用于加密对称密钥的密码之一。  
  
## <a name="examples"></a>示例  
  
### <a name="a-opening-a-symmetric-key-by-using-a-certificate"></a>A. 使用证书打开对称密钥  
 以下示例使用 `SymKeyMarketing3` 证书的私钥打开对称密钥 `MarketingCert9` 并对其进行解密。  
  
```sql  
USE AdventureWorks2012;  
OPEN SYMMETRIC KEY SymKeyMarketing3   
    DECRYPTION BY CERTIFICATE MarketingCert9;  
GO  
```  
  
### <a name="b-opening-a-symmetric-key-by-using-another-symmetric-key"></a>B. 使用另一对称密钥打开对称密钥  
 以下示例使用对称密钥 `MarketingKey11` 打开对称密钥 `HarnpadoungsatayaSE3` 并对其进行解密。  
  
```sql  
USE AdventureWorks2012;  
-- First open the symmetric key that you want for decryption.  
OPEN SYMMETRIC KEY HarnpadoungsatayaSE3   
    DECRYPTION BY CERTIFICATE sariyaCert01;  
-- Use the key that is already open to decrypt MarketingKey11.  
OPEN SYMMETRIC KEY MarketingKey11   
    DECRYPTION BY SYMMETRIC KEY HarnpadoungsatayaSE3;  
GO   
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [CLOSE SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/close-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [可扩展密钥管理 &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
