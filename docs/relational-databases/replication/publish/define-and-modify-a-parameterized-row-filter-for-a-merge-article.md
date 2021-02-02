---
title: 定义和修改参数化行筛选器（合并）
description: 了解如何定义和修改 SQL Server 合并发布项目的参数化行筛选器。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- parameterized filters [SQL Server replication], defining
- parameterized filters [SQL Server replication], modifying
- merge replication [SQL Server replication], dynamic filters
- merge replication parameterized filters [SQL Server replication]
- filters [SQL Server replication], parameterized
- modifying filters, parameterized row
- dynamic filters [SQL Server replication]
ms.assetid: de0482a2-3cc8-4030-8a4a-14364549ac9f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 40d9ecf88b81263d6e35b5ade9ee6d0f3adeef49
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076735"
---
# <a name="define-and-modify-a-parameterized-row-filter-for-a-merge-article"></a>定义和修改合并项目的参数化行筛选器
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中定义和修改参数化行筛选器。  
  
 在创建表项目时，可以使用参数化行筛选器。 这些筛选器使用 [WHERE](../../../t-sql/queries/where-transact-sql.md) 子句来选择要发布的相应数据。 请勿在子句中指定文本值（像用静态行筛选器处理那样），而是指定以下一个或两个系统函数：[SUSER_SNAME](../../../t-sql/functions/suser-sname-transact-sql.md) 和 [HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md) 有关详细信息，请参阅 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
-   **定义和修改参数化行筛选器，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果在初始化对发布的订阅后添加、修改或删除参数化行筛选器，必须在更改后生成新的快照并重新初始化所有订阅。 有关属性更改要求的详细信息，请参阅[更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   出于性能方面的考虑，我们建议您不要将这些函数应用于参数化行筛选器子句（如 `LEFT([MyColumn]) = SUSER_SNAME()`）中的列名。 如果在筛选子句中使用 HOST_NAME 并覆盖 HOST_NAME 值，则可能需要使用 CONVERT 转换数据类型。 有关此情况的最佳实践的详细信息，请参阅主题 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“覆盖 HOST_NAME() 值”一节。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”对话框的“筛选行”页上定义、修改和删除参数化行筛选器。 有关如何使用该向导和如何访问该对话框的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)和[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-define-a-parameterized-row-filter"></a>定义参数化行筛选器  
  
1.  在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”的“筛选行”页上，单击“添加”，然后单击“添加筛选器”。  
  
2.  在 **“添加筛选器”** 对话框中，从下拉列表框中选择要筛选的表。  
  
3.  在 **“筛选语句”** 文本框中创建一个筛选语句。 您可以在文本区域中直接键入，也可以从 **“列”** 列表框中拖放列。  
  
    -   **“筛选语句”** 文本区域包括默认的文本，其格式为：  
  
        ```  
        SELECT <published_columns> FROM [tableowner].[tablename] WHERE  
        ```  
  
    -   默认文本无法更改；请使用标准的 SQL 语法在 WHERE 关键字后键入筛选子句。 参数化筛选器包括对系统函数 **HOST_NAME()** 和/或 **SUSER_SNAME()** 的调用，或者对引用这两个函数之一或全部的用户定义函数的调用。 以下是参数化行筛选器的一个完整筛选子句的示例：  
  
        ```  
        SELECT <published_columns> FROM [HumanResources].[Employee] WHERE LoginID = SUSER_SNAME()  
        ```  
  
         WHERE 子句应使用由两部分构成的命名方式，不支持由三部分和四部分构成的命名方式。  
  
4.  选择指示订阅服务器之间如何共享数据的选项：  
  
    -   **此表中的行将转到多个订阅**  
  
    -   **此表中的行将仅转到一个订阅**  
  
     如果选择 **“此表中的行将仅转到一个订阅”** ，则合并复制可以通过存储和处理较少的元数据来优化性能。 但是，必须确保在对数据分区时不能将行复制到多个订阅服务器。 有关详细信息，请参阅主题 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“设置‘分区选项’”部分。  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
6.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框 。  
  
#### <a name="to-modify-a-parameterized-row-filter"></a>修改参数化行筛选器  
  
1.  在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”的“筛选行”页上，在“筛选的表”窗格中选择筛选器，然后单击“编辑”。      
  
2.  在 **“编辑筛选器”** 对话框中，修改筛选器。  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-parameterized-row-filter"></a>删除参数化行筛选器  
  
1.  在新建发布向导的“筛选表行”页或“发布属性 - \<Publication>”的“筛选行”页上，在“筛选的表”窗格中选择筛选器，然后单击“删除”。      
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式创建和修改参数化行筛选器。  
  
#### <a name="to-define-a-parameterized-row-filter-for-an-article-in-a-merge-publication"></a>为合并发布中的项目定义参数化行筛选器  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。 指定 \@publication，为 \@article 指定项目名称，为 \@source_object 指定要发布的表，为 \@subset_filterclause 指定定义参数化筛选器的 WHERE 子句（不包括 `WHERE`），并为 \@partition_options（说明从参数化行筛选器得出的分区类型）指定下列值之一    ：  
  
    -   **0** - 对项目的筛选是静态的，或者不为每个分区生成唯一的数据子集（“重叠”分区）。  
  
    -   **1** - 导致分区重叠，并且在订阅服务器上所做的更新不能更改行所属的分区。  
  
    -   **2** - 对项目的筛选将生成不重叠分区，但多个订阅服务器可以接收相同的分区。  
  
    -   **3** - 对项目的筛选将为每个订阅生成唯一的不重叠分区。  
  
#### <a name="to-change-a-parameterized-row-filter-for-an-article-in-a-merge-publication"></a>更改合并发布中的项目的参数化行筛选器  
  
1.  在发布服务器上，对发布数据库执行 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。 指定 \@publication 和 \@article，为 \@property 指定 subset_filterclause 值，为 \@value 指定定义参数化筛选器的表达式（不包括 `WHERE`），并将 \@force_invalidate_snapshot 和 \@force_reinit_subscription 的值指定为 1       。  
  
2.  如果此更改导致不同的分区行为，则再次执行 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) 。 指定 \@publication 和 \@article，为 \@property 指定 partition_options 值，并为 \@value 指定最适合的选项，该选项可以为下列选项之一：    ：  
  
    -   **0** - 对项目的筛选是静态的，或者不为每个分区生成唯一的数据子集（“重叠”分区）。  
  
    -   **1** - 导致分区重叠，并且在订阅服务器上所做的更新不能更改行所属的分区。  
  
    -   **2** - 对项目的筛选将生成不重叠分区，但多个订阅服务器可以接收相同的分区。  
  
    -   **3** - 对项目的筛选将为每个订阅生成唯一的不重叠分区。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例在合并发布中定义一组项目，其中的项目是使用一系列联接筛选器基于 `Employee` 表筛选的，而该表则是使用参数化行筛选器基于 **LoginID** 列进行自身筛选的。 在同步期间，由 [HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md) 函数返回的值将被覆盖。 有关详细信息，请参阅主题 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“覆盖 HOST_NAME() 值”。  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-para_1.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [定义和修改合并项目间的联接筛选器](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Join Filters](../../../relational-databases/replication/merge/join-filters.md)   
 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
