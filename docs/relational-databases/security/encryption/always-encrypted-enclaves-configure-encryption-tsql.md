---
description: 使用 Transact-SQL 配置列加密
title: 使用 Transact-SQL 就地配置列加密 | Microsoft Docs
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: e1e72a9e06c2012390a88243c3ef865ac222564b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477688"
---
# <a name="configure-column-encryption-in-place-with-transact-sql"></a>使用 Transact-SQL 配置列加密
[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本文介绍如何使用具有安全 Enclave 的 Always Encrypted 与 [ALTER TABLE ](../../../odbc/microsoft/alter-table-statement.md)/`ALTER COLUMN` 语句对列执行就地加密操作。 有关就地加密和一般先决条件的基本信息，请参阅[使用具有安全 Enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)。

使用 `ALTER TABLE` 或 `ALTER COLUMN` 语句，可以为列设置目标加密配置。 执行语句时，服务器端安全 enclave 会对列中存储的数据进行加密、重新加密或解密，具体取决于语句的列定义中指定的当前和目标加密配置。 
- 如果列当前未加密，则在列定义中指定 `ENCRYPTED WITH` 子句时会对其进行加密。
- 如果列当前已加密，且未在列定义中指定 `ENCRYPTED WITH` 子句，则会将其解密（转换为纯文本列）。
- 如果列当前已加密，则当指定 `ENCRYPTED WITH` 子句且指定的列加密类型或列加密密钥不同于当前使用的加密类型或列加密密钥时，会对列重新加密。 

> [!NOTE]
> 除了将列更改为 `NULL` 或 `NOT NULL` 或更改排序规则外，不能将加密操作与单个 `ALTER TABLE`/`ALTER COLUMN` 语句中的其他更改合并。 例如，不能在单个 `ALTER TABLE`/`ALTER COLUMN` Transact-SQL 语句中加密列并更改列的数据类型。 请单独使用两个语句。

与使用服务器端安全 Enclave 的任何查询一样，必须在启用了 Always Encrypted 和 Enclave 计算的情况下，通过连接来发送用于触发就地加密的 `ALTER TABLE`/`ALTER COLUMN` 语句。 

本文的其余部分介绍如何在 SQL Server Management Studio 中使用 `ALTER TABLE`/`ALTER COLUMN` 语句来触发就地加密。 或者，可以从应用程序发出 `ALTER TABLE`/`ALTER COLUMN`。 

> [!NOTE]
> 目前，除 SSMS 以外的工具（包括 SqlServer PowerShell 模块中的 [Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd) cmdlet 和 [Sqlcmd](../../../tools/sqlcmd-utility.md)）都不支持使用 `ALTER TABLE`/`ALTER COLUMN` 进行就地加密操作。

## <a name="perform-in-place-encryption-with-transact-sql-in-ssms"></a>在 SSMS 中使用 Transact-SQL 执行就地加密
### <a name="pre-requisites"></a>先决条件
- [使用具有安全 Enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)中已介绍了相关先决条件。
- SQL Server Management Studio 18.3 或更高版本。

### <a name="steps"></a>步骤
1. 打开一个查询窗口，并为数据库连接启用 Always Encrypted 和 enclave 计算。 有关详细信息，请参阅[为数据库连接启用和禁用 Always Encrypted](always-encrypted-query-columns-ssms.md#en-dis)。
2. 在查询窗口中，发出 `ALTER TABLE`/`ALTER COLUMN` 语句，并在 `ENCRYPTED WITH` 子句中指定启用 enclave 的列加密密钥。 对于字符串列（例如，`char`、`varchar`、`nchar`、`nvarchar`），可能还需要将排序规则更改为 BIN2 排序规则。 
    
    > [!NOTE]
    > 如果列主密钥存储在 Azure Key Vault 中，系统可能会提示你登录到 Azure。

3. 清除访问表的所有批处理和存储过程的计划缓存，以刷新参数加密信息。 
 
    ```sql
    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```
    > [!NOTE]
    > 如果不从缓存中删除受影响的查询计划，加密后首次执行查询可能会失败。

    > [!NOTE]
    > 请谨慎使用 `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` 或 `DBCC FREEPROCCACHE` 来清除计划缓存，因为这可能会导致查询性能暂时降低。 若要使清除缓存的负面影响降到最低，可以选择性地仅删除受影响的查询计划。

4.  调用 [sp_refresh_parameter_encryption](../../system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) 来更新每个模块（存储过程、函数、视图、触发器）的参数元数据，这些参数暂留在 [sys.parameters](../..//system-catalog-views/sys-parameters-transact-sql.md) 中，并且可能已通过加密列而失效。

### <a name="examples"></a>示例
#### <a name="encrypting-a-column-in-place"></a>就地加密列
下面的示例假定：
- `CEK1` 是已启用 enclave 的列加密密钥。
- `SSN` 列是纯文本列，目前使用默认的数据库排序规则，例如 Latin1，而非 BIN2 排序规则（例如，`Latin1_General_CI_AI_KS_WS`）。

该语句使用随机加密和已启用 enclave 的列加密密钥对 `SSN` 列进行就地加密。 它还使用相应的（在相同代码页）BIN2 排序规则覆盖默认数据库排序规则。

该操作在联机模式下执行 (`ONLINE = ON`)。 另请注意对 `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` 的调用，进行此调用会重新创建受到表架构更改影响的查询计划。

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char] COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
WITH
(ONLINE = ON);
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

#### <a name="re-encrypt-a-column-in-place-to-change-encryption-type"></a>对列进行就地重新加密以更改加密类型
下面的示例假定：
- `SSN` 列使用确定性加密和已启用 enclave 的列加密密钥 `CEK1` 进行加密。
- 当前在列级别设置的排序规则是 `Latin1_General_BIN2`。

以下语句使用随机加密和相同密钥 (`CEK1`) 对列进行重新加密

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

#### <a name="re-encrypt-a-column-in-place-to-rotate-a-column-encryption-key"></a>就地重新加密列以轮换列加密密钥
下面的示例假定：
- `SSN` 列使用随机加密和已启用 enclave 的列加密密钥 `CEK1` 进行加密。
- `CEK2` 是已启用 enclave 的列加密密钥（与 `CEK1` 不同）。
- 当前在列级别设置的排序规则是 `Latin1_General_BIN2`。

以下语句使用 `CEK2` 重新加密列。

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK2]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```
#### <a name="decrypt-a-column-in-place"></a>就地解密列
下面的示例假定：
- `SSN` 列使用已启用 enclave 的列加密密钥进行加密。
- 当前在列级别设置的排序规则是 `Latin1_General_BIN2`。

以下语句将解密列（并保持排序规则不变 - 或者，可以选择更改排序规则，例如，在同一语句中更改为非 BIN2 排序规则）。

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
WITH (ONLINE = ON);
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

## <a name="next-steps"></a>后续步骤
- [查询使用具有安全 enclave 的 Always Encrypted 的列](always-encrypted-enclaves-query-columns.md)
- [对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)
- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>另请参阅  
- [使用具有安全 Enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)
- [为现有加密列启用具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)