---
title: 管理参数化筛选器分区（合并）
description: 使用用于 SQL Server 合并复制的参数化筛选器管理分区。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- partitions [SQL Server replication]
- merge replication partitions [SQL Server replication], SQL Server Management Studio
- parameterized filters [SQL Server replication], partition management
ms.assetid: fb5566fe-58c5-48f7-8464-814ea78e6221
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0b24e2e404dbea75d77cdfc8acdfa50189db2286
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765722"
---
# <a name="manage-partitions-for-a-merge-publication-with-parameterized-filters"></a>通过参数化筛选器为合并发布管理分区
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中通过参数化筛选器为合并发布管理分区。 参数化行筛选器可用于生成不重叠的分区。 可以限制这些分区，以便只有一个订阅接收到给定分区。 在这类情况下，大量订阅服务器将导致大量的分区，从而需要同等数量的分区快照。 有关详细信息，请参阅 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
-   **通过参数化筛选器为合并发布管理分区，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   如果对复制拓扑编写脚本（建议这样做），则发布脚本包含用于创建数据分区的存储过程调用。 该脚本提供了对所创建的分区的引用和一种在必要时重建分区的途径。 有关详细信息，请参阅 [Scripting Replication](../../../relational-databases/replication/scripting-replication.md)。  
  
-   如果发布具有的参数化筛选器可生成带有非重叠分区的订阅，并且如果特定订阅丢失并需要重新创建，则您必须执行以下操作：删除曾订阅的分区，重新创建订阅，然后重新创建该分区。 有关详细信息，请参阅 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。 生成发布创建脚本时，复制会为现有订阅服务器分区生成创建脚本。 有关详细信息，请参阅 [Scripting Replication](../../../relational-databases/replication/scripting-replication.md)。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在“发布属性 - \<Publication>”对话框的“数据分区”页面上管理分区 。 有关访问此对话框的详细信息，请参阅 [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。 在此页上，可以执行下列操作：创建和删除分区、允许订阅服务器启动快照的生成和传递、生成一个或多个分区的快照和清除快照。  
  
#### <a name="to-create-a-partition"></a>创建分区  
  
1.  在“发布属性 - \<Publication>”对话框的“数据分区”页面上，单击“添加”  。  
  
2.  在 **“添加数据分区”** 对话框中，输入与要创建的分区相关联的 **HOST_NAME()** 和/或 **SUSER_SNAME()** 的值。  
  
3.  还可以指定快照刷新计划：  
  
    1.  选择 **“安排此分区的快照代理在以下时间运行”**  
  
    2.  接受默认的快照刷新计划，或者单击 **“更改”** 以指定其他计划。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-delete-a-partition"></a>删除分区  
  
1.  在 **“数据分区”** 页上，在网格中选择分区。  
  
2.  单击 **“删除”** 。  
  
#### <a name="to-allow-subscribers-to-initiate-snapshot-generation-and-delivery"></a>允许订阅服务器启动快照的生成和传递  
  
1.  在 **“数据分区”** 页上，选择 **“在新订阅服务器尝试同步时，根据需要自动定义分区并生成快照”** 。  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-generate-a-snapshot-for-a-partition"></a>生成分区快照  
  
1.  在 **“数据分区”** 页上，在网格中选择分区。  
  
2.  单击 **“立即生成所选快照”**  
  
#### <a name="to-clean-up-a-snapshot-for-a-partition"></a>清除分区快照  
  
1.  在 **“数据分区”** 页上，在网格中选择分区。  
  
2.  单击 **“清除现有快照”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 为了更好地管理带有参数化筛选器的发布，可使用复制存储过程以编程方式枚举现有的分区。 还可以创建和删除现有的分区。 可以获取以下有关现有分区的信息：  
  
-   如何筛选分区（使用 [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md) 或 [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md)）。  
  
-   用于生成分区快照的作业的名称。  
  
-   分区快照作业上次运行的时间。  
  
 尽管在初始化新订阅时可以按需生成由两部分构成的快照的第二部分，但通过下面的过程可以控制此快照的生成方式，并在最方便的时候预生成该快照。 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
#### <a name="to-view-information-on-existing-partitions"></a>查看有关现有分区的信息  
  
1.  在发布服务器上，对发布数据库执行 [sp_helpmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md)。 为 `@publication` 指定发布名称。 （可选）指定 `@suser_sname` 或 `@host_name` 以仅根据单个筛选条件返回信息。  
  
#### <a name="to-define-a-new-partition-and-generate-a-new-partitioned-snapshot"></a>定义新分区并生成新分区快照  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)。 为 `@publication` 指定发布名称，并为以下某个参数指定用于定义分区的参数化值：  
  
    -   `@suser_sname` - 当参数化筛选器由 [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md) 返回的值进行定义时。  
  
    -   `@host_name` - 当参数化筛选器由 [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md) 返回的值进行定义时。  
  
2.  创建并初始化此新分区的参数化快照。 有关详细信息，请参阅 [为包含参数化筛选器的合并发布创建快照](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
#### <a name="to-delete-a-partition"></a>删除分区  
  
1.  在发布服务器上，对发布数据库执行 [sp_dropmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md)。 为 `@publication` 指定发布的名称，并为以下某个参数指定用于定义分区的参数化值：  
  
    -   `@suser_sname` - 当参数化筛选器由 [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md) 返回的值进行定义时。  
  
    -   `@host_name` - 当参数化筛选器由 [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md) 返回的值进行定义时。  
  
     此操作还将删除快照作业以及该分区的任何快照文件。  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 若要更好地管理具有参数化筛选器的发布，可以通过使用复制管理对象 (RMO) 以编程方式创建新的订阅服务器分区，枚举现有的订阅服务器分区以及删除订阅服务器分区。 有关如何创建订阅服务器分区的信息，请参阅 [为包含参数化筛选器的合并发布创建快照](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。 可以获得有关现有分区的以下信息：  
  
-   分区所基于的值和筛选函数。  
  
-   为订阅服务器生成参数化快照的作业的名称。  
  
-   参数化快照作业上次运行的时间。  
  
#### <a name="to-view-information-on-existing-partitions"></a>查看有关现有分区的信息  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例。 设置发布的 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergePartitions%2A> 方法，然后将结果传递至 <xref:Microsoft.SqlServer.Replication.MergePartition> 对象数组。  
  
5.  对数组中的每个 <xref:Microsoft.SqlServer.Replication.MergePartition> 对象，获取感兴趣的任何属性。  
  
#### <a name="to-delete-existing-partitions"></a>删除现有分区  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例。 设置发布的 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergePartitions%2A> 方法，然后将结果传递至 <xref:Microsoft.SqlServer.Replication.MergePartition> 对象数组。  
  
5.  对数组中的每个 <xref:Microsoft.SqlServer.Replication.MergePartition> 对象，确定是否应删除分区。 此决定通常基于 <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterLogin%2A> 属性的值或 <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterHostName%2A> 属性的值。  
  
6.  在步骤 2 中的 <xref:Microsoft.SqlServer.Replication.MergePublication.RemoveMergePartition%2A> 对象上调用 <xref:Microsoft.SqlServer.Replication.MergePublication> 方法。 传递步骤 5 中的 <xref:Microsoft.SqlServer.Replication.MergePartition> 对象。  
  
7.  对已删除的每个分区重复步骤 6。  
  
## <a name="see-also"></a>另请参阅  
 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  
  
  
  
