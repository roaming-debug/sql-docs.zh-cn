---
title: sp_addarticle (Transact-sql) |Microsoft Docs
description: 创建项目并将其添加到发布中。 此存储过程在发布服务器的发布数据库上运行。
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addarticle
- sp_addarticle_TSQL
helpviewer_keywords:
- sp_addarticle
ms.assetid: 0483a157-e403-4fdb-b943-23c1b487bef0
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 5ee845eac866c3954ba03080c037179fa76ad519
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182834"
---
# <a name="sp_addarticle-transact-sql"></a>sp_addarticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建项目并将其添加到发布中。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addarticle [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
    [ , [ @source_table = ] 'source_table' ]  
    [ , [ @destination_table = ] 'destination_table' ]   
    [ , [ @vertical_partition = ] 'vertical_partition' ]   
    [ , [ @type = ] 'type' ]   
    [ , [ @filter = ] 'filter' ]   
    [ , [ @sync_object= ] 'sync_object' ]   
        [ , [ @ins_cmd = ] 'ins_cmd' ]   
    [ , [ @del_cmd = ] 'del_cmd' ]   
        [ , [ @upd_cmd = ] 'upd_cmd' ]   
    [ , [ @creation_script = ] 'creation_script' ]   
    [ , [ @description = ] 'description' ]   
    [ , [ @pre_creation_cmd = ] 'pre_creation_cmd' ]   
    [ , [ @filter_clause = ] 'filter_clause' ]   
    [ , [ @schema_option = ] schema_option ]   
    [ , [ @destination_owner = ] 'destination_owner' ]   
    [ , [ @status = ] status ]   
    [ , [ @source_owner = ] 'source_owner' ]   
    [ , [ @sync_object_owner = ] 'sync_object_owner' ]   
    [ , [ @filter_owner = ] 'filter_owner' ]   
    [ , [ @source_object = ] 'source_object' ]   
    [ , [ @artid = ] article_ID  OUTPUT ]   
    [ , [ @auto_identity_range = ] 'auto_identity_range' ]   
    [ , [ @pub_identity_range = ] pub_identity_range ]   
    [ , [ @identity_range = ] identity_range ]   
    [ , [ @threshold = ] threshold ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @use_default_datatypes = ] use_default_datatypes  
    [ , [ @identityrangemanagementoption = ] identityrangemanagementoption ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @fire_triggers_on_snapshot = ] 'fire_triggers_on_snapshot' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含项目的发布的名称。 名称在数据库中必须是唯一的。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 项目的名称。 该名称在发布中必须唯一。 *项目* 是 **sysname**，无默认值。  
  
`[ @source_table = ] 'source_table'` 此参数已弃用;改用 *source_object* 。  
  
 *Oracle 发布服务器不支持此参数。*  
  
`[ @destination_table = ] 'destination_table'` 与 *source_table* 或存储过程不同的目标 (订阅) 表的名称。 *destination_table* 的类型为 **sysname**，默认值为 NULL，这意味着 *source_table* 等于 *destination_table * *。*  
  
`[ @vertical_partition = ] 'vertical_partition'` 启用和禁用对表项目的列筛选。 *vertical_partition* 为 **nchar (5)**，默认值为 FALSE。  
  
 **false** 指示没有垂直筛选并发布所有列。  
  
 **如果为 true，则** 清除除声明的主键之外的所有列、无默认值的可为 null 的列和唯一键列。 使用 [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)添加列。  
  
