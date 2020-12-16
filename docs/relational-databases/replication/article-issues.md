---
description: 项目问题
title: 项目问题 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.articleissues.f1
ms.assetid: bde57da2-dd47-412f-9df7-9224968b2448
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 53df3848ffac7b7408d90b48d74a27410cd2a155
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467268"
---
# <a name="article-issues"></a>项目问题
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  **“项目问题”** 页列出了已发现的项目情况以及这些情况导致的所有必需更改。 下表列出了可能出现的问题以及确保复制和现有应用程序正常运行所必需的操作：  
  
|项目问题|详细信息|必需的操作|  
|-------------------|-------------|---------------------|  
|Uniqueidentifier 列将添加到表中。|对于允许更新订阅的合并发布或事务发布中的所有项目，复制需要数据类型为 **uniqueidentifier** 的列。|在生成第一个快照时，复制会自动将数据类型为 **uniqueidentifier** 的列添加到没有该列的已发布表中。 必须确保引用这些表的 INSERT 语句和 UPDATE 语句使用列列表。 还应确保磁盘上有足够的空间可用于其他列。|  
|IDENTITY 列需要 NOT FOR REPLICATION 选项。|复制要求所有的 IDENTITY 列都使用 NOT FOR REPLICATION 选项。 如果已发布的 IDENTITY 列未使用此选项，则 INSERT 命令可能无法正常复制。|适用于运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 及早期版本的发布服务器上所创建的发布。 必须为所有 IDENTITY 列指定 NOT FOR REPLICATION 属性。|  
|IDENTITY 属性未传输到订阅服务器。|此发布不允许在订阅服务器上进行更新。 在将 IDENTITY 列传输到订阅服务器时，将不会传输 IDENTITY 属性。 例如，在发布服务器上定义为 INT IDENTITY 的列，在订阅服务器上将定义为 INT。|适用于运行 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 及早期版本的发布服务器上所创建的发布。 无需执行任何操作。|  
|要求视图所引用的表。|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要求已发布的视图和索引视图引用的所有表在订阅服务器上均应可用。 如果没有将被引用表作为此发布中的项目发布，则必须在订阅服务器上手动创建这些对象。|使用 **“上一步”** 按钮导航到 **“项目”** 页。 添加所需的所有对象。|  
|要求存储过程引用的对象。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要求已发布存储过程引用的所有对象（例如，表和用户定义函数）在订阅服务器上均应可用。 如果没有将被引用的对象发布为该发布中的项目，则必须在订阅服务器上手动创建这些对象。|使用 **“上一步”** 按钮导航到 **“项目”** 页。 添加所需的所有对象。|  
  
## <a name="see-also"></a>另请参阅  
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [创建发布](../../relational-databases/replication/publish/create-a-publication.md)  
  
  
