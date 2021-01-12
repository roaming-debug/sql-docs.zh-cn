---
description: suspect_pages (Transact-SQL)
title: suspect_pages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- suspect_page_table
- suspect_page_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- suspect_pages system table
- suspect pages [SQL Server]
ms.assetid: 119c8d62-eea8-44fb-bf72-de469c838c50
author: cawrites
ms.author: chadam
ms.openlocfilehash: c2b5952f12c857a7bf58c066744e1c366279b1e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096121"
---
# <a name="suspect_pages-transact-sql"></a>suspect_pages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于每个由于出现次要 823 错误或 824 错误而失败的页，都包含一行。 页面列出在此表中是因为怀疑这些页面存在错误，但它们实际上可能是正常的。 修复可疑页面后，会在 " **event_type** " 列中更新其状态。  
  
 下表（限制为1000行）存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|应用此页的数据库的 ID。|  
|file_id|**int**|数据库中文件的 ID。|  
|**page_id**|**bigint**|可疑页的 ID。 每一页都有一个 32 位值的页 ID，该值标识页在数据库中的位置。 **Page_id** 是 8 KB 页的数据文件的偏移量。 每个页 ID 在文件中都是唯一的。|  
|event_type|**int**|错误类型；其具体类型有：<br /><br /> 1 = 导致出现可疑页的 823 错误（如磁盘错误）或校验和错误或页撕裂之外的 824 错误（如页 ID 错误）。<br /><br /> 2 = 校验和错误。<br /><br /> 3 = 页撕裂。<br /><br /> 4 = 已还原（页在标记为错误后已还原）。<br /><br /> 5 = 已修复（DBCC 修复了页）。<br /><br /> 7 = 已由 DBCC 释放。|  
|**error_count**|**int**|错误出现的次数。|  
|**last_update_date**|**datetime**|上次更新的日期时间戳。|  
  
## <a name="permissions"></a>权限  
 任何拥有 **msdb** 访问权限的人员都可以读取 **suspect_pages** 表中的数据。 任何拥有 suspect_pages 表的 UPDATE 权限的人员都可以更新它的记录。 **msdb** 上的 **db_owner** 固定数据库角色或 **sysadmin** 固定服务器角色的成员都可以插入、更新和删除记录。  
  
## <a name="see-also"></a>另请参阅  
 [还原页 (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [数据库可疑数据页事件类](../../relational-databases/event-classes/database-suspect-data-page-event-class.md)   
 [系统表 &#40;Transact-sql&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [管理 suspect_pages 表 (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  
