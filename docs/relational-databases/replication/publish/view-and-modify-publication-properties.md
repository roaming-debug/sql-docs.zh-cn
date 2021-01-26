---
title: 查看和修改发布属性 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或复制管理对象在 SQL Server 中查看和修改发布属性。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing replication properties
- modifying replication properties, articles
- articles [SQL Server replication], modifying
- publications [SQL Server replication], properties
- articles [SQL Server replication], properties
- modifying replication properties, publications
- publications [SQL Server replication], modifying
ms.assetid: 27d72ea4-bcb6-48f2-b4aa-eb1410da7efc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: dfb58f47996df608218e867fda4225f8cb36a7e6
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766463"
---
# <a name="view-and-modify-publication-properties"></a>查看和修改发布属性
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中查看和修改发布属性。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
-   **查看和修改发布属性，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   创建复制后，有些属性便不可以再进行修改，如果该发布存在订阅，则其他属性也无法再进行修改。 不能进行修改的属性将显示为只读。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   创建发布之后，某些属性更改要求新的快照。 如果发布具有多个订阅，某些更改还会要求重新初始化所有订阅。 有关详细信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)和[向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在“发布属性 - \<Publication>”对话框（在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 和复制监视器中可用）中查看和修改发布属性。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
 “发布属性 - \<Publication>”对话框中包括以下页面：  
  
-   **“常规”** 页，包含发布名称和说明、数据库名称、发布类型以及订阅过期设置。  
  
-   **“项目”** 页，对应于新建发布向导中的 **“项目”** 页。 使用此页可添加和删除项目，以及更改项目的属性和列筛选。  
  
-   **“筛选行”** 页，对应于新建发布向导中的 **“筛选表行”** 页。 使用此页可添加、编辑和删除所有发布类型的静态行筛选器，以及添加、编辑和删除合并发布的参数化行筛选器和联接筛选器。  
  
-   **“快照”** 页，使您可以指定快照的格式和位置、快照是否压缩，以及应用快照前后要运行的脚本。  
  
-   **“FTP 快照”** 页（适用于快照和事务发布，以及运行 SQL Server 2005 之前版本的发布服务器的合并发布），使您可以指定订阅服务器是否可以通过文件传输协议 (FTP) 下载快照文件。  
  
-   **“FTP 快照和 Internet”** 页（适用于运行 SQL Server 2005 或更高版本的发布服务器的合并发布），使您可以指定订阅服务器是否可以通过 FTP 下载快照文件，以及订阅服务器是否可以通过 HTTPS 对订阅进行同步。  
  
-   **“订阅选项”** 页，使您可以设置多个应用于所有订阅的选项。 这些选项会随着发布类型而有所不同。  
  
-   **“发布访问列表”** 页，使您可以指定可以访问发布的登录名和组。  
  
-   **“代理安全性”** 页，使您可以访问用于运行下列代理并连接复制拓扑中计算机的帐户设置：所有发布的快照代理、所有事务发布的日志读取器代理以及允许排队更新订阅的事务发布的队列读取器代理。  
  
-   **“数据分区”** 页（适用于来自运行 SQL Server 2005 或更高版本的发布服务器的合并发布），使您可以指定在快照不可用时使用参数化筛选器的发布的订阅服务器是否可以请求快照。 它还允许一次性或按循环计划生成一个或多个分区的快照。  
  
#### <a name="to-view-and-modify-publication-properties-in-management-studio"></a>在 Management Studio 中查看和修改发布属性  
  
1.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
3.  右键单击发布，然后单击 **“属性”** 。  
  
4.  根据需要修改属性，然后单击 **“确定”** 。  

#### <a name="to-view-and-modify-publication-properties-in-replication-monitor"></a>在复制监视器中查看和修改发布属性  
  
1.  在复制监视器的左侧窗格中展开发布服务器组，然后展开一个发布服务器。  
  
2.  右键单击发布，然后单击 **“属性”** 。  
  
3.  根据需要修改属性，然后单击 **“确定”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式修改发布以及返回其属性。 您使用的存储过程取决于发布的类型。  
  
#### <a name="to-view-the-properties-of-a-snapshot-or-transactional-publication"></a>查看快照发布或事务发布的属性  
  
1.  执行 [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)，为 \@publication 参数指定该发布的名称。 如果未指定此参数，则会返回发布服务器上所有发布的相关信息。  
  
#### <a name="to-change-the-properties-of-a-snapshot-or-transactional-publication"></a>更改快照发布或事务发布的属性  
  
1.  执行 [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)，在 \@property 参数中指定要更改的发布属性，并在 \@value 参数中指定此属性的新值 。  
  
    > [!NOTE]  
    >  如果该更改将要求生成新快照，则还必须将 \@force_invalidate_snapshot 的值指定为 1，而如果该更改将要求重新初始化订阅服务器，则必须将 \@force_reinit_subscription 的值指定为 1   。 有关更改时需要新快照或重新初始化的属性的详细信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
#### <a name="to-view-the-properties-of-a-merge-publication"></a>查看合并发布的属性  
  
1.  执行 [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)，为 \@publication 参数指定该发布的名称。 如果未指定此参数，则会返回发布服务器上所有发布的相关信息。  
  
#### <a name="to-change-the-properties-of-a-merge-publication"></a>更改合并发布的属性  
  
1.  执行 [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)，在 \@property 参数中指定要更改的发布属性，并在 \@value 参数中指定此属性的新值 。  
  
    > [!NOTE]  
    >  如果该更改将要求生成新快照，则还必须将 \@force_invalidate_snapshot 的值指定为 1，而如果该更改将要求重新初始化订阅服务器，则必须将 \@force_reinit_subscription 的值指定为 1。有关那些在更改后要求生成新快照或重新初始化的属性的详细信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   。  
  
#### <a name="to-view-the-properties-of-a-snapshot"></a>查看快照的属性  
  
1.  执行 [sp_helppublication_snapshot](../../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md)，为 \@publication 参数指定该发布的名称。  
  
#### <a name="to-change-the-properties-of-a-snapshot"></a>更改快照的属性  
  
1.  执行 [sp_changepublication_snapshot](../../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)，为相应的快照参数指定一个或多个新的快照属性。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此事务复制示例将返回该发布的属性。  
  
 [!code-sql[HowTo#sp_helppublication](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-publicat_1.sql)]  
  
 此事务复制示例将为发布禁用架构复制。  
  
 [!code-sql[HowTo#sp_changepublication](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-publicat_2.sql)]  
  
 此合并复制示例将返回该发布的属性。  
  
 [!code-sql[HowTo#sp_helpmergepublication](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-publicat_3.sql)]  
  
 此合并复制示例将为发布禁用架构复制。  
  
 [!code-sql[HowTo#sp_changemergepublication](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-publicat_4.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 可以使用复制管理对象 (RMO) 以编程方式修改发布以及访问其属性。 查看或修改发布属性所使用的 RMO 类取决于发布的类型。  
  
#### <a name="to-view-or-modify-properties-of-a-snapshot-or-transactional-publication"></a>查看或修改快照发布或事务发布的属性  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例，为发布设置 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为在步骤 1 中创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  （可选）若要更改属性，为一个或多个可设置的属性设置新值。 使用逻辑 AND 运算符（在 Microsoft Visual C# 中为 **&** ，在 Microsoft Visual Basic 中为 **And** ）可确定是否为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes> 属性设置了给定的 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 值。 使用逻辑 OR 运算符（在 Visual C# 中为 **|** ，在 Visual Basic 中为 **Or** ）和逻辑异或运算符（在 Visual C# 中为 **^** ，在 Visual Basic 中为 **Xor** ）可更改 <xref:Microsoft.SqlServer.Replication.PublicationAttributes> 属性的 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 值。  
  
5.  （可选）如果已将 **P:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges** 的值指定为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>，则调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 方法以在服务器上提交更改。 如果将 **false** 的值指定为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> （默认值），则会将更改立即发送到服务器。  
  
#### <a name="to-view-or-modify-properties-of-a-merge-publication"></a>查看或修改合并发布的属性  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例，为发布设置 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> 属性并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为在步骤 1 中创建的连接。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 **false**，则说明步骤 2 中的发布属性定义不正确，或者此发布不存在。  
  
4.  （可选）若要更改属性，为一个或多个可设置的属性设置新值。 使用逻辑 AND 运算符（在 Microsoft Visual C# 中为 **&** ，在 Visual Basic 中为 **And** ）可确定是否为 <xref:Microsoft.SqlServer.Replication.PublicationAttributes> 属性设置了给定的 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 值。 使用逻辑 OR 运算符（在 Visual C# 中为 **|** ，在 Visual Basic 中为 **Or** ）和逻辑异或运算符（在 Visual C# 中为 **^** ，在 Visual Basic 中为 **Xor** ）可更改 <xref:Microsoft.SqlServer.Replication.PublicationAttributes> 属性的 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 值。  
  
5.  （可选）如果已将 **P:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges** 的值指定为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>，则调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 方法以在服务器上提交更改。 如果将 **false** 的值指定为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> （默认值），则会将更改立即发送到服务器。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 该示例设置事务发布的发布属性。 这些更改将被缓存，直至将其显式发送到服务器。  
  
 [!code-cs[HowTo#rmo_ChangeTranPub_cached](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changetranpub_cached)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeTranPub_cached](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changetranpub_cached)]  
  
 该示例禁用合并发布的 DDL 复制。  
  
 [!code-cs[HowTo#rmo_ChangeMergePub_ddl](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changemergepub_ddl)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergePub_ddl](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changemergepub_ddl)]  
  
## <a name="see-also"></a>另请参阅  
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [对发布数据库进行架构更改](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [在发布中添加和删除项目 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md)   
 [使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [查看和修改项目属性](../../../relational-databases/replication/publish/view-and-modify-article-properties.md)  
  
  