`[ @type = ] 'type'` 项目的类型。 *类型* 为 **sysname**，可以为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**aggregate schema only**|仅具有架构的聚合函数。|  
|**func schema only**|仅具有架构的函数。|  
|**indexed view logbased**|基于日志的索引视图项目。 Oracle 发布服务器不支持。 对于此类型的项目，不需要单独发布基表。|  
|**indexed view logbased manualboth**|具有手动筛选器和手动视图并且基于日志的索引视图项目。 此选项要求您同时指定 *sync_object* 参数和 *筛选器* 参数。 对于此类型的项目，不需要单独发布基表。 Oracle 发布服务器不支持。|  
|**indexed view logbased manualfilter**|具有手动筛选器并且基于日志的索引视图项目。 此选项要求您同时指定 *sync_object* 参数和 *筛选器* 参数。 对于此类型的项目，不需要单独发布基表。 Oracle 发布服务器不支持。|  
|**indexed view logbased manualview**|具有手动视图并且基于日志的索引视图项目。 此选项要求指定 *sync_object* 参数。 对于此类型的项目，不需要单独发布基表。 Oracle 发布服务器不支持。|  
|**indexed view schema only**|仅具有架构的索引视图。 对于此类型的项目，还必须发布基表。|  
|**logbased** (默认值) |基于日志的项目。|  
|**logbased manualboth**|具有手动筛选器和手动视图并且基于日志的项目。 此选项要求您同时指定 *sync_object* 参数和 *筛选器* 参数。 Oracle 发布服务器不支持。|  
|**logbased manualfilter**|具有手动筛选器并且基于日志的项目。 此选项要求您同时指定 *sync_object* 参数和 *筛选器* 参数。 Oracle 发布服务器不支持。|  
|**logbased manualview**|具有手动视图并且基于日志的项目。 此选项要求指定 *sync_object* 参数。 Oracle 发布服务器不支持。|  
|**proc exec**|将存储过程的执行复制到项目的所有订阅服务器。 Oracle 发布服务器不支持。 建议使用选项 " **serializable proc exec** "，而不是 **进程 exec**。 有关详细信息，请参阅在 [事务复制中发布存储过程执行](../../relational-databases/replication/transactional/publishing-stored-procedure-execution-in-transactional-replication.md)中的 "存储过程执行项目的类型" 一节。 在启用了变更数据捕获的情况下无法使用。|  
|**proc schema only**|仅具有架构的过程。 Oracle 发布服务器不支持。|  
|**serializable proc exec**|仅当存储过程在可串行事务上下文内执行时才复制存储过程的执行。 Oracle 发布服务器不支持。<br /><br /> 此外，还必须在显式事务内执行该过程，以便复制该过程的执行。|  
|**view schema only**|仅具有架构的视图。 Oracle 发布服务器不支持。 使用此选项时，还必须发布基表。|  
  
`[ @filter = ] 'filter'` (创建的存储过程，用于复制) 用于水平筛选表。 *筛选器* 为 **nvarchar (386)**，默认值为 NULL。 必须手动执行[sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)和[sp_articlefilter](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) ，才能创建视图和筛选存储过程。 如果不为 NULL，则不创建筛选过程（假定存储过程是手动创建的）。  
  
`[ @sync_object = ] 'sync_object'` 表或视图的名称，该表或视图用于生成用于表示本文快照的数据文件。 *sync_object* 为 **nvarchar (386)**，默认值为 NULL。 如果为 NULL，则调用 [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) 来自动创建用于生成输出文件的视图。 将任何列添加到 [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)后，会发生这种情况。 如果不为 NULL，则不创建视图（假定视图是手动创建的）。  
  
`[ @ins_cmd = ] 'ins_cmd'` 复制此项目的插入时所使用的复制命令类型。 *ins_cmd* 为 **nvarchar (255)**，可以是以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**NONE**|不执行任何操作。|  
|**CALL sp_MSins_**<br /> **_表_**  (默认值) <br /><br /> - 或 -<br /><br /> **CALL custom_stored_procedure_name**|在订阅服务器中调用要执行的存储过程。 若要使用此复制方法，请使用 *schema_option* 指定自动创建存储过程，或在项目的每个订阅服务器的目标数据库中创建指定的存储过程。 *custom_stored_procedure* 是用户创建的存储过程的名称。 <strong>sp_MSins_ *表*</strong> 包含用于替换参数 *_table* 部分的目标表的名称。 指定 *destination_owner* 时，它会预置目标表名称。 例如，对于订阅服务器上的 **生产** 架构拥有的 **ProductCategory** 表，该参数将为 `CALL sp_MSins_ProductionProductCategory` 。 对于对等复制拓扑中的项目， *_table* 追加 GUID 值。 不支持将 *custom_stored_procedure* 指定为更新订阅服务器。|  
|**SQL** 或 NULL|复制 INSERT 语句。 需要为 INSERT 语句提供项目中发布的所有列的值。 对插入复制以下命令：<br /><br /> `INSERT INTO <table name> VALUES (c1value, c2value, c3value, ..., cnvalue)`|  
  
 有关详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
