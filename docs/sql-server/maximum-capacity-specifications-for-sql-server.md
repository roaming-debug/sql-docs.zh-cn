---
title: SQL Server 的最大容量规范
description: 本文介绍 SQL Server 组件中定义的各种对象的最大大小和最大数量以及其他信息。
ms.date: 03/05/2020
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- objects [SQL Server]
- number capacity specifications [SQL Server]
- size [SQL Server], capacity specifications
- number of objects
- capacity specifications [SQL Server]
- maximum capacity specifications [SQL Server]
- size [SQL Server]
- replication capacity specifications [SQL Server]
- objects [SQL Server], capacity specifications
- Database Engine [SQL Server], capacity specifications
ms.assetid: 13e95046-0e76-4604-b561-d1a74dd824d7
ms.author: mikeray
author: MikeRayMSFT
ms.openlocfilehash: c0d0da7b2f74eb9dacdef390ede2cdd34d0356fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336029"
---
# <a name="maximum-capacity-specifications-for-sql-server"></a>SQL Server 的最大容量规范

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

本文介绍 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 组件中定义的各种对象的最大大小和最大数量。

>[!NOTE]
>除了本文中的信息外，你可能还会发现以下链接有用：
>
>* [下载 SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
>* [安装 SQL Server 的硬件和软件要求](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
>* [检查系统配置检查器的参数](../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)
>

## <a name="ssde-objects"></a>[!INCLUDE[ssDE](../includes/ssde-md.md)]对象

在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库中定义的或在 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句中引用的各种对象的最大大小和最大数量。

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)] 对象|最大大小/数量 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] （64 位）|其他信息|
|---------------------------------------------------------|------------------------------------------------------------------|----------------------------|
|批大小|65,536 <sup>*</sup>（网络数据包大小）|网络数据包大小指的是用于在应用程序和关系[!INCLUDE[ssDE](../includes/ssde-md.md)]之间进行通信的表格格式数据流 (TDS) 数据包的大小。 默认的数据包大小为 4 KB，由“网络数据包大小”配置选项控制。|
|每个短字符串列的字节数|8,000||
|每 `GROUP BY`、`ORDER BY` 的字节数|8,060||
|每个索引键的字节数|聚集索引为 900 字节。 非聚集索引为 1,700 字节。|在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中，聚集索引键的最大字节数不能超过 900。 对于非聚集索引键，最大值为 1700 个字节数。<br /><br /> 你可以使用可变长度列来定义键，这些列的最大大小之和可超过此限制。 但是，这些列中数据的总大小绝不能超过此限制。<br /><br /> 在非聚集索引中，可以包含额外的非键列，且这些非键列不会算入键的大小限制。 非键列可能有助于更好地执行某些查询。|
|内存优化表中的每个索引键的字节数|非聚集索引为 2,500 字节。 哈希索引没有限制，只要全部索引键均适应行内即可。|在内存优化表上，非聚集索引不能具有声明的最大大小超过 2500 个字节的键列。 这与键列中实际数据是否短于声明的最大大小并不相关。<br /><br /> 哈希索引键的大小没有硬性限制。<br /><br /> 对于内存优化表的索引，不存在“包含的列”这一概念，因为所有索引本来就覆盖了所有的列。<br /><br /> 对于内存优化表，即使行大小为 8060 个字节，一些可变长度列也可以物理方式存储于这 8060 个字节以外的空间。 但是，表上所有索引的所有键列，加上表中任何其他固定长度列，其最大声明大小不得超过 8060 个字节。|
|每个外键的字节数|900||
|每个主键的字节数|900||
|每行的字节数|8,060|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持行溢出存储，行溢出存储使长度可变的列可以被推送到行外。 对于被推送到行外的长度可变的列，主记录中仅存储 24 个字节根。 此功能允许的限制实际上比以前的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本更高。 有关更多信息，请参阅[大型行支持](../relational-databases/pages-and-extents-architecture-guide.md#large-row-support)。|
|内存优化表中的每行字节数|8,060|启动 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] 内存优化表支持行外存储。 如果表中的所有列的最大大小超过 8060 个字节，则长度可变的列被推送到行外；此操作是编译时的决定。 存储于行外的列仅有 8 字节的引用存储于行内。 有关详细信息，请参阅 [内存优化表中的表和行大小](../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)。|
|存储过程源文本中的字节数|批处理大小中的较小者或 250 MB||
|每个 `varchar(max) `、`varbinary(max)`、`xml`、`text` 或 `image` 列的字节数|2^31-1||
|每个 `ntext` 或 `nvarchar(max)` 列的字符数|2^30-1||
|每个表的聚集索引数|1||
|`GROUP BY`、`ORDER BY` 中的列|仅受字节数限制||
|`GROUP BY WITH CUBE` 或 `WITH ROLLUP` 语句中的列或表达式|10||
|每个索引键的列数|32|如果表包含一个或多个 XML 索引，由于 XML 列被添加到主 XML 索引的聚集键，因此用户表的聚集键被限制为 31 列。 在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中，可在非聚集索引中包括非键列以避免最多为 32 个键列的限制。 有关详细信息，请参阅 [Create Indexes with Included Columns](../relational-databases/indexes/create-indexes-with-included-columns.md)。|
|每个外键或主键的列数|32||
|每个 `INSERT` 语句的列数|4,096||
|每个 `SELECT` 语句的列数|4,096||
|每个表的列数|1,024|包含稀疏列集的表最多包含 30,000 列。 请参阅[稀疏列集](../relational-databases/tables/use-column-sets.md)。|
|每个 `UPDATE` 语句的列数|4,096|[稀疏列集](../relational-databases/tables/use-column-sets.md)有不同的限制。|
|每个视图的列数|1,024||
|每个客户端的连接个数|已配置连接的最大值||
|数据库大小|524,272 TB||
|每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|32,767||
|每个数据库的文件组个数|32,767||
|每个数据库的内存优化数据文件组个数|1||
|每个数据库的文件个数|32,767||
|文件大小（数据）|16 TB||
|文件大小（日志）|2 TB||
|每个数据库的内存优化数据文件个数|[!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)] 中为 4,096。 更高版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不会施加这样的严格限制。||
|每个内存优化数据文件的差异文件|1||
|每个表的外键表引用数|传出 = 253。 传入 = 10,000。|有关限制，请参阅 [Create Foreign Key Relationships](../relational-databases/tables/create-foreign-key-relationships.md)。|
|标识符长度（以字符计）|128||
|每台计算机的实例数|独立服务器上为 50 个实例。<br /><br />使用共享群集磁盘作为存储时，有 25 个故障转移群集实例。<br/><br/>使用 SMB 文件共享作为存储选项时，有 50 个故障转移群集实例。||
|每个内存优化表的索引个数|自 [!INCLUDE[ssSQL17](../includes/ssSQL17-md.md)] 起以及在 [!INCLUDE[ssSDSFull](../includes/ssSDSFull-md.md)] 中为 999<br/>[!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)] 和 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] 中为 8||
|包含 SQL 语句的字符串的长度（批大小）|65,536（网络数据包大小）|网络数据包大小指的是用于在应用程序和关系[!INCLUDE[ssDE](../includes/ssde-md.md)]之间进行通信的表格格式数据流 (TDS) 数据包的大小。 默认的数据包大小为 4 KB，由“网络数据包大小”配置选项控制。|
|每个连接的锁数|每个服务器的最大锁数||
|每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|仅受内存限制|此值针对静态锁分配。 动态锁仅受内存限制。|
|嵌套存储过程级别数|32|如果存储过程访问的数据库多于 64 个，或者交替访问的数据库多于 2 个，将收到错误信息。|
|嵌套子查询个数|32||
|嵌套事务|4,294,967,296|| 
|嵌套触发器层数|32||
|每个表的非聚集索引数|999||
|当存在以下任意子句时，`GROUP BY` 子句中的非重复表达式数：`CUBE`、`ROLLUP`、`GROUPING SETS`、`WITH CUBE`、`WITH ROLLUP`|32||
|`GROUP BY` 子句中的运算符生成的分组集数目|4,096||
|每个存储过程的参数个数|2,100||
|每个用户定义函数的参数个数|2,100||
|每个数据表的 REFERENCE 个数|253||
|每个数据表的行数|受可用存储空间限制||
|每个数据库的表数|受数据库中对象总数限制|对象包括表、视图、存储过程、用户定义函数、触发器、规则、默认值和约束。 数据库中所有对象的数量总和不能超过 2,147,483,647。|
|每个分区表或索引的分区数|15,000||
|非索引列的统计信息条数|30,000|| 
|每个 `SELECT` 语句的表数目|仅受可用资源限制||
|每个表的触发器数|受数据库中对象数限制|对象包括表、视图、存储过程、用户定义函数、触发器、规则、默认值和约束。 数据库中所有对象的数量总和不能超过 2,147,483,647。|
|用户连接|32,767||
|XML 索引|249||

