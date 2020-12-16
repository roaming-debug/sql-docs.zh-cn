---
title: 复制冲突查看器（对等）
description: 了解复制冲突查看器和如何使用它来查看对等事务复制和具有排队更新订阅的事务复制的冲突。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.replconflictviewer.cvqueued.f1
ms.assetid: eec59d8e-cadb-4623-a31f-9f42ec09c97f
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: f18d7b665d9e546054fccf681e2e78f4f9fbf963
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481028"
---
# <a name="replication-conflict-viewer-transactional-replication"></a>复制冲突查看器（事务复制）
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  利用复制冲突查看器，您可以查看同步期间对等事务复制和具有排队更新订阅的事务复制发生的冲突。 有关详细信息，请参阅[查看事务发布的数据冲突 (SQL Server Management Studio)](../../relational-databases/replication/view-data-conflicts-for-transactional-publications-sql-server-management-studio.md)。  
  
> [!NOTE]  
>  复制冲突查看器显示在合并复制和事务复制中发生的冲突。 对于事务复制，可以使用复制冲突查看器查看冲突数据，但无法为冲突选择不同的解决方法。  
  
## <a name="options"></a>选项  
 复制冲突查看器划分为两个部分。 对话框的上半部分显示所选表的冲突列表。 单击冲突列表中的项时，将在对话框的下半部分显示该冲突的详细信息。  
  
 在下半部分中，冲突数据显示在两个相对应的列（ **“冲突解决入选方”** 和 **“冲突解决落选方”** ）中。 如果冲突发生在更新的数据和删除的数据之间，则对于冲突中删除的一方来说，可能没有可显示的数据。 在这种情况下，复制冲突查看器会在其中一列中显示一条消息，指示在一个位置删除了该行，在另一个位置更新了该行。 此外，它还会指出建议的解决方法。  
  
 **Database**  
 选择包含具有冲突的发布的数据库。  
  
 **发布**  
 选择包含具有冲突的表的发布。  
  
 **表**  
 选择包含冲突的表。  
  
 **定义筛选器**  
 单击此项可打开 **“定义筛选器”** 对话框。  
  
 **应用或删除筛选器**  
 单击此项可应用或删除在 **“定义筛选器”** 对话框中定义的筛选器。  
  
 **全选**  
 单击此项可选择网格中列出的所有冲突。  
  
 **全部不选**  
 单击此项可取消选择网格中列出的所有冲突。  
  
 **删除**  
 单击此项可从查看器中删除所选冲突，并从复制系统表中删除与冲突关联的元数据。  
  
 **显示所有列**  
 选择此选项可显示表中的所有列。  
  
 **显示前五列以及包含冲突数据的其他列**  
 选择此选项可显示前五列以及所有包含冲突的列。 当表包含很多列，而您只想查看与解决冲突最相关的列时，这非常有用。 前五列始终包含在此视图中，因为标识行的字段（如主键或名称字段）通常位于表的前几列中。  
  
 **显示列信息** ( **…** )  
 单击此项可查看列信息： **“表名”** 、 **“列名”** 、 **“数据类型”** 和 **“列值”** 。  
  
 **记录冲突详细信息**  
 选中此框可将冲突的详细信息记录到文件。 若要指定文件的位置，请指向 **“视图”** 菜单，再单击 **“选项”** 。 输入一个值，或单击 **“浏览(...)”** ，然后导航到相应的文件。 单击 **“确定”** 可退出 **“选项”** 对话框。  
  
## <a name="see-also"></a>另请参阅  
 [对等复制中的冲突检测](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)   
 [查看事务发布的数据冲突 &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-data-conflicts-for-transactional-publications-sql-server-management-studio.md)  
  
  
