---
title: 驱动程序功能支持矩阵
description: 了解 SQL Server 的驱动程序支持的常用功能，以及可找到有关这些功能的信息的位置。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-daenge
ms.openlocfilehash: e93307a7f29f31aa078a7d705a904812e6aa3847
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166061"
---
# <a name="driver-feature-support-matrix-for-microsoft-sql-server"></a>Microsoft SQL Server 的驱动程序功能支持矩阵

如果你计划在 Microsoft SQL Server 中使用某项功能，但并非所有驱动程序中都提供了该功能。 某项功能可能不在特定驱动程序中提供的一些原因包括：

- 此功能不适用于该驱动程序技术。
- 此功能是新功能，尚未在所有驱动程序中实现。
- 特定驱动程序中不需要此功能。
- 首先要实现其他功能。

我们希望所有驱动程序都可支持每项功能，并努力确保实现各驱动程序之间的功能奇偶一致性。 然而，这并非总是可行的。 为了帮助你根据需要选择适当的驱动程序，下方列出了常用功能和实现这些功能的驱动程序。

- [.NET](#table1)
- [ODBC](#table2)
- [OLE DB](#table2)
- [JDBC](#table2)
- [PHP](#table3)
- [Node.js/JavaScript](#table3)
- [Python](#table3)

| <a id="table1"></a>功能 | [Microsoft.<wbr>Data.<wbr>SqlClient (.NET Core)](ado-net/microsoft-ado-net-sql-server.md) | [Microsoft.<wbr>Data.<wbr>SqlClient (.NET Framework)](ado-net/microsoft-ado-net-sql-server.md) | System.<wbr>Data.<wbr>SqlClient (.NET Core) | [System.<wbr>Data.<wbr>SqlClient (.NET Framework)](/dotnet/framework/data/adonet/sql/) |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [是](ado-net/sql/sqlclient-support-always-encrypted.md) | [是](ado-net/sql/sqlclient-support-always-encrypted.md) | | [是](ado-net/sql/sqlclient-support-always-encrypted.md) |
| [具有安全 Enclave 的 Always Encrypted](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [是](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) | [是](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) | | [是](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) |
| [Azure Active Directory 访问令牌身份验证](/azure/active-directory/develop/access-tokens) | [是](/dotnet/api/system.data.sqlclient.sqlconnection.accesstoken) | [是](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) | [是](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) | [是](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) |
| [Azure Active Directory 密码身份验证](/azure/sql-database/sql-database-aad-authentication) | [是](ado-net/sql/azure-active-directory-authentication.md) | [是](ado-net/sql/azure-active-directory-authentication.md) | | 是 |
| [Azure Active Directory 集成身份验证](/azure/sql-database/sql-database-aad-authentication) | [是](ado-net/sql/azure-active-directory-authentication.md) | [是](ado-net/sql/azure-active-directory-authentication.md) | | 是 |
| [Azure Active Directory 交互式 (MFA) 身份验证](/azure/sql-database/sql-database-aad-authentication) | [是](ado-net/sql/azure-active-directory-authentication.md) | [是](ado-net/sql/azure-active-directory-authentication.md) | | 是 |
| [Azure Active Directory 托管标识身份验证](/azure/active-directory/managed-identities-azure-resources/overview) | [是](ado-net/sql/azure-active-directory-authentication.md) | [是](ado-net/sql/azure-active-directory-authentication.md) | | |
| [Azure Active Directory 服务主体身份验证](/azure/active-directory/develop/app-objects-and-service-principals) | [是](ado-net/sql/azure-active-directory-authentication.md) | [是](ado-net/sql/azure-active-directory-authentication.md) | | |
| [Windows 集成身份验证](/windows-server/security/windows-authentication/windows-authentication-overview) | [是](ado-net/sql/authentication-sql-server.md) | [是](ado-net/sql/authentication-sql-server.md) | [是](/dotnet/framework/data/adonet/sql/authentication-in-sql-server) | [是](/dotnet/framework/data/adonet/sql/authentication-in-sql-server) |
| [大容量复制](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | [是](ado-net/sql/bulk-copy-operations-sql-server.md) | [是](ado-net/sql/bulk-copy-operations-sql-server.md) | [是](/dotnet/framework/data/adonet/sql/bulk-copy-operations-in-sql-server) | [是](/dotnet/framework/data/adonet/sql/bulk-copy-operations-in-sql-server) |
| [数据敏感度和分类元数据](../relational-databases/security/sql-data-discovery-and-classification.md) | [是](ado-net/sql/data-classification.md) | [是](ado-net/sql/data-classification.md) | 是 | 是 |
| [多重活动结果集 (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [是](ado-net/sql/multiple-active-result-sets-mars.md) | [是](ado-net/sql/multiple-active-result-sets-mars.md) | [是](/dotnet/framework/data/adonet/sql/multiple-active-result-sets-mars) | [是](/dotnet/framework/data/adonet/sql/multiple-active-result-sets-mars) |
| [空间数据类型](../relational-databases/spatial/spatial-data-sql-server.md) | | “是” | | 是 |
| [表值参数 (TVP)](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | [是](ado-net/sql/table-valued-parameters.md) | [是](ado-net/sql/table-valued-parameters.md) | [是](/dotnet/framework/data/adonet/sql/table-valued-parameters) | [是](/dotnet/framework/data/adonet/sql/table-valued-parameters) |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [是](ado-net/sql/sqlclient-support-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [是](ado-net/sql/sqlclient-support-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [是](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.multisubnetfailover?view=netcore-1.0&preserve-view=true) | [是](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.multisubnetfailover?view=netframework-4.8&preserve-view=true) |
| [透明网络 IP 解析](odbc/using-transparent-network-ip-resolution.md) | | [是](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring?view=sqlclient-dotnet-1.1&preserve-view=true) | | [是](/dotnet/api/system.data.sqlclient.sqlconnection.connectionstring?view=netframework-4.8&preserve-view=true) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

| <a id="table2"></a>功能 | [Windows 上的 ODBC Driver for SQL Server](odbc/microsoft-odbc-driver-for-sql-server.md) | [Linux 和 macOS 上的 ODBC Driver for SQL Server](odbc/microsoft-odbc-driver-for-sql-server.md) | [JDBC Driver for SQL Server](jdbc/microsoft-jdbc-driver-for-sql-server.md) | [适用于 SQL Server 的 OLE DB 驱动程序](oledb/oledb-driver-for-sql-server.md) |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [是](odbc/using-always-encrypted-with-the-odbc-driver.md) | [是](odbc/using-always-encrypted-with-the-odbc-driver.md) | [是](jdbc/using-always-encrypted-with-the-jdbc-driver.md) |
| [具有安全 Enclave 的 Always Encrypted](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [是](odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves) | [是](odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves) | [是](jdbc/using-always-encrypted-with-the-jdbc-driver.md) | |
| [Azure Active Directory 访问令牌身份验证](/azure/active-directory/develop/access-tokens) | [是](odbc/using-azure-active-directory.md#authenticating-with-an-access-token) | [是](odbc/using-azure-active-directory.md#authenticating-with-an-access-token) | [是](jdbc/connecting-using-azure-active-directory-authentication.md#connecting-using-access-token) | [是](oledb/features/using-azure-active-directory.md) |
| [Azure Active Directory 密码身份验证](/azure/sql-database/sql-database-aad-authentication) |  [是](odbc/using-azure-active-directory.md) | [是](odbc/using-azure-active-directory.md) | [是](jdbc/connecting-using-azure-active-directory-authentication.md) | [是](oledb/features/using-azure-active-directory.md) |
| [Azure Active Directory 集成身份验证](/azure/sql-database/sql-database-aad-authentication) | [是](odbc/using-azure-active-directory.md) | [是](odbc/using-azure-active-directory.md) | [是](jdbc/connecting-using-azure-active-directory-authentication.md) | [是](oledb/features/using-azure-active-directory.md) |
| [Azure Active Directory 交互式 (MFA) 身份验证](/azure/sql-database/sql-database-aad-authentication) | [是](odbc/using-azure-active-directory.md) | | [是](jdbc/connecting-using-azure-active-directory-authentication.md) | [是](oledb/features/using-azure-active-directory.md) |
| [Azure Active Directory 托管标识身份验证](/azure/active-directory/managed-identities-azure-resources/overview) | [是](odbc/using-azure-active-directory.md) | [是](odbc/using-azure-active-directory.md) | [是](jdbc/connecting-using-azure-active-directory-authentication.md) | [是](oledb/features/using-azure-active-directory.md) |
| [Azure Active Directory 服务主体身份验证](/azure/active-directory/develop/app-objects-and-service-principals) | [是](odbc/using-azure-active-directory.md) | [是](odbc/using-azure-active-directory.md) | [是](jdbc/connecting-using-azure-active-directory-authentication.md) | [是](oledb/features/using-azure-active-directory.md) |
| [Windows 集成身份验证](/windows-server/security/windows-authentication/windows-authentication-overview) | 是 | [是](odbc/linux-mac/using-integrated-authentication.md) | [是](jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server.md) | 是 |
| [大容量复制](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | [是](../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md) | [是](../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md) | [是](jdbc/using-bulk-copy-with-the-jdbc-driver.md) | [是](oledb/features/performing-bulk-copy-operations.md) |
| [数据发现和分类元数据](../relational-databases/security/sql-data-discovery-and-classification.md) | [是](odbc/data-classification.md) | [是](odbc/data-classification.md) | [是](jdbc/data-discovery-classification-sample.md) | |
| [多重活动结果集 (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [是](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [是](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | | [是](oledb/features/using-multiple-active-result-sets-mars.md) |
| [空间数据类型](../relational-databases/spatial/spatial-data-sql-server.md) | | | [是](jdbc/use-spatial-datatypes.md) | |
| [表值参数 (TVP)](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | [是](../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md) | [是](../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md) | [是](jdbc/using-table-valued-parameters.md) | [是](oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md) |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [是](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [是](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [是](jdbc/jdbc-driver-support-for-high-availability-disaster-recovery.md) | [是](oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) |
| [透明网络 IP 解析](odbc/using-transparent-network-ip-resolution.md) | [是](odbc/using-transparent-network-ip-resolution.md) | [是](odbc/using-transparent-network-ip-resolution.md) | [是](jdbc/setting-the-connection-properties.md) | [是](oledb/features/using-transparent-network-ip-resolution.md) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

| <a id="table3"></a>功能 | [Windows 上的 Driver for PHP for SQL Server](php/microsoft-php-driver-for-sql-server.md)<sup>[1](#note1)</sup> | [Linux 和 macOS 上的 Driver for PHP for SQL Server](php/microsoft-php-driver-for-sql-server.md)<sup>[1](#note1)</sup> | [Tedious (Node.js)](node-js/node-js-driver-for-sql-server.md) | [pyODBC (Python)](python/pyodbc/python-sql-driver-pyodbc.md)<sup>[1](#note1)</sup> |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [是](php/using-always-encrypted-php-drivers.md) | [是](php/using-always-encrypted-php-drivers.md) | | 是 |
| [具有安全 Enclave 的 Always Encrypted](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [是](php/always-encrypted-secure-enclaves.md) | [是](php/always-encrypted-secure-enclaves.md) | | 是 |
| [Azure Active Directory 访问令牌身份验证](/azure/active-directory/develop/access-tokens) | [是](php/azure-active-directory.md) | [是](php/azure-active-directory.md) | [是](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | 是 |
| [Azure Active Directory 密码身份验证](/azure/sql-database/sql-database-aad-authentication) | [是](php/azure-active-directory.md) | [是](php/azure-active-directory.md) | [是](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | 是 |
| [Azure Active Directory 集成身份验证](/azure/sql-database/sql-database-aad-authentication) | [是](php/azure-active-directory.md) | [是](php/azure-active-directory.md) | | 是 |
| [Azure Active Directory 交互式 (MFA) 身份验证](/azure/sql-database/sql-database-aad-authentication) | | | | 是<sup>[2](#note2)</sup> |
| [Azure Active Directory 托管标识身份验证](/azure/active-directory/managed-identities-azure-resources/overview) | [是](php/azure-active-directory.md) | [是](php/azure-active-directory.md) | [是](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | 是 |
| [Azure Active Directory 服务主体身份验证](/azure/active-directory/develop/app-objects-and-service-principals) | | | | |
| [Windows 集成身份验证](/windows-server/security/windows-authentication/windows-authentication-overview) | [是](php/how-to-connect-using-windows-authentication.md) | [是](odbc/linux-mac/using-integrated-authentication.md) | | 是 |
| [大容量复制](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | | | [是](https://tediousjs.github.io/tedious/bulk-load.html) | |
| [数据发现和分类元数据](../relational-databases/security/sql-data-discovery-and-classification.md) | “是” | 是 | | |
| [多重活动结果集 (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [是](php/how-to-disable-multiple-active-resultsets-mars.md) | [是](php/how-to-disable-multiple-active-resultsets-mars.md) | | 是 |
| [空间数据类型](../relational-databases/spatial/spatial-data-sql-server.md) | | | | |
| [表值参数 (TVP)](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | | | [是](https://tediousjs.github.io/tedious/parameters.html) | 是 |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [是](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | [是](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | | [是](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) |
| [透明网络 IP 解析](odbc/using-transparent-network-ip-resolution.md) | [是](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | [是](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | | [是](odbc/using-transparent-network-ip-resolution.md) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<a id="note1"></a><sup>1</sup>由于这些驱动程序依赖 Microsoft ODBC Driver for SQL Server，因此还必须使用支持此功能的驱动程序版本。

<a id="note2"></a><sup>2</sup>仅限在 Windows 上。

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
