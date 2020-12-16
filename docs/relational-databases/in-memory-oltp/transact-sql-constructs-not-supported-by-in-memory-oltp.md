---
title: 内存中 OLTP 不支持 T-SQL
description: 了解内存优化表、本机编译存储过程和用户定义函数不支持的 Transact-SQL 功能。
ms.custom: seo-dt-2019
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e3f8009c-319d-4d7b-8993-828e55ccde11
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f28ea2b3ce9520eca770b0808738a53073d70316
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438730"
---
# <a name="transact-sql-constructs-not-supported-by-in-memory-oltp"></a>内存中 OLTP 不支持的 Transact-SQL 构造
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  内存优化表、本机编译的存储过程和用户定义函数不支持由基于磁盘的表、解释 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程和用户定义函数所支持的完整 [!INCLUDE[tsql](../../includes/tsql-md.md)] 外围应用。 尝试使用某个不支持的功能时，服务器返回错误。  
  
 错误消息文本提及 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的类型（例如功能、操作、选项）以及功能或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 关键字的名称。 大多数不支持的功能将返回错误 10794，并带有错误消息文本，指示不支持该功能。 下表列出可在错误消息文本中显示的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能和关键字，以及用于解决问题的更正操作。  
  
 有关针对内存优化表和编辑编译的存储过程的支持的功能的详细信息，请参阅：  
  
