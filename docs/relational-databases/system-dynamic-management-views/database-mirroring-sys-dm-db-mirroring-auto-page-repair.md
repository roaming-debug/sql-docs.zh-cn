---
description: 数据库镜像-sys.dm_db_mirroring_auto_page_repair
title: sys.dm_db_mirroring_auto_page_repair (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_mirroring_auto_page_repair_TSQL
- sys.dm_db_mirroring_auto_page_repair_TSQL
- sys.dm_db_mirroring_auto_page_repair
- dm_db_mirroring_auto_page_repair
dev_langs:
- TSQL
helpviewer_keywords:
- automatic page repair
- database mirroring [SQL Server], automatic page repair
- sys.dm_db_mirroring_auto_page_repair dynamic management view
ms.assetid: 49f0fc2a-e25e-47e1-a135-563adb509af1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cef38d675dde0d36ca97f63c16f8d4589760a620
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101666"
---
# <a name="database-mirroring---sysdm_db_mirroring_auto_page_repair"></a>数据库镜像-sys.dm_db_mirroring_auto_page_repair
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对服务器实例上所有镜像数据库的每个自动页修复尝试返回一行。 该视图包含对应于给定镜像数据库上最新自动页修复尝试的行，每个数据库最多可对应 100 行。 只要一个数据库对应的行达到最大值，则它的下个自动页修复尝试对应的行将替换现有的一个项。 下表定义了各个列的含义。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|此行对应的数据库的 ID。|  
|file_id|**int**|页所在文件的 ID。|  
|**page_id**|**bigint**|文件中页的 ID。|  
|**error_type**|**int**|错误类型。 值可以是：<br /><br /> **-** 1 = 所有硬件823错误<br /><br /> 1 = 错误的校验和或页撕裂以外的 824 错误（例如，错误的页 ID）<br /><br /> 2 = 错误的校验和<br /><br /> 3 = 页撕裂|  
|**page_status**|**int**|页修复尝试的状态：<br /><br /> 2 = 排队等候来自伙伴的请求。<br /><br /> 3 = 请求已发送到伙伴。<br /><br /> 4 = 排队等候自动页修复（已收到来自伙伴的响应）。<br /><br /> 5 = 自动页修复已成功，页应当可用。<br /><br /> 6 = 无法修复页。 这表示在页修复尝试期间发生了错误，例如，可能是由于伙伴上的页也已损坏、已经断开与伙伴的连接或网络发生故障。 此状态不是最终状态；如果在此页上再次发现损坏，则将再次请求伙伴上的该页。|  
|**modification_time**|**datetime**|页状态最后发生更改的时间。|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [自动页修复（可用性组：数据库镜像）](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [suspect_pages &#40;Transact-sql&#41;](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [管理 suspect_pages 表 (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  


