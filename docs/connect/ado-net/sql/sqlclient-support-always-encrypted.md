---
title: 结合使用 Always Encrypted 和 SqlClient
description: 了解如何使用 Microsoft.Data.SqlClient 和 Always Encrypted 开发应用程序，以确保数据安全。
ms.date: 11/16/2020
ms.assetid: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: cheenamalhotra
ms.author: v-chmalh
ms.reviewer: v-kaywon
ms.openlocfilehash: bb971ed9fdc24491babf1ce9fe777210778037de
ms.sourcegitcommit: 4c3949f620d09529658a2172d00bfe37aeb1a387
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2020
ms.locfileid: "96123904"
---
# <a name="using-always-encrypted-with-the-microsoft-net-data-provider-for-sql-server"></a>结合使用 Always Encrypted 和 Microsoft .NET Data Provider for SQL Server

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

本文介绍了如何使用 [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 或[具有安全 Enclave 的 Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-enclaves.md) 和 [Microsoft .NET Data Provider for SQL Server  ](../microsoft-ado-net-sql-server.md) 开发 .NET 应用程序。

始终加密允许客户端应用程序对敏感数据进行加密，并且永远不向 SQL Server 或 Azure SQL 数据库显示该数据或加密密钥。 为此，已启用 Always Encrypted 的驱动程序（如 Microsoft .NET Data Provider for SQL Server  ）通过在客户端应用程序中以透明方式对敏感数据进行加密和解密。 该驱动程序自动确定哪些查询参数与敏感数据库列（使用始终加密进行保护）相对应，并对这些参数的值进行加密，然后再将数据传递到 SQL Server 或 Azure SQL 数据库。 同样，该驱动程序以透明方式对查询结果中从加密数据库列检索到的数据进行解密。 有关详细信息，请参阅[使用 Always Encrypted 开发应用程序](../../../relational-databases/security/encryption/always-encrypted-client-development.md)和[使用具有安全 Enclave 的 Always Encrypted 开发应用程序](../../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md)。

## <a name="prerequisites"></a>先决条件

- 在数据库中配置始终加密。 这涉及为选定数据库列预配始终加密密钥和设置加密。 如果还没有配置具有 Always Encrypted 的数据库，请按照 [Always Encrypted 入门](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted)中的说明操作。
- 请确保已在开发计算机上安装相应 .NET 平台。 借助 [Microsoft.Data.SqlClient](../microsoft-ado-net-sql-server.md)，.NET Framework 和 .NET Core 都支持 Always Encrypted 功能。 确保将 [.NET Framework 4.6](/dotnet/framework/) 或更高版本/[.NET Core 2.1](/dotnet/core/) 或更高版本配置为开发环境中的目标 .NET 平台版本。 从 Microsoft.Data.SqlClient 版本 2.1.0 开始，Always Encrypted 功能在 [.NET Standard 2.0](/dotnet/standard/net-standard) 中也受支持。 要使用具有安全 Enclave 的 Always Encrypted，需要使用 [.NET Standard 2.1](/dotnet/standard/net-standard)。 如果使用的是 Visual Studio，请参阅[框架目标概述](/visualstudio/ide/visual-studio-multi-targeting-overview)。

下表汇总了结合使用 Always Encrypted 和 Microsoft.Data.SqlClient 所需的 .NET 平台。

| 支持 Always Encrypted | 支持具有安全 Enclave 的 Always Encrypted  | 目标 Framework | Microsoft.Data.SqlClient 版本 | 操作系统 |
|:--|:--|:--|:--:|:--:|
| 是 | 是 | .NET Framework 4.6 及更高版本 | 1.1.0+ | Windows |
| 是 | 是 | .NET Core 2.1+ | 2.1.0+<sup>1</sup> | Windows、Linux、macOS |
| 是 | 否 | .NET Standard 2.0 | 2.1.0+ | Windows、Linux、macOS |
| 是 | 是 | .NET Standard 2.1+ | 2.1.0+ | Windows、Linux、macOS |

> [!NOTE]
> <sup>1</sup> 在 Microsoft.Data.SqlClient 版本 2.1.0 之前，Always Encrypted 仅在 Windows 上受支持。 

## <a name="enabling-always-encrypted-for-application-queries"></a>为应用程序查询启用始终加密

若要启用对参数加密，以及对定目标到加密列的查询结果解密，最简单的方法是将 `Column Encryption Setting` 连接字符串关键字的值设置为 enabled  。

以下示例使用启用 Always Encrypted 的连接字符串：

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
SqlConnection connection = new SqlConnection(connectionString);
```

以下代码片段是使用 SqlConnectionStringBuilder.ColumnEncryptionSetting 属性的等效示例。

```cs
SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
builder.DataSource = "server63";
builder.InitialCatalog = "Clinic";
builder.IntegratedSecurity = true;
builder.ColumnEncryptionSetting = SqlConnectionColumnEncryptionSetting.Enabled;
SqlConnection connection = new SqlConnection(builder.ConnectionString);
connection.Open();
```

还可以为单个查询启用始终加密。 请参阅下面的 **控制 Always Encrypted 对性能的影响** 部分。
启用 Always Encrypted 不足以成功实现加密或解密。 你还需要确保：

- 应用程序具有 *查看任意列主密钥定义* 和 *查看任意列加密密钥定义* 数据库权限，这是访问数据库中始终加密密钥的相关元数据所必需的权限。 有关详细信息，请参阅 [Always Encrypted（数据库引擎）中的“数据库权限”部分](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions)。
- 应用程序可以访问用于保护列加密密钥的列主密钥，以便对查询到的数据库列加密。

## <a name="enabling-always-encrypted-with-secure-enclaves"></a>启用具有安全 Enclave 的 Always Encrypted

自 Microsoft.Data.SqlClient 版本 1.1.0 起，驱动程序支持[具有安全 Enclave 的 Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-enclaves.md)。

必须将应用程序配置为启用 enclave 计算和 enclave 证明，才能启用在连接到 [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)] 或更高版本时使用 enclave。

有关 enclave 计算和 enclave 证明中的客户端驱动程序角色的一般信息，请参阅[使用具有安全 Enclave 的 Always Encrypted 开发应用程序](../../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md)。

要配置应用程序：

1. 请确保已为 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 实例配置 enclave 类型（请参阅[为 Always Encrypted 服务器配置选项配置 enclave 类型](../../../database-engine/configure-windows/configure-column-encryption-enclave-type.md)）。 [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)] 支持 VBS enclave 类型和用于证明的[主机保护者服务](/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs)。
2. 通过将连接字符串中的 `Enclave Attestation URL` 关键字设置为证明终结点，为从应用程序到数据库的连接启用 enclave 计算。 关键字的值应设置为，在环境中配置的 HGS 服务器的证明终结点。
3. 通过在连接字符串中设置 `Attestation Protocol` 关键字，提供要使用的证明协议。 此关键字的值应设置为“HGS”。

有关分步教程，请参阅[教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](tutorial-always-encrypted-enclaves-develop-net-apps.md)。


## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>检索和修改加密列中的数据

为应用程序查询启用 Always Encrypted 后，就可以使用标准 SqlClient API（请参阅[在 ADO.NET 中检索和修改数据](/dotnet/framework/data/adonet/retrieving-and-modifying-data)）或 [Microsoft.Data.SqlClient Namespace](/dotnet/api/microsoft.data.sqlclient) 中定义的 [Microsoft .NET Data Provider for SQL Server  ](index.md) API，检索或修改加密数据库列中的数据。 假设应用程序拥有所需的数据库权限，并可以访问列主密钥，那么 Microsoft .NET Data Provider for SQL Server 会加密任何面向加密列的查询参数，并解密从返回 .NET 类型（对应于为数据库架构中的列设置的 SQL Server 数据类型）纯文本值的加密列中检索到的数据。
如果未启用 Always Encrypted，具有面向加密列的参数的查询将失败。 只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。 不过，Microsoft .NET Data Provider for SQL Server  不会尝试解密从加密列中检索到的任何值，并且应用程序会收到二进制加密数据（以字节数组形式）。

下表概述了查询的行为，具体取决于是否启用了 Always Encrypted：

|查询特征 | Always Encrypted 已启用，并且应用程序可以访问密钥和密钥元数据|Always Encrypted 已启用，但应用程序无法访问密钥或密钥元数据 | 禁用了始终加密|
|:---|:---|:---|:---|
| 具有面向加密列的参数的查询。 | 以透明方式加密参数值。 | 错误 | 错误 |
| 从加密列中检索数据且没有面向加密列的参数的查询。 | 以透明方式解密来自加密列的结果。 应用程序收到 .NET 数据类型（对应于为加密列配置的 SQL Server 类型）的纯文本值。 | 错误 | 不解密来自加密列的结果。 应用程序收到字节数组形式的加密值 (byte[])。 |

以下示例说明如何检索和修改加密列中的数据。 这些示例假定目标表具有以下架构。 `SSN` 和 `BirthDate` 列进行了加密。

```sql
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1),
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

