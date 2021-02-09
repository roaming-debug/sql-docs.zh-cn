---
title: Azure Active Directory
description: 了解如何将 Azure Active Directory 身份验证用于 Microsoft Drivers for PHP for SQL Server。
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- azure active directory, authentication, access token
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae3e734dee5f8ac46de2646cca9c37a7ed7748df
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076955"
---
# <a name="connect-using-azure-active-directory-authentication"></a>使用 Azure Active Directory 身份验证进行连接
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[Azure Active Directory](/azure/active-directory/active-directory-whatis) (Azure AD) 是一项中心用户 ID 管理技术，可以作为 [SQL Server 身份验证](how-to-connect-using-sql-server-authentication.md)的替代方法。 Azure AD 允许使用用户名和密码、Windows 集成身份验证或 Azure AD 访问令牌在 Azure AD 中使用联合身份连接到 Microsoft Azure SQL 数据库和 Azure Synapse Analytics。 适用于 SQL Server 的 PHP 驱动程序提供对这些功能的部分支持。

若要使用 Azure AD，请使用“Authentication”  或“AccessToken”  关键字（它们互相排斥），如下表所示。 有关更多技术详细信息，请参阅[结合使用 Azure Active Directory 和 ODBC 驱动程序](../odbc/using-azure-active-directory.md)。

|关键字|值|说明|
|-|-|-|
|**AccessToken**|未设置（默认值）|身份验证模式由其他关键字确定。 有关详细信息，请参阅 [Connection Options](connection-options.md)。 |
||一个字节字符串|从 OAuth JSON 响应中提取的 Azure AD 访问令牌。 连接字符串不能包含用户 ID、密码或 Authentication 关键字（在 Linux 或 macOS 中需要 ODBC Driver 版本 17 或更高版本）。 |
|**身份验证**|未设置（默认值）|身份验证模式由其他关键字确定。 有关详细信息，请参阅 [Connection Options](connection-options.md)。 |
||`SqlPassword`|使用用户名和密码直接对 SQL Server 实例（可能是 Azure 实例）进行身份验证。 必须使用“UID”  和“PWD”  关键字将用户名和密码传递到连接字符串。 |
||`ActiveDirectoryPassword`|使用用户名和密码对 Azure Active Directory 标识进行身份验证。 必须使用“UID”  和“PWD”  关键字将用户名和密码传递到连接字符串。 |
||`ActiveDirectoryMsi`|使用系统分配的托管标识或用户分配的托管标识进行身份验证（需要 ODBC Driver 版本 17.3.1.1 或更高版本）。 有关概述和教程，请参阅 [什么是 Azure 资源的托管标识？](/azure/active-directory/managed-identities-azure-resources/overview)。|
||`ActiveDirectoryServicePrincipal`|使用服务主体对象进行身份验证（需要 ODBC 驱动程序版本 17.7 或更高版本）。 有关详细信息和更多示例，请参阅 [Azure Active Directory 中的应用程序对象和服务主体对象](/azure/active-directory/develop/app-objects-and-service-principals)。|

“Authentication”  关键字影响连接安全设置。 如果在连接字符串中设置了此关键字，则默认情况下“Encrypt”  关键字设置为 true，这意味着客户端将请求加密。 而且，除非 TrustServerCertificate  设置为 true（默认情况下为 false  ），否则将验证服务器证书，而不考虑加密设置。 此功能有别于旧的、不安全的登录方法，在此方法中，仅当在连接字符串中明确请求加密时，才会验证服务器证书。

#### <a name="limitations"></a>限制

在 Windows 上，基础 ODBC 驱动程序支持“Authentication”  关键字的另一个值“ActiveDirectoryIntegrated”  ，但 PHP 驱动程序在任何平台上都不支持这个值。

## <a name="example---connect-using-sqlpassword-and-activedirectorypassword"></a>示例 - 使用 SqlPassword 和 ActiveDirectoryPassword 进行连接

