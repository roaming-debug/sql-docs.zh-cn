---
title: CREATE DATABASE (Transact-SQL) | Microsoft Docs
description: 创建适用于 SQL Server、Azure SQL 数据库、Azure Synapse Analytics 和 Analytics Platform System 的数据库语法
ms.custom: references_regions
ms.date: 12/11/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_TSQL
- DATABASE
- CONTAINMENT_TSQL
- CREATE DATABASE
- CREATE_DATABASE_TSQL
- CONTAINS_FILESTREAM_TSQL
- CONTAINS FILESTREAM
- CONTAINMENT
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], creating
- databases [SQL Server], creating
- model database [SQL Server], database creation
- mounted drives [SQL Server]
- CREATE DATABASE
- CREATE DATABASE statement
- file creation [SQL Server]
- creating databases
- containment
- filegroups [SQL Server], database creation
- database creation [SQL Server], CREATE DATABASE statement
- moving databases
- attaching databases [SQL Server], CREATE DATABASE...FOR ATTACH
ms.assetid: 29ddac46-7a0f-4151-bd94-75c1908c89f8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016'
ms.openlocfilehash: 509e4fc7445371c57d0fed7208d556e53aa18fe8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063032"
---
# <a name="create-database"></a>CREATE DATABASE

创建新数据库。

单击以下选项卡之一，了解所使用的特定 SQL 版本的语法、参数、备注、权限和示例。

有关语法约定的详细信息，请参阅 [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL 数据库](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="overview"></a>概述

在 SQL Server 中，此语句创建新数据库和使用的文件及其文件组。 它还可用于创建数据库快照，或附加数据库文件以从其他数据库的分离文件创建数据库。

## <a name="syntax"></a>语法

创建数据库

```syntaxsql
CREATE DATABASE database_name
[ CONTAINMENT = { NONE | PARTIAL } ]
[ ON
      [ PRIMARY ] <filespec> [ ,...n ]
      [ , <filegroup> [ ,...n ] ]
      [ LOG ON <filespec> [ ,...n ] ]
]
[ COLLATE collation_name ]
[ WITH <option> [,...n ] ]
[;]

<option> ::=
{
      FILESTREAM ( <filestream_option> [,...n ] )
    | DEFAULT_FULLTEXT_LANGUAGE = { lcid | language_name | language_alias }
    | DEFAULT_LANGUAGE = { lcid | language_name | language_alias }
    | NESTED_TRIGGERS = { OFF | ON }
    | TRANSFORM_NOISE_WORDS = { OFF | ON}
    | TWO_DIGIT_YEAR_CUTOFF = <two_digit_year_cutoff>
    | DB_CHAINING { OFF | ON }
    | TRUSTWORTHY { OFF | ON }
    | PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='<Filepath to folder on DAX formatted volume>' )
}

<filestream_option> ::=
{
      NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }
    | DIRECTORY_NAME = 'directory_name'
}

<filespec> ::=
{
(
    NAME = logical_file_name ,
    FILENAME = { 'os_file_name' | 'filestream_path' }
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB | % ] ]
)
}

<filegroup> ::=
{
FILEGROUP filegroup name [ [ CONTAINS FILESTREAM ] [ DEFAULT ] | CONTAINS MEMORY_OPTIMIZED_DATA ]
    <filespec> [ ,...n ]
}
```

附加数据库

```syntaxsql
CREATE DATABASE database_name
    ON <filespec> [ ,...n ]
    FOR { { ATTACH [ WITH <attach_database_option> [ , ...n ] ] }
        | ATTACH_REBUILD_LOG }
[;]

<attach_database_option> ::=
{
      <service_broker_option>
    | RESTRICTED_USER
    | FILESTREAM ( DIRECTORY_NAME = { 'directory_name' | NULL } )
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
}
```

创建数据库快照

```syntaxsql
CREATE DATABASE database_snapshot_name
    ON
    (
        NAME = logical_file_name,
        FILENAME = 'os_file_name'
    ) [ ,...n ]
    AS SNAPSHOT OF
[;]
```

## <a name="arguments"></a>参数

database_name 是新数据库的名称。 数据库名称在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中必须唯一，并且必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。

除非没有为日志文件指定逻辑名称，否则 database_name 最多可以包含 128 个字符。 如果未指定逻辑日志文件名称，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会通过向 database_name 追加后缀来为日志生成 logical_file_name 和 os_file_name  。 这会将 *database_name* 限制为 123 个字符，从而使生成的逻辑文件名称不超过 128 个字符。

如果未指定数据文件名称，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会使用 database_name 同时作为 logical_file_name 和 os_file_name  。 默认路径从注册表中获得。 可以使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的“服务器属性”（“数据库设置”页）更改默认路径。 更改默认路径要求重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。

CONTAINMENT = { NONE | PARTIAL }

**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本

指定数据库的包含状态。 NONE = 非包含数据库。 PARTIAL = 部分包含的数据库。

ON 指定显式定义用来存储数据库数据部分的磁盘文件（数据文件）。 当后面是以逗号分隔的、用以定义主文件组的数据文件的 \<filespec> 项列表时，需要使用 ON。 主文件组的文件列表可后跟以逗号分隔的、用以定义用户文件组及其文件的 \<filegroup> 项列表（可选）。

PRIMARY 指定关联的 \<filespec> 列表定义主文件。 在主文件组的 \<filespec> 项中指定的第一个文件将成为主文件。 一个数据库只能有一个主文件。 有关详细信息，请参阅 [数据库文件和文件组](../../relational-databases/databases/database-files-and-filegroups.md)。

如果没有指定 PRIMARY，那么 CREATE DATABASE 语句中列出的第一个文件将成为主文件。

LOG ON 指定显式定义用来存储数据库日志的磁盘文件（日志文件）。 LOG ON 后跟以逗号分隔的用以定义日志文件的 \<filespec> 项列表。 如果没有指定 LOG ON，将自动创建一个日志文件，其大小为该数据库的所有数据文件大小总和的 25% 或 512 KB，取两者之中的较大者。 此文件放置于默认的日志文件位置。 有关此位置的信息，请参阅[查看或更改数据文件和日志文件的默认位置 - SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md)。

不能对数据库快照指定 LOG ON。

COLLATE collation_name 指定数据库的默认排序规则。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 如果没有指定排序规则，则将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认排序规则分配为数据库的排序规则。 不能对数据库快照指定排序规则名称。

不能使用 FOR ATTACH 或 FOR ATTACH_REBUILD_LOG 子句指定排序规则名称。 有关如何更改附加数据库的排序规则的信息，请访问此 [Microsoft 网站](https://go.microsoft.com/fwlink/?linkid=16419&kbid=325335)。

有关 Windows 和 SQL 排序规则名称的详细信息，请参阅 [COLLATE](~/t-sql/statements/collations.md)。

> [!NOTE]
> 包含数据库的排序方式不同于非包含数据库。 有关详细信息，请参阅[包含的数据库排序规则](../../relational-databases/databases/contained-database-collations.md)。

WITH \<option>
 **\<filestream_option>**

NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL } 适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本 。

指定对数据库的非事务性 FILESTREAM 访问的级别。

|值|说明|
|-----------|-----------------|
|OFF|禁用非事务性访问。|
|READONLY|可以通过非事务性进程读取此数据库中的 FILESTREAM 数据。|
|FULL|启用对 FILESTREAM FileTable 的完全非事务性访问。|

DIRECTORY_NAME = \<directory_name>
适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本

与 Windows 兼容的目录名称。 此名称应在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的所有 Database_Directory 名称中唯一。 无论 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则设置如何，唯一性比较都不区分大小写。 在此数据库中创建 FileTable 之前，应设置此选项。

仅在将 CONTAINMENT 设置为 PARTIAL 之后，才允许使用以下选项。 如果将 CONTAINMENT 设置为 NONE，将发生错误。

- **DEFAULT_FULLTEXT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本

  有关此选项的完整说明，请参阅[配置“默认全文语言”服务器配置选项](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md)。

- **DEFAULT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本

  有关此选项的完整说明，请参阅[配置“默认语言”服务器配置选项](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md)。

- **NESTED_TRIGGERS = { OFF | ON}**

  **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本

  有关此选项的完整说明，请参阅[配置“嵌套触发器”服务器配置选项](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md)。

- **TRANSFORM_NOISE_WORDS = { OFF | ON}**

  **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本

  有关此选项的完整说明，请参阅[“转换干扰词”服务器配置选项](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md)。

- **TWO_DIGIT_YEAR_CUTOFF = { 2049 | \<any year between 1753 and 9999> }**

  表示一年的四位。 2049 为默认值。 有关此选项的完整说明，请参阅[配置 two digit year cutoff 服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。

- **DB_CHAINING { OFF | ON }**

  当指定为 ON 时，数据库可以为跨数据库所有权链的源或目标。

  当为 OFF 时，数据库不能参与跨数据库所有权链接。 默认为 OFF。

  > [!IMPORTANT]
  > 如果 cross db ownership chaining 服务器选项为 0 (OFF)，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将可以识别此设置。 如果 cross db ownership chaining 为 1 (ON)，则不论此选项为何值，所有用户数据库都可以参与跨数据库所有权链。 此选项是使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 来设置的。

  若要设置此选项，要求具有 sysadmin 固定服务器角色的成员身份。 不能针对下列系统数据库设置 DB_CHAINING 选项：master、model 和 tempdb。

- **TRUSTWORTHY { OFF | ON }**

  当指定 ON 时，使用模拟上下文的数据库模块（例如，视图、用户定义函数或存储过程）可以访问数据库以外的资源。

  当为 OFF 时，模拟上下文中的数据库模块不能访问数据库以外的资源。 默认为 OFF。

  只要附加数据库，TRUSTWORTHY 就会设置为 OFF。

  默认情况下，除 msdb 数据库之外的所有系统数据库都将 TRUSTWORTHY 设置为 OFF。 对于 model 和 tempdb 数据库，不能更改此值。 建议在任何情况下都不要将 master 数据库的 TRUSTWORTHY 选项设置为 ON。

- **PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='' )**

  指定此选项后，将在位于由存储类内存（NVDIMM-N 永久性内存）支持的磁盘设备的卷上创建事务日志缓冲区 - 也称为持久性日志缓冲区。 有关详细信息，请参阅[使用存储类内存的事务提交延迟加速](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)。 适用于：[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 及更高版本。

FOR ATTACH [ WITH \< attach_database_option > ] 指定通过[附加](../../relational-databases/databases/database-detach-and-attach-sql-server.md)一组现有的操作系统文件来创建数据库。 必须有一个指定主文件的 \<filespec> 项。 至于其他 \<filespec> 项，只需要指定与第一次创建数据库或上一次附加数据库时路径不同的文件的那些项即可。 必须有一个 \<filespec> 项指定这些文件。

FOR ATTACH 具有以下要求：

- 所有数据文件（MDF 和 NDF）都必须可用。
- 如果存在多个日志文件，这些文件都必须可用。

如果一个可读/写数据库具有一个当前不可用的日志文件，并且进行附加操作前在没有使用用户或打开的事务的情况下关闭了该数据库，那么 FOR ATTACH 会自动重新生成日志文件并更新主文件。 相比之下，对于只读数据库，由于主文件不能更新，将不能重新生成日志。 因此，如果附加一个日志不可用的只读数据库，则必须提供日志文件，或提供 FOR ATTACH 子句中的文件。

> [!NOTE]
> 无法在早期版本的 SQL Server 中附加由较新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建的数据库。

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，作为待附加数据库的组成部分的所有全文文件也将随之一起附加。 若要指定全文目录的新路径，请指定不带全文操作系统文件名的新位置。 有关详细信息，请参阅“示例”部分。

将包含 FILESTREAM 选项“目录名称”的数据库附加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中将提示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 验证 Database_Directory 名称是否唯一。 如果该名称不唯一，附加操作将失败，并显示错误“FILESTREAM Database_Directory name \<name> 在此 SQL Server 实例中不唯一”。 为避免此错误，应将可选参数 *directory_name* 传递给此操作。

不能对数据库快照指定 FOR ATTACH。

对于 ATTACH，可以指定 RESTRICTED_USER 选项。 RESTRICTED_USER 只允许 db_owner 固定数据库角色成员以及 dbcreator 和 sysadmin 固定服务器角色成员连接到数据库，不过对连接数没有限制。 无资格用户的尝试将被拒绝。

如果数据库使用 [!INCLUDE[ssSB](../../includes/sssb-md.md)]，请在 FOR ATTACH 子句中使用 WITH \<service_broker_option>：

\<service_broker_option> 控制 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息传递和数据库的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符。 仅当使用 FOR ATTACH 子句时，才能指定 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 选项。

ENABLE_BROKER 指定对指定的数据库启用 [!INCLUDE[ssSB](../../includes/sssb-md.md)]。 也就是说，启动了消息传递，并且在 sys.databases 目录视图中将 is_broker_enabled 设置为 true。 数据库保留现有的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符。

NEW_BROKER 在 sys.databases 和还原数据库中都创建一个新的 service_broker_guid 值，并通过清除结束所有会话端点。 Broker 已启用，但未向远程会话端点发送消息。 必须使用新标识符重新创建任何引用旧 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符的路由。

ERROR_BROKER_CONVERSATIONS 结束所有会话，并产生一个错误指出数据库已附加或还原。 Broker 一直处于禁用状态直到此操作完成，然后再将其启用。 数据库保留现有的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符。

当您附加已复制的数据库而不是分离的数据库时，请注意以下事项：

- 如果将数据库附加到与原始数据库相同的服务器实例和版本，则不需要执行其他步骤。
- 如果将数据库附加到同一个服务器实例，但是版本已升级，则必须执行 [sp_vupgrade_replication](../../relational-databases/system-stored-procedures/sp-vupgrade-replication-transact-sql.md) 才能在附加操作完成后升级复制。
- 如果将数据库附加到不同的服务器实例，而不考虑版本，则必须执行 [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) 才能在附加操作完成后删除复制。

> [!NOTE]
> 附加操作使用 **vardecimal** 存储格式进行，但必须将 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]至少升级到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2。 无法将使用 Vardecimal 存储格式的数据库附加到早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 有关 **vardecimal** 存储格式的详细信息，请参阅 [数据压缩](../../relational-databases/data-compression/data-compression.md)。

当数据库第一次附加或还原到新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例时，数据库主密钥（由服务主密钥加密）的副本尚未存储在服务器中。 必须使用 **OPEN MASTER KEY** 语句解密数据库主密钥 (DMK)。 一旦 DMK 解密后，通过使用 **ALTER MASTER KEY REGENERATE** 语句向服务器提供 DMK（使用服务主密钥 (SMK) 加密）的副本，即可拥有将来启用自动解密的选项。 当数据库已从较早版本升级后，应重新生成 DMK 以使用更新的 AES 算法。 有关重新生成 DMK 的详细信息，请参阅 [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md)。 重新生成 DMK 密钥以升级到 AES 所需的时间取决于 DMK 保护的对象数。 重新生成 DMK 密钥以升级到 AES 只在必需时执行一次，不影响将来作为密钥循环策略的一部分而重新生成的过程。 有关如何使用附加来升级数据库的信息，请参阅[使用分离和附加来升级数据库](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md)。

> [!IMPORTANT]
> 建议您不要附加未知或不可信源中的数据库。 此类数据库可能包含恶意代码，这些代码可能会执行非预期的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，或者通过修改架构或物理数据库结构导致错误。 使用来自未知源或不可信源的数据库前，请在非生产服务器上针对数据库运行 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)，然后检查数据库中的代码，例如存储过程或其他用户定义代码。
> [!NOTE]
> 在附加数据库时，**TRUSTWORTHY** 和 **DB_CHAINING** 选项没有影响。

