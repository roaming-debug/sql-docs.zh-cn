---
title: 对发布数据库进行架构更改 | Microsoft Docs
description: 复制支持对已发布对象进行一系列架构更改。 了解默认传播到所有 SQL Server 订阅服务器的架构更改。
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], schema changes
- snapshot replication [SQL Server], replicating schema changes
- merge replication [SQL Server replication], replicating schema changes
- transactional replication, replicating schema changes
- schemas [SQL Server replication], replicating changes
- publishing [SQL Server replication], schema changes
ms.assetid: 926c88d7-a844-402f-bcb9-db49e5013b69
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 81929c8da672e9294d276b5b562e55886a1c3515
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468998"
---
# <a name="make-schema-changes-on-publication-databases"></a>对发布数据库进行架构更改
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  复制支持对已发布对象进行多种架构更改。 对 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 发布服务器中相应的发布对象进行下列任何一种架构更改时，默认会将该更改传播到所有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器：  
  
-   ALTER TABLE  
  
-   如果已启用架构更改复制，并且拓扑包括 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或 [!INCLUDE[ssEWnoversion](../../../includes/ssewnoversion-md.md)] 订阅服务器，则不应使用 ALTER TABLE SET LOCK ESCALATION。

-   ALTER VIEW  
  
-   ALTER PROCEDURE  
  
-   ALTER FUNCTION  
  
-   ALTER TRIGGER  
  
     由于无法复制数据定义语言 [DDL] 触发器，ALTER TRIGGER 只能用于数据操作语言 [DML] 触发器。  
  
