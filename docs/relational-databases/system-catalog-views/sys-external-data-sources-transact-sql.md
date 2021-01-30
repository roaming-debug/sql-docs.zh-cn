---
description: sys.external_data_sources (Transact-SQL)
title: sys.external_data_sources (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 1016db6e-9950-4ae2-a004-bd4171e27359
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4854c6e7e7b02b127cd8622ecb3c089485da5464
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203841"
---
# <a name="sysexternal_data_sources-transact-sql"></a>sys.external_data_sources (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  对于、和的当前数据库中的每个外部数据源，都包含一行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSDS](../../includes/sssds-md.md)] [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。  
  
 为服务器上的每个外部数据源包含一行 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|data_source_id|**int**|外部数据源的对象 ID。||  
|name|**sysname**|外部数据源的名称。||  
|location|**nvarchar(4000)**|连接字符串，其中包括用于外部数据源的协议、IP 地址和端口。||  
|type_desc|**nvarchar(255)**|以字符串形式显示的数据源类型。|HADOOP、RDBMS、SHARD_MAP_MANAGER、RemoteDataArchiveTypeExtDataSource|  
|type|**tinyint**|以数字形式显示的数据源类型。|0-HADOOP<br /><br /> 1-RDBMS<br /><br /> 2-SHARD_MAP_MANAGER<br /><br /> 3-RemoteDataArchiveTypeExtDataSource|  
|resource_manager_location|**nvarchar(4000)**|对于类型 HADOOP，为 Hadoop 资源管理器的 IP 和端口位置。 这用于在 Hadoop 数据源上提交作业。<br /><br /> 对于其他类型的外部数据源为 NULL。||  
|credential_id|**int**|用于连接到外部数据源的数据库范围凭据的对象 ID。||  
|database_name|**sysname**|对于类型 RDBMS，为远程数据库的名称。 对于 "类型"，请 SHARD_MAP_MANAGER 分片映射管理器数据库的名称。 对于其他类型的外部数据源为 NULL。||  
|shard_map_name|**sysname**|对于类型 SHARD_MAP_MANAGER，则为分片映射的名称。 对于其他类型的外部数据源为 NULL。||  
  
## <a name="permissions"></a>权限  
 目录视图中仅显示用户拥有的安全对象的元数据，或用户对其拥有某些权限的安全对象的元数据。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.external_file_formats &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_tables &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)   
 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  
