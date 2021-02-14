---
description: sys.dm_os_buffer_descriptors (Transact-SQL)
title: sys.dm_os_buffer_descriptors (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_buffer_descriptors_TSQL
- dm_os_buffer_descriptors_TSQL
- sys.dm_os_buffer_descriptors
- dm_os_buffer_descriptors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_descriptors dynamic management view
ms.assetid: 012aab95-8888-4f35-9ea3-b5dff6e3f60f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83162825d5c4598c8c9f5fecae10feed03364abd
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344321"
---
# <a name="sysdm_os_buffer_descriptors-transact-sql"></a>sys.dm_os_buffer_descriptors (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 缓冲池中当前所有数据页的信息。 可以使用该视图的输出，根据数据库、对象或类型来确定缓冲池内数据库页的分布。 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中，此动态管理视图还返回有关缓冲池扩展文件中的数据页的信息。 有关详细信息，请参阅 [缓冲池扩展](../../database-engine/configure-windows/buffer-pool-extension.md)。  
  
 当从磁盘读取数据页时，该数据页被复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 缓冲池并被缓存以供重复使用。 每个缓存的数据页都有一个缓冲描述符。 缓冲描述符唯一地标识 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中当前缓存的每个数据页。 sys.dm_os_buffer_descriptors 返回所有用户数据库和系统数据库的缓存页。 这包括与 Resource 数据库相关联的页。  
  
> **注意：** 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_buffer_descriptors**。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|与缓冲池中的页关联的数据库 ID。 可以为 Null。|  
|file_id|**int**|存储页的持久化图像的文件 ID。 可以为 Null。|  
|page_id|**int**|文件中页面的 ID。 可以为 Null。|  
|page_level|**int**|页的索引级别。 可以为 Null。|  
|allocation_unit_id|**bigint**|页的分配单元的 ID。 可使用此值联接 sys.allocation_units。 可以为 Null。|  
|page_type|**nvarchar(60)**|页类型，例如数据页或索引页。 可以为 Null。|  
|row_count|**int**|页中的行数。 可以为 Null。|  
|free_space_in_bytes|**int**|页中的可用空间（字节）。 可以为 Null。|  
|is_modified|**bit**|1 = 从磁盘读取页后已对其进行修改。 可以为 Null。|  
|numa_node|**int**|缓冲区的非一致性内存访问节点。 可以为 Null。|  
|read_microsec|**bigint**|将此页读入缓冲区所需的实际时间（微秒）。 重用缓冲区时重置该数值。 可以为 Null。|  
|is_in_bpool_extension|**bit**|1 = 页在缓冲池扩展中。 可以为 Null。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
   
## <a name="remarks"></a>备注  
 sys.dm_os_buffer_descriptors 返回资源数据库正在使用的页。 sys.dm_os_buffer_descriptors 不会返回有关免费或被盗页面的信息，也不会返回有关在读取时出错的页的信息。  
  
|从|功能|开|关系|  
|----------|--------|--------|------------------|  
|sys.dm_os_buffer_descriptors|sys.databases|database_id|多对一|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.allocation_units|allocation_unit_id|多对一|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.database_files|file_id|多对一|  
|sys.dm_os_buffer_descriptors|sys.dm_os_buffer_pool_extension_configuration|file_id|多对一|  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-cached-page-count-for-each-database"></a>A. 返回每个数据库的缓存页计数  
 以下示例返回为每个数据库加载的页的计数。  
  
```  
SELECT COUNT(*)AS cached_pages_count  
    ,CASE database_id   
        WHEN 32767 THEN 'ResourceDb'   
        ELSE db_name(database_id)   
        END AS database_name  
FROM sys.dm_os_buffer_descriptors  
GROUP BY DB_NAME(database_id) ,database_id  
ORDER BY cached_pages_count DESC;  
```  
  
### <a name="b-returning-cached-page-count-for-each-object-in-the-current-database"></a>B. 返回当前数据库中每个对象的缓存页计数  
 以下示例返回为当前数据库中每个对象加载的页的计数。  
  
```  
SELECT COUNT(*)AS cached_pages_count   
    ,name ,index_id   
FROM sys.dm_os_buffer_descriptors AS bd   
    INNER JOIN   
    (  
        SELECT object_name(object_id) AS name   
            ,index_id ,allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.hobt_id   
                    AND (au.type = 1 OR au.type = 3)  
        UNION ALL  
        SELECT object_name(object_id) AS name     
            ,index_id, allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.partition_id   
                    AND au.type = 2  
    ) AS obj   
        ON bd.allocation_unit_id = obj.allocation_unit_id  
WHERE database_id = DB_ID()  
GROUP BY name, index_id   
ORDER BY cached_pages_count DESC;  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.allocation_units (Transact-SQL)](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 
 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [Resource 数据库](../../relational-databases/databases/resource-database.md)   
 [sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)  
  
  


