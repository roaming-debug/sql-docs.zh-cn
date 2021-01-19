---
title: 远程 Blob 存储 (RBS) (SQL Server) | Microsoft Docs
description: SQL Server 远程 BLOB 存储 (RBS) 将二进制大型对象存储在商用存储中，而不是存储在主数据库服务器上。 了解此附加产品组件。
ms.custom: ''
ms.date: 11/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- Remote Blob Store (RBS) [SQL Server]
- RBS (Remote Blob Store) [SQL Server]
ms.assetid: 31c947cf-53e9-4ff4-939b-4c1d034ea5b1
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: bfd234025664e7508d7d9cf942ff81a216b447a2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171749"
---
# <a name="remote-blob-store-rbs-sql-server"></a>远程 Blob 存储区 (RBS) (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 远程 BLOB 存储区 (RBS) 是一个可选的附加组件，它允许数据库管理员在商用存储解决方案中存储二进制大型对象，而不是直接存储在主数据库服务器上。  
  
 RBS 包括在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质上，但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序不安装它。 在安装媒体上搜索 RBS 以找到安装文件。

 如果没有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装媒体，可以在以下位置之一下载 RBS：

| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本 | RBS 下载位置 |
|:---|:---|
| [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] | [[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 功能包](https://www.microsoft.com/download/details.aspx?id=56833) |
| [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] | [[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 功能包](https://www.microsoft.com/download/details.aspx?id=55992) |
| [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] | [[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] RBS 下载页面](https://go.microsoft.com/fwlink/?linkid=2109005) |
| &nbsp; | &nbsp; |
  
 
  
## <a name="why-rbs"></a>为什么选择 RBS？  
  
### <a name="optimized-database-storage-and-performance"></a>优化的数据库存储和性能  
 在数据库中存储 BLOB 可能会占用大量文件空间和昂贵的服务器资源。 RBS 可以将 BLOB 传输到你选择的专用存储解决方案，并在数据库中存储对 BLOB 的引用。 这样可以释放服务器存储空间用于结构化数据，释放服务器资源用于进行数据库操作。  
  
### <a name="efficient-blob-management"></a>高效 BLOB 管理  
 有好几项 RBS 功能都支持存储的 BLOB 管理：  
  
-   BLOB 是通过 ACID（原子性、一致性、隔离性和持久性）事务管理的。  
  
-   BLOB 组织成集合。  
  
-   包括垃圾收集、一致性检查和其他维护功能。  
  
### <a name="standardized-api"></a>标准化 API  
 RBS 定义一组 API，可为应用程序提供用于访问和修改任何 BLOB 存储的标准化编程模型。 每个 BLOB 存储区都可以指定自己的提供程序库（该库嵌入到 RBS 客户端库），并指定存储和访问 BLOB 的方式。  
  
 许多第三方存储解决方案供应商都开发了符合这些标准 API、在不同存储平台上支持 BLOB 存储的 RBS 提供程序。  
  
## <a name="rbs-requirements"></a>RBS 要求  
 - 对于存储 BLOB 元数据的主数据库服务器，RBS 要求安装有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise。  但是，如果使用提供的 FILESTREAM 提供程序，则可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard 上存储 BLOB 本身。 若要连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，RBS 要求对 [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] 至少使用 ODBC 驱动程序版本 11，对 [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]至少使用 ODBC 驱动程序版本 13。 驱动程序可在 [下载 ODBC Driver for SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md)中获得。    
  
 RBS 包括 FILESTREAM 提供程序，因此，您可以使用 RBS 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例上存储 BLOB。 如果要使用 RBS 在其他存储解决方案中存储 BLOB，则必须使用为该存储解决方案开发的第三方 RBS 提供程序，或者使用 RBS API 开发自定义 RBS 提供程序。 [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190)上以学习资源的形式提供了在 NTFS 文件系统中存储 BLOB 的示例提供程序。  
  
## <a name="rbs-security"></a>RBS 安全性  
 SQL 远程 Blob 存储团队博客是有关此功能的一个非常好的信息源。 [RBS 安全模型](/archive/blogs/sqlrbs/rbs-security-model)一文中介绍了 RBS 安全模型。  
  
### <a name="custom-providers"></a>自定义提供程序  
 使用自定义提供程序在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]外部存储 BLOB 时，请确保使用适合于自定义提供程序所使用的存储介质的权限和加密选项保护存储的 BLOB。  
  
### <a name="credential-store-symmetric-key"></a>凭据存储对称密钥  
 如果提供程序需要设置和使用凭据存储中存储的密码，RBS 将使用对称密钥加密提供程序密码，客户端可能会使用该密码获取对提供程序 blob 存储的授权。  
  
-   RBS 2016 使用 **AES_128** 对称密钥。 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 不允许创建新的 **TRIPLE_DES** 密钥，但为实现向后兼容的情况除外。 有关详细信息，请参阅 [CREATE SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/create-symmetric-key-transact-sql.md)。  
  
-   RBS 2014 及以前的版本使用凭据存储，其中包含使用已过时的 **TRIPLE_DES** 对称密钥算法加密的密码。 如果当前正在使用 **TRIPLE_DES**，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议你遵循本主题中的步骤将密钥轮换为更强的加密方法，以增强安全性。  
  
 你可以通过在 RBS 数据库中执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句来确定 RBS 凭据存储对称密钥属性：   
`SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';` 如果该语句的输出显示仍在使用 **TRIPLE_DES** ，则应轮换此密钥。  
  
### <a name="rotating-the-symmetric-key"></a>轮换对称密钥  
 使用 RBS 时，应定期轮换凭据存储对称密钥。 这是一种常见的可满足组织安全策略的安全最佳实践。  轮换 RBS 凭据存储对称密钥的方法之一是使用 RBS 数据库中的 [以下脚本](#Key_rotation) 。  你也可以使用此脚本迁移到更强的加密强度属性，如算法或密钥长度。 在轮换密钥前，先备份数据库。  脚本结束时，它有一些验证步骤。  
如果你的安全策略需要与所提供属性不同的密钥属性（例如，算法或密钥长度），可以将该脚本用作模板。 在以下两个位置修改密钥属性：1) 创建临时密钥的位置 2) 创建永久密钥的位置。  
  
##  <a name="rbs-resources"></a><a name="rbsresources"></a> RBS 资源  
  
 **RBS 示例**  
 [Codeplex](https://go.microsoft.com/fwlink/?LinkId=210190) 上提供的 RBS 示例演示如何开发 RBS 应用程序，如何开发和安装自定义 RBS 提供程序。  
  
 **RBS 博客**  
 [RBS 博客](/archive/blogs/sqlrbs/) 提供可帮助你理解、部署和维护 RBS 的其他信息。  
  
##  <a name="key-rotation-script"></a><a name="Key_rotation"></a> 密钥轮换脚本  
 此示例创建一个名为 `sp_rotate_rbs_symmetric_credential_key` 的存储过程，以将当前使用的 RBS 凭据存储对称密钥替换为  
你所选的密钥。  如果安全策略要求定期轮换密钥   
或者有特定的算法要求，你可能想要这样做。  
 在此存储过程中，使用 **AES_256** 的对称密钥将替换当前的密钥。  由于  
替换了对称密钥，因此需要用新密钥对密码重新加密。  此存储   
过程也会对密码重新加密。  在轮换密钥前，应先备份数据库。  
  
```  
CREATE PROC sp_rotate_rbs_symmetric_credential_key  
AS  
BEGIN  
BEGIN TRANSACTION;  
BEGIN TRY  
CLOSE ALL SYMMETRIC KEYS;  
  
/* Prove that all secrets can be re-encrypted, by creating a   
temporary key (#mssqlrbs_encryption_skey) and create a   
temp table (#myTable) to hold the re-encrypted secrets.    
Check to see if all re-encryption worked before moving on.*/  
  
CREATE TABLE #myTable(sql_user_sid VARBINARY(85) NOT NULL,  
    blob_store_id SMALLINT NOT NULL,  
    credential_name NVARCHAR(256) COLLATE Latin1_General_BIN2 NOT NULL,  
    old_secret VARBINARY(MAX), -- holds secrets while existing symmetric key is deleted  
    credential_secret VARBINARY(MAX)); -- holds secrets with the new permanent symmetric key  
  
/* Create a new temporary symmetric key with which the credential store secrets   
can be re-encrypted. These will be used once the existing symmetric key is deleted.*/  
CREATE SYMMETRIC KEY #mssqlrbs_encryption_skey    
    WITH ALGORITHM = AES_256 ENCRYPTION BY   
    CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY #mssqlrbs_encryption_skey    
    DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
INSERT INTO #myTable   
    SELECT cred_store.sql_user_sid, cred_store.blob_store_id, cred_store.credential_name,   
    encryptbykey(  
        key_guid('#mssqlrbs_encryption_skey'),   
        decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
            NULL, cred_store.credential_secret)  
        ),   
    NULL  
    FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials] AS cred_store;  
  
IF( EXISTS(SELECT * FROM #myTable WHERE old_secret IS NULL))  
BEGIN  
    PRINT 'Abort. Failed to read some values';  
    SELECT * FROM #myTable;  
    ROLLBACK;  
END;  
ELSE  
BEGIN  
/* Re-encryption worked, so go ahead and drop the existing RBS credential store   
 symmetric key and replace it with a new symmetric key.*/  
DROP SYMMETRIC KEY [mssqlrbs_encryption_skey];  
  
CREATE SYMMETRIC KEY [mssqlrbs_encryption_skey]   
WITH ALGORITHM = AES_256   
ENCRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY [mssqlrbs_encryption_skey]   
DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
/*Re-encrypt using the new permanent symmetric key.    
Verify if encryption provided a result*/  
UPDATE #myTable   
SET [credential_secret] =   
    encryptbykey(key_guid('mssqlrbs_encryption_skey'), decryptbykey(old_secret))  
  
IF( EXISTS(SELECT * FROM #myTable WHERE credential_secret IS NULL))  
BEGIN  
    PRINT 'Aborted. Failed to re-encrypt some values'  
    SELECT * FROM #myTable  
    ROLLBACK  
END  
ELSE  
BEGIN  
  
/* Replace the actual RBS credential store secrets with the newly   
encrypted secrets stored in the temp table #myTable.*/                
SET NOCOUNT ON;  
DECLARE @sql_user_sid varbinary(85);  
DECLARE @blob_store_id smallint;  
DECLARE @credential_name varchar(256);  
DECLARE @credential_secret varbinary(256);  
DECLARE curSecretValue CURSOR   
    FOR SELECT sql_user_sid, blob_store_id, credential_name, credential_secret   
FROM #myTable ORDER BY sql_user_sid, blob_store_id, credential_name;  
  
OPEN curSecretValue;  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    UPDATE [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        SET [credential_secret] = @credential_secret   
        FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        WHERE sql_user_sid = @sql_user_sid AND blob_store_id = @blob_store_id AND   
            credential_name = @credential_name  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
END  
CLOSE curSecretValue  
DEALLOCATE curSecretValue  
  
DROP TABLE #myTable;  
CLOSE ALL SYMMETRIC KEYS;  
DROP SYMMETRIC KEY #mssqlrbs_encryption_skey;  
  
/* Verify that you can decrypt all encrypted credential store entries using the certificate.*/  
IF( EXISTS(SELECT * FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
WHERE decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
    NULL, credential_secret) IS NULL))  
BEGIN  
    print 'Aborted. Failed to verify key rotation'  
    ROLLBACK;  
END;  
ELSE  
    COMMIT;  
END;  
END;  
END TRY  
BEGIN CATCH  
     PRINT 'Exception caught: ' + cast(ERROR_NUMBER() as nvarchar) + ' ' + ERROR_MESSAGE();  
     ROLLBACK  
END CATCH  
END;  
GO  
```  
  
 现在，你可以使用 `sp_rotate_rbs_symmetric_credential_key` 存储过程轮换 RBS 凭据存储对称密钥，密码在密钥轮换前后保持不变。  
  
```  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
EXEC sp_rotate_rbs_symmetric_credential_key;  
  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
/* See that the RBS credential store symmetric key properties reflect the new changes*/  
SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';  
```  
  
## <a name="see-also"></a>另请参阅  
[远程 Blob 存储和 AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
