---
title: 结合使用 Always Encrypted 和 JDBC 驱动程序
description: 了解如何结合使用 Always Encrypted 和 Microsoft ODBC Driver for SQL Server 来开发 ODBC 应用程序。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 02e306b8-9dde-4846-8d64-c528e2ffe479
ms.author: v-chojas
author: v-chojas
ms.openlocfilehash: ab1b5b73ad1bd6ba02baa5ee31bb4be4b42bb63f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199003"
---
# <a name="using-always-encrypted-with-the-odbc-driver-for-sql-server"></a>在适用于 SQL Server 的 ODBC 驱动程序中使用 Always Encrypted
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

### <a name="applicable-to"></a>适用于

- 适用于 SQL Server 的 ODBC 驱动程序 13.1
- 适用于 SQL Server 的 ODBC 驱动程序 17

### <a name="introduction"></a>简介

本文介绍如何使用 [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)或[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md) 和[适用于 SQL Server 的 ODBC 驱动程序](microsoft-odbc-driver-for-sql-server.md)。

始终加密允许客户端应用程序对敏感数据进行加密，并且永远不向 SQL Server 或 Azure SQL 数据库显示该数据或加密密钥。 启用了 Always Encrypted 的驱动程序（例如适用于 SQL Server 的 ODBC 驱动程序）通过在客户端应用程序中以透明方式对敏感数据进行加密和解密来实现此目标。 该驱动程序自动确定哪些查询参数与敏感数据库列（使用始终加密进行保护）相对应，并对这些参数的值进行加密，然后再将数据传递到 SQL Server 或 Azure SQL 数据库。 同样，该驱动程序以透明方式对查询结果中从加密数据库列检索到的数据进行解密。 具有安全 enclave 的 Always Encrypted  扩展此功能，以便对敏感数据启动更丰富的功能，同时保持数据的机密性。

有关详细信息，请参阅 [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)和[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)。

### <a name="prerequisites"></a>先决条件

在数据库中配置始终加密。 这涉及为选定数据库列预配始终加密密钥和设置加密。 如果还没有配置了始终加密的数据库，请按照 [始终加密入门](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted)中的说明操作。 尤其要注意的是，数据库应包含列主密钥 (CMK)、列加密密钥 (CEK) 和包含一个或多个使用该 CEK 加密的表的元数据定义。

如果使用的是具有安全 enclave 的 Always Encrypted，请参阅[使用具有安全 enclave 的 Always Encrypted 开发应用程序](../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md)，了解其他先决条件。

### <a name="enabling-always-encrypted-in-an-odbc-application"></a>在 ODBC 应用程序中启用 Always Encrypted

若要同时启用参数加密和结果集加密列解密，最简单的方法是将 `ColumnEncryption` 连接字符串关键字的值设置为“Enabled”  。 以下是启用 Always Encrypted 的连接字符串示例：

```
SQLWCHAR *connString = L"Driver={ODBC Driver 17 for SQL Server};Server={myServer};Trusted_Connection=yes;ColumnEncryption=Enabled;";
```

此外，还可以通过以下方式在 DSN 配置中启用 Always Encrypted：使用相同的键和值（若有连接字符串设置，则将重写它们），或使用 `SQL_COPT_SS_COLUMN_ENCRYPTION` 预连接属性以编程方式完成。 这样设置后，将重写在连接字符串或 DSN 中设置的值：

```
 SQLSetConnectAttr(hdbc, SQL_COPT_SS_COLUMN_ENCRYPTION, (SQLPOINTER)SQL_COLUMN_ENCRYPTION_ENABLE, 0);
```

