---
description: ADD SIGNATURE (Transact-SQL)
title: ADD SIGNATURE (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ADD SIGNATURE
- ADD_SIGNATURE_TSQL
helpviewer_keywords:
- ADD SIGNATURE statement
- adding digital signatures
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 64d8b682-6ec1-4e5b-8aee-3ba11e72d21f
author: VanMSFT
ms.author: vanto
ms.reviewer: ''
ms.custom: ''
ms.date: 06/10/2020
ms.openlocfilehash: 86513345502531da670b870b5ecf70de9270f18f
ms.sourcegitcommit: ece104654ac14e10d32e59f45916fa944665f4df
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102474901"
---
# <a name="add-signature-transact-sql"></a>ADD SIGNATURE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

将数字签名添加到存储过程、函数、程序集或 DML 触发器中。 此外，还将副署添加到存储过程、函数、程序集或 DML 触发器。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>语法

```syntaxsql
ADD [ COUNTER ] SIGNATURE TO module_class::module_name
    BY <crypto_list> [ ,...n ]
  
<crypto_list> ::=  
    CERTIFICATE cert_name  
    | CERTIFICATE cert_name [ WITH PASSWORD = 'password' ]
    | CERTIFICATE cert_name WITH SIGNATURE = signed_blob
    | ASYMMETRIC KEY Asym_Key_Name  
    | ASYMMETRIC KEY Asym_Key_Name [ WITH PASSWORD = 'password'.]
    | ASYMMETRIC KEY Asym_Key_Name WITH SIGNATURE = signed_blob
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

module_class  
向其中添加签名的模块的类。 架构范围内的模块的默认类为 OBJECT。  
  
 module_name  
 要签名或副署的存储过程、函数、程序集或触发器的名称。  
  
 CERTIFICATE cert_name  
 对存储过程、函数、程序集或触发器进行签名或副署时所用证书的名称。  
  
 WITH PASSWORD ='password'  
 是对证书私钥或非对称密钥进行解密时所需的密码。 仅当数据库主密钥不保护私钥时才需要该子句。  
  
 SIGNATURE =signed_blob  
 指定模块的已签名二进制大型对象 (BLOB)。 如果不提供私钥，而只提供模块时，则该子句非常有用。 当使用该子句时，只需要模块、签名和公钥便可将已签名的二进制大型对象添加到数据库。 signed_blob 是以十六进制格式表示的 Blob 自身。  
  
 ASYMMETRIC KEY Asym_Key_Name  
 对存储过程、函数、程序集或触发器进行签名或副署时所用非对称密钥的名称。  
  
## <a name="remarks"></a>备注

要签名或副署的模块以及用来对模块进行签名的证书或非对称密钥必须已经存在。 模块中的每个字符都包括在签名计算中。 这包括前导回车符和换行符。  
  
 可以使用任意数量的证书和非对称密钥对模块进行签名和副署。  
  
 当更改模块时，便会删除模块的签名。  
  
 如果模块包含 EXECUTE AS 子句，则也会将主体的安全 ID (SID) 作为签名过程的一部分包括在内。  
  
> [!CAUTION]
> 模块签名只应用于授予权限，不应用于拒绝或撤消权限。  
  
 无法对数据定义语言 (DDL) 触发器和内联表值函数进行签名。  
  
 可以在 sys.crypt_properties 目录视图中看到有关签名的信息。  
  
> [!WARNING]
>  当为签名重新创建过程时，原始批处理中的所有语句必须与重新创建批处理相匹配。 如果批处理的任何部分有所不同，即使是空格或注释不同，所生成的签名也将会不同。  
  
## <a name="countersignatures"></a>副署  
 当执行签名的模块时，签名将临时添加到 SQL 标记中，但如果该模块执行其他模块或该模块终止执行，则会丢失签名。 副署是一种特殊形式的签名。 副署本身并不授予任何权限，但它允许在对副署的对象进行调用的期间保留同一证书或非对称密钥创建的签名。  
  
 例如，假设用户 Alice 调用过程 ProcForAlice，此过程将调用过程 ProcSelectT1，而后者从表 T1 中进行选择。 Alice 对 ProcForAlice 和 ProcSelectT1 具有 EXECUTE 权限，但它对于 T1 不具备 SELECT 权限，并且在此整个链中不涉及任何所有权链。 Alice 无法访问表 T1，无论是直接访问，还是通过使用 ProcForAlice 和 ProcSelectT1 访问。 因为我们希望 Alice 始终使用 ProcForAlice 进行访问，所以我们不希望授予其执行 ProcSelectT1 的权限。 如何实现呢？  
  
-   如果我们对 ProcSelectT1 进行签名，以便 ProcSelectT1 可以访问 T1，则 Alice 可以直接调用 ProcSelectT1，而不必调用 ProcForAlice。  
  
-   我们可以拒绝授予 Alice 对 ProcSelectT1 的 EXECUTE 权限，但 Alice 便无法通过 ProcForAlice 调用 ProcSelectT1。
  
-   对 ProcForAlice 进行签名本身并不会起作用，因为在调用 ProcSelectT1 时，此签名将丢失。  
  
然而，通过使用用于对 ProcForAlice 进行签名的相同证书来对 ProcSelectT1进行副署，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在整个调用链中保留此签名，并允许访问 T1。 如果 Alice 试图直接调用 ProcSelectT1，她将无法访问 T1，因为副署不授予任何权限。 下面的示例 C 显示了此示例的 [!INCLUDE[tsql](../../includes/tsql-md.md)]。  
  
## <a name="permissions"></a>权限  

需要对对象拥有 ALTER 权限，并且对证书或非对称密钥拥有 CONTROL 权限。 如果关联的私钥受密码保护，则用户还必须具有相应的密码。  
  
## <a name="examples"></a>示例  
  
### <a name="a-signing-a-stored-procedure-by-using-a-certificate"></a>A. 通过使用证书对存储过程进行签名

 以下示例使用证书 `HumanResources.uspUpdateEmployeeLogin` 对存储过程 `HumanResourcesDP` 进行签名。  
  
```sql  
USE AdventureWorks2012;  
ADD SIGNATURE TO HumanResources.uspUpdateEmployeeLogin   
    BY CERTIFICATE HumanResourcesDP;  
GO  
```  
  
### <a name="b-signing-a-stored-procedure-by-using-a-signed-blob"></a>B. 通过使用已签名的 BLOB 对存储过程进行签名

下面的示例创建一个新数据库，并创建要在示例中使用的证书。 该示例创建一个简单的存储过程并且对该存储过程进行签名，并且从 `sys.crypt_properties` 检索该签名 BLOB。 最后，将删除并再次添加该签名。 该示例通过使用 WITH SIGNATURE 语法对该工程进行签名。  
  
```sql  
CREATE DATABASE TestSignature ;  
GO  
USE TestSignature ;  
GO  
-- Create a CERTIFICATE to sign the procedure.  
CREATE CERTIFICATE cert_signature_demo   
    ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
    WITH SUBJECT = 'ADD SIGNATURE demo';  
GO  

-- Create a simple procedure.  
CREATE PROC [sp_signature_demo]  
AS  
    PRINT 'This is the content of the procedure.' ;  
GO  
-- Sign the procedure.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]   
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y' ;  
GO  

