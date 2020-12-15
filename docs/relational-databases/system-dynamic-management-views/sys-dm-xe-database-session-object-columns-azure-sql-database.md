---
description: sys.dm_xe_database_session_object_columns（Azure SQL 数据库）
title: sys.dm_xe_database_session_object_columns
titleSuffix: Azure SQL Database
ms.date: 06/10/2016
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 0e6adc54-4d97-4ef0-bf4f-b4538d69f136
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 92d1bb472d9d8369e116501b31f78dd2c5d3c8ff
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440702"
---
# <a name="sysdm_xe_database_session_object_columns-azure-sql-database"></a>sys.dm_xe_database_session_object_columns（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  显示绑定到会话的对象的配置值。  
  
||  
|-|  
|**适用** 于： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 和任何更高版本。|  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8)**|事件会话的内存地址。 与 sys.dm_xe_database_sessions 具有多对一关系。 不可为 null。|  
|column_name|**nvarchar(60)**|配置值的名称。 不可为 null。|  
|column_id|**int**|列的 ID。 在对象中是唯一的。 不可为 null。|  
|column_value|**nvarchar(2048)**|列的配置值。 可以为 Null。|  
|object_type|**nvarchar(60)**|对象的类型。  不 nullable.object_type 是以下其中之一：<br /><br /> event<br /><br /> 目标|  
|object_name|**nvarchar(60)**|此列所属对象的名称。 不可为 null。|  
|object_package_guid|**uniqueidentifier**|包含该对象的包的 GUID。 不可为 null。|  
  
## <a name="permissions"></a>权限  
 要求拥有 VIEW DATABASE STATE 权限。  
  
### <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|关系|  
|----------|--------|------------------|  
|dm_xe_database_session_object_columns dm_xe_database_session_object_columns.object_name<br /><br /> dm_xe_database_session_object_columns dm_xe_database_session_object_columns.object_package_guid|sys.dm_xe_objects.package_guid<br /><br /> sys.dm_xe_objects.name|多对一|  
|dm_xe_database_session_object_columns dm_xe_database_session_object_columns.column_name<br /><br /> dm_xe_database_session_object_columns dm_xe_database_session_object_columns.column_id|sys.dm_xe_object_columns.name<br /><br /> sys.dm_xe_object_columns.column_id|多对一|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)  
  
  
