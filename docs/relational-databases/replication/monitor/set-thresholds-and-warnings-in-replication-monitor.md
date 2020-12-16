---
title: 设置阈值和警告（复制监视器）
description: 了解如何在 SQL Server Management Studio (SSMS) 中使用复制监视器对复制可能出现的各种情况启用警告。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server replication]
- Merge Agent, thresholds and warnings
- Distribution Agent, thresholds and warnings
- thresholds [SQL Server replication]
- Replication Monitor, thresholds and warnings
- monitoring performance [SQL Server replication], thresholds and warnings
ms.assetid: 3a409c2c-b77e-4001-b81a-1dcd918618ec
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 3c738370cc02b3ca36123a5e6232ec04b9a4e882
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432927"
---
# <a name="set-thresholds-and-warnings-in-replication-monitor"></a>在复制监视器中设置阈值和警告
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制监视器显示发布和订阅的状态信息。 默认情况下，复制监视器只为未初始化的订阅显示警告，但是，您可以为其他情况启用警告。 建议您对拓扑启用警告，以便及时获悉有关状态和性能的信息。  
  
 启用警告时，需要指定阈值。 达到或超过该阈值时，将显示警告（除非需要显示更高优先级的问题）。 除了在复制监视器中显示警告之外，达到阈值也可以触发警报。 您可以为下列情况启用警告：  
  
-   订阅即将过期  
  
     这种情况适用于所有类型的复制。 如果达到或超过指定的阈值，订阅状态会显示为 **“即将过期/已过期”** 。  
  
-   超过指定的滞后时间（从事务在发布服务器上提交到相应的事务在订阅服务器上提交之间间隔的时间）。  
  
     这适用于事务复制。 如果达到或超过指定的阈值，订阅状态便会显示为 **“‘严重’状态下的性能”** 。  
  
-   超出了指定的同步时间。  
  
     这适用于合并复制。 如果达到或超过指定的阈值，状态将显示为 **“长时间运行的合并”** 。 您可以为拨号连接和局域网 (LAN) 连接指定不同的阈值。  
  
-   在给定时间内未处理完指定的行数。  
  
     这适用于合并复制。 如果达到或超过指定的阈值，状态将显示为 **“‘严重’状态下的性能”** 。 可以为拨号连接和 LAN 连接指定不同的阈值。  
  
 有关“‘严重’状态下的性能”和“长时间运行的合并”   警告的详细信息，请参阅[使用复制监视器监视性能](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)。  
  
 **本主题内容**  
  
