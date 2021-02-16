---
description: Microsoft SQL Server 分布式查询：OLE DB 连接
title: 创建链接服务器提供程序
ms.date: 07/01/2019
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: MikeRayMSFT
ms.topic: conceptual
author: pmasl
ms.author: pelopes
manager: rothj
ms.custom: seo-dt-2019
ms.openlocfilehash: 75134031d7567dcc5a653d28ecb1c92a26891d23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080408"
---
# <a name="microsoft-sql-server-distributed-queries-ole-db-connectivity"></a>Microsoft SQL Server 分布式查询：OLE DB 连接

本文介绍 Microsoft SQL Server 查询处理器如何与 OLE DB 提供程序进行交互以启用分布式和异类查询。 本文主要面向 OLE DB 提供程序开发者，并假定他们对 OLE DB 规范有充分的了解。 重点介绍 SQL Server 查询处理器和 OLE DB 提供程序之间的 OLE DB 接口，而不是分布式查询功能本身。 有关分布式查询功能的完整说明，请参阅[链接服务器](../../relational-databases/linked-servers/linked-servers-database-engine.md)。

## <a name="overview-and-terminology"></a>概述和术语

 在 Microsoft SQL Server 中，分布式查询使 SQL Server 用户能够访问基于 SQL Server 的服务器之外的数据，无论数据是在运行 SQL Server 的其他服务器中还是在公开 OLE DB 接口的其他数据源中。 OLE DB 提供了一种从异类数据源统一访问表格数据的方法。

就本文而言，分布式查询是指任何 `SELECT`、`INSERT`、`UPDATE` 或 `DELETE` 语句，这些语句引用一个或多个外部 OLE DB 数据源中的表和行集。

远程表是存储在 OLE DB 数据源中的表，并且处于运行 SQL Server（用于执行查询）的服务器外部。 分布式查询访问一个或多个远程表。

### <a name="ole-db-provider-categories"></a>OLE DB 提供程序类别

以下是从 SQL Server 分布式查询角度，根据 OLE DB 提供程序的功能对其进行的分类。 根据定义，它们并不相互排斥；一个给定的提供程序可能属于以下类别中的多个类别：

- SQL 命令提供程序

- 索引提供程序

- 简单表提供程序

- 非 SQL 命令提供程序

#### <a name="sql-command-providers"></a>SQL 命令提供程序

支持 `Command` 对象和 SQL Server 可识别的 SQL 标准方言的提供程序属于此类别。 给定的 OLE DB 提供程序只有满足以下特定要求才会被 SQL Server 视为 SQL 命令提供程序：

- 提供程序必须支持 `Command` 对象及其所有强制性 OLE DB 接口：`ICommand`、`ICommandText`、`IColumnsInfo`、`ICommandProperties` 和 `IAccessor`。

- 提供程序支持的 SQL 方言必须至少为 SQL Subminimum。 提供程序必须通过 `DBPROP_SQLSUPPORT` 属性报告方言。

SQL 命令提供程序的示例包括 SQL Server 的 Microsoft OLE DB 提供程序和 ODBC 的 Microsoft OLE DB 提供程序。

#### <a name="index-providers"></a>索引提供程序

索引提供程序是那些根据 OLE DB 支持和公开索引，并允许基于索引的基表进行查找的提供程序。 给定的 OLE DB 提供程序只有满足以下特定要求才会被 SQL Server 视为索引提供程序：

- 提供程序必须支持带有 TABLES、COLUMNS 和 INDEXES 架构行集的 `IDBSchemaRowset` 接口。

- 提供程序必须支持通过 `IOpenRowset`（指定索引名称和相应的基表名称）在索引上打开行集。

- `Index` 对象必须支持其所有必需的接口：`IRowset`、`IRowsetIndex`、`IAccessor`、`IColumnsInfo`、`IRowsetInfo` 和 `IConvertTypes`。

- 针对索引基表打开的行集（通过 `IOpenRowset`）必须支持 `IRowsetLocate` 接口，以便根据书签在行上进行定位。

如果 OLE DB 提供程序满足上述要求，则用户可以设置 `Index As Access Path` 提供程序选项，使 SQL Server 能够使用提供程序的索引来评估查询。 默认情况下，除非设置了此选项，否则 SQL Server 不会尝试使用提供程序的索引。

>[!NOTE]
>SQL Server 支持影响 SQL Server 访问 OLE DB 提供程序的方式的各种选项。 可使用 SQL Server 企业管理器中的 `Linked Server Properties` 对话框设置这些选项。

#### <a name="simple-table-providers"></a>简单表提供程序

这些提供程序通过 `IOpenRowset` 接口向基表公开行集的开头。 这些提供程序既不是 SQL 命令提供程序也不是索引提供程序；而是 SQL Server 分布式查询可以使用的最简单的提供程序类。

对于此类提供程序，SQL Server 在分布式查询评估期间只能执行表扫描。

#### <a name="non-sql-command-providers"></a>非 SQL 命令提供程序

支持 `Command` 对象及其强制性接口，但不支持 SQL Server 可识别的 SQL 标准方言的提供程序属于此类别。

非 SQL 命令提供程序的两个示例是索引服务的 Microsoft OLE DB 提供程序和 [Microsoft Active Directory 域服务的 Microsoft OLE DB 提供程序](../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-active-directory-service.md)。

### <a name="transact-sql-subset"></a>Transact-SQL 子集

如果提供程序支持所需的 OLE DB 接口，则分布式查询支持以下各类 Transact-SQL 语句。

- 除以远程表作为目标表的 `SELECT` INTO 语句外，允许使用所有 `SELECT` 语句。

- 如果提供程序支持插入所需的接口，则允许对远程表使用 `INSERT` 语句。 有关 INSERT 的 OLE DB 要求的详细信息，请参阅本文后面的 \"INSERT 语句\"。

- 如果提供程序符合指定表的 OLE DB 接口要求，则允许对远程表使用 `UPDATE` 和 DELETE 语句。 有关可以更新或删除远程表的 OLE DB 接口要求和条件，请参阅本文后面的 \"UPDATE 和 DELETE 语句\"。

### <a name="cursor-support"></a>游标支持

如果提供程序支持必需的 OLE DB 功能，则分布式查询支持快照和键集游标。 分布式查询不支持动态游标。 用户对分布式查询的动态游标请求被降级为键集游标请求。

快照游标在游标打开时进行填充，结果集保持不变；基础表的更新、插入和删除操作不会反映在游标中。

键集游标在游标打开时进行填充，结果集在游标的整个生命周期内保持不变。 但是，在访问行时，游标中会显示对基础表的更新和删除操作。 对可能影响游标成员身份的基础表的插入不可见。

如果提供程序符合对某个远程表进行更新和删除的条件，例如，表 `UPDATE` \| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`，则可通过在分布式查询中定义并引用远程表的游标对该远程表进行更新或删除。 有关详细信息，请参阅本文后面的 \"UPDATE 和 DELETE 语句\"。

#### <a name="keyset-cursor-support-requirements"></a>键集游标支持要求

如果满足所有 Transact-SQL 语法要求并且存在以下任何一项，则分布式查询支持键集游标：

- OLE DB 提供程序支持查询中所有远程表上的可重用书签。 可重用书签可从给定表的某行集使用，并可用于同一个表中的不同行集。 通过将 BOOKMARK_DURABILITY 列设置为 BMK_DURABILITY_INTRANSACTION 或更高的持续性，可以通过 `IDBSchemaRowset` 的 TABLES_INFO 架构行集来指示对可重用书签的支持。

- 所有远程表都通过 `IDBSchemaRowset` 接口的 INDEXES 行集公开唯一键。 应有一个索引条目，其 UNIQUE 列设置为 VARIANT_TRUE。

涉及 OpenQuery 函数的分布式查询不支持键集游标。

#### <a name="updatable-keyset-cursor-requirements"></a>可更新的键集游标要求

可以通过在分布式查询上定义的键集游标更新或删除远程表，例如 `UPDATE` \| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`。 下面是允许使用分布式查询中的可更新游标的条件：

- 如果提供程序也满足更新和删除远程表的条件，则允许使用可更新游标。 有关详细信息，请参阅本文后面的 \"UPDATE 和 DELETE 语句\"。

- 所有可更新键集游标操作必须位于具有读取可重复隔离级别或更高隔离级别的用户定义事务中。 此外，提供程序必须通过 `ITransactionJoin` 接口支持分布式事务。

## <a name="ole-db-provider-interaction-phases"></a>OLE DB 提供程序交互阶段

 所有分布式查询执行方案都有以下六个操作：

- 连接建立和属性检索操作，指示 SQL Server 与 OLE DB 提供程序的连接方式以及使用的提供程序属性。

- 表名解析和元数据检索操作，指示 SQL Server 如何将远程表名称（通过以下两种方式之一进行指定：基于链接服务器的名称或临时名称）解析为提供程序中的相应数据对象。 这还包括 SQL Server 从提供程序检索的表元数据，以便编译和优化分布式查询。

