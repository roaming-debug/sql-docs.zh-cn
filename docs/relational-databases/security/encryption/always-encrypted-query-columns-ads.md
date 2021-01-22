---
description: 通过 Azure Data Studio 查询使用 Always Encrypted 的列
title: 通过 Azure Data Studio 查询使用 Always Encrypted 的列 | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 93bb5c30eeb9cdc6f10c3b71d0e4f70f3f8c6477
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534416"
---
# <a name="query-columns-using-always-encrypted-with-azure-data-studio"></a>通过 Azure Data Studio 查询使用 Always Encrypted 的列
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

本文介绍如何使用 [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) 查询使用 [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 加密的列。 使用 Azure Data Studio，可以执行以下操作：
- 检索存储在加密列中的密码文本值。 
- 检索存储在加密列中的纯文本值。  
- 发送定目标到加密列的纯文本值（例如，在 `INSERT` 或 `UPDATE` 语句中，以及作为 `SELECT` 语句中 `WHERE` 子句的查找参数）。 

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>检索存储在加密列中的密码文本值    
本节介绍如何检索以已加密文本形式存储在加密列中的数据。

### <a name="steps"></a>步骤
1. 对于将在其中运行检索已加密文本值的 `SELECT` 查询的查询窗口，确保为其数据库连接禁用了 Always Encrypted。 请参阅下面的[为数据库连接启用和禁用 Always Encrypted](#enabling-and-disabling-always-encrypted-for-a-database-connection)。     
1. 运行 `SELECT` 查询。 从加密列中检索到的任何数据都将作为二进制（加密）值返回。   

### <a name="example"></a>示例
假定 `SSN` 是 `Patients` 表中的加密列，如果为数据库连接禁用了 Always Encrypted，则以下所示的查询将检索二进制密码文本值。   

![屏幕截图显示了 SELECT * FROM [dbo].[Patients] 查询以及作为二进制已加密文本值显示的查询结果。](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>检索存储在加密列中的纯文本值    
本节介绍如何检索以已加密文本形式存储在加密列中的数据。

### <a name="prerequisites"></a>先决条件
- Azure Data Studio 版本 17.1 或更高版本。
- 你需要有权访问列主密钥以及有关保护列（对其运行查询）的密钥的元数据。 有关详细信息，请参阅下面的[查询加密列的权限](#permissions-for-querying-encrypted-columns)。
- 列主密钥必须存储在 Azure Key Vault 或 Windows 证书存储中。 Azure Data Studio 不支持其他密钥存储。

### <a name="steps"></a>步骤
1.  对于将在其中运行检索并解密数据的 `SELECT` 查询的查询窗口，为其数据库连接启用 Always Encrypted。 这将指示 [SQL Server 的 Microsoft .NET 数据提供程序](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md)（由 Azure Data Studio 使用）对查询结果集中的加密列进行解密。 请参阅下面的[为数据库连接启用和禁用 Always Encrypted](#enabling-and-disabling-always-encrypted-for-a-database-connection)。
1.  运行 `SELECT` 查询。 从加密列中检索到的任何数据都将作为原始数据类型的纯文本值返回。
 
### <a name="example"></a>示例
假定 SSN 是 `Patients` 表中的加密列，如果为数据库连接启用了 Always Encrypted，并且你有权访问为 `SSN` 列配置的列主密钥，则以下所示的查询将返回纯文本值。   

![屏幕截图显示了 SELECT * FROM [dbo].[Patients] 查询以及作为纯文本值显示的查询结果。](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>发送针对加密列的纯文本值       
本节介绍如何运行发送针对加密列的值的查询。 例如，通过存储在加密列中的值来插入、更新或筛选的查询：

### <a name="prerequisites"></a>先决条件
- Azure Data Studio 版本 18.1 或更高版本。
- 你需要有权访问列主密钥以及有关保护列（对其运行查询）的密钥的元数据。 有关详细信息，请参阅下面的[查询加密列的权限](#permissions-for-querying-encrypted-columns)。
- 列主密钥必须存储在 Azure Key Vault 或 Windows 证书存储中。 Azure Data Studio 不支持其他密钥存储。

### <a name="steps"></a>步骤
1. 对于将在其中运行检索并解密数据的 `SELECT` 查询的查询窗口，为其数据库连接启用 Always Encrypted。 这将指示 [SQL Server 的 Microsoft .NET 数据提供程序](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md)（由 Azure Data Studio 使用）来加密针对加密列的查询参数，并解密从加密列检索的结果。 请参阅下面的[为数据库连接启用和禁用 Always Encrypted](#enabling-and-disabling-always-encrypted-for-a-database-connection)。 
1. 为查询窗口启用 Always Encrypted 参数化。 有关详细信息，请参阅下面的 [Always Encrypted 参数化](#parameterization-for-always-encrypted) 。
1. 声明 Transact-SQL 变量，并使用要发送（插入、更新或筛选）到数据库的值对它进行初始化。 
1. 运行查询，将 Transact-SQL 变量的值发送到数据库。 Azure Data Studio 会将变量转换为查询参数并对其值进行加密，然后再将加密值发送到数据库。   

### <a name="example"></a>示例
假设 `SSN` 是 `Patients` 表中加密的 `char(11)` 列，则以下脚本将尝试在 SSN 列中查找包含 `'795-73-9838'` 的行。 如果为数据库连接启用了 Always Encrypted，为查询窗口启用了 Always Encrypted 参数化，并且你有权访问为 `SSN` 列配置的列主密钥，则返回结果。   

![屏幕截图显示了 DECLARE @SSN char(11) = '795-73-9838' SELECT * FROM [dbo].[Patients] WHERE [SSN] = @SSN 查询以及查询结果。](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-parameters.png)

## <a name="permissions-for-querying-encrypted-columns"></a>查询加密列的权限

若要对加密列运行任何查询（包括在已加密文本中检索数据的查询），则需要具有数据库中的“VIEW ANY COLUMN MASTER KEY DEFINITION”和“VIEW ANY COLUMN ENCRYPTION KEY DEFINITION”权限 。

若要对任何查询结果进行解密或对任何查询参数（通过对 Transact-SQL 变量进行参数化而生成）进行加密，除了上述权限，还需具有访问保护目标列的列主密钥的权限：

- **证书存储 - 本地计算机：** 必须对用作列主密钥的证书具有“读取”访问权限，或者是计算机上的管理员。   
- **Azure Key Vault：** 需要包含列主密钥的密钥保管库上的 get、unwrapKey 和 verify 权限  。

有关详细信息，请参阅 [创建并存储列主密钥 (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a>为数据库连接启用和禁用 Always Encrypted   

在 Azure Data Studio 中连接到数据库时，可以为数据库连接启用或禁用 Always Encrypted。 默认情况下，Always Encrypted 处于禁用状态。 

为数据库连接启用 Always Encrypted 将指示 [SQL Server 的 Microsoft .NET 数据提供程序](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md)（由 Azure Data Studio 使用）以透明方式尝试以下操作：   
-   对从加密列中检索到的并在查询结果中返回的任何值进行解密。   
-   对针对加密数据库的参数化 Transact-SQL 变量的值进行加密。   

如果没有为连接启用 Always Encrypted，则 SQL Server 的 Microsoft .NET 数据提供程序不会尝试加密查询参数或解密结果。

可以在连接到数据库时启用或禁用“Always Encrypted”。 有关如何连接到数据库的常规信息，请参阅：
- [快速入门：使用 Azure Data Studio 连接和查询 SQL Server](../../../azure-data-studio/quickstart-sql-server.md)
- [快速入门：使用 Azure DataStudio 连接和查询 Azure SQL 数据库](../../../azure-data-studio/quickstart-sql-database.md)

若要启用（禁用）Always Encrypted，请执行以下操作：
1. 在“连接”对话框中，单击“高级…” 。
2. 若要为连接启用 Always Encrypted，请将“Always Encrypted”字段设置为“启用” 。 若要禁用 Always Encrypted，请将“Always Encrypted”字段的值留空或将其设置为“禁用” 。
3. 单击“确定”以关闭“高级属性” 。

![短片介绍了为连接启用 Always Encrypted 的相关步骤。](../../../relational-databases/security/encryption/media/always-encrypted-ads-connect.gif)

若要在使用[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 时运行利用服务器端安全 enclave 的语句，除了启用连接的 Always Encrypted 外，还需要指定 enclave 证明协议和 enclave 证明 URL。 有关详细信息，请参阅[在 Azure Data Studio 中运行使用 enclave 的 T-SQL 语句的先决条件](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio)。

> [!TIP]
> 若要在现有查询窗口中启用和禁用 Always Encrypted，请先单击“断开连接”，再单击“连接”并完成以上步骤，以使用“Always Encrypted”字段的所需值重新连接到数据库  。 

> [!NOTE] 
> 查询窗口中的“更改连接”按钮当前不支持在启用和禁用“Always Encrypted”之间切换。

## <a name="parameterization-for-always-encrypted"></a>Always Encrypted 参数化

Always Encrypted 参数化是 Azure Data Studio 18.1 和更高版本中的一种功能，可自动将 Transact-SQL 变量转换为查询参数（[SqlParameter 类](/dotnet/api/microsoft.data.sqlclient.sqlparameter)的实例）。 这允许 [SQL Server 的基础 Microsoft .NET 数据提供程序](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md)针对加密列的数据进行检测，并在将数据发送到数据库之前对其进行加密。
  
如果没有进行参数化，SQL Server 的 Microsoft .NET 数据提供程序会传递你在查询窗口中编写的每个声明，作为非参数化查询。 如果查询包含定目标到加密列的文本或 Transact-SQL 变量，用于 SQL Server 的 .NET Framework 数据提供程序便无法在将查询发送到数据库之前检测和加密它们。 结果由于纯文本文字 Transact-SQL 变量和加密列之间的类型不匹配，查询将失败。 例如，假定 `SSN` 列已加密，如果没有参数化，以下查询将失败。   

```sql
DECLARE @SSN CHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>启用和禁用 Always Encrypted 参数化

默认情况下，将禁用 Always Encrypted。

若要启用/禁用 Always Encrypted 参数化，请执行以下操作：

1. 选择“文件” > “首选项” > “设置”（Mac 上的“代码” > “首选项” > “设置”     ）。
2. 导航到“数据” > “Microsoft SQL Server” 。
3. 选择或取消选择“启用 Always Encrypted 参数化” 。
4. 关闭“设置”窗口。

![短片介绍了如何启用/禁用 Always Encrypted 参数化。](../../../relational-databases/security/encryption/media/always-encrypted-ads-parameterization.gif)

> [!NOTE]
> Always Encrypted 参数化仅在使用启用了 Always Encrypted 的数据库连接的查询中有效（请参阅[为数据库连接启用和禁用 Always Encrypted](#enabling-and-disabling-always-encrypted-for-a-database-connection)）。 如果查询窗口使用未启用 Always Encrypted 的数据库连接，则不会对 Transact-SQL 变量进行参数化。

### <a name="how-parameterization-for-always-encrypted-works"></a>Always Encrypted 参数化的工作方式

如果同时为查询窗口启用了 Always Encrypted 参数化和 Always Encrypted，Azure Data Studio 将尝试对符合以下先决条件的 Transact-SQL 变量进行参数化：

- 在同一语句中进行声明和初始化（内联初始化）。 使用 `SET` 语句单独声明的变量不会进行参数化。
- 使用单个文字进行初始化。 使用表达式（包括任何运算符或函数）进行初始化的变量不会进行参数化。

下面是 Azure Data Studio 将对其进行参数化的变量的示例。

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Azure Data Studio 不会尝试对下面的几个变量示例进行参数化：

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
为了成功完成尝试的参数化操作：   
- 用于要参数化的变量初始化的文字类型必须与变量声明中的类型匹配。   
- 如果变量的声明类型是日期类型或时间类型，变量必须通过使用以下 ISO 8601 标准格式之一的字符串进行初始化。   

下面是会导致参数化错误的 Transact-SQL 变量声明的示例：

```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```

Azure Data Studio 使用 Intellisense 来通知你哪些参数可成功进行参数化而哪些参数化尝试会失败（及其原因）。   

可成功进行参数化的变量声明在查询窗口中使用信息消息下划线进行标记。 如果将鼠标悬停在带有信息消息下划线标记的声明语句之上，便会看到包含参数化过程结果的消息，其中包括生成的 [SqlParameter 类](/dotnet/api/microsoft.data.sqlclient.sqlparameter)对象的键属性值（该变量映射到：[SqlDbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype)、[Size](/dotnet/api/microsoft.data.sqlclient.sqlparameter.size)、[Precision](/dotnet/api/microsoft.data.sqlclient.sqlparameter.precision)、[Scale](/dotnet/api/microsoft.data.sqlclient.sqlparameter.scale)、[SqlValue](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqlvalue)）。 “问题”视图中还会显示已成功进行参数化的所有变量的完整列表。 若要打开“问题”视图，请选择“视图” > “问题”  。    



如果 Azure Data Studio 已尝试对变量进行参数化，但是参数化操作失败，那么变量的声明将以错误下划线进行标记。 如果将鼠标悬停在带有错误下划线标记的声明语句之上，便会看到错误的相关结果。 “问题”视图还显示所有变量的参数化错误的完整列表。

 
> [!NOTE]
> Always Encrypted 支持有限子网的类型转换，在许多情况下，Transact-SQL 变量的数据类型都要求与其针对的目标数据库列的类型相同。 例如，假定 `SSN` 表中的 `Patients` 列是 `char(11)`，以下查询将失败，因为 `@SSN` 变量的类型为 `nchar(11)`，与列的类型不匹配。   

```sql
DECLARE @SSN nchar(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN;
```

```output
Msg 402, Level 16, State 2, Line 5   
The data types char(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') collation_name = 'Latin1_General_BIN2' 
and nchar(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') are incompatible in the equal to operator.
```

> [!NOTE]
> 如果没有进行参数化，整个查询（包括类型转换）将在 SQL Server/Azure SQL 数据库内进行处理。 启用参数化后，某些类型转换由 Azure Data Studio 中的 SQL Server 的 Microsoft .NET 数据提供程序执行。 由于 Microsoft .NET 类型系统和 SQL Server 类型系统之间存在差异（例如某些类型的精度不同，如 float），启用了参数化来执行的查询可产生不同于未启用参数化来执行的查询的结果。 

## <a name="next-steps"></a>后续步骤
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)


## <a name="see-also"></a>另请参阅
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)