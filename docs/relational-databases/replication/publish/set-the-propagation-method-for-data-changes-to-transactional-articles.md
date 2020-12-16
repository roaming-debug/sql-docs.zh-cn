---
title: 为项目的更改设置传播方法（事务）
description: 介绍如何使用 SQL Server Management Studio (SSMS) 或 Transact-SQL (T-SQL) 为事务复制中的事务项目数据更改设置传播方法。
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, propagation methods
- propagating data changes [SQL Server replication]
ms.assetid: 0a291582-f034-42da-a1a3-29535b607b74
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 2079a1dbb17677c7f79f2e1dc27634a24ade5cd8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468908"
---
# <a name="set-the-propagation-method-for-data-changes-to-transactional-articles"></a>为事务项目的数据更改设置传播方法
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中为事务项目的数据更改设置传播方法。  
  
 默认情况下，事务复制使用一组存储过程将每个项目的更改传播到订阅服务器。 可以使用自定义过程替换这些存储过程。 有关详细信息，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
-   **为事务项目的数据更改设置传播方法，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
## <a name="before-you-begin"></a>开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   在编辑复制生成的任何快照文件时，都必须谨慎。 必须测试和支持自定义存储过程中的自定义逻辑。 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 不支持自定义逻辑。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 在“项目属性 - \<Article>”对话框的“属性”选项卡上指定传播方法，该对话框可在“新建发布”向导和“发布属性 - \<Publication>”对话框中找到  。 有关如何使用该向导和如何访问该对话框的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)和[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-specify-the-propagation-method"></a>指定传播方法  
  
1.  在新建发布向导或“发布属性 - \<Publication>”对话框的“项目”页上，选择一个表，然后单击“项目属性”。    
  
2.  单击 **“设置突出显示的表项目的属性”** 。  
  
3.  在“项目属性 - \<Article>”对话框的“属性”选项卡上，在“语句传递”部分中，使用“INSERT 传递格式”、“UPDATE 传递格式”和“DELETE 传递格式”菜单为每个操作指定传播方法     。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框 。  

#### <a name="to-generate-and-use-custom-stored-procedures"></a>生成和使用自定义存储过程  
  
1.  在新建发布向导或“发布属性 - \<Publication>”对话框的“项目”页上，选择一个表，然后单击“项目属性”。    
  
2.  单击 **“设置突出显示的表项目的属性”** 。  
  
     在“项目属性 - \<Article>”对话框的“属性”选项卡上，在“语句传递”部分中，从适当的传递格式菜单（“INSERT 传递格式”、“UPDATE 传递格式”或“DELETE 传递格式”）中选择 CALL 语法，然后键入要在“INSERT 存储过程”、“DELETE 存储过程”或“UPDATE 存储过程”中使用的过程名称        。 若要详细了解如何使用存储过程，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)中的“存储过程的调用语法”。  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
4.  如果处于“发布属性 - \<Publication>”对话框中，请单击“确定”以保存并关闭该对话框 。  
  
5.  发布的快照生成后，将包含上一步骤中指定的过程。 这些过程将使用指定的 CALL 语法，但将包含复制使用的默认逻辑。  
  
     快照生成后，定位到此项目所属的发布的快照文件夹，并找到与此项目同名的 **.sch** 文件。 使用记事本或其他文本编辑器打开此文件，找到用于插入、更新或删除存储过程的 CREATE PROCEDURE 命令，并编辑过程定义，以为传播数据更改提供自定义逻辑。 重新生成快照后，必须重新创建自定义过程。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 通过事务复制，您可以控制如何将更改从发布服务器传播到订阅服务器，而这种传播方法可以在创建和之后更改项目时使用复制存储过程以编程的方式进行设置。  
  
> [!NOTE]  
>  可以为作用于发布数据行的每一种 DML（数据操作语言）操作（插入、更新或删除）指定一种不同的传播方法。  
  
 有关详细信息，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
#### <a name="to-create-an-article-that-uses-transact-sql-commands-to-propagate-data-changes"></a>创建使用 Transact-SQL 命令传播数据更改的项目  
  
1.  在发布服务器上，对发布数据库执行 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。 为 \@publication 指定项目所属的发布的名称，为 \@article 指定项目的名称，为 \@source_object 指定要发布的数据库对象，并为下列至少一个参数指定 SQL 值：  
  
    -   \@ins_cmd - 控制 [INSERT](../../../t-sql/statements/insert-transact-sql.md) 命令的复制。  
  
    -   \@upd_cmd - 控制 [UPDATE](../../../t-sql/queries/update-transact-sql.md) 命令的复制。  
  
    -   \@del_cmd - 控制 [DELETE](../../../t-sql/statements/delete-transact-sql.md) 命令的复制。  
  
    > [!NOTE]  
    >  为以上任意参数指定 **SQL** 值后，该类型的命令将作为相应的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 命令复制到订阅服务器。  
  
     有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
#### <a name="to-create-an-article-that-does-not-propagate-data-changes"></a>创建不传播数据更改的项目  
  
