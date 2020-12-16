---
description: 筛选表行
title: 筛选表行 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.filtertablerows.f1
ms.assetid: 005f5c71-0401-490e-8823-adc54a2e9675
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: f92cdb60988e2b33a5de8382352609b5c25c466b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464738"
---
# <a name="filter-table-rows"></a>筛选表行
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  在 **“筛选表行”** 页中，您可以执行以下操作：  
  
-   将静态行筛选器应用于快照发布、事务发布和合并发布中的表项目。  
  
-   将参数化行筛选器应用于合并发布中的表项目。  
  
-   使用联接筛选器将合并表项目的筛选器扩展到相关表项目。  
  
 有关筛选选项的详细信息，请参阅[筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)。 可以在 **“发布属性”** 对话框的 **“筛选行”** 页中更改筛选。  
  
 为了获得最佳的应用程序性能并减少所需的远程存储量，或者要限定某些数据仅供特定的订阅服务器使用，您应该只发布所需数据。 发布中既可以包含未筛选的表，也可以包含已筛选的表。 例如，可以包含公司产品的完整（未筛选）表，然后使用行筛选器提供一个仅包含特定区域客户的筛选表。 通过筛选已发布数据，可以：  
  
-   最大程度地减少通过网络发送的数据量。  
  
-   减少订阅服务器上需要的存储空间量。  
  
-   根据各个订阅服务器的要求，自定义发布和应用程序。  
  
-   由于可以将不同的数据分区发送到不同的订阅服务器（没有两个订阅服务器会同时更新相同的数据值），因此可以避免或减少订阅服务器更新数据时的冲突。  
  
-   避免传输敏感数据。 行筛选器和列筛选器可以用于限制订阅服务器对数据的访问。 对于合并复制，如果使用包括 HOST_NAME() 的参数化筛选器，则需要考虑安全问题。 有关详细信息，请参阅 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“使用 HOST_NAME() 进行筛选”部分。  
  
 筛选器不能包含复制所用的 **rowguidcol** 来标识行。 默认情况下，这是您设置合并复制时添加的列，命名为 **rowguid**。  
  
## <a name="options"></a>选项  
 **筛选的表**  
 此窗格使用您向发布中的表项目添加的筛选器进行填充。 带行筛选器的表在窗格中显示为顶级节点。 对于合并发布，筛选操作通过联接筛选器扩展到的表显示为子节点。  
  
 **添加**  
 单击 **“添加”** 可以启动一个用于对表项目进行筛选的对话框。 对于快照发布或事务发布，单击 **“添加”** 将立即启动对话框。 对于合并发布，单击“添加”将会显示三个选项 ：“添加筛选器”、“添加联接以扩展所选筛选器”和“自动生成筛选器”  。  
  
-   选择 **“添加筛选器”** 将启动 **“添加筛选器”** 对话框。 使用此对话框可以将行筛选器应用于表项目。 例如，在 **“添加筛选器”** 对话框中，可以指定在将包含客户数据的表复制到订阅服务器时，该表应只包含法国客户的相关数据。  
  
-   选择 **“添加联接以扩展所选筛选器”** 将启动 **“添加联接”** 对话框。 使用 **“添加联接”** 对话框，可以对行筛选器进行扩展，这样就可以筛选与行筛选器所在表相关的表中的数据。 例如，如果筛选一个客户表以便它仅包含法国客户的相关数据，并且还有一个与客户订单相关的表，则可以在两个表之间定义一个联接，以便该订单表仅包括来自法国客户的订单。  
  
    > [!NOTE]  
    >  只有在筛选器窗格中选择了联接的基表后，此选项才可用。  
  
-   选择 **“自动生成筛选器”** 将启动 **“生成筛选器”** 对话框。 使用此对话框可以为合并发布中的某个表定义行筛选器，复制会自动将该行筛选器扩展到通过外键关系相关联的其他表。 例如，一个发布可以包括三个表：客户表、订单表（带有指向客户表的外键）和订单详细信息表（带有指向订单表的外键）。 为客户表定义行筛选器后，复制会将该行筛选器扩展到其他表。  
  
    > [!NOTE]  
    >  当通过复制自动生成筛选器时，会删除发布中的所有现有筛选器。 若要同时包括自动生成的筛选器和手动指定的筛选器，请首先生成筛选器。 对于每个发布，只能指定一组自动生成的筛选器。  
  
 **编辑**  
 在筛选器窗格中选择一个行筛选器或联接筛选器，然后单击 **“编辑”** 以启动 **“编辑筛选器”** 或 **“编辑联接”** 对话框。  
  
 **删除**  
 在筛选器窗格中选择一个行筛选器或联接筛选器，然后单击 **“删除”** 以删除该筛选器。  
  
 **查找表**  
 仅将发布与联接筛选器合并。 单击 **“查找表”** 可以在复杂的筛选器树中查找表。 在关系复杂的数据库中，一个表可以联接到多个表，因此可能出现在筛选器树中的多个位置。  
  
 实际的表只显示在树中的一个位置，该表在其他位置使用快捷方式来表示。 表的快捷方式只是对该表的引用；它不显示该表的子节点。 快捷方式节点标记有快捷方式箭头，展开该节点将显示“单击‘查找表’可查看 \<tablename> 表”文本。  
  
 选择窗格中的快捷方式节点，并单击 **“查找表”** 。 窗格随即展开，并突出显示所查找的表。 如果单击 **“查找表”** 而没有选定快捷方式节点，将会启动 **“查找表”** 对话框。  
  
 **筛选器**  
 包含筛选器窗格中选定筛选器的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 定义。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)   
 [Join Filters](../../relational-databases/replication/merge/join-filters.md)   
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
