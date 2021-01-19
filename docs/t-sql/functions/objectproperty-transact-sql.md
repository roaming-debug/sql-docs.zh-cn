---
description: OBJECTPROPERTY (Transact-SQL)
title: OBJECTPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OBJECTPROPERTY
- OBJECTPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying schema-scoped object information
- viewing schema-scoped object information
- OBJECTPROPERTY function
- schema-scoped objects [SQL Server]
- objects [SQL Server], schema-scoped
ms.assetid: 27569888-f8b5-4cec-a79f-6ea6d692b4ae
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 652d705c92aede4592a079733c8dd281e9dd8e91
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170529"
---
# <a name="objectproperty-transact-sql"></a>OBJECTPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回当前数据库中架构范围内的对象的相关信息。 有关架构范围内对象的列表，请参阅 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。 不能将此函数用于不属于架构范围内的对象，如数据定义语言 (DDL) 触发器和事件通知。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
OBJECTPROPERTY ( id , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *id*  
 是表示当前数据库中对象 ID 的表达式。 id 的数据类型为 int，并假定为当前数据库上下文中的架构范围内的对象。  
  
 *property*  
 一个表达式，提供 id 指定的对象的返回信息。property 可以是下列值之一。  
  
> [!NOTE]  
>  除非另外注明，否则出现以下情况时将返回 NULL：property 不是有效的属性名称；id 不是有效的对象 ID；id 不是指定属性支持的对象类型；调用方无权查看对象的元数据。  
  
|属性名称|对象类型|说明和返回的值|  
|-------------------|-----------------|-------------------------------------|  
|CnstIsClustKey|约束|具有聚集索引的 PRIMARY KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsColumn|约束|单个列上的 CHECK、DEFAULT 或 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsDeleteCascade|约束|具有 ON DELETE CASCADE 选项的 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsDisabled|约束|禁用的约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNonclustKey|约束|具有非聚集索引的 PRIMARY KEY 或 UNIQUE 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNotRepl|约束|约束是使用 NOT FOR REPLICATION 关键字定义的。<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsNotTrusted|约束|启用约束时未检查现有行，因此可能不是所有行都适用该约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|CnstIsUpdateCascade|约束|具有 ON UPDATE CASCADE 选项的 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsAfterTrigger|触发器|AFTER 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsAnsiNullsOn|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|创建时的 ANSI_NULLS 设置。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsDeleteTrigger|触发器|DELETE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstDeleteTrigger|触发器|对表执行 DELETE 时触发的第一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstInsertTrigger|触发器|对表执行 INSERT 时触发的第一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsFirstUpdateTrigger|触发器|对表执行 UPDATE 时触发的第一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsInsertTrigger|触发器|INSERT 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsInsteadOfTrigger|触发器|INSTEAD OF 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastDeleteTrigger|触发器|对表执行 DELETE 时触发的最后一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastInsertTrigger|触发器|对表执行 INSERT 时触发的最后一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsLastUpdateTrigger|触发器|对表执行 UPDATE 时触发的最后一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsQuotedIdentOn|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|创建时的 QUOTED_IDENTIFIER 设置。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsStartup|过程|启动过程。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsTriggerDisabled|触发器|禁用的触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsTriggerNotForRepl|触发器|定义为 NOT FOR REPLICATION 的触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsUpdateTrigger|触发器|UPDATE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|ExecIsWithNativeCompilation|[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 本机编译过程。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|HasAfterTrigger|表、视图|表或视图具有 AFTER 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasDeleteTrigger|表、视图|表或视图具有 DELETE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasInsertTrigger|表、视图|表或视图具有 INSERT 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasInsteadOfTrigger|表、视图|表或视图具有 INSTEAD OF 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|HasUpdateTrigger|表、视图|表或视图具有 UPDATE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsAnsiNullsOn|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、表、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|指定表的 ANSI NULLS 选项设置为 ON。 这表示所有对空值的比较都取值为 UNKNOWN。 只要表存在，此设置就会应用于表定义中的所有表达式，包括计算列和约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsCheckCnst|任何架构范围内的对象|CHECK 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsConstraint|任何架构范围内的对象|列或表的单列 CHECK、DEFAULT 或 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDefault|任何架构范围内的对象|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 绑定的默认值。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDefaultCnst|任何架构范围内的对象|DEFAULT 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsDeterministic|函数、视图|函数或视图的确定性属性。<br /><br /> 1 = 确定<br /><br /> 0 = 不确定|  
|IsEncrypted|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、表、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|指示模块语句的原始文本已转换为模糊格式。 模糊代码的输出在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 的任何目录视图中都不能直接显示。 对系统表或数据库文件没有访问权限的用户不能检索模糊文本。 但是，能通过 [DAC 端口](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)访问系统表的用户或能够直接访问数据库文件的用户可以使用此文本。 此外，能够向服务器进程附加调试器的用户可在运行时从内存中检索原始过程。<br /><br /> 1 = 已加密<br /><br /> 0 = 未加密<br /><br /> 基本数据类型：int|  
|IsExecuted|任何架构范围内的对象|可执行对象（视图、过程、函数或触发器）。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsExtendedProc|任何架构范围内的对象|扩展过程。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsForeignKey|任何架构范围内的对象|FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsIndexed|表、视图|包含索引的表或视图。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsIndexable|表、视图|可创建索引的表或视图。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsInlineFunction|函数|内联函数。<br /><br /> 1 = 内联函数<br /><br /> 0 = 非内联函数|  
|IsMSShipped|任何架构范围内的对象|安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 过程中创建的对象。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsPrimaryKey|任何架构范围内的对象|PRIMARY KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> NULL = 非函数，或对象 ID 无效。|  
|IsProcedure|任何架构范围内的对象|过程。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsQuotedIdentOn|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、表、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图、CHECK 约束、DEFAULT 定义|指定对象的引号标识符设置为 ON。 这表示用英文双引号分隔对象定义中涉及的所有表达式中的标识符。<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|IsQueue|任何架构范围内的对象|Service Broker 队列<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsReplProc|任何架构范围内的对象|复制过程。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsRule|任何架构范围内的对象|绑定规则。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsScalarFunction|函数|标量值函数。<br /><br /> 1 = 标量值函数<br /><br /> 0 = 非标量值函数|  
|IsSchemaBound|函数、视图|使用 SCHEMABINDING 创建的绑定到架构的函数或视图。<br /><br /> 1 = 绑定到架构<br /><br /> 0 = 不绑定到架构。|  
|IsSystemTable|表|系统表。<br /><br /> 1 = True<br /><br /> 0 = False| 
|IsSystemVerified|Object|SQL Server 可以验证对象的确定性和精度属性。<br /><br /> 1 = True<br /><br /> 0 = False| 
|IsTable|表|表。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsTableFunction|函数|表值函数。<br /><br /> 1 = 表值函数<br /><br /> 0 = 非表值函数|  
|IsTrigger|任何架构范围内的对象|触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsUniqueCnst|任何架构范围内的对象|UNIQUE 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsUserTable|表|用户定义的表。<br /><br /> 1 = True<br /><br /> 0 = False|  
|IsView|视图|视图。<br /><br /> 1 = True<br /><br /> 0 = False|  
|OwnerId|任何架构范围内的对象|对象的所有者。<br /><br /> 注意：架构所有者不一定是对象所有者。 例如，子对象（其 parent_object_id 为非 null）将始终返回与父对象相同的所有者 ID。<br /><br /> Nonnull = 对象所有者的数据库用户 ID。|  
|SchemaId|任何架构范围内的对象| 对象所属架构的架构 ID。| 
|TableDeleteTrigger|表|表具有 DELETE 触发器。<br /><br /> >1 = 指定类型的第一个触发器的 ID。|  
|TableDeleteTriggerCount|表|表具有指定数目的 DELETE 触发器。<br /><br /> >0 = DELETE 触发器数目。|  
|TableFullTextMergeStatus|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表所具有的全文检索当前是否正在合并。<br /><br /> 0 = 表没有全文检索，或者全文检索未在合并。<br /><br /> 1 = 全文检索正在合并。|  
|TableFullTextBackgroundUpdateIndexOn|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表已启用全文后台更新索引（自动更改跟踪）。<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE|  
|TableFulltextCatalogId|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表的全文检索数据所在的全文目录的 ID。<br /><br /> 非零 = 全文目录 ID，它与全文检索表中标识行的唯一索引相关。<br /><br /> 0 = 表没有全文检索。|  
|TableFulltextChangeTrackingOn|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表已启用全文更改跟踪。<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE|  
|TableFulltextDocsProcessed|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 自开始全文检索以来所处理的行数。 在为进行全文搜索而正在编制索引的表中，将一个行的所有列视为要编制索引的文档的一部分。<br /><br /> 0 = 没有完成的活动爬网或全文检索。<br /><br /> > 0 = 以下项之一（A 或 B）：A) 自从开始完整、增量或手动更改跟踪填充以来，由插入或更新操作处理的文档数。 B) 自从执行以下操作以来由插入或更新操作处理的行数：启用具有后台更新索引填充功能的更改跟踪、更改全文检索架构、重建全文目录或重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例等。<br /><br /> NULL = 表没有全文索引。<br /><br /> 此属性不监视已删除行，也不对已删除行进行计数。|  
|TableFulltextFailCount|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 全文搜索未编制索引的行数。<br /><br /> 0 = 填充已完成。<br /><br /> > 0 = 以下项之一（A 或 B）：A) 自从开始完整、增量和手动更新更改跟踪填充以来未编制索引的文档数。 B) 对于有后台更新索引的更改跟踪，则是自从填充开始以来未索引的行数，或重新启动填充。 这可能由架构更改、目录重建、服务器重新启动等引起。<br /><br /> NULL = 表没有全文索引。|  
|TableFulltextItemCount|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 成功编制了全文索引的行数。|  
|TableFulltextKeyColumn|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 与参与全文索引定义的单列唯一索引关联的列的 ID。<br /><br /> 0 = 表没有全文检索。|  
|TableFulltextPendingChanges|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 要处理的挂起更改跟踪项的数目。<br /><br /> 0 = 不启用更改跟踪。<br /><br /> NULL = 表没有全文索引。|  
|TableFulltextPopulateStatus|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 0 = 空闲。<br /><br /> 1 = 完整填充正在进行中。<br /><br /> 2 = 增量填充正在进行中。<br /><br /> 3 = 跟踪更改的传播正在进行中。<br /><br /> 4 = 正在进行后台更新索引（例如，自动跟踪更改）。<br /><br /> 5 = 全文索引已中止或暂停。|  
|TableHasActiveFulltextIndex|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表具有活动全文检索。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasCheckCnst|表|表具有 CHECK 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasClustIndex|表|表具有聚集索引。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasDefaultCnst|表|表具有 DEFAULT 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasDeleteTrigger|表|表具有 DELETE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasForeignKey|表|表具有 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasForeignRef|表|表由 FOREIGN KEY 约束引用。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasIdentity|表|表具有标识列。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasIndex|表|表具有任意类型的索引。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasInsertTrigger|表|对象具有 INSERT 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasNonclustIndex|表|表具有非聚集索引。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasPrimaryKey|表|表具有主键。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasRowGuidCol|表|表的 uniqueidentifier 列具有 ROWGUIDCOL。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasTextImage|表|表具有 text、ntext 或 image 列。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasTimestamp|表|表具有 timestamp 列。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasUniqueCnst|表|表具有 UNIQUE 约束。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasUpdateTrigger|表|对象具有 UPDATE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableHasVarDecimalStorageFormat|表|为 vardecimal 存储格式启用了表。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableInsertTrigger|表|表具有 INSERT 触发器。<br /><br /> >1 = 指定类型的第一个触发器的 ID。|  
|TableInsertTriggerCount|表|表具有指定数目的 INSERT 触发器。<br /><br /> >0 = INSERT 触发器的数目。|  
|TableIsFake|表|表不是真实的表。 它将由[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]根据需要在内部进行具体化。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableIsLockedOnBulkLoad|表|因 bcp 或 BULK INSERT 作业而导致表被锁定。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableIsMemoryOptimized|表|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 表是内存优化表<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int<br /><br /> 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。|  
|TableIsPinned|表|驻留表以将其保留在数据缓存中。<br /><br /> 0 = False<br /><br /> [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本不支持此功能。|  
|TableTextInRowLimit|表|text in row 允许的最大字节数。<br /><br /> 如果未设置 text in row 选项，则返回 0。|  
|TableUpdateTrigger|表|表具有 UPDATE 触发器。<br /><br /> > 1 = 指定类型的第一个触发器的 ID。|  
|TableUpdateTriggerCount|表|表具有指定数目的 UPDATE 触发器。<br /><br /> > 0 = UPDATE 触发器数目。|  
|TableHasColumnSet|表|表具有列集。<br /><br /> 0 = False<br /><br /> 1 = True<br /><br /> 有关详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md)。|  
|TableTemporalType|表|**适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 指定表的类型。<br /><br /> 0 = 非时态表<br /><br /> 1 = 系统版本控制表的历史记录表<br /><br /> 2 = 系统版本控制时态表|  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="exceptions"></a>例外  
 出现错误时或调用方没有查看对象的权限时，将返回 NULL。  
  
 用户只能查看符合如下条件的安全对象的元数据：该安全对象为该用户所有，或已授予该用户对该安全对象的权限。 这意味着，如果用户对对象没有任何权限，则元数据生成的内置函数（如 OBJECTPROPERTY）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 假定 object_id 位于当前数据库上下文中。 引用另一个数据库中的 object_id 的查询将返回 NULL 或返回不正确的结果。 例如，在下面的查询中，当前数据库上下文为 master 数据库。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]将尽量返回该数据库（而不是在查询中指定的数据库）中指定的 object_id 的属性值。 由于 `vEmployee` 视图不在 master 数据库中，该查询将返回不正确的结果。  
  
