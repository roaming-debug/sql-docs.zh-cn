---
description: sp_changearticle (Transact-SQL)
title: sp_changearticle (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changearticle
- sp_changearticle_TSQL
helpviewer_keywords:
- sp_changearticle
ms.assetid: 24c33ca5-f03a-4417-a267-131ca5ba6bb5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 22d64091d206161546856e7186958ab60217bcc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203736"
---
# <a name="sp_changearticle-transact-sql"></a>sp_changearticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改事务或快照发布中的项目属性。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changearticle [ [@publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含项目的发布的名称。 *发布* 为 **sysname**，默认值为 NULL。  
  
`[ @article = ] 'article'` 要更改其属性的项目的名称。 *项目* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @property = ] 'property'` 要更改的项目属性。 *属性* 为 **nvarchar (100)**。  
  
`[ @value = ] 'value'` 项目属性的新值。 *值* 为 **nvarchar (255)**。  
  
 下表说明项目的属性和这些属性的值。  
  
|properties|值|说明|  
|--------------|------------|-----------------|  
|**creation_script**||用于创建目标表的项目架构脚本的路径和名称。 默认值为 NULL。|  
|**del_cmd**||要执行的 DELETE 语句，否则从日志构造。|  
|description||项目的新说明项。|  
|**dest_object**||提供该列是为了向后兼容。 使用 **dest_table**。|  
|**dest_table**||新目标表。|  
|**destination_owner**||目标对象所有者的名称。|  
|**filter**||要用于筛选表（水平筛选）的新存储过程。 默认值为 NULL。 对于对等复制中的发布，此值不能更改。|  
|**fire_triggers_on_snapshot**|true|应用初始快照时会执行已复制的用户触发器。<br /><br /> 注意：对于要复制的触发器， *schema_option* 的位掩码值必须包括值 **0x100**。|  
||**false**|应用初始快照时不会执行已复制的用户触发器。|  
|**identity_range**||控制在订阅服务器中分配的标识范围的大小。 对等复制不支持此属性。|  
|**ins_cmd**||要执行的 INSERT 语句，否则从日志构造。|  
|**pre_creation_cmd**||可以在应用同步之前除去、删除或截断目标表的预创建命令。|  
||**无**|不使用命令。|  
||**击落**|删除目标表。|  
||**delete**|删除目标表。|  
||**truncate**|截断目标表。|  
|**pub_identity_range**||控制在订阅服务器中分配的标识范围的大小。 对等复制不支持此属性。|  
|**schema_option**||为给定项目指定架构生成选项的位图。 *schema_option* 是 **(8) 的二进制**。 有关详细信息，请参阅本主题后面的 "备注" 部分。|  
||**0x00**|通过快照代理禁用脚本。|  
||**0x01**|生成对象创建（CREATE TABLE、CREATE PROCEDURE 等）。|  
||**0x02**|如果已定义，则生成传播项目更改的存储过程。|  
||**0x04**|使用 IDENTITY 属性为标识列编写脚本。|  
||**0x08**|复制 **时间戳** 列。 如果未设置，则 **时间戳** 列将作为 **二进制** 复制。|  
||**0x10**|生成对应的聚集索引。|  
||**0x20**|在订阅服务器中将用户定义数据类型 (UDT) 转换为基本数据类型。 如果 UDT 列是主键的一部分或者计算列引用 UDT 列，则当 UDT 列上具有 CHECK 或 DEFAULT 约束时，此选项无法使用。 Oracle 发布服务器不支持。|  
||**0x40**|生成相应的非聚集索引。|  
||**0x80**|包含已对主键声明的引用完整性。|  
||**0x100**|如果已定义，则复制表项目的用户触发器。|  
||**0x200**|复制 FOREIGN KEY 约束。 如果被引用的表不是发布的一部分，则不会复制已发布表的任何 FOREIGN KEY 约束。|  
||**0x400**|复制检查约束。|  
||**0x800**|复制默认值。|  
||**0x1000**|复制列级排序规则。|  
||**0x2000**|复制与已发布项目源对象关联的扩展属性。|  
||**0x4000**|如果在表项目上定义了唯一键，则复制唯一键。|  
||**0x8000**|使用 ALTER TABLE 语句将表项目上的主键和唯一键作为约束复制。<br /><br /> 注意：此选项已被弃用。 请改用 **0x80** 和 **0x4000** 。|  
||**0x10000**|以 NOT FOR REPLICATION 方式复制 CHECK 约束，以便在同步期间不强制执行约束。|  
||**0x20000**|以 NOT FOR REPLICATION 方式复制 FOREIGN KEY 约束，以便在同步期间不强制执行约束。|  
||**0x40000**|复制与已分区表或已分区索引相关联的文件组。|  
||**0x80000**|复制已分区表的分区方案。|  
||**0x100000**|复制已分区索引的分区方案。|  
||**0x200000**|复制表统计信息。|  
||**0x400000**|默认绑定|  
||**0x800000**|规则绑定|  
||**0x1000000**|全文索引|  
||**0x2000000**|不复制绑定到 **xml** 列的 xml 架构集合。|  
||**0x4000000**|复制 **xml** 列的索引。|  
||**0x8000000**|创建订阅服务器中尚不存在的任何架构。|  
||**0x10000000**|将订阅服务器上的 **xml** 列转换为 **ntext** 。|  
||**0x20000000**|将中引入的大型对象数据类型转换 (**nvarchar (max)**、 **varchar (max)** 和 **varbinary (最大**) ) [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。|  
||**0x40000000**|复制权限。|  
||**0x80000000**|尝试删除不属于发布一部分的任何对象的依赖项。|  
||**0x100000000**|使用此选项可以复制 FILESTREAM 属性，如果该属性在 **varbinary (max)** 列上指定。 如果要将表复制到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 订阅服务器，请勿指定此选项。 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]不管如何设置此架构选项，都不支持将包含 FILESTREAM 列的表复制到订阅服务器。<br /><br /> 请参阅相关选项 **0x800000000**。|  
||**0x200000000**|将中引入的日期和时间数据类型转换为在的早期版本中支持的数据类型 (**日期**、 **时间**、 **datetimeoffset** 和 **datetime2**) [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
||**0x400000000**|复制数据和索引的压缩选项。 有关详细信息，请参阅 [Data Compression](../../relational-databases/data-compression/data-compression.md)。|  
||**0x800000000**|设置此选项可将 FILESTREAM 数据存储到订阅服务器上其自身的文件组中。 如果不设置此选项，FILESTREAM 数据将存储在默认文件组中。 由于复制操作不创建文件组，因此如果您设置此选项，您必须先创建文件组，然后在订阅服务器上应用快照。 有关如何在应用快照之前创建对象的详细信息，请参阅 [在应用快照之前和之后执行脚本](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied)。<br /><br /> 请参阅相关选项 **0x100000000**。|  
||**0x1000000000**|将公共语言运行时 (CLR) 用户定义类型 (Udt) 大于8000字节到 **varbinary (最大)** ，以便可以将 UDT 类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。|  
||**0x2000000000**|将 **hierarchyid** 数据类型转换为 **varbinary (max)** ，以便可以将 **hierarchyid** 类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。 有关如何在复制的表中使用 **hierarchyid** 列的详细信息，请参阅 [hierarchyid &#40;transact-sql&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)。|  
||**0x4000000000**|复制表的任何筛选的索引。 有关筛选索引的详细信息，请参阅 [创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)。|  
||**0x8000000000**|将 **geography** 和 **geometry** 数据类型转换为 **varbinary (max)** 以便可以将这些类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。|  
||**0x10000000000**|复制 **地域** 和 **几何图形** 类型的列上的索引。|  
||**0x20000000000**|复制列的 SPARSE 属性。 有关此属性的详细信息，请参阅 [使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。|  
||**0x40000000000**|允许快照代理编写脚本，以便在订阅服务器上创建内存优化表。|  
||**0x80000000000**|将聚集索引转换为内存优化项目的非聚集索引。|  
|**status**||指定属性的新状态。|  
||**dts horizontal partitions**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
||**include column names**|复制的 INSERT 语句中包括列名。|  
||**no column names**|复制的 INSERT 语句中不包括列名。|  
||**no dts horizontal partitions**|项目的水平分区不由可转换的订阅定义。|  
||**无**|清除 [sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md) 表中的所有状态选项，并将项目标记为非活动。|  
||**parameters**|使用参数化命令将更改传播给订阅服务器。 这是新项目的默认设置。|  
||**字符串文本**|使用字符串文字值将更改传播给订阅服务器。|  
|**sync_object**||用于生成同步输出文件的表或视图的名称。 默认值为 NULL。 Oracle 发布服务器不支持。|  
|**表空间**||标识从 Oracle 数据库发布的项目的日志记录表所使用的表空间。 有关详细信息，请参阅[管理 Oracle 表空间](../../relational-databases/replication/non-sql/manage-oracle-tablespaces.md)。|  
|**threshold**||用于控制分发代理何时分配新标识范围的百分比值。 对等复制不支持此属性。|  
|type ||Oracle 发布服务器不支持。|  
||**logbased**|基于日志的项目。|  
||**logbased manualboth**|具有手动筛选器和手动视图并且基于日志的项目。 此选项要求同时设置 " *sync_object* " 和 " *筛选器* " 属性。 Oracle 发布服务器不支持。|  
||**logbased manualfilter**|具有手动筛选器并且基于日志的项目。 此选项要求同时设置 " *sync_object* " 和 " *筛选器* " 属性。 Oracle 发布服务器不支持。|  
||**logbased manualview**|具有手动视图并且基于日志的项目。 此选项要求同时设置 " *sync_object* " 属性。 Oracle 发布服务器不支持。|  
||**索引 viewlogbased**|基于日志的索引视图项目。 Oracle 发布服务器不支持。 对于此类型的项目，不需要单独发布基表。|  
||**indexed viewlogbased manualboth**|具有手动筛选器和手动视图并且基于日志的索引视图项目。 此选项要求同时设置 " *sync_object* " 和 " *筛选器* " 属性。 对于此类型的项目，不需要单独发布基表。 Oracle 发布服务器不支持。|  
||**indexed viewlogbased manualfilter**|具有手动筛选器并且基于日志的索引视图项目。 此选项需要同时设置 *sync_object* 和 *筛选器* 属性。 对于此类型的项目，不需要单独发布基表。 Oracle 发布服务器不支持。|  
||**indexed viewlogbased manualview**|具有手动视图并且基于日志的索引视图项目。 此选项要求同时设置 " *sync_object* " 属性。 对于此类型的项目，不需要单独发布基表。 Oracle 发布服务器不支持。|  
|**upd_cmd**||要执行的 UPDATE 语句，否则从日志构造。|  
|Null|Null|返回可更改的项目属性列表。|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对项目所做的更改可能导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。  
  
 有关在更改时需要生成新快照的属性，请参阅“备注”部分。  
  
`[ @force_reinit_subscription = ]force_reinit_subscription_` 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 是一 **位** ，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化现有订阅，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对项目所做的更改会导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
 有关在更改时需要重新初始化所有现有订阅的属性，请参阅“备注”部分。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  更改发布服务器上的项目属性时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changearticle** 用于快照复制和事务复制。  
  
 当项目属于支持对等事务复制的发布时，只能更改 **说明**、 **ins_cmd**、 **upd_cmd** 和 **del_cmd** 属性。  
  
 更改以下任何属性都需要生成新的快照，并且必须将 *force_invalidate_snapshot* 参数的值指定为 **1** ：  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **ins_cmd**  
  
-   **pre_creation_cmd**  
  
-   **schema_options**  
  
-   **upd_cmd**  
  
 更改以下任何属性都需要重新初始化现有的订阅，并且必须将 *force_reinit_subscription* 参数的值指定为 **1** 。  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **filter**  
  
-   **ins_cmd**  
  
-   **status**  
  
-   **upd_cmd**  
  
 在现有发布中，可以使用 **sp_changearticle** 更改项目，而无需删除并重新创建整个发布。  
  
> [!NOTE]  
>  更改 *schema_option* 的值时，系统不执行位更新。 这意味着，当你使用 **sp_changearticle** 设置 *schema_option* 时，可能会关闭现有的位设置。 若要保留现有设置，应在要设置的值与 *schema_option* 的当前值之间执行 [| (按位 "或")](../../t-sql/language-elements/bitwise-or-transact-sql.md) ，这可通过执行 [sp_helparticle](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)来确定。  
  
## <a name="valid-schema-options"></a>有效架构选项  
 下表描述了 *schema_option* 允许的值，这些值基于在顶部) 中 (显示的复制类型，并且项目类型 (在第一列) 下显示。  
  
| 文章类型 | 复制类型-事务性 | 复制类型-快照 |
| ------------ | -------------------------------- | --------------------------- |
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
>  对于排队更新发布，必须启用 *schema_option* 值 **0x80** 。 非发布支持的 *schema_option* 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包括： **0x01**、 **0x02**、 **0x10**、 **0x40**、 **0x80**、 **0x1000** 和 **0x4000**。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_changetranarticle](../../relational-databases/replication/codesnippet/tsql/sp-changearticle-transac_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_changearticle**。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改项目属性](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addarticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_droparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)  
  
  