## <a name="ssnoversion-utility-objects"></a>[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具对象

在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具中测试的各种对象的最大大小和最大数量。

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具对象|最大大小/数量 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] （64 位）|
|----------------------------------------------|------------------------------------------------------------------|
|每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具的计算机数（物理计算机或虚拟计算机）|100|
|每台计算机的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例数|5|
|每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例总数|200<sup>*</sup>|
|每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例的用户数据库数（包括数据层应用程序）|50|
|每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具的用户数据库总数|1,000|
|每个数据库的文件组数|1|
|每个文件组的数据文件数|1|
|每个数据库的日志文件数|1|
|每台计算机的卷数|3|

<sup>*</sup> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具支持的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 托管实例的最大数目将会依服务器的硬件配置而定。 有关入门信息，请参阅 [SQL Server 实用工具功能和任务](../relational-databases/manage/sql-server-utility-features-and-tasks.md)。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本中均提供 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]。 有关 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅 [SQL Server 2019 各个版本支持的功能](./editions-and-components-of-sql-server-version-15.md)、[SQL Server 2017 各个版本支持的功能](./editions-and-components-of-sql-server-2017.md)或 [SQL Server 2016 各个版本支持的功能](./editions-and-components-of-sql-server-2016.md)。

## <a name="ssnoversion-data-tier-application-objects"></a>[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据层应用程序对象

