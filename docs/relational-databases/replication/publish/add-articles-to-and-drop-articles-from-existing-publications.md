---
title: 添加或删除项目（现有发布）
description: 了解如何向 SQL Server 的现有发布添加项目和从中删除项目。
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- articles [SQL Server replication], dropping
- deleting articles
- removing articles
- dropping articles
- adding articles
- administering replication, articles
- publications [SQL Server replication], adding and dropping articles
- articles [SQL Server replication], adding
ms.assetid: b148e907-e1f2-483b-bdb2-59ea596efceb
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 7353735a34874248e3796763c608bff24a83f649
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907365"
---
# <a name="add-articles-to-and-drop-articles-from-existing-publications"></a>向现有发布添加项目和从中删除项目
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  在创建发布后，可以添加和删除项目。 可以随时添加项目，但删除项目所需的操作取决于复制的类型和删除项目的时间。  
  
## <a name="adding-articles"></a>添加项目  
 添加项目涉及的操作有：将项目添加到发布、为发布创建新的快照、同步订阅以应用新项目的架构和数据。  
  
> [!NOTE]
>  如果向合并发布中添加一个项目和一个依赖于此新项目的现有项目，则必须使用 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 和 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) 的 \@processing_order 参数指定两个项目的处理顺序。 请考虑以下情况：您要发布一个表，但不发布该表引用的函数。 如果不发布该函数，则无法在订阅服务器中创建相应的表。 将此函数添加到发布时：为 sp_addmergearticle 的 \@processing_order 参数指定值 1；为 sp_changemergearticle 的 \@processing_order 参数指定值 2，为参数 \@article 指定表名称。 此处理顺序可确保在创建依赖于某函数的表之前在订阅服务器上创建该函数。 每个项目可以使用不同的数字，只要函数的数字小于表的数字即可。  
  
1.  用以下方法之一添加一个或多个项目：  
  
    -   [在发布中添加和删除项目 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md)  
  
    -   [定义项目](../../../relational-databases/replication/publish/define-an-article.md)  
  
2.  将项目添加到发布后，必须为发布（和所有分区，如果发布是带有参数化筛选器的合并发布）创建新的快照。 然后，分发代理或合并代理将新项目的架构和数据复制到订阅服务器（并不重新初始化整个发布）。  
  
    -   若要创建新快照，请参阅 [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
    -   若要为包含参数化筛选器的合并发布创建新快照，请参阅[为包含参数化筛选器的合并发布创建快照](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
3.  创建快照后，同步订阅以复制新项目的架构和数据。  

    -   若要同步推送订阅，请参阅 [Synchronize a Push Subscription](../../../relational-databases/replication/synchronize-a-push-subscription.md)。  
  
    -   若要同步请求订阅，请参阅 [Synchronize a Pull Subscription](../../../relational-databases/replication/synchronize-a-pull-subscription.md)。  
  
## <a name="dropping-articles"></a>删除项目  
 可以随时从发布中删除项目，但必须考虑以下行为：  
  
-   从发布中删除项目并不会将对象从发布数据库中删除，也不会将相应对象从订阅数据库中删除。 必要时，使用 DROP \<Object> 删除这些对象。 在删除通过外键约束与其他已发布项目相关的项目时，建议手动或使用按需脚本执行在订阅服务器上删除表：指定包含相应 DROP \<Object> 语句的脚本。 有关详细信息，请参阅[在同步期间执行脚本（复制 Transact-SQL 编程）](../../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md)。  
  
-   对于兼容级别为 90RTM 或更高的合并发布，可以随时删除项目，但需要创建一个新的快照。 此外：  
  
    -   如果项目在联接筛选器或逻辑记录关系中是父项目，就必须先删除关系，这需要重新初始化。  
  
    -   如果项目具有发布中的最后一个参数化筛选器，就必须重新初始化订阅。  
  
-   对于兼容级别低于 90RTM 的合并发布，可以在初始同步订阅之前删除项目，而无需特别考虑。 如果在同步一个或多个订阅之后删除项目，则必须删除、重新创建和同步订阅。  
  
-   对于快照发布或事务发布，可以在创建订阅之前删除项目，而无需特别考虑。 如果在创建一个或多个订阅之后删除项目，则必须删除、重新创建和同步订阅。 有关删除订阅的详细信息，请参阅[订阅发布](../../../relational-databases/replication/subscribe-to-publications.md)和 [sp_dropsubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)。 使用 **sp_dropsubscription** 可以删除订阅中的单个项目，而不是删除整个订阅。  
  
1.  从发布中删除项目涉及的操作有：删除项目并为发布创建新的快照。 删除项目会使当前快照失效，因此必须创建新的快照。  
  
    -   若要从发布中删除项目，请参阅[在发布中添加和删除项目 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md) 或[删除项目](../../../relational-databases/replication/publish/delete-an-article.md)。  
  
2.  从发布中删除项目后，必须为发布（和所有分区，如果发布是带有参数化筛选器的合并发布）创建新的快照。  
  
    -   若要创建新快照，请参阅 [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
    -   若要为包含参数化筛选器的合并发布创建新快照，请参阅[为包含参数化筛选器的合并发布创建快照](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
 如上所述，在某些情况下删除项目需要删除、重新创建及同步订阅。 有关详细信息，请参阅[订阅发布](../../../relational-databases/replication/subscribe-to-publications.md)和[同步数据](../../../relational-databases/replication/synchronize-data.md)。  
 
 > [!NOTE]
 > [!INCLUDE[ssSQL15](../../../includes/sssql14-md.md)] Service Pack 2（或更高版本）和 [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)] Service Pack 1（或更高版本）支持针对参与事务复制的项目使用 DROP TABLE DDL 命令删除表  。 如果发布支持 DROP TABLE DDL，则 DROP TABLE 操作将从发布和数据库中删除表。 日志读取器代理将会针对已删除的表分发数据库发布清理命令，并针对发布服务器元数据执行清理。 如果日志读取器尚未处理引用已删除表的所有日志记录，则会忽略与已删除表相关联的新命令。 已处理的记录将传送到分发数据库。 如果在日志读取器清理废弃（已删除）项目之前分发代理将处理这些记录，则可能会在订阅服务器数据库上应用这些记录。 所有事务复制发布的默认设置是不支持 DROP TABLE DDL。 [KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactional-replication-in-sql-server-2014-or-in-sql-server-2016-sp1) 提供了有关此项改进的更多详细信息。

  
## <a name="see-also"></a>另请参阅  
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [重新初始化订阅](../../../relational-databases/replication/reinitialize-subscriptions.md)   
 [对发布数据库进行架构更改](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
