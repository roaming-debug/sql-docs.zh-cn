---
description: 定义项目
title: 定义项目 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- articles [SQL Server replication], defining
- sp_addmergearticle
- adding articles
- sp_addarticle
- articles [SQL Server replication], adding
ms.assetid: 220584d8-b291-43ae-b036-fbba3cc07a2e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b93fbfea46931eb460b842a3a34ae13853726792
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766368"
---
# <a name="define-an-article"></a>定义项目
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中定义项目。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **定义项目，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   项目名称不能包含以下任何字符：%、*、[、]、|、:、"、? , ' , \ , / , < , >. 如果数据库中的对象包括任意上述字符，并且您希望复制它们，那么必须指定一个不同于相应对象名称的项目名称。  
  
##  <a name="security"></a><a name="Security"></a> Security  
 如果可能，请在运行时提示用户输入安全凭据。 如果必须存储凭据，请使用 [Windows .NET Framework 提供的](/previous-versions/aa719848(v=vs.71)) Cryptographic Services [!INCLUDE[msCoName](../../../includes/msconame-md.md)] （加密服务）。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可以使用新建发布向导创建发布和定义项目。 创建发布之后，可在“发布属性 - \<Publication>”对话框中查看和修改发布属性。 有关从 Oracle 数据库创建发布的信息，请参阅[从 Oracle 数据库创建发布](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md)。  
  
#### <a name="to-create-a-publication-and-define-articles"></a>创建发布和定义项目  
  
1.  在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再右键单击 **“本地发布”** 文件夹。  
  
3.  单击 **“新建发布”** 。  
  
4.  按照新建发布向导中的页完成以下任务：  

    -   如果尚未在服务器上配置分发，请指定分发服务器。 有关如何配置分发的详细信息，请参阅[配置发布和分发](../../../relational-databases/replication/configure-publishing-and-distribution.md)。  
  
         如果在 **“分发服务器”** 页上指定将发布服务器用作其自己的分发服务器（本地分发服务器），而未将服务器配置为分发服务器，则新建发布向导将配置该服务器。 在 **“快照文件夹”** 页中指定分发服务器的快照文件夹。 快照文件夹只是指定共享的目录。向此文件夹中执行读写操作的代理必须对其具有足够的访问权限。 有关正确保护文件夹的详细信息，请参阅[保护快照文件夹](../../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  
  
         如果指定另一台服务器作为分发服务器，则必须在 **“管理密码”** 页上输入密码来连接发布服务器和分发服务器。 此密码必须与在远程分发服务器上启用发布服务器时所指定的密码相匹配。  
  
         有关详细信息，请参阅 [Configure Distribution](../../../relational-databases/replication/configure-distribution.md)。  
  
    -   选择发布数据库。  
  
    -   选择发布类型。 有关详细信息，请参阅[复制类型](../../../relational-databases/replication/types-of-replication.md)。  
  
    -   指定要发布的数据和数据库对象；（可选）筛选来自表项目的列，并设置项目属性。  
  
    -   可选择筛选来自表项目的行。 有关详细信息，请参阅[筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)。  
  
    -   设置快照代理调度。  
  
    -   指定运行下列复制代理和进行连接的凭证：  
  
         \- 用于所有发布的快照代理。  
  
         \- 用于所有事务发布的日志读取器代理。  
  
         \- 用于允许更新订阅的事务发布的队列读取器代理。  
  
         有关详细信息，请参阅 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) 和 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)。  
  
    -   （可选）编写发布脚本。 有关详细信息，请参阅 [Scripting Replication](../../../relational-databases/replication/scripting-replication.md)。  
  
    -   指定发布的名称。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 在创建发布后，可以使用复制存储过程以编程方式创建项目。 用于创建项目的存储过程取决于要为其定义项目的发布的类型。 有关详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
#### <a name="to-define-an-article-for-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布定义项目  
  
1.  在发布服务器上，对发布数据库执行 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。 为 `@publication` 指定项目所属的发布名称，为 `@article` 指定项目的名称，并为 `@source_object` 指定要发布的数据库对象，同时指定任何其他可选参数。 如果不是 dbo，则使用 `@source_owner` 指定对象的架构所有权。 如果该项目不是基于日志的表项目，可将 `@type` 指定为该项目类型；有关详细信息，请参阅[指定项目类型（复制 Transact-SQL 编程）](../../../relational-databases/replication/publish/specify-article-types-replication-transact-sql-programming.md)。  
  
2.  若要水平筛选表中的行或查看项目，请使用 [sp_articlefilter](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) 来定义筛选子句。 有关详细信息，请参阅 [Define and Modify a Static Row Filter](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)。  
  
3.  若要垂直筛选表中的列或查看项目，请使用 [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)。 有关详细信息，请参阅 [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)。  
  
4.  如果已经筛选出了项目，请执行 [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) 。  
  
5.  如果发布已具有订阅，并且 [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) 在 **immediate_sync** 列中返回 **0** 值，必须调用 [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) 将项目添加到每个现有订阅。  
  
6.  如果发布已具有请求订阅，可在发布服务器上执行 [sp_refreshsubscriptions](../../../relational-databases/system-stored-procedures/sp-refreshsubscriptions-transact-sql.md) 为现有请求订阅创建只包含新项目的新快照。  
  
    > [!NOTE]  
    >  对于没有使用快照进行初始化的订阅，不需要执行 [sp_refreshsubscriptions](../../../relational-databases/system-stored-procedures/sp-refreshsubscriptions-transact-sql.md) ，因为该过程由 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)执行。  
  
#### <a name="to-define-an-article-for-a-merge-publication"></a>为合并发布定义项目  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。 为 `@publication` 指定发布的名称，为 `@article` 指定项目的名称，为 `@source_object` 指定要发布的对象。 若要水平筛选表行，请为 `@subset_filterclause` 指定一个值。 有关详细信息，请参阅 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md) 和 [定义和修改静态行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)。 如果相应项目不是表项目，请将 `@type` 指定为该项目类型。 有关详细信息，请参阅[指定项目类型（复制 Transact-SQL 编程）](../../../relational-databases/replication/publish/specify-article-types-replication-transact-sql-programming.md)。  
  
2.  （可选）在发布服务器上，对发布数据库执行 [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) 以在两个项目之间定义一个联接筛选器。 有关详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
3.  （可选）在发布服务器上的发布数据库中，执行 [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) 可筛选表列。 有关详细信息，请参阅 [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 本例为某个事务发布定义了一个基于 `Product` 表的项目，其中项目在水平和垂直两个方向上进行了筛选。  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-an-article_1.sql)]  
  
 本例为某个合并发布定义项目，其中 `SalesOrderHeader` 项目基于 **SalesPersonID** 进行静态筛选，而 `SalesOrderDetail` 项目则基于 `SalesOrderHeader`进行联接筛选。  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-an-article_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 可以使用复制管理对象 (RMO) 以编程方式定义项目。 用来定义项目的 RMO 类取决于要为其定义项目的发布的类型。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 下例向一个事务发布添加一个带有行和列筛选器的项目。  
  
 [!code-cs[HowTo#rmo_CreateTranArticles](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranarticles)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranArticles](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranarticles)]  
  
 下例将三个项目添加到一个合并发布。 这些项目具有列筛选器，而且使用两个联接筛选器来将一个参数化行筛选器传播给其他项目。  
  
 [!code-cs[HowTo#rmo_CreateMergeArticles](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergearticles)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergeArticles](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergearticles)]  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)   
 [筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)   
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
