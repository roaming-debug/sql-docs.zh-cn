---
description: DATABASEPROPERTYEX (Transact-SQL)
title: DATABASEPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/23/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATABASEPROPERTYEX
- DATABASEPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DATABASEPROPERTYEX function
- displaying database properties
- database properties [SQL Server]
ms.assetid: 8a9e0ffb-28b5-4640-95b2-a54e3e5ad941
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f7911d4f096c8b90b2ee01341405acd561c46ec9
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96443128"
---
# <a name="databasepropertyex-transact-sql"></a>DATABASEPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中指定的数据库，此函数返回指定数据库选项或属性的当前设置。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATABASEPROPERTYEX ( database , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*database*  
一个表达式，用于指定 `DATABASEPROPERTYEX` 要返回其命名属性信息的数据库的名称。 database 的数据类型为 nvarchar(128)。  

对于 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]，`DATABASEPROPERTYEX` 需要当前数据库的名称。 如果提供其他数据库的名称，所有属性都返回为 NULL。
  
*property*  
一个表达式，用于指定要返回的数据库属性的名称。 property 的数据类型为 varchar(128)，并支持下表中的某个值：
  
> [!NOTE]  
>  如果数据库尚未启动，`DATABASEPROPERTYEX` 通过直接访问数据库检索这些值（而不是从元数据检索）时，调用 `DATABASEPROPERTYEX` 会返回 NULL。 AUTO_CLOSE 设置为“开”或脱机的数据库即“未启动”。  
  
