---
title: 添加和删除发布项目 (SSMS)
description: 介绍如何使用 SQL Server Management Studio (SSMS) 将项目添加到发布中以及从发布中删除项目。
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- articles [SQL Server replication], dropping
- deleting articles
- dropping articles
- adding articles
- articles [SQL Server replication], adding
ms.assetid: d5a3e536-62d2-4473-a178-877ba52f7d7f
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 03440561eb766779309a868ac5caf7c49a885f73
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482988"
---
# <a name="add-articles-to-and-drop-articles-from-a-publication"></a>在发布中添加和删除项目
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  在新建发布向导中创建项目时，首次将其添加到发布中。 有关使用此向导的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
 创建发布后，在“发布属性 - \<Publication>”对话框的“项目”页面上添加和删除项目 。 有关访问此对话框的详细信息，请参阅 [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。 有关添加和删除项目的注意事项的信息，请参阅[向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
### <a name="to-add-an-article-after-a-publication-is-created"></a>创建发布后添加项目  
  
1.  在“发布属性 - \<Publication>”对话框的“项目”页面上，取消选中“仅在列表中显示已选中的对象”复选框  。 这样可以查看发布数据库中未发布的对象。  
  
2.  选中要添加的每个项目旁边的复选框。  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-delete-an-article"></a>删除项目  
  
1.  在“发布属性 - \<Publication>”对话框的“项目”页面上，取消选中要删除的每个项目旁边的复选框 。  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
