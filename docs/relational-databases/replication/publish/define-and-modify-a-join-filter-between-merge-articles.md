---
title: 定义和修改合并项目间的联接筛选器
description: 了解如何使用 SQL Server Management Studio (SSMS) 或 Transact-SQL (T-SQL) 定义和修改用于合并项目的联接筛选器。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- filters [SQL Server replication], join
- merge replication join filters [SQL Server replication]
- modifying filters, join
- join filters
ms.assetid: f7f23415-43ff-40f5-b3e0-0be1d148ee5b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4ef84f22bf255b8dcb21ba371de5c3045da3ca6b
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076745"
---
# <a name="define-and-modify-a-join-filter-between-merge-articles"></a>定义和修改合并项目间的联接筛选器
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中定义和修改合并项目间的联接筛选器。 合并复制支持联接筛选器，这类筛选器通常与参数化筛选器配合使用，以将表分区扩展到其他相关的表项目。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
-   **定义和修改合并项目间的联接筛选器，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   若要创建联接筛选器，发布必须至少包含两个相关表。 联接筛选器可扩展行筛选器；因此必须先对一个表定义行筛选器，才能使用联接将该筛选器扩展到另一个表。 定义一个联接筛选器后，如果发布包含更多相关表，则可使用其他联接筛选器来扩展此联接筛选器。  
  
-   如果在初始化对发布的订阅后添加、修改或删除联接筛选器，必须在更改后生成新的快照并重新初始化所有订阅。 有关属性更改要求的详细信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   可以为一组表手动创建联接筛选器，或者复制可以基于表上定义的外键和主键之间的关系自动生成筛选器。 有关自动生成一组联接筛选器的详细信息，请参阅[在合并项目之间自动生成一组联接筛选器 &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md)。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”对话框的“筛选行”页上定义、修改和删除联接筛选器。   有关如何使用该向导和如何访问该对话框的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)和[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-define-a-join-filter"></a>定义联接筛选器  
  
1.  在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”的“筛选行”页上，在“筛选的表”窗格中选择现有行筛选器或联接筛选器。     
  
2.  单击 **“添加”** ，再单击 **“添加联接以扩展所选筛选器”** 。  
  
3.  创建联接语句：选择 **“使用生成器创建语句”** 或 **“手动编写联接语句”** 。  
  
    -   如果选择使用生成器，请使用网格中的列（ **“连接”** 、 **“筛选的表列”** 、 **“运算符”** 、和 **“联接的表列”** ）生成联接语句。  
  
         网格中的每个列都包含下拉组合框，允许你选择两个列和一个运算符（=、<>、<=、\<**, **>=、> 和 like）。 结果显示在 **“预览”** 文本区域中。 如果联接中涉及多个列对，则从 **“连接词”** 列中选择一个连接词（AND 或 OR），然后再输入两列和一个运算符。  
  
    -   如果选择手动编写语句，那么请在 **“联接语句”** 文本区域编写联接语句。 使用 **“筛选的表列”** 列表框和 **“联接的表列”** 列表框将列拖放到 **“联接语句”** 文本区域。  
  
    -   完整的联接语句如下所示：  
  
        ```  
        SELECT <published_columns> FROM [Sales].[SalesOrderHeader] INNER JOIN [Sales].[SalesOrderDetail] ON [SalesOrderHeader].[SalesOrderID] = [SalesOrderDetail].[SalesOrderID]  
        ```  
  
         JOIN 子句应使用由两个部分组成的名称来命名；它不支持由三个部分和四个部分组成的名称命名。  
  
4.  指定联接选项：  
  
    -   如果在筛选的表（父表）中联接的列是唯一的，则选择 **“唯一键”** 。  
  
        > [!CAUTION]  
        >  选择此选项表示联接筛选器中子表和父表是一对一还是一对多的关系。 仅当子表的联接列上具有保证唯一性的约束时才选择此选项。 如果未能正确设置此选项，可能无法收敛数据。  
  
    -   默认情况下，合并复制在同步过程中会逐行处理更改。 要将筛选的表行和联接的表行中的相关更改作为一个单元进行处理，则选择“逻辑记录”（仅 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本）。 只有满足使用逻辑记录的项目和发布要求，此选项才可用。 有关详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)中的“使用逻辑记录的注意事项”部分。  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
6.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框 。  

#### <a name="to-modify-a-join-filter"></a>修改联接筛选器  
  
1.  在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”的“筛选行”页上，在“筛选的表”窗格中选择筛选器，然后单击“编辑”。      
  
2.  在 **“编辑联接”** 对话框中，修改筛选器。  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-join-filter"></a>删除联接筛选器  
  
1.  在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”的“筛选行”页上，在“筛选的表”窗格中选择筛选器，然后单击“删除”。     如果删除的联接筛选器自身是由其他联接扩展而成的，则也将删除那些联接。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 这些过程显示了父项目上的参数化筛选器以及该项目和相关子项目间的联接筛选器。 可以使用复制存储过程，以编程方式定义和修改联接筛选器。  
  
#### <a name="to-define-a-join-filter-to-extend-an-article-filter-to-related-articles-in-a-merge-publication"></a>定义联接筛选器以将项目筛选器扩展到合并发布中的相关项目  
  
1.  为要联接到的项目（又称为父项目）定义筛选。  
  
    -   对于使用参数化行筛选器筛选的项目，请参阅 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
    -   对于使用静态行筛选器筛选的项目，请参阅 [Define and Modify a Static Row Filter](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)。  
  
2.  在发布服务器上，对发布数据库执行 [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 来为发布定义一个或多个相关项目（又称为子项目）。 有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
3.  在发布服务器上，对发布数据库执行 [sp_addmergefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)。 指定 `@publication`，为 `@filtername` 指定此筛选器的唯一名称，为 `@article` 指定在步骤 2 中创建的子项目的名称，为 `@join_articlename` 指定要联接到的父项目的名称，并为 `@join_unique_key` 指定以下值之一：  
  
    -   **0** - 表示父项目与子项目间的多对一或多对多联接。  
  
    -   **1** - 表示父项目与子项目间的一对一或一对多联接。  
  
     此步骤定义了两个项目间的联接筛选器。  
  
    > [!CAUTION]  
    >  仅当对父项目的基础表中的联接列具有可保证唯一性的约束时，才将 `@join_unique_key` 设置为 1。 如果错误地将 `@join_unique_key` 设置为 1，则可能无法收敛数据。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例定义了合并发布的项目，并针对 `SalesOrderDetail` 表来筛选 `SalesOrderHeader` 表项目，而该表本身使用静态行筛选器进行筛选。 有关详细信息，请参阅 [Define and Modify a Static Row Filter](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)。  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-join_1.sql)]  
  
 此示例定义了合并发布中的一组项目，并针对 `Employee` 表使用一系列联接筛选器来筛选这些项目，而该表本身则使用参数化行筛选器按 [LoginID](../../../t-sql/functions/host-name-transact-sql.md) 列中的 **HOST_NAME** 值进行筛选。 有关详细信息，请参阅 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-join_2.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [Join Filters](../../../relational-databases/replication/merge/join-filters.md)   
 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [为合并复制筛选已发布数据](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)   
 [如何：定义和修改合并项目间的联接筛选器 (SQL Server Management Studio)](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [定义合并表项目间的逻辑记录关系](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md)   
 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)  
  
  