-   [本机编译存储过程的迁移问题](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
-   [对内存中 OLTP 的 Transact-SQL 支持](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)  
  
-   [内存中 OLTP 不支持的 SQL Server 功能](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)  
  
-   [本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
## <a name="databases-that-use-in-memory-oltp"></a>使用内存中 OLTP 的数据库  
 下表列出了不受支持的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能，以及涉及内存中 OLTP 数据库的错误消息文本中可能显示的关键字。 此表还列出了错误的解决方法。  
  
|类型|名称|解决方法|  
|----------|----------|----------------|  
|选项|AUTO_CLOSE|具有 MEMORY_OPTIMIZED_DATA 文件组的数据库不支持数据库选项 AUTO_CLOSE=ON。|  
|选项|ATTACH_REBUILD_LOG|具有 MEMORY_OPTIMIZED_DATA 文件组的数据库不支持 CREATE 数据库选项 ATTACH_REBUILD_LOG。|  
|Feature|DATABASE SNAPSHOT|具有 MEMORY_OPTIMIZED_DATA 文件组的数据库不支持创建数据库快照。|  
|Feature|使用 sync_method“database snapshot”或“database snapshot character”进行复制|具有 MEMORY_OPTIMIZED_DATA 文件组的数据库不支持使用 sync_method“database snapshot”或“database snapshot character”进行复制。|  
|Feature|DBCC CHECKDB<br /><br /> DBCC CHECKTABLE|DBCC CHECKDB 跳过数据库中的内存优化表。<br /><br /> 对内存优化表执行 DBCC CHECKTABLE 将失败。|  
  
## <a name="memory-optimized-tables"></a>内存优化表  
 下表列出了不受支持的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能，以及涉及内存优化表的错误消息文本中可能显示的关键字。 此表还列出了错误的解决方法。  
  
|类型|名称|解决方法|  
|----------|----------|----------------|  
|Feature|ON|内存优化的表不能放置在文件组或分区方案上。 从 **CREATE TABLE** 语句删除 ON 子句。<br /><br /> 所有的内存优化表都映射到内存优化文件组。|  
|数据类型|数据类型名称|不支持所示的数据类型。 使用支持的数据类型之一替换该类型。 有关详细信息，请参阅 [内存中 OLTP 支持的数据类型](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)。|  
|Feature|计算列|适用对象：[!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] 和 [!INCLUDE[ssSQL15-md](../../includes/sssql15-md.md)]<br/>内存优化的表不支持计算列。 从 **CREATE TABLE** 语句删除计算列。<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] 和自 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 开始的 SQL Server 支持内存优化表和索引中的计算列。|  
|Feature|复制|内存优化表不支持复制。|  
|Feature|FILESTREAM|内存优化的表列不支持 FILESTREAM 存储。 从列定义中删除 **FILESTREAM** 关键字。|  
|Feature|SPARSE|内存优化的表列不能定义为 SPARSE。 从列定义中删除 **SPARSE** 关键字。|  
|Feature|ROWGUIDCOL|内存优化的表列不支持选项 ROWGUIDCOL。 从列定义中删除 **ROWGUIDCOL** 关键字。|  
|Feature|FOREIGN KEY|适用对象：[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] 和自 [!INCLUDE[ssSQL15-md](../../includes/sssql15-md.md)] 开始的 SQL Server<br/>对于内存优化表，FOREIGN KEY 约束仅支持引用其他内存优化表主键的外键。 如果外键引用了唯一约束，请从表定义中删除该约束。<br/><br/>在 [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] 中，FOREIGN KEY 约束不支持内存优化表。|  
|Feature|聚集索引|指定非聚集索引。 在主键索引的情况下，务必指定 PRIMARY KEY NONCLUSTERED。|  
|Feature|事务中的 DDL|在用户事务的上下文中无法创建或删除内存优化的表和本机编译的存储过程。 在执行 CREATE 或 DROP 语句前不要启动事务并确保会话设置 IMPLICIT_TRANSACTIONS 为 OFF。|  
|Feature|DDL 触发器|如果存在该 DDL 操作的服务器或数据库触发器，则无法创建或删除内存优化的表和本机编译的存储过程。 在 CREATE/DROP TABLE 和 CREATE/DROP PROCEDURE 时删除服务器和数据库触发器。|  
|Feature|EVENT NOTIFICATION|如果存在该 DDL 操作的服务器或数据库事件通知，则无法创建或删除内存优化的表和本机编译的存储过程。 在 CREATE TABLE 或 DROP TABLE 和 CREATE PROCEDURE 或 DROP PROCEDURE 时删除服务器和数据库事件通知。|  
|Feature|FileTable|无法将内存优化的表创建为文件表。 从 **AS FileTable** 语句删除参数 **CREATE TABLE** 。|  
|Operation|更新主键列|无法更新内存优化的表和表类型中的主键列。 如果需要更新主键，请删除旧的行并插入包含更新的主键的新行。|  
|Operation|CREATE INDEX|必须使用 **CREATE TABLE** 语句或 **ALTER TABLE** 语句以内联方式指定内存优化表的索引。|  
|Operation|CREATE FULLTEXT INDEX|内存优化的表不支持全文检索。|  
|Operation|架构更改|内存优化表和本机编译存储过程不支持某些架构更改：<br/> [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] 和自 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始的 SQL Server：支持 ALTER TABLE、ALTER PROCEDURE 和 sp_rename 操作。 不支持其他架构更改，如添加扩展属性。<br/><br/>[!INCLUDE[ssSQL15-md](../../includes/sssql15-md.md)]：支持 ALTER TABLE 和 ALTER PROCEDURE 操作。 不支持其他架构更改，包括 sp_rename。<br/><br/>[!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)]：不支持架构更改。 若要更改内存优化表或本机编译存储过程的定义，首先删除该对象，然后用所需定义重新创建。| 
|Operation|TRUNCATE TABLE|内存优化的表不支持 TRUNCATE 操作。 若要从表中删除所有行，请使用 **DELETE FROM** table 删除所有行，或删除并重新创建该表。|  
|Operation|ALTER AUTHORIZATION|不支持更改现有内存优化的表或本机编译的存储过程的所有者。 请删除并重新创建该表或过程来更改所有权。|  
|Operation|ALTER SCHEMA|不支持将现有表或本机编译存储过程传输到另一个架构。 删除并重新创建要在架构之间传输的对象。|  
|Operation|DBCC CHECKTABLE|内存优化表不支持 DBCC CHECKTABLE。 若要验证磁盘上检查点文件的完整性，请执行 MEMORY_OPTIMIZED_DATA 文件组的备份。|  
|Feature|ANSI_PADDING OFF|创建内存优化表或本机编译存储过程时会话选项 **ANSI_PADDING** 必须为 ON。 在运行 CREATE 语句前执行 **SET ANSI_PADDING ON** 。|  
|选项|DATA_COMPRESSION|内存优化的表不支持数据压缩。 从表定义中删除该选项。|  
|Feature|DTC|不能从分布式事务访问内存优化的表和本机编译的存储过程。 请改用 SQL 事务。|  
|Operation|内存优化的表作为 MERGE 的目标|内存优化表不能是 **MERGE** 操作的目标。 请改用 INSERT、UPDATE 和 DELETE 语句。|  
  