### <a name="inserting-data-example"></a>插入数据示例

此示例向 Patients 表插入一行。 注意以下事项：

- 对于示例代码中的加密，没有什么特定的注意事项。 Microsoft .NET Data Provider for SQL Server  自动检测并加密定目标到加密列的 `paramSSN` 和 `paramBirthdate` 参数。 这使得加密操作对应用程序而言是透明的。
- 插入到数据库列（包括加密列）中的值将作为 [SqlParameter](/dotnet/api/microsoft.data.sqlclient.sqlparameter) 对象传递。 在将值发送到非加密列时，SqlParameter  是可选的（虽然强烈建议使用它，因为它有助于防止 SQL 注入），而在发送面向加密列的值时，它是必需的。 如果插入 `SSN` 或 `BirthDate` 列中的值作为查询语句中嵌入的文本传递，查询会失败，因为 Microsoft .NET Data Provider for SQL Server 无法确定目标加密列中的值，进而也就不会加密这些值。 因此，服务器会因为与加密列不兼容而拒绝它们。
- 面向 `SSN` 列的参数的数据类型将设置为映射到 char/varchar SQL Server 数据类型的 ANSI（非 Unicode）字符串。 如果该参数的类型设置为映射到 nchar/nvarchar 的 Unicode 字符串（字符串），查询将失败，因为 Always Encrypted 不支持从加密的 nchar/nvarchar 值转换为加密的 char/varchar 值。 有关数据类型映射的信息，请参阅 [SQL Server 数据类型映射](/dotnet/framework/data/adonet/sql-server-data-type-mappings) 。
- 插入 `BirthDate` 列中的参数的数据类型是使用 [SqlParameter.SqlDbType 属性](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqldbtype)显式设置为目标 SQL Server 数据类型，而不依赖使用 [SqlParameter.DbType 属性](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype)时应用的从 .NET 类型到 SQL Server 数据类型的隐式映射。 默认情况下，[DateTime 结构](/dotnet/api/system.datetime)映射到 datetime SQL Server 数据类型。 由于 `BirthDate` 列的数据类型是日期，而 Always Encrypted 不支持将加密的日期时间值转换为加密的日期值，因此，使用默认映射将导致错误。

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";

