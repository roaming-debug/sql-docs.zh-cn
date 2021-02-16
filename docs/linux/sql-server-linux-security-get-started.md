---
title: 开始使用 Linux 上的 SQL Server 安全设置
description: 逐步介绍 Linux 上 SQL Server 的安全功能，以了解需要深入了解的领域。
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ecc72850-8b01-492e-9a27-ec817648f0e0
ms.openlocfilehash: f4201aa6c07d4ae96d44d8aa443b23e275e1f9f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346415"
---
# <a name="walkthrough-for-the-security-features-of-sql-server-on-linux"></a>Linux 上的 SQL Server 的安全功能演练

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

如果你是刚接触 SQL Server 的 Linux 用户，以下任务会引导你完成某些安全任务。 虽然这些并非 Linux 独有或特定的任务，但能有助于你了解需要深入了解的领域。 每个示例中均提供了该领域的详细文档链接。

> [!NOTE]
>  以下示例使用 AdventureWorks2014 示例数据库。 有关如何获取并安装此示例数据库的说明，请参阅[将 SQL Server 数据库从 Windows 还原到 Linux](sql-server-linux-migrate-restore-database.md)。


## <a name="create-a-login-and-a-database-user"></a>创建登录名和数据库用户 

通过使用 [CREATE LOGIN](../t-sql/statements/create-login-transact-sql.md) 语句在 master 数据库中创建登录名，为他人授予访问 SQL Server 的权限。 例如：

```
CREATE LOGIN Larry WITH PASSWORD = '************';  
```

> [!NOTE]
>  始终使用强密码代替以上命令中的星号。

登录名可连接到 SQL Server，且能（通过有限权限）访问 master 数据库。 若要连接到用户数据库，登录名需要数据库级别的相应标识，称为数据库用户。 用户特定于每个数据库，且必须在每个要向其授予访问权限的数据库中单独创建。 以下示例会将用户移至 AdventureWorks2014 数据库，然后使用 [CREATE USER](../t-sql/statements/create-user-transact-sql.md) 语句创建名为 Larry 的用户，并将其与名为 Larry 的登录名关联。 尽管该登录名和用户相关（相互映射），但它们是不同的对象。 登录名是服务器级主体。 用户是数据库级主体。

```
USE AdventureWorks2014;
GO
CREATE USER Larry;
GO
```

- SQL Server 管理员帐户可连接到任何数据库，并可在任何数据库中创建更多登录名和用户。  
- 当用户创建数据库时，他们会成为数据库所有者，可连接到该数据库。 数据库所有者可以创建更多用户。

之后，可向其他登录名授予 `ALTER ANY LOGIN` 权限，使其可以创建更多登录名。 在数据库中，可向其他用户授予 `ALTER ANY USER` 权限，使其可以创建更多用户。 例如：   

```
GRANT ALTER ANY LOGIN TO Larry;   
GO   
   
USE AdventureWorks2014;   
GO   
GRANT ALTER ANY USER TO Jerry;    
GO   
```

现在，登录名 Jerry 可以创建更多登录名，用户 Jerry 可以创建更多用户。


## <a name="granting-access-with-least-privileges"></a>授予特权最少的访问权限

第一批连接到用户数据库的用户将是管理员和数据库所有者帐户。 但这些用户具有数据库的所有权限。 其权限要比大多数用户应该拥有的更多。 

在刚开始时，可以使用内置“固定数据库角色”分配某些常规类别的权限。 例如，`db_datareader` 固定数据库角色可读取数据库中的所有表，但不能进行任何更改。 通过使用 [ALTER ROLE](../t-sql/statements/alter-role-transact-sql.md) 语句授予固定数据库角色中的成员身份。 以下示例将用户 `Jerry` 添加到 `db_datareader` 固定数据库角色。   
   
```   
USE AdventureWorks2014;   
GO   
   
ALTER ROLE db_datareader ADD MEMBER Jerry;   
```   

有关固定数据库角色的列表，请参阅[数据库级角色](../relational-databases/security/authentication-access/database-level-roles.md)。

