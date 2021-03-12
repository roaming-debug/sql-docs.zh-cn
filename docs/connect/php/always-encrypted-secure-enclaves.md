---
title: 结合使用具有安全 enclave 的 Always Encrypted 与 PHP 驱动程序
description: 如何结合使用具有安全 Enclave 的 Always Encrypted 与 Microsoft Drivers for PHP for SQL Server。
ms.date: 01/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: ''
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: c49d81783f82c41cd95e25b137807b006e5b2ad1
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464703"
---
# <a name="using-always-encrypted-with-secure-enclaves-with-the-php-drivers-for-sql-server"></a>结合使用具有安全 enclave 的 Always Encrypted 与适用于 SQL Server 的 PHP 驱动程序

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>适用于

- Microsoft Drivers 5.8.0 for PHP for SQL Server

## <a name="introduction"></a>简介

[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md) 是 SQL Server Always Encrypted 功能的第二个迭代。 使用具有安全 enclave 的 Always Encrypted，用户可以通过创建安全 enclave（服务器上的一个内存区域，会对其中数据库中的加密数据进行解密，以便执行计算）来对加密数据执行丰富的计算。 支持的操作包括与 `LIKE` 子句的比较和模式匹配。

## <a name="enabling-always-encrypted-with-secure-enclaves"></a>启用具有安全 Enclave 的 Always Encrypted

从 5.8.0 开始，适用于 SQL Server 的 PHP 驱动程序提供对具有安全 enclave 的 Always Encrypted 的支持。 具有安全 enclave 的 Always Encrypted 需要 SQL Server 2019 或更高版本以及 ODBC 驱动程序的 17.4+ 版本。 [此处](using-always-encrypted-php-drivers.md)提供了有关适用于 SQL Server 的 PHP 驱动程序中的 Always Encrypted 的常规要求的详细信息。

具有安全 enclave 的 Always Encrypted 可通过认证 enclave 来确保加密数据的安全性，即根据外部认证服务来验证 enclave。 若要使用安全 enclave，`ColumnEncryption` 关键字必须标识认证类型和协议，以及关联的认证数据（用逗号分隔）。 对于 enclave 类型和协议，17.4 版本的 ODBC 驱动程序仅支持基于虚拟化的安全 (VBS) 和主机保护者服务 (HGS) 协议。 关联的认证数据是认证服务器的 URL。 因此，会将以下设置添加到连接字符串中：

```
ColumnEncryption=VBS-HGS,http://attestationserver.mydomain/Attestation
```

如果协议不正确，驱动程序将无法识别该协议，连接失败，并返回一个错误。 如果仅认证 URL 不正确，则连接成功，并且在尝试启用了 enclave 的计算时将引发错误，否则该行为将与原始 Always Encrypted 行为相同。 将 `ColumnEncryption` 设置为 `enabled` 将提供 Always Encrypted 常规功能，但尝试启用了 enclave 的操作将返回错误。

若要详细了解如何将环境配置为支持具有安全 enclave 的 Always Encrypted（包括设置主机保护者服务和创建所需的加密密钥），可单击[此处](../../relational-databases/security/encryption/configure-always-encrypted-enclaves.md)。

## <a name="examples"></a>示例

下面的示例（一个用于 SQLSRV，另一个用于 PDO_SQLSRV）以纯文本格式创建包含多个数据类型的表，然后对其加密并执行比较和模式匹配。 请注意下列详细信息：

- 使用 `ALTER TABLE` 加密表时，每次调用 `ALTER TABLE` 时只能加密一个列，因此需要进行多次调用来加密多个列。
- 将比较阈值作为比较 char 和 nchar 类型的参数传递时，必须在相应的 `SQLSRV_SQLTYPE_*` 中指定列宽度，否则将返回错误 `HY104`、`Invalid precision value`。
- 对于模式匹配，必须使用 `COLLATE` 子句将排序规则指定为 `Latin1_General_BIN2`。
- 将模式匹配字符串作为匹配 char 和 nchar 类型的参数传递时，传递给 `sqlsrv_query` 或 `sqlsrv_prepare` 的 `SQLSRV_SQLTYPE_*` 应指定要匹配的字符串的长度，而不是列的大小，因为 char 和 nchar 类型在字符串末尾填充空白。 例如，将 `%abc%` 字符串与 char(10) 列匹配时，指定 `SQLSRV_SQLTYPE_CHAR(5)`。 如果改为指定 `SQLSRV_SQLTYPE_CHAR(10)`，则查询将匹配 `%abc%     `（追加了五个空格），而追加的空格少于五的列中的所有数据都将不匹配（因此 `abcdef` 不匹配 `%abc%`，因为它有四个填充空格）。 对于 Unicode 字符串，请使用 `mb_strlen` 或 `iconv_strlen` 函数获取字符数。
- PDO 接口不允许指定参数的长度。 相反，请在 `PDOStatement::bindParam` 中指定长度 0 或 `null`。 如果将长度显式设置为另一个数字，则该参数将被视为输出参数。
- 在 Always Encrypted 中，模式匹配对非字符串类型不起作用。
- 为清楚起见，将不包括错误检查。

