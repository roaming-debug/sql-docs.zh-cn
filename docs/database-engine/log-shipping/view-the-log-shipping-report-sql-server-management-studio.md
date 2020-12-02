---
title: 查看日志传送报告 (SSMS)
description: 查看 SQL Server Management Studio 中的事务日志传送状态报告。 在监视服务器、主服务器或辅助服务器中运行状态报告。
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- viewing log shipping reports
- displaying log shipping reports
- log shipping [SQL Server], monitoring
- log shipping [SQL Server], viewing reports
ms.assetid: 3b549f2f-3683-45e5-b8e8-8095276c41ab
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4ee1de4f9d231c2fa3be91cab2010eb1aad358f8
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130736"
---
# <a name="view-the-log-shipping-report-sql-server-management-studio"></a>查看日志传送报告 (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中查看事务日志传送状态报告。 您可以在监视服务器、主服务器或辅助服务器中运行状态报告。 若要查看有关日志传送配置的最完整的信息，请在监视服务器实例上查看此报告。  
  
 该报告显示所有其状态可从您连接到的服务器实例获得的日志传送活动的状态。 如果该服务器实例涉及不同角色（例如，用作一个数据库的监视服务器，同时用作另一个数据库的辅助服务器）的多种配置，则显示的结果包含每个角色的各项配置信息。 如果存储过程能够针对给定的日志传送配置连接到监视服务器实例，则报告将显示此配置的其他状态。  
  
 对于当前服务器实例执行的每个角色，您都可以查看以下信息：  
  
|角色|显示的信息|  
|----------|---------------------------|  
|监视|将此服务器实例用作其监视服务器的每台主服务器和辅助服务器的名称和状态。|  
|主|对于每个主数据库，当前服务器实例（作为主服务器）的状态和名称以及主数据库名称。 报告将显示备份作业（存储在本地主服务器上）的状态。<br /><br /> 报告还包含用于显示每台对应辅助服务器的行。 如果配置使用监视服务器且存储过程可以连接到此监视服务器，这些行将显示最近日志备份的复制状态和还原状态。|  
|辅助副本|对于每个辅助数据库，当前服务器实例（作为辅助服务器）的状态和名称以及辅助数据库名称。<br /><br /> 报告将显示辅助服务器上复制和还原作业的状态。<br /><br /> 报告还包含用于显示对应主服务器的行。 如果配置使用监视服务器且存储过程可以连接到此监视服务器，此行将显示最近日志备份的状态。|  
  
 所显示的信息取决于服务器实例是监视服务器、主服务器还是辅助服务器。 如果没有提供信息，则对应单元格显示为灰色。  
  
 此报告调用 **sp_help_log_shipping_monitor** 来获取数据。 有关所需权限的信息，请参阅 [sp_help_log_shipping_monitor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-transact-sql.md)。  
  
### <a name="to-display-the-transaction-log-shipping-status-report-on-a-server-instance"></a>显示服务器实例上的事务日志传送状态报告  
  
1.  连接到监视服务器、主服务器或辅助服务器。  
  
2.  在对象资源管理器中，右键单击服务器实例，依次指向“报表”和“标准报表”。  
  
3.  单击 **“事务日志传送状态”** 。  
  
## <a name="see-also"></a>另请参阅  
 [监视日志传送 (Transact-SQL)](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
  
