---
title: SQL Server - Deprecated Features 对象 | Microsoft Docs
description: 了解 SQLServer:Deprecated Features 对象，该对象提供一个计数器来监视指定为不推荐使用的功能。
ms.custom: ''
ms.date: 05/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Deprecated Features
- performance counters [SQL Server], deprecated features
- deprecation [SQL Server], performance counters
- Deprecated Features object
ms.assetid: e95de9d6-c950-41cd-8aaa-be529c6de198
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4e6e6ae53bfc9cf3c6c702e9caf8a1a9bdaf29a1
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505713"
---
# <a name="sql-server-deprecated-features-object"></a>SQL Server，Deprecated Features 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 SQLServer:Deprecated Features 对象提供一个计数器来监视指定为不推荐使用的功能。 在每个事例中计数器都提供一个使用计数，列出自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上次启动以来遇到不推荐使用的功能的次数。  
  
 这些计数器的值也可通过执行以下语句而得：  
  
```  
SELECT * FROM sys.dm_os_performance_counters   
WHERE object_name = 'SQLServer:Deprecated Features';  
```  

下表描述了 SQL Server **Deprecated Features** 性能对象。

|**SQL Server Deprecated Features 计数器**|说明|  
|-------------|-----------------|  
|**使用情况**|SQL Server 自上次启动以来的功能使用情况。|
  
 下表描述了 SQL Server Deprecated Features 计数器实例。  
  
