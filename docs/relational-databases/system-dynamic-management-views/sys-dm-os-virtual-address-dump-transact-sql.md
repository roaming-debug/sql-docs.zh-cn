---
description: sys.dm_os_virtual_address_dump (Transact-SQL)
title: sys.dm_os_virtual_address_dump (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_virtual_address_dump
- sys.dm_os_virtual_address_dump_TSQL
- sys.dm_os_virtual_address_dump
- dm_os_virtual_address_dump_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_virtual_address_dump dynamic management view
ms.assetid: 7b24ea55-3873-42fd-a86c-441c92eb6175
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a5226d8e0518f28d91334c3e48a61a8d082d7401
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750687"
---
# <a name="sysdm_os_virtual_address_dump-transact-sql"></a>sys.dm_os_virtual_address_dump (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  返回有关调用进程的虚拟地址空间中的页范围的信息。  
  
> [!NOTE]  
>  此信息也由 **VirtualQuery** Windows API 返回。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_virtual_address_dump**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**region_base_address**|**varbinary(8)**|页区域的基址指针。 不可为 null。|  
|**region_allocation_base_address**|**varbinary(8)**|由 VirtualAlloc Windows API 函数分配的页范围的基址指针。 由 BaseAddress 成员指向的页包含在该分配范围中。 不可为 null。|  
|**region_allocation_protection**|**varbinary(8)**|首次分配区域时的保护属性。 该值是下列值之一：<br /><br /> -PAGE_READONLY<br />-PAGE_READWRITE<br />-PAGE_NOACCESS<br />-PAGE_WRITECOPY<br />-PAGE_EXECUTE<br />-PAGE_EXECUTE_READ<br />-PAGE_EXECUTE_READWRITE<br />-PAGE_EXECUTE_WRITECOPY<br />-PAGE_GUARD<br />-PAGE_NOCACHE<br /><br /> 不可为 null。|  
|**region_size_in_bytes**|**bigint**|起始于基址的区域大小（字节），该区域中的所有页均具有相同的属性。 不可为 null。|  
|**region_state**|**varbinary(8)**|区域的当前状态。 这是以下各项之一：<br /><br /> -MEM_COMMIT<br />-MEM_RESERVE<br />-MEM_FREE<br /><br /> 不可为 null。|  
|**region_current_protection**|**varbinary(8)**|保护属性。 该值是下列值之一：<br /><br /> -PAGE_READONLY<br />-PAGE_READWRITE<br />-PAGE_NOACCESS<br />-PAGE_WRITECOPY<br />-PAGE_EXECUTE<br />-PAGE_EXECUTE_READ<br />-PAGE_EXECUTE_READWRITE<br />-PAGE_EXECUTE_WRITECOPY<br />-PAGE_GUARD<br />-PAGE_NOCACHE<br /><br /> 不可为 null。|  
|**region_type**|**varbinary(8)**|标识区域中的页类型。 值可以是下列任一值：<br /><br /> -MEM_PRIVATE<br />-MEM_MAPPED<br />-MEM_IMAGE<br /><br /> 不可为 null。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