为连接启用后，可以针对单个查询调整 Always Encrypted 的行为。 有关详细信息，请参阅下方的[控制 Always Encrypted 对性能的影响](#controlling-the-performance-impact-of-always-encrypted)。

请注意，启用 Always Encrypted 不足以成功实现加密或解密；还需确保：

- 应用程序具有 *查看任意列主密钥定义* 和 *查看任意列加密密钥定义* 数据库权限，这是访问数据库中始终加密密钥的相关元数据所必需的权限。 有关详细信息，请参阅[数据库权限](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions)。

- 应用程序可以访问保护查询的加密列的 CEK 的 CMK。 它以存储 CMK 的密钥存储提供程序为基础。 有关详细信息，请参阅[使用列主密钥存储](#working-with-column-master-key-stores)。

### <a name="enabling-always-encrypted-with-secure-enclaves"></a>启用“具有安全 Enclaves 的 Always Encrypted”

> [!NOTE]
> 在 Linux 和 macOS 上，必须有 OpenSSL 版本 1.0.1 或更高版本，才能使用具有安全 enclave 的 Always Encrypted。

自版本 17.4 起，驱动程序支持具有安全 Enclave 的 Always Encrypted。 若要在连接到数据库时启用 enclave，请将 `ColumnEncryption` DSN 密钥、连接字符串关键字或连接属性设置为以下值：`<attestation protocol>\<attestation URL>`，其中：

- `<attestation protocol>` - 指定用于 enclave 证明的协议。
  - 如果使用的是 [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] 和主机保护者服务 (HGS)，`<attestation protocol>` 应为 `VBS-HGS`。
  - 如果使用的是 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 Microsoft Azure 证明，`<attestation protocol>` 应为 `SGX-AAS`。

- `<attestation URL>` - 指定证明 URL（证明服务终结点）。 你需要从证明服务管理员处获取环境的证明 URL。

  - 如果使用的是 [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] 和主机保护者服务 (HGS)，请参阅[确定并共享 HGS 证明 URL](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url)。
  - 如果使用的是 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 Microsoft Azure 证明，请参阅[确定证明策略的证明 URL](/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#secure-enclave-attestation)。


为数据库连接启用 enclave 计算的连接字符串示例：

- [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.myDomain;Database=myDataBase;Trusted_Connection=Yes;ColumnEncryption=VBS-HGS,http://myHGSServer.myDomain/Attestation
   ```

- [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] :
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.database.windows.net;Database=myDataBase;Uid=myUsername;Pwd=myPassword;Encrypt=yes;ColumnEncryption=SGX-AAS,https://myAttestationProvider.uks.attest.azure.net/attest/SgxEnclave
   ```

如果正确配置了服务器和证明服务，并为相应列的 CMK 和 CEK 启用了 enclave，则除了Always Encrypted 提供的现有功能，现在还应能够执行使用 enclave（如就地加密和丰富计算）的查询。 有关详细信息，请参阅[配置具有安全 Enclave 的 Always Encrypted](../../relational-databases/security/encryption/configure-always-encrypted-enclaves.md)。


### <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>检索和修改加密列中的数据

在连接上启用 Always Encrypted 后，就可以使用标准 ODBC API 了。 ODBC API 可以检索或修改加密数据库列中的数据。 下面的文档项可能会对此有所帮助：

- [ODBC 示例代码](cpp-code-example-app-connect-access-sql-db.md)
- [ODBC 程序员参考](../../odbc/reference/odbc-programmer-s-reference.md)

应用程序必须具有所需的数据库权限，并且必须能够访问列主密钥。 然后，驱动程序对所有定目标到加密列的查询参数进行加密。 驱动程序还会对从加密列中检索到的数据进行解密。 驱动程序执行所有加密和解密操作，而无需源代码的任何帮助。 对于你的程序，就好像列没有加密一样。

如果未启用 Always Encrypted，具有面向加密列的参数的查询将失败。 只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。 但是，驱动程序不会尝试进行任何解密，并且应用程序将收到二进制加密数据（字节数组形式）。

下表概述了查询的行为，具体取决于是否启用了 Always Encrypted：

|查询特征 | 启用了始终加密，并且应用程序可以访问密钥和密钥元数据|启用了始终加密，但应用程序无法访问密钥或密钥元数据 | 禁用了始终加密|
|:---|:---|:---|:---|
| 面向加密列的参数。 | 以透明方式加密参数值。 | 错误 | 错误|
| 从加密列中检索数据，且没有面向加密列的参数。| 以透明方式解密来自加密列的结果。 应用程序收到纯文本列值。 | 错误 | 不解密来自加密列的结果。 应用程序收到字节数组形式的加密值。

以下示例说明如何检索和修改加密列中的数据。 这些示例假定存在具有以下架构的表。 请注意，SSN 和 BirthDate 列已加密。

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
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] )
 GO
```

#### <a name="data-insertion-example"></a>数据插入示例

此示例向 Patients 表插入一行。 注意以下事项：

- 对于示例代码中的加密，没有什么特定的注意事项。 驱动程序自动检测到面向加密列的 SSN 和日期参数的值，并将其加密。 这使得加密操作对应用程序而言是透明的。

- 插入到数据库列（包括加密列）中的值将作为绑定参数传递（请参阅 [SQLBindParameter 函数](../../odbc/reference/syntax/sqlbindparameter-function.md)）。 在将值发送到非加密列时，可以选择使用参数（强烈建议使用它，因为它有助于防止 SQL 注入），而在发送面向加密列的值时，必须使用该参数。 若将插入 SSN 或 BirthDate 列的值传递为嵌入查询声明的文本，查询将失败，因为驱动程序不会尝试加密或处理查询中的文本。 因此，服务器会因为与加密列不兼容而拒绝它们。

- 将插入 SSN 列的参数的 SQL 类型设置为映射 char SQL Server 数据类型 (`rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);`) 的 SQL_CHAR。  如果将该参数的类型设置为映射到 nchar 的 SQL_WCHAR，查询将失败，因为 Always Encrypted 不支持从加密 nchar 值到加密 char 值的服务器端转换。  若要了解数据类型映射，请参阅 [ODBC 程序员参考 -- 附录 D：数据类型](../../odbc/reference/appendixes/appendix-d-data-types.md)。

```
    SQL_DATE_STRUCT date;
    SQLLEN cbdate;   // size of date structure  

    SQLCHAR SSN[12];
    strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

    SQLWCHAR* firstName = L"Catherine";
    SQLWCHAR* lastName = L"Abel";
    SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

    // Initialize the date structure  
    date.day = 10;
    date.month = 9;
    date.year = 1996;

    // Size of structures   
    cbdate = sizeof(SQL_DATE_STRUCT);

    SQLRETURN rc = 0;

    string queryText = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?) ";

    rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

    //SSN
    rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);
    //FirstName
    rc = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)firstName, 0, &cbFirstName);
    //LastName
    rc = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);
    //BirthDate
    rc = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 10, 0, (SQLPOINTER)&date, 0, &cbdate);

    rc = SQLExecute(hstmt);
```

#### <a name="plaintext-data-retrieval-example"></a>纯文本数据检索示例

以下示例演示如何根据加密值筛选数据，以及从加密列中检索纯文本数据。 注意以下事项：

- WHERE 子句中用于筛选 SSN 列的值需要使用 SQLBindParameter 进行传递，以便驱动程序可以在将其发送到数据库之前以透明方式对其加密。

- 程序打印的所有值均为纯文本形式，因为驱动程序将以透明方式解密从 SSN 和 BirthDate 列中检索到的数据。

> [!NOTE]
> 只有在加密是确定性加密或启用了安全 enclave 的情况下，查询才能对加密列执行相等比较。 有关详细信息，请参阅[选择确定性加密或随机加密](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)。

```
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[SSN] = ? ";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//SSN
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="ciphertext-data-retrieval-example"></a>已加密文本数据检索示例

如果未启用始终加密，只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。

以下示例说明如何从加密列中检索二进制加密数据。 注意以下事项：

- 由于未在连接字符串中启用始终加密，因此，查询将以字节数组的形式返回 SSN 和 BirthDate 的加密值（程序会将值转换为字符串）。
- 如果禁用始终加密，从加密列中检索数据的查询可以有参数，但前提是所有参数均不面向加密列。 上述查询按未在数据库中加密的 LastName 进行筛选。 如果查询按 SSN 或 BirthDate 进行筛选，则将失败。


```
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[LastName] = ?";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//LastName
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="moneysmallmoney-encryption"></a>Money/SmallMoney 加密

从驱动程序版本 17.7 开始，可以将 Always Encrypted 与 MONEY 和 SMALLMONEY 一起使用。 不过，还需要执行一些附加步骤。
插入到已加密的 MONEY 或 SMALLMONEY 列时，请使用以下 C 类型之一：
```
SQL_C_CHAR
SQL_C_WCHAR
SQL_C_SHORT
SQL_C_LONG
SQL_C_FLOAT
SQL_C_DOUBLE
SQL_C_BIT
SQL_C_TINYINT
SQL_C_SBIGINT
SQL_C_NUMERIC
```

以及 `SQL_NUMERIC` 或 `SQL_DOUBLE` 的 SQL 类型（使用此类型时可能会丢失精度）。

##### <a name="binding-the-variable"></a>绑定变量

在已加密列中绑定 MONEY/SMALLMONEY 变量时，必须设置以下描述符字段：

```
// n is the descriptor record of the MONEY/SMALLMONEY parameter
// the type is assumed to be SMALLMONEY if isSmallMoney is true and MONEY otherwise