```sql  
USE master;  
GO  
SELECT OBJECTPROPERTY(OBJECT_ID(N'AdventureWorks2012.HumanResources.vEmployee'), 'IsView');  
GO  
```  
  
 OBJECTPROPERTY(view_id, 'IsIndexable') 可能会耗费大量的计算机资源，这是因为处理 IsIndexable 属性需要分析视图定义、规范化和局部优化。 尽管 IsIndexable 属性可以标识出能编制索引的表或视图，但在实际创建索引时，如果不能满足某些索引键要求，创建过程仍然可能会失败。 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
 如果至少添加了一个表列用于索引，则 OBJECTPROPERTY(table_id, 'TableHasActiveFulltextIndex') 将返回值 1 (true)。 只要添加了用于索引的第一列后，全文检索即可用于填充。  
  
 创建表后，QUOTED IDENTIFIER 选项在表的元数据中始终存储为 ON，即使在创建表时将该选项设置为 OFF 也不例外。 因此，OBJECTPROPERTY(table_id, 'IsQuotedIdentOn') 将始终返回值 1 (true)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-verifying-that-an-object-is-a-table"></a>A. 验证某个对象是否为表  
 以下示例将测试 `UnitMeasure` 是否为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的表。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') = 1  
   PRINT 'UnitMeasure is a table.'  