using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = "795-73-9838";
    paramSSN.Size = 11;
    cmd.Parameters.Add(paramSSN);

    SqlParameter paramFirstName = cmd.CreateParameter();
    paramFirstName.ParameterName = @"@FirstName";
    paramFirstName.DbType = DbType.String;
    paramFirstName.Direction = ParameterDirection.Input;
    paramFirstName.Value = "Catherine";
    paramFirstName.Size = 50;
    cmd.Parameters.Add(paramFirstName);

    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);

    SqlParameter paramBirthdate = cmd.CreateParameter();
    paramBirthdate.ParameterName = @"@BirthDate";
    paramBirthdate.SqlDbType = SqlDbType.Date;
    paramBirthdate.Direction = ParameterDirection.Input;
    paramBirthdate.Value = new DateTime(1996, 09, 10);
    cmd.Parameters.Add(paramBirthdate);

    cmd.ExecuteNonQuery();
}
```

### <a name="retrieving-plaintext-data-example"></a>检索纯文本数据示例

以下示例演示如何根据加密值筛选数据，以及从加密列中检索纯文本数据。 注意以下事项：

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN=@SSN";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = "795-73-9838";
    paramSSN.Size = 11;
    cmd.Parameters.Add(paramSSN);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
            }
        }
    }
}
```

> [!NOTE]
> - 必须使用 SqlParameter 传递 WHERE 子句中用于筛选 `SSN` 列的值，这样 Microsoft .NET Data Provider for SQL Server 就可以在将它发送到数据库之前以透明方式加密它。
>
> - 程序打印的所有值都是纯文本，因为 Microsoft .NET Data Provider for SQL Server 会以透明方式解密从 `SSN` 和 `BirthDate` 列中检索到的数据。
>
> - 查询可以在列上执行相等比较（如果使用确定性加密对它们进行加密。 有关详细信息，请参阅[选择确定性加密或随机加密](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)。

### <a name="retrieving-encrypted-data-example"></a>检索加密数据示例

如果未启用始终加密，只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。

以下示例演示如何从加密列中检索二进制加密数据。 

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";

