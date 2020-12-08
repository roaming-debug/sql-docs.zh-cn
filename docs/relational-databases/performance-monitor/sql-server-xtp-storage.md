---
title: SQL Server XTP 存储 | Microsoft Docs
description: 了解 SQL Server XTP 存储性能对象，其中包含与 SQL Server 中内存中 OLTP 的磁盘上存储相关的计数器。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 4070580b-880d-4f4c-abcc-626a4fe0c9a2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a12431a679ddbe4f47697b181c81ed3b7b3f08f3
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505461"
---
# <a name="sql-server-xtp-storage"></a>SQL Server XTP 存储
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  SQL Server XTP 存储性能对象包含与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的内存中 OLTP 的磁盘存储相关的计数器。  
  
 下表介绍了 **SQL Server XTP 存储** 计数器。  
  
|计数器|说明|  
|-------------|-----------------|  
|**Checkpoints Closed**|联机代理执行的已关闭检查点的计数。|  
|**Checkpoints Completed**|脱机检查点线程处理的检查点计数。|  
|**Core Merges Completed**|合并工作线程完成的核心合并数。 仍需要安装这些合并。|  
|**Merge Policy Evaluations**|自服务器启动以来的合并策略评估数。|  
|**Merge Requests Outstanding**|自服务器启动以来未完成的合并请求数。|  
|**Merges Abandoned**|因失败而放弃的合并数。|  
|**Merges Installed**|成功安装的合并数。|  
|**Total Files Merged**|已合并的源文件总数。 此计数可用于查找合并中源文件的平均数目。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server XTP（内存中 OLTP）性能计数器](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
