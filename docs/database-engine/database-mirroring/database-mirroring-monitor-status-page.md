---
title: 数据库镜像监视器（“状态”页）| Microsoft Docs
description: 使用此只读“数据库镜像监视器”页查看所选数据库的主体和镜像服务器实例的镜像状态。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.status.f1
ms.assetid: 4f64b4e1-89e9-4827-98fa-b92c3dc73b48
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2d912623497cc6dc0baac1c4fbb4c8e794363ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353113"
---
# <a name="database-mirroring-monitor-status-page"></a>数据库镜像监视器（状态页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  该只读页面显示导航树中当前选定数据库的主体和镜像服务器实例的最新镜像状态。 如果有关某一实例的信息当前不可用，则 **“状态”** 网格与该实例对应的一些单元格将呈灰色并显示 **“未知”** 。  
  
 **使用 SQL Server Management Studio 监视数据库镜像**  
  
-   [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>选项  
 **Status**  
 显示包含每个主体和镜像服务器实例的最新高级镜像状态的网格。 **“状态”** 网格行的排列顺序如下：  
  
-   主体服务器实例  
  
-   镜像服务器实例  
  
 这些列如下所示：  
  
|列名称|说明|  
|-----------------|-----------------|  
|**服务器实例**|在 **“状态”** 行显示状态的服务器实例的名称。|  
|**当前角色**|服务器实例的当前角色，即 **“主体”** 或 **“镜像”** 。|  
|**镜像状态**|由服务器实例报告的镜像状态和指示状态严重性的图标。 可能的状态及其相关图标如下所示：<br /><br /> 图标：-，状态为“未知”。 监视器未与任一伙伴建立连接。 唯一可用的信息是监视器缓存的内容。<br /><br /> 图标：警告图标，状态为“正在同步”。 镜像数据库的内容滞后于主体数据库的内容。 主体服务器实例正在向镜像服务器实例发送日志记录，这会对镜像数据库应用更改，使其前滚。 在数据库镜像会话开始时，镜像数据库和主体数据库处于此状态。<br /><br /> 图标：标准数据库圆柱图，状态为“已同步”。 当镜像服务器与主体服务器几乎保持同步时，数据库状态将改为 **“已同步”** 。 只要主体服务器正在向镜像服务器发送更改，并且镜像服务器正在将更改应用于镜像数据库，数据库就会保持此状态。  对于高安全模式，自动故障转移和手动故障转移都可以使用，并且不会造成任何数据丢失。  对于高性能模式，可能总会有些数据丢失，即使在 **已同步** 状态中也是如此。<br /><br /> 图标：警告图标，状态为“已挂起”。 <br />                            主体数据库可用，但没有向镜像服务器发送任何日志。<br /><br /> 图标：错误图标，状态为“已断开连接”。 服务器实例无法与其伙伴建立连接。|  
|**见证服务器连接**|见证服务器的连接状态，前面有状态图标 **“未知”** 、 **“已连接”** 或 **“已断开连接”** 。|  
|**History**|单击以显示服务器实例上的镜像历史记录。 这将打开 **“数据库镜像历史记录”** 对话框，其中显示给定服务器实例中镜像状态的历史记录和镜像数据库的统计信息。<br /><br /> 如果监视器未与服务器实例建立连接， **“历史记录”** 按钮将呈灰色。|  
  
 主体日志 ( \<time> )  
 主体服务器实例上的日志状态，以服务器实例所在的当地时间为准，由 \<time> 指示。 将显示以下参数：  
  
 **未发送日志**  
 在发送队列中等待的日志量（以 KB 计）。  
  
 **最早的未发送事务**  
 发送队列中最早的未发送事务的保留时间。 事务保留时间指示在将事务发送到镜像服务器实例之前将其保留的分钟数。 该值有助于测量数据丢失的可能性（以时间计）。  
  
 **发送日志所需的时间(估计值)**  
 主体服务器实例将当前位于发送队列中的日志发送到镜像服务器实例所需的近似时间值（发送速率）。 由于传入事务的速率变化很大，因此发送日志的时间只是一个估计值。 但是，发送速率对于粗略估计手动故障转移所需的时间非常有用。  
  
 **当前发送速率**  
 将事务发送到镜像服务器实例的速率（以 KB/秒计）。  
  
 **新事务的当前速率**  
 传入事务进入主体日志的速率（以 KB/秒计）。 若要确定镜像是落后、保持同步、还是正在追赶，请将该值与 **“发送日志所需的时间(估计值)”** 值进行比较。  
  
 镜像日志 ( \<time> )  
 镜像服务器实例上的日志状态，以服务器实例所在的当地时间为准，由 \<time> 指示。 将显示以下参数：  
  
 **未还原日志**  
 在重做队列中等待的日志量（以 KB 计）。  
  
 **还原日志所需的时间(估计值)**  
 将当前在重做队列中的日志应用于镜像数据库所需的近似分钟数。  
  
 **当前还原速率**  
 将事务还原到镜像数据库的速率（以 KB/秒计）。  
  
 **镜像提交开销**  
 在主体服务器上生成警告之前，每个事务可允许的平均延迟的毫秒数。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。 该值只适用于高安全模式。  
  
 **发送和还原所有当前日志的所需时间(估计值)**  
 在当前时间下，发送和还原在主体服务器上已提交的所有日志所需的时间。 该时间可能小于“发送日志所需的时间(估计值)”和“还原日志所需的时间(估计值)”字段的总和，因为发送和还原可以并行执行。 该估计值可预测在处理发送队列中的积压事项时，发送和还原在主体服务器上提交的新事务所需的时间。  
  
 **见证服务器地址**  
 见证服务器实例的网络地址。 有关此地址格式的信息，请参阅[指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)。  
  
 **运行模式**  
 数据库镜像会话的运行模式：  
  
-   **高性能(异步)**  
  
-   **不带自动故障转移功能的高安全(同步)**  
  
-   **带自动故障转移功能的高安全(同步)**  
  
## <a name="remarks"></a>备注  
 **dbm_monitor** 固定数据库角色成员可以使用数据库镜像监视器或 **sp_dbmmonitorresults** 存储过程查看现有的镜像状态。 但是这些用户不能更新状态表。 它们依赖于“数据库镜像监视器作业” 来定期更新状态表。 若要了解所显示状态的保留时间，用户可以在“主体日志 (\<time>)”和“镜像日志 (\<time>)”标签上查看时间 。  
  
 如果该作业不存在或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理已停止，状态将变得越来越陈旧，并且可能不再反映镜像会话的配置。 例如，在一次故障转移之后，伙伴可能会分享相同的角色（主体或镜像）。或者，当前主体服务器可能显示为镜像，而当前的镜像服务器显示为主体。  
  
## <a name="see-also"></a>另请参阅  
 [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [启动配置数据库镜像安全向导 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