FOR ATTACH_REBUILD_LOG 指定通过附加一组现有的操作系统文件来创建数据库。 该选项只限于读/写数据库。 必须有一个指定主文件的 \<filespec> 项。 如果缺少一个或多个事务日志文件，将重新生成日志文件。 ATTACH_REBUILD_LOG 自动创建一个新的 1 MB 的日志文件。 此文件放置于默认的日志文件位置。 有关此位置的信息，请参阅[查看或更改数据文件和日志文件的默认位置 - SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md)。

> [!NOTE]
> 如果日志文件可用，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将使用这些文件，而不会重新生成日志文件。

FOR ATTACH_REBUILD_LOG 具有以下要求：

- 完全关闭数据库。
- 所有数据文件（MDF 和 NDF）都必须可用。

> [!IMPORTANT]
> 该操作会中断日志备份链。 建议在完成该操作后执行完整数据库备份。 有关详细信息，请参阅 [BACKUP](../../t-sql/statements/backup-transact-sql.md)。

通常，FOR ATTACH_REBUILD_LOG 用于将具有大型日志的可读/写数据库复制到另一台服务器，在这台服务器上，数据库副本频繁使用，或仅用于读操作，因而所需的日志空间少于原始数据库。

不能对数据库快照指定 FOR ATTACH_REBUILD_LOG。

有关附加数据库和分离数据库的详细信息，请参阅[数据库分离和附加](../../relational-databases/databases/database-detach-and-attach-sql-server.md)。

\<filespec> 控制文件属性。

NAME logical_file_name 指定文件的逻辑名称。 指定 FILENAME 时，需要使用 NAME，除非指定 FOR ATTACH 子句之一。 无法将 FILESTREAM 文件组命名为 PRIMARY。

logical_file_name 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中引用文件时所用的逻辑名称。 *Logical_file_name* 在数据库中必须唯一，并且必须符合 [标识符](../../relational-databases/databases/database-identifiers.md)规则。 名称可以是字符或 Unicode 常量，也可以是常规标识符或分隔标识符。

FILENAME { **'** _os\_file\_name_ **'** | **'** _filestream\_path_ **'** } 指定操作系统（物理）文件名。

**'** *os_file_name* **'** 是创建文件时由操作系统使用的路径和文件名。 文件必须驻留在下列一种设备中：安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地服务器、存储区域网络 [SAN] 或基于 iSCSI 的网络。 执行 CREATE DATABASE 语句前，指定路径必须存在。 有关详细信息，请参阅“备注”部分的“数据库文件和文件组”。

如果为该文件指定了 UNC 路径，则可以设置 SIZE、MAXSIZE 和 FILEGROWTH 参数。

如果文件位于原始分区上，则 *os_file_name* 必须仅指定现有原始分区的驱动器号。 每个原始分区上只能创建一个数据文件。

不应将数据文件放在压缩文件系统中，除非这些文件是只读的辅助文件或数据库是只读的。 日志文件一定不要放在压缩文件系统中。

**'** *filestream_path* **'** 对于 FILESTREAM 文件组，FILENAME 指向将存储 FILESTREAM 数据的路径。 在最后一个文件夹之前的路径必须存在，但不能存在最后一个文件夹。 例如，如果指定路径 C:\MyFiles\MyFilestreamData、C:\MyFiles 必须存在才能运行 ALTER DATABASE，但 MyFilestreamData 文件夹不能存在。

