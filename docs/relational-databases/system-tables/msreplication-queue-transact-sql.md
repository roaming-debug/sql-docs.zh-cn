---
description: MSreplication_queue (Transact-SQL)
title: MSreplication_queue (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_queue
- MSreplication_queue_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_queue system table
ms.assetid: 664bf817-8021-4417-96d6-2bb1e4baabff
author: cawrites
ms.author: chadam
ms.openlocfilehash: ca7e44f973e0e9150511f70e19dec81b5383d994
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098124"
---
# <a name="msreplication_queue-transact-sql"></a>MSreplication_queue (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  复制过程使用 **MSreplication_queue** 表存储使用基于 SQL 的排队的所有排队更新订阅所发出的排队命令。 该表存储在订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**publisher_db**|**sysname**|发布数据库的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**tranid**|**sysname**|执行排队命令时所使用的事务 ID。|  
|**data**|varbinary(8000)|存储有关排队命令信息的压缩字节流。|  
|**datalen**|**int**|数据的长度（字节）。|  
|**commandtype**|**int**|排队的命令类型包括：<br /><br /> 1 = 事务中的用户命令。<br /><br /> 2 = 订阅同步命令。|  
|**insertdate**|**datetime**|插入日期。|  
|**orderkey**|**bigint**|单调增加的标识列。|  
|**cmdstate**|**bit**|命令状态：<br /><br /> 0 = 完成。<br /><br /> 1 = 部分完成。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
