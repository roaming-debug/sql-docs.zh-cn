---
title: 行级安全性 | Microsoft Docs
description: 了解行级别安全性如何使用组成员身份或执行上下文来控制对 SQL Server 中数据库表中行的访问权限。
ms.custom: ''
ms.date: 09/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- access control predicates
- row level security
- security [SQL Server], predicate based access control
- row level security described
- predicate based security
ms.assetid: 7221fa4e-ca4a-4d5c-9f93-1b8a4af7b9e8
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 728650497849454ab7c30dae04317a750665a26c
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442961"
---
# <a name="row-level-security"></a>行级安全性

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  ![行级安全图](../../relational-databases/security/media/row-level-security-graphic.png "行级安全图")  
  
借助行级别安全性，可以使用组成员资格或执行上下文来控制对数据库表中行的访问权限。
  
行级别安全性 (RLS) 简化了应用程序中安全性的设计和编码。 RLS 可帮助你实现对数据行访问的限制。 例如，可以确保工作人员仅访问与其部门相关的数据行。 再例如，将客户的数据访问权限限制为，仅访问与其公司相关的数据。  
  
访问限制逻辑位于数据库层中，而不是在另一个应用层中远离数据。 数据库系统会在每次尝试从任何层进行数据访问时应用访问限制。 这样就会通过减少安全系统的外围应用，使安全系统变得更加可靠和稳健。  
  
可使用 [CREATE SECURITY POLICY](../../t-sql/statements/create-security-policy-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句以及作为 [内联表值函数](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)创建的谓词来实现 RLS。  

**适用范围**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 至 [当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)）、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]（[获取](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)）、[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]。
  
> [!NOTE]
> Azure Synapse 仅支持筛选谓词。 Azure Synapse 暂不支持阻止谓词。

## <a name="description"></a><a name="Description"></a> 说明

RLS 支持两种类型的安全谓词。  
  
- 筛选器谓词以静默方式筛选可用于读取操作（SELECT、UPDATE 和 DELETE）的行。  
  
- 阻止谓词显式阻止违反该谓词的写入操作（AFTER INSERT、AFTER UPDATE、BEFORE UPDATE、BEFORE DELETE）。  
  
 对表中的行级数据的访问将受到定义为内联表值函数的安全谓词的限制。 随后调用该函数，并由安全策略进行实施。 对于筛选谓词，应用程序不知道从结果集中筛选掉的行。 如果所有行都被筛选掉，返回的是空集。 对于阻止谓词，违反该谓词的任何操作将失败并出错。  
  
 筛选谓词在读取基表中数据时应用。 它们影响所有 Get 操作：SELECT、DELETE 和 UPDATE。 用户无法选择或删除筛选掉的行。 用户无法更新筛选掉的行。 但可以更新以后将要筛选掉的行。 阻止谓词影响所有写入操作。  
  
- AFTER INSERT 和 AFTER UPDATE 谓词可以防止用户将行更新为违反该谓词的值。  
  
- BEFORE UPDATE 谓词可以防止用户更新当前违反该谓词的行。  
  
- BEFORE DELETE 谓词可以阻止删除操作。  
  
 筛选器和阻止谓词以及安全策略具有以下行为：  
  
- 你可以定义与另一个表联接和/或调用函数的谓词函数。 如果使用 `SCHEMABINDING = ON`（默认设置）创建安全策略，则该联接或函数可以从查询进行访问并按预期方式工作而无需进行任何其他权限检查。 如果安全策略是使用 `SCHEMABINDING = OFF` 创建的，用户必须对这些附加表和函数拥有 SELECT 权限，才能查询目标表。 如果谓词函数调用 CLR 标量值函数，则还需要 EXECUTE 权限。
  
- 你可以针对已定义但禁用安全谓词的表发出查询。 筛选掉或阻止的任何行都不会受影响。  
  
- 如果 dbo 用户、db_owner 角色的成员或表所有者查询已定义并启用安全策略的表，行按照安全策略所定义被筛选掉或阻止。  
  
- 尝试更改架构绑定安全策略绑定的表的架构会导致错误。 但是，可以更改谓词未引用的列。  
  
- 如果表已针对指定操作定义了谓词，尝试向表添加谓词会导致错误出现。 无论是否已启用谓词，都会这样。
  
- 如果函数在架构绑定安全策略中用作表中的谓词，尝试修改函数会导致错误出现。  
  
