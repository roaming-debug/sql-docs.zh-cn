---
title: 数据库镜像监视器（“警告”页）| Microsoft Docs
description: 在此只读数据库镜像监视器页面中查看数据库镜像事件所支持的警告以及警告阈值（如果有）。
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.warningsandalerts.f1
ms.assetid: 01936122-961d-436b-ba3c-5f79fefe5469
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 02acad8ee93c311c18c80e325450105ac47ee876
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642305"
---
# <a name="database-mirroring-monitor-warnings-page"></a>数据库镜像监视器（警告页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  显示数据库镜像事件所支持警告的只读列表和指定的警告阈值（如果有）。  
  
 **使用 SQL Server Management Studio 监视数据库镜像**  
  
-   [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="columns"></a>列  
 **警告**  
 可以定义阈值的警告包括：  
  
|警告|阈值|  
|-------------|---------------|  
|**如果未发送日志超出了阈值，则发出警告**|指定未发送日志达到多少 KB 后，会在主体服务器实例上生成一个警告。 该警告有助于测量数据丢失的可能性（以 KB 计），并且特别适用于高性能模式。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|  
|**如果未还原日志超出了阈值，则发出警告**|指定未还原日志达到多少 KB 后，会在镜像服务器实例上生成一个警告。 该警告可用于测量故障转移时间（以 KB 计）。 “故障转移时间 ”主要包括前一个镜像服务器前滚其重做队列中剩余的任意日志所需的时间，以及一小段额外时间。<br /><br /> 注意：对于自动故障转移，系统识别错误所需的时间与故障转移时间无关。<br /><br /> 有关详细信息，请参阅 [估计在角色切换期间服务的中断（数据库镜像）](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)。|  
|**如果最早的未发送事务的保留时间超出了阈值，则发出警告**|指定在主体服务器实例上生成警告之前，发送队列中可以累积的事务的分钟数。 该警告有助于测量数据丢失的可能性（以时间计），并且特别适用于高性能模式。 但是，当镜像因伙伴断开连接而暂停或挂起时，该警告也适用于高安全模式。|  
|**如果镜像提交开销超过了阈值则发出警告**|指定在主体服务器上生成警告之前，每个事务可允许的平均延迟的毫秒数。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。 该值只适用于高安全模式。|  
  
 **“** _<server_instance>_ **”处的阈值**  
 针对每个警告，显示一个服务器实例的当前的用户指定阈值（如果存在）。 该服务器实例的完整实例名在对应的列标题中指定。  
  
 有关详细信息，请参阅本主题后面的“备注”。  
  
 **设置阈值**  
 单击此按钮可为每个故障转移合作伙伴的一个警告设置阈值。  
  
 有关详细信息，请参阅本主题后面的“备注”。  
  
## <a name="remarks"></a>备注  
 如果服务器实例的信息目前不可用，则相应 **“阈值”** 列的单元格将显示灰色背景和水印文本。 如果监视器未与服务器实例连接，则网格将根据实例是默认实例还是命名实例，在每个单元格中显示“未连接到 _<SYSTEM_NAME>_ ”或“未连接到 _<SYSTEM_NAME>_ \\ _<instance_name>_ ”  。 如果监视器正在等待返回查询，那么每个单元格中的网格都将显示“等待数据...”。  
  
 当信息可用时，每个警告的单元格将会显示指定的阈值（和度量单位）或“未启用”。  
  
 如果在状态表刷新时超出阈值，则会在记录状态行时将一个事件记录到 Windows 事件日志中。 默认情况下，如果监视器未运行，则每分钟记录一次状态行。 可以使用 SQL Server 代理或其他程序（如 Microsoft Management Operations Manager (MOM)）对每种类型的记录事件配置警报。  
  
 在给定的伙伴上，记录的事件取决于它当前的角色，即主体或镜像。 但是，我们建议您在两个伙伴中都为给定的事件设置警告阈值，以确保数据库进行故障转移时警告仍然存在。 每个伙伴的相应阈值取决于伙伴系统的性能。  
  
> [!NOTE]  
>  也可以使用“sp_dbmmonitorchangealertt”系统存储过程来为等价的事件（如未发送日志、未恢复日志、最早的未发送事务和镜像提交开销）配置阈值。 有关详细信息，请参阅 [sp_dbmmonitorchangealert & #40;Transact SQL & #41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)。  
  
 下表显示与每个警告关联的事件 ID。  
  
|数据库镜像监视器警告|事件名称|事件 ID|  
|----------------------------------------|----------------|--------------|  
|**如果未发送日志超出了阈值，则发出警告**|未发送日志|32042|  
|**如果未还原日志超出了阈值，则发出警告**|未还原日志|32043|  
|**如果最早的未发送事务的保留时间超出了阈值，则发出警告**|最早的未发送事务|32044|  
|**如果镜像提交开销超过了阈值则发出警告**|镜像提交开销|32045|  
  
## <a name="permissions"></a>权限  
 若要拥有完全访问权限，需要具有 **sysadmin** 固定服务器角色的成员身份。 只有 **sysadmin** 的成员才可以配置和查看关键绩效指标的警告阈值。  
  
 如果是 **dbm_monitor** 角色中的成员，则只能查看“警告”页上最新的状态行。  
  
## <a name="see-also"></a>另请参阅  
 [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [启动配置数据库镜像安全向导 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