using (SqlConnection connection = new SqlConnection(connectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName";

    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", BitConverter.ToString((byte[])reader[0]), reader[1], reader[2], BitConverter.ToString((byte[])reader[3]));
            }
        }
    }
}
```

> [!NOTE]
> - 由于未在连接字符串中启用 Always Encrypted，因此，查询将以字节数组的形式返回 `SSN` 和 `BirthDate` 的加密值（程序会将值转换为字符串）。
>
> - 如果禁用始终加密，从加密列中检索数据的查询可以有参数，但前提是所有参数均不面向加密列。 上述查询按未在数据库中加密的 LastName 进行筛选。 如果查询按 `SSN` 或 `BirthDate` 进行筛选，则将失败。

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>避免查询加密列时的常见问题

本节介绍从 .NET 应用程序查询加密列时的常见错误类别，以及有关如何避免这些错误的若干指导。

### <a name="unsupported-data-type-conversion-errors"></a>不支持的数据类型转换错误

始终加密支持对加密数据类型进行若干种转换。 有关受支持类型转换的详细列表，请参阅 [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)。 执行以下操作可避免数据类型转换错误：

- 设置定目标到加密列的参数的类型，以便参数的 SQL Server 数据类型与目标列的类型完全相同，或者支持将参数的 SQL Server 数据类型转换为列的目标类型。 可以使用 SqlParameter.SqlDbType 属性，强行根据需要将 .NET 数据类型映射到特定的 SQL Server 数据类型。
- 验证对于面向列的 decimal 和 numeric SQL Server 数据类型的参数，其精度和小数位数是否与为目标列配置的精度和小数位数相同。  
- 验证面向列的 datetime2、datetimeoffset 或 time SQL Server 数据类型的参数的精度是否不大于目标列的精度（在用于修改目标列中的值的查询中）。  

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>由于传递纯文本而非加密值而发生的错误

面向加密列的任何值都需要在应用程序内加密。 尝试插入/修改或者按纯文本值筛选加密列将导致如下错误：

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', column_encryption_key_database_name = 'Clinic') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

为防止发生此类错误，请确保：

- 为面向加密列的应用程序查询（为特定查询的连接字符串或在 [SqlCommand](/dotnet/api/microsoft.data.sqlclient.sqlcommand) 对象中）启用始终加密）。
- 使用 SqlParameter 发送面向加密列的数据。 下面的示例展示了一个查询，它按文本/常量对加密列 (SSN) 进行错误筛选，而不是传递 SqlParameter 对象内的文本。

```cs
using (SqlCommand cmd = connection.CreateCommand())
{
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN = '795-73-9838'";
    cmd.ExecuteNonQuery();
}
```

## <a name="working-with-column-master-key-stores"></a>使用列主密钥存储

Microsoft .NET Data Provider for SQL Server  必须获取为目标列配置的列加密密钥，才能加密参数值或解密查询结果中的数据。 列加密密钥以加密形式存储在数据库元数据中。 每个列加密密钥都有一个用于加密列加密密钥的相应列主密钥。 数据库元数据不会存储列主密钥 - 它只包含含有特定列主密钥的密钥存储的相关信息，以及该密钥在密钥存储中的位置。

为了获取列加密密钥的纯文本值，Microsoft .NET Data Provider for SQL Server 首先获取有关列加密密钥及其对应的列主密钥的元数据。 然后，它会使用元数据中的信息来访问包含列主密钥的密钥存储，并对加密的列加密密钥进行解密。 Microsoft .NET Data Provider for SQL Server  使用列主密钥存储库提供程序（派生自 [SqlColumnEncryptionKeyStoreProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider)的类实例）与密钥存储库通信。

用于获取列加密密钥的过程：

1. 如果查询已启用 Always Encrypted，Microsoft .NET Data Provider for SQL Server  会以透明方式调用 sys.sp_describe_parameter_encryption  ，以检索定目标到加密列的参数的加密元数据（如果查询有参数的话）。 对于查询结果中包含的加密数据，SQL Server 会自动附加加密元数据。 有关列主密钥的信息包括：
    - 密钥存储提供程序的名称，该提供程序可封装包含列主密钥的密钥存储。
    - 指定列主密钥在密钥存储中的位置的密钥路径。

    有关列加密密钥的信息包括：

    - 列加密密钥的加密值。
    - 用于加密列加密密钥的算法的名称。
2. Microsoft .NET Data Provider for SQL Server 使用列主密钥存储库提供程序的名称在内部数据结构中查找提供程序对象，它是派生自 SqlColumnEncryptionKeyStoreProvider 类的类实例。
3. 为了解密列加密密钥，Microsoft .NET Data Provider for SQL Server  调用 `SqlColumnEncryptionKeyStoreProvider.DecryptColumnEncryptionKey()` 方法，同时传递列主密钥路径、列加密密钥的加密值以及用于生成加密列加密密钥的加密算法的名称。

### <a name="using-built-in-column-master-key-store-providers"></a>使用内置列主密钥存储提供程序

Microsoft .NET Data Provider for SQL Server  附带以下内置列主密钥存储库提供程序，这些提供程序已使用特定的提供程序名称（用于查找提供程序）进行预注册。 这些内置密钥存储提供程序仅在 Windows 上受支持。

| 类 | 说明 | 提供程序（查找）名称 | 平台 |
|:---|:---|:---|:---|
|[SqlColumnEncryptionCertificateStoreProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider) | 用于 Windows 证书存储的提供程序。 | MSSQL_CERTIFICATE_STORE | Windows |
|[SqlColumnEncryptionCngProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncngprovider) | 用于支持 [Microsoft 加密 API：下一代 (CNG) API](/windows/win32/seccng/cng-portal) 的密钥存储的提供程序。 这类存储通常是硬件安全模块 — 一种用于保护和管理数字密钥并提供加密处理的物理设备。 | MSSQL_CNG_STORE | Windows |
| [SqlColumnEncryptionCspProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncspprovider) | 用于支持 [Microsoft 加密 API (CAPI)](/windows/win32/seccrypto/cryptographic-service-providers)的密钥存储的提供程序。 这类存储通常是硬件安全模块 — 一种用于保护和管理数字密钥并提供加密处理的物理设备。 | MSSQL_CSP_PROVIDER | Windows |

无需对应用程序代码进行任何更改，即可使用这些提供程序，但请注意以下几点：

- 你（或你的 DBA）需要确保列主密钥元数据中配置的提供程序名称正确，并且列主密钥路径符合对于给定提供程序有效的密钥路径格式。 建议你使用诸如 SQL Server Management Studio 之类的工具来配置密钥，这类工具在发出 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) 语句时会自动生成有效的提供程序名称和密钥路径。 有关详细信息，请参阅 [使用 SQL Server Management Studio 配置始终加密](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md) 和 [使用 PowerShell 配置始终加密](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)。
- 确保应用程序可以访问密钥存储中的密钥。 这可能涉及向应用程序授予对密钥和/或密钥存储的访问权限（具体取决于密钥存储），或执行其他特定于密钥存储的配置步骤。 例如，若要访问实现 CNG 或 CAPI 的密钥存储（例如硬件安全模块），需确保在应用程序计算机上安装用于为存储实现 CNG 或 CAPI 的库。 有关详细信息，请参阅[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

### <a name="using-the-azure-key-vault-provider"></a>使用 Azure Key Vault 提供程序

Azure 密钥保管库便于存储和管理用于始终加密的列主密钥（尤其是当应用程序在 Azure 中托管时）。 Microsoft .NET Data Provider for SQL Server 不包含用于 Azure Key Vault 的内置列主密钥存储库提供程序，而是以 NuGet 程序包 ([Microsoft.Data.SqLClient.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider)) 的形式提供它，以便你能够将它与应用程序轻松集成。 有关详细信息，请参阅 [始终加密 — 使用数据加密保护 SQL 数据库中的敏感数据并将加密密钥存储在 Azure 密钥保管库中](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure)。

| 类 | 说明 | 提供程序（查找）名称 | 平台 |
|:---|:---|:---|:---|
|[SqlColumnEncryptionAzureKeyVaultProvider Class](/dotnet/api/microsoft.data.sqlclient.alwaysencrypted.azurekeyvaultprovider.sqlcolumnencryptionazurekeyvaultprovider) | Azure Key Vault 的提供程序。 | AZURE_KEY_VAULT | Windows、Linux、macOS |

有关展示如何使用 Azure Key Vault 执行加密/解密的示例，请参阅[使用 Always Encrypted 的 Azure Key Vault](azure-key-vault-example.md) 和[使用具有安全 Enclave 的 Always Encrypted 的 Azure Key Vault](azure-key-vault-enclave-example.md)。

### <a name="implementing-a-custom-column-master-key-store-provider"></a>实现自定义列主密钥存储提供程序

若要将列主密钥存储在现有提供程序不支持的密钥存储库中，可以扩展 [SqlColumnEncryptionKeyStoreProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider)，并使用 [SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/microsoft.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders) 方法进行注册，从而实现自定义提供程序。

```cs
public class MyCustomKeyStoreProvider : SqlColumnEncryptionKeyStoreProvider
{
    public const string ProviderName = "MY_CUSTOM_STORE";

