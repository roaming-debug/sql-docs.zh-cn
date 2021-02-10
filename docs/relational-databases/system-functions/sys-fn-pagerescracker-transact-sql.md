---
title: sys.fn_PageResCracker (Transact-sql) |Microsoft Docs
description: 了解 sys.fn_PageResCracker 系统函数。 请参阅示例，查看其他可用资源。
ms.custom: ''
ms.date: 09/18/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_PageResCracker
- sys.fn_PageResCracker_TSQL
- fn_PageResCracker_TSQL
- sys.fn_PageResCracker
- sys.dm_db_page_info
dev_langs:
- TSQL
helpviewer_keywords:
- fn_PageResCracker function
- page_resource
- sys.fn_PageResCracker function
- sys.dm_db_page_info
- page info
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: 49e8472353f19bcca00e68969579d4cbf6f0b538
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064812"
---
# <a name="sysfn_pagerescracker-transact-sql"></a>sys.fn_PageResCracker (Transact-sql) 
[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

返回 `db_id` `file_id` 给定值的、和 `page_id` `page_resource` 。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
```  
sys.fn_PageResCracker ( page_resource )  
```  
  
## <a name="arguments"></a>参数  
*page_resource*    
是数据库页资源的8字节十六进制格式。
  
## <a name="tables-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|db_id|**int**|数据库 ID|  
|file_id|**int**|文件 ID|  
|page_id|**int**|页面 ID|  
  
## <a name="remarks"></a>备注  
`sys.fn_PageResCracker` 用于将数据库页的8字节十六进制表示形式转换为包含页的数据库 ID、文件 ID 和页 ID 的行集。   

可以从 sys.dm_exec_requests 的列中获取有效的页面资源 `page_resource` [&#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) 动态管理视图或 [ &#40;transact-sql&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md) 系统视图的sys.sys进程。 如果使用了无效的页面资源，则返回值为 NULL。  
的主要用途 `sys.fn_PageResCracker` 是为了获取有关该页的信息（例如，它所属的对象），从而促进这些视图和 [Sys.dm_db_page_info &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) 动态管理功能之间的联接。
  
## <a name="permissions"></a>权限  
用户需要 `VIEW SERVER STATE` 对服务器的权限。  
  
## <a name="examples"></a>示例  
`sys.fn_PageResCracker`函数可与[Sys.dm_db_page_info &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)结合使用，以解决中与页相关的等待和阻止问题 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  下面的脚本是一个示例，说明如何使用这些函数为当前正在等待某种类型的页资源的所有活动请求收集数据库页面信息。 
  
```sql  
SELECT page_info.* 
FROM sys.dm_exec_requests AS d  
CROSS APPLY sys.fn_PageResCracker (d.page_resource) AS r  
CROSS APPLY sys.dm_db_page_info(r.db_id, r.file_id, r.page_id, 1) AS page_info
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_db_page_info &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)  
 [ Transact-sql&#41;的sys.sys进程 &#40;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
