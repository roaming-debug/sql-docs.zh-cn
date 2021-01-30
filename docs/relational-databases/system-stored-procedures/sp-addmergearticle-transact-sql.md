---
description: sp_addmergearticle (Transact-SQL)
title: sp_addmergearticle (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergearticle
- sp_addmergearticle_TSQL
helpviewer_keywords:
- sp_addmergearticle
ms.assetid: 0df654ea-24e2-4c61-a75a-ecaa7a140a6c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 52e59c1c4c7a75c209be99cbb6caacc425bd39f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192358"
---
# <a name="sp_addmergearticle-transact-sql"></a>sp_addmergearticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  在现有合并发布中添加项目。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addmergearticle [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
        , [ @source_object = ] 'source_object'   
    [ , [ @type = ] 'type' ]   
    [ , [ @description = ] 'description' ]   
    [ , [ @column_tracking = ] 'column_tracking' ]   
    [ , [ @status = ] 'status' ]   
    [ , [ @pre_creation_cmd = ] 'pre_creation_cmd' ]   
    [ , [ @creation_script = ] 'creation_script' ]   
    [ , [ @schema_option = ] schema_option ]   
    [ , [ @subset_filterclause = ] 'subset_filterclause' ]   
    [ , [ @article_resolver = ] 'article_resolver' ]   
    [ , [ @resolver_info = ] 'resolver_info' ]   
    [ , [ @source_owner = ] 'source_owner' ]   
    [ , [ @destination_owner = ] 'destination_owner' ]   
    [ , [ @vertical_partition = ] 'vertical_partition' ]   
    [ , [ @auto_identity_range = ] 'auto_identity_range' ]   
    [ , [ @pub_identity_range = ] pub_identity_range ]   
    [ , [ @identity_range = ] identity_range ]   
    [ , [ @threshold = ] threshold ]   
    [ , [ @verify_resolver_signature = ] verify_resolver_signature ]   
    [ , [ @destination_object = ] 'destination_object' ]   
    [ , [ @allow_interactive_resolver = ] 'allow_interactive_resolver' ]   
    [ , [ @fast_multicol_updateproc = ] 'fast_multicol_updateproc' ]   
    [ , [ @check_permissions = ] check_permissions ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @published_in_tran_pub = ] 'published_in_tran_pub' ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @logical_record_level_conflict_detection = ] 'logical_record_level_conflict_detection' ]  
    [ , [ @logical_record_level_conflict_resolution = ] 'logical_record_level_conflict_resolution' ]  
    [ , [ @partition_options = ] partition_options ]  
    [ , [ @processing_order = ] processing_order ]  
    [ , [ @subscriber_upload_options = ] subscriber_upload_options ]  
    [ , [ @identityrangemanagementoption = ] 'identityrangemanagementoption' ]  
    [ , [ @delete_tracking = ] delete_tracking ]  
    [ , [ @compensate_for_errors = ] 'compensate_for_errors' ]   
    [ , [ @stream_blob_columns = ] 'stream_blob_columns' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含项目的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 项目的名称。 该名称在发布中必须唯一。 *项目* 是 **sysname**，无默认值。 *项目* 必须在运行的本地计算机上 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并且必须符合标识符规则。  
  
`[ @source_object = ] 'source_object'` 要发布的数据库对象。 *source_object* **sysname**，无默认值。 有关可使用合并复制发布的对象类型的详细信息，请参阅 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)。  
  
`[ @type = ] 'type'` 项目的类型。 *type* 的数据类型为 **sysname**，默认值为 **table**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**表** (默认值) |具有架构和数据的表。 复制会监视该表以确定要复制的数据。|  
|**func schema only**|仅具有架构的函数。|  
|**仅限****索引视图** 架构|仅具有架构的索引视图。|  
|**proc schema only**|仅具有架构的存储过程。|  
|**仅限同义词架构**|仅具有架构的同义词。|  
|**view schema only**|仅具有架构的视图。|  
  
`[ @description = ] 'description'` 项目的说明。 *描述* 为 **nvarchar (255)**，默认值为 NULL。  
  
`[ @column_tracking = ] 'column_tracking'` 列级跟踪的设置。 *column_tracking* 为 **nvarchar (10)**，默认值为 FALSE。 **true** 表示启用列跟踪。 **false** 将关闭列跟踪，并在行级别保持冲突检测。 如果已经在其他合并发布中发布过该表，则使用的列跟踪值必须与基于此表的现有项目所用的值相同。 此参数只适用于表项目。  
  