对于这两个示例，下面的数据是通用的：

```php
<?php
// Data for testing - integer, datetime2, char, nchar, varchar, and nvarchar
// String data is random, showing that we can match or compare anything
$testValues = array(array(1, "2019-12-31 01:00:00", "abcd", "㬚㔈♠既", "abcd", "㬚㔈♠既"),
                    array(-100, "1753-01-31 14:25:25.25", "#e@?q&zy+", "ઔܛ᎓Ե⅜", "#e@?q&zy+", "ઔܛ᎓Ե⅜"),
                    array(100, "2112-03-15 23:40:10.1594", "zyxwv", "㶋㘚ᐋꗡ", "zyxwv", "㶋㘚ᐋꗡ"),
                    array(0, "8888-08-08 08:08:08.08", "7t", "㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ", "7t", "㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ"),
                    );

// Queries to create the table and insert data
$createTable = "DROP TABLE IF EXISTS $myTable; 
                CREATE TABLE $myTable (c_integer int NULL, 
                                       c_datetime2 datetime2(7) NULL, 
                                       c_char char(32) NULL, 
                                       c_nchar nchar(32) NULL, 
                                       c_varchar varchar(32) NULL, 
                                       c_nvarchar nvarchar(32) NULL);";
$insertData = "INSERT INTO $myTable (c_integer, c_datetime2, c_char, c_nchar, c_varchar, c_nvarchar) VALUES (?, ?, ?, ?, ?, ?)";

// This is the query that encrypts the table in place
$encryptQuery = " ALTER TABLE $myTable
                      ALTER COLUMN [c_integer] integer
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_datetime2] datetime2(7)
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_char] char(32) COLLATE Latin1_General_BIN2
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_nchar] nchar(32) COLLATE Latin1_General_BIN2
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_varchar] varchar(32) COLLATE Latin1_General_BIN2
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_nvarchar] nvarchar(32) COLLATE Latin1_General_BIN2
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;";
?>
```

### <a name="sqlsrv"></a>SQLSRV

