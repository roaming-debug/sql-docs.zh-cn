---
title: 配置预定义的复制警报 (SSMS)
description: 了解如何使用 SQL Server Management Studio (SSMS) 配置预定义的复制警报。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server replication]
- predefined replication alerts [SQL Server replication]
ms.assetid: c0414147-7ffe-4f9a-908c-71c1b5201584
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: fa85c871d8fd1cc8ade0ed0a1b0a41e8e596fee7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439868"
---
# <a name="configure-predefined-replication-alerts-sql-server-management-studio"></a>配置预定义的复制警报 (SQL Server Management Studio)
[!INCLUDE[applies-to-version/_ssnoversion.md](../../../includes/applies-to-version/sqlserver.md)]
  复制提供了下列预定义警报，可以配置这些警报以响应复制事件：  
  
-   **复制: 代理成功**  
  
-   **复制: 代理失败**  
  
-   **复制：代理重试**  
  
-   **复制：已删除过期的订阅**  
  
-   复制：验证失败后重新初始化了订阅  
  
-   复制：订阅服务器未通过数据验证  
  
-   复制：订阅服务器已通过数据验证  
  
-   复制：代理自定义关闭  
  
 在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 中的“警报”文件夹或复制监视器的“警告”选项卡中配置这些警报 。 有关访问此选项卡的详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
 除这些警报之外，复制监视器还提供了一组与状态和性能相关的警告和警报。 有关详细信息，请参阅 [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)。 您也可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 警报基础结构为其他复制事件定义警报。 有关详细信息，请参阅[创建用户定义事件](../../../ssms/agent/create-a-user-defined-event.md)。  
  
### <a name="to-configure-a-predefined-replication-alert-in-management-studio"></a>在 Management Studio 中配置预定义复制警报  
  
1.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中连接到分发服务器，然后展开服务器节点。  
  
2.  展开 “SQL Server 代理” 文件夹，然后展开 “警报” 文件夹。  
  
3.  右键单击一个复制警报，然后单击 “属性”。  
  
4.  在“\<AlertName> 警报属性”对话框中设置选项：  
  
    -   在 **“常规”** 页上，单击 **“启用”** ，指定应用此警报的数据库。  
  
    -   在 **“响应”** 页上，指定是否应发送电子邮件和/或是否应执行作业。  
  
         如果警报是“复制：订阅服务器未通过数据验证”，可以指定复制为此警报提供的响应作业：选择“执行作业”，再单击浏览按钮（“...”）。 在 **“定位作业”** 对话框中，单击 **“浏览”** 。 在 **“查找对象”** 对话框中，选择 **“重新初始化未通过数据验证的订阅”** 。 在两个打开的对话框中，单击 **“确定”** 。 作业执行时，它将把远程过程调用 (RPC) 用于重新初始化订阅的存储过程。 如果发布服务器使用远程分发服务器，您必须在发布服务器上定义远程服务器登录名，以便可以从分发服务器到发布服务器进行 RPC。  
  
    -   在 **“选项”** 页上，自定义响应文本。  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="to-configure-an-alert-for-a-threshold-in-replication-monitor"></a>在复制监视器中配置阈值的警报  
  
1.  在 **“警告”** 选项卡上，单击 **“配置警报”** 。  
  
2.  在 **“配置复制警报”** 对话框中，选择一个警报，然后单击 **“配置”** 。  
  
3.  在“\<AlertName> 警报属性”对话框中设置选项：  
  
    -   在 **“常规”** 页上，单击 **“启用”** ，指定应用此警报的数据库。  
  
    -   在 **“响应”** 页上，指定是否应发送电子邮件和/或是否应执行作业。  
  
         如果警报是“复制：订阅服务器未通过数据验证”，可以指定复制为此警报提供的响应作业：选择“执行作业”，再单击浏览按钮（“...”）。 在 **“定位作业”** 对话框中，单击 **“浏览”** 。 在 **“查找对象”** 对话框中，选择 **“重新初始化未通过数据验证的订阅”** 。 在两个打开的对话框中，单击 **“确定”** 。 作业执行时，它将把远程过程调用 (RPC) 用于重新初始化订阅的存储过程。 如果发布服务器使用远程分发服务器，您必须在发布服务器上定义远程服务器登录名，以便可以从分发服务器到发布服务器进行 RPC。  
  
    -   在 **“选项”** 页上，自定义响应文本。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  单击“关闭”。  
  
## <a name="see-also"></a>另请参阅  
 [对复制代理事件使用警报](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)  
  