> [!NOTE]  
>  如果行跟踪用于冲突检测（默认值），则基表最多可包含 1,024 列，但是必须从项目中筛选列，以便最多发布 246 列。 如果使用列跟踪，则基表最多可包含 246 列。  
  
`[ @status = ] 'status'` 项目的状态。 *状态* 为 **nvarchar (10)**，默认值为 "未 **同步**"。 如果 **处于活动状态**，则运行用于发布表的初始处理脚本。 如果未 **同步**，则发布表的初始处理脚本将在下一次运行快照代理时运行。  
  
`[ @pre_creation_cmd = ] 'pre_creation_cmd'` 指定应用快照时，如果订阅服务器上存在该表，系统将执行的操作。 *pre_creation_cmd* 为 **nvarchar (10)**，可以是以下值之一。  
  
|值|描述|  
|-----------|-----------------|  
|**无**|如果订阅服务器上已存在该表，则不执行任何操作。|  
|**delete**|根据子集筛选器中的 WHERE 子句发出 delete 命令。|  
|**删除** (默认值) |删除该表，然后重新创建一个表。 需要它来支持 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器。|  
|**truncate**|截断目标表。|  
  
`[ @creation_script = ] 'creation_script'` 用于创建订阅数据库中项目的可选项目架构脚本的路径和名称。 *creation_script* 为 **nvarchar (255)**，默认值为 NULL。  
  
> [!NOTE]  
>  创建脚本不在 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器上运行。  
  
`[ @schema_option = ] schema_option` 给定项目的架构生成选项的位图。 *schema_option* 是 **二进制 (8)**，可以是一个或多个值的 [| (按位 "或")](../../t-sql/language-elements/bitwise-or-transact-sql.md) "。  
  