    public override byte[] EncryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] columnEncryptionKey)
    {
        // Logic for encrypting a column encrypted key.
    }
    public override byte[] DecryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] EncryptedColumnEncryptionKey)
    {
        // Logic for decrypting a column encrypted key.
    }
}  
class Program
{
    static void Main(string[] args)
    {
        Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
            new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();
        providers.Add(MyCustomKeyStoreProvider.ProviderName, new MyCustomKeyStoreProvider());
        SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        // ...
    }
}
```

### <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>使用列主密钥存储提供程序进行编程密钥预配

访问加密列时，Microsoft .NET Data Provider for SQL Server  以透明方式查找并调用正确的列主密钥存储库提供程序，以解密列加密密钥。 通常情况下，普通的应用程序代码不会直接调用列主密钥存储提供程序。 不过，你可以显式实例化并调用一个提供程序，以编程方式预配和管理始终加密密钥：以便生成加密列加密密钥以及对列加密密钥解密（例如，在列主密钥轮替过程中）。 有关详细信息，请参阅 [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)。
仅当使用自定义密钥存储提供程序时，才有可能需要实现你自己的密钥管理工具。 使用存储于密钥存储（存在内置提供程序）或 Azure 密钥保管库中的密钥时，可以使用现有工具（如 SQL Server Management Studio 或 PowerShell）来管理和预配密钥。
下面的示例展示了如何生成列加密密钥，并使用 [SqlColumnEncryptionCertificateStoreProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider)通过证书来加密密钥。

```cs
using System.Security.Cryptography;
static void Main(string[] args)
{
    byte[] EncryptedColumnEncryptionKey = GetEncryptedColumnEncryptonKey();
    Console.WriteLine("0x" + BitConverter.ToString(EncryptedColumnEncryptionKey).Replace("-", ""));
    Console.ReadKey();
}

