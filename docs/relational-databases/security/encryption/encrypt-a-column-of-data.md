---
title: 加密数据列 | Microsoft Docs
description: 了解如何使用 Transact-SQL 通过 SQL Server 中的对称加密来加密数据列，该操作有时称为列级加密或单元级加密。
ms.custom: ''
titleSuffix: SQL Server & Azure Synapse Analytics & Azure SQL Database & SQL Managed Instance
ms.date: 12/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], columns
- cryptography [SQL Server], columns
- column level encryption
- cell level encryption
ms.assetid: 38e9bf58-10c6-46ed-83cb-e2d76cda0adc
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: c09f7f91edf2cada0464b6cfbc1922664a86866f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97640949"
---
# <a name="encrypt-a-column-of-data"></a>加密数据列

[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]  

本文介绍如何使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 在 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 中通过对称加密对数据列进行加密。 这有时称为列级加密或单元级加密。 对于 Azure Synapse Analytics，此功能为预览版

本文中的示例已针对 AdventureWorks2017 进行了验证。 要获取示例数据库，请参阅 [AdventureWorks 示例数据库](../../../samples/adventureworks-install-configure.md)。

## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  

下面的权限是执行以下步骤所必需的：  
  
- 数据库的 `CONTROL` 权限。  
- 数据库的 `CREATE CERTIFICATE` 权限。 只有 Windows 登录名、SQL Server 登录名和应用程序角色才能拥有证书。 其他组和角色不能拥有证书。  
- 表的 `ALTER` 权限。  
- 针对密钥的某些权限，并且必须未被拒绝授予 `VIEW DEFINITION` 权限。  
  
## <a name="create-database-master-key"></a>创建数据库主密钥  

要使用下面的示例，必须具有数据库主密钥。 如果数据库尚不具有数据库主密钥，请创建一个。 若要创建一个主密钥，请连接到数据库，然后运行以下脚本。 请确保使用复杂密码。

将以下示例复制并粘贴到连接到 AdventureWorks 示例数据库的查询窗口中。 单击“执行” 。  

```sql  
CREATE MASTER KEY ENCRYPTION BY   
PASSWORD = '<complex password>';  
```  

请始终备份数据库主密钥。 有关数据库主密钥的详细信息，请参阅 [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md)。

## <a name="example-encrypt-with-symmetric-encryption-and-authenticator"></a>示例：通过对称加密和验证器进行加密
  
1. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2. 在标准菜单栏上，单击 **“新建查询”** 。  
  
3. 将以下示例复制并粘贴到连接到 AdventureWorks 示例数据库的查询窗口中。 单击“执行” 。

    ```sql
    CREATE CERTIFICATE Sales09  
       WITH SUBJECT = 'Customer Credit Card Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY CreditCards_Key11  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE Sales.CreditCard   
        ADD CardNumber_Encrypted varbinary(160);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
  
    -- Encrypt the value in column CardNumber using the  
    -- symmetric key CreditCards_Key11.  
    -- Save the result in column CardNumber_Encrypted.    
    UPDATE Sales.CreditCard  
    SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11')  
        , CardNumber, 1, HASHBYTES('SHA2_256', CONVERT( varbinary  
        , CreditCardID)));  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Now list the original card number, the encrypted card number,  
    -- and the decrypted ciphertext. If the decryption worked,  
    -- the original number will match the decrypted number.  
  
    SELECT CardNumber, CardNumber_Encrypted   
        AS 'Encrypted card number', CONVERT(nvarchar,  
        DecryptByKey(CardNumber_Encrypted, 1 ,   
        HASHBYTES('SHA2_256', CONVERT(varbinary, CreditCardID))))  
        AS 'Decrypted card number' FROM Sales.CreditCard;  
    GO  
    ```  
  
## <a name="encrypt-with-simple-symmetric-encryption"></a>通过简单对称加密进行加密  

1. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2. 在标准菜单栏上，单击 **“新建查询”** 。  
  
3. 将以下示例复制并粘贴到连接到 AdventureWorks 示例数据库的查询窗口中。 单击“执行” 。  
  
    ```sql
     CREATE CERTIFICATE HumanResources037  
       WITH SUBJECT = 'Employee Social Security Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY SSN_Key_01  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    USE [AdventureWorks2012];  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE HumanResources.Employee  
        ADD EncryptedNationalIDNumber varbinary(128);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
  
    -- Encrypt the value in column NationalIDNumber with symmetric   
    -- key SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
    UPDATE HumanResources.Employee  
    SET EncryptedNationalIDNumber = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    -- Now list the original ID, the encrypted ID, and the   
    -- decrypted ciphertext. If the decryption worked, the original  
    -- and the decrypted ID will match.  
    SELECT NationalIDNumber, EncryptedNationalIDNumber   
        AS 'Encrypted ID Number',  
        CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
        AS 'Decrypted ID Number'  
        FROM HumanResources.Employee;  
    GO  
    ```  
  
 有关详细信息，请参阅以下主题：  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY (Transact-SQL)](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