`[ @del_cmd = ] 'del_cmd'` 复制该项目的删除时所使用的复制命令类型。 *del_cmd* 为 **nvarchar (255)**，可以是以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**NONE**|不执行任何操作。|  
|**CALLsp_MSdel_**<br /> **_表_**  (默认值) <br /><br /> - 或 -<br /><br /> **CALL custom_stored_procedure_name**|在订阅服务器中调用要执行的存储过程。 若要使用此复制方法，请使用 *schema_option* 指定自动创建存储过程，或在项目的每个订阅服务器的目标数据库中创建指定的存储过程。 *custom_stored_procedure* 是用户创建的存储过程的名称。 <strong>sp_MSdel_ *表*</strong> 包含用于替换参数 *_table* 部分的目标表的名称。 指定 *destination_owner* 时，它会预置目标表名称。 例如，对于订阅服务器上的 **生产** 架构拥有的 **ProductCategory** 表，该参数将为 `CALL sp_MSdel_ProductionProductCategory` 。 对于对等复制拓扑中的项目， *_table* 追加 GUID 值。 不支持将 *custom_stored_procedure* 指定为更新订阅服务器。|  
|**XCALL sp_MSdel_**<br /> **_数据表_**<br /><br /> - 或 -<br /><br /> **XCALL custom_stored_procedure_name**|采用 XCALL 样式参数调用存储过程。 若要使用此复制方法，请使用 *schema_option* 指定自动创建存储过程，或在项目的每个订阅服务器的目标数据库中创建指定的存储过程。 对于更新订阅服务器，不允许指定用户创建的存储过程。|  
|**SQL** 或 NULL|复制 DELETE 语句。 为 DELETE 语句提供所有主键列值。 对删除复制以下命令：<br /><br /> `DELETE FROM <table name> WHERE pkc1 = pkc1value AND pkc2 = pkc2value AND pkcn = pkcnvalue`|  
  
 有关详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
`[ @upd_cmd = ] 'upd_cmd'` 复制此项目的更新时使用的复制命令类型。 *upd_cmd* 为 **nvarchar (255)**，可以是以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**NONE**|不执行任何操作。|  
|**CALL sp_MSupd_**<br /> **_数据表_**<br /><br /> - 或 -<br /><br /> **CALL custom_stored_procedure_name**|在订阅服务器中调用要执行的存储过程。 若要使用此复制方法，请使用 *schema_option* 指定自动创建存储过程，或在项目的每个订阅服务器的目标数据库中创建指定的存储过程。|  
|**MCALL sp_MSupd_**<br /> **_数据表_**<br /><br /> - 或 -<br /><br /> **MCALL custom_stored_procedure_name**|采用 MCALL 样式参数调用存储过程。 若要使用此复制方法，请使用 *schema_option* 指定自动创建存储过程，或在项目的每个订阅服务器的目标数据库中创建指定的存储过程。 *custom_stored_procedure* 是用户创建的存储过程的名称。 <strong>sp_MSupd_ *表*</strong> 包含用于替换参数 *_table* 部分的目标表的名称。 指定 *destination_owner* 时，它会预置目标表名称。 例如，对于订阅服务器上的 **生产** 架构拥有的 **ProductCategory** 表，该参数将为 `MCALL sp_MSupd_ProductionProductCategory` 。 对于对等复制拓扑中的项目， *_table* 追加 GUID 值。 对于更新订阅服务器，不允许指定用户创建的存储过程。|  
|**SCALL sp_MSupd_**<br /> **_表_**  (默认值) <br /><br /> - 或 -<br /><br /> **SCALL custom_stored_procedure_name**|采用 SCALL 样式参数调用存储过程。 若要使用此复制方法，请使用 *schema_option* 指定自动创建存储过程，或在项目的每个订阅服务器的目标数据库中创建指定的存储过程。 *custom_stored_procedure* 是用户创建的存储过程的名称。 <strong>sp_MSupd_ *表*</strong> 包含用于替换参数 *_table* 部分的目标表的名称。 指定 *destination_owner* 时，它会预置目标表名称。 例如，对于订阅服务器上的 **生产** 架构拥有的 **ProductCategory** 表，该参数将为 `SCALL sp_MSupd_ProductionProductCategory` 。 对于对等复制拓扑中的项目， *_table* 追加 GUID 值。 对于更新订阅服务器，不允许指定用户创建的存储过程。|  
|**XCALL sp_MSupd_**<br /> **_数据表_**<br /><br /> - 或 -<br /><br /> **XCALL custom_stored_procedure_name**|采用 XCALL 样式参数调用存储过程。 若要使用此复制方法，请使用 *schema_option* 指定自动创建存储过程，或在项目的每个订阅服务器的目标数据库中创建指定的存储过程。 对于更新订阅服务器，不允许指定用户创建的存储过程。|  
|**SQL** 或 NULL|复制 UPDATE 语句。 UPDATE 语句在所有的列值和主键列值中提供。 对更新复制以下命令：<br /><br /> `UPDATE <table name> SET c1 = c1value, SET c2 = c2value, SET cn = cnvalue WHERE pkc1 = pkc1value AND pkc2 = pkc2value AND pkcn = pkcnvalue`|  
  
