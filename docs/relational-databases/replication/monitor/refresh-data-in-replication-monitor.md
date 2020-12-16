---
description: 刷新复制监视器中的数据
title: 刷新复制监视器中的数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- refreshing data
ms.assetid: e9582244-7d00-45f4-be16-020a65c76a5e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: adc29367fae4cd4e32a4cced3684f16771765c18
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432825"
---
# <a name="refresh-data-in-replication-monitor"></a>刷新复制监视器中的数据
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  在复制监视器中，可以自动或手动刷新主窗口和详细信息窗口（从主窗口启动的那些窗口）。 若要手动刷新窗口，请按 F5 键。 默认情况下，主窗口每五分钟自动刷新一次。每个发布服务器的刷新速率均可以自定义。  
  
 从缓存查询复制监视器中显示的数据。有关缓存和刷新复制监视器之间的关系的信息，请参阅[缓存、刷新和复制监视器性能](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md)。 有关启动复制监视器的信息，请参阅[启动复制监视器](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)。  
  
### <a name="to-set-refresh-options-for-replication-monitor"></a>设置复制监视器的刷新选项
  
1.  右键单击复制监视器左窗格中的发布服务器，然后单击 **“发布服务器设置”** 。  
  
2.  在 **“发布服务器设置”** 对话框中，设置 **“自动刷新”** 和 **“刷新速率”** 选项。 **“自动刷新”** 设置会影响复制监视器的主窗口。 **“刷新速率”** 设置也会影响任何设置为自动刷新的详细信息窗口，对该设置的更改仅影响以后打开的详细信息窗口。  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="to-specify-that-a-detail-window-should-automatically-refresh"></a>指定详细信息窗口应自动刷新  
  
1.  打开复制监视器中的详细信息窗口。 例如：  
  
    1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。  
  
    2.  单击 **“所有订阅”** 选项卡。  
  
    3.  右键单击订阅，然后单击 **“查看详细信息”** 。  
  
2.  在“订阅 \<SubscriptionName>”详细信息窗口中，单击“操作”，然后单击“自动刷新”  。 刷新速率由 **“发布服务器设置”** 对话框中的 **“刷新速率”** 设置决定。  
  
## <a name="see-also"></a>另请参阅  
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