- 定义包含非重叠谓词的多个活动安全策略会成功。  
  
 筛选器谓词具有以下行为：  
  
- 定义筛选表中的行的安全策略。 应用程序不知道任何针对 SELECT、UPDATE 和 DELETE 操作被筛选掉的行。 包括所有行都被筛选掉的情况。应用程序可以对行执行 INSERT 操作，即使这些行将在其他任何操作过程中被筛选掉，也不例外。  
  
 阻止谓词具有以下行为：  
  
- UPDATE 的阻止谓词根据 BEFORE 和 AFTER 拆分为单独的操作。 因此，举例来说，无法阻止用户将行值更新为大于当前值。 如果需要这种逻辑，必须对 [DELETED 和 INSERTED](../triggers/use-the-inserted-and-deleted-tables.md) 中间表使用触发器来一起引用旧值和新值。  
  
- 如果谓词函数使用的列未更改，优化器不会检查 AFTER UPDATE 阻止谓词。 例如：Alice 不应该能够将薪金更改为大于 100,000。 只要谓词中引用的列未更改，Alice 就可以更改薪金已超过 100,000 的员工的地址。  
  
- 批量操作 API（包括 BULK INSERT）未发生变化。 这意味着，阻止谓词 AFTER INSERT 将应用于批量插入操作，就像普通的插入操作一样。  
  
## <a name="use-cases"></a><a name="UseCases"></a> 用例

 下面是有关如何使用 RLS 的设计示例：  
  
- 医院可以创建安全策略，允许护士仅查看自己患者的数据行。  
  
- 银行可以创建策略，以根据员工所属的业务部门或在公司中的职责来限制对财务数据行的访问权限。  
  
- 多租户应用程序可以创建一个策略以强制对每个租户的数据行与所有其他租户的行进行逻辑分离。 可通过将许多租户的数据存储在单个表中来实现效率。 每个租户只能查看自己的数据行。  
  
 RLS 筛选器谓词在功能上等效于追加 **WHERE** 子句。 谓词可以如同业务做法规定一样复杂，或子句可以如同 `WHERE TenantId = 42`一样简单。  
  
 用更正式的术语来说，RLS 引入了基于谓词的访问控制。 它以基于谓词的集中式灵活评估为重点。 谓词可以基于元数据，也可以基于管理员根据需要确定的其他任何条件。 谓词用作一个条件，以便基于用户属性来确定用户是否具有合适的数据访问权限。 基于标签的访问控制可以通过使用基于谓词的访问控制来实现。  
  
## <a name="permissions"></a><a name="Permissions"></a> 权限

 创建、更改或删除安全策略需要 **ALTER ANY SECURITY POLICY** 权限。 创建或删除安全策略需要针对架构的 **ALTER** 权限。  
  
 另外，每个添加的谓词都需要以下权限：  
  
- 针对用作谓词的函数的 **SELECT** 和 **REFERENCES** 权限。  
  
- 针对绑定到策略的目标表的 **REFERENCES** 权限。  
  
- 针对目标表上用作参数的每一列的 **REFERENCES** 权限。  
  
 安全策略应用于所有用户（包括数据库中的 dbo 用户）。 Dbo 用户可以更改或删除安全策略，但是可以审核他们对安全策略进行的更改。 如果高特权用户（如 sysadmin 或 db_owner）需要查看所有行来排除故障或验证数据，必须为此编写安全策略。  
  
 如果使用 `SCHEMABINDING = OFF`创建安全策略，则若要查询目标表，用户必须具有针对谓词函数及在其中使用的任何其他表、视图或函数的  **SELECT** 或 **EXECUTE** 权限。 如果使用 `SCHEMABINDING = ON` 创建安全策略（默认值），则当用户查询目标表时，会绕过这些权限检查。  
  
## <a name="best-practices"></a><a name="Best"></a> 最佳实践  
  
- 强烈建议为 RLS 对象、谓词函数和安全策略单独创建架构。 这有助于将针对这些特殊对象所需的权限与目标表分开。 在多租户数据库中可能需要对不同策略和谓词函数进行额外的分隔，但具体标准各有不同。
  
- **ALTER ANY SECURITY POLICY** 权限适用于高特权用户（如安全策略管理员）。 安全策略管理员不需要针对他们保护的表的 SELECT 权限。  
  
- 避免在谓词函数中进行类型转换以避免潜在的运行时错误。  
  
