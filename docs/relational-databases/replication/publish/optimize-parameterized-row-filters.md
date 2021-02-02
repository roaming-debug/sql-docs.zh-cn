---
description: 优化参数化行筛选器
title: 优化参数化行筛选器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- precomputed partitions [SQL Server replication]
- filters [SQL Server replication], parameterized
- merge replication precomputed partitions [SQL Server replication], SQL Server Management Studio
- parameterized filters [SQL Server replication], optimizing
ms.assetid: 49349605-ebd0-4757-95be-c0447f30ba13
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c8b58c7cf7ffe2b56feb8f59df9b0adef857ab02
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076655"
---
# <a name="optimize-parameterized-row-filters"></a>优化参数化行筛选器
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中优化参数化行筛选器。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
-   **优化参数化行筛选器，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   当使用参数化筛选器时，可通过在创建发布时指定 **use partition groups** 或 **keep partition changes** 选项来控制合并复制处理筛选器的方式。 通过将其他元数据存储在发布数据库中，上述选项可提高具有已筛选项目的发布的同步性能。 通过在创建项目时设置 **partition options** ，您可以控制在订阅服务器之间共享数据的方式。 有关这些要求的详细信息，请参阅 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
     使用 [!INCLUDE[ssEW](../../../includes/ssew-md.md)]SQL Server Compact 订阅服务器时，keep_partition_changes 必须设置为 true 以确保正确传播删除操作。 设置为 false 时，订阅服务器可能有比预期更多的行。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 以下设置可用于优化参数化行筛选器：  
  
 **Partition Options**  
 可在“项目属性 - \<Article>”对话框的“属性”页，或在“添加筛选器”对话框中设置此选项。   两个对话框都可在新建发布向导和“发布属性 - \<Publication>”对话框中获得。 通过“项目属性 - \<Article>”对话框可以为此选项指定在“添加筛选器”对话框中不可用的其他值。   
  
 **预计算分区**  
 如果发布中的项目符合一组要求，则此选项在默认情况下将设置为 **True** 。 有关这些要求的详细信息，请参阅[使用预计算分区优化参数化筛选器性能](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。 可在“发布属性 - \<Publication>”对话框的“订阅选项”页上修改此选项。   
  
 **优化同步**  
 仅当 **“预计算分区”** 设置为 **False** 时此选项才应设置为 **True**。 可在“发布属性 - \<Publication>”对话框的“订阅选项”页上设置此选项。   
  
 有关如何使用新建发布向导和如何访问“发布属性 - \<Publication>”对话框的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)和[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-set-partition-options-in-the-add-filter-or-edit-filter-dialog-box"></a>在“添加筛选器”或“编辑筛选器”对话框中设置分区选项。  
  
1.  在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”对话框的“筛选行”页上，单击“添加”，然后单击“添加筛选器”。      
  
2.  创建参数化筛选器。 有关详细信息，请参阅 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
3.  选择指示订阅服务器之间如何共享数据的选项：  
  
    -   **此表中的行将转到多个订阅**  
  
    -   **此表中的行将仅转到一个订阅**  
  
     如果选择 **“此表中的行将仅转到一个订阅”** ，则合并复制可以通过存储和处理较少的元数据来优化性能。 但是，必须确保在对数据分区时不能将行复制到多个订阅服务器。 有关详细信息，请参阅主题 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“设置‘分区选项’”部分。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框 。  
  
#### <a name="to-set-partition-options-in-the-article-properties---article-dialog-box"></a>在“项目属性 - \<Article>”对话框中设置分区选项  
  
1.  在新建发布向导或“发布属性 - \<Publication>”对话框的“项目”页上，选择一个表，然后单击“项目属性”。    
  
2.  单击 **“设置突出显示的表项目的属性”** 或 **“设置所有表项目的属性”** 。  
  
3.  在“项目属性 - \<Article>”对话框的“属性”选项卡的“目标对象”部分中，为“分区选项”指定以下值之一：     
  
    -   **重叠**  
  
    -   **重叠，不允许分区外的数据更改**  
  
    -   **不重叠，一个订阅**  
  
    -   **不重叠，由所有订阅共享**  
  
     有关这些选项以及它们与 **“添加筛选器”** 和 **“编辑筛选器”** 对话框中选项的关系的详细信息，请参阅 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)的“设置‘分区选项’”部分。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框 。  
  