static byte[]  GetEncryptedColumnEncryptonKey()
{
    int cekLength = 32;
    String certificateStoreLocation = "CurrentUser";
    String certificateThumbprint = "698C7F8E21B2158E9AED4978ADB147CF66574180";
    // Generate the plaintext column encryption key.
    byte[] columnEncryptionKey = new byte[cekLength];
    RNGCryptoServiceProvider rngCsp = new RNGCryptoServiceProvider();
    rngCsp.GetBytes(columnEncryptionKey);

    // Encrypt the column encryption key with a certificate.
    string keyPath = String.Format(@"{0}/My/{1}", certificateStoreLocation, certificateThumbprint);
    SqlColumnEncryptionCertificateStoreProvider provider = new SqlColumnEncryptionCertificateStoreProvider();
    return provider.EncryptColumnEncryptionKey(keyPath, @"RSA_OAEP", columnEncryptionKey);
}
```

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>控制 Always Encrypted 对性能的影响

由于 Always Encrypted 是客户端加密技术，因此大部分性能开销发生在客户端，而不是数据库中。 除加密和解密操作的成本之外，客户端上的其他性能开销来源包括：

- 额外往返数据库以检索查询参数的元数据。
- 调用列主密钥存储以访问列主密钥。

本部分介绍了 Microsoft .NET Data Provider for SQL Server  中的内置性能优化，以及如何控制上述两个因素对性能的影响。

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>控制为了检索查询参数的元数据而往返的次数

如果连接已启用 Always Encrypted，Microsoft .NET Data Provider for SQL Server  默认会为每个参数化查询调用 [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)，同时将查询语句（不带任何参数值）传递到 SQL Server。 sys.sp_describe_parameter_encryption  分析查询语句，以确定是否有任何需要加密的参数；如果有，则会针对每个需要加密的参数返回加密相关信息，以便 Microsoft .NET Data Provider for SQL Server  能够加密参数值。 以上行为可确保实现针对客户端应用程序的高级别透明性。 应用程序（和应用程序开发人员）不需要知道哪些查询访问加密列，只需在 SqlParameter 对象中将定目标到加密列的值传递到 Microsoft .NET Data Provider for SQL Server  即可。

### <a name="query-metadata-caching"></a>查询元数据缓存

Microsoft .NET Data Provider for SQL Server  为每个查询语句缓存 sys.sp_describe_parameter_encryption  的结果。 因此，如果多次执行相同查询语句，则驱动程序仅调用 **sys.sp_describe_parameter_encryption** 一次。 查询语句的加密元数据缓存大大减少了从数据库提取元数据的性能开销。 缓存默认为启用状态。 可以通过将 [SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled 属性](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled)设置为 false 来禁用参数元数据缓存，但不建议这样做，除非是在下面所述的极少数情况下：

请考虑一个具有两个不同架构的数据库：`s1` 和 `s2`。 每个架构都包含一个具有相同名称的表：`t`。 除与加密相关的属性外，`s1.t` 和 `s2.t` 表的定义是相同的：名为 `c` 的列，在 `s1.t` 中未加密，在 `s2.t` 中已加密。 该数据库具有两个用户：`u1` 和 `u2`。 `u1` 用户的默认架构是 `s1`。 `u2` 的默认架构是 `s2`。 一个 .NET 应用程序打开与该数据库之间的两个连接，在一个连接上模拟 `u1` 用户，在另一个连接上模拟 `u2` 用户。 应用程序在用于用户 `u1` 的连接上发送具有面向 `c` 列的参数的查询（查询未指定架构，因此采用默认用户架构）。 接下来，该应用程序在用于 `u2` 用户的连接上发送相同查询。 如果查询元数据缓存已启用，则在第一个查询之后，缓存会使用指明 `c` 列（查询参数目标）未加密的元数据进行填充。 因为第二个查询具有相同的查询语句，所以会使用存储在缓存中的信息。 因此，驱动程序会在未加密参数的情况下发送查询（这是不正确的，因为目标列 `s2.t.c` 进行了加密），从而向服务器泄露参数的纯文本值。 服务器会检测该不兼容行，会强制驱动程序刷新缓存，因此该应用程序会以透明方式重新发送具有正确加密的参数值的查询。 在这种情况下，应禁用缓存以防止向服务器泄露敏感值。

### <a name="setting-always-encrypted-at-the-query-level"></a>在查询级别设置始终加密

若要控制检索参数化查询的加密元数据对性能的影响，可以为各个查询启用 Always Encrypted，而不是为连接设置 Always Encrypted。 这样一来，就可以确保仅针对你知道具有面向加密列的参数的查询调用 **sys.sp_describe_parameter_encryption** 。 不过，请注意，这样做会降低加密的透明度：如果更改数据库列的加密属性，可能需要更改应用程序代码，使它与架构更改保持一致。

> [!NOTE]
> 在查询一级设置 Always Encrypted 会限制参数加密元数据缓存的性能优势。

若要控制单个查询的始终加密行为，需使用 [SqlCommand](/dotnet/api/microsoft.data.sqlclient.sqlcommand) 的此构造函数和 [SqlCommandColumnEncryptionSetting](/dotnet/api/microsoft.data.sqlclient.sqlcommandcolumnencryptionsetting)。 下面是一些有用的指导原则：

- 如果客户端应用程序通过数据库连接发送的大多数查询访问的是加密列，则可执行以下操作：
  - 将“列加密设置”  连接字符串关键字设置为“已启用”  。
  - 对于不访问任何加密列的单个查询，则设置 **SqlCommandColumnEncryptionSetting.Disabled** 。 这将禁止调用 sys.sp_describe_parameter_encryption，同时禁止尝试对结果集中的任何值解密。
  - 对于其参数不需要加密但会从加密列检索数据的单个查询，则设置 SqlCommandColumnEncryptionSetting.ResultSetOnly。 这将禁止调用 sys.sp_describe_parameter_encryption 和参数加密。 查询将能够解密来自加密列的结果。
- 如果客户端应用程序通过数据库连接发送的大多数查询不访问加密列，则可执行以下操作：
  - 将“列加密设置”  连接字符串关键字设置为“已禁用”  。
  - 对于有参数需要加密的单个查询，则设置 **SqlCommandColumnEncryptionSetting.Enabled** 。 这将允许调用 sys.sp_describe_parameter_encryption，同时允许对从加密列中检索到的任何查询结果解密。
  - 对于其参数不需要加密但会从加密列检索数据的查询，则设置 SqlCommandColumnEncryptionSetting.ResultSetOnly。 这将禁止调用 sys.sp_describe_parameter_encryption 和参数加密。 查询将能够解密来自加密列的结果。

在以下示例中，将对数据库连接禁用始终加密。 应用程序发出的查询有一个面向未加密的 LastName 列的参数。 该查询从已加密的 `SSN` 和 `BirthDate` 列中检索数据。 在这种情况下，不需要调用 sys.sp_describe_parameter_encryption 来检索加密元数据。 但是，需要启用查询结果解密，以便应用程序从两个加密列接收纯文本值。 使用 SqlCommandColumnEncryptionSetting.ResultSetOnly 设置可以确保这一点。

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
using (SqlConnection connection = new SqlConnection(connectionString))
using (SqlCommand cmd = new SqlCommand(@"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName",
connection, null, SqlCommandColumnEncryptionSetting.ResultSetOnly))
{
    connection.Open();
    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
            }
        }
    }
}
```

### <a name="column-encryption-key-caching"></a>列加密密钥缓存

为了减少对列加密密钥解密时调用列主密钥存储库的次数，Microsoft .NET Data Provider for SQL Server  将纯文本列加密密钥缓存在内存中。 从数据库元数据收到加密列的加密密钥值之后，驱动程序首先会尝试查找与加密密钥值对应的纯文本列加密密钥。 仅当在缓存中找不到加密列加密密钥值时，驱动程序才会调用包含列主密钥的密钥存储库。

