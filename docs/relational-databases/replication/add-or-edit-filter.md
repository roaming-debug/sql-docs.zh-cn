---
description: 添加或编辑筛选器
title: 添加或编辑筛选器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.addeditfilter.f1
ms.assetid: bdd7c71d-1c59-4044-bfe8-c85f908345bb
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 2cc745a35d1444f19da3916f090c231bb1b0f549
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461888"
---
# <a name="add-or-edit-filter"></a>添加或编辑筛选器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  使用 **“添加筛选器”** 和 **“编辑筛选器”** 对话框，可以添加和编辑静态行筛选器和参数化行筛选器。  
  
> [!NOTE]  
>  编辑现有发布中的筛选器需要为该发布创建新的快照。 如果发布有订阅，则必须重新初始化这些订阅。 有关属性更改的详细信息，请参阅[更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
 所有发布类型都可以包含静态筛选器；合并发布也可以包含参数化筛选器。 在创建发布时将对静态筛选器求值：该发布的所有订阅服务器将接收相同的数据。 在复制同步期间将对参数化筛选器求值：不同的订阅服务器接收不同的数据分区，这取决于各个订阅服务器的登录名或计算机名称。 在该对话框中单击 **“示例语句”** 链接，可以查看每种筛选器的示例。 有关筛选选项的详细信息，请参阅[筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)。  
  
 通过使用行筛选器，可以指定要从表中发布的行的子集。 可以将行筛选器用于隐藏用户不需要查看的行（例如，包含敏感信息或机密信息的行），或用于创建发送到不同订阅服务器的不同数据分区。 将不同的数据分区发布到不同的订阅服务器，还有助于避免由于多个订阅服务器更新相同的数据而导致的冲突。  
  
## <a name="options"></a>选项  
 对于事务发布和快照发布，此对话框涉及的过程分为两个步骤；对于合并分布，此对话框涉及的过程分为三个步骤。 所有发布类型都要求您选择要筛选的表以及要包含在筛选器中的一列或多列，并将筛选器定义为标准的 WHERE 子句。  
  
1.  **选择要筛选的表**  
  
     如果正在编辑现有筛选器，则不能更改选择的表。 如果正在添加新的筛选器，请从下拉列表框中选择表。 只有当在 **“项目”** 页上选择了表并且这些表尚未具有行筛选器时，这些表才会显示在列表框中。 如果表具有行筛选器，若要定义新的筛选器，请执行以下操作：  
  
    1.  在 **“添加筛选器”** 对话框上单击 **“取消”** 。  
  
    2.  在 **“筛选表行”** 页上选择筛选器窗格中的表，再单击 **“编辑”**。  
  
    3.  在 **“编辑筛选器”** 对话框中编辑现有筛选器。  
  
2.  **完成筛选语句以标识订阅服务器所要接收的表行**  
  
     定义新的筛选语句或编辑现有的筛选语句。 **“列”** 列表框列出了要从 **“选择要筛选的表”** 中选择的表中发布的所有列。 **“筛选语句”** 文本区域包括默认的文本，其格式为：  
  
     `SELECT <published_columns> FROM [schema].[tablename] WHERE`  
  
     此文本无法更改；请使用标准的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法在 WHERE 关键字后键入筛选子句。 如果发布服务器为 Oracle 发布服务器，则 WHERE 子句必须符合 Oracle 查询语法。 请尽可能避免使用复杂筛选器。 静态筛选器和参数化筛选器都会增加发布的处理时间，因此应使筛选语句尽可能简单。  
  
    > [!IMPORTANT]  
    >  为提高性能，建议您不要在合并发布的参数化行筛选子句中对列名应用函数,如 `LEFT([MyColumn]) = SUSER_SNAME()`。 如果在筛选子句中使用 HOST_NAME 并覆盖 HOST_NAME 值，则可能需要使用 CONVERT 转换数据类型。 有关此情况的最佳实践的详细信息，请参阅主题 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“覆盖 HOST_NAME() 值”一节。  
  
3.  **指定将从此表接收数据的订阅数**  
  
     仅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本适用；仅限合并复制。 通过合并复制，可以指定最适合您的数据和应用程序的分区类型。 如果选择 **“此表中的行将仅转到一个订阅”**，则合并复制将设置不重叠分区选项。 不重叠分区与预计算分区协同工作以提高性能，使用不重叠分区可以将与预计算分区相关联的上载开销降至最低。 使用的参数化筛选器和联接筛选器越复杂，不重叠分区的性能优势就越明显。 如果选择此选项，则必须确保对数据分区时不能将行复制到多个订阅服务器。 有关详细信息，请参阅主题 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“设置‘分区选项’”部分。  
  
 在添加或编辑完筛选器后，请单击 **“确定”** 以保存更改并关闭该对话框。 将对照 SELECT 子句中的表分析并运行指定的筛选器。 如果筛选语句有语法错误或其他问题，将会通知您编辑该筛选语句。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)   
 [Join Filters](../../relational-databases/replication/merge/join-filters.md)   
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
