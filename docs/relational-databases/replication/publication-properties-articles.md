---
description: 发布属性，项目
title: 发布属性，项目 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.pubproperties.articles.f1
ms.assetid: bdeea318-a153-44b8-9e51-9155f3bad18b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 4dd8faada9a1290595ed03b4b863a75fe02fbec3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473198"
---
# <a name="publication-properties-articles"></a>发布属性，项目
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  **“发布属性”** 对话框的 **“项目”** 页包含与发布中所包含项目有关的信息。使用该页，可以将项目添加到现有发布或从现有发布删除项目；并允许您更改项目属性和列筛选。  
  
> [!NOTE]  
>  创建发布之后，某些属性更改要求新的快照。 如果发布具有多个订阅，某些更改还会要求重新初始化所有订阅。 有关详细信息，请参阅[更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)和[向现有发布添加项目和从中删除项目](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
 如果发布的数据库对象依赖于一个或多个其他数据库对象，则必须发布所有被引用对象。 例如，如果要发布的视图依赖于一个表，则也必须发布该表。  
  
 无法发布的对象旁边有一个红色图标，并在向导页底部的信息面板中附有说明。 无法发布下列对象：  
  
-   加密的对象。  
  
-   包含允许 Null 的列的索引视图。  
  
-   无法在事务发布中发布没有主键的表。  
  
-   在为排队更新订阅启用的合并发布和事务发布中，无法发布表。 有关在多个发布中发布项目的详细信息，请参阅[发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)中的“在多个发布中发布表”部分。  
  
## <a name="oracle-publishers"></a>Oracle 发布服务器  
 Oracle 发布服务器的其他注意事项：  
  
-   有关 Oracle 发布服务器可以发布的对象的列表，请参阅 [Design Considerations and Limitations for Oracle Publishers](../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md)。 无法发布的对象不会显示。  
  
-   有关可以发布的数据类型的列表，请参阅 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)。 带有无法发布的数据类型的列不会显示。  
  
## <a name="column-filters"></a>列筛选器  
 通过展开 **“要发布的对象”** 窗格中的表，然后只选择需要的列，可以对此页上的列进行筛选（可以在此向导的 **“筛选表行”** 页上筛选行）。 由于包括安全（防止复制敏感数据）和性能（例如，避免复制较大的二进制大型对象 (BLOB) 列）在内的很多原因，筛选列非常有用。 有关列筛选（包括无法筛选的列类型的列表）的详细信息，请参阅[筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)。  
  
## <a name="options"></a>选项  
 使用 **“要发布的对象”** 窗格，可以：  
  
-   查看所有可用于复制的对象。  
  
-   通过选中该对象旁边的复选框，可以将项目添加到发布。  
  
-   通过清除该对象旁边的复选框，可以从发布删除项目。 有关何时可以删除项目的信息，请参阅[向现有发布添加项目和从中删除项目](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
-   通过选中对象类型（如 **“表”** ）旁边的复选框，将特定类型（如表）的所有对象包括在发布中。  
  
-   展开表节点可以查看表中的列。  
  
-   通过清除列旁边的复选框来从发布中筛选表列，或通过选中该复选框来包含未发布的列。  
  
-   右键单击窗格中的对象可以查看该对象的命令菜单。  
  
 **项目属性**  
 单击 **“项目属性”** ，再单击下列选项之一：  
  
-   单击“设置突出显示的 \<ObjectType> 的属性”以启动“项目属性 - \<ObjectName>”对话框；在此对话框中进行的属性更改仅应用于在“项目”页上的对象窗格中突出显示的对象  。  
  
-   单击“设置所有 \<ObjectType> 项目的属性”以启动“所有 \<ObjectType> 项目的属性”对话框；在此对话框中进行的属性更改应用于“项目”页上的对象窗格中该类型的所有对象，包括尚未选择进行发布的对象。    
  
    > [!NOTE]  
    >  在“所有 \<ObjectType> 项目的属性”对话框中进行的属性更改会替代之前在“项目属性 - \<ObjectName>”对话框中进行的任何更改。  例如，若要为某对象类型的所有项目设置一些默认值，但还希望为单个对象设置一些属性，请首先设置所有项目的默认值。 然后再设置单个对象的属性。  
  
 **已选中的表仅用于下载**  
 仅限合并复制。 仅限 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本。 选择此项可以指定在使用客户端订阅后不允许在订阅服务器上进行更改。 因为仅供下载的项目不能在订阅服务器上更新，所以跟踪元数据不会发送到订阅服务器。 这可以减少订阅服务器上的存储量并提高性能，特别是当网络连接较慢时。 此选项对应于 **“项目属性”** 对话框中的选项 **“同步方向”** 的值 **“仅下载到订阅服务器，禁止订阅服务器更改”** 。 有关详细信息，请参阅[使用仅下载项目优化合并复制性能](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)。  
  
 **仅显示列表中已选中的对象**  
 选中此复选框可以只显示对象窗格中选定的项目。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [重新初始化订阅](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