在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据层应用程序 (DAC) 中测试的各种对象的最大大小和最大数量。

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] DAC 对象|最大大小/数量 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] （64 位）|
|------------------------------------------|------------------------------------------------------------------|
|每个 DAC 的数据库数|1|
|每个 DAC <sup>*</sup> 的对象数目|受数据库中对象数或可用内存限制。|

<sup>*</sup> 限制中包含的对象类型为用户、表、视图、存储过程、用户定义函数、用户定义数据类型、数据库角色、架构和用户定义表类型。

## <a name="replication-objects"></a>复制对象

在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 复制中定义的各种对象的最大大小和最大数量。

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 复制对象|最大大小/数量 SQL Server（64 位）|
|--------------------------------------------------|---------------------------------------------------|
|项目（合并发布）|2048|
|项目（快照发布或事务发布）|32,767|
|表中的列数<sup>*</sup>（合并发布）|246|
|表中的列数<sup>**</sup>（[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 快照发布或事务发布）|1,000|
|表中的列数<sup>**</sup>（Oracle 快照发布或事务发布）|995|
|行筛选器中使用的列的字节数（合并发布）|1,024|
|行筛选器中使用的列的字节数（快照发布或事务发布）|8,000|

<sup>*</sup>如果行跟踪用于冲突检测（默认值），则基表最多可包含 1,024 列，但是必须从项目中筛选列，以便最多发布 246 列。 如果使用列跟踪，则基表最多可包含 246 列。

<sup>**</sup>基表可以包含发布数据库中允许的最大数量的列（在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中为 1,024），但如果这些列的数目超过为发布类型指定的最大值，则必须从项目中筛选这些列。

## <a name="see-also"></a>另请参阅

[SQL Server 实用工具的功能和任务](../relational-databases/manage/sql-server-utility-features-and-tasks.md)