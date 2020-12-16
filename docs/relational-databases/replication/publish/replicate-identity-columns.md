---
title: 复制标识列 | Microsoft Docs
description: 在 SQL Server 中，复制处理所有发布和订阅类型中的标识列。 手动管理列或通过复制进行管理。
ms.custom: ''
ms.date: 10/04/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- identities [SQL Server replication]
- identity values [SQL Server replication]
- merge replication [SQL Server replication], identity range management
- publishing [SQL Server replication], identity columns
- transactional replication, identity range management
- identity columns [SQL Server], replication
ms.assetid: eb2f23a8-7ec2-48af-9361-0e3cb87ebaf7
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: acc3ce53ae8da54ab89c2a54e2cc8ea6d3708756
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468948"
---
# <a name="replicate-identity-columns"></a>复制标识列
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  将 IDENTITY 属性分配到列时，[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将为在包含标识列的表中插入的新行自动生成顺序编号。 有关详细信息，请参阅 [IDENTITY（属性）&#40;Transact-SQL&#41;](../../../t-sql/statements/create-table-transact-sql-identity-property.md)。 因为包含的标识列可能是主键的一部分，所以请务必避免在标识列中出现重复值。 若要在多个节点上都有更新的复制拓扑中使用标识列，复制拓扑中的每个节点都必须使用不同范围的标识值，以避免出现重复。  
  
 例如，可以为发布服务器分配范围 1-100，为订阅服务器 A 分配范围 101-200，为订阅服务器 B 分配范围 201-300。 如果在发布服务器中插入行，例如标识值是 65，则将该值复制到每个订阅服务器。 复制在每个订阅服务器上插入数据时，不会增加订阅服务器表中的标识列值，而是插入文字值 65。 仅用户插入，而复制代理不插入，将导致标识列值增加。  
  
 复制会处理所有发布和订阅类型的标识列，从而允许您手动管理列或通过复制自动管理列。  
  
> [!NOTE]  
>  不支持向已发布的表添加标识列，因为将列复制到订阅服务器时，会导致无法收敛。 发布服务器的标识列中的值取决于受影响的表中行的物理存储顺序。 行在订阅服务器中的存储顺序可能会有所不同。因此对于相同的行，标识列的值可能会不同。  
  
## <a name="specifying-an-identity-range-management-option"></a>指定标识范围管理选项  
 复制提供了三个标识范围管理选项：  
  
-   自动。 用于在订阅服务器上进行更新的合并复制和事务复制。 指定发布服务器和订阅服务器的大小范围，复制将自动管理新范围的分配。 复制会为订阅服务器的标识列设置 NOT FOR REPLICATION 选项，这样只有用户插入才会导致订阅服务器上的值增加。  
  
    > [!NOTE]  
    >  订阅服务器必须与发布服务器同步，以接收新范围。 由于订阅服务器上的标识范围是自动分配的，所以如果订阅服务器重复请求新范围，则它可能会用尽可提供的整个标识范围。  
  
-   手动。 用于订阅服务器上不进行更新的快照和事务复制、对等事务复制，或者当应用程序必须以编程方式控制标识范围时。 如果指定手动管理，则必须确保将范围分配给发布服务器和每个订阅服务器，还要确保在使用初始范围时，新范围已分配。 复制会为订阅服务器的标识列设置 NOT FOR REPLICATION 选项。  
  
-   无。 建议仅在为了向后兼容早期版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时使用此选项，并且此选项只出现在事务发布的存储过程界面中。  
  
 若要指定标识范围管理选项，请参阅[管理标识列](../../../relational-databases/replication/publish/manage-identity-columns.md)。  
  
## <a name="assigning-identity-ranges"></a>分配标识范围  
 合并复制和事务复制用不同的方法分配范围，这些方法将在本部分中介绍。  
  
 复制标识列时有两种类型的范围需要考虑：分配给发布服务器和订阅服务器的范围，以及列中数据类型的范围。 下表显示了通常标识列中所使用数据类型的可用范围。 此范围可用于拓扑中的所有节点。 例如，如果使用以 1 开始、增量为 1 的 **smallint** ，则对于发布服务器和所有订阅服务器来说，插入的最大数为 32,767。 插入的实际数取决于所用的值是否有间隔，以及是否使用了阈值。 有关阈值的详细信息，请参阅下列“合并复制”和“具有排队更新订阅的事务复制”两部分。  
  
 如果发布服务器在插入后用尽了其标识范围，并且若此插入是由 **db_owner** 固定服务器角色的某个成员来执行的，那么该发布服务器将会自动分配一个新范围。 如果插入是由不属于该角色的用户执行的，则日志读取器代理、合并代理或属于 **db_owner** 角色的成员的用户必须运行 [sp_adjustpublisheridentityrange &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adjustpublisheridentityrange-transact-sql.md)。 对于事务发布，必须运行日志读取器代理，以便自动分配新范围（默认设置是使代理连续运行）。  
  
> [!WARNING]  
>  在大型批插入过程中，复制触发器只激发一次，不是对每个插入行都激发。 如果标识范围在大型插入期间用尽，则会导致 insert 语句（如 **INSERT INTO** 语句）失败。  
  
|数据类型|范围|  
|---------------|-----------|  
|**tinyint**|不支持自动管理|  
|**smallint**|-2^15 (-32,768) 到 2^15-1 (32,767)|  
|**int**|-2^31 (-2,147,483,648) 到 2^31-1 (2,147,483,647)|  
|**bigint**|-2^63 (-9,223,372,036,854,775,808) 到 2^63-1 (9,223,372,036,854,775,807)|  
|**decimal** 和 **numeric**|-10^38+1 到 10^38-1|  
  
> [!NOTE]  
>  要创建一个可在多个表中使用的自动递增数字或者可以从应用程序中调用而不引用任何表的自动递增数字，请参阅[序列号](../../../relational-databases/sequence-numbers/sequence-numbers.md)。  
  
### <a name="merge-replication"></a>合并复制  
 标识范围由发布服务器管理，并由合并代理传播到订阅服务器（在重新发布的层次结构中，范围由根发布服务器和重新发布服务器管理）。 从发布服务器上的池中分配标识值。 在新建发布向导中或通过使用 [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 向发布中添加带有标识列的项目时，请指定以下各项的值：  
  
-   `@identity_range` 参数，它控制初始分配给发布服务器和带有客户端订阅的订阅服务器的标识范围大小。  
  
    > [!NOTE]  
    >  对于运行早期版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的订阅服务器，此参数（而非 `@pub_identity_range` 参数）还控制重新发布的订阅服务器上的标识范围大小。  
  
-   `@pub_identity_range` 参数，它控制分配给带有服务器订阅（重新发布数据所需）的订阅服务器的重新发布标识范围大小。 所有带有服务器订阅的订阅服务器都会接收到重新发布的范围，即使它们实际上并不重新发布数据。  
  
-   `@threshold` 参数，用于确定 [!INCLUDE[ssEW](../../../includes/ssew-md.md)] 或早期版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅何时需要新的标识范围。  
  
 例如，可以将 `@identity_range` 指定为 10000，将 `@pub_identity_range` 指定为 500000。 为运行 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本的发布服务器和所有订阅服务器（包括具有服务器订阅功能的订阅服务器）分配主范围 10000。 还为具有服务器订阅功能的订阅服务器分配主范围 500000，与重新发布订阅服务器同步的订阅服务器可以使用该范围（还必须在重新发布订阅服务器为发布中的项目指定 `@identity_range`、`@pub_identity_range` 和 `@threshold`）。  
  
 每个运行 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本的订阅服务器还接收辅助标识范围。 辅助范围的大小等于主范围的大小。主范围用尽后，可以使用辅助范围，而且合并代理将分配一个新范围给订阅服务器。 新范围将成为辅助范围，而由于订阅服务器使用标识值，处理将继续进行。  
  
  
### <a name="transactional-replication-with-queued-updating-subscriptions"></a>带有排队更新订阅的事务复制  
 标识范围由分发服务器管理，并由分发代理传播到订阅服务器。 标识值从分发服务器上的池中分配。 池的大小根据标识列所使用的数据类型和增量大小而定。 在新建发布向导中或通过使用 [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 向发布中添加带有标识列的项目时，请指定以下各项的值：  
  
-   `@identity_range` 参数，它控制初始分配给所有订阅服务器的标识范围大小。  
  
-   `@pub_identity_range` 参数，它控制分配给发布服务器的标识范围大小。  
  
-   `@threshold` 参数，它用于确定订阅何时需要新的标识范围。  
  
 例如，可以将 `@pub_identity_range`指定为 10000，将 `@identity_range` 指定为 1000（假定订阅服务器上的更新较少），将 `@threshold` 指定为 80%。 在订阅服务器上进行 800 次插入后（即 1000 的 80%），将为订阅服务器分配一个新范围。 在发布服务器上进行 8000 次插入后，将为发布服务器分配一个新范围。 分配新范围后，表中的标识范围值之间将有一定的间隔。 指定较高的阈值会产生较小的间隔，但这会降低系统的容错能力：如果分发代理由于某种原因无法运行，订阅服务器就更容易用完标识。  
  
## <a name="assigning-ranges-for-manual-identity-range-management"></a>为手动标识范围管理分配范围  
 如果指定手动标识范围管理，则必须确保发布服务器和每个订阅服务器使用不同的标识范围。 例如，假设发布服务器上具有标识列定义为 `IDENTITY(1,1)`的表：标识列从 1 开始，在每次插入行时递增 1。 如果发布服务器上的表有 5,000 行，并预计表中的行数在应用程序生存期间会有增长，则发布服务器可使用范围 1-10,000。 假定有两个订阅服务器，订阅服务器 A 可以使用 10,001-20,000，订阅服务器 B 可使用 20,001-30,000。  
  
 使用快照或其他方式初始化订阅服务器后，执行 DBCC CHECKIDENT，为订阅服务器分配其标识范围的起点。 例如，在订阅服务器 A 上，将执行 `DBCC CHECKIDENT('<TableName>','reseed',10001)`。 在订阅服务器 B 上，将执行 `CHECKIDENT('<TableName>','reseed',20001)`。  
  
 若要为发布服务器或订阅服务器分配新范围，请执行 DBCC CHECKIDENT 并指定新值，以对表重设种子。 应有某种方法确定何时必须分配新范围。 例如，应用程序可能有检测何时节点将用尽范围的机制，并可以用 DBCC CHECKIDENT 分配新范围。 您还可以添加检查约束来确保如果添加某行将造成使用超出范围的标识值，则不添加此行。  
  
## <a name="handling-identity-ranges-after-a-database-restore"></a>在数据库还原之后处理标识范围  
 如果正在使用自动标识范围管理，则从备份还原订阅服务器后，它将自动请求标识值的新范围。 如果发布服务器是从备份还原的，则必须确保为发布服务器分配了适当的范围。 对于复制合并，请使用 [sp_restoremergeidentityrange &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-restoremergeidentityrange-transact-sql.md) 分配一个新范围。 对于事务复制，请先确定其使用过的最大值，然后设置新范围的起点。 还原发布数据库后请使用下列过程：  
  
1.  停止所有订阅服务器上的所有活动。  
  
2.  对包含标识列的每个已发布表：  

    1.  在每个订阅服务器的订阅数据库中，执行 `IDENT_CURRENT('<TableName>')`。  
  
    2.  记录在所有订阅服务器中发现的最大值。  
  
    3.  在发布服务器上的发布数据库中，执行 `DBCC CHECKIDENT(<TableName>','reseed',<HighestValueFound+1>`)。  
  
    4.  在发布服务器上的发布数据库中，执行 `sp_adjustpublisheridentityrange <PublicationName>, <TableName>`。  
  
    > [!NOTE]  
    >  如果将标识列中的值设置为减小而非增加，则请记录发现的最小值，然后用此值重设种子。  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../../t-sql/statements/backup-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [IDENT_CURRENT &#40;Transact-SQL&#41;](../../../t-sql/functions/ident-current-transact-sql.md)   
 [IDENTITY（属性）&#40;Transact-SQL&#41;](../../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [sp_adjustpublisheridentityrange &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adjustpublisheridentityrange-transact-sql.md)  
  
  