|SQL Server Deprecated Features 计数器实例|说明|  
|------------------------------------------------------|-----------------|  
|“#”和“##”作为临时表和存储过程的名称|遇到不包含 # 以外的任何字符的标识符。 请至少使用一个其他字符。 每次编译时发生。|  
|“::”函数调用语法|表值函数遇到 :: 函数调用语法。 替换为 `SELECT column_list FROM` <function_name>`()`。 例如，将 `SELECT * FROM ::fn_virtualfilestats(2,1)`替换为 `SELECT * FROM sys.fn_virtualfilestats(2,1)`。 每次编译时发生。|  
|“\@”和以“\@\@”开头的名称作为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符|遇到以 \@ 或 \@\@ 开头的标识符。 请勿使用 \@ 或 \@v@ 或以 \@\@ 开头的名称作为标识符。 每次编译时发生。|  
|ADDING TAPE DEVICE|遇到不推荐使用的功能 sp_addumpdevice'**tape**'。 请改用 sp_addumpdevice'**disk**'。 每次使用时发生。|  
|ALL 权限|遇到 GRANT ALL、DENY ALL 或 REVOKE ALL 语法的总次数。 请修改语法以拒绝特定权限。 每次查询时发生。|  
|ALTER DATABASE WITH TORN_PAGE_DETECTION|自服务器实例启动以来，ALTER DATABASE 的不推荐使用的功能 TORN_PAGE_DETECTION 选项的使用总次数。 请改用 PAGE_VERIFY 语法。 每次在 DDL 语句中使用时发生。|  
|ALTER LOGIN WITH SET CREDENTIAL|遇到不推荐使用的功能语法 ALTER LOGIN WITH SET CREDENTIAL 或 ALTER LOGIN WITH NO CREDENTIAL。 请改用 ADD 或 DROP CREDENTIAL 语法。 每次编译时发生。|  
|Azeri_Cyrilllic_90|每次数据库启动时和每次排序规则使用时发生事件。 计划修改使用该排序规则的应用程序。|  
|Azeri_Latin_90|每次数据库启动时和每次排序规则使用时发生事件。 计划修改使用该排序规则的应用程序。|  
|BACKUP DATABASE 或 LOG TO TAPE|遇到不推荐使用的功能 BACKUP { DATABASE &#124; LOG } TO TAPE 或 BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_tape*。<br /><br /> 请改用 BACKUP { DATABASE &#124; LOG } TO DISK 或 BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_disk*。 每次使用时发生。|  
|BACKUP DATABASE 或 LOG WITH MEDIAPASSWORD|遇到不推荐使用的功能 BACKUP DATABASE WITH MEDIAPASSWORD 或 BACKUP LOG WITH MEDIAPASSWORD。 请勿使用 WITH MEDIAPASSWORD。|  
|BACKUP DATABASE 或 LOG WITH PASSWORD|遇到不推荐使用的功能 BACKUP DATABASE WITH PASSWORD 或 BACKUP LOG WITH PASSWORD。 请勿使用 WITH PASSWORD。|  
|COMPUTE [BY]|遇到 COMPUTE 或 COMPUTE BY 语法。 重写查询以将 GROUP BY 与 ROLLUP 一起使用。 每次编译时发生。|  
|CREATE FULLTEXT CATLOG IN PATH|遇到带有 IN PATH 子句的 CREATE FULLTEXT CATLOG 语句。 该子句在此版本的 SQL Server 中不起作用。 每次使用时发生。|  
|CREATE TRIGGER WITH APPEND|遇到带有 WITH APPEND 子句的 CREATE TRIGGER 语句。 请改为重新创建整个触发器。 每次在 DDL 语句中使用时发生。|  
|CREATE_DROP_DEFAULT|遇到 CREATE DEFAULT 或 DROP DEFAULT 语法。 请使用 CREATE TABLE 或 ALTER TABLE 的 DEFAULT 选项重写该命令。 每次编译时发生。|  
|CREATE_DROP_RULE|遇到 CREATE RULE 语法。 请使用约束重写该命令。 每次编译时发生。|  
|数据类型：text、ntext 或 image|遇到 **text**、 **ntext** 或 **image** 数据类型。 请重写应用程序以使用 **varchar(max)** 数据类型和已删除的 **text**、 **ntext** 和 **image** 数据类型语法。 每次查询时发生。|  
||数据库兼容级别更改为 80 的总次数。 计划在下一版本发布前升级数据库和应用程序。 在启动兼容级别为 80 的数据库时也会发生。|  
|数据库兼容级别 100、110。 120|更改数据库兼容级别的总次数。 计划为未来版本升级数据库和应用程序。 在启动不推荐使用的兼容级别的数据库时也会发生。|  
|DATABASE_MIRRORING|遇到对数据库镜像功能的引用。 计划升级到 AlwaysOn 可用性组，或者，如果正在运行不支持 AlwaysOn 可用性组的 SQL Server 版本，则计划迁移到日志传送。|  
|database_principal_aliases|遇到对不推荐使用的 sys.database_principal_aliases 的引用。 请使用角色而不是别名。 每次编译时发生。|  
|DATABASEPROPERTY|有一个语句引用了 DATABASEPROPERTY。 请将语句 DATABASEPROPERTY 更改为 DATABASEPROPERTYEX。 每次编译时发生。|  
|DATABASEPROPERTYEX('IsFullTextEnabled')|引用 DATABASEPROPERTYEX IsFullTextEnabled 属性的语句。 此属性的值无效。 用户数据库始终启用全文搜索。 请勿使用此属性。 每次编译时发生。|  
|DBCC [UN]PINTABLE|遇到 DBCC PINTABLE 或 DBCC UNPINTABLE 语句。 此语句不起作用，应删除。 每次查询时发生。|  
|DBCC DBREINDEX|遇到 DBCC DBREINDEX 语句。 请重写该语句以使用 ALTER INDEX 的 REBUILD 选项。 每次查询时发生。|  
|DBCC INDEXDEFRAG|遇到 DBCC INDEXDEFRAG 语句。 请重写该语句以使用 ALTER INDEX 的 REORGANIZE 选项。 每次查询时发生。|  
|DBCC SHOWCONTIG|遇到 DBCC SHOWCONTIG 语句。 有关此信息，请查询 sys.dm_db_index_physical_stats。 每次查询时发生。|  
|DEFAULT 关键字作为默认值|遇到使用 DEFAULT 关键字作为默认值的语法。 请勿使用。 每次编译时发生。|  
|不推荐使用的加密算法|下一版本的 SQL Server 中将删除不推荐使用的加密算法 rc4。 请避免在新的开发工作中使用此功能，并计划修改当前使用它的应用程序。 RC4 算法的安全性较低并且仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本中，可以通过任何兼容级别对使用 RC4 或 RC4_128 加密的材料进行解密。|  
|弃用的哈希算法|使用 MD2、MD4、MD5、SHA 或 SHA1 算法。|  
|DESX 算法|遇到了使用 DESX 加密算法的语法。 使用其他算法进行加密。 每次编译时发生。|  
|dm_fts_active_catalogs|因为没有弃用 sys.dm_fts_active_catalogs 视图的某些列，所以 dm_fts_active_catalogs 计数器一直保持为 0。 若要监视不推荐使用的列，请使用特定于列的计数器；例如，dm_fts_active_catalogs.is_paused。|  
|dm_fts_active_catalogs.is_paused|遇到 [sys.dm_fts_active_catalogs](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md) 动态管理视图的 is_paused 列。 请避免使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|dm_fts_active_catalogs.previous_status|遇到 sys.dm_fts_active_catalogs 动态管理视图的 previous_status 列。 请避免使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|dm_fts_active_catalogs.previous_status_description|遇到 sys.dm_fts_active_catalogs 动态管理视图的 previous_status_description 列。 请避免使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|dm_fts_active_catalogs.row_count_in_thousands|遇到 sys.dm_fts_active_catalogs 动态管理视图的 row_count_in_thousands 列。 请避免使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|dm_fts_active_catalogs.status|遇到 sys.dm_fts_active_catalogs 动态管理视图的 status 列。 请避免使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|dm_fts_active_catalogs.status_description|遇到 sys.dm_fts_active_catalogs 动态管理视图的 status_description 列。 请避免使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|dm_fts_active_catalogs.worker_count|遇到 sys.dm_fts_active_catalogs 动态管理视图的 worker_count 列。 请避免使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|dm_fts_memory_buffers|因为没有弃用 sys.dm_fts_memory_buffers 视图的大多数列，所以 dm_fts_memory_buffers 计数器一直保持为 0。 若要监视不推荐使用的列，请使用特定于列的计数器：dm_fts_memory_buffers.row_count。|  
|dm_fts_memory_buffers.row_count|遇到 [sys.dm_fts_memory_buffers](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md) 动态管理视图的 row_count 列。 请避免使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|DROP INDEX 具有两部分构成的名称|DROP INDEX 语法在 DROP INDEX 中包含 *table_name.index_name* 格式语法。 替换为 DROP INDEX 语句中的 *index_name* ON *table_name* 语法。 每次编译时发生。|  
|EXT_CREATE_ALTER_SOAP_ENDPOINT|遇到带有 FOR SOAP 选项的 CREATE 或 ALTER ENDPOINT 语句。 不推荐使用本机 XML Web 服务。 请改用 Windows Communications Foundation (WCF) 或 ASP.NET。|  
|EXT_endpoint_webmethods|遇到 sys.endpoint_webmethods。 不推荐使用本机 XML Web 服务。 请改用 Windows Communications Foundation (WCF) 或 ASP.NET。|  
|EXT_soap_endpoints|遇到 sys.soap_endpoints。 不推荐使用本机 XML Web 服务。 请改用 Windows Communications Foundation (WCF) 或 ASP.NET。|  
|EXTPROP_LEVEL0TYPE|遇到 level0type 的 TYPE。 请使用 SCHEMA 作为 level0type，使用 TYPE 作为 level1type。 每次查询时发生。|  
|EXTPROP_LEVEL0USER|level0type USER 还被指定了 level1type。 请仅使用 level0type 的 USER 作为用户的直接扩展属性。 每次查询时发生。|  
|FASTFIRSTROW|遇到 FASTFIRSTROW 语法。 请重写语句以使用 OPTION (FAST *n*) 语法。 每次编译时发生。|  
|FILE_ID|遇到 FILE_ID 语法。 请重写语句以使用 FILE_IDEX。 每次编译时发生。|  
|fn_get_sql|fn_get_sql 函数已编译。 请改用 sys.dm_exec_sql_text。 每次编译时发生。|  
|fn_servershareddrives|fn_servershareddrives 函数已编译。 请改用 sys.dm_io_cluster_shared_drives。 每次编译时发生。|  
|fn_virtualservernodes|fn_virtualservernodes 函数已编译。 请改用 sys.dm_os_cluster_nodes。 每次编译时发生。|  
|fulltext_catalogs|因为没有弃用 sys.fulltext_catalogs 视图的某些列，所以 fulltext_catalogs 计数器一直保持为 0。 若要监视不推荐使用的列，请使用特定于列的计数器；例如，fulltext_catalogs.data_space_id。 每次在服务器实例检测到对该列的引用时发生。|  
|fulltext_catalogs.data_space_id|遇到 [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) 目录视图的 data_space_id 列。 请勿使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|fulltext_catalogs.file_id|遇到 sys.fulltext_catalogs 目录视图的 file_id 列。 请勿使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|fulltext_catalogs.path|遇到 sys.fulltext_catalogs 目录视图的 path 列。 请勿使用此列。 每次在服务器实例检测到对该列的引用时发生。|  
|FULLTEXTCATALOGPROPERTY('LogSize')|遇到 FULLTEXTCATALOGPROPERTY 函数的 LogSize 属性。 请避免使用此属性。|  
|FULLTEXTCATALOGPROPERTY('PopulateStatus')|遇到 FULLTEXTCATALOGPROPERTY 函数的 PopulateStatus 属性。 请避免使用此属性。|  
|FULLTEXTSERVICEPROPERTY('ConnectTimeout')|遇到 FULLTEXTSERVICEPROPERTY 函数的 ConnectTimeout 属性。 请避免使用此属性。|  
|FULLTEXTSERVICEPROPERTY('DataTimeout')|遇到 FULLTEXTSERVICEPROPERTY 函数的 DataTimeout 属性。 请避免使用此属性。|  
|FULLTEXTSERVICEPROPERTY('ResourceUsage')|遇到 FULLTEXTSERVICEPROPERTY 函数的 ResourceUsage 属性。 请避免使用此属性。|  
|GROUP BY ALL|遇到 GROUP BY ALL 语法的总次数。 请修改语法以按特定表分组。|  
|Hindi|每次数据库启动时和每次排序规则使用时发生事件。 计划修改使用该排序规则的应用程序。 请改用 Indic_General_90。|  
|不带括号的 HOLDLOCK 表提示||  
|IDENTITYCOL|遇到 INDENTITYCOL 语法。 请重写语句以使用 $identity 语法。 每次编译时发生。|  
|不包含 COUNT_BIG(*) 的索引视图选择列表|聚集索引视图的选择列表必须包含 COUNT_BIG (\*)。|  
|INDEX_OPTION|遇到选项两侧没有括号的 CREATE TABLE、ALTER TABLE 或 CREATE INDEX 语法。 请重写语句以使用当前语法。 每次查询时发生。|  
|INDEXKEY_PROPERTY|遇到 INDEXKEY_PROPERTY 语法。 请重写语句以查询 sys.index_columns。 每次编译时发生。|  
|间接 TVF 提示|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的未来版本中将删除对通过视图执行的多语句表值函数 (TVF) 调用的间接应用表提示。|  
|将 NULL 插入 TIMESTAMP 列|NULL 值已插入到 TIMESTAMP 列。 请改用默认值。 每次编译时发生。|  
|INSERT_HINTS||  
|Korean_Wansung_Unicode|每次数据库启动时和每次排序规则使用时发生事件。 计划修改使用该排序规则的应用程序。|  
|Lithuanian_Classic|每次数据库启动时和每次排序规则使用时发生事件。 计划修改使用该排序规则的应用程序。|  
|马其顿语|每次数据库启动时和每次排序规则使用时发生事件。 计划修改使用该排序规则的应用程序。 请改用 Macedonian_FYROM_90。|  
|MODIFY FILEGROUP READONLY|遇到 MODIFY FILEGROUP READONLY 语法。 请重写语句以使用 READ_ONLY 语法。 每次编译时发生。|  
|MODIFY FILEGROUP READWRITE|遇到 MODIFY FILEGROUP READWRITE 语法。 请重写语句以使用 READ_WRITE 语法。 每次编译时发生。|  
|两个以上的部分构成的列名称|查询在列列表中使用了由 3 个部分或 4 个部分构成的名称。 请更改查询以使用标准兼容的由两部分构成的名称。 每次编译时发生。|  
|没有逗号的多个表提示|空格用作表提示之间的分隔符。 请改用逗号。 每次编译时发生。|  
|UPDATE 或 DELETE 中的 NOLOCK 或 READUNCOMMITTED|在 UPDATE 或 DELETE 语句的 FROM 子句中遇到 NOLOCK 或 READUNCOMMITTED。 请从 FROM 子句中删除 NOLOCK 或 READUNCOMMITTED 表提示。|  
|非 ANSI *= 或 =\* 外部联接运算符|遇到使用 *= 或 =\* 联接语法的语句。 请重写语句以使用 ANSI 联接语法。 每次编译时发生。|  
|numbered_stored_procedures||  
|numbered_procedure_parameters|遇到对不推荐使用的 sys.numbered_procedure_parameters 的引用。 请勿使用。 每次编译时发生。|  
|numbered_procedures|遇到对不推荐使用的 sys.numbered_procedure 的引用。 请勿使用。 每次编译时发生。|  
|旧式 RAISEERROR|遇到了已弃用的 RAISERROR（格式：RAISERROR 整数字符串）语法。 请使用当前的 RAISERROR 语法重写语句。 每次编译时发生。|  
|OLEDB 用于即席连接|SQLOLEDB 不是受支持的访问接口。 请将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 用于即席连接。|  
|PERMISSIONS|遇到对 PERMISSIONS 内部函数的引用。 请改为查询 sys.fn_my_permissions。 每次查询时发生。|  
|ProcNums|遇到不推荐使用的 ProcNums 语法。 请重写语句以删除引用。 每次编译时发生。|  
|READTEXT|遇到 READTEXT 语法。 请重写应用程序以使用 **varchar(max)** 数据类型和已删除的 **text** 数据类型语法。 每次查询时发生。|  
|RESTORE DATABASE 或 LOG WITH DBO_ONLY|RESTORE ...WITH DBO_ONLY 语法。 使用 RESTORE ...RESTRICTED_USER。|  
|RESTORE DATABASE 或 LOG WITH MEDIAPASSWORD|RESTORE ...WITH MEDIAPASSWORD 语法。 WITH MEDIAPASSWORD 提供弱安全性，应删除。|  
|RESTORE DATABASE 或 LOG WITH PASSWORD|RESTORE ...WITH PASSWORD 语法。 WITH PASSWORD 提供弱安全性，应删除。|  
|从触发器返回结果|每次触发器调用时发生此事件。 请重写该触发器以便不会返回结果集。|  
|ROWGUIDCOL|遇到 ROWGUIDCOL 语法。 请重写语句以使用 $rowguid 语法。 每次编译时发生。|  
|SET ANSI_NULLS OFF|遇到 SET ANSI_NULLS OFF 语法。 请删除此不推荐使用的语法。 每次编译时发生。|  
|SET ANSI_PADDING OFF|遇到 SET ANSI_PADDING OFF 语法。 请删除此不推荐使用的语法。 每次编译时发生。|  
|SET CONCAT_NULL_YIELDS_NULL OFF|遇到 SET CONCAT_NULL_YIELDS_NULL OFF 语法。 请删除此不推荐使用的语法。 每次编译时发生。|  
|SET DISABLE_DEF_CNST_CHK|遇到 SET DISABLE_DEF_CNST_CHK 语法。 此语法不起作用。 请删除此不推荐使用的语法。 每次编译时发生。|  
|SET FMTONLY ON|遇到 SET FMTONLY 语法。 请删除此不推荐使用的语法。 每次编译时发生。|  
|SET OFFSETS|遇到 SET OFFSETS 语法。 请删除此不推荐使用的语法。 每次编译时发生。|  
|SET REMOTE_PROC_TRANSACTIONS|遇到 SET REMOTE_PROC_TRANSACTIONS 语法。 请删除此不推荐使用的语法。 请改用链接服务器和 sp_serveroption。|  
|SET ROWCOUNT|在 DELETE、INSERT 或 UPDATE 语句中遇到 SET ROWCOUNT 语法。 请使用 TOP 重写该语句。 每次编译时发生。|  
|SETUSER|遇到 SET USER 语句。 请改用 EXECUTE AS。 每次查询时发生。|  
|sp_addapprole|遇到 sp_addapprole 过程。 请改用 CREATE APPLICATION ROLE。 每次查询时发生。|  
|sp_addextendedproc|遇到 sp_addextendedproc 过程。 请改用 CLR。 每次编译时发生。|  
|sp_addlogin|遇到 sp_addlogin 过程。 请改用 CREATE LOGIN。 每次查询时发生。|  
|sp_addremotelogin|遇到 sp_addremotelogin 过程。 请改用链接服务器。|  
|sp_addrole|遇到 sp_addrole 过程。 请改用 CREATE ROLE。 每次查询时发生。|  
|sp_addserver|遇到 sp_addserver 过程。 请改用链接服务器。|  
|sp_addtype|遇到 sp_addtype 过程。 请改用 CREATE TYPE。 每次编译时发生。|  
|sp_adduser|遇到 sp_adduser 过程。 请改用 CREATE USER。 每次查询时发生。|  
|sp_approlepassword|遇到 sp_approlepassword 过程。 请改用 ALTER APPLICATION ROLE。 每次查询时发生。|  
|sp_attach_db|遇到 sp_attach_db 过程。 请改用 CREATE DATABASE FOR ATTACH。 每次查询时发生。|  
|sp_attach_single_file_db|遇到 sp_single_file_db 过程。 请改用 CREATE DATABASE FOR ATTACH_REBUILD_LOG。 每次查询时发生。|  
|sp_bindefault|遇到 sp_bindefault 过程。 请改用 ALTER TABLE 或 CREATE TABLE 的 DEFAULT 关键字。 每次编译时发生。|  
|sp_bindrule|遇到 sp_bindrule 过程。 请改用 check 约束。 每次编译时发生。|  
|sp_bindsession|遇到 sp_bindsession 过程。 请改用多个活动结果集 (MARS) 或分布式事务。 每次编译时发生。|  
|sp_certify_removable|遇到 sp_certify_removable 过程。 请改用 sp_detach_db。 每次查询时发生。|  
|sp_changeobjectowner|遇到 sp_changeobjectowner 过程。 请改用 ALTER SCHEMA 或 ALTER AUTHORIZATION。 每次查询时发生。|  
|sp_change_users_login|遇到 sp_change_users_login 过程。 请改用 ALTER USER。 每次查询时发生。|  
|sp_configure 'allow updates'|遇到 sp_configure 的 allow updates 选项。 系统表不再可更新。 请勿使用。 每次查询时发生。|  
|sp_configure 'disallow results from triggers'|遇到 sp_configure 的 disallow result sets from triggers 选项。 若要禁止从触发器返回结果集，请使用 sp_configure 将该选项设置为 1。 每次查询时发生。|  
|sp_configure 'ft crawl bandwidth (max)'|遇到 sp_configure 的 ft crawl bandwidth (max) 选项。 请勿使用。 每次查询时发生。|  
|sp_configure 'ft crawl bandwidth (min)'|遇到 sp_configure 的 ft crawl bandwidth (min) 选项。 请勿使用。 每次查询时发生。|  
|sp_configure 'ft notify bandwidth (max)'|遇到 sp_configure 的 ft notify bandwidth (max) 选项。 请勿使用。 每次查询时发生。|  
|sp_configure 'ft notify bandwidth (min)'|遇到 sp_configure 的 ft notify bandwidth (min) 选项。 请勿使用。 每次查询时发生。|  
|sp_configure 'locks'|遇到 sp_configure 的 locks 选项。 锁不再可配置。 请勿使用。 每次查询时发生。|  
|sp_configure 'open objects'|遇到 sp_configure 的 open objects 选项。 打开对象的数目不再可配置。 请勿使用。 每次查询时发生。|  
|sp_configure 'priority boost'|遇到 sp_configure 的 priority boost 选项。 请勿使用。 每次查询时发生。 请改用 Windows start /high … program.exe 选项。|  
|sp_configure 'remote proc trans'|遇到 sp_configure 的 remote proc trans 选项。 请勿使用。 每次查询时发生。|  
|sp_configure 'set working set size'|遇到 sp_configure 的 set working set size 选项。 工作集大小不再可配置。 请勿使用。 每次查询时发生。|  
|sp_control_dbmasterkey_password|sp_control_dbmasterkey_password 存储过程不检查是否存在主密钥。 这是为了向后兼容，但会显示警告。 不推荐使用此行为。 在将来版本中，主密钥必须存在，并且在存储过程 sp_control_dbmasterkey_password 中使用的密码必须与用来对数据库主密钥进行加密的密码之一相同。|  
|sp_create_removable|遇到 sp_create_removable 过程。 请改用 CREATE DATABASE。 每次查询时发生。|  
|sp_db_vardecimal_storage_format|遇到 **vardecimal** 存储格式的使用。 请改用数据压缩。|  
|sp_dbcmptlevel|遇到 sp_dbcmptlevel 过程。 使用 ALTER DATABASE...SET COMPATIBILITY_LEVEL。 每次查询时发生。|  
|sp_dbfixedrolepermission|遇到 sp_dbfixedrolepermission 过程。 请勿使用。 每次查询时发生。|  
|sp_dboption|遇到 sp_dboption 过程。 请改用 ALTER DATABASE 和 DATABASEPROPERTYEX。 每次编译时发生。|  
|sp_dbremove|遇到 sp_dbremove 过程。 请改用 DROP DATABASE。 每次查询时发生。|  
|sp_defaultdb|遇到 sp_defaultdb 过程。 请改用 ALTER LOGIN。 每次编译时发生。|  
|sp_defaultlanguage|遇到 sp_defaultlanguage 过程。 请改用 ALTER LOGIN。 每次编译时发生。|  
|sp_denylogin|遇到 sp_denylogin 过程。 请改用 ALTER LOGIN DISABLE。 每次查询时发生。|  
|sp_depends|遇到 sp_depends 过程。 请改用 sys.dm_sql_referencing_entities 和 sys.dm_sql_referenced_entities。 每次查询时发生。|  
|sp_detach_db \@keepfulltextindexfile|在 sp_detach_db 语句中遇到 \@keepfulltextindexfile 参数。 请勿使用此参数。|  
|sp_dropalias|遇到 sp_dropalias 过程。 请将别名替换为用户帐户和数据库角色的组合。 请使用 sp_dropalias 删除已升级数据库中的别名。 每次编译时发生。|  
|sp_dropapprole|遇到 sp_dropapprole 过程。 请改用 DROP APPLICATION ROLE。 每次查询时发生。|  
|sp_dropextendedproc|遇到 sp_dropextendedproc 过程。 请改用 CLR。 每次编译时发生。|  
|sp_droplogin|遇到 sp_droplogin 过程。 请改用 DROP LOGIN。 每次查询时发生。|  
|sp_dropremotelogin|遇到 sp_dropremotelogin 过程。 请改用链接服务器。|  
|sp_droprole|遇到 sp_droprole 过程。 请改用 DROP ROLE。 每次查询时发生。|  
|sp_droptype|遇到 sp_droptype 过程。 请改用 DROP TYPE。|  
|sp_dropuser|遇到 sp_dropuser 过程。 请改用 DROP USER。 每次查询时发生。|  
|sp_estimated_rowsize_reduction_for_vardecimal|遇到 **vardecimal** 存储格式的使用。 请改用数据压缩和 sp_estimate_data_compression_savings。|  
|sp_fulltext_catalog|遇到 sp_fulltext_catalog 过程。 请改用 CREATE/ALTER/DROP FULLTEXT CATALOG。 每次编译时发生。|  
|sp_fulltext_column|遇到 sp_fulltext_column 过程。 请改用 ALTER FULLTEXT INDEX。 每次编译时发生。|  
|sp_fulltext_database|遇到 sp_fulltext_database 过程。 请改用 ALTER DATABASE。 每次编译时发生。|  
|sp_fulltext_service \@action=clean_up|遇到 sp_fulltext_service 过程的 clean_up 选项。 每次查询时发生。|  
|sp_fulltext_service \@action=connect_timeout|遇到 sp_fulltext_service 过程的 connect_timeout 选项。 每次查询时发生。|  
|sp_fulltext_service \@action=data_timeout|遇到 sp_fulltext_service 过程的 data_timeout 选项。 每次查询时发生。|  
|sp_fulltext_service \@action=resource_usage|遇到 sp_fulltext_service 过程的 resource_usage 选项。 此选项没有函数。 每次查询时发生。|  
|sp_fulltext_table|遇到 sp_fulltext_table 过程。 请改用 CREATE/ALTER/DROP FULLTEXT INDEX。 每次编译时发生。|  
|sp_getbindtoken|遇到 sp_getbindtoken 过程。 请改用多个活动结果集 (MARS) 或分布式事务。 每次编译时发生。|  
|sp_grantdbaccess|遇到 sp_grantdbaccess 过程。 请改用 CREATE USER。 每次查询时发生。|  
|sp_grantlogin|遇到 sp_grantlogin 过程。 请改用 CREATE LOGIN。 每次查询时发生。|  
|sp_help_fulltext_catalog_components|遇到 sp_help_fulltext_catalog_components 过程。 此过程返回空行。 请勿使用此过程。 每次编译时发生。|  
|sp_help_fulltext_catalogs|遇到 sp_help_fulltext_catalogs 过程。 请改为查询 sys.fulltext_catalogs。 每次编译时发生。|  
|sp_help_fulltext_catalogs_cursor|遇到 sp_help_fulltext_catalogs_cursor 过程。 请改为查询 sys.fulltext_catalogs。 每次编译时发生。|  
|sp_help_fulltext_columns|遇到 sp_help_fulltext_columns 过程。 请改为查询 sys.fulltext_index_columns。 每次编译时发生。|  
|sp_help_fulltext_columns_cursor|遇到 sp_help_fulltext_columns_cursor 过程。 请改为查询 sys.fulltext_index_columns。 每次编译时发生。|  
|sp_help_fulltext_tables|遇到 sp_help_fulltext_tables 过程。 请改为查询 sys.fulltext_indexes。 每次编译时发生。|  
|sp_help_fulltext_tables_cursor|遇到 sp_help_fulltext_tables_cursor 过程。 请改为查询 sys.fulltext_indexes。 每次编译时发生。|  
|sp_helpdevice|遇到 sp_helpdevice 过程。 请改为查询 sys.backup_devices。 每次查询时发生。|  
|sp_helpextendedproc|遇到 sp_helpextendedproc 过程。 请改用 CLR。 每次编译时发生。|  
|sp_helpremotelogin|遇到 sp_helpremotelogin 过程。 请改用链接服务器。|  
|sp_indexoption|遇到 sp_indexoption 过程。 请改用 ALTER INDEX。 每次编译时发生。|  
|sp_lock|遇到 sp_lock 过程。 请改为查询 sys.dm_tran_locks。 每次查询时发生。|  
|sp_password|遇到 sp_password 过程。 请改用 ALTER LOGIN。 每次查询时发生。|  
|sp_remoteoption|遇到 sp_remoteoption 过程。 请改用链接服务器。|  
|sp_renamedb|遇到 sp_renamedb 过程。 请改用 ALTER DATABASE。 每次查询时发生。|  
|sp_resetstatus|遇到 sp_resetstatus 过程。 请改用 ALTER DATABASE。 每次查询时发生。|  
|sp_revokedbaccess|遇到 sp_revokedbaccess 过程。 请改用 DROP USER。 每次查询时发生。|  
|sp_revokelogin|遇到 sp_revokelogin 过程。 请改用 DROP LOGIN。 每次查询时发生。|  
|sp_srvrolepermission|遇到不推荐使用的 sp_srvrolepermission 过程。 请勿使用。 每次查询时发生。|  
|sp_unbindefault|遇到 sp_unbindefault 过程。 请在 CREATE TABLE 或 ALTER TABLE 语句中改用 DEFAULT 关键字。 每次编译时发生。|  
|sp_unbindrule|遇到 sp_unbindrule 过程。 请使用 check 约束而不是规则。 每次编译时发生。|  
|SQL_AltDiction_CP1253_CS_AS|每次数据库启动时和每次排序规则使用时发生事件。 计划修改使用该排序规则的应用程序。|  
|字符串文字作为列别名|遇到在 SELECT 语句中包含用作列别名的字符串的语法（例如 `'string' = expression`）。 请勿使用。 每次编译时发生。|  
|sys.sql_dependencies|遇到对 sys.sql_dependencies 的引用。 请改用 sys.sql_expression_dependencies。 每次编译时发生。|  
|sysaltfiles|遇到对 sysaltfiles 的引用。 请改用 sys.master_files。 每次编译时发生。|  
|syscacheobjects|遇到对 syscacheobjects 的引用。 请改用 sys.dm_exec_cached_plans、sys.dm_exec_plan_attributes 和 sys.dm_exec_sql_text。 每次编译时发生。|  
|syscolumns|遇到对 syscolumns 的引用。 请改用 sys.columns。 每次编译时发生。|  
|syscomments|遇到对 syscomments 的引用。 请改用 sys.sql_modules。 每次编译时发生。|  
|sysconfigures|遇到对 sysconfigures 表的引用。 请改为引用 sys.sysconfigures 视图。 每次编译时发生。|  
|sysconstraints|遇到对 sysconstraints 的引用。请改用 sys.check_constraints、sys.default_constraints、sys.key_constraints 和 sys.foreign_keys。 每次编译时发生。|  
|syscurconfigs|遇到对 syscurconfigs 的引用。 请改用 sys.configurations。 每次编译时发生。|  
|sysdatabases|遇到对 sysdatabases 的引用。 请改用 sys.databases。 每次编译时发生。|  
|sysdepends|遇到对 sysdepends 的引用。 请改用 sys.sql_dependencies。 每次编译时发生。|  
|sysdevices|遇到对 sysdevices 的引用。 请改用 sys.backup_devices。 每次编译时发生。|  
|sysfilegroups|遇到对 sysfilegroups 的引用。 请改用 sys.filegroups。 每次编译时发生。|  
|sysfiles|遇到对 sysfiles 的引用。 请改用 sys.database_files。 每次编译时发生。|  
|sysforeignkeys|遇到对 sysforeignkeys 的引用。 请改用 sys.foreign_keys。 每次编译时发生。|  
|sysfulltextcatalogs|遇到对 sysfulltextcatalogs 的引用。 请改用 sys.fulltext_catalogs。 每次编译时发生。|  
|sysindexes|遇到对 sysindexes 的引用。 请改用 sys.indexes, sys.partitions、sys.allocation_units 和 sys.dm_db_partition_stats。 每次编译时发生。|  
|sysindexkeys|遇到对 sysindexkeys 的引用。 将改用 sys.index_columns。 每次编译时发生。|  
|syslockinfo|遇到对 syslockinfo 的引用。 请改用 sys.dm_tran_locks。 每次编译时发生。|  
|syslogins|遇到对 syslogins 的引用。 请改用 sys.server_principals 和 sys.sql_logins。 每次编译时发生。|  
|sysmembers|遇到对 sysmembers 的引用。 请改用 sys.database_role_members。 每次编译时发生。|  
|sysmessages|遇到对 sysmessages 的引用。 请改用 sys.messages。 每次编译时发生。|  
|sysobjects|遇到对 sysobjects 的引用。 请改用 sys.objects。 每次编译时发生。|  
|sysoledbusers|遇到对 sysoledbusers 的引用。 请改用 sys.linked_logins。 每次编译时发生。|  
|sysopentapes|遇到对 sysopentapes 的引用。 请改用 sys.dm_io_backup_tapes。 每次编译时发生。|  
|sysperfinfo|遇到对 sysperfinfo 的引用。 使用 sys.dm_os_performance_counters。 。 每次编译时发生。|  
|syspermissions|遇到对 syspermissions 的引用。 请改用 sys.database_permissions 和 sys.server_permissions。 每次编译时发生。|  
|sysprocesses|遇到对 sysprocesses 的引用。 请改用 sys.dm_exec_connections、sys.dm_exec_sessions 和 sys.dm_exec_requests。 每次编译时发生。|  
|sysprotects|遇到对 sysprotects 的引用。 请改用 sys.database_permissions 和 sys.server_permissions。 每次编译时发生。|  
|sysreferences|遇到对 sysreferences 的引用。 请改用 sys.foreign_keys。 每次编译时发生。|  
|sysremotelogins|遇到对 sysremotelogins 的引用。 请改用 sys.remote_logins。 每次编译时发生。|  
|sysservers|遇到对 sysservers 的引用。 请改用 sys.servers。 每次编译时发生。|  
|systypes|遇到对 systypes 的引用。 请改用 sys.types。 每次编译时发生。|  
|sysusers|遇到对 sysusers 的引用。 请改用 sys.database_principals。 每次编译时发生。|  
|不带 WITH 的表提示|遇到使用表提示但不使用 WITH 关键字的语句。 请修改语句以包括单词 WITH。 每次编译时发生。|  
|text in row 表选项|遇到对“text in row”表选项的引用。 请改用 sp_tableoption 'large value types out of row'。 每次查询时发生。|  
|TEXTPTR|遇到对 TEXTPTR 函数的引用。 请重写应用程序以使用 **varchar(max)** 数据类型和已删除的 **text**、 **ntext** 和 **image** 数据类型语法。 每次查询时发生。|  
|TEXTVALID|遇到对 TEXTVALID 函数的引用。 请重写应用程序以使用 **varchar(max)** 数据类型和已删除的 **text**、 **ntext** 和 **image** 数据类型语法。 每次查询时发生。|  
|TIMESTAMP|在 DDL 语句中遇到不推荐使用的 **timestamp** 数据类型的总次数。 请改用 **rowversion** 数据类型。|  
|UPDATETEXT 或 WRITETEXT|遇到 UPDATETEXT 或 WRITETEXT 语句。 请重写应用程序以使用 **varchar(max)** 数据类型和已删除的 **text**、 **ntext** 和 **image** 数据类型语法。 每次查询时发生。|  
|USER_ID|遇到对 USER_ID 函数的引用。 请改用 DATABASE_PRINCIPAL_ID 函数。 每次编译时发生。|  
|对链接服务器使用 OLEDB||  
|vardecimal 存储格式|遇到 **vardecimal** 存储格式的使用。 请改用数据压缩。|  
|XMLDATA|遇到 FOR XML 语法。 对于 RAW 和 AUTO 模式，请使用 XSD 生成。 显式模式无替代项。 每次编译时发生。|  
|XP_API|遇到扩展存储过程语句。 请勿使用。|  
|xp_grantlogin|遇到 xp_grantlogin 过程。 请改用 CREATE LOGIN。 每次编译时发生。|  
|xp_loginConfig|遇到 xp_loginconfig 过程。 请改用 SERVERPROPERTY 的 IsIntegratedSecurityOnly 参数。 每次查询时发生。|  
|xp_revokelogin|遇到 xp_revokelogin 过程。 请改用 ALTER LOGIN DISABLE 或 DROP LOGIN。 每次编译时发生。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 中不推荐使用的数据库引擎功能](../../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [SQL Server 2016 中不推荐使用的全文搜索功能](../../relational-databases/search/deprecated-full-text-search-features-in-sql-server-2016.md)   
 [Deprecation Announcement 事件类](../../relational-databases/event-classes/deprecation-announcement-event-class.md)   
 [Deprecation Final Support 事件类](../../relational-databases/event-classes/deprecation-final-support-event-class.md)   
 [SQL Server 2016 中废止的数据库引擎功能](../../database-engine/discontinued-database-engine-functionality-in-sql-server.md)   
 [SQL Server 2016 中废弃的全文搜索功能](../../database-engine/discontinued-database-engine-functionality-in-sql-server.md)   
 [使用 SQL Server 对象](../../relational-databases/performance-monitor/use-sql-server-objects.md)  
  