|properties|说明|返回的值|  
|---|---|---|
|排序规则|数据库的默认排序规则名称。|排序规则名称<br /><br /> NULL：数据库未启动。<br /><br /> 基本数据类型：nvarchar(128)|  
|ComparisonStyle|排序规则的 Windows 比较样式。 使用以下样式值生成已完成 ComparisonStyle 值的位图：<br /><br /> 忽略大小写：1<br /><br /> 忽略重音：2<br /><br /> 忽略假名：65536<br /><br /> 忽略宽度：131072<br /><br /> <br /><br /> 例如，196609 的默认值是将忽略大小写、忽略假名和忽略宽度选项合并在一起的结果。|返回比较样式。<br /><br /> 对所有二进制排序规则均返回 0。<br /><br /> 基本数据类型：int|  
|版本|数据库版本或服务层。|适用范围：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]、[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]。<br /><br /> <br /><br /> 常规用途<br /><br /> 业务关键<br /><br /> 基本<br /><br /> 标准<br /><br /> Premium<br /><br /> 系统（针对 master 数据库）<br /><br /> NULL：数据库未启动。<br /><br /> 基本数据类型：nvarchar(64)|  
|IsAnsiNullDefault|数据库遵循 ISO 规则，允许 Null 值。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsAnsiNullsEnabled|所有与 Null 的比较将取值为未知。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsAnsiPaddingEnabled|在比较或插入前，字符串将被填充到相同长度。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsAnsiWarningsEnabled|发生标准错误条件时，SQL Server 会发出错误消息或警告消息。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsArithmeticAbortEnabled|如果执行查询时发生溢出或被零除错误，则将结束查询。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsAutoClose|在最后一个用户退出后，数据库完全关闭并释放资源。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsAutoCreateStatistics|查询优化器根据需要创建单列统计信息以提高查询性能。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsAutoCreateStatisticsIncremental|条件允许时，自动创建的单列统计信息递增。|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsAutoShrink|可以定期自动收缩数据库文件。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsAutoUpdateStatistics|查询使用的现有统计信息可能已过期时，查询优化器更新这些统计信息。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：输入无效<br /><br /> 基本数据类型：int|
|IsClone|数据库是使用 DBCC CLONEDATABASE 创建的用户数据库的仅限架构和仅限统计数据副本。 有关详细信息，请参阅 [Microsoft 支持文章](https://support.microsoft.com/help/3177838)。|**适用对象**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 和更高版本。<br /><br /> 1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int| 
|IsCloseCursorsOnCommitEnabled|提交事务后，会关闭所有打开的游标。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsFulltextEnabled|支持对数据库进行全文和语义索引。|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> <br /><br /> 1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：输入无效<br /><br /> 基本数据类型：int<br /><br /> **注意：** 此属性的值现在无效。 用户数据库始终启用全文搜索。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的后续版本会删除此属性。 请不要在新的开发工作中使用此属性，并尽快修改当前仍在使用此属性的应用程序。|  
|IsInStandBy|数据库以只读方式联机，并允许还原日志。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsLocalCursorsDefault|游标声明默认为 LOCAL。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsMemoryOptimizedElevateToSnapshotEnabled|在会话设置 TRANSACTION ISOLATION LEVEL 设置为 READ COMMITTED、READ UNCOMMITTED 或较低的隔离级别时，使用 SNAPSHOT 隔离访问内存优化表。|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> <br /><br /> 1：TRUE<br /><br /> 0：FALSE<br /><br /> 基本数据类型：int|  
|IsMergePublished|如果安装了复制，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持发布数据库表供合并复制使用。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsNullConcat|Null 串联操作数产生 NULL。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsNumericRoundAbortEnabled|表达式中缺少精度时将产生错误。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsParameterizationForced|PARAMETERIZATION 数据库 SET 选项为 FORCED。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入|  
|IsQuotedIdentifiersEnabled|允许对标识符使用英文双引号。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsPublished|如果安装了复制，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持发布数据库表供快照复制或事务复制使用。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsRecursiveTriggersEnabled|已启用触发器递归触发。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsSubscribed|数据库已订阅发布。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsSyncWithBackup|数据库为发布数据库或分发数据库，并且支持在不中断事务复制的情况下还原。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int|  
|IsTornPageDetectionEnabled|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]检测到因电力故障或其他系统故障造成的不完全 I/O 操作。|1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int| 
|IsVerifiedClone|数据库是使用 DBCC CLONEDATABASE 的 WITH VERIFY_CLONEDB 选项创建的用户数据库的仅限架构和仅限统计信息副本。 有关详细信息，请参阅此 [Microsoft 支持文章](https://support.microsoft.com/help/3177838)。|**适用对象**：从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 开始。<br /><br /> <br /><br /> 1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：int| 
|IsXTPSupported|指示数据库是否支持内存中 OLTP，即创建和使用内存优化表和本机编译模块。<br /><br /> 特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]：<br /><br /> IsXTPSupported 与任何 MEMORY_OPTIMIZED_DATA 文件组的存在与否无关，创建内存中 OLTP 对象则需要该文件组。|**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本），以及 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。<br /><br /> 1：TRUE<br /><br /> 0：FALSE<br /><br /> NULL：输入无效，出现错误或不适用<br /><br /> 基本数据类型：int|  
|LastGoodCheckDbTime|在指定数据库上运行的上一成功 DBCC CHECKDB 的日期和时间。<sup>1</sup> 如果 DBCC CHECKDB 未在数据库上运行，则返回 1900-01-01 00:00:00.000。|适用于：从 SP2 开始的 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]。</br>从 CU9 开始的 [!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)]。</br>[!INCLUDE[sssqlv15](../../includes/sssqlv15-md.md)] 或更高版本。</br>Azure SQL 数据库。<br/><br/>日期时间值。<br /><br /> NULL：无效输入<br /><br /> 基本数据类型：datetime| 
|LCID|排序规则的 Windows 区域设置标识符 (LCID)。|LCID 值（十进制格式）。<br /><br /> 基本数据类型：int|  
|MaxSizeInBytes|最大数据库大小（以字节为单位）。|适用范围：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]、[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]。<br /><br />[Azure SQL 数据库和 Azure Synapse Analytics](/azure/sql-database/sql-database-single-database-scale#dtu-based-purchasing-model) - 除非已购买额外的存储空间，否则值基于 SLO。<br /><br />[vCore](/azure/sql-database/sql-database-single-database-scale#vcore-based-purchasing-model) - 值以 1 GB 为增量递增，直到达到最大大小。<br /><br />NULL：数据库未启动<br /><br /> 基本数据类型：bigint|  
|恢复|数据库恢复模式|FULL：完整恢复模式<br /><br /> BULK_LOGGED：大容量日志模型<br /><br /> SIMPLE：简单恢复模式<br /><br /> 基本数据类型：nvarchar(128)|  
|ServiceObjective|描述 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]或 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 中的数据库的性能级别。|下列类型作之一：<br /><br /> NULL：数据库没有启动<br /><br /> 已共享（针对 Web/企业版本）<br /><br /> 基本<br /><br /> S0<br /><br /> S1<br /><br /> S2<br /><br /> S3<br /><br /> P1<br /><br /> P2<br /><br /> P3<br /><br /> ElasticPool<br /><br /> 系统（针对主数据库）<br /><br /> 基本数据类型：nvarchar(32)|  
|ServiceObjectiveId|[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 中的服务器目标 ID。|uniqueidentifier 确定服务目标。|  
|SQLSortOrder|SQL Server 早期版本中支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序顺序 ID。|0：数据库使用的是 Windows 排序规则<br /><br /> >0：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序顺序 ID<br /><br /> NULL：输入无效或数据库未启动<br /><br /> 基本数据类型：tinyint|  
|状态|数据库状态。|ONLINE：数据库可用于查询。<br /><br /> **注意：** 当数据库打开但尚未恢复时，该函数可能返回 ONLINE 状态。 要确定 ONLINE 数据库是否可以接受连接，可以查询 DATABASEPROPERTYEX 的 Collation 属性。 在数据库排序规则返回非 Null 值之后，ONLINE 数据库就可以接受连接了。 对于 Always On 数据库，可以查询 `sys.dm_hadr_database_replica_states` 的 database_state 或 database_state_desc 列。<br /><br /> OFFLINE：已将数据库显式置于脱机状态。<br /><br /> RESTORING：已启动数据库还原。<br /><br /> RECOVERING：已启动数据库还原，且数据库尚未准备好进行查询。<br /><br /> SUSPECT：数据库未恢复。<br /><br /> EMERGENCY：数据库处于紧急只读状态。 只有 sysadmin 成员可进行访问。<br /><br /> 基本数据类型：nvarchar(128)|  
|Updateability|指示是否可以修改数据。|READ_ONLY：数据库支持数据读取，但不支持数据修改。<br /><br /> READ_WRITE：数据库支持数据读取和修改。<br /><br /> 基本数据类型：nvarchar(128)|  
|UserAccess|指示哪些用户可以访问数据库。|SINGLE_USER：一次仅限一个 db_owner、dbcreator 或 sysadmin 用户<br /><br /> RESTRICTED_USER：仅限 db_owner、dbcreator 或 sysadmin 角色的成员<br /><br /> MULTI_USER：所有用户<br /><br /> 基本数据类型：nvarchar(128)|  
|版本|用于创建数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代码的内部版本号。 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|版本号：数据库处于打开状态。<br /><br /> NULL：数据库尚未启动。<br /><br /> 基本数据类型：int| 

<br/>   

> [!NOTE]  
> <sup>1</sup> 对于属于某可用性组的数据库，`LastGoodCheckDbTime` 将返回在主要副本上运行的上一成功 DBCC CHECKDB 的日期和时间，而不管从哪个副本运行命令。 

## <a name="return-types"></a>返回类型
**sql_variant**
  
## <a name="exceptions"></a>例外  
出现错误时或调用方没有查看对象的权限时将返回 NULL。
  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，用户只能查看其拥有的安全对象的元数据，或者已对其授予权限的安全对象的元数据。 这意味着，如果用户对该对象没有任何权限，那些发出元数据的内置函数（如 `OBJECT_ID`）可能会返回 NULL。 有关详细信息，请参阅[元数据可见性配置](../../relational-databases/security/metadata-visibility-configuration.md)。
  
## <a name="remarks"></a>备注  
`DATABASEPROPERTYEX` 一次只返回一个属性设置。 若要显示多个属性设置，请使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图。
  
## <a name="examples"></a>示例  
  
### <a name="a-retrieving-the-status-of-the-auto_shrink-database-option"></a>A. 检索 AUTO_SHRINK 数据库选项的状态  
以下示例返回 `AdventureWorks` 数据库的 AUTO_SHRINK 数据库选项的状态。
  
```sql
SELECT DATABASEPROPERTYEX('AdventureWorks2014', 'IsAutoShrink');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]该结果集指示 AUTO_SHRINK 已关闭。
  
```
------------------  
0  
```  
  
### <a name="b-retrieving-the-default-collation-for-a-database"></a>B. 检索数据库的默认排序规则  
以下示例返回 `AdventureWorks` 数据库的多个属性。
  
```sql
SELECT   
    DATABASEPROPERTYEX('AdventureWorks2014', 'Collation') AS Collation,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'Edition') AS Edition,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'ServiceObjective') AS ServiceObjective,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'MaxSizeInBytes') AS MaxSizeInBytes  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Collation                     Edition        ServiceObjective  MaxSizeInBytes  
----------------------------  -------------  ----------------  --------------  
SQL_Latin1_General_CP1_CI_AS  DataWarehouse  DW1000            5368709120  
```  
  
## <a name="see-also"></a>另请参阅
[ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
[数据库状态](../../relational-databases/databases/database-states.md)  
[sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)
  
  
