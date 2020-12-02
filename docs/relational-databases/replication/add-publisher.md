---
description: 添加发布服务器
title: 添加发布服务器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.addpublisher.f1
helpviewer_keywords:
- Add Publisher dialog box
ms.assetid: 4b57e298-655f-42c2-82bc-25cdad94a194
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 1ce82548d63c88565a48d3930c3a7b9a40884413
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127834"
---
# <a name="add-publisher"></a>添加发布服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  可以使用 **“添加发布服务器”** 对话框，将一个或多个发布服务器添加到复制监视器的左窗格中。 添加发布服务器之后，如果在左窗格中选择发布服务器，将在右窗格中显示该发布服务器的有关信息。  
  
## <a name="options"></a>选项  
 **添加**  
 单击可选择要添加的发布服务器的类型，单击后将启动 **“连接到服务器”** 对话框。 选项包括：  
  
-   **添加 SQL Server 发布服务器...**  
  
     使用 **“连接到服务器”** 对话框连接到发布服务器。  
  
-   **添加 Oracle 发布服务器...**  
  
     使用“连接到服务器”对话框连接到与 Oracle 发布服务器关联的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分发服务器。  
  
-   **指定分发服务器并添加其发布服务器...**  
  
     使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] “连接到服务器” **对话框连接到与一个或多个发布服务器关联的** 分发服务器。  
  
 成功连接到发布服务器或分发服务器之后，将在对话框顶部的网格中显示每个发布服务器及其分发服务器的名称。  
  
> [!NOTE]  
>  分发服务器和发布服务器通常运行在同一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上，但分发服务器可以运行在另一个实例上（此配置称为远程分发服务器）。  
  
 **删除**  
 通过在对话框顶部的网格中选择发布服务器并单击 **“删除”** ，可以从要添加的发布服务器的列表中删除发布服务器。  
  
> [!NOTE]  
>  此按钮不能用来删除已经显示在复制监视器中的发布服务器。 若要删除已经显示的发布服务器，请在复制监视器的左窗格中右键单击该发布服务器，再单击 **“删除”**。  
  
 **复制监视器启动时自动连接**  
 选择此项可以使复制监视器自动连接到分发服务器，并检索在该对话框顶部网格中所选发布服务器的状态信息。 如果清除了此复选框，则必须在启动复制监视器后手动进行连接：在复制监视器左窗格中右键单击发布服务器，然后单击 **“连接”**。  
  
 **自动刷新此发布服务器及其发布的状态**  
 选择此项可以使复制监视器自动刷新在该对话框顶部网格中所选发布服务器的状态。 如果选择了此选项，复制监视器将轮询分发服务器，以获取发布服务器及其发布的状态信息。 轮询间隔是由 **“刷新速率”** 选项设置的。 有关在复制监视器中刷新的详细信息，请参阅[缓存、刷新和复制监视器性能](../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md)。  
  
 **“刷新速率”**  
 输入一个值（秒），指定复制监视器对分发服务器的轮询频率（以了解状态信息）。 值越小表示轮询越频繁，如果监视大量的发布服务器，则可能会影响分发服务器的性能。 建议您测试系统，以确定一个适当的值。 如果在复制监视器的任意详细信息窗口中选择 **“自动刷新”** ，则也将使用 **“刷新速率”** 设置。  
  
 **在以下组中显示此发布服务器**  
 从该列表中选择某个发布服务器组。 该发布服务器显示在左窗格中此组下面。 通过组可以组织发布服务器，而且对复制的功能没有任何影响。 如果没有定义任何组或者希望创建新组，请单击 **“新建组”**。  
  
 **新建组**  
 单击此项可创建新的发布服务器组。 发布服务器组提供了在复制监视器内组织发布服务器的简便方法。 组既不影响数据的复制，也不影响复制拓扑中服务器之间的关系。  
  
## <a name="see-also"></a>另请参阅  
 [启动复制监视器](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [监视复制](../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
