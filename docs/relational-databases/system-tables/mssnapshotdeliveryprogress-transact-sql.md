---
description: MSsnapshotdeliveryprogress (Transact-SQL)
title: MSsnapshotdeliveryprogress (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsnapshotdeliveryprogress_TSQL
- MSsnapshotdeliveryprogress
dev_langs:
- TSQL
helpviewer_keywords:
- MSsnapshotdeliveryprogress system table
ms.assetid: 9164bfe2-6fc4-4b52-946a-09ea3cf67041
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1ff258550fb6954c2138326c440c68a0f1ad88b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192320"
---
# <a name="mssnapshotdeliveryprogress-transact-sql"></a>MSsnapshotdeliveryprogress (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSsnapshotdeliveryprogress** 表用于跟踪在应用快照时已成功传送到订阅服务器的文件。 此数据用于在合并代理无法在会话时传送所有文件的情况下恢复文件传送，从而避免在下次运行合并代理时传送相同文件。 此表存储在订阅服务器的订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_token**|**nvarchar(260)**|标识从中成功传送文件的快照文件夹的路径。 对于使用参数化筛选器的发布，会将字符串 **dynsnap** 追加到值。|  
|**progress_token_hash**|**int**|基于 *progress_token* 的值生成的哈希值可提高给定 *progress_token* 值的查找效率。|  
|**progress_token**|**nvarchar (500)**|标识已成功传送的文件，其值为文件名和路径的组合。|  
|**progress_timestamp**|**datetime**|**Datetime** 值，该值指示快照文件成功传递的时间。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