SQLHANDLE ipd = 0;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, (SQLPOINTER)&ipd, SQL_IS_POINTER, NULL);
SQLSetDescField(ipd, n, SQL_CA_SS_SERVER_TYPE, isSmallMoney ? (SQLPOINTER)SQL_SS_TYPE_SMALLMONEY :
                                                              (SQLPOINTER)SQL_SS_TYPE_MONEY, SQL_IS_INTEGER);
                                                              
                                                              
// If the variable is bound as SQL_NUMERIC, additional descriptor fields have to be set
// var is SQL_NUMERIC_STRUCT containing the value to be inserted

SQLHDESC   hdesc = NULL;
SQLGetStmtAttr(hStmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);
SQLSetDescField(hdesc, n, SQL_DESC_PRECISION, (SQLPOINTER)(var.precision), 0);
SQLSetDescField(hdesc, n, SQL_DESC_SCALE, (SQLPOINTER)(var.scale), 0);
SQLSetDescField(hdesc, n, SQL_DESC_DATA_PTR, &var, 0);
```


#### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>避免查询加密列时的常见问题

本节介绍从 ODBC 应用程序查询加密列时的常见错误类别，以及有关如何避免这些错误的若干指导。

##### <a name="unsupported-data-type-conversion-errors"></a>不支持的数据类型转换错误

始终加密支持对加密数据类型进行若干种转换。 有关受支持类型转换的详细列表，请参阅 [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)。 结合使用 SQLBindParameter 和面向加密列的参数时，请务必注意下面几个事项，以避免数据类型转换错误：

- 此参数的 SQL 类型与目标列的类型完全相同，或支持从 SQL 列转换为此列的类型。

- 对于面向列的 `decimal` 和 `numeric` SQL Server 数据类型的参数，其精度和小数位数与为目标列配置的精度和小数位数相同。

- 在用于修改目标列的查询中，面向列的 `datetime2`、`datetimeoffset` 或 `time` SQL Server 数据类型的参数的精度不大于目标列的精度。  

##### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>由于传递纯文本而非加密值而发生的错误

面向加密列的任何值都需要先完成加密，然后才能发送给服务器。 尝试插入、修改或者按纯文本值筛选加密列将导致错误。 为防止发生此类错误，请确保：

- 启用了 Always Encrypted（在连接前通过下面的方式在 DSN 连接字符串中启用：设置特定连接的 `SQL_COPT_SS_COLUMN_ENCRYPTION` 连接属性，或特定声明的 `SQL_SOPT_SS_COLUMN_ENCRYPTION` 声明属性）。

- 使用 SQLBindParameter 发送面向加密列的数据。 以下示例显示了一个查询，该查询按文本/常量对加密列 (SSN) 进行错误筛选，而不是将文本作为参数传递到 SQLBindParameter。

```
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

### <a name="precautions-when-using-sqlsetpos-and-sqlmoreresults"></a>SQLSetPos 和 SQLMoreResults 使用注意事项

#### <a name="sqlsetpos"></a>SQLSetPos

`SQLSetPos` API 允许应用程序使用已绑定 SQLBindCol 的缓冲区更新结果集中的行，并更新到之前拉取行数据的位置。 由于加密固定长度类型的非对称填充行为，在更新行中的其他列时，可能会意外更改这些列的数据。 使用 AE 时，若值小于缓冲区大小，将填充固定长度字符值。

要忽略不作为 `SQLBulkOperations` 的一部分更新的列，或在将 `SQLSetPos` 用于基于游标的更新时，请使用 `SQL_COLUMN_IGNORE` 标记，以缓解此行为。  应忽略应用程序不直接修改的所有列，以提高性能，并避免截断绑定到小于其实际 (DB) 大小的缓冲区的列。  有关详细信息，请参阅 [SQLSetPos 函数参考](../../odbc/reference/syntax/sqlsetpos-function.md)。

#### <a name="sqlmoreresults--sqldescribecol"></a>SQLMoreResults 和 SQLDescribeCol

应用程序的程序可以调用 [SQLDescribeCol](../../odbc/reference/syntax/sqldescribecol-function.md)来返回有关所准备声明中的列的元数据。  启用 Always Encrypted 后，如果在调用 `SQLDescribeCol` 之前  先调用 `SQLMoreResults`，这样会导致调用 [sp_describe_first_result_set](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)，这样就无法正确返回加密列的纯文本元数据。 请先在预定义的声明上调用 `SQLDescribeCol`，然后再调用 *。* `SQLMoreResults`

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>控制 Always Encrypted 对性能的影响

Always Encrypted 是一种客户端加密技术，因此，大部分性能开销发生在客户端，而不是数据库中。 除加密和解密操作的成本之外，客户端上的其他性能开销来源包括：

- 额外往返数据库以检索查询参数的元数据。

- 调用列主密钥存储以访问列主密钥。

本节介绍适用于 SQL Server 的 ODBC 驱动程序中的内置性能优化，以及如何控制上述两个因素对性能的影响。

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>控制为了检索查询参数的元数据而往返的次数

如果为连接启用了 Always Encrypted，默认情况下，驱动程序将为每个参数化查询调用 [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)，并将查询语句（不带任何参数值）传递到 SQL Server。 此存储过程会分析查询语句，以了解是否有任何参数需要加密；如果有，则会针对每个参数返回加密相关信息，以便驱动程序对它们加密。 以上行为可确保实现针对客户端应用程序的高级别透明性：应用程序（和应用程序开发人员）不需要知道哪些查询在访问加密列，只需将面向加密列的值传递到参数中的驱动程序即可。

从 17.6 版开始，驱动程序还会缓存预定义语句的加密元数据，通过允许后续对 `SQLExecute` 的调用不需要额外的往返来检索加密元数据，从而提高性能。

### <a name="per-statement-always-encrypted-behavior"></a>每个语句的 Always Encrypted 行为