#### <a name="to-set-precompute-partitions"></a>设置预计算分区  
  
1.  在“发布属性 - \<Publication>”对话框的“订阅选项”页上，为“预计算分区”选项选择值。   在以下情况下，则此属性为只读：  
  
    -   发布不满足对预计算分区的要求。  
  
    -   尚未为发布生成快照。 这种情况下，该选项会显示 **“创建快照时自动设置”** 值。  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-set-optimize-synchronization"></a>设置优化同步  
  
1.  在“发布属性 - \<Publication>”对话框的“订阅选项”页上，为“优化同步”选项选择值 `True`  。  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 有关 `@keep_partition_changes` 和 `@use_partition_groups` 的筛选选项的定义，请参阅 [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) 中优化参数化行筛选器。  
  
#### <a name="to-specify-merge-filter-optimizations-when-creating-a-new-publication"></a>创建新发布时指定合并筛选器的优化  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)。 指定 `@publication`，并将值 `true` 指定给以下参数之一：  
  
    -   `@use_partition_groups`：最高级别的性能优化（如果项目符合预计算分区的要求）。 有关详细信息，请参阅[使用预计算分区优化参数化筛选器性能](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。  
  
    -   `@keep_partition_changes` - 如果无法使用预计算分区，请使用此优化。  
  
2.  为发布添加一个快照作业。 有关详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
3.  在发布服务器上，对发布数据库执行 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)，并指定以下参数：  
  
    -   `@publication` - 步骤 1 中发布的名称。  
  
    -   `@article` - 项目的名称  
  
    -   `@source_object` - 发布的数据库对象。  
  
    -   `@subset_filterclause` - 用于以水平方式筛选项目的参数化筛选器子句（可选）。  
  
    -   `@partition_options` - 已过滤项目的分区选项。  
  
4.  对发布中的每个项目重复步骤 3。  
  
5.  （可选）在发布服务器上，对发布数据库执行 [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) 以在两个项目之间定义一个联接筛选器。 有关详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
#### <a name="to-view-and-modify-merge-filter-behaviors-for-an-existing-publication"></a>查看和修改现有发布的合并筛选器行为  
  
1.  （可选）在发布服务器上，对发布数据库执行 [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)，并指定 `@publication` 中优化参数化行筛选器。 请记录结果集中 `keep_partition_changes` 和 `use_partition_groups` 的值。  
  
2.  （可选）在发布服务器上，对发布数据库执行 [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)。 为 `@property` 指定值 `use_partition_groups`，为 `@value` 指定值 `true` 或 `false`。  
  
3.  （可选）在发布服务器上，对发布数据库执行 [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)。 为 `@property` 指定值 `keep_partition_changes`，为 `@value` 指定值 `true` 或 `false`。  
  
    > [!NOTE]  
    >  启用 `keep_partition_changes` 时，必须首先禁用 `use_partition_groups` 并为 `@force_reinit_subscription` 指定值 `1`。  
  
4.  （可选）在发布服务器上，对发布数据库执行 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。 为 `@property` 指定值 `partition_options`，并为 `@value` 指定适当的值。 有关以上筛选选项的定义，请参阅 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 。  
  
5.  （可选）启动快照代理以重新生成快照（如果需要）。 有关哪些更改需要生成新快照的信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
## <a name="see-also"></a>另请参阅  
 [在合并项目之间自动生成一组联接筛选器 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md)   
 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