## <a name="indexes-on-memory-optimized-tables"></a>内存优化的表的索引  
 下表列出可在涉及内存优化表的索引错误消息文本中显示的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能和关键字，以及用于解决问题的更正操作。  
  
|类型|名称|解决方法|  
|----------|----------|----------------|  
|Feature|筛选索引|内存优化的表不支持筛选的索引。 从索引定义中省略 **WHERE** 子句。|  
|Feature|包含列|指定包含列不是内存优化表所必需的。 内存优化表的所有列都隐式包含在每个内存优化索引中。|  
|Operation|DROP INDEX|不支持删除内存优化的表的索引。 可使用 ALTER TABLE 删除索引。<br /><br /> 有关详细信息，请参阅 [更改内存优化表](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)。|  
|索引选项|*索引选项*|仅支持一个索引选项 - BUCKET_COUNT for HASH 索引。|  
  
## <a name="nonclustered-hash-indexes"></a>非聚集哈希索引  
 下表列出可在涉及非聚集哈希索引的错误消息文本中显示的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能和关键字，以及用于解决问题的更正操作。  
  
|类型|名称|解决方法|  
|----------|----------|----------------|  
|选项|ASC/DESC|非聚集哈希索引不排序。 从索引键定义中删除关键字 **ASC** 和 **DESC** 。|  
  
## <a name="natively-compiled-stored-procedures-and-user-defined-functions"></a>本机编译存储过程和用户定义函数  
 下表列出了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能、涉及本机编译存储过程和用户定义函数的错误消息文本中可能显示的关键字，以及解决错误的纠正措施。  
  
