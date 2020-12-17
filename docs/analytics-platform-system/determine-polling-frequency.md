---
title: 确定轮询频率
description: 本文介绍如何确定分析平台系统设备警报的轮询频率。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: ee5c83fee1028b7e165db6dfb8015129c29e4eca
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641474"
---
# <a name="determine-polling-frequency"></a>确定轮询频率
本文介绍如何确定分析平台系统设备警报的轮询频率。  
  
## <a name="to-determine-the-polling-frequency"></a>确定轮询频率  
由于在出现警报时，PDW 当前不支持主动通知，因此监视解决方案需要持续轮询设备 Dll。  在内部，PDW 会按不同的时间间隔轮询组件：  
  
-   群集-60 秒  
  
-   检测信号-60 秒  
  
-   所有其他组件-5 分钟  
  
-   性能计数器-三秒  
  
轮询警报的常见时间间隔，也由 System Center 使用， **每15分钟一次**。  很显然，您可以查询更多或更少，但不建议每六个小时轮询一次。  
  
更频繁的轮询是可接受的，但轮询过于频繁可能会打乱 [Sys.dm_pdw_nodes_exec_requests](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) DMV。  轮询太频繁会使用户难以在快速退出视图时诊断查询性能问题。  
  
## <a name="see-also"></a>另请参阅  
<!-- MISSING LINKS [Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
[设备监视 &#40;分析平台系统&#41;](appliance-monitoring.md)  