- 尽可能避免在谓词函数中进行递归以避免性能降低。 查询优化器会尝试检测直接递归，但无法保证查找间接递归。 间接递归是指第二个函数调用谓词函数。  
  
- 避免在谓词函数中使用过多表联接以便使性能最大化。  
  
 避免使用依赖于会话特定 [SET 选项](../../t-sql/statements/set-statements-transact-sql.md)的谓词逻辑：如果用户可以执行任意查询，则其逻辑依赖于会话特定的 SET 选项的谓词函数可能会透漏信息，不过，这种逻辑很少在实际应用程序中使用。 例如，将字符串隐式转换为 **datetime** 的谓词函数可能会根据当前会话的 **SET DATEFORMAT** 选项筛选不同的行。 一般而言，谓词函数应遵守以下规则：  
  
- 谓词函数不应将字符串隐式转换为 **date**、**smalldatetime**、**datetime**、**datetime2** 或 **datetimeoffset**，也不应执行相反的转换，因为这些转换受 [SET DATEFORMAT (Transact-SQL)](../../t-sql/statements/set-dateformat-transact-sql.md) 和 [SET LANGUAGE (Transact-SQL)](../../t-sql/statements/set-language-transact-sql.md) 选项的影响。 应改用 **CONVERT** 函数并显式指定样式参数。  
  
- 谓词函数不应依赖于每周第一天的值，因为此值受 [SET DATEFIRST (Transact-SQL)](../../t-sql/statements/set-datefirst-transact-sql.md) 选项的影响。  
  
- 谓词函数不得依赖在出错（如溢出或被零除）时返回 NULL 的算术表达式或聚合表达式，因为这种行为受 [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md)、[SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md) 和 [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md) 选项影响。  
  
- 谓词函数不应将串联字符串与 **NULL** 进行比较，因为这种行为受 [SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md) 选项的影响。  

## <a name="security-note-side-channel-attacks"></a><a name="SecNote"></a>安全说明：旁道攻击

### <a name="malicious-security-policy-manager"></a>恶意安全策略管理员

观察到以下这点十分重要：具有足够权限来基于敏感列创建安全策略并且有权创建或更改内联表值函数的恶意安全策略管理员可以与另一个对表具有选择权限的用户串通，通过恶意创建旨在使用旁路攻击推断数据的内联表值函数来泄漏数据。 此类攻击需要进行串通（或向恶意用户授予过多权限），并且可能需要多次反复修改策略（需要删除谓词以便中断架构绑定的权限）、修改内联表值函数并重复对目标表运行选择语句。 建议根据需要限制权限，并监视是否有任何可疑活动。 应监视不断更改与行级别安全性相关的策略和内联表值函数等活动。  
  
### <a name="carefully-crafted-queries"></a>精心设计的查询

使用精心设计的查询可能会造成信息泄露。 例如， `SELECT 1/(SALARY-100000) FROM PAYROLL WHERE NAME='John Doe'` 会让恶意用户知道 John Doe 的薪金是 100000 美元。 即使采用安全谓词来防止恶意用户直接查询其他人的薪金，用户仍然可以确定查询何时返回被零除异常。  

## <a name="cross-feature-compatibility"></a><a name="Limitations"></a> 跨功能兼容性

 一般情况下，行级别安全性将按预期对各种功能正常运行。 但存在几种例外情况。 本部分介绍对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的某些其他功能使用行级别安全性的说明和注意事项。  
  
- **DBCC SHOW_STATISTICS** 报告有关未筛选数据的统计信息，因此可能会泄漏在其他情况下受安全策略保护的信息。 因此，应限制使用行级别安全性策略查看表的统计信息对象的访问权限。 用户必须是表所有者，或必须是 sysadmin 固定服务器角色、db_owner 固定服务器角色或 db_ddladmin 固定数据库角色的成员。  
  
- **Filestream：** RLS 与 Filestream 不兼容。  
  
- **PolyBase：** 仅支持对 Azure Synapse 的 PolyBase 外部表使用 RLS。

- **内存优化表：** 必须使用 `WITH NATIVE_COMPILATION` 选项定义在内存优化表中用作安全谓词的内联表值函数。 使用此选项时，内存优化表不支持的语言功能将被禁止，并在创建时发出相应的错误。 有关详细信息，请参阅 **内存优化表简介** 中的 [内存优化表中的行级别安全性](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md)部分。  
  