若要控制检索参数化查询的加密元数据时对性能的影响，在已为连接启用 Always Encrypted 的情况下，可以为单个查询更改 Always Encrypted 行为。 这样一来，就可以确保仅针对你知道具有面向加密列的参数的查询调用 `sys.sp_describe_parameter_encryption`。 但请注意，这样做会降低加密的透明度：如果加密数据库中的其他列，可能需要更改应用程序代码，使其与架构更改保持一致。

请调用 SQLSetStmtAttr，将 `SQL_SOPT_SS_COLUMN_ENCRYPTION` 语句属性设置为下列值之一，以便控制语句的 Always Encrypted 行为：

|值|说明|
|-|-|
|`SQL_CE_DISABLED` (0)|对语句禁用了 Always Encrypted|
|`SQL_CE_RESULTSETONLY` (1)|仅解密。 解密结果集和返回值，不对参数加密|
|`SQL_CE_ENABLED` (3) | 启用 Always Encrypted，并将其用于参数和结果|

由已启用 Always Encrypted 的连接（默认值为 SQL_CE_ENABLED）生成的新语句句柄。 由已禁用 Always Encrypted 的连接生成的句柄（默认值为 SQL_CE_DISABLED）（并且无法对它们启用 Always Encrypted）。

若客户端应用程序的多数查询都访问加密列，推荐使用下面的值：

- 将 `ColumnEncryption` 连接字符串关键字设置为 `Enabled`。

- 在不访问任何加密列的语句上将 `SQL_SOPT_SS_COLUMN_ENCRYPTION` 属性设置为 `SQL_CE_DISABLED`。 这将禁止调用 `sys.sp_describe_parameter_encryption`，并阻止解密结果集中的任何值。
    
- 如果语句不包含任何需要加密的参数，但从加密列检索数据，则将 `SQL_SOPT_SS_COLUMN_ENCRYPTION` 属性设置为 `SQL_CE_RESULTSETONLY`。 这将禁止调用 `sys.sp_describe_parameter_encryption` 和参数加密。 将继续解密包含加密列的结果。

- 对将多次执行的查询使用预定义语句；使用 `SQLPrepare` 准备查询并保存语句句柄，并在每次执行时将其与 `SQLExecute` 一起重用。 即使没有加密列，这也是性能的首选方法，并允许驱动程序利用缓存的元数据。

## <a name="always-encrypted-security-settings"></a>Always Encrypted 安全性设置

### <a name="force-column-encryption"></a>强制执行列加密

若要强制执行参数加密，请通过 SQLSetDescField 函数调用设置 `SQL_CA_SS_FORCE_ENCRYPT` 实现参数描述符 (IPD) 字段。 如果未针对关联的参数返回加密元数据，则非零值会导致驱动程序返回错误。

```
SQLHDESC ipd;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, &ipd, 0, 0);
SQLSetDescField(ipd, paramNum, SQL_CA_SS_FORCE_ENCRYPT, (SQLPOINTER)TRUE, SQL_IS_SMALLINT);   
```

如果 SQL Server 告知驱动程序参数不需加密，则使用该参数的查询会失败。 这提供针对安全攻击的额外保护，这些攻击涉及受损 SQL Server 向客户端提供不正确的加密元数据，这可能会导致数据泄漏。

### <a name="column-encryption-key-caching"></a>列加密密钥缓存

为了减少对列加密密钥解密时调用列主密钥存储的次数，驱动程序会将纯文本 CEK 缓存在内存中。 CEK 缓存是面向驱动程序的全局缓存，未关联任何一个连接。 从数据库元数据收到 ECEK 之后，驱动程序首先会尝试查找与缓存中的加密密钥值对应的纯文本 CEK。 只有在驱动程序无法从缓存中找到对应的纯文本 CEK 时，才会调用包含 CMK 的密钥存储。

> [!NOTE]
> 在 ODBC Driver for SQL Server 中，超时 2 个小时后将收回缓存中的项。 这意味着，对于某个给定的 ECEK，驱动程序在应用程序生存期间只会访问密钥存储一次，或每两个小时一次（无论哪种情况，持续时间都较短）。

从 ODBC Driver 17.1 for SQL Server 起，可以使用 `SQL_COPT_SS_CEKCACHETTL` 连接属性指定 CEK 在缓存中驻留的秒数，来调整 CEK 缓存超时。 由于缓存是全局缓存，因此可以从任何对驱动程序有效的连接句柄调整该属性。 缓存生存时间缩短时，同时将收回超出新生存时间的现有 CEK。 若为 0，将不缓存任何 CEK。

### <a name="trusted-key-paths"></a>受信任的密钥路径

从 ODBC Driver 17.1 for SQL Server 开始，`SQL_COPT_SS_TRUSTEDCMKPATHS` 连接属性允许应用程序要求：Always Encrypted 操作仅使用其密钥路径标识的 CMK 指定列表。 默认情况下，此属性为 null，即表示驱动程序接受任何密钥路径。 若要使用此功能，请设置 `SQL_COPT_SS_TRUSTEDCMKPATHS`，以指向以 null 分隔 null 结尾的宽字符串，它将列出允许的密钥路径。 在使用设置该属性的连接句柄加密或解密时，此属性指向的内存必须可用 - 驱动程序将在它上面检查服务器元数据指定的 CMK 路径是否在此列表中（不区分大小写）。 若 CMK 路径未在列表中，操作将失败。 应用程序可以更改此属性指向的内存的内容，这样无需再重新设置此属性，即可更改其受信任 CMK 列表。

## <a name="working-with-column-master-key-stores"></a>使用列主密钥存储

驱动程序需要获取为目标列配置的 CEK，才能加密或解密数据。 CEK 以加密形式 (ECEK) 存储在数据库元数据中。 每个 CEK 均有一个用于加密其自身的相应 CMK。 [数据库元数据](../../t-sql/statements/create-column-master-key-transact-sql.md)本身不存储 CMK，它只包含密钥存储的名称以及密钥存储可用于查找 CMK 的信息。

若要获取 ECEK 的纯文本值，驱动程序首先要获取 CEK 及其相应的 CMK 的元数据，然后使用此信息联系包含 CMK 的密钥储存，并请求它将 ECEK 解密。 驱动程序使用密钥存储提供程序与密钥存储通信。

### <a name="built-in-keystore-providers"></a>内置密钥存储提供程序

适用于 SQL Server 的 ODBC 包含下列内置密钥存储提供程序：