```php
<?php
// Specify Azure Key Vault credentials using the KeyStoreAuthentication, KeyStorePrincipalId, and KeyStoreSecret keywords
// Otherwise, the local Windows Certificate Store will be used
$options = array('database'=>$myDatabase,
                 'uid'=>$myUsername,
                 'pwd'=>$myPassword,
                 'CharacterSet'=>'UTF-8',
                 'ReturnDatesAsStrings'=>true,
                 'ColumnEncryption'=>"VBS-HGS,http://myattestationserver.mydomain/Attestation",
                 );
                 
$conn = sqlsrv_connect($myServer, $options);

// Create the table and insert the test data
$stmt = sqlsrv_query($conn, $createTable);

foreach ($testValues as $values) {
    $stmt = sqlsrv_prepare($conn, $insertData, $values);
    sqlsrv_execute($stmt);
}

// Encrypt the table in place
$stmt = sqlsrv_query($conn, $encryptQuery);

// Test comparison and pattern matching on the encrypted table
echo "Test comparisons:\n";

$intThreshold = 0;
$testGreater = "SELECT c_integer FROM $myTable WHERE c_integer > ?";
$param = array($intThreshold, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_INT);
$stmt = sqlsrv_prepare($conn, $testGreater, array($param));
getResults($stmt);
// Expect:
// 1
// 100

$datetimeThreshold = "3000-01-01 00:00:00.0";
$testLess = "SELECT c_datetime2 FROM $myTable WHERE c_datetime2 < ?";
$param = array($datetimeThreshold, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_DATETIME2);
$stmt = sqlsrv_prepare($conn, $testLess, array($param));
getResults($stmt);
// Expect:
// 2019-12-31 01:00:00.0000000
// 1753-01-31 14:25:25.2500000
// 2112-03-15 23:40:10.1594000

$charThreshold = "abcd";
$ncharThreshold = "㬚㔈♠既";

$testGreaterEqual = "SELECT c_char FROM $myTable WHERE c_char >= ?";
$param = array($charThreshold, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_CHAR(32));
$stmt = sqlsrv_prepare($conn, $testGreaterEqual, array($param));
getResults($stmt);
// Expect:
// abcd                            
// zyxwv                           

$testLessEqual = "SELECT c_nchar FROM $myTable WHERE c_nchar <= ?";
$param = array($ncharThreshold, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING('UTF-8'), SQLSRV_SQLTYPE_NCHAR(32));
$stmt = sqlsrv_prepare($conn, $testLessEqual, array($param));
getResults($stmt);
// Expect:
// 㬚㔈♠既                            
// ઔܛ᎓Ե⅜                           
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    

$testNotGreater = "SELECT c_varchar FROM $myTable WHERE c_varchar !> ?";
$param = array($charThreshold, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_VARCHAR);
$stmt = sqlsrv_prepare($conn, $testNotGreater, array($param));
getResults($stmt);
// Expect:
// abcd
// #e@?q&zy+
// 7t

$testNotLess = "SELECT c_nvarchar FROM $myTable WHERE c_nvarchar !< ?";
$param = array($ncharThreshold, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING('UTF-8'), SQLSRV_SQLTYPE_NVARCHAR);
$stmt = sqlsrv_prepare($conn, $testNotLess, array($param));
getResults($stmt);
// Expect:
// 㬚㔈♠既
// 㶋㘚ᐋꗡ

echo "\nTest pattern matching:\n";

$charMatch = "%zy%";
$ncharMatch = "%㔈♠既%";

$param = array($charMatch, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_CHAR(strlen($charMatch)));
$testCharMatch = "SELECT c_char FROM $myTable WHERE c_char LIKE ? COLLATE Latin1_General_BIN2";
$stmt = sqlsrv_prepare($conn, $testCharMatch, array($param));
getResults($stmt);
// Expect:
// #e@?q&zy+                       
// zyxwv                           

$param = array($ncharMatch, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING("UTF-8"), SQLSRV_SQLTYPE_NCHAR(iconv_strlen($ncharMatch)));
$testNCharMatch = "SELECT c_nchar FROM $myTable WHERE c_nchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = sqlsrv_prepare($conn, $testNCharMatch, array($param));
getResults($stmt);
// Expect:
// 㬚㔈♠既                            
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    

$param = array($charMatch, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_VARCHAR(strlen($charMatch)));
$testVarcharMatch = "SELECT c_varchar FROM $myTable WHERE c_varchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = sqlsrv_prepare($conn, $testVarcharMatch, array($param));
getResults($stmt);
// Expect:
// #e@?q&zy+
// zyxwv

$param = array($ncharMatch, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING("UTF-8"), SQLSRV_SQLTYPE_NVARCHAR(iconv_strlen($ncharMatch)));
$testNVarcharMatch = "SELECT c_nvarchar FROM $myTable WHERE c_nvarchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = sqlsrv_prepare($conn, $testNVarcharMatch, array($param));
getResults($stmt);
// Expect:
// 㬚㔈♠既
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ

function getResults($stmt)
{
    sqlsrv_execute($stmt);
    while ($res = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_NUMERIC)) {
        print_r($res[0]);
        echo "\n";
    }
}
?>
```

### <a name="pdo_sqlsrv"></a>PDO_SQLSRV