|类型|Feature|解决方法|  
|----------|-------------|----------------|  
|Feature|内联表变量|不能使用变量声明内联声明表类型。 必须使用 **CREATE TYPE** 语句显式声明表类型。|  
|Feature|游标|本机编译的存储过程不支持游标。<br /><br /> 从客户端执行该过程时，请使用 RPC 而非游标 API。 对于 ODBC，避免 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 **EXECUTE**，直接指定过程的名称。<br /><br /> 从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理或另一存储过程执行该过程时，请避免将游标用于本机编译的存储过程。<br /><br /> 创建本机编译的存储过程时，请使用基于集的逻辑或 **WHILE** 循环而不要使用游标。|  
|Feature|非常量参数默认值|将默认值与本机编译的存储过程参数一起使用时，这些值必须为常量。 从参数声明中删除所有通配符。|  
|Feature|EXTERNAL|无法本机编译 CLR 存储过程。 从 CREATE PROCEDURE 语句中删除 AS EXTERNAL 子句或 NATIVE_COMPILATION 选项。|  
|Feature|带编号的存储过程|不能对本机编译的存储过程编号。 从 **CREATE PROCEDURE**_语句中删除_ ; **number** 。|  
|Feature|多行 INSERT ...VALUES 语句|在本机编译的存储过程中无法使用同一 **INSERT** 语句插入多行。 为每行创建 **INSERT** 语句。|  
|Feature|公用表表达式 (CTE)|本机编译的存储过程中不支持公用表表达式 (CTE)。 重写查询。|  
|Feature|COMPUTE|不支持 **COMPUTE** 子句。 从查询中删除它。|  
|Feature|SELECT INTO|**INTO** 语句不支持 **SELECT** 子句。 将查询重写为 INSERT INTO Table SELECT 。|  
|Feature|不完整的插入列列表|一般情况下，在 INSERT 语句中，必须为表中的所有列指定值。<br /><br /> 但是，我们支持内存优化表上的 DEFAULT 约束和 IDENTITY(1,1) 列。 可以将这些列（如果为 IDENTITY 列，则必须）从 INSERT 列列表中忽略。|  
|Feature|*Function*|本机编译存储过程中不支持某些内置函数。 从存储过程中删除被拒绝的函数。 有关支持的内置函数的详细信息，请参阅<br />[本机编译的 T-SQL 模块支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)，或<br />[本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)。|  
|Feature|CASE|适用对象：[!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] 和自 [!INCLUDE[ssSQL15-md](../../includes/sssql15-md.md)] 开始的 SQL Server<br/>本机编译存储过程中的查询不支持 CASE 表达式。 创建每个情况的查询。 有关详细信息，请参阅 [在本机编译的存储过程中实现 CASE 表达式](../../relational-databases/in-memory-oltp/implementing-a-case-expression-in-a-natively-compiled-stored-procedure.md)。<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] 和自 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始的 SQL Server 不支持 CASE 表达式。|  
|Feature|INSERT EXECUTE|删除引用。|  
|Feature|EXECUTE|仅支持执行本机编译存储过程和用户定义函数。|  
|Feature|用户定义聚合|不能在本机编译的存储过程中使用用户定义的聚合函数。 从过程中删除对该函数的引用。|  
|Feature|浏览模式元数据|本机编译的存储过程不支持浏览模式元数据。 确保将会话选项 **NO_BROWSETABLE** 设置为 OFF。|  
|Feature|带 FROM 子句的 DELETE|表源在本机编译存储过程中的 **FROM** 语句不支持 **DELETE** 子句。<br /><br /> 在指示要执行删除操作的表时，支持带 **DELETE** 子句的 **FROM** 。|  
|Feature|带 FROM 子句的 UPDATE|本机编译的存储过程中的 **FROM** 语句不支持 **UPDATE** 子句。| 
|Feature|临时程序|临时存储过程无法进行本机编译。 请创建永久的本机编译存储过程或临时的解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程。|  
|隔离级别|READ UNCOMMITTED|本机编译的存储过程不支持隔离级别 READ UNCOMMITTED。 请使用支持的隔离级别，如 SNAPSHOT。|  
|隔离级别|READ COMMITTED|本机编译存储过程不支持隔离级别 READ COMMITTED。 请使用支持的隔离级别，如 SNAPSHOT。|  
|Feature|临时表|tempdb 中的表不能在本机编译的存储过程中使用。 请使用表变量或 DURABILITY=SCHEMA_ONLY 的内存优化表。|  
|Feature|DTC|不能从分布式事务访问内存优化的表和本机编译的存储过程。 请改用 SQL 事务。|  
|Feature|EXECUTE WITH RECOMPILE|本机编译的存储过程不支持选项 **WITH RECOMPILE** 。|  
|Feature|从专用管理员连接执行。|不能从专用管理员连接 (DAC) 执行本机编译的存储过程。 请改用常规连接。|  
|Operation|保存点|不能从具有活动保存点的事务调用本机编译的存储过程。 从事务中删除该保存点。|  
|Operation|ALTER AUTHORIZATION|不支持更改现有内存优化的表或本机编译的存储过程的所有者。 请删除并重新创建该表或过程来更改所有权。|  
|操作员|OPENROWSET|不支持此运算符。 从本机编译的存储过程中删除 **OPENROWSET** 。|  
|操作员|OPENQUERY|不支持此运算符。 从本机编译的存储过程中删除 **OPENQUERY** 。|  
|操作员|OPENDATASOURCE|不支持此运算符。 从本机编译的存储过程中删除 **OPENDATASOURCE** 。|  
|操作员|OPENXML|不支持此运算符。 从本机编译的存储过程中删除 **OPENXML** 。|  
|操作员|CONTAINSTABLE|不支持此运算符。 从本机编译的存储过程中删除 **CONTAINSTABLE** 。|  
|操作员|FREETEXTTABLE|不支持此运算符。 从本机编译的存储过程中删除 **FREETEXTTABLE** 。|  
|Feature|表值函数|不能从本机编译的存储过程引用表值函数。 有可能解决此限制的一种方法是将表值函数中的逻辑添加到过程主体。|  
|操作员|CHANGETABLE|不支持此运算符。 从本机编译的存储过程中删除 **CHANGETABLE** 。|  
|操作员|GOTO|不支持此运算符。 使用其他过程构造，如 WHILE。|  
|操作员|OFFSET|不支持此运算符。 从本机编译的存储过程中删除 **OFFSET** 。|  
|操作员|INTERSECT|不支持此运算符。 从本机编译的存储过程中删除 **INTERSECT** 。 在某些情况下，可以使用 INNER JOIN 获得相同的结果。|  
|操作员|EXCEPT|不支持此运算符。 从本机编译的存储过程中删除 **EXCEPT** 。|  
|操作员|APPLY|适用对象：[!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] 和自 [!INCLUDE[ssSQL15-md](../../includes/sssql15-md.md)] 开始的 SQL Server<br/>不支持此运算符。 从本机编译的存储过程中删除 **APPLY** 。<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] 和自 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始的 SQL Server 不支持本机编译模块中的 APPLY 运算符。|  
|操作员|PIVOT|不支持此运算符。 从本机编译的存储过程中删除 **PIVOT** 。|  
|操作员|UNPIVOT|不支持此运算符。 从本机编译的存储过程中删除 **UNPIVOT** 。|  
|操作员|CONTAINS|不支持此运算符。 从本机编译的存储过程中删除 **CONTAINS** 。|  
|操作员|FREETEXT|不支持此运算符。 从本机编译的存储过程中删除 **FREETEXT** 。|  
|操作员|TSEQUAL|不支持此运算符。 从本机编译的存储过程中删除 **TSEQUAL** 。|  
|操作员|LIKE|不支持此运算符。 从本机编译的存储过程中删除 **LIKE** 。|  
|操作员|NEXT VALUE FOR|不能在本机编译的存储过程内引用序列。 使用解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)]获取值，然后将它传递到本机编译的存储过程。 有关详细信息，请参阅 [在内存优化表中实现 IDENTITY](../../relational-databases/in-memory-oltp/implementing-identity-in-a-memory-optimized-table.md)。|  
|Set 选项|*option*|在本机编译的存储过程内无法更改 SET 选项。 可以使用 BEGIN ATOMIC 语句设置某些选项。 有关详细信息，请参阅 [Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md)中有关原子块的章节。|  
|操作数|TABLESAMPLE|不支持此运算符。 从本机编译的存储过程中删除 **TABLESAMPLE** 。|  
|选项|RECOMPILE|在创建时编译本机编译的存储过程。 从过程定义中删除 **RECOMPILE** 。<br /><br /> 可以对本机编译存储过程执行 sp_recompile，使其在下一次执行时进行重新编译。|  
|选项|ENCRYPTION|不支持此选项。 从过程定义中删除 **ENCRYPTION** 。|  
|选项|FOR REPLICATION|无法为复制创建本机编译的存储过程。 从过程定义中删除 **FOR REPLICATION** 。|  
|选项|FOR XML|不支持此选项。 从本机编译的存储过程中删除 **FOR XML** 。|  
|选项|FOR BROWSE|不支持此选项。 从本机编译的存储过程中删除 **FOR BROWSE** 。|  
|联接提示|HASH、MERGE|本机编译的存储过程仅支持嵌套的循环联接。 不支持哈希和合并联接。 删除联接提示。|  
|查询提示|*查询提示*|此查询提示不位于本机编译的存储过程内。 有关支持的查询提示，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。|  
|选项|PERCENT|**TOP** 子句不支持此选项。 从本机编译的存储过程中的查询删除 **PERCENT** 。|  
|选项|WITH TIES|适用对象：[!INCLUDE[ssSDS14_md](../../includes/sssql14-md.md)] 和 [!INCLUDE[ssSQL15-md](../../includes/sssql15-md.md)]<br/>**TOP** 子句不支持此选项。 从本机编译的存储过程中的查询删除 **WITH TIES** 。<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] 和自 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始的 SQL Server 不支持 TOP WITH TIES。|  
|聚合函数|*聚合函数*|并非支持所有聚合函数。 有关本机编译 T-SQL 模块中支持的聚合函数的详细信息，请参阅[本机编译 T-SQL 模块中支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。|  
|排名函数|*排名函数*|本机编译的存储过程中不支持排名函数。 从过程定义中删除它们。|  
|函数|*Function*|不支持此函数。 有关本机编译 T-SQL 模块中受支持的函数的详细信息，请参阅[本机编译 T-SQL 模块的受支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。|  
|语句|*Statement*|不支持此语句。 有关本机编译 T-SQL 模块中受支持的函数的详细信息，请参阅[本机编译 T-SQL 模块的受支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。|  
|Feature|MIN 和 MAX 用于二进制和字符串|聚合函数 **MIN** 和 **MAX** 不能用于本机编译的存储过程中的字符和二进制字符串值。|  
|Feature|GROUP BY ALL|在本机编译的存储过程中，不能将 ALL 与 GROUP BY 子句一起使用。 从 GROUP BY 子句中删除 ALL。|  
|Feature|GROUP BY ()|不支持按空列表分组。 删除 GROUP BY 子句，或在分组列表中加入列。|  
|Feature|ROLLUP|在本机编译的存储过程中，**ROLLUP** 不能与 **GROUP BY** 子句一起使用。 从过程定义中删除 **ROLLUP** 。|  
|Feature|CUBE|在本机编译的存储过程中，**CUBE** 不能与 **GROUP BY** 子句一起使用。 从过程定义中删除 **CUBE** 。|  
|Feature|GROUPING SETS|在本机编译的存储过程中，**GROUPING SETS** 不能与 **GROUP BY** 子句一起使用。 从过程定义中删除 **GROUPING SETS** 。|  
|Feature|BEGIN TRANSACTION、COMMIT TRANSACTION 和 ROLLBACK TRANSACTION|使用 ATOMIC 块控制事务和错误处理。 有关详细信息，请参阅 [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md)。|  
|Feature|内联表变量声明。|表变量必须引用显式定义的内存优化表类型。 应创建内存优化的表类型并将该类型用于变量声明，而不应在行内指定类型。|  
|Feature|基于磁盘的表|无法从本机编译的存储过程中访问基于磁盘的表。 从本机编译的存储过程中删除对基于磁盘的表的引用。 或者，将基于磁盘的表转换为内存优化的表。|  
|Feature|视图|无法从本机编译的存储过程中访问视图。 不引用视图，改为引用底层基表。|  
|Feature|表值函数|适用于：[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] 和自 [!INCLUDE[ssSQL15-md](../../includes/sssql15-md.md)] 开始的SQL Server<br/>无法从本机编译的 T-SQL 模块中访问多语句表值函数。 支持内联表值函数，但必须使用 NATIVE_COMPILATION 创建该函数。<br/><br/>适用于：[!INCLUDE[ssSQL14-md](../../includes/ssSQL14-md.md)]<br/>不能从本机编译的 T-SQL 模块引用表值函数。|  
|选项|PRINT|删除引用|  
|Feature|DDL|本机编译的 T-SQL 模块不支持 DDL。|  
|选项|STATISTICS XML|不支持。 在启用 STATISTICS XML 的情况下运行查询时，会返回 XML 内容，而不返回本机编译存储过程部分。|  
  
## <a name="transactions-that-access-memory-optimized-tables"></a>访问内存优化表的事务  
 下表列出可在涉及访问内存优化表的事务错误消息文本中显示的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能和关键字，以及用于解决问题的更正操作。  
  
|类型|名称|解决方法|  
|----------|----------|----------------|  
|Feature|保存点|不支持在访问内存优化的表的事务中创建显式保存点。|  
|Feature|绑定的事务|绑定的会话不能参与访问内存优化的表的事务。 在执行过程前不要绑定会话。|  
|Feature|DTC|访问内存优化的表的事务不能是分布式事务。|  
  
## <a name="see-also"></a>另请参阅  
 [迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
