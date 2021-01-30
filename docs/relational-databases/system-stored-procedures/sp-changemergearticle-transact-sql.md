---
description: sp_changemergearticle (Transact-SQL)
title: sp_changemergearticle (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/09/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergearticle_TSQL
- sp_changemergearticle
helpviewer_keywords:
- sp_changemergearticle
ms.assetid: 0dc3da5c-4af6-45be-b5f0-074da182def2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b504644f5b96cc631a6352d71f2b387e8040b5a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159890"
---
# <a name="sp_changemergearticle-transact-sql"></a>sp_changemergearticle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改合并项目的属性。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changemergearticle [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 项目所在的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 要更改的项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @property = ] 'property'` 给定项目和发布的要更改的属性。 *属性* 为 **nvarchar (30)**，可以是表中列出的值之一。  
  
`[ @value = ] 'value'` 指定的属性的新值。 *值* 为 **nvarchar (1000)**，可以是表中列出的值之一。  
  
 下表说明项目的属性和这些属性的值。  
  
|properties|值|说明|  
|--------------|------------|-----------------|  
|**allow_interactive_resolver**|true|允许对项目使用交互式冲突解决程序。|  
||**false**|不允许对项目使用交互式冲突解决程序。|  
|**article_resolver**||项目的自定义冲突解决程序。 仅适用于表项目。|  
|**check_permissions** (位图) |**0x00**|不检查表级权限。|  
||**0x10**|将订阅服务器上的 INSERT 语句应用于发布服务器之前，要在发布服务器上检查表级权限。|  
||**0x20**|将订阅服务器上的 UPDATE 语句应用于发布服务器之前，要在发布服务器上检查表级权限。|  
||**0x40**|将订阅服务器上的 DELETE 语句应用于发布服务器之前，要在发布服务器上检查表级权限。|  
|**column_tracking**|true|打开列级跟踪。 仅适用于表项目。<br /><br /> 注意：发布的列超过246列时，不能使用列级跟踪。|  
||**false**|关闭列级跟踪，保留行级冲突检测。 仅适用于表项目。|  
|**compensate_for_errors**|true|同步过程中发生错误时执行补救措施。 有关详细信息，请参阅 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。|  
||**false**|不执行补救措施，这是默认行为。 有关详细信息，请参阅 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。<br /><br /> **\* \* 重要 \* 提示 \* ：** 尽管受影响的行中的数据可能看起来不收敛，但一旦你解决了任何错误，就可以应用更改，并且数据将聚合在一起。 如果某个项目的源表已在另一个发布中发布，则这两个项目的 *compensate_for_errors* 的值必须相同。|  
|**creation_script**||用于在订阅数据库中创建项目的可选项目架构脚本的路径和名称。|  
|**delete_tracking**|true|复制 DELETE 语句，这是默认行为。|  
||**false**|不复制 DELETE 语句。<br /><br /> **\* \* 重要 \* 的 \*** 设置 **delete_tracking** 为 **false** 会导致非收敛性，需要手动删除删除的行。|  
|description||项目的说明项。|  
|**destination_owner**||订阅数据库中对象的所有者的名称（如果不是 **dbo**）。|  
|**identity_range**||**用于指定** 在分配新标识值时要使用的范围大小（如果项目的 **identityrangemanagementoption** 设置为 **auto** 或 **auto_identity_range** 设置为 **true**。 仅适用于表项目。 有关详细信息，请参阅 [复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)的 "合并复制" 部分。|  
|**identityrangemanagementoption**|**手动**|禁用自动标识范围管理。 使用 NOT FOR REPLICATION 标记标识列，启用手动标识范围处理。 有关详细信息，请参阅[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)。|  
||**无**|禁用所有标识范围管理。|  
|**logical_record_level_conflict_detection**|true|如果逻辑记录中发生了更改，则将检测到冲突。 要求将 **logical_record_level_conflict_resolution** 设置为 **true**。|  
||**false**|**Column_tracking** 使用指定的默认冲突检测。|  
|**logical_record_level_conflict_resolution**|true|整个入选逻辑记录覆盖落选逻辑记录。|  
||**false**|入选行未被约束为逻辑记录。|  
|**partition_options**|**0**|项目的筛选是静态的，或者不会为每个分区生成唯一数据子集（即“重叠”分区）。|  
||**1**|分区是重叠的，订阅服务器上执行的 DML 更新无法更改行所属的分区。|  
||**2**|对项目的筛选将生成不重叠分区，但多个订阅服务器可以接收到相同的分区。|  
||**3**|对项目的筛选将为每个订阅生成唯一的不重叠分区。<br /><br /> 注意：如果为 **partition_options** 指定值 **3** ，则该项目中每个数据分区只能有一个订阅。 如果创建了另一个订阅，而这个新订阅的筛选条件解析到的分区与现有订阅的分区相同，则会删除现有订阅。|  
|**pre_creation_command**|**无**|如果订阅服务器上已存在该表，则不执行任何操作。|  
||**delete**|根据子集筛选器中的 WHERE 子句发出 delete 命令。|  
||**击落**|删除该表，然后重新创建一个表。|  
||**truncate**|截断目标表。|  
|**processing_order**||**int** ，指示合并发布中项目的处理顺序。|  
|**pub_identity_range**||如果项目的 **identityrangemanagementoption** 设置为 **auto** 或 **auto_identity_range** 设置为 **true**，则指定分配给具有服务器订阅的订阅 **服务器的范围** 大小。 此标识范围是为重新发布订阅服务器保留的，用于分配给其自身的订阅服务器。 仅适用于表项目。 有关详细信息，请参阅 [复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)的 "合并复制" 部分。|  
|**published_in_tran_pub**|true|项目也在事务发布中发布。|  
||**false**|项目不在事务发布中发布。|  
|**resolver_info**||用于指定自定义冲突解决程序所需的其他信息。 某些 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 冲突解决程序需要提供列作为冲突解决程序的输入。 **resolver_info** 为 **nvarchar (255)**，默认值为 NULL。 有关详细信息，请参阅 [Microsoft 基于 COM 的冲突解决程序](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md)。|  
|**schema_option** (位图) ||有关详细信息，请参阅本主题后面的 "备注" 部分。|  
||**0x00**|禁用快照代理的脚本，并使用 **creation_script** 中提供的脚本。|  
||**0x01**|生成对象创建脚本（CREATE TABLE、CREATE PROCEDURE 等）。|  
||**0x10**|生成对应的聚集索引。|  
||**0x20**|在订阅服务器中将用户定义数据类型转换为基本数据类型。 如果 UDT 列是主键的一部分或者计算列引用用户定义类型 (UDT) 列，则当 UDT 列上具有 CHECK 或 DEFAULT 约束时，此选项无法使用。|  
||**0x40**|生成相应的非聚集索引。|  
||**0x80**|包含已对主键声明的引用完整性。|  
||**0x100**|如果已定义，则复制表项目的用户触发器。|  
||**0x200**|复制 FOREIGN KEY 约束。 如果被引用的表不是发布的一部分，则不会复制已发布表的任何 FOREIGN KEY 约束。|  
||**0x400**|复制检查约束。|  
||**0x800**|复制默认值。|  
||**0x1000**|复制列级排序规则。|  
||**0x2000**|复制与已发布项目源对象关联的扩展属性。|  
||**0x4000**|如果在表项目上定义了唯一键，则复制唯一键。|  
||**0x8000**|在编写约束脚本时生成 ALTER TABLE 语句。|  
||**0x10000**|以 NOT FOR REPLICATION 方式复制 CHECK 约束，以便在同步期间不强制执行约束。|  
||**0x20000**|以 NOT FOR REPLICATION 方式复制 FOREIGN KEY 约束，以便在同步期间不强制执行约束。|  
||**0x40000**|复制与已分区表或已分区索引相关联的文件组。|  
||**0x80000**|复制已分区表的分区方案。|  
||**0x100000**|复制已分区索引的分区方案。|  
||**0x200000**|复制表统计信息。|  
||**0x400000**|复制默认绑定|  
||**0x800000**|复制规则绑定。|  
||**0x1000000**|复制全文索引。|  
||**0x2000000**|不复制绑定到 **xml** 列的 xml 架构集合。|  
||**0x4000000**|复制 **xml** 列的索引。|  
||**0x8000000**|创建订阅服务器中尚不存在的任何架构。|  
||**0x10000000**|将订阅服务器上的 **xml** 列转换为 **ntext** 。|  
||**0x20000000**|将中引入的大型对象数据类型转换 (**nvarchar (max)**、 **varchar (max)** 和 **varbinary (最大**) ) [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。|  
||**0x40000000**|复制权限。|  
||**0x80000000**|尝试删除不属于发布一部分的任何对象的依赖项。|  
||**0x100000000**|使用此选项可以复制 FILESTREAM 属性，如果该属性在 **varbinary (max)** 列上指定。 如果要将表复制到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 订阅服务器，请勿指定此选项。 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]不管如何设置此架构选项，都不支持将包含 FILESTREAM 列的表复制到订阅服务器。 请参阅相关选项 **0x800000000**。|  
||**0x200000000**|将中引入的日期和时间数据类型转换为在的早期版本中支持的数据类型 (**日期**、 **时间**、 **datetimeoffset** 和 **datetime2**) [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
||**0x400000000**|复制数据和索引的压缩选项。 有关详细信息，请参阅 [Data Compression](../../relational-databases/data-compression/data-compression.md)。|  
||**0x800000000**|设置此选项可将 FILESTREAM 数据存储到订阅服务器上其自身的文件组中。 如果不设置此选项，FILESTREAM 数据将存储在默认文件组中。 由于复制操作不创建文件组，因此如果您设置此选项，您必须先创建文件组，然后在订阅服务器上应用快照。 有关如何在应用快照之前创建对象的详细信息，请参阅 [在应用快照之前和之后执行脚本](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied)。<br /><br /> 请参阅相关选项 **0x100000000**。|  
||**0x1000000000**|将公共语言运行时 (CLR) 用户定义类型 (Udt) 到 **varbinary (max)** ，使类型为 udt 的列能够复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。|  
||**0x2000000000**|将 **hierarchyid** 数据类型转换为 **varbinary (max)** ，以便可以将 **hierarchyid** 类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。 有关如何在复制的表中使用 **hierarchyid** 列的详细信息，请参阅 [hierarchyid &#40;transact-sql&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)。|  
||**0x4000000000**|复制表的任何筛选的索引。 有关筛选索引的详细信息，请参阅 [创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)。|  
||**0x8000000000**|将 **geography** 和 **geometry** 数据类型转换为 **varbinary (max)** 以便可以将这些类型的列复制到运行的订阅服务器 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。|  
||**0x10000000000**|复制 **地域** 和 **几何图形** 类型的列上的索引。|  
||Null|系统自动为项目生成一个有效的架构选项。|  
|**status**|**active**|用于发布表的初始处理脚本已运行。|  
||**unsynced**|用于发布表的初始处理脚本在下一次运行快照代理时运行。|  
|**stream_blob_columns**|true|复制二进制大型对象列时使用数据流优化。 但是，某些合并复制功能（如逻辑记录）仍可阻止使用流优化。 启用 FILESTREAM 时， *stream_blob_columns* 设置为 true。 这使复制 FILESTREAM 数据的性能达到最佳并减少内存使用率。 若要强制 FILESTREAM 表项目不使用 blob 流式处理，请将 *stream_blob_columns* 设置为 false。<br /><br /> **\* \* 重要 \* 说明 \*** 在同步过程中，启用此内存优化可能会影响合并代理的性能。 仅当复制包含数兆字节数据的列时，才应使用此选项。|  
||**false**|复制二进制大型对象列时不使用优化。|  
|**subscriber_upload_options**|**0**|不限制在包含客户端订阅的订阅服务器上进行更新；将更改上载到发布服务器。 更改此属性可能需要重新初始化现有的订阅服务器。|  
||**1**|允许在包含客户端订阅的订阅服务器上进行更改，但不将更改上载到发布服务器。|  
||**2**|不允许在包含客户端订阅的订阅服务器上进行更改。|  
|**subset_filterclause**||用于指定水平筛选的 WHERE 子句。 仅适用于表项目。<br /><br /> 由于性能原因，建议不要对参数化行筛选器子句（如）中的列名应用函数。 **\* \* \* \*** `LEFT([MyColumn]) = SUSER_SNAME()` 如果在筛选子句中使用 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 并重写 HOST_NAME 值，则可能需要使用 [convert](../../t-sql/functions/cast-and-convert-transact-sql.md)转换数据类型。 有关此情况的最佳实践的详细信息，请参阅 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的 "覆盖 HOST_NAME ( # A1 值" 部分。|  
|**threshold**||用于运行 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 或早期版本的的订阅服务器的百分比值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 **阈值** 控制合并代理分配新标识范围的时间。 如果使用了在阈值中指定的百分比值，合并代理将创建新的标识范围。 当 **identityrangemanagementoption** 设置为 **auto** 或 **auto_identity_range** 设置为 **true** 时使用。 仅适用于表项目。 有关详细信息，请参阅 [复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)的 "合并复制" 部分。|  
|**verify_resolver_signature**|**1**|通过验证自定义冲突解决程序的数字签名来确定该签名是否来自可信来源。|  
||**0**|不通过验证自定义冲突解决程序的数字签名来确定该签名是否来自可信来源。|  
|NULL（默认值）||返回 *属性* 的支持值的列表。|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对合并项目所做的更改不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 表示对合并项目所做的更改可能会导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。  
  
 有关在更改时需要生成新快照的属性，请参阅“备注”部分。  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对合并项目所做的更改不会导致重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化现有订阅，则会发生错误，并且不进行任何更改。  
  
 **1** 表示对合并项目所做的更改会导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
 有关在更改时需要重新初始化所有现有订阅的属性，请参阅“备注”部分。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changemergearticle** 用于合并复制。  
  
 由于 **sp_changemergearticle** 用于更改最初使用 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)指定的项目属性，请参阅 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 以获取有关这些属性的其他信息。  
  
 更改以下属性需要生成新的快照，并且必须将 *force_invalidate_snapshot* 参数的值指定为 **1** ：  
  
-   **check_permissions**  
  
-   **column_tracking**  
  
-   **destination_owner**  
  
-   **pre_creation_command**  
  
-   **schema_options**  
  
-   **subset_filterclause**  
  
 若要更改下列属性，则需要重新初始化现有的订阅，并且必须将 *force_reinit_subscription* 参数的值指定为 **1** ：  
  
-   **check_permissions**  
  
-   **column_tracking**  
  
-   **destination_owner**  
  
-   **pre_creation_command**  
  
-   **identityrangemanagementoption**  
  
-   **subscriber_upload_options**  
  
-   **subset_filterclause**  
  
-   **creation_script**  
  
-   **schema_option**  
  
-   **logical_record_level_conflict_detection**  
  
-   **logical_record_level_conflict_resolution**  
  
 将 **partition_options** 的值指定为3时，只要合并代理运行，分区快照就会更快地过期。 使用此选项时，应考虑启用订阅服务器请求的分区快照。 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
 设置 **column_tracking** 属性时，如果表已在其他合并发布中发布，则列跟踪必须与基于此表的现有项目所使用的值相同。 此参数只适用于表项目。  
  
 如果多个发布基于相同的基础表发布项目，则更改一个项目的 **delete_tracking** 属性或 **compensate_for_errors** 属性会导致对基于同一个表的其他项目进行相同的更改。  
  
 如果合并进程使用的发布服务器登录名/用户帐户没有正确的表权限，则无效更改将被记录为冲突。  
  
 更改 **schema_option** 的值时，系统不执行位更新。 这意味着，当你使用 **sp_changemergearticle** 设置 **schema_option** 时，可能会关闭现有的位设置。 若要保留现有设置，应在要设置的值和 *schema_option* 的当前值之间执行 [& (位与)](../../t-sql/language-elements/bitwise-and-transact-sql.md) ，可以通过执行 [sp_helpmergearticle](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)来确定。  
  
> [!CAUTION]  
>  如果有很多 (可能是发布中的数百) 项目，并且你对其中一篇文章执行 **sp_changemergearticle** ，则可能需要很长时间才能完成执行。  
  
## <a name="valid-schema-option-table"></a>有效架构选项表  
 下表描述了允许 *schema_option* 值，具体取决于项目类型。  
  
|文章类型|架构选项值|  
|------------------|--------------------------|  
|**func schema only**|**0x01** 和 **0x2000**|  
|**indexed view schema only**|**0x01**、 **0x040**、 **0x0100**、 **0x2000**、 **0x40000**、 **0x1000000** 和 **0x200000**|  
|**proc schema only**|**0x01** 和 **0x2000**|  
|**table**|所有选项。|  
|**view schema only**|**0x01**、 **0x040**、 **0x0100**、 **0x2000**、 **0x40000**、 **0x1000000** 和 **0x200000**|  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_changemergearticle](../../relational-databases/replication/codesnippet/tsql/sp-changemergearticle-tr_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_changemergearticle**。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改项目属性](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergearticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_dropmergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [sp_helpmergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