| 名称 | 说明 | 提供程序（元数据）名称 |可用性|
|:---|:---|:---|:---|
|Azure Key Vault |将 CMK 存储在 Azure 密钥保管库中 | `AZURE_KEY_VAULT` |Windows、macOS、Linux|
|Windows 证书存储|将 CMK 存储在本地 Windows 密钥存储中| `MSSQL_CERTIFICATE_STORE`|Windows|

- 你（或你的 DBA）需要确保列主密钥元数据中配置的提供程序名称正确，并且列主密钥路径符合对于给定提供程序的密钥路径格式。 建议你使用诸如 SQL Server Management Studio 之类的工具来配置密钥，这类工具在发出 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/create-column-master-key-transact-sql.md) 语句时会自动生成有效的提供程序名称和密钥路径。

- 需要确保应用程序可以访问密钥存储中的密钥。 这可能涉及向应用程序授予对密钥和/或密钥存储的访问权限（具体取决于密钥存储），或执行其他特定于密钥存储的配置步骤。 例如，需要提供密钥存储的相应凭据，才能访问 Azure 密钥保管库。

### <a name="using-the-azure-key-vault-provider"></a>使用 Azure Key Vault 提供程序

Azure Key Vault (AKV) 便于存储和管理用于 Always Encrypted 的列主密钥（尤其是当应用程序在 Azure 中托管时）。 适用于 Linux、macOS 和 Windows 上的 SQL Server 的 ODBC 驱动程序包含用于 Azure 密钥保管库的内置列主密钥存储提供程序。 有关为 Azure 密钥保管库设置 Always Encrypted 的详细信息，请参阅 [Azure 密钥保管库 - 分步说明](/archive/blogs/kv/azure-key-vault-step-by-step)、[密钥保管库入门](/azure/key-vault/general/overview)以及[在 Azure 密钥保管库中创建列主密钥](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault)。

> [!NOTE]
> ODBC 驱动程序仅支持对 Azure Active Directory 直接进行 AKV 身份验证。 如果对 AKV 使用的是 Azure Active Directory 身份验证，并且 Active Directory 配置要求针对 Active Directory 联合服务终结点进行身份验证，则身份验证可能会失败。
> 对于 Linux 和 macOS 上的驱动程序版本 17.2 以及更高版本，需要提供 `libcurl` 才能使用此提供程序，但这不是显式依赖项，因为对驱动程序执行的其他操作不需要它。 如果遇到有关 `libcurl` 的错误，请确保它已安装。

驱动程序支持使用下列凭据类型对 Azure 密钥保管库进行身份验证：

- 用户名/密码 - 使用此方法时，凭据是 Azure Active Directory 用户的名称及其密码。

- 客户端 ID/机密 - 使用此方法时，凭据是应用程序客户端 ID 及应用程序机密。

- 托管标识 (17.5.2+) - 系统或用户分配；有关详细信息，请参阅 [Azure 资源的托管标识](/azure/active-directory/managed-identities-azure-resources/)。

- Azure Key Vault 交互（17.7+ Windows 驱动程序）- 使用此方法时，将使用登录 ID 通过 Azure Active Directory 对凭据进行身份验证。

若要允许驱动程序将 AKV 存储的 CMK 用于列加密，请使用下列仅连接字符串关键字：

|凭据类型|<code>KeyStoreAuthentication</code>|<code>KeyStorePrincipalId</code>|<code>KeyStoreSecret</code>|
|-|-|-|-|
|用户名/密码| `KeyVaultPassword`|用户主体名称|密码|
|客户端 ID/机密| `KeyVaultClientSecret`|客户端 ID|机密|
|托管标识|`KeyVaultManagedIdentity`|对象 ID（可选，仅用于用户分配）|（未指定）|
|AKV 交互|`KeyVaultInteractive`|(未设置)|(未设置)|

#### <a name="example-connection-strings"></a>连接字符串示例

下面的链接字符串显示了如何使用两种凭据类型对 Azure 密钥保管库进行身份验证：

**客户端 ID/机密**：

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<clientId>;KeyStoreSecret=<secret>
```

**用户名/密码**：

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultPassword;KeyStorePrincipalId=<username>;KeyStoreSecret=<password>
```

**托管标识（系统分配）**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity
```

**托管标识（用户分配）**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity;KeyStorePrincipalId=<objectID>
```

**AKV 交互**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultInteractive;UID=<userID>;PWD=<password>
```

将 AKV 用于 CMK 存储无需其他 ODBC 应用程序更改。

> [!NOTE]
> 驱动程序包含它信任的 AKV 终结点的列表。 自驱动程序版本 17.5.2 起，此列表是可配置的：可以将驱动程序或 DSN 的 ODBCINST.INI 或 ODBC.INI 注册表项 (Windows) 或 `odbcinst.ini`/`odbc.ini` 文件部分 (Linux/macOS) 中的 `AKVTrustedEndpoints` 属性设置为分号分隔列表。 在 DSN 中进行设置的优先级高于在驱动程序中设置。 如果该值以分号开头，则它将扩展默认列表；否则，它将替换默认列表。 默认列表（自 17.5 起）为 `vault.azure.net;vault.azure.cn;vault.usgovcloudapi.net;vault.microsoftazure.de`。 从 17.7 开始，列表还包括 `managedhsm.azure.net;managedhsm.azure.cn;managedhsm.usgovcloudapi.net;managedhsm.microsoftazure.de`。


### <a name="using-the-windows-certificate-store-provider"></a>使用 Windows 证书存储提供程序

适用于 Windows 上的 SQL Server 的 ODBC 驱动程序包含用于 Windows 证书存储的内置列主密钥存储提供程序 `MSSQL_CERTIFICATE_STORE`。 （此提供程序在 macOS 或 Linux 上不可用。）此提供程序会将 CMK 存储在客户端计算机本地，应用程序无需额外配置即可将它与驱动程序配合使用。 但是，应用程序必须有权访问存储中的证书及其私钥。 有关详细信息，请参阅[创建并存储列主密钥 (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

### <a name="using-custom-keystore-providers"></a>使用自定义密钥存储提供程序

ODBC Driver for SQL Server 同时支持使用 CEKeystoreProvider 接口的自定义第三方密钥存储提供程序。 这允许应用程序加载、查询和配置密钥存储提供程序，这样驱动程序即可使用它们访问加密列。 此外，应用程序还可以直接与密钥存储提供程序交互，以加密 SQL Server 存储的 CEK，并执行使用 ODBC 访问加密列之外的任务；有关详细信息，请参阅[自定义密钥存储提供程序](custom-keystore-providers.md)。

使用两个连接属性与自定义密钥存储提供程序进行交互。 它们分别是：

- `SQL_COPT_SS_CEKEYSTOREPROVIDER`

- `SQL_COPT_SS_CEKEYSTOREDATA`

使用前者加载并枚举已加载的密钥存储提供程序，使用后者实现应用程序提供程序通信。 随时可使用这两个连接属性，在建立连接之前或之后均可，因为应用程序提供程序交互不涉及与 SQL Server 通信。 但是，由于尚未加载驱动程序，因此如果在连接前设置并获取这些属性，将由驱动程序管理器处理它们，并可能不会生成预期结果。

#### <a name="loading-a-keystore-provider"></a>加载密钥存储提供程序

设置 `SQL_COPT_SS_CEKEYSTOREPROVIDER` 连接属性将允许客户端应用程序加载提供程序库，从而可以使用其中包含的密钥存储提供程序。

```
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| 参数 | 说明 |
|:---|:---|
|`ConnectionHandle`|[输入] 连接句柄。 必须为有效连接句柄，但在同一个进程中，可以通过其他任何连接句柄访问由某个连接句柄加载的提供程序。|
|`Attribute`|[输入] 要设置的属性：`SQL_COPT_SS_CEKEYSTOREPROVIDER` 常量。|
|`ValuePtr`|[输入] 指向以 null 结尾并指定提供程序库文件名的字符串的指针。 对于 SQLSetConnectAttrA，此为 ANSI（多字节）字符串。 对于 SQLSetConnectAttrW，此为 Unicode (wchar_t) 字符串。|
|`StringLength`|[输入] ValuePtr 字符串的长度，或 SQL_NTS。|