> [!IMPORTANT]  
>  对表进行架构更改时必须使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理对象 (SMO)。 在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中进行架构更改时， [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 将尝试删除并重新创建表。 因为无法删除已发布的对象，所以架构更改失败。  
  
 对于事务复制和合并复制，当分发代理或合并代理运行时，将增量传播架构更改。 对于快照复制，将在订阅服务器中应用新快照时传播架构更改。 在快照复制中，每次发生同步时，都将架构的一个新副本发送到订阅服务器。 因此，每次同步时都自动传播对以前发布的对象的所有架构更改（而不只是上面列出的那些）。  
  
 有关在发布中添加和删除项目的信息，请参阅[向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
 **复制架构更改**  
  
 默认情况下会复制上面列出的架构更改。 有关禁止复制架构更改的信息，请参阅 [Replicate Schema Changes](../../../relational-databases/replication/publish/replicate-schema-changes.md)。  
  
## <a name="considerations-for-schema-changes"></a>架构更改的注意事项  
 复制架构更改时，请牢记下列注意事项：  
  
### <a name="general-considerations"></a>一般注意事项  
  
-   架构更改需遵守 [!INCLUDE[tsql](../../../includes/tsql-md.md)]规定的所有限制。 例如，ALTER TABLE 不允许对主键列执行 ALTER 语句。  
  
-   仅对初始快照执行数据类型映射。 架构更改不会映射到以前版本的数据类型。 例如，如果在 `ALTER TABLE ADD datetime2 column` 中使用 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]语句，则不会将数据类型转换为 **ssVersion2005** 订阅服务器的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 。 在某些情况下，架构更改在发布服务器上受到阻止。  
  
-   如果发布被设置为允许传播架构更改，则不论为发布中的项目设置的相关架构选项是什么，都会传播架构更改。 例如，如果选择不复制某个表项目的外键约束，但随后又发出 ALTER TABLE 命令，将外键添加到发布服务器中的表中，那么外键将被添加到订阅服务器中的表中。 若要防止出现这种情况，请在发出 ALTER TABLE 命令之前禁用架构更改的传播。  
  
-   架构更改只应在发布服务器中进行，不应在订阅服务器中（包括重新发布订阅服务器）上进行。 合并复制禁止在订阅服务器中进行架构更改。 事务复制虽然不禁止更改，但更改可能会导致复制失败。  
  
-   传播到重新发布订阅服务器的更改默认情况下会传播到它的订阅服务器。  
  
-   如果架构更改引用存在于发布服务器中而不存在于订阅服务器中的对象或约束，则架构更改将在发布服务器中成功，但在订阅服务器上失败。  
  
-   添加外键时引用的订阅服务器中的所有对象必须与发布服务器中的相应对象具有相同的名称和所有者。  
  
-   不会复制显式添加、删除或更改索引，并且涉及显式索引的任何更改都需要在每个副本集上单独运行。 支持为约束（如主键约束）隐式创建的索引。  
  
-   不支持更改或删除由复制管理的标识列。 有关自动管理标识列的详细信息，请参阅[复制标识列](../../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
-   不支持包含不确定性函数的架构更改，因为它们可能会导致发布服务器和订阅服务器中的数据不同（称为无法收敛）。 例如，如果在发布服务器发出以下命令： `ALTER TABLE SalesOrderDetail ADD OrderDate DATETIME DEFAULT GETDATE()`，则将此命令复制到订阅服务器并执行时，会得到不同的值。 有关不确定性函数的详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
-   建议显式命名约束。 如果不显式命名约束， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将为约束生成名称，并且这些名称在发布服务器和每个订阅服务器中都不同。 这在复制架构更改时会引发问题。 例如，如果在发布服务器中删除一列并删除了一个相关约束，则复制将尝试在订阅服务器中删除该约束。 由于约束名称不同，因此在订阅服务器上的删除将失败。 如果由于约束命名问题而导致同步失败，可以在订阅服务器中手动删除约束然后重新运行合并代理。  
  
-   如果为复制发布了一个表，并且已生成了发布快照，则无法将该表中的列更改为 XML 数据类型。若要更改列，必须先删除复制。  
  
-   对已发布的表执行 DDL 时，“未提交读”不是受支持的隔离级别。  
  
-   不应使用 **SET CONTEXT_INFO** 来修改已对发布的对象执行架构更改的事务的上下文。  
  
#### <a name="adding-columns"></a>添加列  
  
-   若要将一个新列添加到表中并在现有发布中包括该列，请执行 ALTER TABLE \<Table> ADD \<Column>。 默认情况下，此列然后将被复制到所有订阅服务器中。 此列必须允许 NULL 值或包含默认约束。 有关添加列的详细信息，请参阅本主题中的“合并复制”部分。  
  
-   若要将一个新列添加到表中但不在现有发布中包括此列，请禁用架构更改复制，然后执行 ALTER TABLE \<Table> ADD \<Column>。  
  
-   若要在现有发布中包括现有列，请使用 [&#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)、[sp_mergearticlecolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) 或“发布属性 - \<Publication>”对话框。  
  
     有关详细信息，请参阅 [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)。 这要求重新初始化订阅。  
  
-   不支持向已发布的表添加标识列，因为将列复制到订阅服务器时，会导致无法收敛。 发布服务器的标识列中的值取决于受影响的表中行的物理存储顺序。 行在订阅服务器中的存储顺序可能会有所不同。因此对于相同的行，标识列的值可能会不同。  
  
#### <a name="dropping-columns"></a>删除列  
  
-   若要从现有发布中删除列并从发布服务器中的表内删除该列，请执行 ALTER TABLE \<Table> DROP \<Column>。 默认情况下，该列然后将从所有订阅服务器中的表中删除。  
  
-   若要从现有发布中删除列，但在发布服务器中的表内保留该列，请使用 [sp_articlecolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)、[sp_mergearticlecolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) 或“发布属性 - \<Publication>”对话框。  
  
     有关详细信息，请参阅 [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)。 这要求生成一个新的快照。  
  
-   要删除的列不能用于数据库中任何发布的任何项目的筛选子句中。  
  
-   从已发布项目中删除列时，请考虑任何可能影响数据库的列的约束、索引或属性。 例如：  
  
    -   无法从事务发布的项目中删除主键中使用的列，因为这些列由复制使用。  
  
    -   无法从合并发布的项目中删除 rowguid 列，也不能从支持更新订阅的事务性发布的项目中删除 mstran_repl_version 列，因为这些列由复制使用。  
  
    -   不会将索引更改传播到订阅服务器：如果在发布服务器上删除一列并删除相关索引，则不会复制索引删除。 在发布服务器上删除列之前，应在订阅服务器中删除索引，这样从发布服务器向订阅服务器复制列删除时才会成功。 如果由于订阅服务器中的索引而导致同步失败，可以手动删除索引然后重新运行合并代理。  
  
    -   约束应显式命名，以允许删除。 有关详细信息，请参阅本主题后面的“一般注意事项”部分。  
  
### <a name="transactional-replication"></a>事务复制  
  
-   架构更改传播到运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]早期版本的订阅服务器中，但 DDL 语句只应包含订阅服务器中的 SQL Server 版本支持的语法。  
  
     如果订阅服务器重新发布数据，唯一支持的架构更改是添加和删除列。 应当在发布服务器上使用 [sp_repladdcolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-repladdcolumn-transact-sql.md) 和 [sp_repldropcolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-repldropcolumn-transact-sql.md) 执行这些更改，不应使用 ALTER TABLE DDL 语法。  
  
-   不向非 SQL Server 订阅服务器复制架构更改。  
  
-   不会从非[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 发布服务器中传播架构更改。  
  
-   无法更改按表复制的索引视图。 可以更改按索引视图复制的索引视图，但更改它们将导致它们成为普通视图，而不是索引视图。  
  
-   如果发布支持立即更新订阅或排队更新订阅，则必须在进行架构更改前停止系统：必须在发布服务器和订阅服务器中停止已发布的表上的所有活动，还必须将挂起的数据更改传播到所有节点。 架构更改传播到所有节点后，可以在已发布的表上恢复活动。  
  
-   如果发布在对等拓扑中，必须在进行架构更改前停止系统。 有关详细信息，请参阅[停止复制拓扑（复制 Transact-SQL 编程）](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md)。  
  
-   将一个时间戳列添加到表中并将时间戳映射到 binary(8) 将导致重新初始化所有活动订阅的项目。  
  
### <a name="merge-replication"></a>合并复制  
  
-   合并复制处理架构更改的方式由发布兼容级别以及快照设置为本机模式（默认）还是字符模式决定：  
  
    -   若要复制架构更改，发布兼容级别必须至少是 90RTM。 如果订阅服务器运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的早期版本或者兼容级别小于 90RTM，则可以使用 [sp_repladdcolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-repladdcolumn-transact-sql.md) 和 [sp_repldropcolumn &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-repldropcolumn-transact-sql.md) 来添加和删除列。 但是，不推荐使用这些过程。  
  
    -   如果您试图在现有项目中添加包含 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]中引入的数据类型的列，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 具有以下行为：  
  
        ||100RTM，本机快照|100RTM，字符快照|所有其他兼容级别|  
        |-|-----------------------------|--------------------------------|------------------------------------|  
        |**hierarchyid**|允许更改|阻止更改|阻止更改|  
        |**geography** 和 **geometry**|允许更改|允许更改*|阻止更改|  
        |**文件流 (filestream)**|允许更改|阻止更改|阻止更改|  
        |**date**、 **time**、 **datetime2** 和 **datetimeoffset**|允许更改|允许更改*|阻止更改|  
  
         *SQL Server Compact 订阅服务器在订阅服务器中转换这些数据类型。  
  
-   如果在应用架构更改时发生错误（如由于添加的外键引用订阅服务器中不可用的表而导致错误），同步将失败，必须重新初始化订阅。  
  
-   如果在联接筛选器或参数化筛选器中涉及的列上进行架构更改，必须重新初始化所有订阅并重新生成快照。  
  
-   合并复制提供了在排除故障期间跳过架构更改的存储过程。 有关详细信息，请参阅 [sp_markpendingschemachange &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md) 和 [sp_enumeratependingschemachanges &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumeratependingschemachanges-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER VIEW &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-view-transact-sql.md)   
 [ALTER PROCEDURE (Transact-SQL)](../../../t-sql/statements/alter-procedure-transact-sql.md)   
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-function-transact-sql.md)   
 [ALTER TRIGGER (Transact-SQL)](../../../t-sql/statements/alter-trigger-transact-sql.md)   
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [重新生成自定义事务过程以反映架构更改](../../../relational-databases/replication/transactional/transactional-articles-regenerate-to-reflect-schema-changes.md)  
  
  
