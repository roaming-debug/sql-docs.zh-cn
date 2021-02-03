---
title: 使用安全 enclave 运行 Transact-SQL 语句
description: 运行数据定义语言 (DDL) 语句配置列的加密或管理加密列的索引，以及查询加密列
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: bc92b0af972236b588369869afc5b023735ae699
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237124"
---
# <a name="run-transact-sql-statements-using-secure-enclaves"></a>使用安全 enclave 运行 Transact-SQL 语句

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 使某些 Transact-SQL (T-SQL) 语句可对服务器端安全 enclave 中的加密数据库列执行机密计算。

## <a name="statements-using-secure-enclaves"></a>使用安全 enclave 的语句

以下类型的 T-SQL 语句利用了安全 enclave。

### <a name="ddl-statements-using-secure-enclaves"></a>使用安全 enclave 的 DDL 语句

以下类型的[数据定义语言 (DDL)](../../../t-sql/statements/statements.md#data-definition-language) 语句需要使用安全 enclave。

- 使用已启用 enclave 的密钥触发就地加密操作的 [ALTER TABLE column_definition (Transact-SQL)](../../../t-sql/statements/alter-table-column-definition-transact-sql.md) 语句。 有关详细信息，请参阅[使用具有安全 enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)。
- 使用随机加密创建或更改已启用 enclave 的列上索引的 [CREATE INDEX (Transact-SQL)](../../../t-sql/statements/create-index-transact-sql.md) 和 [ALTER INDEX (Transact-SQL)](../../../t-sql/statements/alter-index-transact-sql.md) 语句。 有关详细信息，请参阅[对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)。
  
### <a name="dml-statements-using-secure-enclaves"></a>使用安全 enclave 的 DML 语句

以下[数据操作语言 (DML)](../../../t-sql/statements/statements.md#data-manipulation-language) 语句或针对使用随机加密且已启用 enclave 的列的查询需要使用安全 enclave：

- 使用以下一个或多个 Transact-SQL 运算符（受安全 enclave 支持）的查询：
  - [比较运算符](../../../mdx/comparison-operators.md)
  - [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md)
  - [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md)
  - [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md)
  - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
  - [联接](../../performance/joins.md) - [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]仅支持嵌套循环联接。 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 支持嵌套循环、哈希和合并联接
  - [SELECT - ORDER BY 子句 (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md)。 在 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中受支持。 在 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]中不受支持
  - [SELECT - GROUP BY 子句 (Transact-SQL)](../../../t-sql/queries/select-group-by-transact-sql.md)。 在 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中受支持。 在 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]中不受支持
- 可插入、更新或删除行的查询，而这些查询会触发向/从已启用 enclave 的列的索引插入和/或删除索引键。 有关详细信息，请参阅[对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)。

> [!NOTE]
> 只有使用随机加密且已启用 enclave 的列支持对索引的操作和使用 enclave 的机密 DML 查询。 确定性加密不受支持。
>
> 在 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 中，对字符串列（`char``nchar`）上使用 enclave 的机密查询需要使用为列配置的 binary2 排序顺序 (BIN2) 排序规则。 在 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中，需要使用 BIN2 或 UTF-8 排序规则。

### <a name="dbcc-commands-using-secure-enclaves"></a>使用安全 enclave 的 DBCC 命令

如果数据库包含使用随机加密且已启用 enclave 的列的索引，需要检查索引完整性的 [DBCC (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-transact-sql.md) 管理命令也可能需要使用安全 enclave。 例如 [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 和 [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)。

## <a name="prerequisites-for-running-statements-using-secure-enclaves"></a>运行使用安全 enclave 的语句的先决条件

环境需要满足以下要求才能支持使用安全 enclave 的执行语句。

- [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 实例或 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中的数据库和服务器必须正确配置为支持 enclave 和证明。 有关详细信息，请参阅[设置安全 enclave 和证明](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation)。
- 你需要从证明服务管理员处获取环境的证明 URL。

  - 如果使用的是 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 和主机监护服务 (HGS)，请参阅[确定并共享 HGS 证明 URL](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url)。
  - 如果使用的是 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 和 Microsoft Azure 证明，请参阅[确定证明策略的证明 URL](/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#secure-enclave-attestation)。

- 如果使用应用程序连接到数据库，则该应用程序使用的客户端驱动程序必须支持具有安全 enclave 的 Always Encrypted。 应用程序必须连接到已为数据库连接启用 Always Encrypted 且已正确配置证明协议和证明 URL 的数据库。 有关详细信息，请参阅[使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)。
- 如果使用的是 SQL Server Management Studio (SSMS) 或 Azure SQL Data Studio，则需在连接到数据库时启用 Always Encrypted 并配置证明协议和证明 URL。 有关详细信息，请参阅以下部分。

> [!NOTE]
> 如果使用的是缓存列加密密钥，以下操作就不需要连接到已配置 Always Encrypted 和证明的数据库：可创建或更改索引的 DDL 查询，可更新索引的 DML 查询和可检查索引完整性的 DBCC 命令。 有关详细信息，请参阅[使用缓存列加密密钥调用索引操作](always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys)。

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms"></a>在 SSMS 中运行使用 enclave 的 T-SQL 语句的先决条件

SQL Server Management Studio 的最低版本要求：

- 如果使用的是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，则为 SSMS 18.3。
- 如果使用的是 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]，则为 SSMS 18.8。

确保在查询窗口中运行语句，该查询窗口使用的连接配置了 Always Encrypted 和正确证明 URL。

1. 在“连接到服务器”对话框中，指定服务器名称，选择身份验证方法，并指定凭据。
2. 单击“选项 >>”，并选择“Always Encrypted”选项卡。
3. 选中“启用 Always Encrypted (列加密)”复选框，并指定 enclave 证明 URL。 例如，`https://hgs.bastion.local/Attestation` 或 `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`。

    ![使用 SSMS 连接到具有证明的服务器](./media/always-encrypted-enclaves/column-encryption-setting.png)

4. 选择“连接”。
5. 如果系统提示启用 Always Encrypted 参数化查询，并且你计划运行参数化 DML 查询，请选择“启用”。 有关详细信息，请参阅 [Always Encrypted 参数化](always-encrypted-query-columns-ssms.md#param)。

有关其他信息，请参阅[为数据库连接启用和禁用 Always Encrypted](always-encrypted-query-columns-ssms.md#en-dis)。

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio"></a>在 Azure Data Studio 中运行使用 enclave 的 T-SQL 语句的先决条件

建议至少使用 1.23 版本或更高版本。

确保在查询窗口中运行语句，该查询窗口使用的连接启用了 Always Encrypted 并正确配置了证明协议和证明 URL。

1. 在“连接”对话框中，单击“高级…” 。
2. 若要为连接启用 Always Encrypted，请将“Always Encrypted”字段设置为“启用” 。
3. 指定证明协议和证明 URL。
    - 如果使用的是 [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)]，请将“证明协议”设置为“主机监护服务”，然后在“Enclave 证明 URL”字段中输入主机监护服务证明 URL  。
    - 如果使用的是 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]，请将“证明协议”设置为“Azure 证明”，然后在“Enclave 证明 URL”字段中输入引用了 Microsoft Azure 证明中策略的证明 URL  。

    ![使用 Azure Data Studio 连接到具有证明的服务器](./media/always-encrypted-enclaves/azure-data-studio-connect-with-enclaves.png)

4. 单击“确定”以关闭“高级属性” 。

有关其他信息，请参阅[为数据库连接启用和禁用 Always Encrypted](always-encrypted-query-columns-ads.md#enabling-and-disabling-always-encrypted-for-a-database-connection)。

如果你计划运行参数化 DML 查询，则还需启用 [Always Encrypted 参数化](always-encrypted-query-columns-ads.md#parameterization-for-always-encrypted)。

## <a name="examples"></a>示例

本部分包含使用 enclave 的 DML 查询示例。 

这些示例使用以下架构。

```sql
CREATE SCHEMA [HR];
GO

CREATE TABLE [HR].[Jobs](
 [JobID] [int] IDENTITY(1,1) PRIMARY KEY,
 [JobTitle] [nvarchar](50) NOT NULL,
 [MinSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [MaxSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
);
GO

CREATE TABLE [HR].[Employees](
 [EmployeeID] [int] IDENTITY(1,1) PRIMARY KEY,
 [SSN] [char](11) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [FirstName] [nvarchar](50) NOT NULL,
 [LastName] [nvarchar](50) NOT NULL,
 [Salary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [JobID] [int] NULL,
 FOREIGN KEY (JobID) REFERENCES [HR].[Jobs] (JobID)
);
GO
```

### <a name="exact-match-search"></a>精确匹配搜索

以下查询对加密的 `SSN` 字符串列执行精确匹配搜索。

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="pattern-matching-search"></a>模式匹配搜索

以下查询对加密的 `SSN` 字符串列执行模式匹配搜索，搜索具有指定社会安全号码后四位的员工。

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="range-comparison"></a>范围比较

以下查询对加密的 `Salary` 列执行范围比较，搜索薪水处于指定范围内的员工。

```sql
DECLARE @MinSalary money = 40000;
DECLARE @MaxSalary money = 45000;
SELECT * FROM [HR].[Employees] WHERE [Salary] > @MinSalary AND [Salary] < @MaxSalary;
GO
```

### <a name="joins"></a>联接

以下查询使用加密的 `Salary` 列在 `Employees` 和 `Jobs` 表之间执行联接。 该查询检索薪水未处于其职位薪水范围内的员工。

```sql
SELECT * FROM [HR].[Employees] e
JOIN [HR].[Jobs] j
ON e.[JobID] = j.[JobID] AND e.[Salary] > j.[MaxSalary] OR e.[Salary] < j.[MinSalary];
GO
```

### <a name="sorting"></a>排序

以下查询基于加密的 `Salary` 列对员工记录进行排序，检索薪水前十的员工。
> [!NOTE]
> [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 支持对加密的列进行排序，但 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 不支持。

```sql
SELECT TOP(10) * FROM [HR].[Employees]
ORDER BY [Salary] DESC;
GO
```

## <a name="next-steps"></a>后续步骤

- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>另请参阅

- [排查具有安全 enclave 的 Always Encrypted 的常见问题](always-encrypted-enclaves-troubleshooting.md)
- [教程：在 SQL Server 中开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [教程：在 Azure SQL 数据库中开始使用具有安全 enclave 的 Always Encrypted](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [使用具有安全 Enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)
- [对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)