出于安全原因，会在可配置的生存时间间隔之后中逐出缓存项。 默认生存时间值是 2 小时。 如果你对应用程序中以纯文本形式缓存列加密密钥的时长有更严格的安全要求，可以使用 [SqlConnection.ColumnEncryptionKeyCacheTtl 属性](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionkeycachettl)更改它。 

## <a name="enabling-additional-protection-for-a-compromised-sql-server"></a>为遭到入侵的 SQL Server 启用附加保护

默认情况下，Microsoft .NET Data Provider for SQL Server 依赖数据库系统（SQL Server 或 Azure SQL 数据库）来提供有关加密数据库中哪些列以及如何加密的元数据。 借助加密元数据，Microsoft .NET Data Provider for SQL Server  可以加密查询参数，并解密查询结果，而无需从应用程序进行任何输入，从而大大减少了应用程序中所需的更改量。 不过，如果 SQL Server 进程遭到入侵，并且攻击者篡改 SQL Server 发送给 Microsoft .NET Data Provider for SQL Server  的元数据，攻击者可能会窃取敏感信息。 本部分介绍可帮助针对这种类型的攻击提供附加保护级别（以降低透明度为代价）的 API。

### <a name="forcing-parameter-encryption"></a>强制实施参数加密

在 Microsoft .NET Data Provider for SQL Server  将参数化查询发送到 SQL Server 之前，它会要求 SQL Server（通过调用 [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)）分析查询语句，并提供有关应加密查询中哪些参数的信息。 遭到入侵的 SQL Server 实例可能会发送指明参数不定目标到加密列的元数据来误导 Microsoft .NET Data Provider for SQL Server  ，尽管列实际上是在数据库中加密。 因此，Microsoft .NET Data Provider for SQL Server  不会加密参数值，而是以纯文本形式将它发送到遭到入侵的 SQL Server 实例。

若要阻止这类攻击，应用程序可以将参数的 [SqlParameter.ForceColumnEncryption 属性](/dotnet/api/microsoft.data.sqlclient.sqlparameter.forcecolumnencryption) 设置为 true。 这会导致 Microsoft .NET Data Provider for SQL Server  抛出异常（如果它从服务器收到的元数据指明参数不需要加密的话）。

虽然使用 SqlParameter.ForceColumnEncryption 属性  有助于提高安全性，但它同样降低了客户端应用程序的加密透明度。 如果更新数据库架构以更改加密列集，则可能还需要进行应用程序更改。

下面的代码示例展示了如何使用 SqlParameter.ForceColumnEncryption 属性  防止以纯文本形式将身份证号发送到数据库。

```cs
using (SqlCommand cmd = _sqlconn.CreateCommand())
{
    // Use parameterized queries to access Always Encrypted data.

    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = @SSN;";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = ssn;
    paramSSN.Size = 11;
    paramSSN.ForceColumnEncryption = true;
    cmd.Parameters.Add(paramSSN);

    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        // Do something.
    }
}
```

### <a name="configuring-trusted-column-master-key-paths"></a>配置可信列主密钥路径

SQL Server 对定目标到加密列的查询参数和从加密列检索到的结果返回的加密元数据包含，用于标识密钥存储库以及密钥在密钥存储库中的位置的列主密钥密钥路径。 如果 SQL Server 实例遭到入侵，它可能会发送将 Microsoft .NET Data Provider for SQL Server  定向到攻击者控制的位置的密钥路径。 对于需要应用程序进行身份验证的密钥存储库，这可能会导致泄漏密钥存储库凭据。

为了阻止此类攻击，应用程序可以使用 [SqlConnection.ColumnEncryptionTrustedMasterKeyPaths 属性](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths)指定给定服务器的可信密钥路径列表。 如果 Microsoft .NET Data Provider for SQL Server  收到可信密钥路径列表不包含的密钥路径，它会抛出异常。

虽然设置可信密钥路径会提高应用程序的安全性，不过每当你轮播列主密钥时（每当列主密钥路径更改时），都需要更改应用程序的代码或/和配置。

下面的示例演示如何配置可信列主密钥路径：

```cs
// Configure trusted key paths to protect against fake key paths sent by a compromised SQL Server instance
// First, create a list of trusted key paths for your server
List<string> trustedKeyPathList = new List<string>();
trustedKeyPathList.Add("CurrentUser/my/425CFBB9DDDD081BB0061534CE6AB06CB5283F5Ea");

// Register the trusted key path list for your server
SqlConnection.ColumnEncryptionTrustedMasterKeyPaths.Add(serverName, trustedKeyPathList);

```

## <a name="copying-encrypted-data-using-sqlbulkcopy"></a>使用 SqlBulkCopy 复制加密数据

使用 SqlBulkCopy，可以将已加密并且存储在某个表中的数据复制到另一个表，而无需对数据解密。 若要执行该操作：

- 请确保目标表的加密配置与源表的配置完全相同。 特别是，两个表必须对相同的列加密，并且必须使用相同的加密类型和相同的加密密钥对列加密。 如果任何目标列的加密方式与其相应的源列不同，你都不能在复制操作完成后对目标表中的数据进行解密。 数据将损坏。
- 配置数据库到源表和目标表的连接，而不启用 Always Encrypted。
- 设置 `AllowEncryptedValueModifications` 选项（请参阅 [SqlBulkCopyOptions](/dotnet/api/microsoft.data.sqlclient.sqlbulkcopyoptions)）。

