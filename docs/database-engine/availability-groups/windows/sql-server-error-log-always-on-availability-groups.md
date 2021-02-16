---
title: SQL Server 错误日志（可用性组）
description: 了解影响 Always On 可用性组的 SQL Server 错误日志事件，以及需要查看错误日志的症状。
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 39d0c98d-75af-4dd1-b908-30d31af56f2a
author: rothja
ms.author: jroth
ms.openlocfilehash: 6356cc13f3b15a1a8991ec02c71dd01e74f97461
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342436"
---
# <a name="sql-server-error-log-always-on-availability-groups"></a>SQL Server 错误日志（Always On 可用性组）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  SQL Server 错误日志报告影响 Always On 可用性组的事件，如：  
  
-   与 Windows Server 故障转移群集 (WSFC) 群集的通信    
-   可用性副本的状态转换    
-   可用性数据库的状态转换    
-   主要副本和次要副本之间的可用性数据库的连接状态    
-   可用性组终结点的状态    
-   可用性组侦听程序的状态    
-   SQL Server 资源 DLL（在 WSFC 群集中运行）和 SQL Server 实例之间的租用状态（有关详细信息，请参阅 [How It Works:How It Works: SQL Server Always On lease timeout](/archive/blogs/psssql/how-it-works-sql-server-alwayson-lease-timeout)（工作原理：SQL Server Always On 租用超时））    
-   可用性组中的错误事件  

出现以下症状时应查看 SQL Server 错误日志：  

-   无法访问可用性数据库    
-   意外的可用性组故障转移    
-   可用性组意外处于“正在解决”状态    
-   可用性组处于不确定状态  
  
有关详细信息，请参阅[查看 SQL Server 错误日志 (SQL Server Management Studio)](~/relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md)。  
  
