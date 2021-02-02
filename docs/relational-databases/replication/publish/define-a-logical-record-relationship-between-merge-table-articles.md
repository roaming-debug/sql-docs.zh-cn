---
title: 定义合并表项目间的逻辑记录关系
description: 了解如何定义用于合并复制项目的相关表之间的逻辑记录关系。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication logical records [SQL Server replication]
- articles [SQL Server replication], logical records
- logical records [SQL Server replication]
ms.assetid: ff847b3a-c6b0-4eaf-b225-2ffc899c5558
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 397ecaba4cf6b8e56f3dbcb6366dca8121ae345d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076785"
---
# <a name="define-a-logical-record-relationship-between-merge-table-articles"></a>定义合并表项目间的逻辑记录关系
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中定义合并表项目间的逻辑记录关系。  
  
 通过使用合并复制，您可以定义不同表中的相关行之间的关系。 然后就可以在同步过程中将这些行作为一个事务单元进行处理。 无论两个项目之间是否存在联接筛选器关系，都可以在这两个项目之间定义逻辑记录。 有关详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
-   **定义合并表项目间的逻辑记录关系，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果在初始化对发布的订阅后添加、修改或删除逻辑记录，必须在更改后生成新的快照并重新初始化所有订阅。 有关属性更改要求的详细信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在“添加联接”对话框（在新建发布向导和“发布属性 - \<Publication>”对话框中可用）中定义逻辑记录。  有关如何使用该向导和如何访问该对话框的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)和[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
 仅当逻辑记录应用于合并发布中的联接筛选器且发布遵循使用预计算分区的要求时，才可以在 **“添加联接”** 对话框中定义逻辑记录。 若要定义不应用于联接筛选器的逻辑记录并在逻辑记录级设置冲突检测和解决方法，必须使用存储过程。  
  
#### <a name="to-define-a-logical-record-relationship"></a>定义逻辑记录关系  
  
1.  在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”对话框的“筛选行”页上，在“筛选的表”窗格中选择行筛选器。     
  
     逻辑记录关系与扩展行筛选器的联接筛选器相关联。 因此，必须定义一个行筛选器，才能用联接来扩展该筛选器并应用逻辑记录关系。 定义一个联接筛选器后，可使用其他联接筛选器来扩展此联接筛选器。 有关定义联接筛选器的详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
2.  单击 **“添加”** ，再单击 **“添加联接以扩展所选筛选器”** 。  
  
3.  在 **“添加联接”** 对话框中定义一个联接筛选器，然后选中 **“逻辑记录”** 复选框。  
  
4.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框。   
  
#### <a name="to-delete-a-logical-record-relationship"></a>删除逻辑记录关系  
  
-   只删除逻辑记录关系，或者删除逻辑记录关系及其相关联的联接筛选器。  
  
     只删除逻辑记录关系：  
  
    1.  在新建发布向导的“筛选行”页或“发布属性 - \<Publication>”对话框的“筛选行”页上，在“筛选的表”窗格中选择与逻辑记录关系关联的联接筛选器，然后单击“编辑”。      
  
    2.  在 **“编辑联接”** 对话框中，清除 **“逻辑记录”** 复选框。  
  
    3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
     删除逻辑记录关系及其相关联的联接筛选器：  
  
    -   在新建发布向导或“发布属性 - \<Publication>”对话框的“筛选行”页上，在“筛选的表”窗格中选择筛选器，然后单击“删除”。    如果删除的联接筛选器自身是由其他联接扩展而成的，则也将删除那些联接。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 您可以使用复制存储过程以编程方式指定项目之间的逻辑记录关系。  
  
#### <a name="to-define-a-logical-record-relationship-without-an-associated-join-filter"></a>在没有关联的联接筛选器的情况下定义逻辑记录关系  
  
1.  如果发布包含已筛选的任何项目，则执行 [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)，并注意结果集中的 **use_partition_groups** 值。  
  
    -   如果值为 **1**，则已使用预计算分区。  
  
    -   如果值为 **0**，请在发布服务器上对发布数据库执行 [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) 。 将 \@property 的值指定为 use_partition_groups，并将 \@value 的值指定为 true   。  
  
        > [!NOTE]  
        >  如果发布不支持预计算分区，则无法使用逻辑记录。 有关详细信息，请参阅[使用预计算分区优化参数化筛选器性能](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)主题中的“使用预计算分区的要求”。  
  
    -   如果值为 NULL，则需要运行快照代理以生成发布的初始快照。  
  
2.  如果将包含逻辑记录的项目不存在，请在发布服务器上对发布数据库执行 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 。 为逻辑记录指定以下冲突检测和解决选项中的一项：  
  
    -   若要检测并解决发生在逻辑记录的相关行之间的冲突，请将 \@logical_record_level_conflict_detection 和 \@logical_record_level_conflict_resolution 的值指定为 true  。  
  
    -   若要使用标准行级或列级冲突检测和解决方法，请将 \@logical_record_level_conflict_detection 和 \@logical_record_level_conflict_resolution 的值指定为 false，此为默认值  。  
  
3.  为每个将包含逻辑记录的项目重复步骤 2。 您必须为逻辑记录中的每个项目使用相同的冲突检测和解决选项。 有关详细信息，请参阅 [检测并解决逻辑记录中的冲突](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md)。  
  
4.  在发布服务器上，对发布数据库执行 [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)。 指定 \@publication，将 \@article 指定为该关系中一个项目的名称，将 \@join_articlename 指定为第二个项目的名称，将 \@filtername 指定为该关系的名称，将 \@join_filterclause 指定为定义两个项目之间关系的子句，将 \@join_unique_key 指定为联接的类型，并将 \@filter_type 指定为以下值之一      ：  
  
    -   **2** - 定义逻辑关系。  
  
    -   **3** - 定义与联接筛选器的逻辑关系。  
  
    > [!NOTE]  
    >  如果未使用联接筛选器，则两个项目之间的关系方向并不重要。  
  
5.  为发布中剩余的每个逻辑记录关系重复步骤 2。  
  
#### <a name="to-change-conflict-detection-and-resolution-for-logical-records"></a>为逻辑记录更改冲突检测和解决方法  
  
1.  检测和解决发生在逻辑记录中相关行之间的冲突：  
  
    -   在发布服务器上，对发布数据库执行 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。 将 \@property 的值指定为 logical_record_level_conflict_detection，并将 \@value 的值指定为 true   。 将 \@force_invalidate_snapshot 和 \@force_reinit_subscription 的值指定为 1  。  
  
    -   在发布服务器上，对发布数据库执行 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。 将 \@property 的值指定为 logical_record_level_conflict_resolution，并将 \@value 的值指定为 true   。 将 \@force_invalidate_snapshot 和 \@force_reinit_subscription 的值指定为 1  。  
  
2.  使用标准行级或列级冲突检测和解决方法：  
  
    -   在发布服务器上，对发布数据库执行 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。 将 \@property 的值指定为 logical_record_level_conflict_detection，并将 \@value 的值指定为 false   。 将 \@force_invalidate_snapshot 和 \@force_reinit_subscription 的值指定为 1  。  
  
    -   在发布服务器上，对发布数据库执行 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。 将 \@property 的值指定为 logical_record_level_conflict_resolution，并将 \@value 的值指定为 false   。 将 \@force_invalidate_snapshot 和 \@force_reinit_subscription 的值指定为 1  。  
  
#### <a name="to-remove-a-logical-record-relationship"></a>删除逻辑记录关系  
  
1.  在发布服务器上，对发布数据库执行以下查询，以返回有关为指定的发布定义的所有逻辑记录关系的信息：  
  
     [!code-sql[HowTo#sp_ReturnMergeLogicalRecords](../../../relational-databases/replication/codesnippet/tsql/define-a-logical-record-_1.sql)]  
  
     注意在结果集 **filtername** 列中的被删除逻辑记录关系的名称。  
  
    > [!NOTE]  
    >  该查询返回的信息与 [sp_helpmergefilter](../../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)相同；然而，该系统存储过程仅返回有关逻辑记录关系（也是联接筛选器）的信息。  
  
2.  在发布服务器上，对发布数据库执行 [sp_dropmergefilter](../../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)。 指定 \@publication，为 \@article 指定该关系中其中一个项目的名称，并为 \@filtername 指定步骤 1 中关系的名称  。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例对现有发布启用预计算分区，并创建包含 `SalesOrderHeader` 和 `SalesOrderDetail` 表的两个新项目的逻辑记录。  
  
 [!code-sql[HowTo#sp_AddMergeLogicalRecord](../../../relational-databases/replication/codesnippet/tsql/define-a-logical-record-_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
  
> [!NOTE]  
>  利用合并复制，您可以指定在逻辑记录级跟踪和解决冲突，但使用 RMO 却无法设置这些选项。  
  
#### <a name="to-define-a-logical-record-relationship-without-an-associated-join-filter"></a>在没有关联的联接筛选器的情况下定义逻辑记录关系  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例，为发布设置 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为在步骤 1 中创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  如果 <xref:Microsoft.SqlServer.Replication.MergePublication.PartitionGroupsOption%2A> 属性设置为 <xref:Microsoft.SqlServer.Replication.PartitionGroupsOption.False>，请将其设为 <xref:Microsoft.SqlServer.Replication.PartitionGroupsOption.True>。  
  
5.  如果要构成逻辑记录的项目不存在，请创建 <xref:Microsoft.SqlServer.Replication.MergeArticle> 类的一个实例，然后设置以下属性：  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Article.Name%2A>设置为项目的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>设置为发布的名称。  
  
    -   （可选）如果对项目进行水平筛选，则将 <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> 属性指定为行筛选器子句。 使用此属性可指定静态行筛选器或参数化行筛选器。 有关详细信息，请参阅 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
     有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.Article.Create%2A> 方法。  
  
7.  对构成逻辑记录的每个项目，重复执行步骤 5 和 6。  
  
8.  创建 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> 类的一个实例以定义项目之间的逻辑记录关系。 然后，设置以下属性：  
  
    -   将 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.ArticleName%2A> 属性设置为逻辑记录关系中子项目的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.JoinArticleName%2A> 属性设置为逻辑记录关系中现有父项目的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.FilterName%2A> 属性设置为逻辑记录关系的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.JoinFilterClause%2A> 属性设置为定义关系的表达式。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.FilterTypes.LogicalRecordLink> 属性的值设置为 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.FilterTypes%2A> 。 如果此逻辑记录关系同时也是一个联接筛选器，请将此属性的值指定为 <xref:Microsoft.SqlServer.Replication.FilterTypes.JoinFilterAndLogicalRecordLink> 。 有关详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
9. 对表示此关系中的子项目的对象调用 <xref:Microsoft.SqlServer.Replication.MergeArticle.AddMergeJoinFilter%2A> 方法。 传递步骤 8 中的 <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> 对象以定义此关系。  
  
10. 对发布中的其余每个逻辑记录关系重复执行步骤 8 和 9。  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 此示例为 `SalesOrderHeader` 和 `SalesOrderDetail` 表创建一个由两个新项目构成的逻辑记录。  
  
 [!code-cs[HowTo#rmo_CreateLogicalRecord](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createlogicalrecord)]  
  
 [!code-vb[HowTo#rmo_vb_CreateLogicalRecord](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createlogicalrecord)]  
  
## <a name="see-also"></a>另请参阅  
 [定义和修改合并项目间的联接筛选器](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [定义和修改静态行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [通过逻辑记录对相关行的更改进行分组](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)   
 [使用预计算分区优化参数化筛选器的性能](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)   
 [通过逻辑记录对相关行的更改进行分组](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)  
  
  