- 事务管理操作，指定与 OLE DB 提供程序的所有与事务相关的交互。

- 数据类型处理操作，指示 SQL Server 在处理分布式查询时使用 OLE DB 提供程序中的数据或将数据导出到 OLE DB 提供程序时，处理 OLE DB 数据类型的方式。

- 错误处理操作，指示 SQL Server 使用提供程序中的扩展错误信息的方式。

- 安全操作，指示 SQL Server 安全性与提供程序安全性的交互方式。

### <a name="connection-establishment-and-property-retrieval"></a>连接建立和属性检索

SQL Server 支持两种远程数据对象命名约定：基于链接服务器的四部分名称和使用 `OPENROWSET` 函数的临时名称。

#### <a name="linked-server-based-names"></a>基于链接服务器的名称

链接服务器作为对 OLE DB 数据源的抽象。 基于链接服务器的名称是 `<linked-server>.<catalog>` 形式的四部分名称。 `<schema>.<object>`，其中 `<linked-server>` 是链接服务器的名称。 SQL Server 解释 `<linked-server>` 以派生 OLE DB 提供程序以及用于标识提供程序的数据源的连接属性。 名称的其他三个部分由 OLE DB 数据源解释以标识特定的远程表。 :::

#### <a name="ad-hoc-names"></a>临时名称

临时名称是基于 `OPENROWSET` 或 `OPENDATASOURCE` 函数的名称。 它包括在分布式查询中每次引用远程表时的所有连接信息（即，要使用的 OLE DB 提供程序、标识数据源所需的属性、用户 ID 和密码）。

除 sysadmin 角色的成员外，默认情况下不允许使用临时名称。 为了对 OLE DB 提供程序使用临时名称，应将提供程序选项 `DisallowAdhocAccess` 设置为 `0`。

如果使用链接服务器名称，SQL Server 会从链接服务器定义中提取 OLE DB 提供程序名称和提供程序的初始化属性。 如果使用临时名称，SQL Server 会从 `OPENROWSET` 函数的参数中提取相同的信息。

如需深入了解如何设置使用四部分名称和基于临时名称的语法的链接服务器，请参阅[创建链接服务器](create-linked-servers-sql-server-database-engine.md)。

### <a name="connecting-to-an-ole-db-provider"></a>连接到 OLE DB 提供程序

以下是 SQL Server 连接到 OLE DB 提供程序时执行的高级步骤：

1. SQL Server 创建数据源对象。

   SQL Server 使用提供程序的 `ProgID` 来实例化其数据源对象 (DSO)。 ProgID 被指定为链接服务器配置的 `provider_name` 参数，或临时名称的 `OPENROWSET` 函数的第一个参数。

   SQL Server 通过 OLE DB 服务组件接口 `IDataInitialize` 来实例化提供程序的 DSO。 这允许服务组件管理器将其服务（例如滚动和更新支持）聚合到提供程序的本机功能之上。 此外，通过 `IDataInitialize` 实例化提供程序可使 OLE DB 服务组件对提供程序使用池连接，从而减少一些连接和初始化开销。

   可将给定提供程序配置为在与 SQL Server 相同的进程中或在其自己的进程中进行实例化。 在单独的进程中进行实例化可以保护 SQL Server 进程免受提供程序中的故障的影响。 同时，从 SQL Server 进程外封送的 OLE DB 调用会带来性能开销。 通过设置 `Allow In Process` 提供程序选项，可以将提供程序配置为在进程内或进程外进行实例化。 有关详细信息，请参阅[设置提供程序选项](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md)。

   要了解有关 OLE DB 服务组件和会话池的详细信息，请参阅 OLE DB 文档以了解提供程序要求。

2. 数据源已初始化。

   创建 DSO 后，如果服务器配置选项 `remote login timeout` 大于 0，则 `IDBProperties` 接口将设置 DBPROP_INIT_TIMEOUT 初始化属性；这是必需的属性。

   如果在链接服务器定义中或在 `OPENROWSET` 函数的第二个参数中指定或隐含这些属性，则会设置这些属性：

   - `DBPROP_INIT_PROVIDERSTRING`

   - `DBPROP_INIT_DATASOURCE`

   - `DBPROP_INIT_LOCATION`

   - `DBPROP_INIT_CATALOG`

   - `DBPROP_AUTH_USERID`

   - `DBPROP_AUTH_PASSWORD`

   设置这些属性后，将调用 `IDBInitialize::Initialize` 以使用指定的属性初始化 DSO。

3. SQL Server 收集特定于提供程序的信息。

   SQL Server 收集要在分布式查询评估中使用的几个提供程序属性；通过调用 `IDBProperties::GetProperties` 来检索这些属性。 所有这些属性都是可选的；但是，支持所有相关属性可使 SQL Server 能够充分利用提供程序的功能。 例如，需要 `DBPROP_SQLSUPPORT` 来确定 SQL Server 是否可以向提供程序发送查询。 如果不支持此属性，则即使远程提供程序是 SQL 命令提供程序，SQL Server 也不会将其用作 SQL 命令提供程序。 在下表中，“默认值”列指示 SQL Server 在提供程序不支持该属性时假定的值。

Property| 默认值| 使用 |
|:----|:----|:----|
|`DBPROP_DBMSNAME`|无|用于错误消息。|
|`DBPROP_DBMSVER` |无|用于错误消息。|
|`DBPROP_PROVIDERNAME`|无|用于错误消息。|
|`DBPROP_PROVIDEROLEDBVER1`|1.5|用于确定 2.0 功能的可用性。
|`DBPROP_CONCATNULLBEHAVIOR`|无|用于确定提供程序的 `NULL` 串联行为是否与 SQL Server 的串联行为相同。|
|`DBPROP_NULLCOLLATION`|无|仅当 `NULLCOLLATION` 与 SQL Server 实例 null 排序规则行为匹配时，才允许使用排序/索引。|
|`DBPROP_OLEOBJECTS`|无|确定提供程序是否支持用于大型数据对象列的结构化存储接口。|
|`DBPROP_STRUCTUREDSTORAGE`|无|确定大型对象类型（`ILockBytes`、`Istream` 和 `ISequentialStream`）支持的结构化存储接口。|
|`DBPROP_MULTIPLESTORAGEOBJECTS`|错误|确定是否可以同时打开多个大型对象列。|
|`DBPROP_SQLSUPPORT`|无|确定是否可以向提供程序发送 SQL 查询。|
|`DBPROP_CATALOGLOCATION`|`DBPROPVAL_CL_START`|用于构造多部分表名。
|`SQLPROP_DYNAMICSQL`|错误|SQL Server 特定的属性：如果返回 `VARIANT_TRUE`，则表示参数化查询执行支持 `?` 参数标记。
|`SQLPROP_NESTEDQUERIES`|错误|SQL Server 特定的属性：如果返回 `VARIANT_TRUE`，则表示提供程序支持 `FROM` 子句中的嵌套 `SELECT` 语句。
|`SQLPROP_GROUPBY`|错误|SQL Server 特定的属性：如果返回 `VARIANT_TRUE`，则表示提供程序支持 SQL-92 标准中指定的 `SELECT` 语句中的 GROUP BY 子句。
|`SQLPROP_DATELITERALS `|错误|SQL Server 特定的属性：如果返回 `VARIANT_TRUE`，则表示提供程序支持按 SQL Server Transact-SQL 语法的日期时间文本。
|`SQLPROP_ANSILIKE `|错误|SQL Server 特定的属性：对于支持 SQL-Minimum 级别的提供程序而言，此属性很有用，并且它根据 SQL-92 入门级别支持 `LIKE` 运算符（\'%\' 和 \'_\' 作为通配符）。
|`SQLPROP_SUBQUERIES `|错误|SQL Server 属性：此属性在支持 SQL-Minimum 级别的提供程序中很有用。 此属性指示提供程序支持 SQL-92 入门级别指定的子查询。 这包括支持相关子查询的 `SELECT` 列表和 `WHERE` 子句中的子查询以及 `IN`、`EXISTS`、`ALL` 和 `ANY` 运算符。
|`SQLPROP_INNERJOIN`|错误|SQL Server 特定的属性：此属性对于支持 SQL-Minimum 级别的提供程序很有用。 此属性表示支持使用 `FROM` 子句中的多个表进行联接。 ------ ---

以下是从 `IDBInfo::GetLiteralInfo` 中检索的三种文本：`DBLITERAL_CATALOG_SEPARATOR`、`DBLITERAL_SCHEMA_SEPARATOR`（根据对象的目录、架构和对象名称部分构造完整的对象名称）和 `DBLITERAL_QUOTE`（在发送给提供程序的 SQL 查询中引用标识符名称）。

如果提供程序不支持分隔符文本，则 SQL Server 使用句点 (.) 作为默认分隔符。 如果提供程序仅支持目录分隔符但不支持架构分隔符，则 SQL Server 也会将目录分隔符用作架构分隔符。 如果提供程序不支持 `DBLITERAL_QUOTE`，则 SQL Server 使用单引号 (`'`) 作为引用字符。