驱动程序尝试使用平台定义的动态库加载机制加载 ValuePtr 参数标识的库（在 Linux 和 macOS 上为 `dlopen()`，在 Windows 上为 `LoadLibrary()`），并将其中定义的任何提供程序添加到驱动程序已知的提供程序列表。 可能会出现以下错误：

| 错误 | 说明 |
|:--|:--|
|`CE203`|未能加载动态库。|
|`CE203`|无法在库中找到“CEKeyStoreProvider”导出符号。|
|`CE203`|已从库中加载一个或多个提供程序。|

`SQLSetConnectAttr` 返回常规错误或成功值，可通过标准的 ODBC 诊断机制获取所出现的任何错误的详细信息。

> [!NOTE]
> 应用程序程序员必须确保，在通过任何连接发送需要任意自定义提供程序的任意查询前，已加载这些提供程序。 如果没有这样做，会导致错误：

| 错误 | 说明 |
|:--|:--|
|`CE200`|找不到密钥存储提供程序 %1。 请确保已加载适当的密钥存储提供程序库。|

> [!NOTE]
> 密钥存储提供程序实现器应避免以其自定义提供程序的名义使用 `MSSQL`。 此术语专用于 Microsoft，可能会与未来的内置提供程序冲突。 以自定义提供程序的名义使用此术语可能会引发 ODBC 警告。

#### <a name="getting-the-list-of-loaded-providers"></a>获取已加载提供程序列表

获取此连接属性后，客户端应用程序即可以确定驱动程序当前已加载的密钥存储提供程序（包括内置的提供程序）。此操作只能在完成连接后执行。

```
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| 参数 | 说明 |
|:---|:---|
|`ConnectionHandle`|[输入] 连接句柄。 必须为有效连接句柄，但在同一个进程中，可以通过其他任何连接句柄访问由某个连接句柄加载的提供程序。|
|`Attribute`|[输入] 要检索的属性：`SQL_COPT_SS_CEKEYSTOREPROVIDER` 常量。|
|`ValuePtr`|[输出] 指向会返回下一个已加载提供程序名称的内存的指针。|
|`BufferLength`|[输入] 缓冲区 ValuePtr 的长度。|
|`StringLengthPtr`|[输出] 指向会返回 \*ValuePtr 可返回的字节总数的缓冲区的指针（不包括以 null 结尾的字符）。 若 ValuePtr 为 null 指针，将不返回任何长度。 若属性值为字符串，并且可返回的字节数大于减去 null 结尾字符长度后的缓冲区长度，\* 中的数据将截断到减去 null 结尾字符长度后的缓冲区长度，并且驱动程序会以 null 为它结尾。|

每个 Get 操作将返回当前提供程序的名称，并使内部计数器增加到下一个计数，以便可以检索整个列表。 此计数器访问列表尾部后，将返回空字符串 ("")，并重置计数器；后续 Get 操作将继续重新从列表开头部分开始。

### <a name="communicating-with-keystore-providers"></a>与密钥存储提供程序通信

`SQL_COPT_SS_CEKEYSTOREDATA` 连接属性允许客户端应用程序与用于配置其他参数、密钥内容等的已加载密钥存储提供程序通信。客户端应用程序和提供程序之间的通信基于使用此连接属性的 Get 和 Set 请求按照简单的请求-响应协议进行。 仅客户端应用程序可以启动通信。

> [!NOTE]
> 由于 CEKeyStoreProvider 响应的 ODBC 调用的特性 (SQLGet/SetConnectAttr)，ODBC 接口仅支持在解析连接上下文时设置数据。

应用程序通过 CEKeystoreData 结构借助驱动程序与密钥存储提供程序进行通信：

```
typedef struct CEKeystoreData {
wchar_t *name;
unsigned int dataSize;
char data[];
} CEKEYSTOREDATA;
```

| 参数 | 说明 |
|:---|:---|
|`name`|[输入] 在 Set 上，为数据要发送到的提供程序的名称。 在 Get 上将忽略它。 以 Null 结尾的宽字符串。|
|`dataSize`|[输入] 结构后面的数据数组的大小。|
|`data`|[InOut] 在 Set 上，为要发送到提供程序的数据。 它可以是任意数据；驱动程序不会尝试解释它。 在 Get 上，为要接收从提供程序读取的数据的缓冲区。|

#### <a name="writing-data-to-a-provider"></a>将数据写入到提供程序

`SQLSetConnectAttr` 调用使用 `SQL_COPT_SS_CEKEYSTOREDATA` 属性将数据“包”写入到指定的密钥存储提供程序。
```
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| 参数 | 说明 |
|:---|:---|
|`ConnectionHandle`| [输入] 连接句柄。 必须为有效连接句柄，但在同一个进程中，可以通过其他任何连接句柄访问由某个连接句柄加载的提供程序。|
|`Attribute`|[输入] 要设置的属性：`SQL_COPT_SS_CEKEYSTOREDATA` 常量。|
|`ValuePtr`|[输入] CEKeystoreData 结构放入指针。 结构的名称字段指示数据适用的提供程序。|
|`StringLength`|[输入] SQL_IS_POINTER 常量|

