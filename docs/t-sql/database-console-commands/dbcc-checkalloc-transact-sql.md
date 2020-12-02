---
description: DBCC CHECKALLOC (Transact-SQL)
title: DBCC CHECKALLOC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKALLOC_TSQL
- DBCC CHECKALLOC
- DBCC_CHECKALLOC_TSQL
- CHECKALLOC
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKALLOC statement
- checking database space allocation
- database space allocation [SQL Server]
- consistency [SQL Server], disk space allocations
- space allocation [SQL Server]
- allocation checks
- disk space [SQL Server], allocation consistency checks
- space allocation [SQL Server], checking
ms.assetid: bc1218eb-ffff-44ce-8122-6e4fa7d68a79
author: pmasl
ms.author: umajay
ms.openlocfilehash: b4631e88e76aeb9657327d9dbe4d6300b36b70df
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128601"
---
# <a name="dbcc-checkalloc-transact-sql"></a>DBCC CHECKALLOC (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

检查指定数据库的磁盘空间分配结构的一致性。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC CHECKALLOC   
[  
    ( database_name | database_id | 0   
      [ , NOINDEX   
      | , { REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD } ]  
    )  
    [ WITH   
        {   
          [ ALL_ERRORMSGS ]  
          [ , NO_INFOMSGS ]   
          [ , TABLOCK ]   
          [ , ESTIMATEONLY ]   
        }  
    ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 database_name | database_id | 0   
 要检查分配和页使用情况的数据库的名称或 ID。
如果未指定，或者指定为 0，则使用当前数据库。
数据库名必须遵循有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则。

 NOINDEX  
 指定不检查用户表的非聚集索引。<br>维护 NOINDEX 只是为了实现向后兼容性，并不会影响 DBCC CHECKALLOC。

 REPAIR_ALLOW_DATA_LOSS \| REPAIR_FAST \| REPAIR_REBUILD  
 指定 DBCC CHECKALLOC 修复找到的错误。 database_name 必须处于单用户模式。

 REPAIR_ALLOW_DATA_LOSS  
 试图修复找到的任何错误。 这些修复可能会导致一些数据丢失。 REPAIR_ALLOW_DATA_LOSS 是允许修复分配错误的唯一选项。

 REPAIR_FAST  
 保留语法只是为了向后兼容。 未执行修复操作。

 REPAIR_REBUILD  
 不适用。  
 仅将 REPAIR 选项作为最后手段使用。 若要修复错误，建议您通过备份进行还原。 修复操作不会考虑表本身或表之间可能存在的任何约束。 如果指定的表与一个或多个约束有关，建议您在修复操作后运行 DBCC CHECKCONSTRAINTS。 如果必须使用 REPAIR，则运行不带有修复选项的 DBCC CHECKDB 来查找要使用的修复级别。 如果使用 REPAIR_ALLOW_DATA_LOSS 级别，则建议您在运行带有此选项的 DBCC CHECKDB 之前备份数据库。

 WITH  
 启用要指定的选项。

 ALL_ERRORMSGS  
 显示所有错误消息。 默认情况下显示所有错误消息。 指定或省略此选项都不起作用。

 NO_INFOMSGS  
 取消所有信息性消息和关于所用空间的报告。

 TABLOCK  
 使 DBCC 命令获取排他数据库锁。

 ESTIMATEONLY  
 显示当指定所有其他选项时运行 DBCC CHECKALLOC 所需的估计 tempdb 空间大小。
  
## <a name="remarks"></a>备注  
DBCC CHECKALLOC 将检查数据库中所有页的分配，而不管其所属的页类型或对象类型。 它还可验证各种内部结构，这些结构可用于跟踪这些页以及它们之间的关系。
如果未指定 NO_INFOMSGS，则 DBCC CHECKALLOC 将收集有关数据库中所有对象的空间使用情况信息。 将这一信息与找到的任何错误一起进行打印。
  
> [!NOTE]  
> DBCC CHECKALLOC 功能包含在 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 和 [DBCC CHECKFILEGROUP](../../t-sql/database-console-commands/dbcc-checkfilegroup-transact-sql.md)中。 这意味着您不必将 DBCC CHECKALLOC 与这些语句分开运行。   DBCC CHECKALLOC 不会检查 FILESTREAM 数据。 FILESTREAM 在文件系统中存储二进制大型对象 (BLOB)。  
  
## <a name="internal-database-snapshot"></a>内部数据库快照  
DBCC CHECKALLOC 可使用内部数据库快照来提供执行这些检查所需的事务的一致性。 如果无法创建快照，或指定了 TABLOCK，则 DBCC CHECKALLOC 将尝试获取排他 (X) 数据库锁，以获取所需的一致性。
  
> [!NOTE]  
> 对 tempdb 运行 DBCC CHECKALLOC 不会执行任何检查。 这是因为，为了提高性能，不允许对 tempdb 使用数据库快照。 这意味着，无法获得所需的事务一致性。 停止和启动 MSSQLSERVER 服务可以解决任何 tempdb 分配问题。 此操作将删除并重新创建 tempdb 数据库。  
  
## <a name="understanding-dbcc-error-messages"></a>了解 DBCC 错误消息  
DBCC CHECKALLOC 命令完成后，会将一条消息写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志。 如果 DBCC 命令成功执行，则消息指示成功完成以及命令运行的时间。 如果 DBCC 命令在完成检查之前由于错误而停止，则消息将指示命令已终止，并指示状态值和命令运行的时间。 下表列出并说明了此消息中可包含的状态值。
  
|状态|说明|  
|---|---|  
|0|出现错误号 8930。 这指示导致 DBCC 命令终止的元数据损坏。|  
|1|出现错误号 8967。 存在一个内部 DBCC 错误。|  
|2|在紧急模式数据库修复过程中出错。|  
|3|这指示导致 DBCC 命令终止的元数据损坏。|  
|4|检测到断言或访问违规。|  
|5|出现终止了 DBCC 命令的未知错误。|  
  
## <a name="error-reporting"></a>错误报告  
只要 DBCC CHECKALLOC 检测到损坏错误，就将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] LOG 目录中创建微型转储文件 (SQLDUMPnnnn.txt)。 如果为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启用了“功能使用情况数据收集”和“错误报告”功能，该文件将被自动转发给 [!INCLUDE[msCoName](../../includes/msconame-md.md)]。 收集的数据将用于改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。
转储文件包含 DBCC CHECKALLOC 命令的结果以及其他诊断输出数据。 该文件拥有任意访问控制列表 (DACL)。 只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户和 sysadmin 角色的成员有权进行访问。 默认情况下，sysadmin 角色包含 Windows BUILTIN\Administrators 组和本地管理员组的所有成员。 如果数据收集进程失败，DBCC 命令不会失败。
  
## <a name="resolving-errors"></a>纠正错误  
如果 DBCC CHECKALLOC 报告了任何错误，则建议您通过数据库备份来还原该数据库，而不是运行修复。 如果备份不存在，则运行修复也可纠正报告的错误；但是，纠正这些错误时可能需要删除某些页，进而删除数据。
修复可在用户事务内执行。 允许回滚所做的更改。 如果回滚所做的更改，则数据库仍将包含错误，因此必须通过备份进行还原。 修复完成后，备份该数据库。
  
## <a name="result-sets"></a>结果集  
下表说明了 DBCC CHECKALLOC 返回的信息。
  
|Item|说明|  
|---|---|  
|FirstIAM|仅限内部使用。|  
|Root|仅限内部使用。|  
|Dpages|数据页计数。|  
|Pages used|分配的页。|  
|Dedicated extents|分配给对象的区数。<br /><br /> 如果使用混合分配页，则可能有未分配区数的页。|  
  
DBCC CHECKALLOC 还会报告每条索引和每个文件中分区的分配摘要。 此摘要说明了数据的分布情况。
  
|Item|说明|  
|---|---|  
|Reserved pages|分配给索引的页和已分配区数中未使用的页。|  
|Used pages|分配给索引和索引正在使用的页。|  
|分区 ID|仅限内部使用。|  
|Alloc unit ID|仅限内部使用。|  
|行内数据|页包含索引或堆数据。|  
|LOB 数据|页包含 varchar(max)、nvarchar(max)、text、ntext，xml 和 image 数据。|  
|行溢出数据|页包含已推送到行外的可变长度列数据。|  
  
DBCC CHECKALLOC 将返回以下结果集（值可能有所不同），指定了 ESTIMATEONLY 或 NO_INFOMSGS 时除外。
  
```
DBCC results for 'master'.  
***************************************************************  
Table sysobjects                Object ID 1.  
Index ID 1         FirstIAM (1:11)   Root (1:12)    Dpages 22.  
    Index ID 1. 24 pages used in 5 dedicated extents.  
Index ID 2         FirstIAM (1:1368)   Root (1:1362)    Dpages 10.  
    Index ID 2. 12 pages used in 2 dedicated extents.  
Index ID 3         FirstIAM (1:1392)   Root (1:1408)    Dpages 4.  
    Index ID 3. 6 pages used in 0 dedicated extents.  
Total number of extents is 7.  
***************************************************************  
'...'  
***************************************************************  
Table spt_server_info                Object ID 1938105945.  
Index ID 1         FirstIAM (1:520)   Root (1:508)    Dpages 1.  
    Index ID 1. 3 pages used in 0 dedicated extents.  
Total number of extents is 0.  
***************************************************************  
Processed 52 entries in sysindexes for database ID 1.  
File 1. Number of extents = 210, used pages = 1126, reserved pages = 1280.  
           File 1 (number of mixed extents = 73, mixed pages = 184).  
    Object ID 1, Index ID 0, data extents 5, pages 24, mixed extent pages 9.  
'...'  
    Object ID 1938105945, Index ID 0, data extents 0, pages 3, mixed extent pages 3.  
Total number of extents = 210, used pages = 1126, reserved pages = 1280 in this database.  
       (number of mixed extents = 73, mixed pages = 184) in this database.  
CHECKALLOC found 0 allocation errors and 0 consistency errors in database 'master'.  
DBCC results for 'master'.  
***************************************************************  
Table sys.sysrowsetcolumns                Object ID 4.  
Index ID 1, partition ID 262144, alloc unit ID 262144 (type In-row data). FirstIAM (1:98). Root (1:94). Dpages 7.  
Index ID 1, partition ID 262144, alloc unit ID 262144 (type In-row data). 9 pages used in 1 dedicated extents.  
Index ID 1, partition ID 262144, alloc unit ID 262398 (type Row-overflow data). FirstIAM (0:0). Root (0:0). Dpages 0.  
Index ID 1, partition ID 262144, alloc unit ID 262398 (type Row-overflow data). 0 pages used in 0 dedicated extents.  
Total number of extents is 1.  
...  
***************************************************************  
Processed 201 entries in system catalog for database ID 1.  
File 1. Number of extents = 44, used pages = 300, reserved pages = 345.  
           File 1 (number of mixed extents = 29, mixed pages = 225).  
    Object ID 4, index ID 1, partition ID 262144, alloc unit ID 262144 (type In-row data), data extents 1, pages 9, mixed extent pages 8.  
    Object ID 5, index ID 1, partition ID 327680, alloc unit ID 327680 (type In-row data), data extents 0, pages 2, mixed extent pages 2.  
    Object ID 7, index ID 1, partition ID 458752, alloc unit ID 458752 (type In-row data), data extents 0, pages 5, mixed extent pages 5.  
    Object ID 8, index ID 0, partition ID 524288, alloc unit ID 524288 (type In-row data), data extents 0, pages 2, mixed extent pages 2.  
    Object ID 13, index ID 1, partition ID 851968, alloc unit ID 851968 (type In-row data), data extents 1, pages 9, mixed extent pages 8.  
    Object ID 15, index ID 1, partition ID 983040, alloc unit ID 983040 (type In-row data), data extents 0, pages 2, mixed extent pages 2.  
    Object ID 26, index ID 1, partition ID 281474978414592, alloc unit ID 1703937 (type In-row data), data extents 0, pages 3, mixed extent pages 3.  
    Object ID 27, index ID 1, partition ID 281474978480128, alloc unit ID 1769473 (type In-row data), data extents 0, pages 3, mixed extent pages 3.  
    Object ID 27, index ID 2, partition ID 562949955190784, alloc unit ID 1769474 (type In-row data), index extents 0, pages 3, mixed extent pages 3.  
...  
    Object ID 1179151246, index ID 1, partition ID 72057594038845440, alloc unit ID 13435136 (type In-row data), data extents 2, pages 18, mixed extent pages 8.  
    Object ID 1179151246, index ID 2, partition ID 72057594038910976, alloc unit ID 13566208 (type In-row data), index extents 1, pages 16, mixed extent pages 8.  
    Object ID 1911677858, index ID 0, partition ID 72057594039631872, alloc unit ID 15073536 (type In-row data), data extents 0, pages 2, mixed extent pages 2.  
Total number of extents = 41, used pages = 289, reserved pages = 323 in this database.  
       (number of mixed extents = 27, mixed pages = 211) in this database.  
CHECKALLOC found 0 allocation errors and 0 consistency errors in database 'master'.  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
如果指定了 ESTIMATEONLY，则 DBCC CHECKALLOC 将返回以下结果集。
  
```
Estimated TEMPDB space needed for CHECKALLOC (KB)   
-------------------------------------------------   
34  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>权限  
要求具有 sysadmin 固定服务器角色或 db_owner 固定数据库角色的成员身份。
  
## <a name="examples"></a>示例  
下面的示例将对当前数据库和 `DBCC CHECKALLOC` 数据库执行 `AdventureWorks2012`。
  
```sql  
-- Check the current database.  
DBCC CHECKALLOC;  
GO  
-- Check the AdventureWorks2012 database.  
DBCC CHECKALLOC (AdventureWorks2012);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
  