> [!NOTE]  
>  对于传播到订阅服务器的数据量，CALL、MCALL、SCALL 和 XCALL 语法各不相同。 CALL 语法传递所有已插入和已删除的列的所有值。 SCALL 语法只传递受影响的列的值。 XCALL 语法传递所有列的值（无论更改与否），包括列以前的值。 有关详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
`[ @creation_script = ] 'creation_script'` 用于创建订阅数据库中项目的可选项目架构脚本的路径和名称。 *creation_script* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @description = ] 'description'` 是项目的描述性条目。 *描述* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @pre_creation_cmd = ] 'pre_creation_cmd'` 指定当应用此项目的快照时，如果在订阅服务器上检测到同名的现有对象，系统应执行的操作。 *pre_creation_cmd* 为 **nvarchar (10)**，可以是以下值之一。  
  
|值|描述|  
|-----------|-----------------|  
|**无**|不使用命令。|  
|**delete**|在应用快照之前从目标表中删除数据。 水平筛选项目时，将只删除筛选子句所指定的列中的数据。 定义水平筛选时，不支持用于 Oracle 发布服务器。|  
|**删除** (默认值) |删除目标表。|  
|**truncate**|截断目标表。 对 ODBC 或 OLE DB 订阅服务器无效。|  
  
`[ @filter_clause = ] 'filter_clause'` 用于定义水平筛选器的) 子句 (的限制。 当输入限制子句时，将省略关键字 WHERE。 *filter_clause* 为 **ntext**，默认值为 NULL。 有关详细信息，请参阅[筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)。  
  
`[ @schema_option = ] schema_option` 给定项目的架构生成选项的位掩码。 *schema_option* 是 **二进制 (8)**，可以是以下一个或多个值的 [| (按位或)](../../t-sql/language-elements/bitwise-or-transact-sql.md) 产品：  
  
> [!NOTE]  
>  如果该值为 NULL，则系统将为依赖于其他项目属性的项目自动生成有效的架构选项。 "备注" 中给出的 " **默认架构选项** " 表显示根据项目类型和复制类型的组合所选择的值。  
  
|值|说明|  
|-----------|-----------------|  
|**0x00**|禁用快照代理的脚本并使用 *creation_script*。|  
|**0x01**|生成对象创建脚本（CREATE TABLE、CREATE PROCEDURE 等）。 该值是存储过程项目的默认值。|  
|**0x02**|如果已定义，则生成传播项目更改的存储过程。|  
|**0x04**|使用 IDENTITY 属性为标识列编写脚本。|  
|**0x08**|复制 **时间戳** 列。 如果未设置，则 **时间戳** 列将作为 **二进制** 复制。|  
|**0x10**|生成对应的聚集索引。 即使不设置此选项，但如果已在已发布的表中定义与主键相关的索引以及唯一约束，那么也会生成它们。|  
|**0x20**|在订阅服务器中将用户定义数据类型 (UDT) 转换为基本数据类型。 如果 UDT 列是主键的一部分或者计算列引用 UDT 列，则当 UDT 列上具有 CHECK 或 DEFAULT 约束时，此选项无法使用。 *对于 Oracle 发布服务器不支持*。|  
|**0x40**|生成相应的非聚集索引。 即使不设置此选项，但如果已在已发布的表中定义与主键相关的索引以及唯一约束，那么也会生成它们。|  
|**0x80**|复制主键约束。 即使未启用选项 **0x10** 和 **0x40** ，也会复制与约束相关的任何索引。|  
|**0x100**|如果已定义，则复制表项目的用户触发器。 *对于 Oracle 发布服务器不支持*。|  
|**0x200**|复制外键约束。 如果被所引用的表不是发布的一部分，则不会复制已发布表的任何外键约束。 *对于 Oracle 发布服务器不支持*。|  
|**0x400**|复制检查约束。 *对于 Oracle 发布服务器不支持*。|  
|**0x800**|复制默认值。 *对于 Oracle 发布服务器不支持*。|  
|**0x1000**|复制列级排序规则。<br /><br /> **注意：** 应为 Oracle 发布服务器设置该选项，以启用区分大小写的比较。|  
|**0x2000**|复制与已发布项目源对象关联的扩展属性。 *对于 Oracle 发布服务器不支持*。|  
|**0x4000**|复制 UNIQUE 约束。 即使未启用选项 **0x10** 和 **0x40** ，也会复制与约束相关的任何索引。|  
|**0x8000**|此选项对 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 发布服务器无效。|  
|**0x10000**|以 NOT FOR REPLICATION 方式复制 CHECK 约束，以便在同步期间不强制执行约束。|  
|**0x20000**|以 NOT FOR REPLICATION 方式复制 FOREIGN KEY 约束，以便在同步期间不强制执行约束。|  
|**0x40000**|复制与已分区表或已分区索引相关联的文件组。|  
|**0x80000**|复制已分区表的分区方案。|  
|**0x100000**|复制已分区索引的分区方案。|  
|**0x200000**|复制表统计信息。|  
|**0x400000**|默认绑定|  
|**0x800000**|规则绑定|  
|**0x1000000**|全文索引|  
|**0x2000000**|不复制绑定到 **xml** 列的 xml 架构集合。|  
|**0x4000000**|复制 **xml** 列的索引。|  
|**0x8000000**|创建订阅服务器中尚不存在的任何架构。|  
|**0x10000000**|将订阅服务器上的 **xml** 列转换为 **ntext** 。|  
|**0x20000000**|将 (nvarchar 中引入的大型对象数据类型（ **(max)**、 **varchar (max)** 和 **varbinary** ）转换 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 为支持的数据类型 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。|  
|**0x40000000**|复制权限。|  
|**0x80000000**|尝试删除不属于发布一部分的任何对象的依赖项。|  
|**0x100000000**|使用此选项可以复制 FILESTREAM 属性，如果该属性在 **varbinary (max)** 列上指定。 如果要将表复制到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 订阅服务器，请勿指定此选项。 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]不管如何设置此架构选项，都不支持将包含 FILESTREAM 列的表复制到订阅服务器。<br /><br /> 请参阅相关选项 **0x800000000**。|  
|**0x200000000**|将中引入的日期和时间数据类型转换为在的早期版本中支持的数据类型 (**日期**、**时间**、datetimeoffset 和 **datetime2**) [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**0x400000000**|复制数据和索引的压缩选项。 有关详细信息，请参阅 [Data Compression](../../relational-databases/data-compression/data-compression.md)。|  
|**0x800000000**|设置此选项可将 FILESTREAM 数据存储到订阅服务器上其自身的文件组中。 如果不设置此选项，FILESTREAM 数据将存储在默认文件组中。 由于复制操作不创建文件组，因此如果您设置此选项，您必须先创建文件组，然后在订阅服务器上应用快照。 有关如何在应用快照之前创建对象的详细信息，请参阅 [在应用快照之前和之后执行脚本](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied)。<br /><br /> 请参阅相关选项 **0x100000000**。|  
|**0x1000000000**|将公共语言运行时 (CLR) 用户定义类型 (Udt) 大于8000字节到 **varbinary (max)** ，以便将 UDT 类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。|  
|**0x2000000000**|将 **hierarchyid** 数据类型转换为 **varbinary (max)** ，以便可以将 **hierarchyid** 类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。 有关如何在复制的表中使用 **hierarchyid** 列的详细信息，请参阅 [hierarchyid &#40;transact-sql&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)。|  
|**0x4000000000**|复制表的任何筛选的索引。 有关筛选索引的详细信息，请参阅 [创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)。|  
|**0x8000000000**|将 **geography** 和 **geometry** 数据类型转换为 **varbinary (max)** 以便可以将这些类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。|  
|**0x10000000000**|复制 **地域** 和 **几何图形** 类型的列上的索引。|  
|**0x20000000000**|复制列的 SPARSE 属性。 有关此属性的详细信息，请参阅 [使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。|  
|**0x40000000000**|允许快照代理编写脚本，以便在订阅服务器上创建内存优化表。|  
|**0x80000000000**|将聚集索引转换为内存优化项目的非聚集索引。|  
|**0x400000000000**|将表上的所有非聚集列存储索引复制 (s) |  
|**0x800000000000**|复制 () 的表的所有 flitered 非聚集列存储索引。|  
|Null|复制会自动将 *schema_option* 设置为默认值，该值取决于其他项目属性。 “备注”部分的“默认架构选项”表根据项目类型和复制类型列出了默认架构选项。<br /><br /> 非发布的默认值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为 **0x050D3**。|  
  
 并非所有 *schema_option* 值都对每种类型的复制和项目类型都有效。 "备注" 部分中的 " **有效架构选项** " 表显示了可根据项目类型和复制类型的组合选择的有效架构选项。  
  
`[ @destination_owner = ] 'destination_owner'` 目标对象的所有者的名称。 *destination_owner* 的默认值为 **sysname**，默认值为 NULL。 当未指定 *destination_owner* 时，将根据以下规则自动指定所有者：  
  
|条件|目标对象所有者|  
|---------------|------------------------------|  
|发布使用本机模式的大容量复制来生成初始快照，该快照只支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。|默认为 *source_owner* 的值。|  
|从非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器发布。|默认值为目标数据库的所有者。|  
|发布使用字符模式的大容量复制来生成初始快照，该快照支持非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。|不分配。|  
  
 若要支持非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器， *destination_owner* 必须为 NULL。  
  
`[ @status = ] status` 指定项目是否处于活动状态，以及如何传播更改的其他选项。 *状态* 为 **tinyint**，可以是一个或多个值的 [| (按位或)](../../t-sql/language-elements/bitwise-or-transact-sql.md) 产品。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|项目是活动的。|  
|**8**|在 INSERT 语句中包含列名。|  
|**16** (默认值) |使用参数化语句。|  
|**24**|在 INSERT 语句中包含列名，并使用参数化语句。|  
|**64**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 例如，使用参数化语句的活动项目在此列中的值为 17。 如果值为 **0** ，则表示项目处于非活动状态，而且未定义其他属性。  
  
`[ @source_owner = ] 'source_owner'` 源对象的所有者。 *source_owner* 的默认值为 **sysname**，默认值为 NULL。 必须为 Oracle 发布服务器指定 *source_owner* 。  
  
`[ @sync_object_owner = ] 'sync_object_owner'` 用于定义已发布项目的视图的所有者。 *sync_object_owner* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @filter_owner = ] 'filter_owner'` 筛选器的所有者。 *filter_owner* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @source_object = ] 'source_object'` 要发布的数据库对象。 *source_object* 的默认值为 **sysname**，默认值为 NULL。 如果 *source_table* 为 null，则 *source_object* 不能为 null。应使用 *source_object* 而不是 *source_table*。 有关可以使用快照复制或事务复制进行发布的对象类型的详细信息，请参阅 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)。  
  
`[ @artid = ] _article_ID_ OUTPUT` 新项目的项目 ID。 *article_ID* 的默认值为 **int** ，默认值为 NULL，它是一个 OUTPUT 参数。  
  
`[ @auto_identity_range = ] 'auto_identity_range'` 启用和禁用在创建发布时对其进行的自动标识范围处理。 *auto_identity_range* 为 **nvarchar (5)**，可以是以下值之一：  
  
|值|说明|  
|-----------|-----------------|  
|true|启用自动标识范围处理|  
|**false**|禁用自动标识范围处理|  
|NULL（默认值）|标识范围处理由 *identityrangemanagementoption* 设置。|  
  
> [!NOTE]  
>  *auto_identity_range* 已被弃用，提供它只是为了向后兼容。 应使用 *identityrangemanagementoption* 来指定标识范围管理选项。 有关详细信息，请参阅[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
`[ @pub_identity_range = ] pub_identity_range` 如果项目已将 *identityrangemanagementoption* 设置为 **auto** 或 *auto_identity_range* 设置为 **true**，则控制发布服务器上的范围大小。 *pub_identity_range* 为 **bigint**，默认值为 NULL。 *对于 Oracle 发布服务器不支持*。  
  
`[ @identity_range = ] identity_range` 如果项目的 *identityrangemanagementoption* 设置为 **auto** 或 *auto_identity_range* 设置为 **true**，则控制订阅服务器上的范围大小。 *identity_range* 为 **bigint**，默认值为 NULL。 当 *auto_identity_range* 设置为 **true** 时使用。 *对于 Oracle 发布服务器不支持*。  
  
`[ @threshold = ] threshold` 控制分发代理何时分配新标识范围的百分比值。 当使用 " *阈值* " 中指定的百分比值时，分发代理将创建一个新的标识范围。 *阈值* 为 **bigint**，默认值为 NULL。 当 *identityrangemanagementoption* 设置为 **auto** 或 *auto_identity_range* 设置为 **true** 时使用。 *对于 Oracle 发布服务器不支持*。  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为0。  
  
 **0** 指定添加项目不会导致快照无效。 如果存储过程检测到更改需要新的快照，则会发生错误并且不进行任何更改。  
  
 **1** 指定添加项目可能会导致快照无效，如果存在需要新快照的订阅，将授予将现有快照标记为过时并生成新快照的权限。  
  
`[ @use_default_datatypes = ] use_default_datatypes` 在从 Oracle 发布服务器发布项目时，是否使用默认的列数据类型映射。 *use_default_datatypes* 为 bit，默认值为1。  
  
 **1** = 使用默认项目列映射。 可以通过执行 [sp_getdefaultdatatypemapping](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)来显示默认的数据类型映射。  
  
 **0** = 定义了自定义项目列映射，因此 **sp_addarticle** 不调用 [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) 。  
  
 如果 *use_default_datatypes* 设置为 **0**，则对于每个要从默认值更改的列映射，都必须执行一次 [sp_changearticlecolumndatatype](../../relational-databases/system-stored-procedures/sp-changearticlecolumndatatype-transact-sql.md) 。 定义所有自定义列映射后，必须执行 [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)。  
  
> [!NOTE]  
>  该参数只应当用于 Oracle 发布服务器。 如果为发布服务器将 *use_default_datatypes* 设置为 **0** ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 则会生成错误。  
  
`[ @identityrangemanagementoption = ] identityrangemanagementoption` 指定如何处理项目的标识范围管理。 *identityrangemanagementoption* 是 **nvarchar (10)**，可以是以下值之一。  
  
|值|描述|  
|-----------|-----------------|  
|**无**|复制不会显式执行标识范围管理。 仅当为了保持与 SQL Server 早期版本的向后兼容性时，才建议使用该选项。 禁止用于对等复制。|  
|**手动**|使用 NOT FOR REPLICATION 标记标识列，以启用手动标识范围处理。|  
|**auto**|指定自动管理标识范围。|  
|NULL（默认值）|如果 *auto_identity_range* 的值不为 **true**，则默认值为 "**无**"。 对等拓扑默认情况下为 **手动** (*auto_identity_range* 将被忽略) 。|  
  
 为了向后兼容，当 *identityrangemanagementoption* 的值为 NULL 时，将检查 *auto_identity_range* 的值。 但是，当 *identityrangemanagementoption* 的值不为 NULL 时，将忽略 *auto_identity_range* 的值。  
  
 有关详细信息，请参阅[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  在将项目添加到发布服务器时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @fire_triggers_on_snapshot = ] 'fire_triggers_on_snapshot'` 在应用初始快照时是否执行已复制的用户触发器。 *fire_triggers_on_snapshot* 为 **nvarchar (5)**，默认值为 FALSE。 **如果为 true，则** 表示在应用快照时将执行复制的表上的用户触发器。 为了复制触发器， *schema_option* 的位掩码值必须包括值 **0x100**。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addarticle** 用于快照复制或事务复制。  
  
 默认情况下，如果复制不支持列数据类型，则复制不发布源表中的任何列。 如果需要发布这样的列，则必须执行 [sp_articlecolumn](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) ，才能添加列。  
  
 将项目添加到支持对等事务复制的发布中时，将应用下列限制：  
  
-   必须为所有基于日志的项目指定参数化语句。 *Status* 值中必须包含 **16** 。  
  
-   目标表的名称和所有者必须与源表匹配。  
  
-   不能水平或垂直筛选项目。  
  
-   不支持自动标识范围管理。 必须将 *identityrangemanagementoption* 的值指定为 manual。  
  
-   如果表中存在 **timestamp** 列，则必须将0x08 包含在 *schema_option* 中，以便将列复制为 **时间戳**。  
  
-   不能为 *ins_cmd*、 *upd_cmd* 和 *del_cmd* 指定 **SQL** 值。  
  
 有关详细信息，请参阅 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)。  
  
 发布对象时，对象的定义会复制到订阅服务器。 如果要发布的数据库对象依赖于一个或多个其他对象，则必须发布所有被引用对象。 例如，如果要发布的视图依赖于一个表，则也必须发布该表。  
  
 如果 *vertical_partition* 设置为 " **true**"，则 **sp_addarticle** 在 sp_articlecolumn 添加最后一个 [)](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)后将视图的创建推迟到 [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) (调用之后。  
  
 如果发布允许更新订阅，并且已发布的表中没有 **唯一标识符** 列，则 **sp_addarticle** 会自动将 **uniqueidentifier** 列添加到表中。  
  
 复制到不是 (异类复制) 实例的订阅服务器时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，仅 [!INCLUDE[tsql](../../includes/tsql-md.md)] 支持 **INSERT**、 **UPDATE** 和 **DELETE** 命令语句。  
  
 运行日志读取器代理时，将项目添加到对等发布可能导致日志读取器代理和添加该项目的进程之间的死锁。 为了避免此问题，在将项目添加到对等发布前，请使用复制监视器停止正在添加项目的节点上的日志读取器代理。 在添加项目后，重新启动日志读取器代理。  
  
 设置 `@del_cmd = 'NONE'` 或时 `@ins_cmd = 'NONE'` ， **更新** 命令的传播还会受到限制，因为在发生界限更新时不发送这些命令。 （界定更新是来自发布服务器的一种 UPDATE 语句，它复制为订阅服务器上的 DELETE/INSERT 对。）  
  
## <a name="default-schema-options"></a>默认架构选项  
 此表描述了复制设置的默认值（如果用户未指定 *schema_options* ），其中此值依赖于 (在顶部) 显示的复制类型，项目类型 (向下) 第一列显示。  
  
|文章类型|事务复制|快照复制|  
|------------------|----------------------|------|  
|**aggregate schema only**|**0x01**|**0x01**|  
|**func schema only**|**0x01**|**0x01**|  
|**indexed view schema only**|**0x01**|**0x01**|  
|**indexed view logbased**|**0x30F3**|**0x3071**|  
|**indexed view logbase manualboth**|**0x30F3**|**0x3071**|  
|**indexed view logbased manualfilter**|**0x30F3**|**0x3071**|  
|**indexed view logbased manualview**|**0x30F3**|**0x3071**|  
|**logbased**|**0x30F3**|**0x3071**|  
|**logbased manualfilter**|**0x30F3**|**0x3071**|  
|**logbased manualview**|**0x30F3**|**0x3071**|  
|**proc exec**|**0x01**|**0x01**|  
|**proc schema only**|**0x01**|**0x01**|  
|**serializable proc exec**|**0x01**|**0x01**|  
|**view schema only**|**0x01**|**0x01**|  
  
> [!NOTE]
>  如果为排队更新启用了发布，则会将 *schema_option* 值 **0x80** 添加到表中显示的默认值。 非发布的默认 *schema_option* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是 **0x050D3**。  
  
## <a name="valid-schema-options"></a>有效架构选项  
 下表描述了 *schema_option* 允许的值，这些值基于在顶部) 中 (显示的复制类型，项目类型 (在第一列) 下显示。  
  
|文章类型|事务复制|快照复制|  
|------------------|----------------------|------|  
|**logbased**|所有选项|所有选项，但 **0x02**|  
|**logbased manualfilter**|所有选项|所有选项，但 **0x02**|  
|**logbased manualview**|所有选项|所有选项，但 **0x02**|  
|**indexed view logbased**|所有选项|所有选项，但 **0x02**|  
|**indexed view logbased manualfilter**|所有选项|所有选项，但 **0x02**|  
|**indexed view logbased manualview**|所有选项|所有选项，但 **0x02**|  
|**indexed view logbase manualboth**|所有选项|所有选项，但 **0x02**|  
|**proc exec**|**0x01**、 **0x20**、 **0x2000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x10000000**、 **0x20000000**、 **0x40000000** 和 **0x80000000**|**0x01**、 **0x20**、 **0x2000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x10000000**、 **0x20000000**、 **0x40000000** 和 **0x80000000**|  
|**serializable proc exec**|**0x01**、 **0x20**、 **0x2000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x10000000**、 **0x20000000**、 **0x40000000** 和 **0x80000000**|**0x01**、 **0x20**、 **0x2000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x10000000**、 **0x20000000**、 **0x40000000** 和 **0x80000000**|  
|**proc schema only**|**0x01**、 **0x20**、 **0x2000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x10000000**、 **0x20000000**、 **0x40000000** 和 **0x80000000**|**0x01**、 **0x20**、 **0x2000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x10000000**、 **0x20000000**、 **0x40000000** 和 **0x80000000**|  
|**view schema only**|**0x01**、 **0x010**、 **0x020**、 **0x040**、 **0x0100**、 **0x2000**、 **0x40000**、 **0x100000**、 **0x200000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x40000000** 和 **0x80000000**|**0x01**、 **0x010**、 **0x020**、 **0x040**、 **0x0100**、 **0x2000**、 **0x40000**、 **0x100000**、 **0x200000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x40000000** 和 **0x80000000**|  
|**func schema only**|**0x01**、 **0x20**、 **0x2000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x10000000**、 **0x20000000**、 **0x40000000** 和 **0x80000000**|**0x01**、 **0x20**、 **0x2000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x10000000**、 **0x20000000**、 **0x40000000** 和 **0x80000000**|  
|**indexed view schema only**|**0x01**、 **0x010**、 **0x020**、 **0x040**、 **0x0100**、 **0x2000**、 **0x40000**、 **0x100000**、 **0x200000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x40000000** 和 **0x80000000**|**0x01**、 **0x010**、 **0x020**、 **0x040**、 **0x0100**、 **0x2000**、 **0x40000**、 **0x100000**、 **0x200000**、 **0x400000 处**、 **0x800000**、 **0x2000000**、 **0x8000000**、 **0x40000000** 和 **0x80000000**|  
  
> [!NOTE]
>  对于排队更新发布，必须启用 **0x8000** 和 **0x80** *schema_option* 值。 非发布支持的 *schema_option* 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包括： **0x01**、 **0x02**、 **0x10**、 **0x40**、 **0x80**、 **0x1000**、 **0x4000** 和 **0x8000**。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-addarticle-transact-sql_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addarticle**。  
  
## <a name="see-also"></a>另请参阅  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [sp_articlecolumn (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_articlefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_articleview &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [复制存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