之后，当你准备配置更精确的数据访问权限时（强烈推荐），请使用 [CREATE ROLE](../t-sql/statements/create-role-transact-sql.md) 语句创建自己的用户定义数据库角色。 然后将特定精细级别的权限分配给自定义角色。

例如，以下语句创建名为 `Sales` 的数据库角色，为 `Sales` 组授予从 `Orders` 表查看、更新和删除行的权限，并将用户 `Jerry` 添加到 `Sales` 角色。   
   
```   
CREATE ROLE Sales;   
GRANT SELECT ON Object::Sales TO Orders;   
GRANT UPDATE ON Object::Sales TO Orders;   
GRANT DELETE ON Object::Sales TO Orders;   
ALTER ROLE Sales ADD MEMBER Jerry;   
```

有关权限系统的详细信息，请参阅[数据库引擎权限入门](../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。


## <a name="configure-row-level-security"></a>配置行级别安全性  

利用[行级安全性](../relational-databases/security/row-level-security.md)，你可以根据执行查询的用户限制对数据库中行的访问。 此功能对于确保客户只能访问自己的数据或员工只能访问与其部门相关的数据等方案非常有用。   

以下步骤将逐步介绍如何设置两个对 `Sales.SalesOrderHeader` 表具有不同的行级别访问权限的用户。 

创建两个用户帐户以测试行级别安全性：    
   
```   
USE AdventureWorks2014;   
GO   
   
CREATE USER Manager WITHOUT LOGIN;     
   
CREATE USER SalesPerson280 WITHOUT LOGIN;    
```

向两个用户授予对 `Sales.SalesOrderHeader` 表的读取访问权限：    
   
```   
GRANT SELECT ON Sales.SalesOrderHeader TO Manager;      
GRANT SELECT ON Sales.SalesOrderHeader TO SalesPerson280;
```
   
创建一个新架构和一个内联表值函数。 当 `SalesPersonID` 列中的行与 `SalesPerson` 登录名的 ID 匹配，或者执行查询的用户是 Manager 用户时，函数返回 1。   
   
```     
CREATE SCHEMA Security;   
GO   
   
CREATE FUNCTION Security.fn_securitypredicate(@SalesPersonID AS int)     
    RETURNS TABLE   
WITH SCHEMABINDING   
AS     
   RETURN SELECT 1 AS fn_securitypredicate_result    
WHERE ('SalesPerson' + CAST(@SalesPersonId as VARCHAR(16)) = USER_NAME())     
    OR (USER_NAME() = 'Manager');    
```

创建一个安全策略，将此函数同时作为筛选器谓词和阻止谓词添加到表上：  

```
CREATE SECURITY POLICY SalesFilter   
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader,   
ADD BLOCK PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader   
WITH (STATE = ON);   
```

执行以下代码，以每个用户的身份查询 `SalesOrderHeader` 表。 验证是否 `SalesPerson280` 只看到他们自己销售额的 95 行，而 `Manager` 可以看到表中的所有行。  

```    
EXECUTE AS USER = 'SalesPerson280';   
SELECT * FROM Sales.SalesOrderHeader;    
REVERT; 
 
EXECUTE AS USER = 'Manager';   
SELECT * FROM Sales.SalesOrderHeader;   
REVERT;   
```
 
更改安全策略以禁用策略。  现在两个用户都可以访问所有行。 

```
ALTER SECURITY POLICY SalesFilter   
WITH (STATE = OFF);    
``` 


## <a name="enable-dynamic-data-masking"></a>启用动态数据掩码

利用[动态数据掩码](../relational-databases/security/dynamic-data-masking.md)，你可以通过完全或部分掩蔽特定列来限制对应用程序用户公开敏感数据。 

使用 `ALTER TABLE` 语句将掩码函数添加到 `Person.EmailAddress` 表中的 `EmailAddress` 列： 
 
```
USE AdventureWorks2014;
GO
ALTER TABLE Person.EmailAddress    
ALTER COLUMN EmailAddress    
ADD MASKED WITH (FUNCTION = 'email()');
``` 
 
在表上创建一个具有 `SELECT` 权限的新用户 `TestUser`，然后以 `TestUser` 身份执行查询以查看掩码数据：   

```  
CREATE USER TestUser WITHOUT LOGIN;   
GRANT SELECT ON Person.EmailAddress TO TestUser;    
 
EXECUTE AS USER = 'TestUser';   
SELECT EmailAddressID, EmailAddress FROM Person.EmailAddress;       
REVERT;    
```
 
验证掩蔽函数是否将第一条记录中的电子邮件地址更改为：
  
|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |ken0@adventure-works.com |    
 
更改为 

|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |kXXX@XXXX.com |   


## <a name="enable-transparent-data-encryption"></a>启用透明数据加密

数据库面临的一个威胁是有人会从硬盘中盗取数据库文件。 通过问题员工的操作，或通过盗窃包含文件的计算机（如笔记本电脑），从而通过入侵获得对系统的高级访问权限，就可能发生这种情况。

透明数据加密 (TDE) 对存储在硬盘上的数据文件进行加密。 SQL Server 数据库引擎的 master 数据库具有加密密钥，因此数据库引擎可以操作数据。 如果没有密钥的访问权限，则无法读取数据库文件。 高级管理员可以管理、备份和重新创建密钥，因此可以移动数据库，但只能由选定的人员移动。 配置 TDE 后，`tempdb` 数据库也会自动加密。 

由于数据库引擎可以读取数据，因此透明数据加密无法防止计算机管理员进行未经授权的访问，管理员可以直接读取内存，也可以通过管理员帐户访问 SQL Server。

### <a name="configure-tde"></a>配置 TDE

- 创建主密钥
- 创建或获取由主密钥保护的证书
- 创建数据库加密密钥并通过此证书保护该密钥
- 将数据库设置为使用加密

配置 TDE 需要对 master 数据库具有 `CONTROL` 权限和对用户数据库具有 `CONTROL` 权限。 通常由管理员配置 TDE。 

下面的示例演示如何使用安装在名为 `AdventureWorks2014` 的服务器上的证书加密和解密 `MyServerCert`数据库。


```
USE master;  
GO  

CREATE MASTER KEY ENCRYPTION BY PASSWORD = '**********';  
GO  

CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My Database Encryption Key Certificate';  
GO  

USE AdventureWorks2014;  
GO
  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO
  
ALTER DATABASE AdventureWorks2014  
SET ENCRYPTION ON;   
```

若要删除 TDE，请执行 `ALTER DATABASE AdventureWorks2014 SET ENCRYPTION OFF;`   

加密和解密操作由 SQL Server 安排在后台线程中执行。 您可以使用本主题后面部分显示的列表中的目录视图和动态管理视图查看这些操作的状态。   

> [!WARNING]
>  启用了 TDE 的数据库的备份文件也使用数据库加密密钥进行加密。 因此，当您还原这些备份时，用于保护数据库加密密钥的证书必须可用。 也就是说，除了备份数据库之外，您还要确保自己保留了服务器证书的备份以防数据丢失。 如果证书不再可用，将会导致数据丢失。 有关详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。  

有关 TDE 的详细信息，请参阅[透明数据加密 (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md)。   


## <a name="configure-backup-encryption"></a>配置备份加密
SQL Server 能够在创建备份时加密数据。 通过在创建备份时指定加密算法和加密程序（证书或非对称密钥），可创建加密的备份文件。    
  
> [!WARNING]
> 备份证书或非对称密钥很重要，并且最好备份到与用于加密的备份文件不同的位置。 没有证书或非对称密钥，你将无法还原备份，从而使备份文件无法使用。 
 
 
以下示例创建证书，然后创建受该证书保护的备份。

```
USE master;  
GO  
CREATE CERTIFICATE BackupEncryptCert   
   WITH SUBJECT = 'Database backups';  
GO 
BACKUP DATABASE [AdventureWorks2014]  
TO DISK = N'/var/opt/mssql/backups/AdventureWorks2014.bak'  
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

有关详细信息，请参阅[备份加密](../relational-databases/backup-restore/backup-encryption.md)。


## <a name="next-steps"></a>后续步骤

有关 SQL Server 安全功能的详细信息，请参阅 [SQL Server 数据库引擎和 Azure SQL 数据库的安全中心](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)。