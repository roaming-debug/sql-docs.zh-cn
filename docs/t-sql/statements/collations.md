---
description: COLLATE (Transact-SQL)
title: COLLATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COLLATE
- COLLATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], COLLATE clause
- COLLATE clause
ms.assetid: 76763ac8-3e0d-4bbb-aa53-f5e7da021daa
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dc115153671a4cd8aed490205b19f4a21c9e8949
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489487"
---
# <a name="collate-transact-sql"></a>COLLATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

定义数据库或表列的排序规则，或应用于字符串表达式时的排序规则强制转换操作。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 如果创建数据库期间未指定，则会为数据库分配 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认排序规则。 如果创建表列期间未指定，则会为该列分配数据库的默认排序规则。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
COLLATE { <collation_name> | database_default }
<collation_name> :: =
    { Windows_collation_name } | { SQL_collation_name }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

collation_name 应用于表达式、列定义或数据库定义的排序规则的名称。 collation_name 只能是指定的 Windows_collation_name 或 SQL_collation_name。 collation_name 必须是文本值。 collation_name 不能用变量或表达式表示。

Windows_collation_name 是 [Windows 排序规则名称](../../t-sql/statements/windows-collation-name-transact-sql.md)的排序规则名称。

SQL_collation_name 是 [SQL Server 排序规则名称](../../t-sql/statements/sql-server-collation-name-transact-sql.md)的排序规则名称。

database_default 使 COLLATE 子句继承当前数据库的排序规则。

## <a name="remarks"></a>备注

可以在多个级别指定 COLLATE 子句。 这些功能包括以下这些：

1. 创建或更改数据库。

    可使用 `CREATE DATABASE` 或 `ALTER DATABASE` 语句的 COLLATE 子句指定数据库的默认排序规则。 还可以在使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 创建数据库时指定排序规则。 如果不指定排序规则，则将为数据库分配 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认排序规则。

    > [!NOTE]
    > Windows 的仅 Unicode 排序规则只能与 COLLATE 子句一起使用，将排序规则应用于列级和表达式级数据的 nchar、nvarchar 和 ntext 数据类型，而不能与 COLLATE 子句一起使用来定义或更改数据库或服务器实例的排序规则。

2. 创建或更改表列。

    可以使用 `CREATE TABLE` 或 `ALTER TABLE` 语句的 COLLATE 子句指定每个字符串列的排序规则。 还可以在使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 创建表时指定排序规则。 如果不指定排序规则，将为列分配数据库的默认排序规则。

    还可使用 COLLATE 子句中的 `database_default` 选项，指定临时表中的列使用连接的当前用户数据库（而不是 tempdb）的默认排序规则。

3. 转换表达式的排序规则。

    可以使用 COLLATE 子句将字符表达式应用于某个排序规则。 为字符文本和变量分配当前数据库的默认排序规则。 为列引用分配列的定义排序规则。

标识符的排序规则取决于定义标识符的级别。 为实例级对象（如登录名和数据库名）的标识符分配实例的默认排序规则。 为数据库对象（如表、视图和列名）的标识符分配数据库的默认排序规则。 例如，对于名称差别仅在于大小写的两个表，可在使用区分大小写排序规则的数据库中创建，而不能在使用不区分大小写排序规则的数据库中创建。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。

当连接上下文与某个数据库相关时，可以创建变量、GOTO 标签、临时存储过程和临时表，且当已将上下文切换到其他数据库时引用它们。 变量、GOTO 标签、临时存储过程和临时表的标识符位于服务器实例的默认排序规则中。

COLLATE 子句仅适用于 char、varchar、text、nchar、nvarchar 和 ntext 数据类型。

COLLATE 使用 collate_name 来引用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则或 Windows 排序规则的名称，以应用于表达式、列定义或数据库定义。 collation_name 只能是指定的 Windows_collation_name 或 SQL_collation_name，并且参数必须包含文本值。 collation_name 不能用变量或表达式表示。

排序规则一般由排序规则名称标识，安装过程中除外。 在安装过程中，应该为 Windows 排序规则指定根排序规则指示符（排序规则区域设置），然后指定区分或不区分大小写或重音的排序选项。

可以执行系统函数 [fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md) 来检索 Windows 排序规则和 SQL Server 排序规则的所有有效排序规则名称的列表：

```sql
SELECT name, description
FROM fn_helpcollations();
```

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 只支持由基础操作系统支持的代码页。 在执行取决于排序规则的操作时，引用的对象所使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则必须使用计算机上运行的操作系统所支持的代码页。 这些操作可能包括：

- 当创建或更改数据库时，为数据库指定默认排序规则。
- 当创建或更改表时，为列指定排序规则。
- 还原或附加数据库时，操作系统必须支持数据库的默认排序规则，并支持数据库中的任何 char、varchar 和 text 列或参数的排序规则。

> [!NOTE]
> char 和 varchar 数据类型支持代码页转换，但是 text 数据类型不支持    。 不报告代码页转换过程中的数据丢失。
>
> 如果被引用的对象所使用或指定的排序规则使用 Windows 不支持的代码页，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会显示错误。

## <a name="examples"></a>示例

### <a name="a-specifying-collation-during-a-select"></a>A. 在执行 SELECT 过程中指定排序规则

下面的示例创建一个简单表并插入 4 行。 然后，该示例在从表中选择数据时应用了两个排序规则，演示 `Chiapas` 如何以不同方式排序。

```sql
CREATE TABLE Locations
(Place varchar(15) NOT NULL);
GO
INSERT Locations(Place) VALUES ('Chiapas'),('Colima')
                             , ('Cinco Rios'), ('California');
GO
--Apply an typical collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Latin1_General_CS_AS_KS_WS ASC;
GO
-- Apply a Spanish collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Traditional_Spanish_ci_ai ASC;
GO
```

这里是第一个查询的结果。

```output
Place
-------------
California
Chiapas
Cinco Rios
Colima
```

这里是第二个查询的结果。

```output
Place
-------------
California
Cinco Rios
Colima
Chiapas
```

### <a name="b-additional-examples"></a>B. 其他示例

有关使用 COLLATE 的其他示例，请参阅 [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md#examples) 示例 **G. 创建数据库并指定排序规则名称和选项**，以及 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#alter_column) 示例 **V. 更改列排序规则**。

## <a name="see-also"></a>另请参阅

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)
- [排序规则优先级](../../t-sql/statements/collation-precedence-transact-sql.md)
- [常量](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [表数据类型](../../t-sql/data-types/table-transact-sql.md)
