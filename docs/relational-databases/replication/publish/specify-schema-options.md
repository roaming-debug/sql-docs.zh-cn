---
description: 为 SQL Server 复制指定架构选项
title: 为 SQL Server 复制指定架构选项 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- schemas [SQL Server replication], options
- articles [SQL Server replication], transactional replication options
- articles [SQL Server replication], merge replication options
- articles [SQL Server replication], schema options
ms.assetid: 1f85a479-bd6e-4023-abf7-7435a7e5b567
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 91de6a53c6caf9e0bfd0339ea3c1354ce8984f4b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468898"
---
# <a name="specify-schema-options-for-sql-server-replication"></a>为 SQL Server 复制指定架构选项
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中指定架构选项。 在发布表或视图时，可以控制为发布的对象复制的对象创建选项。 创建项目时可以设置这些选项，还可以在以后更改它们。 如果没有为某项目显式指定这些选项，将定义默认的选项集。  
  
> [!NOTE]  
>  使用复制存储过程时的默认架构选项可能与使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]添加项目时的默认选项不同。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
-   **指定架构选项，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果在创建发布后更改架构选项，则必须生成新的快照。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   有关架构选项的完整列表，请参阅 [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 和 [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 的 `@schema_option` 参数。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 在“项目属性 - \<Article>”对话框的“属性”选项卡上指定架构选项，例如是否将约束和触发器复制到订阅服务器 。 此选项卡可在新建发布向导和“发布属性 - \<Publication>”对话框中获得。 有关如何使用该向导和如何访问该对话框的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)和[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-specify-schema-options"></a>指定架构选项  
  
1.  在“新建发布”向导或“发布属性 - \<Publication>”对话框的“项目”页面上，选择一个项目，然后单击“项目属性”  。  
  
2.  选择要将架构选项更改应用于哪些项目：  
  
    -   单击“设置突出显示的 \<ObjectType> 项目的属性”以启动“项目属性 - \<ObjectName>”对话框；在此对话框中进行的属性更改仅应用于在“项目”页上的对象窗格中突出显示的对象。    
  
    -   单击“设置所有 \<ObjectType> 项目的属性”以启动“所有 \<ObjectType> 项目的属性”对话框；在此对话框中进行的属性更改应用于“项目”页面上对象窗格中该类型的所有对象，包括尚未选择进行发布的对象  。  
  
        > [!NOTE]  
        >  在“所有 \<ObjectType> 项目的属性”对话框中进行的属性更改会替代之前在“项目属性 - \<ObjectName>”对话框中进行的任何更改。  例如，若要为某对象类型的所有项目设置一些默认值，但还希望为单个对象设置一些属性，请首先设置所有项目的默认值。 然后再设置单个对象的属性。  
  
3.  在“项目属性 - \<Article>”对话框的“属性”选项卡的“将对象和设置复制到订阅服务器”和“目标对象”部分中，为各选项指定值   。  
  
4.  根据需要修改属性，然后单击 **“确定”** 。  
  
5.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框 。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 架构选项指定为十六进制值，该值为一个或多个选项的 [|（位或）](../../../t-sql/language-elements/bitwise-or-transact-sql.md) 结果。 有关详细信息，请参阅 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 和 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。  
  
> [!NOTE]  
>  必须先将架构选项值从 **binary** 转换为 **int** ，才能执行位运算。 有关详细信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../../t-sql/functions/cast-and-convert-transact-sql.md)。  
  
#### <a name="to-specify-schema-options-when-defining-an-article-for-a-snapshot-or-transactional-publication"></a>为快照或事务发布定义项目时指定架构选项  
  
1.  在发布服务器上，对发布数据库执行 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。 为 `@publication` 指定项目所属的发布的名称，为 `@article` 指定项目的名称，为 `@source_object` 指定要发布的数据对象，为 `@type` 指定数据对象的类型，为 `@schema_option` 指定一个或多个架构选项的 [|（按位 OR）](../../../t-sql/language-elements/bitwise-or-transact-sql.md)结果。 有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
#### <a name="to-specify-schema-options-when-defining-an-article-for-a-merge-publication"></a>为合并发布定义项目时指定架构选项  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。 为 `@publication` 指定项目所属的发布的名称，为 `@article` 指定项目的名称，为 `@source_object` 指定要发布的数据对象，并为 `@schema_option` 指定一个或多个架构选项的 [|（按位 OR）](../../../t-sql/language-elements/bitwise-or-transact-sql.md)结果。 有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
#### <a name="to-change-schema-options-for-an-existing-article-in-a-snapshot-or-transactional-publication"></a>更改快照发布或事务发布中的现有项目的架构选项  
  
1.  在发布服务器上，对发布数据库执行 [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)。 为 `@publication` 指定项目所属的发布的名称，并为 `@article` 指定项目的名称。 记下结果集中 `schema_option` 列的值。  
  
2.  使用步骤 1 中的值和所需的架构选项值执行 [&（按位与）](../../../t-sql/language-elements/bitwise-and-transact-sql.md)运算，以确定是否设置了此选项。  
  
    -   如果结果为 **0**，则表示未设置此选项。  
  
    -   如果结果为选项值，则表示已设置此选项。  
  
3.  如果未设置此选项，则使用步骤 1 中的值和所需的架构选项值执行 [|（位或）](../../../t-sql/language-elements/bitwise-or-transact-sql.md) 运算。  
  
4.  在发布服务器上，对发布数据库执行 [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)。 为 `@publication` 指定项目所属的发布的名称，为 `@article` 指定项目的名称，为 `@property` 指定 `schema_option` 值，并为 `@value` 指定步骤 3 中的十六进制结果。  
  
5.  运行快照代理以生成新快照。 有关详细信息，请参阅 [创建并应用初始快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
#### <a name="to-change-schema-options-for-an-existing-article-in-a-merge-publication"></a>为合并发布中的现有项目更改架构选项  
  
1.  在发布服务器上，对发布数据库执行 [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)。 为 `@publication` 指定项目所属的发布的名称，并为 `@article` 指定项目的名称。 记下结果集中 **schema_option** 列的值。  
  
2.  使用步骤 1 中的值和所需的架构选项值执行 [&（按位与）](../../../t-sql/language-elements/bitwise-and-transact-sql.md)运算，以确定是否设置了此选项。  
  
    -   如果结果为 **0**，则表示未设置此选项。  
  
    -   如果结果为选项值，则表示已设置此选项。  
  
3.  如果未设置此选项，则使用步骤 1 中的值和所需的架构选项值执行 [|（位或）](../../../t-sql/language-elements/bitwise-or-transact-sql.md) 运算。  
  
4.  在发布服务器上，对发布数据库执行 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)。 为 `@publication` 指定项目所属的发布的名称，为 `@article` 指定项目的名称，为 `@property` 指定 `schema_option` 值，并为 `@value` 指定步骤 3 中的十六进制结果。  
  
5.  运行快照代理以生成新快照。 有关详细信息，请参阅 [创建并应用初始快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
## <a name="see-also"></a>另请参阅  
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Article Options for Transactional Replication](../../../relational-databases/replication/transactional/article-options-for-transactional-replication.md)  
  
  