> [!NOTE]
> 请谨慎指定 `AllowEncryptedValueModifications`，因为这可能会导致损坏数据库，因为 Microsoft .NET Data Provider for SQL Server 不会检查数据是否确实已加密，也不会检查是否使用与目标列相同的加密类型、算法和密钥对数据进行了正确加密。

下面是将数据从一个表复制到另一个表的示例。 其中假定 `SSN` 和 `BirthDate` 列已加密。

```cs
static public void CopyTablesUsingBulk(string sourceTable, string targetTable)
{
    string sourceConnectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
    string targetConnectionString = "Data Source=server64; Initial Catalog=Clinic; Integrated Security=true";
    using (SqlConnection connSource = new SqlConnection(sourceConnectionString))
    {
        connSource.Open();
        using (SqlCommand cmd = new SqlCommand(string.Format("SELECT [PatientID], [SSN], [FirstName], [LastName], [BirthDate] FROM {0}", sourceTable), connSource))
        {
            using (SqlDataReader reader = cmd.ExecuteReader())
            using (SqlBulkCopy copy = new SqlBulkCopy(targetConnectionString, SqlBulkCopyOptions.KeepIdentity | SqlBulkCopyOptions.AllowEncryptedValueModifications))
            {
                copy.EnableStreaming = true;
                copy.DestinationTableName = targetTable;
                copy.WriteToServer(reader);
            }
        }
    }
}
```

## <a name="always-encrypted-api-reference"></a>始终加密 API 参考

**命名空间:** [Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.sqlclient)

**程序集：** Microsoft.Data.SqlClient.dll

|名称|说明|
|:---|:---|
|[SqlColumnEncryptionCertificateStoreProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider)|用于 Windows 证书存储的密钥存储库提供程序。|
|[SqlColumnEncryptionCngProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncngprovider)|用于 Microsoft 加密 API 的密钥存储库提供程序：存储提供程序。|
|[SqlColumnEncryptionCspProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncspprovider)|用于 Microsoft 基于 CAPI 的加密服务提供程序 (CSP) 的密钥存储库提供程序。|
|[SqlColumnEncryptionKeyStoreProvider 类](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider)|密钥存储提供程序的基类。|
|[SqlCommandColumnEncryptionSetting 枚举](/dotnet/api/microsoft.data.sqlclient.sqlcommandcolumnencryptionsetting)|控制各个查询的始终加密行为的设置。|
|[SqlConnectionAttestationProtocol 枚举](/dotnet/api/microsoft.data.sqlclient.sqlconnectionattestationprotocol)|使用具有安全 Enclave 的 Always Encrypted 时指定证明协议的值|
|[SqlConnectionColumnEncryptionSetting 枚举](/dotnet/api/microsoft.data.sqlclient.sqlconnectioncolumnencryptionsetting)|为数据库连接启用加密和解密的设置。|
|[SqlConnectionStringBuilder.ColumnEncryptionSetting 属性](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting)|获取并设置连接字符串中的始终加密。|
|[SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled 属性](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled)|启用和禁用加密查询元数据缓存。|
|[SqlConnection.ColumnEncryptionKeyCacheTtl 属性](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionkeycachettl)|获取和设置列加密密钥缓存中的项的生存时间。|
|[SqlConnection.ColumnEncryptionTrustedMasterKeyPaths 属性](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths)|允许你为数据库服务器设置受信任的密钥路径的列表。 如果在处理应用程序查询时驱动程序接收到不位于列表中的密钥路径，则查询会失败。 此属性提供针对安全攻击的额外保护，这些攻击涉及到提供伪造密钥路径的受损 SQL Server，这可能导致泄露密钥存储凭据。|
|[SqlConnection.RegisterColumnEncryptionKeyStoreProviders 方法](/dotnet/api/microsoft.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders)|允许你注册自定义密钥存储提供程序。 它是一个将密钥存储提供程序名称映射到密钥存储提供程序实现的字典。|
|[SqlCommand 构造函数 (String, SqlConnection, SqlTransaction, SqlCommandColumnEncryptionSetting)](/dotnet/api/microsoft.data.sqlclient.sqlcommand.-ctor?view=sqlclient-dotnet-core-1.0&preserve-view=true#Microsoft_Data_SqlClient_SqlCommand__ctor_System_String_Microsoft_Data_SqlClient_SqlConnection_Microsoft_Data_SqlClient_SqlTransaction_Microsoft_Data_SqlClient_SqlCommandColumnEncryptionSetting_)|允许你控制各个查询的始终加密行为。|
|[SqlParameter.ForceColumnEncryption 属性](/dotnet/api/microsoft.data.sqlclient.sqlparameter.forcecolumnencryption)|强制实施参数加密。 如果 SQL Server 告知驱动程序参数不需加密，则使用该参数的查询会失败。 此属性提供针对安全攻击的额外保护，这些攻击涉及受损 SQL Server 向客户端提供不正确的加密元数据，这可能会导致数据泄漏。|
|[连接字符串](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring)关键字：`Column Encryption Setting=enabled`|启用或禁用针对连接的始终加密功能。|

## <a name="see-also"></a>另请参阅

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [具有安全 Enclave 的 Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-enclaves.md)
- [SQL 数据库教程：使用 Always Encrypted 保护敏感数据](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
- [教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [示例：使用 Always Encrypted 的 Azure Key Vault](azure-key-vault-example.md)
- [示例：使用具有安全 Enclave 的 Always Encrypted 的 Azure Key Vault](azure-key-vault-enclave-example.md)。