- **索引视图：** 一般情况下，可以在视图基础之上创建安全策略，并能在安全策略绑定的表基础之上创建视图。 但是，不能在具有安全策略的表顶层创建索引视图，因为通过索引执行的行查找将跳过策略。  
  
- **变更数据捕获：** 变更数据捕获可能会将应被筛选掉的全部行泄露给 db_owner 角色的成员，或在为表启用 CDC 时指定的“限制”角色的成员用户（请注意，可以显式将此功能设置为 NULL，让所有用户都能访问变更数据）。 实际上， **db_owner** 和此选通角色的成员可以看到对表所做的所有数据更改，即使表中存在安全策略。  
  
- **更改跟踪：** 更改跟踪可能会将应被筛选掉的行的主键泄露给同时拥有 SELECT 和 VIEW CHANGE TRACKING 权限的用户。 实际数据值不会泄漏；只会透露已更新/插入/删除具有 B 主键的行的列 A 这一事实。 如果主键包含机密元素（如社会安全号码），这会产生问题。 但是，在实践中，此 **CHANGETABLE** 几乎始终与原始表联接以获取最新数据。  
  
- **全文搜索：** 对于使用以下全文搜索和语义搜索函数的查询，性能应该会下降，因为引入了附加联接，以应用行级别安全性，并避免泄露应被筛选掉的行的主键：CONTAINSTABLE、FREETEXTTABLE、semantickeyphrasetable、semanticsimilaritydetailstable、semanticsimilaritytable 。  
  
- **列存储索引：** RLS 与聚集和非聚集列存储索引兼容。 但是，由于行级别安全性应用了一个函数，优化器可能会修改查询计划，从而不会使用批处理模式。  
  
- **分区视图：** 无法对分区视图定义阻止谓词，无法在使用阻止谓词的表基础之上创建分区视图。 筛选器谓词与分区视图兼容。  
  
- **临时表：** 临时表与 RLS 兼容。 但是，当前表中的安全谓词不会自动复制到历史记录表。 若要将安全策略应用到当前表和历史记录表，必须单独在每个表中添加安全谓词。  
  
## <a name="examples"></a><a name="CodeExamples"></a> 示例  
  
### <a name="a-scenario-for-users-who-authenticate-to-the-database"></a><a name="Typical"></a> A. 用户在数据库中进行身份验证的方案

 此示例创建三个用户和一个表，并在表中填充六行。 然后，它为表创建内联表值函数和安全策略。 接下来，此示例展示如何为各种用户筛选选择语句。  
  
 创建将演示不同访问功能的三个用户帐户。  


```sql  
CREATE USER Manager WITHOUT LOGIN;  
CREATE USER Sales1 WITHOUT LOGIN;  
CREATE USER Sales2 WITHOUT LOGIN;  
```

创建用于保留数据的表。  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

 使用六行数据填充该表（对于每个销售代表显示三个订单）。  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

向每个用户授予表的读取访问权限。  

```sql
GRANT SELECT ON Sales TO Manager;  
GRANT SELECT ON Sales TO Sales1;  
GRANT SELECT ON Sales TO Sales2;  
```

创建一个新架构和一个内联表值函数。 该函数在 SalesRep 列中的行与执行查询的用户相同时 (`@SalesRep = USER_NAME()`) 或是在执行查询的用户是 Manager 用户 (`USER_NAME() = 'Manager'`) 时返回 1。

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

创建一个安全策略（将该函数添加为筛选器谓词）。 状态必须设置为 ON 以启用该策略。

```sql
CREATE SECURITY POLICY SalesFilter  
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales  
WITH (STATE = ON);  
```

允许 fn_securitypredicate 函数的 SELECT 权限 
```sql
GRANT SELECT ON security.fn_securitypredicate TO Manager;  
GRANT SELECT ON security.fn_securitypredicate TO Sales1;  
GRANT SELECT ON security.fn_securitypredicate TO Sales2;  
```

现在通过作为每个用户从 Sales 表进行选择来测试筛选器谓词。

```sql
EXECUTE AS USER = 'Sales1';  
SELECT * FROM Sales;
REVERT;  
  
EXECUTE AS USER = 'Sales2';  
SELECT * FROM Sales;
REVERT;  
  
EXECUTE AS USER = 'Manager';  
SELECT * FROM Sales;
REVERT;  
```

管理员应看到所有六行。 Sales1 和 Sales2 用户应只能看到自己的销售情况。

更改安全策略以禁用策略。

