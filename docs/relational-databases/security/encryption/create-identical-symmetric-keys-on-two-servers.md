---
title: 在两个服务器上创建相同的对称密钥 | Microsoft Docs
description: 了解如何使用 Transact-SQL 在 SQL Server 中两台服务器上创建相同的对称密钥。 这支持在不同的数据库或服务器中进行加密。
ms.custom: ''
ms.date: 05/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- symmetric keys [SQL Server], creating
ms.assetid: a13d0b21-a43b-43c0-9c22-7ba8f3d15e80
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 780944c299548aaa9a5664fef4688c11cdf69646
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250331"
---
# <a name="create-identical-symmetric-keys-on-two-servers"></a>在两个服务器上创建相同的对称密钥
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中的两台不同的服务器上创建相同的对称密钥。 为了对密码进行解密，需要用于加密密码的密钥。 在一个数据库中同时执行加密和解密时，密钥保存在数据库中并可同时用于（取决于权限）加密和解密。 但在不同的数据库或服务器中执行加密和解密时，保存在一个数据库中的密钥不能用于另一数据库。
  
## <a name="before-you-begin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a>限制和局限  
  
- 创建对称密钥时，必须至少使用以下项之一来对该对称密钥进行加密：证书、密码、对称密钥、非对称密钥或 PROVIDER。 可使用上述每种类型中的多项对密钥进行加密。 换言之，可以同时使用多个证书、密码、对称密钥以及非对称密钥对单个对称密钥进行加密。  
  
- 当使用密码（而不是数据库主密钥的公钥）对对称密钥进行加密时，便会使用 TRIPLE DES 加密算法。 因此，用强加密算法（如 AES）创建的密钥本身受较弱算法的保护。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求对数据库具有 ALTER ANY SYMMETRIC KEY 权限。 如果指定了 AUTHORIZATION，则要求对数据库用户具有 IMPERSONATE 权限，或者对应用程序角色具有 ALTER 权限。 如果使用证书或非对称密钥进行加密，则要求对证书或非对称密钥具有 VIEW DEFINITION 权限。 只有 Windows 登录名、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名和应用程序角色才能拥有对称密钥。 其他组和角色不能拥有对称密钥。  
  
## <a name="using-transact-sql"></a>“使用 Transact-SQL”  
  
### <a name="to-create-identical-symmetric-keys-on-two-different-servers"></a>在两台不同的服务器上创建相同的对称密钥  
  
1. 在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2. 在标准菜单栏上，单击 **“新建查询”** 。  
  
3. 通过运行以下 CREATE MASTER KEY、CREATE CERTIFICATE 和 CREATE SYMMETRIC KEY 语句来创建密钥。  
  
    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'My p@55w0Rd';  
    GO  
    CREATE CERTIFICATE [cert_keyProtection] WITH SUBJECT = 'Key Protection';  
    GO  
    CREATE SYMMETRIC KEY [key_DataShare] WITH  
        KEY_SOURCE = 'My key generation bits. This is a shared secret!',  
        ALGORITHM = AES_256,   
        IDENTITY_VALUE = 'Key Identity generation bits. Also a shared secret'  
        ENCRYPTION BY CERTIFICATE [cert_keyProtection];  
    GO  
    ```  
  
4. 连接到一个独立的服务器实例，打开不同的查询窗口，然后运行上述 SQL 语句来在另一台服务器上创建相同的密钥。  
  
5. 先在第一台服务器上运行以下 OPEN SYMMETRIC KEY 语句和 SELECT 语句，以测试密钥。  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    SELECT encryptbykey(key_guid('key_DataShare'), 'MyData' )  
    GO  
    -- For example, the output might look like this: 0x2152F8DA8A500A9EDC2FAE26D15C302DA70D25563DAE7D5D1102E3056CE9EF95CA3E7289F7F4D0523ED0376B155FE9C3  
    ```  
  
6. 在另一服务器上，将上一个 SELECT 语句的结果作为 `@blob` 的值粘贴到以下代码中，并运行以下代码以验证复制的密钥可对密码进行解密。  
  
    ```sql
    OPEN SYMMETRIC KEY [key_DataShare]   
        DECRYPTION BY CERTIFICATE cert_keyProtection;  
    GO  
    DECLARE @blob varbinary(8000);  
    SET @blob = SELECT CONVERT(varchar(8000), decryptbykey(@blob));  
    GO  
    ```  
  
7. 在两个服务器上关闭对称密钥。  
  
    ```sql
    CLOSE SYMMETRIC KEY [key_DataShare];  
    GO  
    ```  

### <a name="encryption-changes-in-sql-server-2017-cu2"></a>SQL Server 2017 CU2 中的加密更改

QL Server 2016 使用 SHA1 哈希算法进行其加密工作。 从 SQL Server 2017 开始，改用 SHA2。 这意味着可能需要额外的步骤来获取由 SQL Server 2016 加密的 SQL Server 2017 安装解密项。 额外步骤如下：

- 确保 SQL Server 2017 至少更新到累积更新 2 (CU2)。
  - 有关重要的详细信息，请参阅 [SQL Server 2017 的累积更新 2 (CU2)](https://support.microsoft.com/help/4052574)。
- 安装 CU2 后，打开 SQL Server 2017 中的跟踪标志 4631：`DBCC TRACEON(4631, -1);`
  - 跟踪标志 4631 是 SQL Server 2017 中的新增功能。 跟踪标志 4631 需设置为全局 `ON`，然后才能在 SQL Server 2017 中创建主密钥、证书或对称密钥。 这可确保所创建的这些项能够与 SQL Server 2016 及更低版本进行互操作。

有关更多指导，请参阅：

- [修复：SQL Server 2017 无法使用相同的对称密钥来解密由更早版本的 SQL Server 加密的数据](https://support.microsoft.com/help/4053407/sql-server-2017-cannot-decrypt-data-encrypted-by-earlier-versions)
- [相同的对称密钥无法同时正常作用于 SQL Server 2017 和其他 SQL Server 版本](https://feedback.azure.com/forums/908035-sql-server/suggestions/33116269-identical-symmetric-keys-do-not-work-between-sql-s) <!-- Issue 2225. Thank you Stephen W and Sam Rueby. -->

## <a name="for-more-information"></a>更多信息

-   [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ENCRYPTBYKEY (Transact-SQL)](../../../t-sql/functions/encryptbykey-transact-sql.md)  
  
-   [DECRYPTBYKEY (Transact-SQL)](../../../t-sql/functions/decryptbykey-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY (Transact-SQL)](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