必须在同一语句中创建文件组和文件 (`<filespec>`)。

SIZE 和 FILEGROWTH 属性不适用于 FILESTREAM 文件组。

SIZE size 指定文件的大小。

将 *os_file_name* 指定为 UNC 路径时，不能指定 SIZE。 SIZE 不适用于 FILESTREAM 文件组。

size 文件的初始大小。

如果没有为主文件提供 *size*，[!INCLUDE[ssDE](../../includes/ssde-md.md)]会使用 model 数据库中主文件的大小。 model 数据库中主文件的默认大小为 8 MB（从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始）或 1 MB（对于较早版本）。 如果指定了辅助数据文件或日志文件，但未指定该文件的 *size*，[!INCLUDE[ssDE](../../includes/ssde-md.md)]会以 8 MB（从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始）或 1 MB（对于较早版本）作为该文件的大小。 为主文件指定的大小至少应与 model 数据库的主文件大小相同。

可以使用千字节 (KB)、兆字节 (MB)、千兆字节 (GB) 或兆兆字节 (TB) 后缀。 默认值为 MB。 指定一个整数，不包含小数位。 *Size* 是一个整数值。 对于大于 2147483647 的值，使用更大的单位。

MAXSIZE max_size 指定文件可增大到的最大大小。 将 *os_file_name* 指定为 UNC 路径时，不能指定 MAXSIZE。

max_size 最大的文件大小。 可以使用 KB、MB、GB 和 TB 后缀。 默认值为 MB。 指定一个整数，不包含小数位。 如果未指定 *max_size*，文件将一直增长到磁盘变满为止。 *Max_size* 是一个整数值。 对于大于 2147483647 的值，使用更大的单位。

UNLIMITED 指定文件将增长到磁盘充满。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，指定为不限制增长的日志文件的最大大小为 2 TB，而数据文件的最大大小为 16 TB。

> [!NOTE]
> 为 FILESTREAM 容器指定此选项时，没有最大大小。 它将继续增大，直到磁盘已满。

FILEGROWTH growth_increment 指定文件的自动增量。 文件的 FILEGROWTH 设置不能超过 MAXSIZE 设置。 将 *os_file_name* 指定为 UNC 路径时，不能指定 FILEGROWTH。 FILEGROWTH 不适用于 FILESTREAM 文件组。

growth_increment 每次需要新空间时为文件添加的空间量。

该值可以 MB、KB、GB、TB 或百分比 (%) 为单位指定。 如果未在数量后面指定 MB、KB 或 %，则默认值为 MB。 如果指定 %，则增量大小为发生增长时文件大小的指定百分比。 指定的大小舍入为最接近的 64 KB 的倍数，最小值为 64 KB。

值为 0 时表明自动增长被设置为关闭，不允许增加空间。

如果未指定 FILEGROWTH，则默认值为：

|版本|默认值|
|-------------|--------------------|
|从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始|数据 64 MB。 日志文件 64 MB。|
|从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始|数据 1 MB。 日志文件 10%。|
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 之前|数据 10%。 日志文件 10%。|

\<filegroup> 控制文件组属性。 不能对数据库快照指定文件组。

FILEGROUP filegroup_name 文件组的逻辑名称。

filegroup_name
filegroup_name 在数据库中必须唯一，并且不能是系统提供的名称 PRIMARY 和 PRIMARY_LOG。 名称可以是字符或 Unicode 常量，也可以是常规标识符或分隔标识符。 名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。

CONTAINS FILESTREAM 指定文件组在文件系统中存储 FILESTREAM 二进制大型对象 (BLOB)。

CONTAINS MEMORY_OPTIMIZED_DATA

**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本