```sql
ALTER SECURITY POLICY SalesFilter  
WITH (STATE = OFF);  
```

现在，Sales1 和 Sales2 用户可以看到所有六行。

连接到 SQL 数据库以清理资源

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

### <a name="b-scenarios-for-using-row-level-security-on-an-azure-synapse-external-table"></a><a name="external"></a> B. 对 Azure Synapse 外部表使用行级别安全性的方案

此简短示例创建三个用户，以及一个包含六行的外部表。 然后，它为外部表创建内联表值函数和安全策略。 该示例演示如何为各种用户筛选选择语句。 

### <a name="prerequisites"></a>先决条件

1. 必须具有专用 SQL 池。 请参阅[创建专用 SQL 池](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal)
1. 托管专用 SQL 池的服务器必须向 AAD 注册，并且你应具有一个包含存储博客参与者权限的 Azure 存储帐户。 按照[此处](/azure/azure-sql/database/vnet-service-endpoint-rule-overview#steps)的步骤操作。
1. 为 Azure 存储帐户创建一个文件系统。 使用存储资源管理器查看存储帐户。 右键单击容器，然后选择“创建文件系统”。  

满足先决条件后，创建三个用户帐户来演示不同的访问功能。

```sql
--run in master
CREATE LOGIN Manager WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales1 WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales2 WITH PASSWORD = '<user_password>'
GO

--run in master and your dedicated SQL pool database
CREATE USER Manager FOR LOGIN Manager;  
CREATE USER Sales1  FOR LOGIN Sales1;  
CREATE USER Sales2  FOR LOGIN Sales2 ;
```

创建用于保留数据的表。  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

使用六行数据填充该表（对于每个销售代表显示三个订单）。  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

根据刚创建的 Sales 表，创建 Synapse Analytics 外部表。

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<user_password>';

CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';

CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://<file_system_name@storage_account>.dfs.core.windows.net', CREDENTIAL = msi_cred);

CREATE EXTERNAL FILE FORMAT MSIFormat  WITH (FORMAT_TYPE=DELIMITEDTEXT);
  
CREATE EXTERNAL TABLE Sales_ext WITH (LOCATION='<your_table_name>', DATA_SOURCE=ext_datasource_with_abfss, FILE_FORMAT=MSIFormat, REJECT_TYPE=Percentage, REJECT_SAMPLE_VALUE=100, REJECT_VALUE=100)
AS SELECT * FROM sales;
```

为已创建的外部表 Sales_ext 上的三个用户授予 SELECT 权限。

```sql
GRANT SELECT ON Sales_ext TO Sales1;  
GRANT SELECT ON Sales_ext TO Sales2;  
GRANT SELECT ON Sales_ext TO Manager;
```

创建一个新架构和一个内联表值函数，你可能已在示例 A 中完成了此操作。该函数在 SalesRep 列中的行与执行查询的用户相同时 (`@SalesRep = USER_NAME()`)，或者在执行查询的用户是 Manager 用户 (`USER_NAME() = 'Manager'`) 时返回 1。

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

将内联表值函数用作筛选谓词，对外部表创建安全策略。 状态必须设置为 ON 以启用该策略。

```sql
CREATE SECURITY POLICY SalesFilter_ext
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales_ext  
WITH (STATE = ON);
```

现在，通过从 Sales_ext 外部表中进行选择，测试筛选谓词。 以每个用户（Sales1、Sales2 和管理员）的身份登录。 以每个用户的身份运行以下命令。

```sql
SELECT * FROM Sales_ext;
```

管理员应看到所有六行。 Sales1 和 Sales2 用户应只能看到自己的销售额。

更改安全策略以禁用策略。

```sql
ALTER SECURITY POLICY SalesFilter_ext  
WITH (STATE = OFF);  
```

现在 Sales1 和 Sales2 用户可以看到所有六行。

连接到 Azure Synapse 数据库以清理资源

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter_ext;
DROP TABLE Sales;
DROP EXTERNAL TABLE Sales_ext;
DROP EXTERNAL DATA SOURCE ext_datasource_with_abfss ;
DROP EXTERNAL FILE FORMAT MSIFormat;
DROP DATABASE SCOPED CREDENTIAL msi_cred; 
DROP MASTER KEY;
```

连接到逻辑主数据库以清理资源。

```sql
DROP LOGIN Sales1;
DROP LOGIN Sales2;
DROP LOGIN Manager;
```

### <a name="c-scenario-for-users-who-connect-to-the-database-through-a-middle-tier-application"></a><a name="MidTier"></a> C. 用户通过中间层应用程序连接到数据库的方案

> [!NOTE]
> 在此示例中，Azure Synapse当前不支持阻止谓词功能，因此 Azure Synapse 不会阻止插入错误用户 ID 的行。

此示例演示一个中间层应用程序如何实现连接筛选，其中应用程序用户（或租户）共享同一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户（应用程序）。 应用程序连接到数据库之后在 [SESSION_CONTEXT (Transact-SQL)](../../t-sql/functions/session-context-transact-sql.md) 中设置当前应用程序用户 ID，然后安全策略以透明方式筛选不应对此 ID 可见的行，同时阻止用户插入错误用户 ID 的行。 无需进行任何其他应用更改。  
  
 创建用于保留数据的表。

```sql
CREATE TABLE Sales (  
    OrderId int,  
    AppUserId int,  
    Product varchar(10),  
    Qty int  
);  
```

使用六行数据填充该表（对于每个应用程序用户显示三个订单）。

```sql
INSERT Sales VALUES
    (1, 1, 'Valve', 5),
    (2, 1, 'Wheel', 2),
    (3, 1, 'Valve', 4),  
    (4, 2, 'Bracket', 2),
    (5, 2, 'Wheel', 5),
    (6, 2, 'Seat', 5);  
```

创建应用程序用来建立连接的低特权用户。

```sql
-- Without login only for demo  
CREATE USER AppUser WITHOUT LOGIN;
GRANT SELECT, INSERT, UPDATE, DELETE ON Sales TO AppUser;  
  
-- Never allow updates on this column  
DENY UPDATE ON Sales(AppUserId) TO AppUser;  
```

创建一个新架构和谓词函数（将使用存储在 **SESSION_CONTEXT** 中的应用程序用户 ID 来筛选行）。

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@AppUserId int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result  
    WHERE  
        DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('AppUser')
        AND CAST(SESSION_CONTEXT(N'UserId') AS int) = @AppUserId;
GO  
```

创建一个安全策略，用于将此函数添加为 `Sales`上的筛选器谓词和阻止谓词。 阻止谓词只需要 **AFTER INSERT**，因为 **BEFORE UPDATE** 和 **BEFORE DELETE** 已筛选；不需要 **AFTER UPDATE** ，因为 `AppUserId` 列不能更新为其他值，原因是前面设置了列权限。

```sql
CREATE SECURITY POLICY Security.SalesFilter  
    ADD FILTER PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales,  
    ADD BLOCK PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales AFTER INSERT
    WITH (STATE = ON);  
```

在 `Sales` SESSION_CONTEXT **中设置不同的用户 ID 后，可以通过从** 表进行选择，来模拟连接筛选。 在实践中，应用程序负责在打开连接后在 **SESSION_CONTEXT** 中设置当前用户 ID。

```sql
EXECUTE AS USER = 'AppUser';  
EXEC sp_set_session_context @key=N'UserId', @value=1;  
SELECT * FROM Sales;  
GO  
  
/* Note: @read_only prevents the value from changing again until the connection is closed (returned to the connection pool)*/
EXEC sp_set_session_context @key=N'UserId', @value=2, @read_only=1;
  
SELECT * FROM Sales;  
GO  
  
INSERT INTO Sales VALUES (7, 1, 'Seat', 12); -- error: blocked from inserting row for the wrong user ID  
GO  
  
REVERT;  
GO  
```

清理数据库资源。

```sql
DROP USER AppUser;

DROP SECURITY POLICY Security.SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

## <a name="see-also"></a>另请参阅

 [CREATE SECURITY POLICY (Transact-SQL)](../../t-sql/statements/create-security-policy-transact-sql.md)</br>
 [ALTER SECURITY POLICY (Transact-SQL)](../../t-sql/statements/alter-security-policy-transact-sql.md)</br>
 [DROP SECURITY POLICY (Transact-SQL)](../../t-sql/statements/drop-security-policy-transact-sql.md)</br>
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)</br>
 [SESSION_CONTEXT (Transact-SQL)](../../t-sql/functions/session-context-transact-sql.md)</br>
 [sp_set_session_context (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)</br>
 [sys.security_policies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)</br>
 [sys.security_predicates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)</br>
 [创建用户定义函数（数据库引擎）](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)