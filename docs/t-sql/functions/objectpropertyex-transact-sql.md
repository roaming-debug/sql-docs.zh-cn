---
description: OBJECTPROPERTYEX (Transact-SQL)
title: OBJECTPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECTPROPERTYEX
- OBJECTPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying schema-scoped object information
- viewing schema-scoped object information
- OBJECTPROPERTYEX function
- schema-scoped objects [SQL Server]
- objects [SQL Server], schema-scoped
ms.assetid: be36b3e3-3309-4332-bfb5-c7e9cf8dc8bd
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e0802ac92d900b3ef4ddacaf714e377ce95a85b8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754447"
---
# <a name="objectpropertyex-transact-sql"></a>OBJECTPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回当前数据库中架构范围内的对象的相关信息。 有关这些对象的列表，请参阅 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。 OBJECTPROPERTYEX 不能用于非架构范围内的对象，如数据定义语言 (DDL) 触发器和事件通知。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
OBJECTPROPERTYEX ( id , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *id*  
 是表示当前数据库中对象 ID 的表达式。 id 的数据类型为 int，并假定为当前数据库上下文中的架构范围内的对象。  
  
 *property*  
 一个表达式，包含要为 ID 所指定的对象返回的信息。返回类型为 sql_variant。 下表显示了各属性值的基本数据类型。  
  
> [!NOTE]  
>  除非另外注明，否则出现以下情况时将返回 NULL：property 不是有效的属性名称；id 不是有效的对象 ID；id 不是指定属性支持的对象类型；调用方无权查看对象的元数据。  
  
|属性名称|对象类型|说明和返回的值|  
|-------------------|-----------------|-------------------------------------|  
|BaseType|任何架构范围内的对象|指定对象的基类型。 当指定的对象为 SYNONYM 时，将返回基础对象的基类型。<br /><br /> Nonnull = 对象类型<br /><br /> 基本数据类型：char(2)|  
|CnstIsClustKey|约束|具有聚集索引的 PRIMARY KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|CnstIsColumn|约束|单个列上的 CHECK、DEFAULT 或 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|CnstIsDeleteCascade|约束|具有 ON DELETE CASCADE 选项的 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|CnstIsDisabled|约束|禁用的约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|CnstIsNonclustKey|约束|具有非聚集索引的 PRIMARY KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|CnstIsNotRepl|约束|约束是使用 NOT FOR REPLICATION 关键字定义的。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|CnstIsNotTrusted|约束|启用约束时未检查现有行。 因此，可能并非所有行都受约束的限制。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|CnstIsUpdateCascade|约束|具有 ON UPDATE CASCADE 选项的 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsAfterTrigger|触发器|AFTER 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsAnsiNullsOn|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|创建时的 ANSI_NULLS 设置。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsDeleteTrigger|触发器|DELETE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsFirstDeleteTrigger|触发器|对表执行 DELETE 时触发的第一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsFirstInsertTrigger|触发器|对表执行 INSERT 时触发的第一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsFirstUpdateTrigger|触发器|对表执行 UPDATE 时触发的第一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsInsertTrigger|触发器|INSERT 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsInsteadOfTrigger|触发器|INSTEAD OF 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsLastDeleteTrigger|触发器|对表执行 DELETE 时触发的最后一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsLastInsertTrigger|触发器|对表执行 INSERT 时触发的最后一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsLastUpdateTrigger|触发器|对表执行 UPDATE 时触发的最后一个触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsQuotedIdentOn|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|创建时的 QUOTED_IDENTIFIER 设置。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsStartup|过程|启动过程。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsTriggerDisabled|触发器|禁用的触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsTriggerNotForRepl|触发器|定义为 NOT FOR REPLICATION 的触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsUpdateTrigger|触发器|UPDATE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|ExecIsWithNativeCompilation|[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 本机编译过程。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|HasAfterTrigger|表、视图|表或视图具有 AFTER 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|HasDeleteTrigger|表、视图|表或视图具有 DELETE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|HasInsertTrigger|表、视图|表或视图具有 INSERT 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|HasInsteadOfTrigger|表、视图|表或视图具有 INSTEAD OF 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|HasUpdateTrigger|表、视图|表或视图具有 UPDATE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsAnsiNullsOn|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、表、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|指定表的 ANSI NULLS 选项设置为 ON，表示与 Null 值的所有比较结果都为 UNKNOWN。 只要表存在，此设置就会应用于表定义中的所有表达式，包括计算列和约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsCheckCnst|任何架构范围内的对象|CHECK 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsConstraint|任何架构范围内的对象|约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsDefault|任何架构范围内的对象|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 绑定的默认值。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsDefaultCnst|任何架构范围内的对象|DEFAULT 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsDeterministic|标量值函数和表值函数、视图|函数或视图的确定性属性。<br /><br /> 1 = 确定<br /><br /> 0 = 不确定<br /><br /> 基本数据类型：int|  
|IsEncrypted|[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、表、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|指示模块语句的原始文本已转换为模糊格式。 模糊代码的输出在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 的任何目录视图中都不能直接显示。 对系统表或数据库文件没有访问权限的用户不能检索模糊文本。 但是，能通过 [DAC 端口](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)访问系统表的用户或能够直接访问数据库文件的用户可以使用此文本。 此外，能够向服务器进程附加调试器的用户可在运行时从内存中检索原始过程。<br /><br /> 1 = 已加密<br /><br /> 0 = 未加密<br /><br /> 基本数据类型：int|  
|IsExecuted|任何架构范围内的对象|指定可以执行对象（视图、过程、函数或触发器）。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsExtendedProc|任何架构范围内的对象|扩展过程。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsForeignKey|任何架构范围内的对象|FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsIndexed|表、视图|具有索引的表或视图。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsIndexable|表、视图|可以创建索引的表或视图。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsInlineFunction|函数|内联函数。<br /><br /> 1 = 内联函数<br /><br /> 0 = 非内联函数<br /><br /> 基本数据类型：int|  
|IsMSShipped|任何架构范围内的对象|在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 期间创建的对象。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsPrecise|计算列、函数、用户定义类型、视图|指示对象是否包含不精确计算，如浮点运算。<br /><br /> 1 = 精确<br /><br /> 0 = 不精确<br /><br /> 基本数据类型：int|  
|IsPrimaryKey|任何架构范围内的对象|PRIMARY KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsProcedure|任何架构范围内的对象|过程。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsQuotedIdentOn|CHECK 约束、DEFAULT 定义、[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数、[!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、表、[!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器、视图|指定对象的带引号标识符设置为 ON，表示在对象定义所涉及的所有表达式中使用双引号分隔标识符。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsQueue|任何架构范围内的对象|Service Broker 队列<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsReplProc|任何架构范围内的对象|复制过程。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsRule|任何架构范围内的对象|绑定规则。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsScalarFunction|函数|标量值函数。<br /><br /> 1 = 标量值函数<br /><br /> 0 = 非标量值函数<br /><br /> 基本数据类型：int|  
|IsSchemaBound|函数、过程、视图|使用 SCHEMABINDING 创建的绑定到架构的函数或视图。<br /><br /> 1 = 绑定到架构<br /><br /> 0 = 不绑定到架构<br /><br /> 基本数据类型：int|  
|IsSystemTable|表|系统表。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsSystemVerified|计算列、函数、用户定义类型、视图|对象的精度和确定性属性可以由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进行验证。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsTable|表|表。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsTableFunction|函数|表值函数。<br /><br /> 1 = 表值函数<br /><br /> 0 = 非表值函数<br /><br /> 基本数据类型：int|  
|IsTrigger|任何架构范围内的对象|触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsUniqueCnst|任何架构范围内的对象|UNIQUE 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsUserTable|表|用户定义的表。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|IsView|视图|视图。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|OwnerId|任何架构范围内的对象|对象的所有者。<br /><br /> **注意：** 架构所有者不一定是对象所有者。 例如，子对象（其 parent_object_id 为非 null）将始终返回与父对象相同的所有者 ID。<br /><br /> Nonnull = 对象所有者的数据库用户 ID。<br /><br /> NULL = 不支持的对象类型，或对象 ID 无效。<br /><br /> 基本数据类型：int|  
|SchemaId|任何架构范围内的对象|与对象关联的架构的 ID。<br /><br /> Nonnull = 对象的架构 ID。<br /><br /> 基本数据类型：int|  
|SystemDataAccess|函数、视图|对象访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本地实例中的系统数据、系统目录或虚拟系统表。<br /><br /> 0 = 无<br /><br /> 1 = 读取<br /><br /> 基本数据类型：int|  
|TableDeleteTrigger|表|表具有 DELETE 触发器。<br /><br /> >1 = 指定类型的第一个触发器的 ID。<br /><br /> 基本数据类型：int|  
|TableDeleteTriggerCount|表|表具有指定数目的 DELETE 触发器。<br /><br /> Nonnull = DELETE 触发器数<br /><br /> 基本数据类型：int|  
|TableFullTextMergeStatus|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表所具有的全文检索当前是否正在合并。<br /><br /> 0 = 表没有全文检索，或者全文检索未在合并。<br /><br /> 1 = 全文检索正在合并。|  
|TableFullTextBackgroundUpdateIndexOn|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表已启用全文后台更新索引（自动更改跟踪）。<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE<br /><br /> 基本数据类型：int|  
|TableFulltextCatalogId|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表的全文检索数据所在的全文目录的 ID。<br /><br /> 非零 = 全文目录 ID，它与全文检索表中标识行的唯一索引相关。<br /><br /> 0 = 表没有全文检索。<br /><br /> 基本数据类型：int|  
|TableFullTextChangeTrackingOn|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表已启用全文更改跟踪。<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE<br /><br /> 基本数据类型：int|  
|TableFulltextDocsProcessed|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 自开始全文检索以来所处理的行数。 在为进行全文搜索而正在编制索引的表中，将一个行的所有列视为要编制索引的文档的一部分。<br /><br /> 0 = 没有完成的活动爬网或全文检索。<br /><br /> > 0 = 以下结果之一（A 或 B）：A) 自从执行以下操作以来由插入或更新操作处理的文档数：开始完整、增量或手动的更改跟踪填充功能；B) 自从执行以下操作以来由插入或更新操作处理的行数：启用具有后台更新索引填充功能的更改跟踪、更改全文检索架构、重建全文目录或重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例等。<br /><br /> NULL = 表没有全文索引。<br /><br /> 基本数据类型：int<br /><br /> **注意** 此属性不监视删除的行，也不对删除的行进行计数。|  
|TableFulltextFailCount|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 全文搜索未索引的行数。<br /><br /> 0 = 填充已完成。<br /><br /> >0 = 以下结果之一（A 或 B）：A) 自从开始完整、增量和手动更新更改跟踪填充以来未编制索引的文档数；B) 对于有后台更新索引的更改跟踪，则为自从开始填充或重启填充以来未编制索引的行数。 这可以由架构更改、目录重建、服务器重新启动等等导致。<br /><br /> NULL = 表没有全文检索。<br /><br /> 基本数据类型：int|  
|TableFulltextItemCount|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> Nonnull = 成功进行全文检索的行数。<br /><br /> NULL = 表没有全文索引。<br /><br /> 基本数据类型：int|  
|TableFulltextKeyColumn|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 与作为全文检索和语义索引定义的一部分的单列唯一索引关联的列的 ID。<br /><br /> 0 = 表没有全文检索。<br /><br /> 基本数据类型：int|  
|TableFulltextPendingChanges|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 要处理的挂起更改跟踪项的数目。<br /><br /> 0 = 不启用更改跟踪。<br /><br /> NULL = 表没有全文索引。<br /><br /> 基本数据类型：int|  
|TableFulltextPopulateStatus|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 0 = 空闲。<br /><br /> 1 = 完整填充正在进行中。<br /><br /> 2 = 增量填充正在进行中。<br /><br /> 3 = 跟踪更改的传播正在进行中。<br /><br /> 4 = 正在进行后台更新索引（例如，自动跟踪更改）。<br /><br /> 5 = 全文索引已中止或暂停。<br /><br /> 6 = 发生了错误。 检查爬网日志了解详细信息。 有关详细信息，请参阅[填充全文检索](../../relational-databases/search/populate-full-text-indexes.md)的“排除全文填充（爬网）中的错误”一节。<br /><br /> 基本数据类型：int|  
|TableFullTextSemanticExtraction|表|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 支持对表进行语义索引。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasActiveFulltextIndex|表|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> 表具有活动全文检索。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasCheckCnst|表|表具有 CHECK 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasClustIndex|表|表具有聚集索引。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasDefaultCnst|表|表具有 DEFAULT 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasDeleteTrigger|表|表具有 DELETE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasForeignKey|表|表具有 FOREIGN KEY 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasForeignRef|表|表由 FOREIGN KEY 约束引用。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasIdentity|表|表具有标识列。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasIndex|表|表具有任意类型的索引。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasInsertTrigger|表|对象具有 INSERT 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasNonclustIndex|表|表具有非聚集索引。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasPrimaryKey|表|表具有主键。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasRowGuidCol|表|表的 uniqueidentifier 列具有 ROWGUIDCOL。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasTextImage|表|表具有 text、ntext 或 image 列。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasTimestamp|表|表具有 timestamp 列。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasUniqueCnst|表|表具有 UNIQUE 约束。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasUpdateTrigger|表|对象具有 UPDATE 触发器。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableHasVarDecimalStorageFormat|表|为 vardecimal 存储格式启用了表。<br /><br /> 1 = True<br /><br /> 0 = False|  
|TableInsertTrigger|表|表具有 INSERT 触发器。<br /><br /> >1 = 指定类型的第一个触发器的 ID。<br /><br /> 基本数据类型：int|  
|TableInsertTriggerCount|表|表具有指定数目的 INSERT 触发器。<br /><br /> >0 = INSERT 触发器的数目。<br /><br /> 基本数据类型：int|  
|TableIsFake|表|表不是真实的表。 它将由[!INCLUDE[ssDE](../../includes/ssde-md.md)]根据需要在内部进行具体化。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableIsLockedOnBulkLoad|表|由于 bcp 或 BULK INSERT 任务，表被锁定。<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int|  
|TableIsMemoryOptimized|表|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 表是内存优化表<br /><br /> 1 = True<br /><br /> 0 = False<br /><br /> 基本数据类型：int<br /><br /> 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。|  
|TableIsPinned|表|驻留表以将其保留在数据缓存中。<br /><br /> 0 = False<br /><br /> [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本不支持此功能。|  
|TableTextInRowLimit|表|表设置了 text in row 选项。<br /><br /> > 0 = text in row 所允许的最大字节数。<br /><br /> 0 = 未设置 text in row option 选项。<br /><br /> 基本数据类型：int|  
|TableUpdateTrigger|表|表具有 UPDATE 触发器。<br /><br /> > 1 = 指定类型的第一个触发器的 ID。<br /><br /> 基本数据类型：int|  
|TableUpdateTriggerCount|表|表具有指定数目的 UPDATE 触发器。<br /><br /> > 0 = UPDATE 触发器数目。<br /><br /> 基本数据类型：int|  
|UserDataAccess|函数、视图|指示对象访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本地实例中的用户数据和用户表。<br /><br /> 1 = 读取<br /><br /> 0 = 无<br /><br /> 基本数据类型：int|  
|TableHasColumnSet|表|表具有列集。<br /><br /> 0 = False<br /><br /> 1 = True<br /><br /> 有关详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md)。|  
|基数|表（系统或用户定义）、视图或索引|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指定对象中的行数。|  
|TableTemporalType|表|**适用于**：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 指定表的类型。<br /><br /> 0 = 非时态表<br /><br /> 1 = 系统版本控制表的历史记录表<br /><br /> 2 = 系统版本控制时态表|  
  
## <a name="return-types"></a>返回类型  
 **sql_variant**  
  
## <a name="exceptions"></a>例外  
 出现错误时或调用方没有查看对象的权限时，将返回 NULL。  
  
 用户只能查看符合如下条件的安全对象的元数据：该安全对象为该用户所有，或已授予该用户对该安全对象的权限。 也就是说，如果用户对该对象没有任何权限，则某些会产生元数据的内置函数（如 OBJECTPROPERTYEX）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 假定 object_id 位于当前数据库上下文中。 引用另一个数据库中的 object_id 的查询将返回 NULL 或返回不正确的结果。 例如，在下面的查询中，当前数据库上下文为 master 数据库。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将尝试从该数据库（而不是在查询中指定的数据库）中返回指定的 object_id 的属性值。 由于 `vEmployee` 视图不在 master 数据库中，该查询将返回不正确的结果。  
  
```sql  
USE master;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID(N'AdventureWorks2012.HumanResources.vEmployee'), 'IsView');  
GO  
```  
  
 OBJECTPROPERTYEX(view_id, 'IsIndexable') 可能会耗费大量的计算机资源，这是因为处理 IsIndexable 属性需要分析视图定义、规范化和局部优化。 尽管 IsIndexable 属性可以标识出能编制索引的表或视图，但在实际创建索引时，如果不能满足某些索引键要求，创建过程仍然可能会失败。 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
 如果至少添加了一个表列用于索引，则 OBJECTPROPERTYEX (table_id, 'TableHasActiveFulltextIndex') 将返回值 1 (true)。 只要添加了用于索引的第一列后，全文检索即可用于填充。  
  
 对元数据可视性的限制将应用于结果集。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-finding-the-base-type-of-an-object"></a>A. 查找对象的基类型  
 以下示例为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的 `MyEmployeeTable` 表创建一个 SYNONYM `Employee`，然后返回 SYNONYM 的基类型。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SYNONYM MyEmployeeTable FOR HumanResources.Employee;  
GO  
SELECT OBJECTPROPERTYEX ( object_id(N'MyEmployeeTable'), N'BaseType')AS [Base Type];  
GO  
```  
  
 结果集显示基础对象（`Employee` 表）的基类型是用户表。  
  
 ```
Base Type 
--------  
U
```  
  
### <a name="b-returning-a-property-value"></a>B. 返回属性值  
 以下示例返回指定表中的 UPDATE 触发器数。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID(N'HumanResources.Employee'), N'TABLEUPDATETRIGGERCOUNT');  
GO  
```  
  
### <a name="c-finding-tables-that-have-a-foreign-key-constraint"></a>C. 查找具有 FOREIGN KEY 约束的表  
 以下示例使用 `TableHasForeignKey` 属性返回具有 FOREIGN KEY 约束的所有表。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name, object_id, schema_id, type_desc  
FROM sys.objects   
WHERE OBJECTPROPERTYEX(object_id, N'TableHasForeignKey') = 1  
ORDER BY name;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-finding-the-base-type-of-an-object"></a>D：查找对象的基类型  
 下例返回 `dbo.DimReseller` 对象的基类型。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OBJECTPROPERTYEX ( object_id(N'dbo.DimReseller'), N'BaseType')AS BaseType;  
```  
  
 结果集显示基础对象（`dbo.DimReseller` 表）的基类型是用户表。  
  
```  
BaseType   
--------   
U   
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SYNONYM (Transact-SQL)](../../t-sql/statements/create-synonym-transact-sql.md)   
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_ID (Transact-SQL)](../../t-sql/functions/object-id-transact-sql.md)   
 [OBJECT_NAME (Transact-SQL)](../../t-sql/functions/object-name-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md)  
  
  

