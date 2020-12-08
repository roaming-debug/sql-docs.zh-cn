---
title: SQL Server - FileTable 对象 | Microsoft Docs
description: 了解 SQLServer:FileTable 性能对象，该对象提供与 FileTable 和非事务性访问相关的统计信息的计数器。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:FileTable
ms.assetid: 325f5e58-1095-450f-9321-dfacfe6fd55f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 310bbb694eb3751d16543c5bc108ccdf2a880b02
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505705"
---
# <a name="sql-server-filetable-object"></a>SQL Server，FileTable 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
**SQLServer:FileTable** 性能对象提供与 FileTable 和非事务性访问相关的统计信息的计数器。

下表介绍了 SQL Server **FileTable** 性能对象。

|**SQL Server FileTable 计数器**|说明|  
|-------------|-----------------|  
|**删除 FileTable 项的平均时间**|删除一个 FileTable 项所花的平均时间（毫秒）。|
|**FileTable 枚举平均时间**|一个 FileTable 枚举请求所花的平均时间（毫秒）。|
|**终止 FileTable 句柄的平均时间**|终止一个 FileTable 句柄所花的平均时间（毫秒）。|
|**移动 FileTable 项的平均时间**|移动一个 FileTable 项所花的平均时间（毫秒）。|
|**每个文件 I/O 请求的平均时间**|处理一个传入的文件 I/O 请求所花的平均时间（毫秒）。|
|**每个文件 I/O 响应的平均时间**|处理一个传出的文件 I/O 响应所花的平均时间（毫秒）。|
|**重命名 FileTable 项的平均时间**|重命名一个 FileTable 项所花的平均时间（毫秒）。|
|**获取 FileTable 项的平均时间**|检索一个 FileTable 项所花的平均时间（毫秒）。|
|**更新 FileTable 项的平均时间**|更新一个 FileTable 项所花的平均时间（毫秒）。|
|**FileTable db 操作数/秒**|FileTable 存储组件每秒处理的数据库操作事件总数。|
|**FileTable 枚举请求/秒**|每秒 FileTable 枚举项请求总数。|
|**FileTable 文件 I/O 请求/秒**|每秒传入的 FileTable 文件 I/O 请求总数。|
|**FileTable 文件 I/O 响应/秒**|每秒传出的文件 I/O 响应的总数。|
|**FileTable 项删除请求/秒**|每秒 FileTable 删除项请求总数。|
|**FileTable 项获取请求/秒**|每秒 FileTable 检索项请求总数。|
|**FileTable 项移动请求/秒**|每秒 FileTable 移动项请求总数。|
|**FileTable 项重命名请求/秒**|每秒 FileTable 重命名项请求总数。|
|**FileTable 项更新请求/秒**|每秒 FileTable 更新项请求总数。|
|**FileTable 终止句柄操作/秒**|每秒 FileTable 句柄终止操作的总数。|
|**FileTable 表操作数/秒**|FileTable 存储组件每秒处理的表操作事件总数。|
|**删除 FileTable 项的基准时间**|仅供内部使用。|
|**FileTable 枚举基准时间**|仅供内部使用。|
|**终止 FileTable 句柄的基准时间**|仅供内部使用。|
|**移动 FileTable 项的基准时间**|仅供内部使用。|
|**每个文件 I/O 请求的基准时间**|仅供内部使用。|
|**每个文件 I/O 响应的基准时间**|仅供内部使用。|
|**重命名 FileTable 项的基准时间**|仅供内部使用。|
|**获取 FileTable 项的基准时间**|仅供内部使用。|
|**更新 FileTable 项的基准时间**|仅供内部使用。| 
 
## <a name="see-also"></a>另请参阅  
[监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)