>[!NOTE]
>如果提供程序的名称分隔符文本与这些默认值不匹配，则提供程序必须通过 `IDBInfo` 公开它们，以便 SQL Server 通过四部分名称访问其表。 如果未公开这些文本，则只能针对此类提供程序使用传递查询。

有关如何公开 `SQLPROP_DYNAMICSQL` 和 `SQLPROP_NESTEDQUERIES` 属性的信息，请参阅 [SQL Server 特定属性](#appendixc)。

### <a name="table-name-resolution-and-meta-data-retrieval"></a>表名解析和元数据检索

SQL Server 将分布式查询中的给定远程表名称解析为 OLE DB 数据源中的特定表或视图。 基于链接服务器的命名方案和临时命名方案均会生成一个三部分名称，该名称由提供程序进行解释。 对于基于链接服务器的名称，四部分名称的最后三部分构成目录、架构和对象名称。 对于临时名称，`OPENROWSET` 函数的第三个参数指定一个由三部分组成的名称，用于描述目录、架构和对象名称。 目录名称和架构名称其中之一可以为空或两者均可为空。 （具有空目录名称和架构名称的四部分名称将类似于 `<server-name>...<object-name>`。）在这种情况下，SQL Server 使用 `NULL` 作为在架构行集表中查找的相应值。

SQL Server 使用的名称解析规则和元数据检索步骤取决于提供程序是否支持 `Session` 对象上的 `IDBSchemaRowset` 接口。

如果支持 `IDBSchemaRowset`，则从 `IDBSchemaRowset` 接口使用 `TABLES`、`COLUMNS`、`INDEXES` 和 `TABLES_INFO` 架构行集。 （`TABLES_INFO` 架构行集在 OLE DB 2.0 中进行定义。）SQL Server 限制 `IDBSchemaRowset` 接口返回的架构行集，以查找与指定的远程表名称部分匹配的架构行。 以下是与提供程序支持的对架构行集的限制相关的规则，以及 SQL Server 如何使用它们来检索远程表的元数据：

- 始终需要对 `TABLE_NAME` 和 `COLUMN_NAME` 列的限制。

- 如果提供程序支持对 `TABLE_CATALOG`（或 `TABLE_SCHEMA`）的限制，则 SQL Server 会对 `TABLE_CATALOG`（或 `TABLE_SCHEMA`）使用该限制。 如果远程表名称中未指定目录（或架构）名称，则使用 `NULL` 值作为相应的限制值。 如果已指定目录（或架构）名称，则提供程序必须支持对 `TABLE_CATALOG`（或 `TABLE_SCHEMA`）的相应限制。

- 提供程序必须支持对 `TABLES` 和 `COLUMNS` 中 `TABLE_SCHEMA` 列的限制，或者两者都不支持。 提供程序必须支持对 `TABLES` 和 `COLUMNS` 行集的目录名称限制，或者两者都不支持。

- 如果支持对 INDEXES 的任何限制，则提供程序必须支持对 `TABLES` 和 INDE`XES or support them on neither. The provider must either support catalog name restriction on both `TABLES` and `INDEXES` 行集的架构限制，或者两者都不支持。

SQL Server 根据上述规则设置限制，从 `TABLES` 架构行集中检索 `TABLE_CATALOG`、`TABLE_SCHEMA`、`TABLE_NAME`、`TABLE_TYPE`、`TABLE_GUID` 列。

SQL Server 从 `COLUMNS` 架构行集中检索 `TABLE_CATALOG`、`TABLE_SCHEMA`、`TABLE_NAME`、`COLUMN_NAME`、`COLUMN_GUID`、`ORDINAL_POSITION`、`COLUMN_FLAGS`、`IS_NULLABLE`、`DATA_TYPE`、`TYPE_GUID`、`CHARACTER_MAXIMUM_LENGTH`、`NUMERIC_PRECISION` 和 `NUMERIC_SCALE` 列。 `COLUMN_NAME`、`DATA_TYPE` 和 `ORDINAL_POSITION` 必须返回有效的非 null 值。 如果 `DATA_TYPE` 为 `DBTYPE_NUMERIC` 或 `DBTYPE_DECIMAL`，则相应的 `NUMERIC_PRECISION` 和 `NUMERIC_SCALE` 必须是有效的非 null 值。

SQL Server 按照先前的规则设置限制，从可选的 `INDEXES` 架构行集中查找指定远程表上的索引。 SQL Server 从找到的匹配索引条目中检索 `TABLE_CATALOG`、`TABLE_SCHEMA`、`TABLE_NAME`、`INDEX_CATALOG`、`INDEX_SCHEMA`、`INDEX_NAME`、`PRIMARY_KEY`、`UNIQUE`、`CLUSTERED`、`FILL_FACTOR`、`ORDINAL_POSITION`、`COLUMN_NAME`、`COLLATION`、`CARDINALITY` 和 `PAGES` 列。

SQL Server 从可选的 `TABLES_INFO` 行集中查找有关指定远程表的其他信息，例如书签支持、书签类型和长度。 使用除 `TABLES_INFO` 行集的 `DESCRIPTION` 列之外的所有列。 `TABLES_INFO` 行集中信息的使用如下：

- `BOOKMARK_DURABILITY` 列用于实现更高效的键集游标。 如果此列的值为 `BMK_DURABILITY_INTRANSACTION` 或更高的持续性值，则 SQL Server 使用基于书签的检索和远程表行的更新来实现键集游标。

- `BOOKMARK_TYPE`、`BOOKMARK_DATA TYPE` 和 `BOOKMARK_MAXIMUM_LENGTH` 列用于在查询编译时确定书签元数据。 如果不支持这些列，SQL Server 会在编译期间通过 `IOpenRowset` 打开基表行集以获取书签信息。

如果不支持 `IDBSchemaRowset` 且远程表名称包含目录或架构名称，则 SQL Server 需要 `IDBSchemaRowset` 并返回一个错误。 但是，如果目录和架构名称均未提供，则 SQL Server 将打开与远程表对应的行集，并从行集对象的强制性 `IColumnsInfo` 接口检索列元数据。

SQL Server 通过调用 `IOpenRowset::OpenRowset` 打开与表对应的行集。 提供给 `OPENROWSET` 的表名是根据目录、架构和对象名称部分构造的。

- 每个名称部分（`catalog`、`schema`、`object name`）都使用提供程序的引号字符 (`DBLITERAL_QUOTE`) 括起来，然后使用 `DBLITERAL_CATALOG_SEPARATOR` 字符和它们之间嵌入的 `DBLITERAL_SCHEMA_SEPARATOR` 字符连接起来。 名称构造遵循 `IOpenRowset` 中的 OLE DB 规则。

- 打开行集对象后，通过 `IColumnsInfo::GetColumnInfo` 检索表的列元数据。

如果 TABLES、COLUMNS 和 TABLES_INFO 行集不支持 `IDBSchemaRowset`，则 SQL Server 会针对基表打开两次行集：一次在查询编译期间打开以检索元数据，一次在查询执行期间打开。 因打开行集（例如，运行可以改变实时设备状态的代码、发送电子邮件、运行任意用户提供的代码）而产生副作用的提供程序必须了解此行为。

### <a name="statistics-retrieval"></a>统计信息检索

如果提供程序支持基表上的分布统计信息，则 SQL Server 将使用这些统计信息。 对 SQL Server 查询处理器有用的统计信息有以下两种：

- **列（或元组）基数**。 这是表的列（或列的组合）中非重复值的数量。 可用于估计列中谓词的选择性。 支持分布统计信息的提供程序应至少支持一种类型的基数。

- **直方图**。 如果值的分布不均，则 非重复值的数量不足以准确地估计谓词的选择性。 在这种情况下，可以使用直方图提供有关表中列值分布的更细化信息。

统计信息的可用性使 SQL Server 查询优化器能够更好地估计查询中的中间操作的基数，从而为它们生成更好的执行计划。

OLE DB 提供程序应支持以下分布统计信息：

- **强制**。 支持属性 (1) `DBPROP_TABLESTATISTICS`（指示是否支持列或元组基数以及是否支持直方图）以及 (2) `DBPROP_OPENROWSETSUPPORT`（表示使用 `DBPROPVAL_ORS_HISTOGRAM` 位时是否支持直方图）。

- **强制**。 `TABLE_STATISTICS` 架构行集。 `TABLE_STATISTICS` 架构行集列出了给定数据库中可用的统计信息。 它还包括架构行集本身中的列和元组基数，并指示特定列是否支持直方图。 要使 SQL Server 使用统计信息，此架构行集中必须包含 `TABLE_NAME`、`STATISTICS_NAME`、`STATISTICS_TYPE`、`COLUMN_NAME` 和 `ORDINAL_POSITION` 列。 至少必须包含 `COLUMN_CARDINALITY` 或 `TUPLE_CARDINALITY` 中的一个。 如果支持直方图，则也必须包含 `NO_OF_RANGES`。

- 可选。 （可选）如果提供程序支持直方图，则它应支持 `IOpenRowset::OpenRowset` 增强方法，该方法允许通过指定相应统计信息的 `DBID` 来打开直方图行集。

有关统计信息接口的完整信息，请参阅 OLE DB 2.6 规范。

### <a name="constraints"></a>约束

如果 OLE DB 提供程序支持 OLE DB 2.6 架构行集 `CHECK_CONSTRAINTS_BY_TABLE`，则 SQL Server 查询优化器还会使用远程数据源中基表上定义的 `CHECK` 约束。 架构行集的 `CHECK_CLAUSE` 列应返回符合 SQL-92 语法的 `CHECK` 子句谓词。 查询优化器使用约束信息来消除或简化已知始终为 false 或始终为 true 的谓词，因为表上存在 CHECK 约束。

### <a name="transaction-management"></a>事务管理

SQL Server 使用提供程序的 `ITransactionLocal`（用于本地事务）和 `ITransactionJoin`（用于分布式事务）OLE DB 接口来对外部数据进行基于事务的访问。 SQL Server 通过对提供程序启动本地事务，保证了原子写入操作。 SQL Server 通过使用分布式事务，确保了涉及多个节点的事务在所有节点中具有相同的结果（提交或中止）。 如果提供程序不支持必需的 OLE DB 事务相关接口，则不允许根据本地事务上下文对该提供程序执行更新操作。

下表描述了在用户根据提供程序的功能和本地事务上下文执行分布式查询时会发生的情况。 对提供程序执行的读取操作是指 `SELECT` 语句或将远程表读入 `SELECT INTO`、`INSERT`、`UPDATE` 或 `DELETE` 语句的输入端。 对提供程序执行的写入操作是指以远程表作为目标表的 `INSERT`、`UPDATE` 或 `DELETE` 语句。

基于提供程序功能和事务上下文的分布式查询的结果：

|发生分布式查询|提供程序不支持 `ITransactionLocal`|提供程序支持 `ITransactionLocal`，但不支持 `ITransactionJoin`|提供程序同时支持 `ITransactionLocal` 和 `ITransactionJoin`|
|:-----|:-----|:-----|:-----|
| 在单独的事务中（无用户事务）。|默认情况下，仅允许读取操作。 如果启用了提供程序级别选项 `Nontransacted Updates`，则允许写入操作。 （启用此选项时，SQL Server 无法保证提供程序数据的原子性和一致性。 这可能会导致在远程数据源中反映出写入操作的部分效果，而无法撤消它们。）| 允许对远程数据使用所有语句。 键集游标是只读的。 本地事务在具有当前 SQL Server 会话隔离级别的提供程序上启动，并在成功的语句计算结束时提交。 （除非使用 `SET TRANSACTION ISOLATION LEVEL` 语句修改 SQL Server 会话，否则其默认隔离级别为 `READ COMMITTED`。 提供程序必须支持请求的隔离级别。） | 允许使用所有语句。 键集游标是只读的。 本地事务在具有当前 SQL Server 会话隔离级别的提供程序上启动，并在成功的语句计算结束时提交。 |
| 在用户事务中（即，在 `BEGIN TRAN` 或 `BEGIN DISTRIBUTED TRAN` 和 `COMMIT` 之间）。 | 如果事务的隔离级别为 `READ COMMITTED`（默认值）或更低，则允许读取操作。 如果隔离级别较高，则不允许执行分布式查询。 | 仅允许读取操作。 使用当前 SQL Server 会话的隔离级别在提供程序上启动新的分布式事务。 |允许使用所有语句。 使用当前 SQL Server 会话的隔离级别在提供程序上启动新的分布式事务，并在用户事务提交时提交。 对于数据修改语句，默认情况下，SQL Server 会在分布式事务下启动嵌套事务，以便可以在某些错误条件下停止数据修改语句，而无需停止周围事务。 如果启用了 `XACT_ABORT SET` 选项，则 SQL Server 无需嵌套事务支持，并在数据修改语句中出现错误时停止周围事务。 |
|  |  |  |

### <a name="data-type-handling-in-distributed-queries"></a>分布式查询中的数据类型处理

OLE DB 提供程序根据 OLE DB 定义的数据类型（在 OLE DB 中由 `DBTYPE` 表示）公开其数据。 SQL Server 将服务器内部的外部数据作为本机 SQL Server 类型进行处理；这种处理方式可将 OLE DB 数据类型映射到 SQL Server 本机类型，反之亦然，因为 SQL Server 对数据的使用或导出是分别进行的。 除非另有说明，否则将隐式完成此映射。

分布式查询中的数据类型使用以下两种映射方法之一进行处理：

- 当远程表出现在 `SELECT` 语句中以及 INSERT、UPDATE 和 DELETE 语句的输入端时，使用端映射会将 OLE DB 数据类型中的类型映射到使用端的 SQL Server 本机数据类型。

- 当远程表显示为 `INSERT` 或 `UPDATE` 语句的目标表时，导出端映射会将 SQL Server 数据类型中的类型映射到导出端的 OLE DB 数据类型。

SQL Server 和 OLE-DB 数据类型映射表。

| OLE DB 类型 | `DBCOLUMNFLAG` | SQL Server 数据类型 |
|-----|-----|-----|
|`DBTYPE_I1`*| |`numeric(3, 0)`|
|`DBTYPE_I2`| |`smallint`|
|`DBTYPE_I4`| |`int`|
|`DBTYPE_I8`| |`numeric(19,0)`|
|`DBTYPE_UI1`| |`tinyint`|
|`DBTYPE_UI2`*| |`numeric(5,0)`|
|`DBTYPE_UI4`*| |`numeric(10,0)`|
|`DBTYPE_UI8`*| |`numeric(20,0)`|
|`DBTYPE_R4`| |`float`|
|`DBTYPE_R8`| |`real`|
|`DBTYPE_NUMERIC`| |`numeric`|
|`DBTYPE_DECIMAL`| |`decimal`|
|`DBTYPE_CY`| |`money`|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`<br>或<br> 最大长度 > 4000 个字符|ntext|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`|nchar|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|nvarchar|
|`DBTYPE_IDISPATCH`| |错误|
|`DBTYPE_ERROR`| |错误|
|`DBTYPE_BOOL`| |`bit`|
|`DBTYPE_VARIANT`*| |nvarchar|
|`DBTYPE_IUNKNOWN`| |错误|
|`DBTYPE_GUID`| |`uniqueidentifier`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISLONG=true` <br>或<br> 最大长度 > 8000|`image`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISROWVER=true`、`DBCOLUMNFLAGS_ISFIXEDLENGTH=true`、列宽 = 8 <br>或<br> 未报告最大长度。 | `timestamp` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `binary` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varbinary`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `char`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varchar` |
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISLONG=true` <br>或<br> 最大长度 > 8000 个字符 <br>或<br>   未报告最大长度。 | `text`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` |`nchar`|
|`DBTYPE_WSTR` | `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|`nvarchar`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISLONG=true` <br>或<br> 最大长度 > 4000 个字符 <br>或<br>   未报告最大长度。 | `ntext`|
|`DBTYPE_UDT`| |错误|
|`DBTYPE_DATE`* | | `datetime` |
|`DBTYPE_DBDATE` | | `datetime`（需要显式转换）|
|`DBTYPE_DBTIME`| | `datetime`（需要显式转换）|
|`DBTYPE_DBTIMESTAMP`* | | `datetime`|
|`DBTYPE_ARRAY` | |错误|
|`DBTYPE_BYREF` | | 忽略 |
|`DBTYPE_VECTOR` | |错误|
|`DBTYPE_RESERVED`| |错误|

\* 指示对 SQL Server 类型表示的某种形式的转换，因为 SQL Server 中没有完全等效的数据类型。 此类转换可能会导致精度下降、溢出或下溢。 如果 SQL Server 的未来版本支持相应的数据类型，则将来可以更改默认的隐式映射。

>[!NOTE]
>`numeric(p,s)` 指示 SQL Server 数据类型 `numeric` 的精度为 `p`，小数位数为 `s`。 `DBTYPE_NUMERIC` 和 `DBTYPE_DECIMAL` 的最大允许精度为 38。 在创建访问器时，提供程序必须支持以 `DBTYPE_WSTR` 的形式绑定到 `DBTYPE_BSTR` 列。 `DBTYPE_VARIANT` 列用作 Unicode 字符串 `nvarchar`。 这需要支持从 `DBTYPE_VARIANT` 到提供程序中的 `DBTYPE_WSTR` 的转换。 希望提供程序按照 OLE DB 中的定义来实现此转换。 有关详细信息，请参阅 [OLE DB 的数据类型规范](#appendixa)。

#### <a name="interpreting-data-type-mapping"></a>解释数据类型映射

到 SQL Server 类型的映射由 OLE DB 数据类型和描述列或标量值的 DBCOLUMNFLAGS 值确定。 对于 `COLUMNS` 架构行集，`DATA_TYPE` 和 `COLUMN_FLAGS` 列表示这些值。 对于 `IColumnsInfo::GetColumnInfo` 接口，`DBCOLUMNINFO` 结构的 `wType` 和 `dwFlags` 成员表示此信息。

要对具有特定 `DBTYPE` 和 `DBCOLUMNFLAG` 值的给定列应用使用端映射，请在表中查找相应的 SQL Server 类型。 表达式中远程表的列的类型规则可以通过以下简单规则进行描述：

>如果表中相应的映射 SQL Server 类型在 Transact-SQL 表达式中是合法的，则给定的远程列值在同一上下文中也是合法的。

表和规则定义：

- 比较和表达式。

通常，如果 `<op>` 是 `X` 数据类型和 `<remote-column>` 映射到的数据类型上的有效运算符，则 `X <op> <remote-column>` 是有效的表达式。

- 显式转换。

如果 `<remote-column>` 的 `DBTYPE` 映射到本机数据类型 `Y`（根据上表）并且允许从 `Y` 到 `X` 的显式转换，则允许 `Convert(X, <remote-column>)`。

如果用户希望将远程数据转换为非默认的本机数据类型，则必须使用显式转换。

对于针对远程表的 `UPDATE` 和 `INSERT` 语句，若要使用导出端映射，请使用同一个表将本机 SQL Server 数据类型映射到 OLE DB 数据类型。 如果存在以下任一情况，则允许从 SQL Server 类型 `S1` 映射到给定的 OLE DB 类型 `T`：

- 可以直接在映射表中找到相应的映射。

- 允许将 `S1` 隐式转换为另一种 SQL Server 类型 `S2`，以便 `S2` 可以映射到映射表中的类型 `T`。

#### <a name="large-object-lob-handling"></a>大型对象 (LOB) 处理

如映射表中所示，如果类型为 `DBTYPE_STR`、`DBTYPE_WSTR` 或 `DBTYPE_BSTR` 的列也报告 `DBCOLUMNFLAGS_ISLONG`，或者它们的最大长度超过 4,000 个字符（或者未报告最大长度），则 SQL Server 会根据需要将它们视为 `text` 或 `ntext` 列。 同样，对于 `DBTYPE_BYTES` 列，如果设置了 `DBCOLUMNFLAGS_ISLONG` 或者最大长度超过 8,000 个字节（或者未报告最大长度），则将该列视为 `image` 列。 `Text`、`ntext` 和 `image` 列称为 LOB 列。

SQL Server 不会从 OLE DB 提供程序公开 LOB 上的完整文本和图像功能。 OLE DB 提供程序中的大型对象不支持 `TEXTPTRS`；因此，不支持任何相关功能，例如，`TEXTPTR` 系统功能以及 `READTEXT`、`WRITETEXT` 和 `UPDATETEXT` 语句。 支持用于检索整个 LOB 列的 `SELECT` 语句，以及远程表中整个大型对象列的 `UPDATE` 和 `INSERT` 语句。

如果提供程序支持结构化存储接口，则 SQL Server 将在 LOB 列上使用它们。 按优先级和功能递增顺序列出的结构化存储接口如下：`ISequentialStream`、`Istream` 或 `ILockBytes`。 如果支持这些接口中的一个或多个接口，则在通过 `IDBProperties` 接口进行查询时，提供程序必须返回 DBPROPVAL_OO_BLOB 作为 `DBPROP_OLEOBJECTS` 属性的值。 此外，提供程序应在 `DBPROP_STRUCTUREDSTORAGE` 属性中指示对其支持的接口的支持。

如果提供程序不支持 LOB 列上的任何结构化存储接口，则 SQL Server 会自行实现此接口，并仍将它们公开为 `text`、`ntext` 或`image` 列。

#### <a name="accessing-lob-columns"></a>访问 LOB 列

如果提供程序支持任一结构化存储接口，则 SQL Server 会执行以下步骤以在查询执行期间检索 LOB 列：

1. 在通过 `IOpenRowset::OpenRowset` 打开行集之前，SQL Server 会先请求对大型对象列上一个或多个结构化存储接口（`ISequentialStream``Istream` 和 `ILockBytes`）的支持。 提供程序支持的第一个接口是必需的；其他接口要求\"若价廉则设置\"，方法是将相应 DBPROP 结构的 dwOptions 元素设置为 DBPROPOPTIONS_SETIFCHEAP。 例如，如果提供程序同时支持 `ISequentialStream` 和 `ILockBytes`，则 `ISequentialStream` 是必需的，`ILockBytes` 要求\"若价廉则设置\"。

4. 打开行集后，SQL Server 使用 `IRowsetInfo::GetProperties` 来标识行集中可用的实际接口。 使用提供程序返回的最后一个或最优的接口。 当 SQL Server 针对大型对象列创建访问器时，该列绑定为 DBTYPE_IUNKNOWN，其中绑定集中 DBOBJECT 结构的 iid 元素绑定到接口。

#### <a name="reading-from-lob-columns"></a>读取 LOB 列

使用请求的结构化存储接口的接口指针（返回自 `IRowset::GetData` 的行缓冲区）读取大型对象列。 如果提供程序不支持同时打开多个 LOB（即，它不支持 `DBPROP_MULTIPLE_STORAGEOBJECTS`）并且该行有多个大型对象列，则 SQL Server 会将 LOB 列复制到本地工作表中。

#### <a name="update-and-insert-statements-on-lob-columns"></a>LOB 列上的 `UPDATE` 和 `INSERT` 语句

SQL Server 将指向新存储对象的指针传递给提供程序，而不是使用提供程序提供的接口来修改存储对象。 对于每个 LOB 列，使用所选的结构化存储接口创建更新或插入到存储对象上的值。 指向存储对象的指针可分别通过 `IRowsetChange::SetData` 或 `IRowsetChange::InsertRow` 传递给提供程序，具体取决于它是 `UPDATE` 还是 `INSERT` 操作。

### <a name="error-handling"></a>错误处理

当针对 OLE DB 提供程序的特定方法调用返回错误代码时，SQL Server 会在向用户返回有关错误条件的信息之前查找提供程序的扩展错误信息。

SQL Server 使用 OLE DB 指定的 OLE DB 错误对象。 其中一些高级步骤如下：

1. 当方法调用从提供程序返回错误代码时，SQL Server 将查找 `ISupportErrorInfo` 接口。 如果支持此接口，则 SQL Server 将调用 `ISupportErrorInfo::InterfaceSupportsErrorInfo`，以验证生成错误代码的接口是否支持错误对象。

<!-- -->

5. 如果接口支持错误对象，则 SQL Server 将调用 `GetErrorInfo` 函数，以获取当前错误对象上的 `IErrorInfo` 接口指针。

6. SQL Server 使用 `IErrorInfo` 接口来获取指向 `IErrorRecords` 接口的指针。

7. SQL Server 使用 `IErrorRecords` 来循环访问对象中的所有错误记录，并获取与每条记录对应的错误消息文本。

有关如何使用提供程序的错误对象的详细信息，请参阅 OLE DB 文档。

### <a name="security"></a>安全性

当使用者连接到 OLE DB 提供程序时，除非使用者希望作为集成安全用户进行身份验证，否则提供程序通常需要用户 ID 和密码。 对于分布式查询，SQL Server 代表执行分布式查询的 SQL Server 登录来充当 OLE DB 提供程序的使用者。 SQL Server 将当前的 SQL Server 登录映射到链接服务器上的用户 ID 和密码。

这些映射可以由用户为给定的链接服务器指定，并且可以由系统存储过程 `sp_addlinkedsrvlogin` 和 `sp_droplinkedsrvlogin` 设置和管理。 如果通过 `IDBProperties::SetProperties` 设置初始化组属性 DBPROP_AUTH_USERID 和 DBPROP_AUTH_PASSWORD，则由映射确定的用户 ID 和密码可在连接建立期间传递给提供程序。

当客户端通过 Windows 身份验证连接到 SQL Server 时，如果登录使用 `sp_addlinkedsrvlogin` 设置了 `self` 映射，则 SQL Server 会尝试模拟客户端的安全上下文，并在连接建立过程中在提供程序上设置 `DBPROP_AUTH_INTEGRATED` 属性。 此过程称为“委派”。

确定用于连接的安全上下文后，此安全上下文的身份验证以及其针对数据源中的数据对象的权限检查完全取决于 OLE DB 提供程序。

有关详细信息，请参阅 [`sp_addlinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 和 [`sp_droplinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)。

## <a name="query-execution-scenarios"></a>查询执行方案

 评估分布式查询时，SQL Server 在以下一个或多个方案中与 OLE DB 提供程序进行交互：

- 远程查询

- 索引访问

- 纯表扫描

- `UPDATE` 和 DELETE 语句

- `INSERT` 语句

- 传递查询

### <a name="remote-query"></a>Remote Query

SQL Server 生成一个 SQL 查询，该查询评估可由提供程序进行整体评估的原始查询的一部分。 此方案仅适用于 SQL 命令提供程序。 SQL Server 通过生成 SQL 查询将操作推送到提供程序的程度，取决于提供程序支持的 SQL 语法。 提供程序应通过以下方式指示其 SQL 支持级别：

1. 通过 `DBPROP_SQLSUPPORT` 属性指示 SQL Minimum、ODBC Core 或 SQL-92 入门级别的支持。 SQL Minimum 语法级别是 SQL Server 支持的一个新级别，它允许 SQL Server 向支持简单 SQL 子集的简单提供程序发送远程查询。 此级别包含一个基本的 `SELECT` 语句，该语句不包含子查询、`FROM` 子句中的多个表（因此没有联接）和 GROUP BY。 有关 SQL Server 用于针对各语法级别的提供程序生成远程查询的 SQL 语法子集，请参阅[用于生成远程查询的 SQL 子集](#appendixb)。

1. 通过支持各种 SQL Server 特定属性来指示对 DBPROP_SQLSUPPORT 报告的语法级别中未包含的单个 SQL 功能的支持。 本部分稍后将介绍属性列表以及 SQL Server 对这些属性的使用方式。

SQL Server 使用参数化查询执行，以问号 (?) 作为 Transact-SQL 字符串中的参数标记。 参数化查询执行用于 SQL Server、Microsoft Jet 和 Oracle OLE DB 提供程序。 对于其他提供程序，如果提供程序支持 `Command` 对象上的 `ICommandWithParameters` 并且至少满足以下条件之一，则使用参数化查询执行：

- 提供程序通过 `DBPROP_SQLSUPPORT` 属性指示 SQL Server 的 ODBC Core 级别的支持。

- 提供程序通过 `IDBPProperties` 支持 SQLPROP_DYNCMICSQL SQL Server 特定属性来指示对问号 (?) 参数标记的支持。 有关详细信息，请参阅有关提供程序属性的下一部分。

- 管理员在提供程序上设置 `Dynamic Parameters` 提供程序选项，使 SQL Server 生成参数化查询。

当 SQL Server 生成要远程执行的 SQL 文本时，将使用提供程序的引号字符将表名和列名括起来，如通过 `IDBInfo` 接口的 `DBLITERAL_QUOTE` 文本报告的那样。 如果不支持此文本，则表名和列名将不会被括起来。

如果提供程序支持参数化查询执行，则 SQL Server 会考虑使用参数化查询执行策略来评估远程表与本地表的联接。 对于从本地表的各行生成的参数值，需要重复执行参数化查询。 此策略减少了从提供程序检索的行数，并且当具有少量行的本地表与具有大量行的远程表联接时，此策略非常有用。 此远程联接策略可以通过 `REMOTE` 联接优化器提示强制执行。 有关参数化查询的详细信息，请参阅[如何：执行参数化查询](../../connect/php/how-to-perform-parameterized-queries.md)。

以下是远程查询方案中针对提供程序的较高级别的步骤。

1. SQL Server 使用 `IDBCreateCommand::CreateCommand` 从 `Session` 对象创建 `Command` 对象。

9. 如果 `Remote Query Timeout` 服务器配置选项设置为大于 0 的值，则 SQL Server 会使用 `ICommandProperties::SetProperties` 将 `Command` 对象上的 `DBPROP_COMMANDTIMEOUT` 属性设置为相同的值；必须调用 `ICommand::SetCommandText` 才能将命令文本设置为生成的 Transact-SQL 字符串。

10. SQL Server 调用 `ICommandPrepare::Prepare` 来准备命令。 如果提供程序不支持此接口，则 SQL Server 将继续执行步骤 4。

11. 如果生成的查询是参数化的，则 SQL Server 使用 `ICommandWithParameters::SetParameterInfo` 来描述参数，使用 `IAccessor::CreateAccessor` 来创建参数的访问器。

12. SQL Server 调用 `ICommand::Execute` 来执行命令并创建行集。

13. SQL Server 使用 `IRowset` 接口来导航和使用表中的行。 使用 `IRowset::GetNextRows` 来提取行，使用 `IRowset::RestartPosition` 来重新定位到行集的开头，使用 `IRowset::ReleaseRows` 来释放行。

#### <a name="provider-properties-of-interest-for-remote-query-execution"></a>远程查询执行相关的提供程序属性

如果提供程序支持 DBPROP_SQLSUPPORT 中报告的语法级别未涵盖的 SQL 功能，则可以使用各种提供程序的特定属性来指示它们。

- SQLPROP_GROUPBY。 此属性对支持 SQL-Minimum 级别的提供程序很有用。 此属性指示提供程序支持 `SELECT` 语句中的 GROUP BY 和 HAVING 子句。 此外，此属性还指示提供程序支持以下五种聚合函数：MIN、MAX、SUM、COUNT 和 AVG。 提供程序可能不支持对这些聚合函数的参数使用 DISTINCT。

- SQLPROP_SUBQUERIES。 此属性在支持 SQL-Minimum 级别的提供程序中很有用。 它指示提供程序支持 SQL-92 入门级别指定的子查询。 这包括支持相关子查询的 `SELECT` 列表和 `WHERE` 子句中的子查询以及 `IN`、`EXISTS`、`ALL` 和 `ANY` 运算符。

- SQLPROP_DATELITERALS。 此属性对任何提供程序（包括支持 SQL-92 入门级别的提供程序）都很有用。 对日期时间文本的标准文本语法的支持不属于 SQL-92 入门级别。 此 SQL Server 的特定属性指示提供程序支持 SQL-92 标准指定的日期时间文本语法。

- SQLPROP_ANSILIKE。 对支持 SQL-Minimum 级别的提供程序很有用。 此属性指示提供程序根据 SQL-92 入门级别支持 `LIKE` 运算符（\'%\' 和 \'_\' 为通配符）。 这将可用于支持 SQL-Minimum 级别的提供程序，因为 SQL-Minimum 级别不包含 `LIKE` 支持。

- SQLPROP_INNERJOIN。 此属性对于支持 SQL-Minimum 级别的提供程序很有用。 它指示支持 `FROM` 子句中的多个表。 这将可用于仅支持 SQL-Minimum 级别的提供程序，因为 SQL-Minimum 级别不包含对联接的支持。 这并未指示支持显式 JOIN 关键字，也未指示支持 OUTER 联接。 它指示通过 `FROM` 子句中的表列表仅支持隐式联接。

- SQLPROP_DYNAMICSQL。 指示支持将 `?` 作为参数标记。 应在 `WHERE` 子句或 `SELECT` 列表中的标量项处支持参数标记。 支持 `?` 参数标记，使 SQL Server 可以向提供程序发送参数化查询。

- SQLPROP_NESTEDQUERIES。 指示支持 `FROM` 子句中嵌套的 SELECT（例如，`SELECT * FROM (SELECT * FROM T))`。 在许多情况下，SQL Server 在查询生成要远程执行的查询字符串后，会在其 `FROM` 子句中使用嵌套的 `SELECT` 语句。 由于 SQL-92 入门级别不需要对嵌套的 `SELECT` 的支持，因此除非提供程序也设置此属性，否则 SQL Server 不会将具有嵌套的 `SELECT` 语句的查询委托给提供程序。 此外，管理员还可以为提供程序设置 `Nested Queries` 提供程序选项，使 SQL Server 针对提供程序生成嵌套查询。

提供程序可以使用名为 `SQLPROPSET_OPTHINTS` 的 SQL Server 特定属性集来支持这些属性，并定义 `PROPID` 值。 属性集 `SQLPROPSET_OPTHINTS` 和两个属性是使用以下常数定义的：

```
extern const GUID `SQLPROPSET_OPTHINTS` = { 0x2344480c, 0x33a7, 0x11d1, { 0x9b, 0x1a, 0x0, 0x60, 0x8, 0x26, 0x8b, 0x9e } };
enum SQLPROPERTIES {
SQLPROP_NESTEDQUERIES = 0x4,
SQLPROP_DYNAMICSQL = 0x5,
SQLPROP_GROUPBY = 0x6,
SQLPROP_DATELITERALS = 0x7,
SQLPROP_ANSILIKE = 0x8,
SQLPROP_INNERJOIN = 0x9,
SQLPROP_SUBQUERIES = 0x10
};
```

#### <a name="character-set-and-sort-order-implications"></a>字符集和排序顺序的含义

SQL Server 支持在各列级别为字符数据指定排序规则。 排序规则包括非 Unicode 字符数据（`char` 和 `varchar` 列）的字符集和排序顺序规范。 对于 Unicode 数据（`nchar` 和 `nvarchar`列），排序规则仅指定排序顺序。

仅当链接服务器与本地服务器使用相同的字符集（针对非 Unicode 数据）、排序顺序和字符串比较语义时，SQL Server 才会将字符串比较委托给提供程序。

对于 SQL Server 链接服务器，SQL Server 会自动确定排序规则兼容性。 对于其他提供程序，管理员必须向 SQL Server 指示来自给定链接服务器的字符数据的排序规则。 在 SQL Server 中，支持名为 `Collation Name` 的新链接服务器选项。 如果管理员确定链接服务器采用的排序规则语义与 SQL Server 标准排序规则之一相同，则可以将 `Collation Name` 选项设置为该排序规则名称。 可以使用 `sp_serveroption` 系统存储过程设置 `Collation Name` 选项。 仅当以下两个条件均满足时，才应设置此选项：

- 远程排序顺序和字符集与指定的 SQL Server 排序规则相同。

- OLE DB 提供程序使用的字符串比较语义遵循 SQL-92 标准规范的字符串比较语义，或者等效于 SQL Server 的比较语义。

出于后向兼容性原因，SQL Server 7.0 中支持的“排序规则兼容”选项仍受支持。 将其设置为 true 等效于将“排序规则名称”选项设置为 SQL Server master 数据库的默认排序规则。 新应用程序应使用“排序规则名称”选项而不是“排序规则兼容”选项。

### <a name="indexed-access"></a>索引访问

SQL Server 使用提供程序公开的索引来评估分布式查询的某些谓词。 此方案仅适用于索引提供程序以及用户设置了 `Index as Access Path` 提供程序选项时。 以下是 SQL Server 在使用索引执行查询时对提供程序执行的主要高级步骤：

1. 使用完整的表名和索引名，通过 `IOpenRowset::OpenRowset` 打开索引行集。 如前面的远程查询方案中所述，生成完整的表名和索引名。

1. 使用完整的表名，通过 `IOpenRowset::OpenRowset` 打开基表行集。

1. 根据查询谓词，通过 `IRowsetIndex::SetRange` 设置索引行集的范围。

1. 通过索引行集上的 `IRowset` 扫描索引行集中的行。

1. 使用检索到的索引行中的书签列，通过 `IRowsetLocate::GetRowsByBookmark` 提取基表行集中相应的行。

在针对基表打开的行集上，需要提供行集属性 `DBPROP_IRowsetLocate` 和 `DBPROP_BOOKMARKS`。

### <a name="pure-table-scans"></a>纯表扫描

SQL Server 从提供程序扫描整个远程表，并在本地执行所有查询评估。 通过调用 `IOpenRowset::OpenRowset` 打开与该表对应的行集。 SQL Server 根据名称的目录、架构和对象部分构造提供给 `OPENROWSET` 的表名，如下所示：

1. 每个名称部分都使用提供程序的引号字符 (`DBLITERAL_QUOTE`) 括起来，然后使用它们之间嵌入的 `DBLITERAL_CATALOG_SEPARATOR` 字符连接起来。

1. 打开行集对象后，SQL Server 使用 `IColumnsInfo` 接口来验证执行时元数据是否与表的编译时元数据相同。

1. SQL Server 使用 `IRowset` 接口来导航和使用表中的行。 使用 `IRowset::GetNextRows` 来提取行，使用 `IRowset::RestartPosition` 来重新定位到行集的开头，使用 `IRowset::ReleaseRows` 来释放行。

### <a name="update-and-delete-statements"></a>`UPDATE` 和 `DELETE` 语句

要从 SQL Server 分布式查询更新或删除远程表，必须满足以下条件：

- 对于要更新或删除的表，提供程序必须支持通过 `IOpenRowset` 打开的行集的书签。

- 对于要更新或删除的表，提供程序必须支持通过 `IOpenRowset` 打开的行集上的 `IRowsetLocate` 和 `IRowsetChange` 接口。

- `IRowsetChange` 接口必须支持更新 (`SetData`) 和删除 (`DeleteRows`) 方法。

- 如果提供程序不支持 `ITransactionLocal`，则仅当为该提供程序设置了 `Non-transacted` 选项且该语句不在用户事务中时，才允许使用 `UPDATE` 和 `DELETE` 语句。

- 如果提供程序不支持 `ITransactionJoin`，则仅当它不在用户事务中时，才允许使用 `UPDATE` 或 `DELETE` 语句。

针对更新的表打开的行集需要以下行集属性：`DBPROP_IRowsetLocate`、`DBPROP_IRowsetChange` 和 `DBPROP_BOOKMARKS`。 `DBPROP_UPDATABILITY` 行集属性可分别设置为 `DBPROPVAL_UP_CHANGE` 或 `DBPROPVAL_UP_DELETE`，具体取决于执行的操作是 `UPDATE` 还是 `DELETE`。

针对处理 `UPDATE` 或 `DELETE` 操作的提供程序执行以下高级步骤：

1. SQL Server 通过 `IOpenRowset` 接口打开基表行集。 SQL Server 需要有关行集的上述属性。

1. SQL Server 确定要更新或删除的符合条件的行集。

1. SQL Server 使用书签，通过 `IRowsetLocate` 接口来定位符合条件的行。

1. 使用 `IRowsetChange::SetData` 进行 `UPDATE` 操作，或使用 `IRowsetChange::DeleteRows` 进行删除操作，以对符合条件的行执行所需的更改。

### <a name="insert-statement"></a>`INSERT` 语句

支持远程表的 `INSERT` 语句的条件不如 `UPDATE` 和 `DELETE` 语句严格：

- 提供程序必须支持对要插入的基表上打开的行集使用 `IRowsetChange::InsertRow`。

- 如果提供程序不支持 `ITransactionLocal`，则仅当为该链接服务器设置了 `Non-transacted updates` 选项且该语句不在用户事务中时，才允许使用 `INSERT` 语句。

- 如果提供程序不支持 `ITransactionJoin`，则仅当 `INSERT` 语句不在用户事务中时，才允许使用它们。

SQL Server 使用 `IOpenRowset::OpenRowset` 在基表上打开行集，并调用 `IRowsetChange::InsertRow` 将新行插入基本行集中。

### <a name="pass-through-queries"></a>传递查询

此方案类似于远程查询中的方案，只是提供给 `ICommand` 的命令文本是用户提交的命令字符串，并且不由 SQL Server 解释。 SQL Server 在调用 `ICommandText::SetCommandText` 时使用 `DBGUID_DEFAULT` 作为方言标识符。 `DBGUID_DEFAULT` 指示提供程序应使用其默认方言。 如果此命令文本返回多个结果集，例如，如果该命令调用返回多个结果集的存储过程，则 SQL Server 将只使用该命令的第一个结果集。

有关 SQL Server 使用的所有 OLE DB 接口的列表，请参阅 [SQL Server 使用的 OLE DB 接口](#appendixa)。

### <a name="conclusion"></a>结论

Microsoft SQL Server 提供了一组最强大的工具，用于访问来自异类数据源的数据。 通过了解 SQL Server 公开的 OLE-DB 接口，开发者可以在分布式查询中实现高度的控制和复杂性。

## <a name="ole-db-interfaces-consumed-by-sql-server"></a><a name="appendixa"></a> SQL Server 使用的 OLE DB 接口

下表列出了 SQL Server 使用的所有 OLE DB 接口。 “必需”列指示接口是否是 SQL Server 所需的最小 OLE DB 功能的一部分，或者该接口是否是可选的。 如果给定的接口未标记为必需，则 SQL Server 仍可以访问提供程序，但是对于该提供程序，某些特定的 SQL Server 功能或优化无法使用。

对于可选接口，“方案”列指示使用指定接口的六个方案中的一个或多个方案。 例如，基表行集上的 `IRowsetChange` 接口是可选接口；此接口用于 `UPDATE` 和 DELETE 语句以及 `INSERT` 语句方案。 如果不支持此接口，则不支持对该提供程序使用 UPDATE、DELETE 和 `INSERT` 语句。 某些其他可选接口在“方案”列中标记为\"性能\"，表示该接口的基本性能更佳。 例如，如果不支持 `IDBSchemaRowset` 接口，则 SQL Server 必须打开行集两次：一次用于其元数据，一次用于执行查询。 通过支持 `IDBSchemaRowset`，SQL Server 性能得到了改进。

|Object|接口|必需|注释|方案|
|:-----|:-----|:-----|:-----|:-----|
|数据源对象|`IDBInitialize`|是|初始化并设置数据和安全上下文。| |
| |`IDBCreateSession`|是|创建 DB 会话对象。| |
| |`IDBProperties`|是|获取有关提供程序功能、设置初始化属性和必需属性的信息：DBPROP_INIT_TIMEOUT。| |
| |`IDBInfo`|否|获取引用文本、目录、名称、部件、分隔符、字符等。|远程查询。|
|DB 会话对象|`IDBSchemaRowset`|否|获取表/列元数据。 所需的行集：`TABLES`、`COLUMNS`、`PROVIDER_TYPES`；使用的其他行集（如果可用）：`INDEXES`、`TABLE_STATISTICS`。|性能，索引访问。|
| |`IOpenRowset`|是|打开表、索引或直方图上的行集。| |
| |`IGetDataSource`|是|用于从数据库会话对象返回到 DSO。| |
| |`IDBCreateCommand`|否|用于为支持查询的提供程序创建命令对象（查询）。|远程查询，传递查询。|
| |`ITransactionLocal`|否|用于事务更新。|`UPDATE`、`DELETE` 和 `INSERT` 语句。|
| |`ITransactionJoin`|否|用于分布式事务支持。|如果在用户事务中，则为 `UPDATE`、`DELETE` 和 `INSERT` 语句。|
|行集对象|IRowset|是|扫描行。| |
| |`IAccessor`|是|绑定到行集中的列。| |
| |`IColumnsInfo`|是|获取有关行集中的列的信息。| |
| |`IRowsetInfo`|是|获取有关行集属性的信息。| |
| |`IRowsetLocate`|否|`UPDATE`/`DELETE` 操作以及执行基于索引的查找所需；用于按书签查找行。|索引访问，`UPDATE` 和`DELETE` 语句。|
| |`IRowsetChange`|否|行集上的 `INSERTS`/`UPDATES`/ `DELETES` 所需。 针对基表的行集应支持用于 `INSERT`、`UPDATE` 和 `DELETE` 语句的此接口。|`UPDATE`、`DELETE` 和 `INSERT` 语句。|
| |`IConvertType`|是|用于验证行集是否支持对其列进行特定数据类型转换。| |
|索引|`IRowset`|是|扫描行。|索引访问，性能。|
| |`IAccessor`|是|绑定到行集中的列。|索引访问，性能。|
| |`IColumnsInfo`|是|获取有关行集中的列的信息。|索引访问，性能。|
| |`IRowsetInfo`|是|获取有关行集属性的信息。|索引访问，性能。|
| |`IRowsetIndex`|是|仅为索引上的行集所需；用于索引功能（设置范围、查找）。|索引访问，性能。|
|Command|`ICommand`|是| |远程查询，传递查询。|
| |`ICommandText`|是|用于定义查询文本。|远程查询，传递查询。|
| |`IColumnsInfo`|是|用于获取查询结果的列元数据。|远程查询，传递查询。|
| |`ICommandProperties`|是|用于指定命令返回的行集的必需属性。|远程查询，传递查询。|
| |`ICommandWithParameters`|否|用于参数化查询执行。|远程查询，性能。|
| |`ICommandPrepare`|否|用于准备获取元数据的命令（如果可用，则用于传递查询）。|远程查询，性能。|
|错误对象|`IErrorRecords`|是|用于获取指向与单个错误记录对应的 `IErrorInfo` 接口的指针。| |
| |`IErrorInfo`|是|用于获取指向与单个错误记录对应的 `IErrorInfo` 接口的指针。| |
|任何对象|`ISupportErrorInfo`|否|用于验证给定接口是否支持错误对象。| |
|  |  |  |  |  |

>[!NOTE]
>`Index` 对象、`Command` 对象和 `Error` 对象不是必需的。 但是，如果它们受支持，则列出的接口是必需的，如“必需”列中所指定的那样。

## <a name="sql-subset-used-for-generating-remote-queries"></a><a name="appendixb"></a>用于生成远程查询的 SQL 子集

SQL Server 查询处理器针对 SQL 命令提供程序生成的 SQL 子集取决于提供程序支持的语法级别，如 `DBPROP_SQLSUPPORT` 属性所示。

支持 SQL 入门级别或 ODBC Core 的 SQL 命令提供程序

对于支持 SQL-92 入门级别或 ODBC Core 的 SQL 命令提供程序评估的查询，SQL Server 使用以下 SQL 语言子集：

1. 包含 `SELECT`、`FROM`、`WHERE`、`GROUP BY`、`UNION`、`UNION ALL`、`ORDER BY DESC`、`ASC` 和 `HAVING` 子句的 `SELECT` 语句。

1. 仅针对支持 SQL-92 入门级别的提供程序，而非支持 ODBC Core 的提供程序生成 `UNION` 和 `UNION ALL`。

1. `SELECT` 子句：

   - `SELECT` 列表中的标量子查询。

   - 不带 `AS` 关键字的列别名。

1. `FROM` 子句：

   - 不使用显式联接关键字；逗号分隔的表名用于指定内联，并且远程查询中未指定外部联接。

   - `FROM` ( `<nested query>` ) `<alias>` 形式的嵌套查询。

   - 不带 AS 关键字的表别名。

1. `WHERE` 子句使用带有 `NOT`、`EXISTS`、`ANY`、`ALL` 的子查询。

1. 表达式：

   - 所使用的聚合函数：`MIN([DISTINCT])`、`MAX([DISTINCT])`、`COUNT([DISTINCT])`、`SUM([DISTINCT])`、`AVG([DISTINCT])` 和 `COUNT(*)`。

   - 比较运算符：`<`、`=`、`<=`、`>`、`<>`、`>=`、`IS NULL` 和 `IS NOT NULL`。

   - 布尔运算符：`AND`、`OR` 和 `NOT`。

 - 算术运算符：`+`、`-`、`*` 和 `/`。

1. 常数：

- 数字和货币文本周围始终带有 `( )`。

- 字符文本用 `' '` 括起来。

支持 SQL Minimum 级别的 SQL 命令提供程序

对于支持 SQL Minimum 级别的 SQL 命令提供程序，SQL Server 使用以下语法生成 SQL。

此语法是使用 ODBC 3.0 中描述的 SQL 最低语法派生的。 突出显示了与该语法的所有差异。 `*bold italics`* 中显示的项是添加到 ODBC 3.0 中描述的 SQL 最低语法的项。 以绿色显示的已删除项是从该语法中删除的项。

*select-statement* ::=

`SELECT [ALL | DISTINCT] *select-list* FROM *table-reference-list*[WHERE *search-condition*] [order-by-clause]`

`SELECT` clause

select-list ::= `*` | `select-sublist [, select-sublist]...`

select-sublist ::= expression * [`alias`]*

`*alias ::= user-defined-name`*

`FROM clause`

table-reference-list ::= table-reference

table-identifier ::= user-defined-name

table-name ::= table-identifier

table-reference ::= table-name

`WHERE clause`

search-condition ::= boolean-term \[OR search-condition\]

boolean-term ::= boolean-factor \[AND boolean-term\]

boolean-factor ::= \[NOT\] boolean-primary

boolean-primary ::= comparison-predicate \| ( search-condition )

comparison-predicate ::= expression comparison-operator expression

*\| `expression IS \[NOT\] NULL`*

comparison-operator ::= `< \| >` \| `<= \| >`= \| = \| `<>`

`ORDER BY clause`

order-by-clause ::= ORDER BY sort-specification \[, sort-specification\]\..

sort-specification ::= { \| column-name } \[ASC \| DESC\]

`Common syntactic elements`

expression ::= term \| expression {+\|--} term

term ::= factor \| term {\*\|/} factor

factor ::= \[+\|--\] primary

primary ::= column-name

\| literal

\| ( expression )

column-name ::= \[table-name.\]column-identifier

literal ::= character-string-literal

\| integer-literal

\| exact-numeric-literal

character-string-literal ::= \'{character}...\'

（字符是驱动程序/数据源的字符集中的任意字符。 要在字符-字符串-文本中包含单个文本引号字符 (\')，请使用两个文本引号字符 (\'\')。）

`*integer-literal ::=* \[*+ \| -*\] *unsigned-integer`*

`*exact-numeric-literal::=* \[*+ \| -*\] *unsigned-integer* \[*period unsigned-integer*\]`

`*\| period unsigned-integer`*

base-table-name ::= base-table-identifier

base-table-identifier ::= user-defined-name

column-identifier ::= user-defined-name

user-defined-name ::= letter\[digit \| letter \| _\]\..

unsigned-integer ::= {digit}...

digit ::= 0 \| 1 \| 2 \| 3 \| 4 \| 5 \| 6 \| 7 \| 8 \| 9

period ::= . 

## <a name="sql-server-specific-properties"></a><a name="appendixc"></a>SQL Server 特定的属性

```
enum SQLPROPERTIES
       {
       SQLPROP_NOHPNEEDED = 0x1,
       SQLPROP_FREETHREADED = 0x2,
       SQLPROP_UMSENABLED = 0x3,
       SQLPROP_NESTEDQUERIES = 0x4,
       SQLPROP_DYNAMICSQL = 0x5,
       SQLPROP_GROUPBY = 0x6,
       SQLPROP_DATELITERALS = 0x7,
       SQLPROP_ANSILIKE = 0x8,
       SQLPROP_INNERJOIN = 0x9,
       SQLPROP_SUBQUERIES = 0x10, 
       SQLPROP_PARALLELSCAN = 0x11,
       SQLPROP_COLUMNCOLLATION = 0x12,
       SQLPROP_CARDINALITY = 0x13,
       SQLPROP_SIMPLEUPDATES = 0x14,
       SQLPROP_SQLLIKE = 0x15,
       SQLPROP_BITREMOTING = 0x16,
       SQLPROP_UNICODELITERALS = 0x17,
       SQLPROP_USELATESTCOLLATIONVERSION = 0x18
       };

```
