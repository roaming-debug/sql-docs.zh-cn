---
title: 在适用于 SQL Server 的 PHP 驱动程序中使用 Always Encrypted
description: 了解如何在 PHP Drivers for SQL Server 中使用 Always Encrypted 来保护应用程序中的敏感数据。
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-kaywon
ms.author: v-daenge
author: David-Engel
manager: v-mabarw
ms.openlocfilehash: faa8974dd8ab7201158592c50288c31e7656e5f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058162"
---
# <a name="using-always-encrypted-with-the-php-drivers-for-sql-server"></a>在适用于 SQL Server 的 PHP 驱动程序中使用 Always Encrypted
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>适用于
 -   Microsoft Drivers 5.2 for PHP for SQL Server
 
## <a name="introduction"></a>简介

本文介绍了如何使用 [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)和 [PHP Driver for SQL Server](../../connect/php/Microsoft-php-driver-for-sql-server.md) 来开发 PHP 应用程序。

始终加密允许客户端应用程序对敏感数据进行加密，并且永远不向 SQL Server 或 Azure SQL 数据库显示该数据或加密密钥。 启用了 Always Encrypted 的驱动程序（如适用于 SQL Server 的 ODBC 驱动程序）在客户端应用程序中以透明方式对敏感数据进行加密和解密。 该驱动程序自动确定哪些查询参数与敏感数据库列（使用始终加密进行保护）相对应，并对这些参数的值进行加密，然后再将数据传递到 SQL Server 或 Azure SQL 数据库。 同样，该驱动程序以透明方式对查询结果中从加密数据库列检索到的数据进行解密。 有关详细信息，请参阅 [始终加密（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)。 PHP Driver for SQL Server 利用 ODBC Driver for SQL Server 来加密敏感数据。

## <a name="prerequisites"></a>先决条件

 -   在数据库中配置始终加密。 此配置涉及为选定数据库列预配 Always Encrypted 密钥和设置加密。 如果还没有配置了始终加密的数据库，请按照 [始终加密入门](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted)中的说明操作。 尤其要注意的是，数据库应包含列主密钥 (CMK)、列加密密钥 (CEK) 和包含一个或多个使用该 CEK 加密的表的元数据定义。
 -   确保在开发计算机上安装 ODBC Driver for SQL Server 版本 17 或更高版本。 有关详细信息，请参阅 [ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)。

## <a name="enabling-always-encrypted-in-a-php-application"></a>在 PHP 应用程序中启用 Always Encrypted

若要对面向加密列的参数启用加密，以及对查询结果启用解密，最简单的方法是将 `ColumnEncryption` 连接字符串关键字的值设置为 `Enabled`。 下面是在 SQLSRV 和 PDO_SQLSRV 驱动程序中启用 Always Encrypted 的示例：

SQLSRV：
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled");
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV：
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

