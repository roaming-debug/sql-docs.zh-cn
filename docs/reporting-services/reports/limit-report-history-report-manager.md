---
title: 限制报表历史记录 - Reporting Services | Microsoft Docs
description: 了解如何为报表服务器配置报表历史记录。 还将了解如何为特定的报表配置报表历史记录。
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
helpviewer_keywords:
- viewing report history
- report history [Reporting Services], viewing history
- report history [Reporting Services], configuring history
- historical data [Reporting Services]
- displaying report history
ms.assetid: 8e255792-d9ef-496f-a26c-9e969c1209a0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d01a008e00d2effccb109555799bbe6d55baf1e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466568"
---
# <a name="limit-report-history---reporting-services"></a>限制报表历史记录 - Reporting Services
  报表历史记录是随着时间变化而创建的报表快照的集合。 您可以按需创建报表历史记录，也可以安排快照的创建频率以及将快照添加到报表历史记录中的频率。  
  
 报表历史记录存储在报表服务器数据库中。 如果报表快照中包含大量数据，则可考虑限制报表历史记录，尽可能地减少快照保持期对数据库大小的影响。  

::: moniker range="=sql-server-2016"
  
## <a name="to-configure-report-history-for-a-report-server"></a>为报表服务器配置报表历史记录  
  
1.  在报表管理器中，单击全局工具栏上的 **“站点设置”** 。  
  
2.  如果希望无限制地保留所有报表历史记录，请选择 **“不限制报表历史记录中保留的快照数”** 。 否则，请选择 **“限制报表历史记录的副本数”** ，以指定可以为任何给定报表保留的最多快照数。  
  
3.  单击“应用”  。  
  
## <a name="to-configure-report-history-for-a-specific-report"></a>为特定的报表配置报表历史记录  
  
1.  在报表管理器中，导航到要配置历史记录的报表，再单击该报表将其打开。  
  
2.  单击“属性”  选项卡。  
  
3.  单击 **“历史记录”** 选项卡。  
  
4.  为您的报表选择相应选项，再单击 **“应用”** 。 有关每个选项的详细信息，请参阅[“快照选项”属性页（报表管理器）](/previous-versions/sql/sql-server-2016/ms189952(v=sql.130))。  
  
## <a name="see-also"></a>另请参阅  
 [向报表历史记录添加快照（报表管理器）](../../reporting-services/report-server/add-a-snapshot-to-report-history-report-manager.md)   
 [报表管理器（SSRS 本机模式）](../web-portal-ssrs-native-mode.md)  

::: moniker-end

::: moniker range=">=sql-server-2017"

## <a name="to-configure-report-history-for-a-report-server"></a>为报表服务器配置报表历史记录  
  
1.  在 Web 门户中，单击全局工具栏上的“站点设置”  。  
  
2.  如果希望无限制地保留所有报表历史记录，请选择 **“不限制报表历史记录中保留的快照数”** 。 否则，请选择 **“限制报表历史记录的副本数”** ，以指定可以为任何给定报表保留的最多快照数。  
  
3.  单击“应用”  。  
  
## <a name="to-configure-report-history-for-a-specific-report"></a>为特定的报表配置报表历史记录  
  
1.  在 Web 门户中，导航到要配置历史记录的报表，再单击该报表将其打开。  
  
2.  单击“属性”  选项卡。  
  
3.  单击 **“历史记录”** 选项卡。  
  
4.  为您的报表选择相应选项，再单击 **“应用”** 。 要详细了解每个选项，请参阅[“快照选项”属性页](/previous-versions/sql/sql-server-2016/ms189952(v=sql.130))。  
  
## <a name="see-also"></a>另请参阅  
 [向报表历史记录添加快照](../../reporting-services/report-server/add-a-snapshot-to-report-history-report-manager.md)   

::: moniker-end
