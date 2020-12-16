---
title: 动态数据掩码 | Microsoft Docs
description: 了解动态数据掩码，它通过对非特权用户屏蔽敏感数据来限制此类数据的公开。 它可极大地简化 SQL Server 中的安全保护。
ms.date: 05/02/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
ms.assetid: a62f4ff9-2953-42ca-b7d8-1f8f527c4d66
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 568b3f60205e94bd0b81ff5e80e8b3db372a4691
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480908"
---
# <a name="dynamic-data-masking"></a>动态数据屏蔽
[!INCLUDE [SQL Server 2016 ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

![动态数据掩码](../../relational-databases/security/media/dynamic-data-masking.png)

动态数据掩码 (DDM) 通过对非特权用户屏蔽敏感数据来限制敏感数据的公开。 它可以用于显著简化应用程序中安全性的设计和编码。  

动态数据掩码允许用户在尽量减少对应用程序层的影响的情况下，指定需要披露的敏感数据量，从而防止对敏感数据的非授权访问。 可以在指定的数据库字段上配置 DDM，在查询结果集中隐藏敏感数据。 使用 DDM 时，数据库中的数据不会更改。 可以轻松地对现有应用程序使用动态数据屏蔽，因为屏蔽规则是应用于查询结果。 许多应用程序可以屏蔽敏感数据，而无需修改现有查询。

* 一个中央数据掩码策略直接对数据库中的敏感字段起作用。
* 指定有权访问敏感数据的特权用户或角色。
* DDM 采用完全掩码和部分掩码功能，以及用于数值数据的随机掩码。
* 简单的 [!INCLUDE[tsql_md](../../includes/tsql-md.md)] 命令定义和管理掩码。

动态数据屏蔽旨在限制敏感数据的公开，防止没有访问权限的用户查看敏感数据。 动态数据屏蔽并不是要防止数据库用户直接连接到数据库并运行可以公开敏感数据的详尽查询。 动态数据屏蔽是对其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全功能（审核、加密、行级别安全性…）的补充，因此，强烈建议你将此功能与上述功能一起使用，以便更好地保护数据库中的敏感数据。  
  
动态数据屏蔽在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 和 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]中提供，使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令进行配置。 有关如何使用 Azure 门户来配置动态数据掩码的详细信息，请参阅[开始使用 SQL 数据库动态数据掩码（Azure 门户）](/azure/azure-sql/database/dynamic-data-masking-overview)。  
  
## <a name="defining-a-dynamic-data-mask"></a>定义动态数据屏蔽
 针对表中的列定义屏蔽规则即可模糊该列中的数据。 可以使用四种类型的屏蔽。  
  
|函数|说明|示例|  
|--------------|-----------------|--------------|  
|默认|根据指定字段的数据类型进行完全屏蔽。<br /><br /> 对于字符串数据类型，可以使用 XXXX 或者在字段不到 4 个字符长的情况下使用更少的 X（**char**、**nchar**、**varchar**、**nvarchar**、**text**、**ntext**）。  <br /><br /> 对于数字数据类型，可使用零值（**bigint** **bit** **decimal**、 **int**、 **money**、 **numeric**、 **smallint**、 **smallmoney**、 **tinyint**、 **float**、 **real**）。<br /><br /> 对于日期和时间数据类型，可使用 01.01.1900 00:00:00.0000000（**date**、**datetime2**、**datetime**、**datetimeoffset**、**smalldatetime**、**time**）。<br /><br />对于二进制数据类型，可使用单字节的 ASCII 值 0（**binary**、 **varbinary**、 **image**）。|列定义语法示例： `Phone# varchar(12) MASKED WITH (FUNCTION = 'default()') NULL`<br /><br /> ALTER 语法示例：`ALTER COLUMN Gender ADD MASKED WITH (FUNCTION = 'default()')`|  
|电子邮件|该屏蔽方法公开电子邮件地址的第一个字母，以及电子邮件地址格式中的常量后缀“.com”。 `aXXX@XXXX.com`.|定义语法示例： `Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL`<br /><br /> ALTER 语法示例：`ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()')`|  
|随机|一种随机屏蔽函数，适用于任何数字类型，可以在指定范围内使用随机值来屏蔽原始值。|定义语法示例： `Account_Number bigint MASKED WITH (FUNCTION = 'random([start range], [end range])')`<br /><br /> ALTER 语法示例：`ALTER COLUMN [Month] ADD MASKED WITH (FUNCTION = 'random(1, 12)')`|  
|自定义字符串|该屏蔽方法公开第一个和最后一个字母，在中间添加自定义填充字符串。 `prefix,[padding],suffix`<br /><br /> 注意：如果因原始值太短而无法进行完整的屏蔽，则不会公开部分前缀或后缀。|定义语法示例： `FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(prefix,[padding],suffix)') NULL`<br /><br /> ALTER 语法示例：`ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)')`<br /><br /> 其他示例：<br /><br /> `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(5,"XXXXXXX",0)')`|  
  
## <a name="permissions"></a>权限  
 不需任何特殊权限即可使用动态数据屏蔽来创建表，只需标准的 **CREATE TABLE** 权限以及对架构的 **ALTER** 权限。  
  
 添加、替换或删除对列的屏蔽，需要 **ALTER ANY MASK** 权限以及对表的 **ALTER** 权限。 可以将 **ALTER ANY MASK** 权限授予安全负责人。  
  
 具有表的 **SELECT** 权限的用户可以查看表数据。 列在被定义为“已屏蔽”后，将显示屏蔽后的数据。 对于需要从定义了屏蔽的列中检索非屏蔽数据的用户，可授予其 **UNMASK** 权限。  
  
 针对数据库的 **CONTROL** 权限包括 **ALTER ANY MASK** 和 **UNMASK** 权限。  
  
## <a name="best-practices-and-common-use-cases"></a>最佳实践和常规用例  
  
-   对列进行屏蔽不会阻止对该列进行更新。 因此，即使用户在查询被屏蔽的列时收到的是被屏蔽的数据，该用户也可以更新这些数据，前提是具有写入权限。 仍需使用适当的访问控制策略来限制更新权限。  
  
-   使用 `SELECT INTO` 或 `INSERT INTO` 将数据从经过屏蔽的列复制到另一表中会导致目标表中显示屏蔽的数据。  
  
-   运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出时，将应用动态数据屏蔽。 数据库包含已掩码的列将导致导出的数据文件也包含已掩码的数据（假定该文件是由没有 UNMASK 特权的用户导出的），并且导入的数据库将包含已静态掩码的数据。  
  
## <a name="querying-for-masked-columns"></a>查询屏蔽列  
 使用 **sys.masked_columns** 视图可查询对其应用了屏蔽函数的表列。 该视图继承自 **sys.columns** 视图。 该视图会返回 **sys.columns** 视图中的所有列，以及 **is_masked** 和 **masking_function** 列，表明该列是否被屏蔽，以及在该列被屏蔽的情况下定义了什么屏蔽函数。 该视图仅显示在其上应用了屏蔽函数的列。  
  
```sql 
SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.[object_id] = tbl.[object_id]  
WHERE is_masked = 1;  
```  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 不能针对以下列类型定义屏蔽规则：  
  
-   加密列（始终加密）  
  
-   FILESTREAM  
  
-   COLUMN_SET 或属于列集一部分的稀疏列。  
  
-   不能在经过计算的列上配置屏蔽，但如果这个经过计算的列所依赖的列标有 MASK 字样，则这个经过计算的列会返回经过屏蔽的数据。  
  
-   进行数据屏蔽的列不能是 FULLTEXT 索引的键。  
  
 如果用户没有 **UNMASK** 权限，则无法在配置了动态数据屏蔽的列上执行不推荐使用的 **READTEXT**、 **UPDATETEXT** 和 **WRITETEXT** 语句。 
 
 添加动态数据屏蔽是作为架构更改对基础表执行的，因此无法对具有依赖项的列执行。 若要解决此限制，可先删除依赖项，然后添加动态数据屏蔽，再重新创建依赖项。 例如，如果依赖项是由于索引依赖于该列，则可以删除索引，然后添加掩码，再重新创建依赖索引。
 

## <a name="security-note-bypassing-masking-using-inference-or-brute-force-techniques"></a>安全说明：可使用推断或强力破解技术绕过屏蔽

动态数据屏蔽旨在通过在应用程序使用的一组预定义查询中限制数据泄露，来简化应用程序开发。 虽然动态数据屏蔽也可以用于在直接访问生产数据库时防止敏感数据的意外泄露，不过请务必注意，具有即席查询权限的非特权用户可以应用技术来获取对实际数据的访问权限。 如果需要授予这类即席访问权限，则应使用审核监视所有数据库活动并缓解这种情况。
 
例如，请考虑一个数据库主体，该主体具有足够权限来对数据库运行即席查询，尝试“猜测”基础数据并最终推断实际值。 假设我们对 `[Employee].[Salary]` 列定义了一个掩码，此用户直接连接到数据库并开始猜测值，从而最终推断一组员工的 `[Salary]` 值：
 

```sql
SELECT ID, Name, Salary FROM Employees
WHERE Salary > 99999 and Salary < 100001;
```

>    |  ID | 名称| 薪水 |   
>    | ----- | ---------- | ------ | 
>    |  62543 | Jane Doe | 0 | 
>    |  91245 | John Smith | 0 |  

这演示动态数据屏蔽不应用作独立度量值来完全保护敏感数据免受用户对数据库运行的即席查询。 这适用于防止意外的敏感数据泄露，但无法防范推断基础数据的恶意企图。
 
请务必正确管理针对数据库的权限，并且始终遵循最小必需权限原则。 此外，请记住启用审核以跟踪对数据库进行的所有活动。

  
## <a name="examples"></a>示例  
  
### <a name="creating-a-dynamic-data-mask"></a>创建动态数据屏蔽  
 以下示例创建的表使用三种不同类型的动态数据屏蔽。 该示例会对表进行填充，在执行选择操作后即可显示结果。  
  
```sql
CREATE TABLE Membership  
  (MemberID int IDENTITY PRIMARY KEY,  
   FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)') NULL,  
   LastName varchar(100) NOT NULL,  
   Phone varchar(12) MASKED WITH (FUNCTION = 'default()') NULL,  
   Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL);  
  
INSERT Membership (FirstName, LastName, Phone, Email) VALUES   
('Roberto', 'Tamburello', '555.123.4567', 'RTamburello@contoso.com'),  
('Janice', 'Galvin', '555.123.4568', 'JGalvin@contoso.com.co'),  
('Zheng', 'Mu', '555.123.4569', 'ZMu@contoso.net');  
SELECT * FROM Membership;  
```  
  
 创建了一个新用户并向其授予了对表的 **SELECT** 权限。 执行查询后， `TestUser` 看到的是经过屏蔽的数据。  
  
```sql 
CREATE USER TestUser WITHOUT LOGIN;  
GRANT SELECT ON Membership TO TestUser;  
  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;  
```  
  
 结果表明对数据进行了屏蔽，即数据已从  
  
 `1    Roberto     Tamburello    555.123.4567    RTamburello@contoso.com`  
  
 更改为  
  
 `1    RXXXXXXX    Tamburello    xxxx            RXXX@XXXX.com`  
  
### <a name="adding-or-editing-a-mask-on-an-existing-column"></a>添加或编辑对现有列的屏蔽  
 使用 **ALTER TABLE** 语句可以添加对表中现有列的屏蔽，或者对该列的屏蔽进行编辑。  
以下示例向 `LastName` 列添加了一个屏蔽函数：  
  
```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName ADD MASKED WITH (FUNCTION = 'partial(2,"XXX",0)');  
```  
  
 以下示例更改了 `LastName` 列的屏蔽函数：  

```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName varchar(100) MASKED WITH (FUNCTION = 'default()');  
```  
  
### <a name="granting-permissions-to-view-unmasked-data"></a>授权查看未经屏蔽数据的权限  
 授予 **UNMASK** 权限即可让 `TestUser` 查看未经屏蔽的数据。  
  
```sql
GRANT UNMASK TO TestUser;  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;   
  
-- Removing the UNMASK permission  
REVOKE UNMASK TO TestUser;  
```  
  
### <a name="dropping-a-dynamic-data-mask"></a>删除动态数据屏蔽  
 以下语句将删除上述示例中创建的针对 `LastName` 列的屏蔽：  
  
```sql  
ALTER TABLE Membership   
ALTER COLUMN LastName DROP MASKED;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_definition (Transact-SQL)](../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
 [sys.masked_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-masked-columns-transact-sql.md)   
 [开始使用 SQL 数据库动态数据掩码（Azure 门户）](/azure/azure-sql/database/dynamic-data-masking-overview)