```php
<?php
// First connect to a local SQL Server instance by setting Authentication to SqlPassword
$serverName = "myserver.mydomain";

$connectionInfo = array("UID"=>$myusername, "PWD"=>$mypassword, "Authentication"=>'SqlPassword');

$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Authentication=SqlPassword.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=SqlPassword.\n";
    sqlsrv_close($conn);
}

// Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
$azureServer = "myazureserver.database.windows.net";
$azureDatabase = "myazuredatabase";
$azureUsername = "myuid";
$azurePassword = "mypassword";
$connectionInfo = array("Database"=>$azureDatabase,
                        "UID"=>$azureUsername,
                        "PWD"=>$azurePassword,
                        "Authentication"=>'ActiveDirectoryPassword');

$conn = sqlsrv_connect($azureServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
    sqlsrv_close($conn);
}

?>
```

## <a name="example---connect-using-the-pdo_sqlsrv-driver"></a>示例 - 使用 PDO_SQLSRV 驱动程序进行连接

```php
<?php
// First connect to a local SQL Server instance by setting Authentication to SqlPassword
$serverName = "myserver.mydomain";

$connectionInfo = "Database = $databaseName; Authentication = SqlPassword;";

try {
    $conn = new PDO("sqlsrv:server = $serverName ; $connectionInfo", $myusername, $mypassword);
    echo "Connected successfully with Authentication=SqlPassword.\n";
    $conn = null;
} catch (PDOException $e) {
    echo "Could not connect with Authentication=SqlPassword.\n";
    print_r($e->getMessage());
    echo "\n";
}

// Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
$azureServer = "myazureserver.database.windows.net";
$azureDatabase = "myazuredatabase";
$azureUsername = "myuid";
$azurePassword = "mypassword";
$connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryPassword;";

try {
    $conn = new PDO("sqlsrv:server = $azureServer ; $connectionInfo", $azureUsername, $azurePassword);
    echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-azure-ad-access-token"></a>示例 - 使用 Azure AD 访问令牌进行连接

### <a name="sqlsrv-driver"></a>SQLSRV 驱动程序

```php
<?php
// Using an access token to connect: do not use UID or PWD connection options
// Assume $accToken is the valid byte string extracted from an OAuth JSON response
$connectionInfo = array("Database"=>$azureAdDatabase, "AccessToken"=>$accToken);
$conn = sqlsrv_connect($azureAdServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Azure AD Access Token.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Azure AD Access Token.\n";
    sqlsrv_close($conn);
}
?>
```

### <a name="pdo_sqlsrv-driver"></a>PDO_SQLSRV 驱动程序

```php
<?php
try {
    // Using an access token to connect: do not pass in $uid or $pwd
    // Assume $accToken is the valid byte string extracted from an OAuth JSON response
    $connectionInfo = "Database = $azureAdDatabase; AccessToken = $accToken;";
    $conn = new PDO("sqlsrv:server = $azureAdServer; $connectionInfo");
    echo "Connected successfully with Azure AD Access Token\n";
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Azure AD Access Token.\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-managed-identities-for-azure-resources"></a>示例 - 使用 Azure 资源的托管标识进行连接

### <a name="using-the-system-assigned-managed-identity-with-sqlsrv-driver"></a>对 SQLSRV 驱动程序使用系统分配的托管标识

使用系统分配的托管标识进行连接时，请不要使用 UID 或 PWD 选项。

```php
<?php

$azureServer = 'myazureserver.database.windows.net';
$azureDatabase = 'myazuredatabase';
$connectionInfo = array('Database'=>$azureDatabase,
                        'Authentication'=>'ActiveDirectoryMsi');
$conn = sqlsrv_connect($azureServer, $connectionInfo);

if ($conn === false) {
    echo "Could not connect with Authentication=ActiveDirectoryMsi (system-assigned).\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=ActiveDirectoryMsi (system-assigned).\n";
    
    $tsql = "SELECT @@Version AS SQL_VERSION";
    $stmt = sqlsrv_query($conn, $tsql);
    if ($stmt === false) {
        echo "Failed to run the simple query (system-assigned).\n";
        print_r(sqlsrv_errors());
    } else {
        while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
            echo $row['SQL_VERSION'] . PHP_EOL;
        }

        sqlsrv_free_stmt($stmt);
    }
    
    sqlsrv_close($conn);
}
?>
```

### <a name="using-the-user-assigned-managed-identity-with-pdo_sqlsrv-driver"></a>对 PDO_SQLSRV 驱动程序使用用户分配的托管标识

用户分配的托管标识作为独立的 Azure 资源创建。 Azure 会在由所用订阅信任的 Azure AD 租户中创建一个标识。 在创建标识后，可以将标识分配到一个或多个 Azure 服务实例。 复制此标识的 `Object ID`，并将其设置为连接字符串中的用户名。 

因此，当使用用户分配的托管标识进行连接时，请提供对象 ID 作为用户名，但省略密码。

```php
<?php

$azureServer = 'myazureserver.database.windows.net';
$azureDatabase = 'myazuredatabase';
$azureUser = '2d68f56e-9547-4dae-aee8-f3g28ab9674x';    // Object ID of the identity
$connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryMsi;";

try {
    $conn = new PDO("sqlsrv:server = $azureServer; $connectionInfo", $azureUser);
    echo "Connected successfully with Authentication=ActiveDirectoryMsi (user-assigned).\n";
    
    $tsql = "SELECT @@Version AS SQL_VERSION";
    
    try {
        $stmt = $conn->query($tsql);
        $result = $stmt->fetchall(PDO::FETCH_ASSOC);
        print_r($result);

        unset($stmt);
    } catch (PDOException $e) {
        echo "Failed to run the simple query (user-assigned).\n";
    }
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Authentication=ActiveDirectoryMsi (user-assigned).\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-service-principal-objects-in-azure-active-directory"></a>示例 - 使用 Azure Active Directory 中的服务主体对象进行连接

若要使用服务主体对象进行身份验证，将需要相应的[应用程序客户端 ID](/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-and-app-id-values-for-signing-in) 和[客户端密码](/azure/active-directory/develop/howto-create-service-principal-portal#option-2-create-a-new-application-secret)。


### <a name="sqlsrv-driver"></a>SQLSRV 驱动程序

```php
<?php

$adServer = 'myazureserver.database.windows.net';
$adDatabase = 'myazuredatabase';
$adSPClientId = 'myAppClientId';
$adSPClientSecret = 'myClientSecret';

$conn = false;
$connectionInfo = array("Database"=>$adDatabase, 
                        "Authentication"=>"ActiveDirectoryServicePrincipal",
                        "UID"=>$adSPClientId,
                        "PWD"=>$adSPClientSecret);

$conn = sqlsrv_connect($adServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect using Azure AD Service Principal." . PHP_EOL;
    print_r(sqlsrv_errors());
}

sqlsrv_close($conn);

?>
```

### <a name="pdo_sqlsrv-driver"></a>PDO_SQLSRV 驱动程序

```php
<?php

$adServer = 'myazureserver.database.windows.net';
$adDatabase = 'myazuredatabase';
$adSPClientId = 'myAppClientId';
$adSPClientSecret = 'myClientSecret';

$conn = false;
try {
    $connectionInfo = "Database = $adDatabase; Authentication = ActiveDirectoryServicePrincipal;";
    $conn = new PDO("sqlsrv:server = $adServer; $connectionInfo", $adSPClientId, $adSPClientSecret);
} catch (PDOException $e) {
    echo "Could not connect using Azure AD Service Principal.\n";
    print_r($e->getMessage());
    echo PHP_EOL;
}

unset($conn);
?>
```


## <a name="see-also"></a>另请参阅
[结合使用 Azure Active Directory 和 ODBC 驱动程序](../odbc/using-azure-active-directory.md)

[什么是 Azure 资源的托管标识？](/azure/active-directory/managed-identities-azure-resources/overview)