若要了解更多错误详细信息，可参阅 [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md)。

> [!NOTE]
> 在必要的情况下，提供程序可以使用连接句柄，来将写入的数据与特定连接关联。 这将有助于实现基于连接的配置。 它还可忽略连接上下文，并且无论用于发送数据的连接是什么，均以相同方式处理数据。 有关详细信息，请参阅[上下文关联](custom-keystore-providers.md#context-association)。

#### <a name="reading-data-from-a-provider"></a>从提供程序读取数据

使用 `SQLGetConnectAttr` 属性调用 `SQL_COPT_SS_CEKEYSTOREDATA` 后，将从上次写入到的提供程序读取数据“包”。  若不存在这样的提供程序，将出现“函数序列错误”。 如有必要，建议密钥存储提供程序实施者支持 0 字节大小的“虚拟写入”，以在不造成其他负面影响的情况下选择读取操作提供程序。

```
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| 参数 | 说明 |
|:---|:---|
|`ConnectionHandle`|[输入] 连接句柄。 必须为有效连接句柄，但在同一个进程中，可以通过其他任何连接句柄访问由某个连接句柄加载的提供程序。|
|`Attribute`|[输入] 要检索的属性：`SQL_COPT_SS_CEKEYSTOREDATA` 常量。|
|`ValuePtr`|[输出] 指向 CEKeystoreData 结构（从提供程序读取的数据位于其中）的指针。|
|`BufferLength`|[输入] SQL_IS_POINTER 常量|
|`StringLengthPtr`|[输出] 指向 BufferLength 要返回到的缓冲区的指针。 若 *ValuePtr 为 null 指针，将不返回任何长度。|

调用方必须确保，为要写入到的提供程序分配了采用 CEKEYSTOREDATA 结构且长度足够长的缓冲区。 返回后，其 dataSize 字段将更新为从提供程序读取的数据的实际长度。 若要了解更多错误详细信息，可参阅 [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md)。

对于应用程序和密钥存储提供程序之间的传输数据的格式，此接口没有额外要求。 各个提供程序可以根据自身需要定义自己的协议/数据格式。

有关实现自己的密钥存储提供程序的示例，请参阅[自定义密钥存储提供程序](custom-keystore-providers.md)

## <a name="limitations-of-the-odbc-driver-when-using-always-encrypted"></a>使用 Always Encrypted 时的 ODBC 驱动程序限制

### <a name="asynchronous-operations"></a>异步操作
尽管 ODBC 驱动程序允许结合使用[异步操作](../../relational-databases/native-client/odbc/creating-a-driver-application-asynchronous-mode-and-sqlcancel.md)和 Always Encrypted，但是在启用 Always Encrypted 后，将对异步操作的性能产生影响。 用于确定语句加密元数据的 `sys.sp_describe_parameter_encryption` 调用受阻，并将导致驱动程序要先等待服务器返回元数据后，才能返回 `SQL_STILL_EXECUTING`。

### <a name="retrieve-data-in-parts-with-sqlgetdata"></a>使用 SQLGetData 部分检索数据
在 ODBC Driver 17 for SQL Server 之前，无法使用 SQLGetData 部分检索加密字符和二进制列。 只能使用一个长度够长的缓冲区（用于包含整个列数据）执行 SQLGetData 调用一次。

### <a name="send-data-in-parts-with-sqlputdata"></a>使用 SQLPutData 部分发送数据
在 ODBC Driver 17.3 for SQL Server 之前，无法使用 SQLPutData 部分发送要插入或比较的数据。 只能使用包含完整数据的缓冲区执行一次 SQLPutData 调用。 若要将长数据插入加密列，请使用输入数据文件利用批量复制 API 完成，具体步骤如下节所述。

### <a name="encrypted-money-and-smallmoney"></a>加密的 money 和 smallmoney
参数无法面向加密的 money 或 smallmoney 列，因为没有映射这些类型的特定 ODBC 数据类型，从而会引发操作数类型冲突错误。  

## <a name="bulk-copy-of-encrypted-columns"></a>批量复制加密列

自 ODBC Driver 17 for SQL Server 起，支持将 [SQL 批量复制函数](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)和 bcp 实用工具与 Always Encrypted 结合使用。  可使用批量复制 (bcp_&#42;) API 和 bcp 实用工具插入和检索纯文本（用于插入时将加密，用于检索时将解密）和已加密文本（已转换的逐字字符串）  。

- 若要检索使用 varbinary(max) 格式的已加密文本（如用于批量加载到其他数据库），请以不使用 `ColumnEncryption` 选项的方式连接（或将其设置为 `Disabled`），并执行 BCP OUT 操作。

- 若要插入并检索纯文本，并允许驱动程序根据需要以透明方式执行加密和解密，将 `ColumnEncryption` 设置为 `Enabled` 即可。 BCP API 的功能不变。

- 若要插入使用 varbinary(max) 格式的已加密文本（如前面检索到的内容），请将 `BCPMODIFYENCRYPTED` 选项设置为 TRUE，并执行 BCP IN 操作。 请确保目标列的 CEK 与已加密文本的初始获取位置的 CEK 相同，以便生成的数据可解密。

如果使用的是 bcp  实用工具：若要控制 `ColumnEncryption` 设置，请使用 -D 选项，并指定包含所需值的 DSN。 若要插入已加密文本，请确保启用了用户的 `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` 设置。

下表汇总了作用于加密列时的操作：

|<code>ColumnEncryption</code>|BCP 方向|说明|
|----------------|-------------|-----------|
|`Disabled`|OUT（面向客户端）|检索已加密文本。 观察到的数据类型为 varbinary(max)。 |
|`Enabled`|OUT（面向客户端）|检索纯文本。 驱动程序将解密列数据。|
|`Disabled`|IN（面向服务器）|插入已加密文本。 它专门用于在无需解密加密数据的情况下以不透明方式移动加密数据。 若未为用户设置 `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` 选项，或未为连接句柄设置 BCPMODIFYENCRYPTED，操作将失败。 有关详细信息，请参阅下文。|
|`Enabled`|IN（面向服务器）|插入纯文本。 驱动程序将加密列数据。|

### <a name="the-bcpmodifyencrypted-option"></a>BCPMODIFYENCRYPTED 选项

为了防止数据损坏，服务器通常不允许直接将已加密文本插入加密列，因此此类操作将失败；不过，对于使用 BCP API 批量加载加密数据，将 `BCPMODIFYENCRYPTED` [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) 选项设置为 TRUE，即可允许直接插入已加密文本，并降低了为用户帐户设置 `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` 选项导致加密数据损坏的风险。 即使如此，密钥也必须与数据匹配，并且在完成批量插入后以及进一步使用数据前，最好对插入的数据执行一些只读检查。

有关详细信息，请参阅[迁移通过 Always Encrypted 保护的敏感数据](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md)。

## <a name="always-encrypted-api-summary"></a>Always Encrypted API 摘要

### <a name="connection-string-keywords"></a>连接字符串关键字

|名称|说明|  
|----------|-----------------|  
|`ColumnEncryption`|接受的值为 `Enabled`/`Disabled`。<br>`Enabled` - 启用或针对连接的 Always Encrypted 功能。<br>`Disabled` - 禁用针对连接的 Always Encrypted 功能。<br>证明协议、证明 URL --（版本 17.4 和更高版本）使用指定的证明协议和证明 URL 启用具有安全 enclave 的 Always Encrypted。 <br><br>默认为 `Disabled`。|
|`KeyStoreAuthentication` | 有效值：`KeyVaultPassword`、`KeyVaultClientSecret`、`KeyVaultInteractive` |
|`KeyStorePrincipalId` | 为 `KeyStoreAuthentication` = `KeyVaultPassword` 时，将此值设置为有效的 Azure Active Directory 用户主体名称。 <br>为 `KeyStoreAuthetication` = `KeyVaultClientSecret` 时，将此值设置为有效的 Azure Active Directory 应用程序客户端 ID |
|`KeyStoreSecret` | 为 `KeyStoreAuthentication` = `KeyVaultPassword` 时，将此值设置为相应用户名的密码。 <br>为 `KeyStoreAuthentication` = `KeyVaultClientSecret` 时，将此值设置为 Azure Active Directory 应用程序客户端 ID 关联的应用程序机密 |


### <a name="connection-attributes"></a>连接属性

|名称|类型|说明|  
|----------|-------|----------|  
|`SQL_COPT_SS_COLUMN_ENCRYPTION`|预连接|`SQL_COLUMN_ENCRYPTION_DISABLE` (0) -- 禁用 Always Encrypted <br>`SQL_COLUMN_ENCRYPTION_ENABLE` (1) -- 启用 Always Encrypted<br> 指向证明协议的指针、证明 URL 字符串 -（版本 17.4 及更高版本）使用安全 enclave 启用|
|`SQL_COPT_SS_CEKEYSTOREPROVIDER`|后连接|[Set] 尝试加载 CEKeystoreProvider<br>[Get] 返回 CEKeystoreProvider 名称|
|`SQL_COPT_SS_CEKEYSTOREDATA`|后连接|[Set] 将数据写入 CEKeystoreProvider<br>[Get] 从 CEKeystoreProvider 读取数据|
|`SQL_COPT_SS_CEKCACHETTL`|后连接|[Set] 设置 CEK 缓存 TTL<br>[Get] 获取当前 CEK 缓存 TTL|
|`SQL_COPT_SS_TRUSTEDCMKPATHS`|后连接|[Set] 设置受信任的 CMK 路径指针<br>[Get] 获取当前受信任的 CMK 路径指针|

### <a name="statement-attributes"></a>语句属性

|名称|说明|  
|----------|-----------------|  
|`SQL_SOPT_SS_COLUMN_ENCRYPTION`|`SQL_CE_DISABLED` (0) -- 对语句禁用了 Always Encrypted <br>`SQL_CE_RESULTSETONLY` (1) -- 仅解密。 解密结果集和返回值，不对参数加密 <br>`SQL_CE_ENABLED` (3) -- 启用 Always Encrypted，并将其用于参数和结果|

### <a name="descriptor-fields"></a>描述符字段

|IPD 字段|大小/类型|默认值|说明|
|-|-|-|-|  
|`SQL_CA_SS_FORCE_ENCRYPT` (1236)|单词（2 个字节）|0|如果值为 0（默认值）：决定是否加密此参数取决于加密元数据的可用性。<br><br>为非零值时：若提供了此参数的加密元数据，将加密它。 否则，请求将由于下面的错误而失败：[CE300] [Microsoft][ODBC Driver 17 for SQL Server] 对参数指定了强制加密，但服务器未提供任何加密元数据。|

### <a name="bcp_control-options"></a>bcp_control 选项

|选项名称|默认值|说明|
|-|-|-|
|`BCPMODIFYENCRYPTED` (21)|FALSE|如果值为 TRUE，可以将 varbinary(max) 值插入加密列。 如果为 FALSE，除非提供正确的类型和加密元数据，否则将阻止插入。|

## <a name="troubleshooting"></a>疑难解答

如果使用 Always Encrypted 时遇到问题，请先检查以下几点：

- 加密相应列的 CEK 在服务器上存在且可访问。

- 加密 CEK 的 CMK 在服务器上有可访问的元数据，并能从客户端进行访问。

- `ColumnEncryption` 在 DSN、连接字符串或连接属性中启用，并采用正确格式（如果使用安全 enclave 的话）。

此外，在使用安全 enclave 时，证明失败会根据下表确定证明过程中出现故障的步骤：

|步骤|说明|
|----|-----------|
|0-99| 证明响应无效，或签名验证错误。 |
|100-199| 从证明 URL 检索证书时出错。 请确保 `<attestation URL>/v2.0/signingCertificates` 有效且可访问。 |
|200-299| enclave 标识的格式异常或不正确。 |
|300-399| 与 enclave 建立安全通道时出错。 |

## <a name="see-also"></a>另请参阅

- [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [具有安全 Enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)