为了成功实现加密或解密，单单启用 Always Encrypted 还不够；还需要确保：
 -   应用程序具有“查看任意列主密钥定义”和“查看任意列加密密钥定义”数据库权限，这是访问数据库中 Always Encrypted 密钥的相关元数据所必需的权限。 有关详细信息，请参阅[数据库权限](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions)。
 -   应用程序可以访问 CMK，以保护已查询的加密列的 CEK。 此要求取决于存储 CMK 的密钥存储提供程序。 有关详细信息，请参阅[使用列主密钥存储](#working-with-column-master-key-stores)。

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>检索和修改加密列中的数据

对连接启用 Always Encrypted 后，可以使用标准 SQLSRV API（请参阅 [SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)）或 PDO_SQLSRV API（请参阅 [PDO_SQLSRV 驱动程序 API 参考](../../connect/php/pdo-sqlsrv-driver-reference.md)），来检索或修改加密数据库列中的数据。 假设应用程序拥有必需的数据库权限，并能访问列主密钥，且驱动程序负责加密任何定目标到加密列的查询参数，并解密从加密列检索到的数据，对应用程序的行为方式透明，就像列未加密一样。

如果未启用 Always Encrypted，具有面向加密列的参数的查询将失败。 只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。 不过，驱动程序不会尝试进行任何解密，并且应用程序会收到二进制加密数据（字节数组形式）。

下表概述了查询的行为，具体取决于是否启用了 Always Encrypted：

|查询特征|启用了始终加密，并且应用程序可以访问密钥和密钥元数据|启用了始终加密，但应用程序无法访问密钥或密钥元数据|禁用了始终加密|
|---|---|---|---|
|面向加密列的参数。|以透明方式加密参数值。|错误|错误|
|从加密列中检索数据，且没有面向加密列的参数。|以透明方式解密来自加密列的结果。 应用程序收到纯文本列值。 |错误|不解密来自加密列的结果。 应用程序收到字节数组形式的加密值。|
 
以下示例说明如何检索和修改加密列中的数据。 这些示例假定存在具有以下架构的表。 SSN 和 BirthDate 列已加密。
```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="data-insertion-example"></a>数据插入示例

下面的示例演示如何使用 SQLSRV 和 PDO_SQLSRV 驱动程序向患者表中插入行。 请注意以下几点：
 -   对于示例代码中的加密，没有什么特定的注意事项。 驱动程序自动检测并加密定目标到加密列的 SSN 和 BirthDate 参数的值。 该机制使得加密操作对应用程序而言是透明的。
 -   插入到数据库列（包括加密列）中的值将作为绑定参数传递。 在将值发送到非加密列时，可以选择使用参数（强烈建议使用它，因为它有助于防止 SQL 注入），而在发送面向加密列的值时，必须使用该参数。 若将插入 SSN 或 BirthDate 列的值传递为嵌入查询声明的文本，查询将失败，因为驱动程序不会尝试加密或处理查询中的文本。 因此，服务器会因为与加密列不兼容而拒绝它们。
 -   当使用绑定参数插入值时，必须将与目标列的数据类型相同或支持将其转换为目标列的数据类型的 SQL 类型传递给数据库。 此要求是因为 Always Encrypted 支持很少的类型转换（有关详细信息，请参阅 [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)）。 这两个 PHP 驱动程序（SQLSRV 和 PDO_SQLSRV）都有一种机制，可帮助用户确定值的 SQL 类型。 因此，用户不必显式提供 SQL 类型。
  -   对于 SQLSRV 驱动程序，用户有两个选项：
   -   依赖于 PHP 驱动程序来确定和设置正确的 SQL 类型。 在这种情况下，用户必须使用 `sqlsrv_prepare` 和 `sqlsrv_execute` 来执行参数化查询。
   -   显式设置 SQL 类型。
  -   对于 PDO_SQLSRV 驱动程序，用户不能选择显式设置参数的 SQL 类型。 PDO_SQLSRV 驱动程序会在绑定参数时自动帮助用户确定 SQL 类型。
 -   为了使驱动程序确定 SQL 类型，有一些限制适用：
  -   SQLSRV 驱动程序：
   -   如果用户希望驱动程序确定加密列的 SQL 类型，则用户必须使用 `sqlsrv_prepare` 和 `sqlsrv_execute`。
   -   如果首选 `sqlsrv_query`，则用户负责指定所有参数的 SQL 类型。 指定的 SQL 类型必须包含字符串类型的字符串长度以及十进制类型的数值范围和精度。
  -   PDO_SQLSRV 驱动程序：
   -   参数化查询不支持语句属性 `PDO::SQLSRV_ATTR_DIRECT_QUERY`。
   -   参数化查询不支持语句属性 `PDO::ATTR_EMULATE_PREPARES`。
   
SQLSRV 驱动程序和 [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md)：
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel;
$birthDate = "1996-10-19";
$params = array($ssn, $firstName, $lastName, $birthDate);
// during sqlsrv_prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = sqlsrv_prepare($conn, $query, $params);
sqlsrv_execute($stmt);
```

SQLSRV 驱动程序和 [sqlsrv_query](../../connect/php/sqlsrv-query.md)：
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel";
$birthDate = "1996-10-19";
// need to provide the SQL types for ALL parameters  
// note the SQL types (including the string length) have to be the same at the column definition
$params = array(array(&$ssn, null, null, SQLSRV_SQLTYPE_CHAR(11)),
                array(&$firstName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$lastName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$birthDate, null, null, SQLSRV_SQLTYPE_DATE));
sqlsrv_query($conn, $query, $params);
```

PDO_SQLSRV 驱动程序和 [PDO::prepare](../../connect/php/pdo-prepare.md)：
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Able";
$birthDate = "1996-10-19";
// during PDO::prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
$stmt->bindParam(2, $firstName);
$stmt->bindParam(3, $lastName);
$stmt->bindParam(4, $birthDate);
$stmt->execute();
```

### <a name="plaintext-data-retrieval-example"></a>纯文本数据检索示例

以下示例演示如何使用 SQLSRV and PDO_SQLSRV 驱动程序，根据加密值筛选数据，以及从加密列中检索纯文本数据。 请注意以下几点：
 -   WHERE 子句中用于筛选 SSN 列的值需要使用 bind 参数进行传递，以便驱动程序可以在将其发送到数据库之前以透明方式对其加密。
 -   使用绑定参数执行查询时，PHP 驱动程序会自动确定用户的 SQL 类型，除非用户在使用 SQLSRV 驱动程序时显式指定 SQL 类型。
 -   程序打印的所有值均为纯文本形式，因为驱动程序将以透明方式解密从 SSN 和 BirthDate 列中检索到的数据。
 
注意：只有在加密是确定性的情况下，查询才能对加密列执行相等比较。 有关详细信息，请参阅[选择确定性加密或随机加密](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)。

SQLSRV：
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = sqlsrv_prepare($conn, $query, array(&$ssn));
// during sqlsrv_execute, the driver encrypts the ssn value and passes it to the database
sqlsrv_execute($stmt);
// fetch like usual
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV：
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
// during PDOStatement::execute, the driver encrypts the ssn value and passes it to the database
$stmt->execute();
// fetch like usual
$row = $stmt->fetch();
```

### <a name="ciphertext-data-retrieval-example"></a>已加密文本数据检索示例

如果未启用始终加密，只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。

以下示例说明如何使用 SQLSRV 和 PDO_SQLSRV 驱动程序从加密列中检索二进制加密数据。 请注意以下几点：
 -   由于未在连接字符串中启用 Always Encrypted，因此，查询将以字节数组的形式返回 SSN 和 BirthDate 的加密值（程序会将值转换为字符串）。
 -   如果禁用始终加密，从加密列中检索数据的查询可以有参数，但前提是所有参数均不面向加密列。 以下查询按未在数据库中加密的 LastName 进行筛选。 如果查询按 SSN 或 BirthDate 进行筛选，则查询将失败。
 
SQLSRV：
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = sqlsrv_prepare($conn, $query, array(&$lastName));
sqlsrv_execute($stmt);
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV：
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $lastName);
$stmt->execute();
$row = $stmt->fetch();
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>避免查询加密列时的常见问题

本节介绍从 PHP 应用程序查询加密列时的常见错误类别，以及有关如何避免这些错误的若干指导。

#### <a name="unsupported-data-type-conversion-errors"></a>不支持的数据类型转换错误

始终加密支持对加密数据类型进行若干种转换。 有关受支持类型转换的详细列表，请参阅 [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)。 执行以下操作可避免数据类型转换错误：
 -   将 SQLSRV 驱动程序与 `sqlsrv_prepare` 和 `sqlsrv_execute` 一起使用时，会自动确定 SQL 类型、列大小和参数的小数位数。
 -   使用 PDO_SQLSRV 驱动程序执行查询时，还会自动确定 SQL 类型、列大小和参数的小数位数
 -   将 SQLSRV 驱动程序与 `sqlsrv_query` 一起使用来执行查询时：
  -   此参数的 SQL 类型与目标列的类型完全相同，或支持从 SQL 列转换为此列的类型。
  -   对于面向列的 `decimal` 和 `numeric` SQL Server 数据类型的参数，其精度和小数位数与为目标列配置的精度和小数位数相同。
  -   在用于修改目标列的查询中，面向列的 `datetime2`、`datetimeoffset` 或 `time` SQL Server 数据类型的参数的精度不大于目标列的精度。
 -   不要在参数化查询中使用 PDO_SQLSRV 语句属性 `PDO::SQLSRV_ATTR_DIRECT_QUERY` 或 `PDO::ATTR_EMULATE_PREPARES`
 
#### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>由于传递纯文本而非加密值而发生的错误

面向加密列的任何值都需要先完成加密，然后才能发送给服务器。 尝试插入、修改或者按纯文本值筛选加密列会导致错误。 为防止发生此类错误，请确保：
 -   启用 Always Encrypted（在连接字符串中，将 `ColumnEncryption` 关键字设置为 `Enabled`）。
 -   使用 bind 参数发送面向加密列的数据。 以下示例显示了一个查询，该查询按文本/常量对加密列 (SSN) 进行错误筛选：
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

## <a name="controlling-performance-impact-of-always-encrypted"></a>控制始终加密对性能的影响

Always Encrypted 是一种客户端加密技术，因此，大部分性能开销发生在客户端，而不是数据库中。 除加密和解密操作的成本之外，客户端上的其他性能开销来源包括：
 -   额外往返数据库以检索查询参数的元数据。
 -   调用列主密钥存储以访问列主密钥。
 
### <a name="round-trips-to-retrieve-metadata-for-query-parameters"></a>为了检索查询参数的元数据而往返的次数

如果为连接启用了 Always Encrypted，默认情况下，ODBC 驱动程序将为每个参数化查询调用 [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)，并将查询语句（不带任何参数值）传递到 SQL Server。 此存储过程会分析查询语句，以了解是否有任何参数需要加密；如果有，则会针对每个参数返回加密相关信息，以便驱动程序对它们加密。

由于 PHP 驱动程序允许用户在未提供 SQL 类型的情况下绑定预定义语句中的参数，因此，在启用 Always Encrypted 的连接中绑定参数时，PHP 驱动程序会在参数上调用 [SQLDescribeParam](../../odbc/reference/syntax/sqldescribeparam-function.md)，以获取 SQL 类型、列大小和小数位数。 然后使用元数据调用 [SQLBindParameter]( ../../odbc/reference/syntax/sqlbindparameter-function.md)。 这些额外的 `SQLDescribeParam` 调用不需要对数据库进行额外的往返，因为 ODBC 驱动程序已在调用 `sys.sp_describe_parameter_encryption` 时在客户端上存储了信息。

上述行为可确保对客户端应用程序（和应用程序开发人员）高度透明，应用程序不需要知道哪些查询访问加密列，但前提是定目标到加密列的值在参数中传递到驱动程序。

与 ODBC Driver for SQL Server 不同的是，PHP 驱动程序尚不支持在语句/查询级别启用 Always Encrypted。 

### <a name="column-encryption-key-caching"></a>列加密密钥缓存

为了减少对列主密钥存储的调用次数以解密列加密密钥 (CEK)，驱动程序在内存中缓存纯文本 CEK。 从数据库元数据收到加密 CEK (ECEK) 后，ODBC 驱动程序会先尝试查找与缓存中的加密密钥值对应的纯文本 CEK。 只有在驱动程序无法从缓存中找到对应的纯文本 CEK 时，才会调用包含 CMK 的密钥存储。

注意：在 ODBC Driver for SQL Server 中，缓存中的项在两小时的超时过后被收回。 这种行为意味着，对于给定 ECEK，驱动程序在应用程序生存期内或每两小时（以较短持续时间为准）只联系密钥存储一次。

## <a name="working-with-column-master-key-stores"></a>使用列主密钥存储

驱动程序需要获取为目标列配置的 CEK，才能加密或解密数据。 CEK 以加密形式 (ECEK) 存储在数据库元数据中。 每个 CEK 均有一个用于加密其自身的相应 CMK。 [数据库元数据](../../t-sql/statements/create-column-master-key-transact-sql.md)本身不存储 CMK；它只包含密钥存储的名称，以及密钥存储可用于查找 CMK 的信息。

为了获取 ECEK 的纯文本值，驱动程序会先获取 CEK 及其相应 CMK 的元数据，再使用此信息联系包含 CMK 的密钥储存，并请求它解密 ECEK。 驱动程序使用密钥存储提供程序与密钥存储通信。

对于 Microsoft Driver 5.3.0 for PHP for SQL Server，只支持 Windows 证书存储提供程序和 Azure Key Vault。 尚不支持 ODBC 驱动程序支持的其他密钥存储提供程序（自定义密钥存储提供程序）。

### <a name="using-the-windows-certificate-store-provider"></a>使用 Windows 证书存储提供程序

适用于 Windows 上的 SQL Server 的 ODBC 驱动程序包含用于 Windows 证书存储的内置列主密钥存储提供程序 `MSSQL_CERTIFICATE_STORE`。 （此提供程序在 macOS 或 Linux 上不可用。）此提供程序会将 CMK 存储在客户端计算机本地，应用程序无需额外配置即可将它与驱动程序配合使用。 但是，应用程序必须有权访问存储中的证书及其私钥。 有关详细信息，请参阅 [创建并存储列主密钥 (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

### <a name="using-azure-key-vault"></a>使用 Azure Key Vault

Azure Key Vault 提供了一种使用 Azure 存储加密密钥、密码和其他密码的方法，并且可以用于存储 Always Encrypted 密钥。 ODBC Driver for SQL Server（版本 17 及更高版本）包含用于 Azure Key Vault 的内置主密钥存储提供程序。 以下连接选项处理 Azure Key Vault 配置：`KeyStoreAuthentication`、`KeyStorePrincipalId` 和 `KeyStoreSecret`。 
 -   `KeyStoreAuthentication` 可以采用以下两个可能的字符串值之一：`KeyVaultPassword` 和 `KeyVaultClientSecret`。 这些值控制与其他两个关键字一起使用的身份验证凭据类型。
 -   `KeyStorePrincipalId` 获取一个字符串，该字符串表示用于访问 Azure Key Vault 的帐户的标识符。 
     -   如果 `KeyStoreAuthentication` 设置为 `KeyVaultPassword`，则 `KeyStorePrincipalId` 必须是 Azure ActiveDirectory 用户的名称。
     -   如果 `KeyStoreAuthentication` 设置为 `KeyVaultClientSecret`，则 `KeyStorePrincipalId` 必须是应用程序客户端 ID。
 -   `KeyStoreSecret` 采用表示凭据密码的字符串。 
     -   如果 `KeyStoreAuthentication` 设置为 `KeyVaultPassword`，则 `KeyStoreSecret` 必须是用户的密码。 
     -   如果 `KeyStoreAuthentication` 设置为 `KeyVaultClientSecret`，则 `KeyStoreSecret` 必须是与应用程序客户端 ID 相关联的应用程序机密。

要使用 Azure Key Vault 的连接字符串中必须存在所有三个选项。 此外，`ColumnEncryption` 必须设置为 `Enabled`。 如果 `ColumnEncryption` 设置为 `Disabled`，但 Azure Key Vault 选项存在，则脚本将无错误地继续运行，但不会执行任何加密。

下面的示例演示如何使用 Azure Key Vault 连接到 SQL Server。

SQLSRV：

使用 Azure Active Directory 帐户：
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultPassword", "KeyStorePrincipalId"=>$AADUsername, "KeyStoreSecret"=>$AADPassword);
$conn = sqlsrv_connect($server, $connectionInfo);
```
使用 Azure 应用程序客户端 ID 和密码：
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultClientSecret", "KeyStorePrincipalId"=>$applicationClientID, "KeyStoreSecret"=>$applicationClientSecret);
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV：使用 Azure Active Directory 帐户：
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultPassword; KeyStorePrincipalId = $AADUsername; KeyStoreSecret = $AADPassword;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```
使用 Azure 应用程序客户端 ID 和密码：
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultClientSecret; KeyStorePrincipalId = $applicationClientID; KeyStoreSecret = $applicationClientSecret;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

## <a name="limitations-of-the-php-drivers-when-using-always-encrypted"></a>使用 Always Encrypted 时的 PHP 驱动程序限制

SQLSRV 和 PDO_SQLSRV：
 -   Linux/macOS 不支持 Windows 证书存储提供程序
 -   强制实施参数加密
 -   在语句级别启用 Always Encrypted 
 -   在 Linux 和 macOS 上使用 Always Encrypted 功能和非 UTF8 区域设置时（例如“en_US.ISO-8859-1”），除非系统上已安装代码页 1252，否则将 null 数据或空字符串插入加密的 char(n) 列可能不起作用
 
仅限 SQLSRV：
 -   无需指定 SQL 类型即可使用 `sqlsrv_query` 绑定参数
 -   使用 `sqlsrv_prepare` 在一批 SQL 语句中绑定参数  
 
仅限 PDO_SQLSRV：
 -   在参数化查询中指定的语句属性 `PDO::SQLSRV_ATTR_DIRECT_QUERY`
 -   在参数化查询中指定的语句属性 `PDO::ATTR_EMULATE_PREPARE`
 -   在一批 SQL 语句中绑定参数
 
PHP 驱动程序还继承了 ODBC Driver for SQL Server 和数据库施加的限制。 请参阅[使用 Always Encrypted 时的 ODBC 驱动程序限制](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)和 [Always Encrypted 功能详细信息](../../relational-databases/security/encryption/always-encrypted-database-engine.md#feature-details)。  
  
## <a name="see-also"></a>另请参阅  
[PHP SQL 驱动程序编程指南](../../connect/php/programming-guide-for-php-sql-driver.md)

[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)  

[PDO_SQLSRV 驱动程序 API 参考](../../connect/php/pdo-sqlsrv-driver-reference.md)  
  
