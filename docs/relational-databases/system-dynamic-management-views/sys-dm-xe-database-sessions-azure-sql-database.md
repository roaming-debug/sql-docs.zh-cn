---
description: sys.dm_xe_database_sessions（Azure SQL 数据库）
title: Azure SQL Database (sys.dm_xe_database_sessions) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 33ea5179-16bb-4abd-96cc-9bc696e80987
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current
ms.openlocfilehash: f20d02ba7607ce0f905af39f15a5a76f3516638a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440672"
---
# <a name="sysdm_xe_database_sessions-azure-sql-database"></a>sys.dm_xe_database_sessions（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  返回有关会话事件的信息。 事件是离散执行点。 如果事件不包含所需的信息，则可对事件应用谓词以阻止其激发。  
  
||  
|-|  
|**适用** 于： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 和任何更高版本。|  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8)**|事件会话的内存地址。 不可为 null。|  
|event_name|**nvarchar(60)**|操作绑定到的事件的名称。 不可为 null。|  
|event_package_guid|**uniqueidentifier**|包含事件的包的 GUID。 不可为 null。|  
|event_predicate|**nvarchar(2048)**|应用于事件的谓词树的 XML 表示形式。 可以为 Null。|  
  
## <a name="permissions"></a>权限  
 要求拥有 VIEW DATABASE STATE 权限。  
  
### <a name="relationship-cardinalities"></a>关系基数  
从2015-07-13，"sys.dm_xe_objects" 是在其名称中不包含 "_database" 的其中一个 XEvents Dmv。 不是下表的右侧列中的错误键入或错误。 Microsoft SQL Server 和 Azure SQL 数据库中的名称相同。  
  
|From|功能|关系|  
|--------|------|----------------|  
|sys.dm_xe_database_session_events sys.dm_xe_database_session_events.event_session_address|sys.dm_xe_database_sessions 地址|多对一|  
|sys.databases _xe_database_session_events. event_package_guid，sys.databases _xe_database_session_events. event_name|sys.dm_xe_objects.name、sys.dm_xe_objects.package_guid|多对一|  
  
## <a name="see-also"></a>另请参阅  
[Azure SQL 数据库中的扩展事件](/azure/azure-sql/database/xevent-db-diff-from-svr)  
[扩展事件](../../relational-databases/extended-events/extended-events.md)  
  