指定文件组在文件系统中存储内存优化数据。 有关详细信息，请参阅[内存中 OLTP - 内存中优化](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。 每个数据库只能有一个 MEMORY_OPTIMIZED_DATA 文件组。 有关通过创建文件组来存储内存优化数据的代码示例，请参阅[创建内存优化表和本机编译的存储过程](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md)。

DEFAULT 指定命名文件组为数据库中的默认文件组。

database_snapshot_name 新数据库快照的名称。 数据库快照名称必须在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中唯一，并且必须符合标识符规则。 *database_snapshot_name* 最多可以包含 128 个字符。

ON ( NAME =logical\_file\_name, FILENAME ='os\_file\_name') [ ,... n ] 若要创建数据库快照，请在源数据库中指定文件列表。 若要使快照工作，必须分别指定所有数据文件。 但是，日志文件不允许用于数据库快照。 数据库快照不支持 FILESTREAM 文件组。 如果在 CREATE DATABASE ON 子句中包含了 FILESTREAM 数据文件，该语句将失败，并且会引发错误。

有关 NAME 和 FILENAME 以及其值的说明，请参阅等效的 \<filespec> 值的说明。

> [!NOTE]
> 创建数据库快照时，不允许使用其他 \<filespec> 选项和关键字 PRIMARY。

AS SNAPSHOT OF source_database_name 指定要创建的数据库为 source_database_name 指定的源数据库的数据库快照。 快照和源数据库必须位于同一实例中。

有关详细信息，请参阅“备注”部分的[数据库快照](#database-snapshots)。

## <a name="remarks"></a>备注

创建、修改或删除用户数据库后，应备份 [master 数据库](../../relational-databases/databases/master-database.md)。

`CREATE DATABASE` 语句必须在自动提交模式（默认事务管理模式）下运行，且不允许用于显式或隐式事务中。

使用一条 `CREATE DATABASE` 语句即可创建数据库以及存储该数据库的文件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过使用以下步骤实现 CREATE DATABASE 语句：

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 [model 数据库](../../relational-databases/databases/model-database.md)的副本初始化该数据库及其元数据。
2. 为数据库分配 Service Broker GUID。
3. 然后，[!INCLUDE[ssDE](../../includes/ssde-md.md)]使用空页填充数据库的剩余部分，包含记录数据库中空间使用情况的内部数据页除外。

在一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例中最多可以指定 32,767 个数据库。

每个数据库都有一个所有者，它可以在数据库中执行特殊操作。 所有者是创建数据库的用户。 数据库所有者可使用 [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) 方法进行更改。

某些数据库功能依赖文件系统中存在的功能来实现数据库的完整功能。 依赖于文件系统功能集的功能的一些示例包括：

- DBCC CHECKDB
- FileStream
- 使用 VSS 和文件快照的联机备份
- 数据库快照创建
- 内存优化数据文件组

## <a name="database-files-and-filegroups"></a>数据库文件和文件组

每个数据库至少有两个文件（一个主文件和一个事务日志文件）和一个文件组 。 可以为每个数据库指定最多 32,767 个文件和 32,767 个文件组。

在创建数据库时，请根据数据库中预期的最大数据量，创建尽可能大的数据文件。

建议使用存储区域网络 (SAN)、基于 iSCSI 的网络或本地附加的磁盘来存储 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库文件，因为该配置使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的性能和可靠性得到了优化。

## <a name="database-snapshots"></a>数据库快照

可以使用 `CREATE DATABASE` 语句创建源数据库的只读静态视图（数据库快照） 。 当创建快照时，源数据库已存在，所以数据库快照在事务上与源数据库一致。 源数据库可以具有多个快照。

> [!NOTE]
> 创建数据库快照时，`CREATE DATABASE` 语句不能引用日志文件、脱机文件、还原文件和不存在的文件。

如果创建数据库快照失败，快照便成为可疑快照，必须将其删除。 有关详细信息，请参阅 [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)。

每个快照都将一直存在，直到使用 `DROP DATABASE` 将其删除为止。

有关详细信息，请参阅[数据库快照](../../relational-databases/databases/database-snapshots-sql-server.md)。

## <a name="database-options"></a>数据库选项

创建数据库时，总会自动设置几个数据库选项。 有关这些选项的列表，请参阅 [ALTER DATABASE SET 选项](../../t-sql/statements/alter-database-transact-sql-set-options.md)。

## <a name="the-model-database-and-creating-new-databases"></a>model 数据库和创建新数据库

[model 数据库](../../relational-databases/databases/model-database.md)中的所有用户定义对象都会复制到所有新创建的数据库中。 可以向 model 数据库中添加任何对象（例如表、视图、存储过程、数据类型等），以将这些对象包括到所有新建数据库中。

指定 `CREATE DATABASE <database_name>` 语句而不带其他大小参数时，主数据文件与模型数据库中的主文件具有相同的大小。

除非指定了 `FOR ATTACH`，否则每个新数据库都从模型数据库继承数据库选项设置。 例如，在 model 数据库和任何新建数据库中，数据库选项 auto shrink 都设置为 **true**。 如果更改了 model 数据库中的选项，则这些新选项设置也将用于您所创建的所有新数据库中。 在 model 数据库中的更改操作不会影响现有数据库。 如果在 CREATE DATABASE 语句中指定了 FOR ATTACH，则新数据库将继承原始数据库的数据库选项设置。

## <a name="viewing-database-information"></a>查看数据库信息

可以使用目录视图、系统函数和系统存储过程返回有关数据库、文件和文件组的信息。 有关详细信息，请参阅[系统视图](../language-reference.md)。

## <a name="permissions"></a>权限

需要 `CREATE DATABASE`、`CREATE ANY DATABASE` 或 `ALTER ANY DATABASE` 权限。

为了控制对运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的计算机上的磁盘使用，通常只有少数登录帐户才有创建数据库的权限。

以下示例向数据库用户 Fay 提供创建数据库的权限。

```sql
USE master;
GO
GRANT CREATE DATABASE TO [Fay];
GO
```

### <a name="permissions-on-data-and-log-files"></a>对数据文件和日志文件的权限

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，会对每个数据库的数据文件和日志文件设置特定的权限。 每当对数据库执行下列操作时，便会设置下列权限：

- 附加
- 备份
- 创建
- 分离
- 修改以添加新文件
- 还原

如果这些文件位于具有打开权限的目录中，那么以上权限可以防止文件被意外篡改。

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] 不设置数据文件和日志文件权限。

## <a name="examples"></a>示例

### <a name="a-creating-a-database-without-specifying-files"></a>A. 创建未指定文件的数据库

以下示例创建名为 `mytest` 的数据库，并创建相应的主文件和事务日志文件。 因为语句没有 \<filespec> 项，所以主数据库文件的大小为 model 数据库主文件的大小。 事务日志设置为下面这些值中较大的一个：512KB 或主数据文件大小的 25%。 因为没有指定 MAXSIZE，文件可以增大到填满所有可用的磁盘空间为止。 此示例演示如何在创建 `mytest` 数据库之前删除名为 `mytest` 的数据库（如果它存在）。

```sql
USE master;
GO
IF DB_ID (N'mytest') IS NOT NULL
DROP DATABASE mytest;
GO
CREATE DATABASE mytest;
GO
-- Verify the database files and sizes
SELECT name, size, size*1.0/128 AS [Size in MBs]
FROM sys.master_files
WHERE name = N'mytest';
GO
```

### <a name="b-creating-a-database-that-specifies-the-data-and-transaction-log-files"></a>B. 创建指定数据和事务日志文件的数据库

以下示例会创建数据库 `Sales`。 由于未使用关键字 PRIMARY，因此第一个文件 (`Sales_dat`) 将成为主文件。 因为在 `Sales_dat` 文件的 SIZE 参数中没有指定 MB 或 KB，将使用 MB 并按 MB 分配。 `Sales_log` 文件以 MB 为单位进行分配，因为 `SIZE` 参数中显式声明了 `MB` 后缀。

```sql
USE master;
GO
CREATE DATABASE Sales
ON
( NAME = Sales_dat,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\saledat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="c-creating-a-database-by-specifying-multiple-data-and-transaction-log-files"></a>C. 通过指定多个数据和事务日志文件创建数据库

以下示例创建数据库 `Archive`，该数据库具有三个 `100-MB` 数据文件和两个 `100-MB` 事务日志文件。 主文件是列表中的第一个文件，并使用 `PRIMARY` 关键字显式指定。 事务日志文件在 `LOG ON` 关键字后指定。 请注意用于 `FILENAME` 选项中各文件的扩展名：`.mdf` 用于主数据文件，`.ndf` 用于辅助数据文件，`.ldf` 用于事务日志文件。 此示例将数据库放置于 `D:` 驱动器上，而非 `master` 数据库中。

```sql
USE master;
GO
CREATE DATABASE Archive
ON
PRIMARY
    (NAME = Arch1,
    FILENAME = 'D:\SalesData\archdat1.mdf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch2,
    FILENAME = 'D:\SalesData\archdat2.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch3,
    FILENAME = 'D:\SalesData\archdat3.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20)
LOG ON
  (NAME = Archlog1,
    FILENAME = 'D:\SalesData\archlog1.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
  (NAME = Archlog2,
    FILENAME = 'D:\SalesData\archlog2.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20) ;
GO
```

### <a name="d-creating-a-database-that-has-filegroups"></a>D. 创建具有文件组的数据库

以下示例创建数据库 `Sales`，该数据库具有以下文件组：

- 包含文件 `Spri1_dat` 和 `Spri2_dat` 的主文件组。 将这些文件的 FILEGROWTH 增量指定为 `15%`。
- 名为 `SalesGroup1` 的文件组，其中包含文件 `SGrp1Fi1` 和 `SGrp1Fi2`。
- 名为 `SalesGroup2` 的文件组，其中包含文件 `SGrp2Fi1` 和 `SGrp2Fi2`。

此示例将数据和日志文件放置于不同的磁盘上，以便提高性能。

```sql
USE master;
GO
CREATE DATABASE Sales
ON PRIMARY
( NAME = SPri1_dat,
    FILENAME = 'D:\SalesData\SPri1dat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
( NAME = SPri2_dat,
    FILENAME = 'D:\SalesData\SPri2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
FILEGROUP SalesGroup1
( NAME = SGrp1Fi1_dat,
    FILENAME = 'D:\SalesData\SG1Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp1Fi2_dat,
    FILENAME = 'D:\SalesData\SG1Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
FILEGROUP SalesGroup2
( NAME = SGrp2Fi1_dat,
    FILENAME = 'D:\SalesData\SG2Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp2Fi2_dat,
    FILENAME = 'D:\SalesData\SG2Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'E:\SalesLog\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="e-attaching-a-database"></a>E. 附加数据库

以下示例分离在示例 D 中创建的数据库 `Archive`，然后使用 `FOR ATTACH` 子句附加该数据库。 `Archive` 定义为具有多个数据和日志文件。 但是，由于文件的位置自创建后没有发生更改，所以只需在 `FOR ATTACH` 子句中指定主文件。 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，要附加的数据库中包含的所有全文文件也将随数据库一起附加。

```sql
USE master;
GO
sp_detach_db Archive;
GO
CREATE DATABASE Archive
  ON (FILENAME = 'D:\SalesData\archdat1.mdf')
  FOR ATTACH ;
GO
```

### <a name="f-creating-a-database-snapshot"></a>F. 创建数据库快照

以下示例会创建数据库快照 `sales_snapshot0600`。 由于数据库快照是只读的，所以不能指定日志文件。 为了符合语法要求，指定了源数据库中的每个文件，但没有指定文件组。

该示例的源数据库是在示例 D 中创建的 `Sales` 数据库。

```sql
USE master;
GO
CREATE DATABASE sales_snapshot0600 ON
    ( NAME = SPri1_dat, FILENAME = 'D:\SalesData\SPri1dat_0600.ss'),
    ( NAME = SPri2_dat, FILENAME = 'D:\SalesData\SPri2dt_0600.ss'),
    ( NAME = SGrp1Fi1_dat, FILENAME = 'D:\SalesData\SG1Fi1dt_0600.ss'),
    ( NAME = SGrp1Fi2_dat, FILENAME = 'D:\SalesData\SG1Fi2dt_0600.ss'),
    ( NAME = SGrp2Fi1_dat, FILENAME = 'D:\SalesData\SG2Fi1dt_0600.ss'),
    ( NAME = SGrp2Fi2_dat, FILENAME = 'D:\SalesData\SG2Fi2dt_0600.ss')
AS SNAPSHOT OF Sales ;
GO
```

### <a name="g-creating-a-database-and-specifying-a-collation-name-and-options"></a>G. 创建数据库并指定排序规则名称和选项

以下示例会创建数据库 `MyOptionsTest`。 指定了排序规则名称，并将 `TRUSTYWORTHY` 和 `DB_CHAINING` 选项设置为 `ON`。

```sql
USE master;
GO
IF DB_ID (N'MyOptionsTest') IS NOT NULL
DROP DATABASE MyOptionsTest;
GO
CREATE DATABASE MyOptionsTest
COLLATE French_CI_AI
WITH TRUSTWORTHY ON, DB_CHAINING ON;
GO
--Verifying collation and option settings.
SELECT name, collation_name, is_trustworthy_on, is_db_chaining_on
FROM sys.databases
WHERE name = N'MyOptionsTest';
GO
```

### <a name="h-attaching-a-full-text-catalog-that-has-been-moved"></a>H. 附加已移动的全文目录

以下示例同时附加全文目录 `AdvWksFtCat` 以及 `AdventureWorks2012` 数据和日志文件。 在该示例中，将全文目录从其默认位置移动到新位置 `c:\myFTCatalogs`。 数据和日志文件保留在其默认位置。

```sql
USE master;
GO
--Detach the AdventureWorks2012 database
sp_detach_db AdventureWorks2012;
GO
-- Physically move the full text catalog to the new location.
--Attach the AdventureWorks2012 database and specify the new location of the full-text catalog.
CREATE DATABASE AdventureWorks2012 ON
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_data.mdf'),
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_log.ldf'),
    (FILENAME = 'c:\myFTCatalogs\AdvWksFtCat')
FOR ATTACH;
GO
```

### <a name="i-creating-a-database-that-specifies-a-row-filegroup-and-two-filestream-filegroups"></a>I. 创建指定一个行文件组和两个 FILESTREAM 文件组的数据库

下面的示例将创建数据库 `FileStreamDB`。 该数据库在创建之时包含一个行文件组和两个 FILESTREAM 文件组。 每个文件组都包含一个文件：

- `FileStreamDB_data` 包含行数据。 它包含一个文件，即带有默认路径的 `FileStreamDB_data.mdf`。
- `FileStreamPhotos` 包含 FILESTREAM 数据。 它包含两个 FILESTREAM 数据容器，`FSPhotos`（位于 `C:\MyFSfolder\Photos`）和 `FSPhotos2`（位于 `D:\MyFSfolder\Photos`）。 它被标记为默认 FILESTREAM 文件组。
- `FileStreamResumes` 包含 FILESTREAM 数据。 它包含一个位于`C:\MyFSfolder\Resumes`中的 FILESTREAM 数据容器， `FSResumes` 。

```sql
USE master;
GO
-- Get the SQL Server data path.
DECLARE @data_path nvarchar(256);
SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

 -- Execute the CREATE DATABASE statement.
EXECUTE ('CREATE DATABASE FileStreamDB
ON PRIMARY
    (
    NAME = FileStreamDB_data
    ,FILENAME = ''' + @data_path + 'FileStreamDB_data.mdf''
    ,SIZE = 10MB
    ,MAXSIZE = 50MB
    ,FILEGROWTH = 15%
    ),
FILEGROUP FileStreamPhotos CONTAINS FILESTREAM DEFAULT
    (
    NAME = FSPhotos
    ,FILENAME = ''C:\MyFSfolder\Photos''
-- SIZE and FILEGROWTH should not be specified here.
-- If they are specified an error will be raised.
, MAXSIZE = 5000 MB
    ),
    (
      NAME = FSPhotos2
      , FILENAME = ''D:\MyFSfolder\Photos''
      , MAXSIZE = 10000 MB
     ),
FILEGROUP FileStreamResumes CONTAINS FILESTREAM
    (
    NAME = FileStreamResumes
    ,FILENAME = ''C:\MyFSfolder\Resumes''
    )
LOG ON
    (
    NAME = FileStream_log
    ,FILENAME = ''' + @data_path + 'FileStreamDB_log.ldf''
    ,SIZE = 5MB
    ,MAXSIZE = 25MB
    ,FILEGROWTH = 5MB
    )'
);
GO
```

### <a name="j-creating-a-database-that-has-a-filestream-filegroup-with-multiple-files"></a>J. 创建一个数据库，该数据库具有含多个文件的 FILESTREAM 文件组

下面的示例将创建数据库 `BlobStore1`。 该数据库在创建之时包含一个行文件组和一个 FILESTREAM 文件组 `FS`。 该 FILESTREAM 文件组包含两个文件：`FS1` 和 `FS2`。 然后，通过将第三个文件 `FS3` 添加到 FILESTREAM 文件组来改变该数据库。

```sql
USE master;
GO

CREATE DATABASE [BlobStore1]
CONTAINMENT = NONE
ON PRIMARY
(
    NAME = N'BlobStore1',
    FILENAME = N'C:\BlobStore\BlobStore1.mdf',
    SIZE = 100MB,
    MAXSIZE = UNLIMITED,
    FILEGROWTH = 1MB
),
FILEGROUP [FS] CONTAINS FILESTREAM DEFAULT
(  
    NAME = N'FS1',
    FILENAME = N'C:\BlobStore\FS1',
    MAXSIZE = UNLIMITED
),
(
    NAME = N'FS2',
    FILENAME = N'C:\BlobStore\FS2',
    MAXSIZE = 100MB
)
LOG ON
(
    NAME = N'BlobStore1_log',
    FILENAME = N'C:\BlobStore\BlobStore1_log.ldf',
    SIZE = 100MB,
    MAXSIZE = 1GB,
    FILEGROWTH = 1MB
);
GO

ALTER DATABASE [BlobStore1]
ADD FILE
(
    NAME = N'FS3',
    FILENAME = N'C:\BlobStore\FS3',
    MAXSIZE = 100MB
)
TO FILEGROUP [FS];
GO
```

## <a name="see-also"></a>另请参阅

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [数据库分离和附加](../../relational-databases/databases/database-detach-and-attach-sql-server.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)
- [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)
- [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)
- [数据库快照](../../relational-databases/databases/database-snapshots-sql-server.md)
- [移动数据库文件](../../relational-databases/databases/move-database-files.md)
- [数据库](../../relational-databases/databases/databases.md)
- [二进制大型对象 - Blob 数据](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        \* SQL 数据库 \*
    :::column-end:::
    :::column:::
        [SQL 托管实例](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL 数据库

## <a name="overview"></a>概述

在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，此语句可与 Azure SQL 服务器一起使用，从而创建单一数据库或在弹性池中创建数据库。 使用此语句，可以指定数据库名称、排序规则、最大大小、版本、服务目标以及新数据库的弹性池（如果适用）。 它还可用于在弹性池中创建数据库。 此外，它还可用于在其他 SQL 数据库服务器上创建数据库的副本。

## <a name="syntax"></a>语法

### <a name="create-a-database"></a>创建数据库

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
{
  (<edition_options> [, ...n])
}
[ WITH <with_options> [,..n]]
[;]

<with_options> ::=
{
  CATALOG_COLLATION = { DATABASE_DEFAULT | SQL_Latin1_General_CP1_CI_AS }
  | BACKUP_STORAGE_REDUNDANCY = { 'LOCAL' | 'ZONE' | 'GEO' }
}

<edition_options> ::=
{

  MAXSIZE = { 100 MB | 500 MB | 1 ... 1024 ... 4096 GB }
  | ( EDITION = { 'Basic' | 'Standard' | 'Premium' | 'GeneralPurpose' | 'BusinessCritical' | 'Hyperscale' }
  | SERVICE_OBJECTIVE =
    { 'Basic' | 'S0' | 'S1' | 'S2' | 'S3' | 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
}
```

### <a name="copy-a-database"></a>复制数据库

```syntaxsql
CREATE DATABASE database_name
    AS COPY OF [source_server_name.] source_database_name
    [ ( SERVICE_OBJECTIVE =
      { 'Basic' |'S0' | 'S1' | 'S2' | 'S3'| 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
   ]
[;]
```

## <a name="arguments"></a>参数

database_name 新数据库的名称。 此名称在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上必须唯一，并且必须符合 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的标识符规则。 有关详细信息，请参阅[标识符](../../relational-databases/databases/database-identifiers.md)。

Collation_name 指定数据库的默认排序规则。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 如果未指定，则会为数据库分配默认排序规则（即 SQL_Latin1_General_CP1_CI_AS）。

有关 Windows 和 SQL 排序规则名称的详细信息，请参阅 [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md)。

CATALOG_COLLATION 指定元数据目录的默认排序规则。 *DATABASE_DEFAULT* 指定用于系统视图和系统表的元数据目录按数据库的默认排序规则进行整理。 这是在 SQL Server 中所发现的行为。

*SQL_Latin1_General_CP1_CI_AS* 指定用于系统视图和表的元数据目录按固定的 SQL_Latin1_General_CP1_CI_AS 排序规则进行整理。 如果未指定，这将是 Azure SQL 数据库上的默认设置。

BACKUP_STORAGE_REDUNDANCY 指定如何复制数据库的时间点还原备份和长期保留备份。 仅当使用“异地”备份存储冗余创建数据库时，“异地还原”或“从区域中断恢复”功能才可用。 除非显式指定，否则通过 T-SQL 创建的数据库将使用异地冗余备份存储。 

> [!IMPORTANT]
> Azure SQL 数据库的 BACKUP_STORAGE_REDUNDANCY 选项已在巴西南部提供公共预览版，且仅在 Azure 东南亚地区正式发布。  

EDITION 指定数据库的服务层。

单一数据库和共用数据库。 可用值有：'Basic'、'Standard'、'Premium'、'GeneralPurpose'、'BusinessCritical' 和 'Hyperscale'。

MAXSIZE 指定数据库的最大大小。 MAXSIZE 必须对指定 EDITION（服务层）有效。下面是服务层支持的 MAXSIZE 值和默认值 (D)。

> [!NOTE]
> MAXSIZE 参数不适用于超大规模服务层中的单一数据库。 超大规模层数据库根据需要而增长，最大 100 TB。 SQL 数据库服务会自动添加存储空间，而无需设置最大大小。

**SQL 数据库服务器上适用于单一数据库和共用数据库的 DTU 模型**

|**MAXSIZE**|**基本**|**S0-S2**|**S3-S12**|**P1-P6**| **P11-P15** |
|:---|:---|:---|:---|:---|:---|
|100 MB|√|√|√|√|√|
|500 MB|√|√|√|√|√|
|1 GB|√|√|√|√|√|
|2 GB|√ (D)|√|√|√|√|
|5 GB|空值|√|√|√|√|
|10 GB|空值|√|√|√|√|
|20 GB|空值|√|√|√|√|
|30 GB|空值|√|√|√|√|
|40 GB|空值|√|√|√|√|
|50 GB|空值|√|√|√|√|
|100 GB|空值|√|√|√|√|
|150 GB|空值|√|√|√|√|
|200 GB|空值|√|√|√|√|
|250 GB|空值|√ (D)|√ (D)|√|√|
|300 GB|空值|空值|√|√|√|
|400 GB|空值|空值|√|√|√|
|500 GB|空值|空值|√|√ (D)|√|
|750 GB|空值|空值|√|√|√|
|1024 GB|空值|空值|√|√|√ (D)|
|从 1024 GB 到最大 4096 GB，增量为 256 GB* |空值|空值|空值|不适用|√|√|

\* P11 和 P15 允许 MAXSIZE 达到 4 TB，默认大小为 1024 GB。 P11 和 P15 可以使用最大 4 TB 的内含存储，且无需额外费用。 在高级层中，目前在以下区域提供大于 1 TB 的 MAXSIZE：美国东部 2、美国西部、US Gov 弗吉尼亚州、西欧、德国中部、东南亚、日本东部、澳大利亚东部、加拿大中部和加拿大东部。 有关 DTU 模型资源限制的其他详细信息，请参阅 [DTU 资源限制](/azure/sql-database/sql-database-dtu-resource-limits)。

DTU 模型的 MAXSIZE 值（如果指定）必须为上表中所示的指定服务层的有效值。

**vCore 模型**

**常规用途 - 预配的计算 - Gen4 （第 1 部分）**

|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6|
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|最大数据大小 (GB)|1024|1024|1024|1536|1536|1536|

**常规用途 - 预配的计算 - Gen4（第 2 部分）**

|MAXSIZE|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|最大数据大小 (GB)|1536|3072|3072|3072|4096|4096|

**常规用途 - 预配的计算 - Gen5（第 1 部分）**

|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|最大数据大小 (GB)|1024|1024|1024|1536|1536|1536|1536|

**常规用途 - 预配的计算 - Gen5（第 2 部分）**

|MAXSIZE|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|最大数据大小 (GB)|3072|3072|3072|4096|4096|4096|4096|

**常规用途 - 预配的计算 - Fsv2 系列（第 1 部分）**

|MAXSIZE|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14|GP_Fsv2_16|GP_Fsv2_18|
|:----- | ------: | ------: | ------: | ------: | ------: | ------: |
|最大数据大小 (GB)|1024|1024|1024|1024|1536|1536|

**常规用途 - 预配的计算 - Fsv2 系列（第 2 部分）**

|MAXSIZE|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32|GP_Fsv2_36|GP_Fsv2_72|
|:----- | ------: | ------: | ------: | ------: | ------: |
|最大数据大小 (GB)|1536|1536|3072|3072|4096|

**常规用途 - 无服务器计算 - Gen5（第 1 部分）**

|MAXSIZE|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:----- | ------: |-------: |-------: |-------: |--------: |
|最大 vCore 数|1|2|4|6|8|

**常规用途 - 无服务器计算 - Gen5（第 2 部分）**

|MAXSIZE|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:----- | ------: |-------: |-------: |-------: |
|最大 vCore 数|10|12|14|16|

**常规用途 - 无服务器计算 - Gen5（第 3 部分）**

|MAXSIZE|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:----- | ------: |-------: |-------: |-------: |--------: |
|最大 vCore 数|18|20|24|32|40|

**业务关键 - 预配的计算 - Gen4（第 1 部分）**

|计算大小（服务目标）|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--------------- | ------: |-------: |-------: |-------: |-------: |-------: |
|最大数据大小 (GB)|1024|1024|1024|1024|1024|1024|

**业务关键 - 预配的计算 - Gen4（第 2 部分）**

|计算大小（服务目标）|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--------------- | ------: |-------: |-------: |--------: |--------: |--------: |
|最大数据大小 (GB)|1024|1024|1024|1024|1024|1024|

**业务关键 - 预配的计算 - Gen5（第 1 部分）**

|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |---------: |--------:|--------: |
|最大数据大小 (GB)|1024|1024|1024|1536|1536|1536|1536|

**业务关键 - 预配的计算 - Gen5（第 2 部分）**

|MAXSIZE|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:----- | -------: |--------: |--------: |--------: |--------: |---------:|--------: |
|最大数据大小 (GB)|3072|3072|3072|4096|4096|4096|4096|

**业务关键 - 预配的计算 - M 系列（第 1 部分）**

|MAXSIZE|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:----- | -------: | -------: | -------: | -------: | -------: | -------: |
|最大数据大小 (GB)|512|640|768|896|1024|1152|

**业务关键 - 预配的计算 - M 系列（第 2 部分）**

|MAXSIZE|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:----- | -------: | -------: | -------: | -------: | -------: |
|最大数据大小 (GB)|1280|1536|2048|4096|4096|

如果在使用 vCore 模型时未设置 `MAXSIZE` 值，默认为 32GB。 有关 vCore 模型资源限制的其他详细信息，请参阅 [vCore 资源限制](/azure/sql-database/sql-database-dtu-resource-limits)。

以下规则适用于 MAXSIZE 和 EDITION 参数：

- 如果指定了 EDITION 但未指定 MAXSIZE，则使用版本的默认值。 例如，如果 EDITION 设置为 Standard 并且未指定 MAXSIZE，则 MAXSIZE 将自动设置为 250 MB。
- 如果 MAXSIZE 和 EDITION 均未指定，则 EDITION 设置为 `GeneralPurpose`，MAXSIZE 设置为 32 GB。

SERVICE_OBJECTIVE

- 针对单一数据库和入池数据库

  - 指定计算大小（服务目标）。 服务目标的可用值包括：`S0`、`S1`、`S2`、`S3`、`S4`、`S6`、`S7`、`S9`、`S12`、`P1`、`P2`、`P4`、`P6`、`P11`、`P15`、`GP_GEN4_1`、`GP_GEN4_2`、`GP_GEN4_3`、`GP_GEN4_4`、`GP_GEN4_5`、`GP_GEN4_6`、`GP_GEN4_7`、`GP_GEN4_8`、`GP_GEN4_7`、`GP_GEN4_8`、`GP_GEN4_9`、`GP_GEN4_10`、`GP_GEN4_16`、`GP_GEN4_24`、`BC_GEN4_1`、`BC_GEN4_2`、`BC_GEN4_3`、`BC_GEN4_4`、`BC_GEN4_5`、`BC_GEN4_6`、`BC_GEN4_7`、`BC_GEN4_8`、`BC_GEN4_9`、`BC_GEN4_10`、`BC_GEN4_16`、`BC_GEN4_24`、`GP_Gen5_2`、`GP_Gen5_4`、`GP_Gen5_6`、`GP_Gen5_8`、`GP_Gen5_10`、`GP_Gen5_12`、`GP_Gen5_14`、`GP_Gen5_16`、`GP_Gen5_18`、`GP_Gen5_20`、`GP_Gen5_24`、`GP_Gen5_32`、`GP_Gen5_40`、`GP_Gen5_80`、`GP_Fsv2_8`、`GP_Fsv2_10`、`GP_Fsv2_12`、`GP_Fsv2_14`、`GP_Fsv2_16`、`GP_Fsv2_18`、`GP_Fsv2_20`、`GP_Fsv2_24`、`GP_Fsv2_32`、`GP_Fsv2_36`、`GP_Fsv2_72`、`BC_Gen5_2`、`BC_Gen5_4`、`BC_Gen5_6`、`BC_Gen5_8`、`BC_Gen5_10`、`BC_Gen5_12`、`BC_Gen5_14`、`BC_Gen5_16`、`BC_Gen5_18`、`BC_Gen5_20`、`BC_Gen5_24`、`BC_Gen5_32`、`BC_Gen5_40`、`BC_Gen5_80`、`BC_M_8`、`BC_M_10`、`BC_M_12`、`BC_M_14`、`BC_M_16`、`BC_M_18`、`BC_M_20`、`BC_M_24`、`BC_M_32`、`BC_M_64`、`BC_M_128`。

- **对于无服务器数据库**
- 
  - 指定计算大小（服务目标）。 服务目标的可用值包括：`GP_S_Gen5_1`、`GP_S_Gen5_2`、`GP_S_Gen5_4`、`GP_S_Gen5_6`、`GP_S_Gen5_8`、`GP_S_Gen5_10`、`GP_S_Gen5_12`、`GP_S_Gen5_14`、`GP_S_Gen5_16`、`GP_S_Gen5_18`、`GP_S_Gen5_20`、`GP_S_Gen5_24`、`GP_S_Gen5_32`、`GP_S_Gen5_40`。

- **针对超大规模服务层中的单一数据库**

  - 指定计算大小（服务目标）。 服务目标的可用值包括：`HS_GEN4_1` `HS_GEN4_2` `HS_GEN4_4` `HS_GEN4_8` `HS_GEN4_16`、`HS_GEN4_24`、`HS_Gen5_2`、`HS_Gen5_4`、`HS_Gen5_8`、`HS_Gen5_16`、`HS_Gen5_24`、`HS_Gen5_32`、`HS_Gen5_48`、`HS_Gen5_80`。

有关服务目标说明以及大小、版本和服务目标组合的详细信息，请参阅 [Azure SQL 数据库服务层](/azure/sql-database/sql-database-service-tiers)。 如果 EDITION 不支持指定的 SERVICE_OBJECTIVE，你会收到一个错误。 若要将 SERVICE_OBJECTIVE 值从一层更改为另一层（例如从 S1 更改为 P1），还必须更改 EDITION 值。 有关服务目标说明以及大小、版本和服务目标组合的详细信息，请参阅 [Azure SQL 数据库服务层和性能级别](/azure/azure-sql/database/purchasing-models)、[DTU 资源限制](/azure/sql-database/sql-database-dtu-resource-limits)和 [vCore 资源限制](/azure/sql-database/sql-database-dtu-resource-limits)。 删除了对 PRS 服务目标的支持。 如有问题，请使用此电子邮件别名：premium-rs@microsoft.com。

ELASTIC_POOL (name = \<elastic_pool_name>) 适用于：仅限单一数据库和池化数据库。 不适用于超大规模服务层中的数据库。
要在弹性数据库池中创建新数据库，请将数据库的 SERVICE_OBJECTIVE 设置为 ELASTIC_POOL，并提供池的名称。 有关详细信息，请参阅[弹性池有助于管理和缩放多个 Azure SQL 数据库](/azure/azure-sql/database/elastic-pool-overview)。

AS COPY OF [source_server_name.]source_database_name **适用于：** 仅限单一数据库和池化数据库。
将数据库复制到同一台或其他 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 服务器上。

source_server_name 源数据库所在的 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 服务器的名称。 当源数据库和目标数据库位于同一台 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 服务器上时，此参数是可选的。

> [!NOTE]
> `AS COPY OF` 参数不支持完全限定的唯一域名。 换言之，如果您的服务器的完全限定域名是 `serverName.database.windows.net`，则在数据库复制期间仅使用 `serverName`。

*source_database_name*

要复制的数据库的名称。

## <a name="remarks"></a>备注

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中的数据库具有多个在创建数据库时设置的默认设置。 有关这些默认设置的详细信息，请参阅 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 中的值列表。

`MAXSIZE` 提供限制数据库大小的功能。 如果数据库的大小达到其 `MAXSIZE`，你将收到错误代码 40544。 如果发生这种情况，您不能插入或更新数据或创建新的对象（如表、存储过程、视图和函数）。 不过，您仍可以读取和删除数据、截断表、删除表和索引以及重新建立索引。 然后，可以将 `MAXSIZE` 更新为比当前数据库大小更大的值，或者删除一些数据以释放存储空间。 在您可以插入新数据之前，可能有长达十五分钟的延迟。

若要在以后更改大小、版本或服务目标值，请使用 [ALTER DATABASE - Azure SQL 数据库](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)。

`CATALOG_COLLATION` 参数仅在数据库创建期间可用。

## <a name="database-copies"></a>数据库复制

**适用于：** 仅限单一数据库和池化数据库。

使用 `CREATE DATABASE` 语句复制数据库是一个异步操作。 因此，在整个复制过程中，不需要与 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 服务器建立连接。 `CREATE DATABASE` 语句会在 sys.databases 中的条目创建之后，但是在数据库复制操作完成之前将控制权返还给用户。 换言之，当数据库复制仍在进行时，`CREATE DATABASE` 语句会成功返回。

- 在 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] 服务器上监视复制进程：在 [dm_database_copies](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md) 中查询 `percentage_complete` 或 `replication_state_desc` 列，或在 sys.databases 视图中查询 `state` 列。 可以使用 [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) 视图，它还会返回数据库操作（包括数据库复制）的状态。

在复制过程成功完成后，目标数据库与源数据库在事务上是一致的。

以下语法和语义规则应用于您对 `AS COPY OF` 参数的使用：

- 源服务器名称与复制目标的服务器名称可能相同，也可能不同。 如果相同，此参数是可选的，默认情况下将使用当前会话的服务器上下文。
- 必须指定源数据库名称和目标数据库名称，并且这些名称必须唯一且符合 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符规则。 有关详细信息，请参阅[标识符](../../relational-databases/databases/database-identifiers.md)。
- 必须在将创建新数据库的 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 服务器的 master 数据库的上下文中执行 `CREATE DATABASE` 语句。
- 在复制完成之后，必须将目标数据库作为独立的数据库进行管理。 您可以独立于源数据库，针对新数据库执行 `ALTER DATABASE` 和 `DROP DATABASE` 语句。 您还可以将新数据库复制到另一个新数据库。
- 当正在进行数据库复制时，可以继续访问源数据库。

有关详细信息，请参阅[使用 TRANSACT-SQL 创建 Azure SQL 数据库的副本](/azure/azure-sql/database/database-copy)。

> [!IMPORTANT]
> 默认使用与源数据库相同的备份存储冗余创建数据库副本。 不支持在通过 T-SQL 创建数据库副本时更改备份存储冗余。 


## <a name="permissions"></a>权限

要创建数据库，登录名必须为下列各项之一：

- 服务器级别主体登录名
- 本地 Azure SQL Server 的 Azure AD 管理员
- 登录名为 `dbmanager` 数据库角色的成员

**使用 `CREATE DATABASE ... AS COPY OF` 语法的其他要求：** 在本地服务器上执行语句的登录名还必须至少是源服务器上的 `db_owner`。 如果登录名基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，那么在本地服务器上执行语句的登录名必须在源 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 服务器上具有匹配的登录名，名称和密码均完全相同。

## <a name="examples"></a>示例

### <a name="simple-example"></a>简单示例

创建数据库的简单示例。

```sql
CREATE DATABASE TestDB1;
```

### <a name="simple-example-with-edition"></a>版本的简单示例

创建常规用途数据库的简单示例。

```sql
CREATE DATABASE TestDB2
( EDITION = 'GeneralPurpose' );
```

### <a name="example-with-additional-options"></a>其他选项的示例

使用多个选项的示例。

```sql
CREATE DATABASE hito
COLLATE Japanese_Bushu_Kakusu_100_CS_AS_KS_WS
( MAXSIZE = 500 MB, EDITION = 'GeneralPurpose', SERVICE_OBJECTIVE = 'GP_GEN4_8' ) ;
```

### <a name="creating-a-copy"></a>创建副本

创建数据库副本的示例。

**适用于：** 仅限单一数据库和池化数据库。

```sql
CREATE DATABASE escuela
AS COPY OF school;
```

### <a name="creating-a-database-in-an-elastic-pool"></a>在弹性池中创建数据库

在名为 S3M100 的池中创建新数据库：

**适用于：** 仅限单一数据库和池化数据库。

```sql
CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = S3M100 ) ) ;
```

### <a name="creating-a-copy-of-a-database-on-another-server"></a>在其他服务器上创建数据库副本

下面的示例创建 db_original 数据库的副本，将其名为 db_copy，保存在针对单个数据库的 P2 计算大小（服务对象）中。 无论 db_original 是位于弹性池中还是采用针对单个数据库的计算大小（服务对象），都是如此。

**适用于：** 仅限单一数据库和池化数据库。

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original ( SERVICE_OBJECTIVE = 'P2' );
```

下面的示例在名为 ep1 的弹性池中创建名为 db_copy 的 db_original 数据库的副本。 无论 db_original 是位于弹性池中还是采用针对单个数据库的计算大小（服务对象），都是如此。 如果 db_original 位于具有不同名称的弹性池中，那么仍将在 ep1 中创建 db_copy。

**适用于：** 仅限单一数据库和池化数据库。

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original
  (SERVICE_OBJECTIVE = ELASTIC_POOL( name = ep1 ) ) ;
```

### <a name="create-database-with-specified-catalog-collation-value"></a>使用指定的目录排序规则值创建数据库

下面的示例在数据库创建期间将目录排序规则设置为 DATABASE_DEFAULT，其会将目录排序规则设置为与数据库排序规则相同。

```sql
CREATE DATABASE TestDB3 COLLATE Japanese_XJIS_140 (MAXSIZE = 100 MB, EDITION = 'Basic')
  WITH CATALOG_COLLATION = DATABASE_DEFAULT
```

### <a name="create-database-using-zone-redundancy-for-backups"></a>使用备份区域冗余创建数据库

下面的示例为数据库备份设置区域冗余。 时间点还原备份和长期保留备份（如果已配置）将使用相同的备份存储冗余。

```sql
CREATE DATABASE test_zone_redundancy 
  WITH BACKUP_STORAGE_REDUNDANCY = 'ZONE';
```

## <a name="see-also"></a>另请参阅

- [sys.dm_database_copies - Azure SQL 数据库](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)
- [ALTER DATABASE - Azure SQL 数据库](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL 托管实例 \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL 托管实例

## <a name="overview"></a>概述

在 Azure SQL 托管实例中，此语句用于创建数据库。 在托管实例上创建数据库时，可以指定数据库名称和排序规则。

## <a name="syntax"></a>语法

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;]
```

> [!IMPORTANT]
> 要在托管实例中为数据库添加文件或设置包含，请使用 [ALTER DATABASE](alter-database-transact-sql.md?tabs=sqldbmi) 语句。

## <a name="arguments"></a>参数

database_name 新数据库的名称。 此名称在 SQL 服务器上必须唯一，并且必须符合标识符的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 规则。 有关详细信息，请参阅[标识符](../../relational-databases/databases/database-identifiers.md)。

Collation_name 指定数据库的默认排序规则。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 如果未指定，则会为数据库分配默认排序规则（即 SQL_Latin1_General_CP1_CI_AS）。

有关 Windows 和 SQL 排序规则名称的详细信息，请参阅 [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md)。

## <a name="remarks"></a>备注

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中的数据库具有多个在创建数据库时设置的默认设置。 有关这些默认设置的详细信息，请参阅 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 中的值列表。

> [!IMPORTANT]
> `CREATE DATABASE` 语句必须是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理中的唯一语句。

以下是 `CREATE DATABASE` 限制：

- 无法定义文件和文件组。
- 不支持 `WITH` 选项。

  > [!TIP]
  > 解决方法是使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)。 在 `CREATE DATABASE` 之后设置数据库选项并添加文件。

## <a name="permissions"></a>权限

要创建数据库，登录名必须为下列各项之一：

- 服务器级别主体登录名
- 本地 Azure SQL Server 的 Azure AD 管理员
- 登录名为 `dbcreator` 数据库角色的成员

## <a name="examples"></a>示例

### <a name="simple-example"></a>简单示例

创建数据库的简单示例。

```sql
CREATE DATABASE TestDB1;
```

## <a name="see-also"></a>另请参阅

请参阅 [ALTER DATABASE](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="overview"></a>概述

在 Azure Synapse 中，此语句可与 Azure SQL 数据库服务器一起使用，以创建 SQL Analytics 数据库。 使用此语句，可以指定数据库名称、排序规则、最大大小、版本和服务目标。

## <a name="syntax"></a>语法

### <a name="dedicated-sql-pool"></a>[专用 SQL 池](#tab/sqlpool)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
(
    [ MAXSIZE = {
          250 | 500 | 750 | 1024 | 5120 | 10240 | 20480 | 30720
        | 40960 | 51200 | 61440 | 71680 | 81920 | 92160 | 102400
        | 153600 | 204800 | 245760
      } GB ,
    ]
    EDITION = 'datawarehouse',
    SERVICE_OBJECTIVE = {
         'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500' | 'DW600'
        | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000' | 'DW3000' | 'DW6000'
        |'DW100c' | 'DW200c' | 'DW300c' | 'DW400c' | 'DW500c'
        | 'DW1000c' | 'DW1500c' | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c'
        | 'DW6000c' | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
    }
)
[;]
```
### <a name="serverless-sql-pool"></a>[无服务器 SQL 池](#tab/sqlod)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;] 
```
---

## <a name="arguments"></a>参数

database_name 新数据库的名称。 此名称在 SQL Server 上必须是唯一的，它可托管 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 数据库和 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 数据库，且符合标识符的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 规则。 有关详细信息，请参阅[标识符](../../relational-databases/databases/database-identifiers.md)。

collation_name 指定数据库的默认排序规则。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 如果未指定，则会为数据库分配默认排序规则（即 SQL_Latin1_General_CP1_CI_AS）。

有关 Windows 和 SQL 排序规则名称的详细信息，请参阅 [COLLATE (Transact-SQL)](./collations.md)。

EDITION 指定数据库的服务层。 对于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]，使用“datawarehouse”。

MAXSIZE 默认为 245,760 GB (240 TB)。

**适用于：** 已针对计算代系 1 进行优化

允许的最大数据库大小。 数据库大小不能超出 MAXSIZE。

**适用于：** 已针对计算代系 2 进行优化

数据库中允许的最大行存储数据大小。 存储在行存储表中的数据、列存储索引的增量存储或非聚集索引（聚集在列存储索引上）都不可超过 MAXSIZE。压缩到列存储格式的数据没有大小限制，不受 MAXSIZE 约束。

SERVICE_OBJECTIVE 指定计算大小（服务目标）。 有关 Azure Synapse 服务目标的详细信息，请参阅[数据仓库单位 (DWU)](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu)。

## <a name="general-remarks"></a>一般备注

使用 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 查看数据库属性。

使用 [ALTER DATABASE - Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true) 在以后更改最大大小或服务目标值。

Azure Synapse 设置为 COMPATIBILITY_LEVEL 130，且不得更改。 有关详细信息，请参阅[在 Azure SQL 数据库中通过兼容性级别 130 优化查询性能](./alter-database-transact-sql-compatibility-level.md)。

## <a name="permissions"></a>权限

所需的权限：

- 服务器级别主体登录名（由预配进程创建），或者
- `dbmanager` 数据库角色的成员。

## <a name="error-handling"></a>错误处理

如果数据库的大小达到 MAXSIZE，你将收到错误代码 40544。 如果发生这种情况，你不能插入或更新数据或创建新的对象（如表、存储过程、视图和函数）。 不过，仍可以读取和删除数据、截断表、删除表和索引以及重新生成索引。 然后，您可以将 MAXSIZE 更新为比当前数据库大小更大的值，或者删除一些数据以释放存储空间。 在您可以插入新数据之前，可能有长达十五分钟的延迟。

## <a name="limitations-and-restrictions"></a>限制和局限

您必须连接到 master 数据库才能创建新的数据库。

`CREATE DATABASE` 语句必须是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理中的唯一语句。

创建数据库后，无法更改数据库排序规则。

## <a name="examples-sssdwfull"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

### <a name="a-simple-example"></a>A. 简单示例
创建数据仓库数据库的简单示例。 这将创建最小最大大小为 10240 GB、默认排序规则为 SQL_Latin1_General_CP1_CI_AS 且 最小计算功率为 DW100 的数据库。

```sql
CREATE DATABASE TestDW
(EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
```

### <a name="b-create-a-data-warehouse-database-with-all-the-options"></a>B. 创建具有所有选项的数据仓库数据库
创建使用所有选项的 10 TB 数据仓库的示例。

```sql
CREATE DATABASE TestDW COLLATE Latin1_General_100_CI_AS_KS_WS
(MAXSIZE = 10240 GB, EDITION = 'datawarehouse', SERVICE_OBJECTIVE = 'DW1000');
```

## <a name="see-also"></a>另请参阅

- [ALTER DATABASE- Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [CREATE TABLE- Azure Synapse Analytics](../../t-sql/statements/create-table-azure-sql-data-warehouse.md)
- [DROP DATABASE - Transact-SQL](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics Platform<br />System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>分析平台系统

## <a name="overview"></a>概述

在 Analytics Platform System 中，此语句用于在 Analytics Platform System 设备上创建新数据库。 使用该语句创建与设备数据库相关联的所有文件，并为数据库表和事务日志设置最大大小和自动增长选项。

## <a name="syntax"></a>语法

```syntaxsql
CREATE DATABASE database_name
WITH (
    [ AUTOGROW = ON | OFF , ]
    REPLICATED_SIZE = replicated_size [ GB ] ,
    DISTRIBUTED_SIZE = distributed_size [ GB ] ,
    LOG_SIZE = log_size [ GB ] )
[;]
```

## <a name="arguments"></a>参数

database_name 新数据库的名称。 有关允许的数据库名称的详细信息，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中的“对象命名规则”和“保留的数据库名称”。

AUTOGROW = ON | OFF 指定此数据库的 replicated_size、distributed_size 和 log_size 参数是否将根据需要自动增加到超出其指定大小。 默认值为 OFF。

如果 AUTOGROW 为 ON，则 replicated_size、distributed_size 和 log_size 将根据所需（不是在初始指定大小的块中），随需要比已分配存储更多存储的每次数据插入、更新或其他操作增加  。

如果 AUTOGROW 为 OFF，则大小不会自动增加。 当尝试执行需要将 replicated_size、distributed_size 或 log_size 增加到超过其指定值的操作时，[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 将返回错误  。

对于所有大小，AUTOGROW 要么都设置为 ON，要么都为 OFF。 例如，不可能对于 log_size 将 AUTOGROW 设置为 ON，对于 replicated_size，却不这样设置 。

replicated_size [ GB ] 一个正数。 为分配到复制表和每个 Compute 节点上的相应数据的总空间设置大小（整数或十进制 GB）。 有关 replicated_size 的最小和最大要求的信息，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中的“最小值和最大值”。

如果 AUTOGROW 为 ON，复制表允许增加到超出限制。

如果 AUTOGROW 为 OFF，那么当用户尝试创建新的复制表、将数据插入到现有复制表或以增加的大小可能会超过 replicated_size 的方式更新现有复制表时，将返回错误。

distributed_size [ GB ] 一个正数。 整个设备上分配给分布式表（以及相应数据）的总空间大小（整数或十进制 GB）。 有关 distributed_size 的最小和最大要求的信息，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中的“最小值和最大值”。

如果 AUTOGROW 为 ON，分布式表将可以增加到超出限制。

如果 AUTOGROW 为 OFF，那么当用户尝试创建新的分布式表、将数据插入到现有分布式表或以增加的大小可能会超过 replicated_size 的方式更新现有分布式表时，将返回错误。

log_size [ GB ] 一个正数。 整个设备上的事务日志的大小（整数或十进制 GB）。

有关 log_size 的最小和最大要求的信息，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中的“最小值和最大值”。

如果 AUTOGROW 为 ON，日志文件将可以增加到超出限制。 使用 [DBCC SHRINKLOG (Azure Synapse Analytics)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) 语句将日志文件大小减少至初始大小。

如果 AUTOGROW 为 OFF，那么对于在单个计算节点上增加的日志大小可能会超过 log_size 的任何操作，将向用户返回错误。

## <a name="permissions"></a>权限

需要 master 数据库中的 `CREATE ANY DATABASE` 权限，或者 sysadmin 固定服务器角色的成员身份。

以下示例向数据库用户 Fay 提供创建数据库的权限。

```sql
USE master;
GO
GRANT CREATE ANY DATABASE TO [Fay];
GO
```

## <a name="general-remarks"></a>一般备注

数据库创建时的数据库兼容性级别为 120，这是 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的兼容级别。 这可确保数据库将能够使用 PDW 所使用的所有 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 功能。

## <a name="limitations-and-restrictions"></a>限制和局限

不允许在显式事务中使用 CREATE DATABASE 语句。 有关详细信息，请参阅[语句](../../t-sql/statements/statements.md)。

有关数据库的最小和最大约束的信息，请参阅 [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] 中的“最小值和最大值”。

在创建数据库时，每个 Compute 节点上必须有足够的可用空间来分配以下大小的总和：

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的表的大小为 replicated_table_size。
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的表的大小为（distributed_table_size/Compute 节点数量）。
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志的大小为（log_size/Compute 节点数量）。

## <a name="locking"></a>锁定

在 DATABASE 对象上采用共享锁。

## <a name="metadata"></a>元数据

成功执行此操作后，[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 和 [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) 元数据视图中将会显示该数据库的条目。

## <a name="examples-sspdw"></a>示例：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="a-basic-database-creation-examples"></a>A. 基本数据库创建示例

以下示例创建数据库 `mytest`，对于复制表，每个 Compute 节点分配 100 GB 存储，对于分布式表，每个设备分配 500 GB 存储，对于事务日志，每个设备分配 100 GB 存储。 在此示例中，AUTOGROW 默认为关。

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB );
```

以下示例使用和以上相同的参数创建数据库 `mytest`，但 AUTOGROW 为关闭。 这允许数据库增加到超过参数的指定大小。

```sql
CREATE DATABASE mytest
  WITH
    (AUTOGROW = ON,
    REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB);
```

### <a name="b-creating-a-database-with-partial-gigabyte-sizes"></a>B. 创建部分 GB 级大小数据库

以下示例创建数据库 `mytest`，（AUTOGROW 为 关）对于复制表，每个计算节点分配 1.5 GB 存储，对于分布式表，每个设备分配 5.25 GB 存储，对于事务日志，每个设备分配 10 GB 存储。

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 1.5 GB,
    DISTRIBUTED_SIZE = 5.25 GB,
    LOG_SIZE = 10 GB);
```

## <a name="see-also"></a>另请参阅

- [ALTER DATABASE - Analytics Platform System](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end