```php
<?php
// Specify Azure Key Vault credentials using the KeyStoreAuthentication, KeyStorePrincipalId, and KeyStoreSecret keywords
// Otherwise, the local Windows Certificate Store will be used
$options = "sqlsrv:server=$myServer;database=$myDatabase;driver={ODBC Driver 17 for SQL Server};";
$options .= "ColumnEncryption=VBS-HGS,http://myattestationserver.mydomain/Attestation",

$conn = new PDO($options, $myUsername, $myPassword);

// Create the table and insert the test data
$stmt = $conn->query($createTable);

foreach ($testValues as $values) {
    $stmt = $conn->prepare($insertData);
    $stmt->execute($values);
}

// Encrypt the table in place
$stmt = $conn->query($encryptQuery);

// Test comparison and pattern matching on the encrypted table
echo "Test comparisons:\n";

$intThreshold = 0;
$testGreater = "SELECT c_integer FROM $myTable WHERE c_integer > ?";
$stmt = $conn->prepare($testGreater);
$stmt->bindParam(1, $intThreshold, PDO::PARAM_INT);
getResults($stmt);
// Expect:
// 1
// 100

$datetimeThreshold = "3000-01-01 00:00:00.0";
$testLess = "SELECT c_datetime2 FROM $myTable WHERE c_datetime2 < ?";
$stmt = $conn->prepare($testLess);
$stmt->bindParam(1, $datetimeThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// 2019-12-31 01:00:00.0000000
// 1753-01-31 14:25:25.2500000
// 2112-03-15 23:40:10.1594000

$charThreshold = "abcd";
$ncharThreshold = "㬚㔈♠既";

$testGreaterEqual = "SELECT c_char FROM $myTable WHERE c_char >= ?";
$stmt = $conn->prepare($testGreaterEqual);
$stmt->bindParam(1, $charThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// abcd                            
// zyxwv                           

$testLessEqual = "SELECT c_nchar FROM $myTable WHERE c_nchar <= ?";
$stmt = $conn->prepare($testLessEqual);
$stmt->bindParam(1, $ncharThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// 㬚㔈♠既                            
// ઔܛ᎓Ե⅜                           
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    

$testNotGreater = "SELECT c_varchar FROM $myTable WHERE c_varchar !> ?";
$stmt = $conn->prepare($testNotGreater);
$stmt->bindParam(1, $charThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// abcd
// #e@?q&zy+
// 7t

$testNotLess = "SELECT c_nvarchar FROM $myTable WHERE c_nvarchar !< ?";
$stmt = $conn->prepare($testNotLess);
$stmt->bindParam(1, $ncharThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// 㬚㔈♠既
// 㶋㘚ᐋꗡ

echo "\nTest pattern matching:\n";

$charMatch = "%zy%";
$ncharMatch = "%㔈♠既%";

$testCharMatch = "SELECT c_char FROM $myTable WHERE c_char LIKE ? COLLATE Latin1_General_BIN2";
$stmt = $conn->prepare($testCharMatch);
$stmt->bindParam(1, $charMatch, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// #e@?q&zy+                       
// zyxwv                           

$testNCharMatch = "SELECT c_nchar FROM $myTable WHERE c_nchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = $conn->prepare($testNCharMatch);
$stmt->bindParam(1, $ncharMatch, PDO::PARAM_STR,null,PDO::SQLSRV_ENCODING_UTF8);
getResults($stmt);
// Expect:
// 㬚㔈♠既                            
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    

$testVarcharMatch = "SELECT c_varchar FROM $myTable WHERE c_varchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = $conn->prepare($testVarcharMatch);
$stmt->bindParam(1, $charMatch, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// #e@?q&zy+
// zyxwv

$testNVarcharMatch = "SELECT c_nvarchar FROM $myTable WHERE c_nvarchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = $conn->prepare($testNVarcharMatch);
$stmt->bindParam(1, $ncharMatch, PDO::PARAM_STR,null,PDO::SQLSRV_ENCODING_UTF8);
getResults($stmt);
// Expect:
// 㬚㔈♠既
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ

function getResults($stmt)
{
    $stmt->execute();
    while($res = $stmt->fetch(PDO::FETCH_NUM)) {
        print_r($res[0]);
        echo "\n";
    }
}
?>
```

输出：

```
Test comparisons:
1
100
2019-12-31 01:00:00.0000000
1753-01-31 14:25:25.2500000
2112-03-15 23:40:10.1594000
abcd                            
zyxwv                           
㬚㔈♠既                            
ઔܛ᎓Ե⅜                           
㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    
abcd
#e@?q&zy+
7t
㬚㔈♠既
㶋㘚ᐋꗡ

Test pattern matching:
#e@?q&zy+                       
zyxwv                           
㬚㔈♠既                            
㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    
#e@?q&zy+
zyxwv
㬚㔈♠既
㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ
```

## <a name="see-also"></a>另请参阅

[PHP SQL 驱动程序编程指南](programming-guide-for-php-sql-driver.md)  
[SQLSRV 驱动程序 API 参考](sqlsrv-driver-api-reference.md)  
[PDO_SQLSRV 驱动程序 API 参考](pdo-sqlsrv-driver-reference.md)  
[在适用于 SQL Server 的 PHP 驱动程序中使用 Always Encrypted](using-always-encrypted-php-drivers.md)
