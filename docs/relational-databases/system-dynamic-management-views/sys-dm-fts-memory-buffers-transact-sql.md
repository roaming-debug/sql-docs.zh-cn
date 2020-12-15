---
description: sys.dm_fts_memory_buffers (Transact-SQL)
title: sys.dm_fts_memory_buffers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_memory_buffers
- dm_fts_memory_buffers_TSQL
- dm_fts_memory_buffers
- sys.dm_fts_memory_buffers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_memory_buffers dynamic management view
ms.assetid: 56895fe5-e8df-4d75-9adc-c1f7757cdef8
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 37f765dd9821d6323a13ec52ad49fcaf2821d9c7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484669"
---
# <a name="sysdm_fts_memory_buffers-transact-sql"></a>sys.dm_fts_memory_buffers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关属于特定内存池的内存缓冲区（作为全文爬网或全文爬网范围的一部分使用）的信息。  
  
> [!NOTE]
> 在的未来版本中将删除以下列 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ： **row_count**。 应避免在新的开发工作中使用该列，并着手修改当前使用该列的应用程序。  

  
|列|数据类型|说明|  
|------------|---------------|-----------------|  
|**pool_id**|**int**|已分配的内存池的 ID。<br /><br /> 0 = 小型缓冲区<br /><br /> 1 = 大型缓冲区|  
|**memory_address**|**varbinary(8)**|已分配的内存缓冲区的地址。|  
|**name**|**nvarchar(4000)**|执行该分配的共享内存缓冲区的名称。|  
|**is_free**|**bit**|内存缓冲区的当前状态。<br /><br /> 0 = 空闲<br /><br /> 1 = 繁忙|  
|**row_count**|**int**|该缓冲区当前正在处理的行数。|  
|**bytes_used**|**int**|该缓冲区中正在使用的内存量（字节）。|  
|**percent_used**|**int**|已分配内存已用的百分比。|  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="physical-joins"></a>物理联接  
 ![此动态管理视图的重要联接](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-memory-buffers-1.gif "此动态管理视图的重要联接")  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|关系|  
|----------|--------|------------------|  
|dm_fts_memory_buffers.pool_id|dm_fts_memory_pools.pool_id|多对一|  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [全文搜索和语义搜索动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