ELSE IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') = 0  
   PRINT 'UnitMeasure is not a table.'  
ELSE IF OBJECTPROPERTY (OBJECT_ID(N'Production.UnitMeasure'),'ISTABLE') IS NULL  
   PRINT 'ERROR: UnitMeasure is not a valid object.';  
GO  
```  
  
### <a name="b-verifying-that-a-scalar-valued-user-defined-function-is-deterministic"></a>B. 验证用户定义的标量值函数是否为确定性函数  
 以下示例将测试用户定义的标量值函数 `ufnGetProductDealerPrice`（该函数返回 money 值）是不是一个确定性函数。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT OBJECTPROPERTY(OBJECT_ID('dbo.ufnGetProductDealerPrice'), 'IsDeterministic');  
GO  
```  
  
 结果集显示 `ufnGetProductDealerPrice` 是一个确定性函数。  
  
 ```
-----  
0
```  
  
### <a name="c-finding-the-tables-that-belong-to-a-specific-schema"></a>C. 查找属于特定架构的表  
 下面的示例返回 dbo 架构中的所有表。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT name, object_id, type_desc  
FROM sys.objects   
WHERE OBJECTPROPERTY(object_id, N'SchemaId') = SCHEMA_ID(N'dbo')  
ORDER BY type_desc, name;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-verifying-that-an-object-is-a-table"></a>D. 验证某个对象是否为表  
 以下示例将测试 `dbo.DimReseller` 是否为 [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)] 数据库中的表。  
  
```sql  
-- Uses AdventureWorks  
  
IF OBJECTPROPERTY (OBJECT_ID(N'dbo.DimReseller'),'ISTABLE') = 1  
   SELECT 'DimReseller is a table.'  
ELSE   
   SELECT 'DimReseller is not a table.';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECTPROPERTYEX (Transact-SQL)](../../t-sql/functions/objectpropertyex-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
  

