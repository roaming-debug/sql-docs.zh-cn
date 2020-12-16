---
description: Microsoft Replication Interactive Conflict Resolver
title: 交互式冲突解决程序（合并）
describes: Describes the Interactive Conflict Resolver that can be used for merge subscriptions that are synchronized using the Windows Synchronization Manager.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.replconflictviewer.interactiveresolver.f1
ms.assetid: d3d4a480-782b-4b1d-b839-565c8cf6cb24
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: a0cbd836bc0a7676128067f44e4079d4901de962
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464718"
---
# <a name="microsoft-replication-interactive-conflict-resolver"></a>Microsoft Replication Interactive Conflict Resolver
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Microsoft Replication Interactive Conflict Resolver 可以用于使用 Windows 同步管理器进行同步的合并订阅。 使用它可以查看、比较、编辑和选择数据冲突的结果。 复制还包括冲突查看器，使用冲突查看器可以提交冲突结果之后查看和修改冲突结果。 Microsoft Replication Interactive Conflict Resolver 允许您在同步期间选择结果。  
  
> [!NOTE]  
>  包含逻辑记录的冲突不会显示在交互式冲突解决程序中。 若要查看有关这些冲突的信息，请使用复制存储过程。 有关详细信息，请参阅[查看合并发布的冲突信息（复制 Transact-SQL 编程）](./view-and-resolve-data-conflicts-for-merge-publications.md)。  
  
## <a name="options"></a>选项  
 **列名**  
 表中所有列的名称。 一个或多个列可能包含冲突的数据。 不管哪些列发生冲突，整个入选行将覆盖整个落选行。  
  
 **建议的解决方法**  
 项目的冲突解决程序所提供的建议解决方法。  
  
 **发布者**  
 发布服务器中的数据值。  
  
 **订阅服务器**  
 订阅服务器中的数据值。  
  
 **“接受建议”**、 **“接受发布服务器”** 和 **“接受订阅服务器”**  
 单击此项可以接受将在发布服务器或订阅服务器上应用的行，具体取决于哪个服务器在冲突中落选。 如果发布服务器在冲突中落选，则其他所有订阅服务器将在下次与发布服务器同步时接收入选行。  
  
 **自动解决剩余冲突**  
 使用项目的冲突解决程序所提供的建议解决方法来解决所有剩余的冲突。  
  
 **记录此冲突的详细信息供今后参考**  
 在系统表中记录冲突的详细内容。  
  
## <a name="see-also"></a>另请参阅  
 [交互式冲突解决方法](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)   
 [查看和解决合并发布的数据冲突 (SQL Server Management Studio)](../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [使用 Windows 同步管理器同步订阅（Windows 同步管理器）](../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md)   
 [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
