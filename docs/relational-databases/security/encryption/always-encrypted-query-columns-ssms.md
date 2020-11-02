---
title: 通过 SQL Server Management Studio 查询使用 Always Encrypted 的列 | Microsoft Docs
description: 了解如何通过 SQL Server Management Studio 查询使用 Always Encrypted 的列。 检索存储在加密列中的密码文本值或文本值。
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 37ac7271be5090f17db16f67968df6eca138856d
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92679034"
---
# <a name="query-columns-using-always-encrypted-with-sql-server-management-studio"></a>通过 SQL Server Management Studio 查询使用 Always Encrypted 的列
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

本文介绍如何使用 [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md) 查询使用 [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 加密的列。 通过 SSMS，可以：
- 检索存储在加密列中的密码文本值。 
- 检索存储在加密列中的纯文本值。   
- 发送定目标到加密列的纯文本值（例如，在 `INSERT` 或 `UPDATE` 语句中，以及作为 `SELECT` 语句中 `WHERE` 子句的查找参数）。

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>检索存储在加密列中的密码文本值    
如果运行的 SELECT 查询检索存储在加密列中数据的已加密文本（不解密数据），则无需可以访问保护数据的列主密钥。 若要在 SSMS 中以已加密文本形式从加密列中检索值，请执行以下操作：

1. 确保可以访问有关保护列（对其运行查询）的密钥的元数据。 虽然无需能够访问实际列主密钥，但是确实需要数据库级别权限才能查看数据库中的列主密钥和列加密密钥元数据对象。 有关详细信息，请参阅下面的[查询加密列的权限](#permissions-for-querying-encrypted-columns)。
1. 对于将在其中运行检索已加密文本值的 `SELECT` 查询的“查询编辑器”窗口，确保为其数据库连接禁用了 Always Encrypted。 请参阅下面的[为数据库连接启用和禁用 Always Encrypted](#en-dis)。     
1. 运行 `SELECT` 查询。 从加密列中检索到的任何数据都将作为二进制（加密）值返回。   

### <a name="example"></a>示例
假定 `SSN` 是 `Patients` 表中的加密列，如果为数据库连接禁用了 Always Encrypted，则以下所示的查询将检索二进制密码文本值。   

![屏幕截图显示了 SELECT [SSN] FROM [dbo].[Patients] 查询以及作为二进制已加密文本值显示的查询结果。](../../../relational-databases/security/encryption/media/always-encrypted-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>检索存储在加密列中的纯文本值    
从加密列中检索值作为纯文本（用于解密值）：   
1. 确保可以访问列主密钥以及有关保护列（对其运行查询）的密钥的元数据。 有关详细信息，请参阅下面的[查询加密列的权限](#permissions-for-querying-encrypted-columns)。
1.  对于将在其中运行检索并解密数据的 `SELECT` 查询的“查询编辑器”窗口，确保为其数据库连接启用了 Always Encrypted。 这将指示用于 SQL Server 的 .NET Framework 数据提供程序（由 SSMS 使用）对查询结果集中的加密列进行解密。 请参阅下面的[为数据库连接启用和禁用 Always Encrypted](#en-dis)。
1.  运行 `SELECT` 查询。 从加密列中检索到的任何数据都将作为原始数据类型的纯文本值返回。
 
### <a name="example"></a>示例
假定 SSN 是 `char(11)` 表中的加密 `Patients` 列，如果为数据库连接启用了 Always Encrypted，并且你有权访问为 `SSN` 列配置的列主密钥，则以下所示的查询将返回纯文本值。   

![屏幕截图显示了 SELECT [SSN] FROM [Clinic].[dbo].[Patients] 查询以及作为纯文本值显示的查询结果。](../../../relational-databases/security/encryption/media/always-encrypted-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>发送针对加密列的纯文本值       
执行发送针对加密列的值的查询，例如，通过存储在加密列中的值进行插入、更新或筛选的查询：

1. 确保可以访问列主密钥以及保护列（对其运行查询）的密钥的元数据。 有关详细信息，请参阅下面的[查询加密列的权限](#permissions-for-querying-encrypted-columns)。
1.  对于将在其中运行检索并解密数据的 `SELECT` 查询的“查询编辑器”窗口，确保为其数据库连接启用了 Always Encrypted。 这将指示用于 SQL Server 的 .NET Framework 数据提供程序（由 SSMS 使用）对查询结果集中的加密列进行解密。 请参阅下面的[为数据库连接启用和禁用 Always Encrypted](#en-dis)。
1. 请确保已为“查询编辑器”窗口启用 Always Encrypted 参数化。 （要求至少为 SSMS 版本 17.0。）声明 Transact-SQL 变量，并使用要发送（插入、更新或筛选）到数据库的值对它进行初始化。 有关详细信息，请参阅下面的 [Always Encrypted 参数化](#param) 。

1. 运行查询，将 Transact-SQL 变量的值发送到数据库。 SSMS 会将变量转换为查询参数并对其值进行加密，然后再将加密值发送到数据库。   

### <a name="example"></a>示例
假定 `SSN` 是 `char(11)` 表中的加密 `Patients` 列，如果已为数据库连接启用 Always Encrypted，为“查询编辑器”窗口启用了 Always Encrypted 参数化，并且你有权访问为 `'795-73-9838'` 列配置的列主密钥，则下面的脚本将尝试在 SSN 列中查找包含 `LastName` 的行并返回 `SSN` 列的值。   

![屏幕截图显示了 DECLARE @SSN CHAR(11) = '795-73-9838' SELECT [LastName] FROM [dbo].[Patients] WHERE [SSN] = @SSN 查询以及查询结果。](../../../relational-databases/security/encryption/media/always-encrypted-patients.png)

## <a name="permissions-for-querying-encrypted-columns"></a>查询加密列的权限

若要对加密列运行任何查询（包括在加密文本中检索数据的查询），则需要具有数据库中的 `VIEW ANY COLUMN MASTER KEY DEFINITION` 和 `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` 权限。

若要对任何查询结果进行解密或对任何查询参数（通过对 Transact-SQL 变量进行参数化而生成）进行加密，除了上述权限，还需具有访问保护目标列的列主密钥的权限：

- **证书存储 - 本地计算机** 必须对用作列主密钥的证书具有 `Read` 访问权限，或必须是计算机上的管理员。   
- Azure Key Vault：需要包含列主密钥的保管库上的 `get`、`unwrapKey` 和 `verify` 权限。
- **密钥存储提供程序(KSP)** ：在你使用密钥存储或密钥时，可能会提示你提供的必需权限和凭据，具体视存储和 KSP 配置而定。   
- **加密服务提供程序(CSP)** ：在你使用密钥存储或密钥时，可能会提示你提供的必需权限和凭据，具体视存储和 CSP 配置而定。

有关详细信息，请参阅 [创建并存储列主密钥 (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a><a name="en-dis"></a> 为数据库连接启用和禁用 Always Encrypted   
在 SSMS 中连接到数据库时，可以为数据库连接启用或禁用 Always Encrypted。 默认情况下，Always Encrypted 处于禁用状态。 

为数据库连接启用 Always Encrypted 将指示用于 SQL Server 的 .NET Framework 数据提供程序（用于 SQL Server Management Studio）以透明方式尝试以下操作：   
-   对从加密列中检索到的并在查询结果中返回的任何值进行解密。   
-   对针对加密数据库的参数化 Transact-SQL 变量的值进行加密。   

如果没有为连接启用 Always Encrypted，则用于 SQL Server 的 .NET Framework 数据提供程序（由 SSMS 使用）不会尝试加密查询参数或解密结果。

可以在使用“连接到服务器”对话框创建新连接或更改现有连接时启用或禁用 Always Encrypted。 

若要启用（禁用）Always Encrypted，请执行以下操作：
1. 打开“连接到服务器”对话框（有关详细信息，请参阅[连接到 SQL Server 实例](../../../ssms/quickstarts/connect-query-sql-server.md#connect-to-a-sql-server-instance)）。
1. 单击“选项 >>”。
1. 如果使用 SSMS 18 或更高版本：
    1. 选择“Always Encrypted”选项卡。
    1. 若要启用 Always Encrypted，请选择“启用 Always Encrypted (列加密)”。 若要禁用 Always Encrypted，请确保不选择“启用 Always Encrypted (列加密)”。
    1. 如果使用 [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)]，且 SQL Server 实例配置了安全 enclave，则可以指定 enclave 证明 url。 如果 SQL Server 实例未使用安全 enclave，请确保将“Enclave 证明 URL”文本框留空。 有关详细信息，请参阅[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md)。
1. 如果使用 SSMS 17 或更低版本：
    1. 选择“其他属性”。
    1. 若要启用 Always Encrypted，请键入 `Column Encryption Setting = Enabled`。 若要禁用 Always Encrypted，请指定 `Column Encryption Setting = Disabled`，或从“其他属性”选项卡中删除“列加密设置”设置（其默认值为“已禁用”）。   
 1. 单击“连接”。

> [!TIP]
> 在为现有“查询编辑器”窗口启用和禁用 Always Encrypted 之间进行切换：   
> 1.    右键单击“查询编辑器”窗口中的任意位置。
> 2.    选择“连接” > “更改连接…” 。这会为“查询编辑器”窗口的当前连接打开“连接到服务器”对话框。 
> 2.    按照以上步骤启用或禁用 Always Encrypted，然后单击“连接”。  
   
## <a name="parameterization-for-always-encrypted"></a><a name="param"></a>Always Encrypted 参数化   
 
Always Encrypted 参数化是 SQL Server Management Studio 中的一种功能，可自动将 Transact-SQL 变量转换为查询参数（ [SqlParameter 类](/dotnet/api/system.data.sqlclient.sqlparameter)的实例）。 （要求至少为 SSMS 版本 17.0。）这允许用于 SQL Server 的基础 .NET Framework 数据提供程序对针对加密列的数据进行检测，并在将数据发送到数据库之前对其进行加密。 
  
如果没有进行参数化，.NET Framework 数据提供程序会传递你在“查询编辑器”中编写的每个声明，作为非参数化查询。 如果查询包含定目标到加密列的文本或 Transact-SQL 变量，用于 SQL Server 的 .NET Framework 数据提供程序便无法在将查询发送到数据库之前检测和加密它们。 结果由于纯文本文字 Transact-SQL 变量和加密列之间的类型不匹配，查询将失败。 例如，假定 `SSN` 列已加密，如果没有参数化，以下查询将失败。   

```sql
DECLARE @SSN NCHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>启用和禁用 Always Encrypted 参数化

默认情况下，将禁用 Always Encrypted。

为当前的“查询编辑器”窗口启用/禁用 Always Encrypted 参数化：

1. 在主菜单中，选择“查询”  。
2. 选择“查询选项…”。
3. 导航到“执行” > “高级”。
4. 选择或取消选择“启用 Always Encrypted 参数化” 。
5. 单击“确定”。

为以后的“查询编辑器”窗口启用/禁用 Always Encrypted 参数化：

1. 在主菜单中，选择“工具”  。
2. 选择“选项...”。
3. 导航到“查询执行” > “SQL Server” > “高级”。
4. 选择或取消选择“启用 Always Encrypted 参数化” 。
5. 单击“确定”。

如果在使用已启用 Always Encrypted 的数据库连接的“查询编辑器”窗口中执行查询，但没有为“查询编辑器”窗口启用参数化，便会看到启用提示。

> [!NOTE]
> Always Encrypted 参数化仅在使用启用了 Always Encrypted 的数据库连接的“查询编辑器”窗口中有效（请参阅[为 Always Encrypted 启用和禁用参数化](#enabling-and-disabling-parameterization-for-always-encrypted)）。 如果“查询编辑器”窗口使用未启用 Always Encrypted 的数据库连接，则不会对 Transact-SQL 变量进行参数化。

### <a name="how-parameterization-for-always-encrypted-works"></a>Always Encrypted 参数化的工作方式

如果在数据库连接中同时为“查询编辑器”窗口启用了 Always Encrypted 参数化和 Always Encrypted 行为，SQL Server Management Studio 将尝试对符合以下先决条件的 Transact-SQL 变量进行参数化：

- 在同一语句中进行声明和初始化（内联初始化）。 使用 `SET` 语句单独声明的变量不会进行参数化。
- 使用单个文字进行初始化。 使用表达式（包括任何运算符或函数）进行初始化的变量不会进行参数化。

下面是 SQL Server Management Studio 将对其进行参数化的变量的示例。

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

SQL Server Management Studio 不会尝试对下面的几个变量示例进行参数化：

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
为了成功完成尝试的参数化操作：   
- 用于要参数化的变量初始化的文字类型必须与变量声明中的类型匹配。   
- 如果变量的声明类型是日期类型或时间类型，变量必须通过使用以下 [ISO 8601 标准格式](../../../t-sql/functions/cast-and-convert-transact-sql.md#date-and-time-styles)之一的字符串进行初始化。    

下面是会导致参数化错误的 Transact-SQL 变量声明的示例：   
```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```
SQL Server Management Studio 使用 Intellisense 来通知你哪些参数可成功进行参数化而哪些参数化尝试会失败（及其原因）。   

可成功进行参数化的变量的声明将在“查询编辑器”中使用警告下划线进行标记。 如果将鼠标悬停在带有警告下划线标记的声明语句之上，便会看到参数化过程结果，其中包括（变量映射到的）所生成 [SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter) 对象的关键属性值：[SqlDbType](/dotnet/api/system.data.sqlclient.sqlparameter.sqldbtype)、[Size](/dotnet/api/system.data.sqlclient.sqlparameter.size)、[Precision](/dotnet/api/system.data.sqlclient.sqlparameter.precision)、[Scale](/dotnet/api/system.data.sqlclient.sqlparameter.scale)、[SqlValue](/dotnet/api/system.data.sqlclient.sqlparameter.sqlvalue)。 还会在“错误列表”  视图的“警告”  选项卡中，显示已成功进行参数化的所有变量的完整列表。 若要打开“错误列表”  视图，请在主菜单中选择“视图”  ，然后选择“错误列表” 。    

如果 SQL Server Management Studio 已尝试对变量进行参数化，但是参数化操作失败，那么变量的声明将以错误下划线进行标记。 如果将鼠标悬停在带有错误下划线标记的声明语句之上，便会看到错误的相关结果。 还会在“错误列表”  视图的“错误”  选项卡中，显示所有变量的参数化错误的完整列表。 若要打开“错误列表”  视图，请在主菜单中选择“视图”  ，然后选择“错误列表” 。   

下面的屏幕截图介绍的是具有六个变量声明的示例。 SQL Server Management Studio 已成功对前三个变量进行了参数化。 最后三个变量不符合参数化先决条件。因此，SQL Server Management Studio 不会尝试对它们进行参数化（声明不会以任何方式进行标记）。

![屏幕截图上的示例是 6 个变量声明，其中 3 个已成功参数化，另外 3 个失败，并显示了相关的警告消息。](../../../relational-databases/security/encryption/media/always-encrypted-parameter-warnings.png)
 
下面是另一个示例，介绍了两个符合参数化的先决条件的变量，但是参数化尝试失败，因为变量未正确进行初始化。    
 
![屏幕截图上的示例是 2 个变量声明，它们最终失败，并显示了相关的错误消息。](../../../relational-databases/security/encryption/media/always-encrypted-error.png)
 
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
> 如果没有进行参数化，整个查询（包括类型转换）将在 SQL Server/Azure SQL 数据库内进行处理。 如果启用了参数化，某些类型转换将由 SQL Server Management Studio 中的 .NET Framework 来执行。 由于 .NET Framework 类型系统和 SQL Server 类型系统之间存在差异（例如某些类型的精度不同，如 float），启用了参数化来执行的查询可产生不同于未启用参数化来执行的查询的结果。 

## <a name="next-steps"></a>后续步骤
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)


## <a name="see-also"></a>另请参阅
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)