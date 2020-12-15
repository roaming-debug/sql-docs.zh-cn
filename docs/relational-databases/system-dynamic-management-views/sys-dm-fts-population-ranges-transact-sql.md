---
description: sys.dm_fts_population_ranges (Transact-SQL)
title: sys.dm_fts_population_ranges (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_population_ranges
- sys.dm_fts_population_ranges_TSQL
- dm_fts_population_ranges_TSQL
- dm_fts_population_ranges
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_population_ranges dynamic management view
ms.assetid: 58d8564b-9c43-4965-a31c-2893890334ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ae7ec8d8a4dde2a30e86b5c6a47e2af4d0c6c73b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484629"
---
# <a name="sysdm_fts_population_ranges-transact-sql"></a>sys.dm_fts_population_ranges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关与当前正在进行的全文索引填充相关的特定范围的信息。  
   
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**memory_address**|**varbinary(8)**|为与全文索引填充子范围相关的活动分配的内存缓冲区地址。|  
|**parent_memory_address**|**varbinary(8)**|代表与全文索引相关的所有填充范围的父对象的内存缓冲区地址。|  
|**is_retry**|**bit**|如果值为 1，则该子范围负责重试出现错误的行。|  
|**session_id**|**smallint**|当前正在处理该任务的会话的 ID。|  
|**processed_row_count**|**int**|此范围内已经处理的行数。 保持前进进度，并且每隔 5 分钟进行一次计数，而不是在每个批次提交时进行计数。|  
|**error_count**|**int**|此范围内已经出现错误的行数。 保持前进进度，并且每隔 5 分钟进行一次计数，而不是在每个批次提交时进行计数。|  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
 
## <a name="physical-joins"></a>物理联接  
 ![此动态管理视图的重要联接](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-population-ranges-1.gif "此动态管理视图的重要联接")  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|关系|  
|----------|--------|------------------|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|多对一|  
  
## <a name="see-also"></a>另请参阅  
  [全文搜索和语义搜索动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