|值|说明|  
|-----------|-----------------|  
|**0x00**|禁用快照代理的脚本，并使用在 *creation_script* 中定义的提供的架构预创建脚本。|  
|**0x01**|生成对象创建（CREATE TABLE、CREATE PROCEDURE 等）。 这是存储过程项目的默认值。|  
|**0x10**|生成对应的聚集索引。 即使不设置此选项，但如果已在已发布的表中定义与主键相关的索引以及 UNIQUE 约束，那么也会生成它们。|  
|**0x20**|在订阅服务器中将用户定义数据类型 (UDT) 转换为基本数据类型。 如果 UDT 列是主键的一部分或者计算列引用 UDT 列，则当 UDT 列上具有 CHECK 或 DEFAULT 约束时，此选项无法使用。|  
|**0x40**|生成相应的非聚集索引。 即使不设置此选项，但如果已在已发布的表中定义与主键相关的索引以及 UNIQUE 约束，那么也会生成它们。|  
|**0x80**|复制 PRIMARY KEY 约束。 即使未启用选项 **0x10** 和 **0x40** ，也会复制与约束相关的任何索引。|  
|**0x100**|如果已定义，则复制表项目的用户触发器。|  
|**0x200**|复制 FOREIGN KEY 约束。 如果被引用的表不是发布的一部分，则不会复制已发布表的任何 FOREIGN KEY 约束。|  
|**0x400**|复制检查约束。|  
|**0x800**|复制默认值。|  
|**0x1000**|复制列级排序规则。|  
|**0x2000**|复制与已发布项目源对象关联的扩展属性。|  
|**0x4000**|复制 UNIQUE 约束。 即使未启用选项 **0x10** 和 **0x40** ，也会复制与约束相关的任何索引。|  
|**0x8000**|此选项对运行 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本的发布服务器无效。|  
|**0x10000**|以 NOT FOR REPLICATION 方式复制 CHECK 约束，以便在同步期间不强制执行约束。|  
|**0x20000**|以 NOT FOR REPLICATION 方式复制 FOREIGN KEY 约束，以便在同步期间不强制执行约束。|  
|**0x40000**|复制与已分区表或已分区索引相关联的文件组。|  
|**0x80000**|复制已分区表的分区方案。|  
|**0x100000**|复制已分区索引的分区方案。|  
|**0x200000**|复制表统计信息。|  
|**0x400000**|复制默认绑定。|  
|**0x800000**|复制规则绑定。|  
|**0x1000000**|复制全文索引。|  
|**0x2000000**|不复制绑定到 **xml** 列的 xml 架构集合。|  
|**0x4000000**|复制 **xml** 列的索引。|  
|**0x8000000**|创建订阅服务器没有的任何架构。|  
|**0x10000000**|将订阅服务器上的 **xml** 列转换为 **ntext** 。|  
|**0x20000000**|将 (nvarchar 中引入的大型对象数据类型（ **(max)**、 **varchar (max)** 和 **varbinary** ）转换 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 为支持的数据类型 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。|  
|**0x40000000**|复制权限。|  
|**0x80000000**|尝试删除不属于发布的任何对象的依赖关系。|  
|**0x100000000**|使用此选项可以复制 FILESTREAM 属性，如果该属性在 **varbinary (max)** 列上指定。 如果要将表复制到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 订阅服务器，请勿指定此选项。 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]不管如何设置此架构选项，都不支持将包含 FILESTREAM 列的表复制到订阅服务器。 请参阅相关选项 **0x800000000**。|  
|**0x200000000**|将中引入的日期和时间数据类型转换为在的早期版本中支持的数据类型 (**日期**、**时间**、datetimeoffset 和 **datetime2**) [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**0x400000000**|复制数据和索引的压缩选项。 有关详细信息，请参阅 [Data Compression](../../relational-databases/data-compression/data-compression.md)。|  
|**0x800000000**|设置此选项可将 FILESTREAM 数据存储到订阅服务器上其自身的文件组中。 如果不设置此选项，FILESTREAM 数据将存储在默认文件组中。 由于复制操作不创建文件组，因此如果您设置此选项，您必须先创建文件组，然后在订阅服务器上应用快照。 有关如何在应用快照之前创建对象的详细信息，请参阅 [在应用快照之前和之后执行脚本](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied)。<br /><br /> 请参阅相关选项 **0x100000000**。|  
|**0x1000000000**|将公共语言运行时 (CLR) 用户定义类型 (Udt) 到 **varbinary (max)** ，使类型为 udt 的列能够复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。|  
|**0x2000000000**|将 **hierarchyid** 数据类型转换为 **varbinary (max)** ，以便可以将 **hierarchyid** 类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。 有关如何在复制的表中使用 **hierarchyid** 列的详细信息，请参阅 [hierarchyid &#40;transact-sql&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)。|  
|**0x4000000000**|复制表的任何筛选的索引。 有关筛选索引的详细信息，请参阅 [创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)。|  
|**0x8000000000**|将 **geography** 和 **geometry** 数据类型转换为 **varbinary (max)** 以便可以将这些类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。|  
|**0x10000000000**|复制 **地域** 和 **几何图形** 类型的列上的索引。|  
  
 如果此值为 NULL，则系统将自动为项目生成一个有效的架构选项。 "备注" 部分的 " **默认架构选项** " 表显示根据项目类型选择的值。 此外，并非所有 *schema_option* 值都对每种类型的复制和项目类型都有效。 "备注" 中给出的 " **有效架构选项** " 表显示了可为给定项目类型指定的选项。  
  
> [!NOTE]  
>  *Schema_option* 参数仅影响初始快照的复制选项。 快照代理生成初始架构并将其应用于订阅服务器后，会根据架构更改复制规则和 [sp_addmergepublication](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)中指定的 *replicate_ddl* 参数设置，将发布架构更改复制到订阅服务器。 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。  
  
`[ @subset_filterclause = ] 'subset_filterclause'` 一个 WHERE 子句，用于指定表项目的水平筛选，但不包含包含的单词。 *subset_filterclause* 为 **nvarchar (1000)**，默认值为空字符串。  
  
> [!IMPORTANT]  
>  为提高性能，建议您不要在参数化行筛选子句中对列名应用函数，如 `LEFT([MyColumn]) = SUSER_SNAME()`。 如果在筛选子句中使用 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 并重写 HOST_NAME 值，则可能需要使用 [convert](../../t-sql/functions/cast-and-convert-transact-sql.md)转换数据类型。 有关此情况的最佳实践的详细信息，请参阅 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的 "覆盖 HOST_NAME ( # A1 值" 部分。  
  
`[ @article_resolver = ] 'article_resolver'` 基于 COM 的冲突解决程序，用于解决表项目中的冲突或为执行表项目中的自定义业务逻辑而调用的 .NET Framework 程序集。 *article_resolver* 是 **varchar (255)**，默认值为 NULL。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 自定义冲突解决程序中列出了此参数的可用值。 如果提供的值并不属于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 冲突解决程序，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将使用指定的冲突解决程序代替系统提供的冲突解决程序。 使用 **sp_enumcustomresolvers** 枚举可用自定义冲突解决程序的列表。 有关详细信息，请参阅 [在合并同步期间执行业务逻辑](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md) 和 [高级合并复制冲突检测和解决](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)。  
  
`[ @resolver_info = ] 'resolver_info'` 用于指定自定义冲突解决程序所需的其他信息。 某些 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 冲突解决程序需要提供列作为冲突解决程序的输入。 *resolver_info* 为 **nvarchar (255)**，默认值为 NULL。 有关详细信息，请参阅 [Microsoft 基于 COM 的冲突解决程序](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md)。  
  
`[ @source_owner = ] 'source_owner'`*Source_object* 的所有者的名称。 *source_owner* 的默认值为 **sysname**，默认值为 NULL。 如果值为 NULL，则假定当前用户为所有者。  
  
`[ @destination_owner = ] 'destination_owner'` 是订阅数据库中对象的所有者，如果不是 "dbo"。 *destination_owner* 的默认值为 **sysname**，默认值为 NULL。 如果值为 NULL，则假定‘dbo’为所有者。  
  
`[ @vertical_partition = ] 'column_filter'` 启用和禁用对表项目的列筛选。 *vertical_partition* 为 **nvarchar (5)** ，默认值为 FALSE。  
  
 **false** 指示没有垂直筛选并发布所有列。  
  
 **如果为 true，则** 清除除声明的主键和 ROWGUID 列之外的所有列。 使用 **sp_mergearticlecolumn** 来添加列。  
  
`[ @auto_identity_range = ] 'automatic_identity_range'` 启用和禁用在创建发布时对此表项目进行的自动标识范围处理。 *auto_identity_range* 为 **nvarchar (5)**，默认值为 FALSE。 **true** 表示启用自动标识范围处理，而 **false** 则禁用。  
  
> [!NOTE]  
>  *auto_identity_range* 已被弃用，提供它只是为了向后兼容。 应使用 *identityrangemanagementoption* 来指定标识范围管理选项。 有关详细信息，请参阅[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
`[ @pub_identity_range = ] pub_identity_range` 当使用自动标识范围管理时，控制分配给具有服务器订阅的订阅服务器的标识范围大小。 此标识范围是为重新发布订阅服务器保留的，用于分配给其自身的订阅服务器。 *pub_identity_range* 为 **bigint**，默认值为 NULL。 如果 *identityrangemanagementoption* 为 **auto** 或 *auto_identity_range* 为 **true**，则必须指定此参数。  
  
`[ @identity_range = ] identity_range` 当使用自动标识范围管理时，控制分配给发布服务器和订阅服务器的标识范围大小。 *identity_range* 为 **bigint**，默认值为 NULL。 如果 *identityrangemanagementoption* 为 **auto** 或 *auto_identity_range* 为 **true**，则必须指定此参数。  
  
> [!NOTE]  
>  *identity_range* 控制使用早期版本的重新发布订阅服务器上的标识范围大小 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @threshold = ] threshold` 控制合并代理何时分配新标识范围的百分比值。 当使用 " *阈值* " 中指定的百分比值时，合并代理将创建一个新的标识范围。 *阈值* 为 **int**，默认值为 NULL。 如果 *identityrangemanagementoption* 为 **auto** 或 *auto_identity_range* 为 **true**，则必须指定此参数。  
  
`[ @verify_resolver_signature = ] verify_resolver_signature` 指定在合并复制中使用冲突解决程序之前是否验证数字签名。 *verify_resolver_signature* 的值为 **int**，默认值为1。  
  
 **0** 指定将不验证签名。  
  
 **1** 指定将验证签名是否来自受信任的源。  
  
`[ @destination_object = ] 'destination_object'` 订阅数据库中对象的名称。 *destination_object* 为 **sysname**，默认值为 " **\@ source_object**"。 仅当项目为仅架构项目（如存储过程、视图和 UDF 等）时，才能指定此参数。 如果指定的项目是表项目，则 *\@ source_object* 中的值将覆盖 *destination_object* 中的值。  
  
`[ @allow_interactive_resolver = ] 'allow_interactive_resolver'` 启用或禁用对项目使用交互式冲突解决程序。 *allow_interactive_resolver* 为 **nvarchar (5)**，默认值为 FALSE。 **如果为 true** ，则允许对项目使用交互式冲突解决程序; **false** 会禁用它。  
  
> [!NOTE]  
>  [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器不支持交互式冲突解决程序。  
  
`[ @fast_multicol_updateproc = ] 'fast_multicol_updateproc'` 此参数已弃用，并且保持为脚本的向后兼容性。  
  
`[ @check_permissions = ] check_permissions` 是在合并代理将更改应用于发布服务器时验证的表级权限的位图。 如果合并进程使用的发布服务器登录名/用户帐户没有正确的表权限，则无效更改将被记录为冲突。 *check_permissions* 为 **int**，可以是以下一个或多个值的 [| (位或)](../../t-sql/language-elements/bitwise-or-transact-sql.md) 乘积。  
  
|值|说明|  
|-----------|-----------------|  
|**0x00** (默认值) |不检查权限。|  
|**0x10**|检查了发布服务器上的权限后，才能上载订阅服务器上的插入操作。|  
|**0x20**|检查了发布服务器上的权限后，才能上载订阅服务器上的更新操作。|  
|**0x40**|检查了发布服务器上的权限后，才能上载订阅服务器上的删除操作。|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为0。  
  
 **0** 指定添加项目不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 指定添加项目可能导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。 向包含现有快照的发布中添加项目时， *force_invalidate_snapshot* 设置为 **1** 。  
  
`[ @published_in_tran_pub = ] 'published_in_tran_pub'` 指示合并发布中的项目也在事务发布中发布。 *published_in_tran_pub* 为 **nvarchar (5)**，默认值为 FALSE。 **true** 指定项目也在事务发布中发布。  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 为一个 **位**，默认值为0。  
  
 **0** 指定添加项目不会导致重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化现有订阅，则会发生错误，并且不进行任何更改。  
  
 **1** 表示对合并项目所做的更改将导致重新初始化现有订阅，并授予重新初始化订阅的权限。 当 *subset_filterclause* 指定参数化行筛选器时， *force_reinit_subscription* 设置为 **1** 。  
  
`[ @logical_record_level_conflict_detection = ] 'logical_record_level_conflict_detection'` 为作为逻辑记录成员的项目指定冲突检测的级别。 *logical_record_level_conflict_detection* 为 **nvarchar (5)**，默认值为 FALSE。  
  
 **true** 指定在逻辑记录中的任意位置进行更改时将检测到冲突。  
  
 **false** 指定 *column_tracking* 使用指定的默认冲突检测。 有关详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
> [!NOTE]  
>  由于订阅服务器不支持逻辑记录 [!INCLUDE[ssEW](../../includes/ssew-md.md)] ，因此必须将 *logical_record_level_conflict_detection* 的值指定为 **False** 以支持这些订阅服务器。  
  
`[ @logical_record_level_conflict_resolution = ] 'logical_record_level_conflict_resolution'` 为作为逻辑记录成员的项目指定冲突解决级别。 *logical_record_level_conflict_resolution* 为 **nvarchar (5)**，默认值为 FALSE。  
  
 **如果为 true，则** 指定整个入选逻辑记录将覆盖失败的逻辑记录。  
  
 **false** 指定入选行不约束为逻辑记录。 如果 *logical_record_level_conflict_detection* 为 **true**，则 *logical_record_level_conflict_resolution* 也必须设置为 **true**。 有关详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
> [!NOTE]  
>  由于订阅服务器不支持逻辑记录 [!INCLUDE[ssEW](../../includes/ssew-md.md)] ，因此必须将 *logical_record_level_conflict_resolution* 的值指定为 **False** 以支持这些订阅服务器。  
  
`[ @partition_options = ] partition_options` 定义项目中数据的分区方式，当所有行只属于一个分区或只属于一个订阅时，将启用性能优化。 *partition_options* 为 **tinyint**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0** （默认值）|项目的筛选是静态的，或者不会为每个分区生成一个唯一数据子集（即“重叠”分区）。|  
|**1**|分区是重叠的，订阅服务器中执行的数据操作语言 (DML) 更新不能更改行所属的分区。|  
|**2**|对项目的筛选将生成不重叠分区，但多个订阅服务器可以接收到相同的分区。|  
|**3**|对项目的筛选将为每个订阅生成唯一的不重叠分区。|  
  
> [!NOTE]  
>  如果某个项目的源表已在另一个发布中发布，则这两个项目的 *partition_options* 的值必须相同。  
  
`[ @processing_order = ] processing_order` 指示合并发布中项目的处理顺序。 *processing_order* 的值为 **int**，默认值为0。 **0** 指定项目未排序，任何其他值表示本文处理顺序的序号值。 项目按值的由低到高顺序进行处理。 如果两个项目具有相同的值，则处理顺序由 [sysmergearticles](../../relational-databases/system-tables/sysmergearticles-transact-sql.md) 系统表中的项目别名的顺序确定。 有关详细信息，请参阅[指定合并复制属性](../../relational-databases/replication/merge/specify-merge-replication-properties.md)。  
  
`[ @subscriber_upload_options = ] subscriber_upload_options` 定义对具有客户端订阅的订阅服务器上所做的更新的限制。 有关详细信息，请参阅[使用仅下载项目优化合并复制性能](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)。 *subscriber_upload_options* 为 **tinyint**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0** （默认值）|无限制。 将订阅服务器上所做的更改上载到发布服务器。|  
|**1**|允许在订阅服务器上进行更改，但不将更改上载到发布服务器。|  
|**2**|不允许在订阅服务器上进行更改。|  
  
 更改 *subscriber_upload_options* 需要通过调用 [sp_reinitmergepullsubscription &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-reinitmergepullsubscription-transact-sql.md)来重新初始化订阅。  
  
> [!NOTE]  
>  如果某个项目的源表已在另一个发布中发布，则这两个项目的 *subscriber_upload_options* 的值必须相同。  
  
`[ @identityrangemanagementoption = ] identityrangemanagementoption` 指定如何处理项目的标识范围管理。 *identityrangemanagementoption* 是 **nvarchar (10)**，可以是以下值之一。  
  
|值|描述|  
|-----------|-----------------|  
|**无**|禁用标识范围管理。|  
|**手动**|使用 NOT FOR REPLICATION 标记标识列，以启用手动标识范围处理。|  
|**auto**|指定自动管理标识范围。|  
|NULL（默认值）|如果 *auto_identity_range* 的值不为 **true**，则默认值为 "**无**"。|  
  
 为了向后兼容，当 *identityrangemanagementoption* 的值为 NULL 时，将检查 *auto_identity_range* 的值。 但是，当 *identityrangemanagementoption* 的值不为 NULL 时，将忽略 *auto_identity_range* 的值。 有关详细信息，请参阅[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
`[ @delete_tracking = ] 'delete_tracking'` 指示是否复制删除。 *delete_tracking* 为 **nvarchar (5)**，默认值为 TRUE。 **false** 表示不复制删除， **如果为 true，则** 指示复制删除，这是合并复制的常见行为。 将 *delete_tracking* 设置为 **false** 时，必须在发布服务器上手动删除订阅服务器上删除的行，并且必须在订阅服务器上手动删除在发布服务器上删除的行。  
  
> [!IMPORTANT]  
>  将 *delete_tracking* 设置为 **false** 将导致无法收敛。 如果某个项目的源表已在另一个发布中发布，则这两个项目的 *delete_tracking* 的值必须相同。  
  
> [!NOTE]  
>  不能使用 **新建发布向导** 或 "**发布属性**" 对话框来设置 *delete_tracking* 选项。  
  
`[ @compensate_for_errors = ] 'compensate_for_errors'` 指示在同步过程中遇到错误时是否执行补偿操作。 *compensate_for_errors i* s **nvarchar (5)**，默认值为 FALSE。 如果设置为 **true**，则在同步过程中不能在订阅服务器或发布服务器上应用的更改将始终导致补偿操作以撤消更改;但是，如果一个错误配置的订阅服务器生成错误，则可能会导致其他订阅服务器和发布服务器上的更改被撤消。 **如果设置为 false** ，则将禁用这些补偿操作，但错误仍记录为带有补偿，后续合并将继续尝试应用更改，直到成功为止。  
  
> [!IMPORTANT]  
>  尽管受影响行中的数据可能会无法收敛，但是只要解决了发生的错误，就可应用更改，并且数据也会收敛。 如果某个项目的源表已在另一个发布中发布，则这两个项目的 *compensate_for_errors* 的值必须相同。  
  
`[ @stream_blob_columns = ] 'stream_blob_columns'` 指定在复制二进制大型对象列时使用数据流优化。 *stream_blob_columns* 为 **nvarchar (5)**，默认值为 FALSE。 **如果为 true** ，则表示将尝试进行优化。 启用 FILESTREAM 时， *stream_blob_columns* 设置为 true。 这使复制 FILESTREAM 数据的性能达到最佳并减少内存使用率。 若要强制 FILESTREAM 表项目不使用 blob 流式处理，请使用 **sp_changemergearticle** 将 *stream_blob_columns* 设置为 false。  
  
> [!IMPORTANT]  
>  在同步过程中，启用此内存优化可能会降低合并代理的性能。 仅当复制包含数兆字节数据的列时，才应使用此选项。  
  
> [!NOTE]  
>  即使 *stream_blob_columns* 设置为 **true**，某些合并复制功能（如逻辑记录）仍会阻止在复制二进制大型对象时使用流优化。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_addmergearticle** 用于合并复制。  
  
 发布对象时，对象的定义会复制到订阅服务器。 如果要发布的数据库对象依赖于一个或多个其他对象，则必须发布所有被引用对象。 例如，如果要发布的视图依赖于一个表，则也必须发布该表。  
  
 如果为 *partition_options* 指定值 **3** ，则该项目中每个数据分区只能有一个订阅。 如果创建了另一个订阅，而这个新订阅的筛选条件解析到的分区与现有订阅的分区相同，则会删除现有订阅。  
  
 将 *partition_options* 的值指定为3时，只要合并代理运行，分区快照就会更快地过期。 使用此选项时，应考虑启用订阅服务器请求的分区快照。 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
 使用 *subset_filterclause* 将具有静态水平筛选器的项目添加到现有发布，其中包含具有参数化筛选器的项目，则需要重新初始化订阅。  
  
 指定 *processing_order* 时，建议在项目顺序值之间留出间隙，这使得以后设置新值变得更加容易。 例如，如果有三个项目 Article1、Article2 和 Article3，请将 *processing_order* 设置为10、20和30，而不是1、2和3。 有关详细信息，请参阅[指定合并复制属性](../../relational-databases/replication/merge/specify-merge-replication-properties.md)。  
  
## <a name="default-schema-option-table"></a>默认架构选项表  
 此表描述存储过程设置的默认值（如果为 *schema_option* 指定了 NULL 值，这取决于项目类型）。  
  
|文章类型|架构选项值|  
|------------------|-------------------------|  
|**func schema only**|**0x01**|  
|**indexed view schema only**|**0x01**|  
|**proc schema only**|**0x01**|  
|**table**|  -  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 具有本机模式快照的0x0C034FD1 及更高版本的兼容发布。<br /><br />   -  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 使用字符模式快照的0x08034FF1 及更高版本的兼容发布。|  
|**view schema only**|**0x01**|  
  
> [!NOTE]  
>  如果发布支持早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则 **表** 的默认架构选项是 **0x30034FF1**。  
  
## <a name="valid-schema-option-table"></a>有效架构选项表  
 下表描述了根据项目类型 *schema_option* 允许的值。  
  
|文章类型|架构选项值|  
|------------------|--------------------------|  
|**func schema only**|**0x01** 和 **0x2000**|  
|**indexed view schema only**|**0x01**、 **0x040**、 **0x0100**、 **0x2000**、 **0x40000**、 **0x1000000** 和 **0x200000**|  
|**proc schema only**|**0x01** 和 **0x2000**|  
|**table**|所有选项。|  
|**view schema only**|**0x01**、 **0x040**、 **0x0100**、 **0x2000**、 **0x40000**、 **0x1000000** 和 **0x200000**|  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddMergeArticle](../../relational-databases/replication/codesnippet/tsql/sp-addmergearticle-trans_1.sql)]  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)   
 [sp_changemergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_dropmergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [sp_helpmergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
