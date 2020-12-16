---
description: 定义和修改静态行筛选器
title: 定义和修改静态行筛选器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying filters, static row
- static row filters
- filters [SQL Server replication], static row
ms.assetid: a6ebb026-026f-4c39-b6a9-b9998c3babab
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 626665bff61cd6f0c1799ef52134c50688d962b6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477728"
---
# <a name="define-and-modify-a-static-row-filter"></a>定义和修改静态行筛选器
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中定义和修改静态行筛选器。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
-   **定义和修改静态行筛选器，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果在初始化对发布的订阅后添加、修改或删除静态行筛选器，则必须在更改后生成新的快照并重新初始化所有订阅。 有关属性更改要求的详细信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
-   如果为对等事务复制启用了发布，则不能筛选表。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   由于这些筛选器是静态的，因此所有订阅服务器都将接收到相同的数据子集。 如果您需要在属于合并发布的表项目中动态筛选行，以使每一订阅服务器都能接收到不同的数据分区，请参阅 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。 您还可使用合并复制基于现有的行筛选器筛选相关的行。 有关详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在“新建发”布向导的“筛选表行”页面或者在“发布属性 - \<Publication>”对话框的“筛选行”页面上定义、修改和删除静态行筛选器  。 有关如何使用该向导和如何访问该对话框的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)和[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-define-a-static-row-filter"></a>定义静态行筛选器  
  
1.  在“新建发布”导的“筛选表行”页面上或者在“发布属性 - \<Publication>”对话框的“筛选行”页面上，发布类型决定了你执行的操作  ：  
  
    -   对于快照发布或事务发布，请单击 **“添加”** 。  
  
    -   对于合并发布，请单击 **“添加”** ，再单击 **“添加筛选器”** 。  
  
2.  在 **“添加筛选器”** 对话框中，从下拉列表框中选择要筛选的表。  
  
3.  在 **“筛选语句”** 文本区域创建筛选语句。 您可以在文本区域中直接键入，也可以从 **“列”** 列表框中拖放列。  
  
    > [!NOTE]  
    >  WHERE 子句应使用由两部分构成的命名方式，不支持由三部分和四部分构成的命名方式。 如果发布来自 Oracle 发布服务器，则 WHERE 子句必须符合 Oracle 语法。  
  
    -   **“筛选语句”** 文本区域包括默认的文本，其格式为：  
  
        ```sql  
        SELECT <published_columns> FROM [schema].[tablename] WHERE  
        ```  
  
    -   默认文本无法更改；请使用标准的 SQL 语法在 WHERE 关键字后键入筛选子句。 完整筛选子句如下所示：  
  
        ```sql  
        SELECT <published_columns> FROM [HumanResources].[Employee] WHERE [LoginID] = 'adventure-works\ranjit0'  
        ```  
  
    -   静态行筛选器可以包含用户定义函数。 包含用户定义函数的静态行筛选器的完整筛选子句如下所示：  
  
        ```sql  
        SELECT <published_columns> FROM [Sales].[SalesOrderHeader] WHERE MyFunction([Freight]) > 100  
        ```  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框 。  

#### <a name="to-modify-a-static-row-filter"></a>修改静态行筛选器  
  
1.  在“新建发布”向导的“筛选表行”页面上或者在“发布属性 - \<Publication>”对话框的“筛选行”页面上，在“筛选的表”窗格中选择一个筛选器，然后单击“编辑”    。  
  
2.  在 **“编辑筛选器”** 对话框中，修改筛选器。  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-static-row-filter"></a>删除静态行筛选器  
  
1.  在“新建发布”向导的“筛选表行”页面上或者在“发布属性 - \<Publication>”对话框的“筛选行”页面上，在“筛选的表”窗格中选择一个筛选器，然后单击“删除”    。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 在创建表项目时，可以定义 WHERE 子句以筛选项目中的行。 定义行筛选器后，还可以对其进行更改。 可使用复制存储过程以编程的方式创建和修改静态行筛选器。  
  
#### <a name="to-define-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布定义静态行筛选器  
  
1.  定义要筛选的项目。 有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
2.  在发布服务器上，对发布数据库执行 [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)。 为 \@article 指定项目的名称，为 \@publication 指定发布的名称，为 \@filter_name 指定筛选器的名称，并为 \@filter_clause 指定筛选子句（不包括 `WHERE`）   。  
  
3.  如果还必须定义列筛选器，请参阅 [定义和修改列筛选器](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)。 否则，执行 [sp_articleview &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)。 为 \@publication 指定发布名称，为 \@article 指定筛选项目的名称，为 \@filter_clause 指定步骤 2 中指定的筛选子句  。 这将为筛选的项目创建同步对象。  
  
#### <a name="to-modify-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布修改静态行筛选器  
  
1.  在发布服务器上，对发布数据库执行 [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)。 为 \@article 指定项目的名称，为 \@publication 指定发布的名称，为 \@filter_name 指定新筛选器的名称，并为 \@filter_clause 指定新筛选子句（不包括 `WHERE`）   。 由于此更改将使现有订阅中的数据失效，因此请将 \@force_reinit_subscription 的值指定为 1 。  
  
2.  在发布服务器上，对发布数据库执行 [sp_articleview &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)。 为 \@publication 指定发布名称，为 \@article 指定筛选项目的名称，为 \@filter_clause 指定步骤 1 中指定的筛选子句  。 这将重新创建定义筛选项目的视图。  
  
3.  对发布重新运行快照代理作业以生成更新的快照。 有关详细信息，请参阅 [创建并应用初始快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
4.  重新初始化订阅。 有关详细信息，请参阅 [重新初始化订阅](../../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
#### <a name="to-delete-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布删除静态行筛选器  
  
1.  在发布服务器上，对发布数据库执行 [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)。 为 \@article 指定项目的名称，为 \@publication 指定发布的名称，将 \@filter_name 的值指定为 NULL，并将 \@filter_clause 的值指定为 NULL   。 由于此更改将使现有订阅中的数据失效，因此请将 \@force_reinit_subscription 的值指定为 1 。  
  
2.  对发布重新运行快照代理作业以生成更新的快照。 有关详细信息，请参阅 [创建并应用初始快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
3.  重新初始化订阅。 有关详细信息，请参阅 [重新初始化订阅](../../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
#### <a name="to-define-a-static-row-filter-for-a-merge-publication"></a>为合并发布定义静态行筛选器  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。 为 \@subset_filterclause 指定筛选子句（不包括 `WHERE`）。 有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
2.  如果还必须定义列筛选器，请参阅 [定义和修改列筛选器](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)。  
  
#### <a name="to-modify-a-static-row-filter-for-a-merge-publication"></a>为合并发布修改静态行筛选器  
  
1.  在发布服务器上，对发布数据库执行 [sp_changemergearticle (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。 为 \@publication 指定发布名称，为 \@article 指定筛选项目的名称，为 \@property 指定 subset_filterclause 值，并为 \@value 指定新筛选子句（不包括 `WHERE`）    。 由于此更改将使现有订阅中的数据失效，因此请将 \@force_reinit_subscription 的值指定为 1。  
  
2.  对发布重新运行快照代理作业以生成更新的快照。 有关详细信息，请参阅 [创建并应用初始快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
3.  重新初始化订阅。 有关详细信息，请参阅 [重新初始化订阅](../../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 在此事务复制示例中，对项目进行水平筛选以删除所有停产的产品。  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-stat_1.sql)]  
  
 在此合并复制示例中，对项目进行水平筛选以仅返回属于指定销售人员的行。 其中还使用了联接筛选器。 有关详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-stat_2.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)   
 [为合并复制筛选已发布数据](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  