1.  在发布服务器上，对发布数据库执行 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。 为 \@publication 指定项目所属的发布的名称，为 \@article 指定项目的名称，为 \@source_object 指定要发布的数据库对象，并为下列至少一个参数指定 NONE 值：  
  
    -   \@ins_cmd - 控制 [INSERT](../../../t-sql/statements/insert-transact-sql.md) 命令的复制。  
  
    -   \@upd_cmd - 控制 [UPDATE](../../../t-sql/queries/update-transact-sql.md) 命令的复制。  
  
    -   \@del_cmd - 控制 [DELETE](../../../t-sql/statements/delete-transact-sql.md) 命令的复制。  
  
    > [!NOTE]  
    >  将以上任意参数指定为 **NONE** 值后，该类型的命令将不会复制到订阅服务器。  
  
     有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
#### <a name="to-create-an-article-with-user-modified-custom-stored-procedures"></a>通过用户修改的自定义存储过程创建项目  
  
1.  在发布服务器上，对发布数据库执行 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。 为 \@publication 指定项目所属的发布的名称，为 \@article 指定项目的名称，为 \@source_object 指定要发布的数据库对象，并为包含值 0x02 （可自动生成自定义存储过程）的 \@schema_option 位掩码以及下列至少一个参数指定值：  
  
    -   **\@ins_cmd** - 指定 CALL sp_MSins_article_name 的值，其中 article_name 是为 \@article 指定的值。  
  
    -   **\@del_cmd** - 指定 CALL sp_MSdel_*article_name 或 CALL sp_MSdel_article_name 的值，其中 article_name 是为 \@article 指定的值。  
  
    -   **\@upd_cmd** - 指定 SCALL sp_MSupd_article_name、CALL sp_MSupd_article_name、XCALL sp_MSupd_article_name 或 MCALL sp_MSupd_article_name 的值，其中 article_name 是为 \@article 指定的值   。  
  
    > [!NOTE]  
    >  对于以上每个命令参数，您都可以为复制生成的存储过程指定您自己的名称。  
  
    > [!NOTE]  
    >  若要了解有关 CALL、SCALL、XCALL 和 MCALL 语法的详细信息，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
     有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
2.  快照生成后，定位到此项目所属的发布的快照文件夹，并找到与此项目同名的 **.sch** 文件。 使用 Notepad.exe 打开此文件，找到用于插入、更新或删除存储过程的 CREATE PROCEDURE 命令，并编辑过程定义以提供用于传播数据更改的任何自定义逻辑。 有关详细信息，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
#### <a name="to-create-an-article-with-custom-scripting-in-the-custom-stored-procedures-to-propagate-data-changes"></a>用自定义存储过程中的自定义脚本创建项目以传播数据更改  
  
1.  在发布服务器上，对发布数据库执行 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。 为 \@publication 指定项目所属的发布的名称，为 \@article 指定项目的名称，为 \@source_object 指定要发布的数据库对象，并为包含值 0x02 （可自动生成自定义存储过程）的 \@schema_option 位掩码以及下列至少一个参数指定值：  
  
    -   **\@ins_cmd** - 指定 CALL sp_MSins_article_name 的值，其中 article_name 是为 \@article 指定的值。  
  
    -   **\@del_cmd** - 指定 CALL sp_MSdel_*article_name 或 CALL sp_MSdel_article_name 的值，其中 article_name 是为 \@article 指定的值。  
  
    -   **\@upd_cmd** - 指定 SCALL sp_MSupd_article_name、CALL sp_MSupd_article_name、XCALL sp_MSupd_article_name 或 MCALL sp_MSupd_article_name 的值，其中 article_name 是为 \@article 指定的值   。  
  
    > [!NOTE]  
    >  对于以上每个命令参数，您都可以为复制生成的存储过程指定您自己的名称。  
  
    > [!NOTE]  
    >  若要了解有关 CALL、SCALL、XCALL 和 MCALL 语法的详细信息，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
     有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
2.  在发布服务器上，对发布数据库使用 [ALTER PROCEDURE](../../../t-sql/statements/alter-procedure-transact-sql.md) 语句来编辑 [sp_scriptpublicationcustomprocs](../../../relational-databases/system-stored-procedures/sp-scriptpublicationcustomprocs-transact-sql.md) ，以使该语句可以返回用于插入、更新和删除自定义存储过程的 [CREATE PROCEDURE](../../../t-sql/statements/create-procedure-transact-sql.md) 脚本。 有关详细信息，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
#### <a name="to-change-the-method-of-propagating-changes-for-an-existing-article"></a>更改为现有项目传播更改的方法  
  
1.  在发布服务器上，对发布数据库执行 [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)。 指定 **\@publication**、 **\@article**，为 **\@property** 指定值 **ins_cm**，**upd_cmd** 或 **del_cmd**，并为 **\@value** 指定合适的传播方法。  
  
2.  为每个要更改的传播方法重复步骤 1。  
  
## <a name="see-also"></a>另请参阅  
 [指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)   
 [创建发布](../../../relational-databases/replication/publish/create-a-publication.md)  
  
  
