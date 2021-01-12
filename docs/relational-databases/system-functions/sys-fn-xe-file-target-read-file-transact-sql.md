---
description: sys.fn_xe_file_target_read_file (Transact-SQL)
title: sys.fn_xe_file_target_read_file (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_xe_file_target_read_file_TSQL
- fn_xe_file_target_read_file
- sys.fn_xe_file_target_read_file_TSQL
- sys.fn_xe_file_target_read_file
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], functions
- fn_xe_file_target_read_file function
- sys.fn_xe_file_target_read_file function
ms.assetid: cc0351ae-4882-4b67-b0d8-bd235d20c901
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8af7ee0bc6c899026e51110264f2b28c2f579dd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096337"
---
# <a name="sysfn_xe_file_target_read_file-transact-sql"></a>sys.fn_xe_file_target_read_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  读取扩展事件异步文件目标创建的文件。 每行返回一个 XML 格式的事件。  
  
> [!WARNING]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)][!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]接受 .xel 和 .xem 格式生成的跟踪结果。 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 扩展事件仅支持 .XEL 格式的跟踪结果。 我们推荐您使用 SQL Server Management Studio 来读取 XEL 格式的跟踪结果。    
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_xe_file_target_read_file ( path, mdpath, initial_file_name, initial_offset )  
```  
  
## <a name="arguments"></a>参数  
 *路径*  
 要读取的文件的路径。 *路径* 可以包含通配符，并包含文件的名称。 *路径* 为 **nvarchar (260)**。 没有默认值。 在 Azure SQL 数据库的上下文中，此值是 Azure 存储中文件的 HTTP URL。
  
 *mdpath*  
 与 *path* 参数所指定的文件相对应的元数据文件的路径。 *mdpath* 是 **nvarchar (260)**。 没有默认值。 从 SQL Server 2016 开始，可以将此参数指定为 null。
  
> [!NOTE]  
>  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 不需要 *mdpath* 参数。 但是，为了在以前的 SQL Server 版本中生成的日志文件的向后兼容性，需要维护该参数。  
  
 *initial_file_name*  
 要从 *路径* 中读取的第一个文件。 *initial_file_name* **(260) 为 nvarchar**。 没有默认值。 如果指定 **null** 作为参数，则读取 *路径* 中找到的所有文件。  
  
> [!NOTE]  
>  *initial_file_name* 和 *initial_offset* 均为成对参数。 如果为其中一个参数指定值，则必须为另一个参数也指定值。  
  
 *initial_offset*  
 用于指定之前读取的上一个偏移量并跳过该偏移量之前的所有事件（包括该偏移量处的事件）。 事件枚举在指定偏移量后开始。 *initial_offset* 为 **bigint**。 如果指定 **null** 作为参数，将读取整个文件。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|module_guid|**uniqueidentifier**|事件模块 GUID。 不可为 null。|  
|package_guid|**uniqueidentifier**|事件包 GUID。 不可为 null。|  
|object_name|**nvarchar(256)**|事件的名称。 不可为 null。|  
|event_data|**nvarchar(max)**|XML 格式的事件内容。 不可为 null。|  
|file_name|**nvarchar(260)**|包含事件的文件的名称。 不可为 null。|  
|file_offset|**bigint**|包含事件的块在文件中的偏移位置。 不可为 null。|  
|timestamp_utc|**datetime2**|**适用于**：[!INCLUDE[ssSQLv14](../../includes/sssqlv14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。<br /><br />事件 (UTC 时区的日期和时间) 。 不可为 null。|  

  
## <a name="remarks"></a>备注  
 通过在中执行 **sys.fn_xe_file_target_read_file** 读取大型结果集 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 可能会导致错误。 使用 " **结果显示文件** 模式" (**Ctrl + Shift + F**) 将大型结果集导出到文件中，并改为使用其他工具读取文件。  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-retrieving-data-from-file-targets"></a>A. 从文件目标检索数据  
 下面的示例从所有文件获取所有行。 在此示例中，文件目标和图元文件位于 C:\ 驱动器上的跟踪文件夹中。  
  
```  
SELECT * FROM sys.fn_xe_file_target_read_file('C:\traces\*.xel', 'C:\traces\metafile.xem', null, null);  
```  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件动态管理视图](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [&#40;Transact-sql&#41;的扩展事件目录视图 ](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [扩展事件](../../relational-databases/extended-events/extended-events.md)  
  
  