-- Get the signature binary BLOB for the sp_signature_demo procedure.  
SELECT cp.crypt_property  
    FROM sys.crypt_properties AS cp  
    JOIN sys.certificates AS cer  
        ON cp.thumbprint = cer.thumbprint  
    WHERE cer.name = 'cert_signature_demo' ;  
GO  
```  
  
 每次您创建一个过程时，由该语句返回的 `crypt_property` 签名都将不同。 请记录结果以便在本示例的后面使用。 对于本示例，演示的结果是：`0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373`。  
  
```sql  
-- Drop the signature so that it can be signed again.  
DROP SIGNATURE FROM [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo];  
GO  

-- Add the signature. Use the signature BLOB obtained earlier.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]  
    WITH SIGNATURE = 0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373 ;  
GO  
```  
  
### <a name="c-accessing-a-procedure-using-a-countersignature"></a>C. 使用副署访问过程

下面的示例说明副署如何帮助控制对于对象的访问。  
  
```sql  
-- Create tesT1 database  
CREATE DATABASE testDB;  
GO  
USE testDB;  
GO  
-- Create table T1  
CREATE TABLE T1 (c VARCHAR(11));  
INSERT INTO T1 VALUES ('This is T1.');  
  
-- Create a TestUser user to own table T1  
CREATE USER TestUser WITHOUT LOGIN;  
ALTER AUTHORIZATION ON T1 TO TestUser;  
  
-- Create a certificate for signing  
CREATE CERTIFICATE csSelectT  
  ENCRYPTION BY PASSWORD = 'SimplePwd01'  
  WITH SUBJECT = 'Certificate used to grant SELECT on T1';  
CREATE USER ucsSelectT1 FROM CERTIFICATE csSelectT;  
GRANT SELECT ON T1 TO ucsSelectT1;  
  
-- Create a principal with low privileges  
CREATE LOGIN Alice WITH PASSWORD = 'SimplePwd01';  
CREATE USER Alice;  
  
-- Verify Alice cannoT1 access T1;  
EXECUTE AS LOGIN = 'Alice';  
    SELECT * FROM T1;  
REVERT;  
  
-- Create a procedure that directly accesses T1  
CREATE PROCEDURE procSelectT1 AS  
BEGIN  
    PRINT 'Now selecting from T1...';  
    SELECT * FROM T1;  
END;  
GO  
GRANT EXECUTE ON ProcSelectT1 to public;  
  
-- Create special procedure for accessing T1  
CREATE PROCEDURE  ProcForAlice AS  
BEGIN  
   IF USER_ID() <> USER_ID('Alice')  
    BEGIN  
        PRINT 'Only Alice can use this.';  
        RETURN  
    END  
   EXEC ProcSelectT1;  
END;  
GO;  
GRANT EXECUTE ON ProcForAlice TO PUBLIC;  
  
-- Verify procedure works for a sysadmin user  
EXEC ProcForAlice;  
  
-- Alice still can't use the procedure yet  
EXECUTE AS LOGIN = 'Alice';  
    EXEC ProcForAlice;  
REVERT;  
  
-- Sign procedure to grant it SELECT permission  
ADD SIGNATURE TO ProcForAlice BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Counter sign ProcSelectT1, to make this work  
ADD COUNTER SIGNATURE TO ProcSelectT1 BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Now the proc works.   
-- Note that calling ProcSelectT1 directly still doesn't work  
EXECUTE AS LOGIN = 'Alice';  
    EXEC ProcForAlice;  
    EXEC ProcSelectT1;  
REVERT;  
  
-- Cleanup  
USE master;  
GO  
DROP DATABASE testDB;  
DROP LOGIN Alice;  
```  
  
## <a name="see-also"></a>另请参阅

- [sys.crypt_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)
- [DROP SIGNATURE (Transact-SQL)](../../t-sql/statements/drop-signature-transact-sql.md)
