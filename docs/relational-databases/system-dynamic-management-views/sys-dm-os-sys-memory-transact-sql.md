---
description: sys.dm_os_sys_memory (Transact-SQL)
title: sys.dm_os_sys_memory (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_sys_memory
- sys.dm_os_sys_memory_TSQL
- sys.dm_os_sys_memory
- dm_os_sys_memory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_memory dynamic management view
ms.assetid: 1ca58814-1caa-44c1-b307-ff0bdcbbef62
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a8d6e6098bf4e480a7279e7243111dd720dcd1b5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484619"
---
# <a name="sysdm_os_sys_memory-transact-sql"></a>sys.dm_os_sys_memory (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  从操作系统返回内存信息。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 受操作系统级别的外部内存条件和基础硬件物理限制的约束并对其有所响应。 确定整个系统的状态是评估 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存使用量的重要方面。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_sys_memory**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**total_physical_memory_kb**|**bigint**|可供操作系统使用的总物理内存大小，单位为千字节 (KB)。|  
|**available_physical_memory_kb**|**bigint**|可用物理内存的大小，单位为 KB。|  
|**total_page_file_kb**|**bigint**|操作系统报告的提交限制的大小，单位为 KB|  
|**available_page_file_kb**|**bigint**|未使用的页面文件总数（KB）。|  
|**system_cache_kb**|**bigint**|系统缓存内存总量，单位为 KB。|  
|**kernel_paged_pool_kb**|**bigint**|分页内核池的总量，单位为 KB。|  
|**kernel_nonpaged_pool_kb**|**bigint**|非分页内核池的总量，单位为 KB。|  
|**system_high_memory_signal_state**|**bit**|系统内存资源充足的状态通知。 值为 1 指示内存充足信号已由 Windows 设置。 有关详细信息，请参阅 MSDN library 中的 [CreateMemoryResourceNotification](/windows/win32/api/memoryapi/nf-memoryapi-creatememoryresourcenotification) 。|  
|**system_low_memory_signal_state**|**bit**|系统内存资源不足的状态通知。 值为 1 指示内存不足信号已由 Windows 设置。 有关详细信息，请参阅 MSDN library 中的 [CreateMemoryResourceNotification](/windows/win32/api/memoryapi/nf-memoryapi-creatememoryresourcenotification) 。|  
|**system_memory_state_desc**|**nvarchar(256)**|内存状态的说明。 请参阅下表。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
|条件|“值”|  
|---------------|-----------|  
|system_high_memory_signal_state = 1<br /><br /> 和<br /><br /> system_low_memory_signal_state = 0|可用物理内存充足|  
|system_high_memory_signal_state = 0<br /><br /> 和<br /><br /> system_low_memory_signal_state = 1|可用物理内存不足|  
|system_high_memory_signal_state = 0<br /><br /> 和<br /><br /> system_low_memory_signal_state = 0|物理内存使用量稳定|  
|system_high_memory_signal_state = 1<br /><br /> 和<br /><br /> system_low_memory_signal_state = 1|物理内存状态正在转换<br /><br /> 不得同时出现充足和不足两种信号。 但是，在操作系统级别的快速变更可能会导致对某个用户模式应用程序同时显示这两个值。 这两个信号同时出现将解释为转换状态。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