-   [为事务发布设置阈值和警告](#Transactional)  
  
-   [为合并发布设置阈值和警告](#Merge)  
  
-   [为快照发布设置阈值和警告](#Snapshot)  
  
##  <a name="to-set-thresholds-and-warnings-for-a-transactional-publication"></a><a name="Transactional"></a> 为事务发布设置阈值和警告  
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。  
  
2.  单击 **“警告”** 选项卡。若要查看有关此选项卡上的选项的详细信息，请在菜单栏上单击 **“帮助”** 。  
  
3.  通过选中相应的复选框来启用警告：“如果订阅将在阈值内过期，则发出警告”或“如果滞后时间超出阈值，则发出警告”   。  
  
4.  在 **“阈值”** 列中，为警告设置阈值。 例如，如果在步骤 3 中选中了 **“如果滞后时间超出阈值，则发出警告”** ，就可以在 **“阈值”** 列中，选择滞后时间 **“60 秒”** 。  
  
5.  单击 **“保存更改”** 。  
  
#### <a name="to-configure-an-alert-for-a-threshold"></a>为阈值配置警报  
  
1.  单击 **“配置警报”** 。  
  
2.  在 **“配置复制警报”** 对话框中，选择一个警报，然后单击 **“配置”** 。  
  
     此对话框将显示所有发布类型的警报，包括与监视阈值无关的警报。 有关详细信息，请参阅[对复制代理事件使用警报](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)。  
  
3.  在“\<AlertName> 警报属性”对话框中设置选项：  
  
    -   在 **“常规”** 页上，单击 **“启用”** ，指定应用此警报的数据库。  
  
    -   在 **“响应”** 页上，指定是否应发送电子邮件和/或是否应执行作业。  
  
    -   在 **“选项”** 页上，自定义响应文本。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  单击“关闭”  。  
  
##  <a name="set-thresholds-and-warnings-for-a-merge-publication"></a><a name="Merge"></a> 为合并发布设置阈值和警告  
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。  
  
2.  单击 **“警告”** 选项卡。若要查看有关此选项卡上的选项的详细信息，请在菜单栏上单击 **“帮助”** 。  
  
3.  通过选中相应的复选框来启用警告：  
  
    -   **“如果订阅将在阈值内过期，则发出警告”**  
  
    -   **如果拨号连接的合并长度超出阈值，则发出警告**  
  
    -   **如果 LAN 连接的合并长度超出阈值，则发出警告**  
  
    -   **如果 LAN 连接每秒合并的行数小于阈值，则发出警告**  
  
    -   **如果拨号连接每秒合并的行数小于阈值，则发出警告**  
  
4.  在 **“阈值”** 列中设置警告阈值。 例如，如果在步骤 3 中选择了 **“如果拨号连接的合并长度超出阈值，则发出警告”** ，则可以在 **“阈值”** 列中选择时间 **“10 分钟”** 。  
  
5.  单击 **“保存更改”** 。  
  
#### <a name="to-configure-an-alert-for-a-threshold"></a>为阈值配置警报  
  
1.  单击 **“配置警报”** 。  
  
2.  在 **“配置复制警报”** 对话框中，选择一个警报，然后单击 **“配置”** 。  
  
     此对话框将显示所有发布类型的警报，包括与监视阈值无关的警报。  
  
3.  在“\<AlertName> 警报属性”对话框中设置选项：  
  
    -   在 **“常规”** 页上，单击 **“启用”** ，指定应用此警报的数据库。  
  
    -   在 **“响应”** 页上，指定是否应发送电子邮件和/或是否应执行作业。  
  
    -   在 **“选项”** 页上，自定义响应文本。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  单击“关闭”  。  
  
##  <a name="set-thresholds-and-warnings-for-a-snapshot-publication"></a><a name="Snapshot"></a> 为快照发布设置阈值和警告  
  
1.  在左窗格中展开发布服务器组，再展开其中的一个发布服务器，然后单击其中的一个发布。  
  
2.  单击 **“警告”** 选项卡。若要查看有关此选项卡上的选项的详细信息，请在顶部菜单上单击 **“帮助”** 。  
  
3.  通过选中 **“如果订阅将在阈值内过期，则发出警告”** 复选框来启用警告。  
  
4.  在 **“阈值”** 列中，为警告设置阈值。 例如，可以在 **“阈值”** 列中选择值 **70%** 。  
  
5.  单击 **“保存更改”** 。  
  
#### <a name="to-configure-an-alert-for-a-threshold"></a>为阈值配置警报  
  
1.  单击 **“配置警报”** 。  
  
2.  在 **“配置复制警报”** 对话框中，选择一个警报，然后单击 **“配置”** 。  
  
     此对话框将显示所有发布类型的警报，包括与监视阈值无关的警报。 有关详细信息，请参阅[对复制代理事件使用警报](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)。  
  
3.  在“\<AlertName> 警报属性”对话框中设置选项：  
  
    -   在 **“常规”** 页上，单击 **“启用”** ，指定应用此警报的数据库。  
  
    -   在 **“响应”** 页上，指定是否应发送电子邮件和/或是否应执行作业。  
  
    -   在 **“选项”** 页上，自定义响应文本。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  单击“关闭”  。  
  
## <a name="see-also"></a>另请参阅  
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
