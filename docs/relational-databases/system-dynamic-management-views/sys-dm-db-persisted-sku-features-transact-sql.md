---
description: sys.dm_db_persisted_sku_features (Transact-SQL)
title: sys.dm_db_persisted_sku_features (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_persisted_sku_features_TSQL
- sys.dm_db_persisted_sku_features
- dm_db_persisted_sku_features_TSQL
- dm_db_persisted_sku_features
dev_langs:
- TSQL
helpviewer_keywords:
- editions [SQL Server]
- sys.dm_db_persisted_sku_features dynamic management view
ms.assetid: b4b29e97-b523-41b9-9528-6d4e84b89e09
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9221515e0c8a6c0704f0a036d851241d849d5f3d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101654"
---
# <a name="sysdm_db_persisted_sku_features-transact-sql"></a>sys.dm_db_persisted_sku_features (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  的某些功能改变了在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 数据库文件中存储信息的方式。 这些功能仅限于特定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本。 不能将包含这些功能的数据库移到不支持这些功能的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。 使用 sys.dm_db_persisted_sku_features 动态管理视图可列出当前数据库中启用的特定于版本的功能。
  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）。
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|feature_name|**sysname**|在数据库中启用但并非所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本都支持的功能的外部名称。 必须先删除此功能，然后才能将数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的所有可用版本。|  
|feature_id|**int**|与功能关联的功能 ID。 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
  
## <a name="permissions"></a>权限  
 需要对数据库拥有 VIEW DATABASE STATE 权限。  
  
## <a name="remarks"></a>备注  
 如果数据库未使用特定版本可能限制的任何功能，则视图不返回任何行。  
  
 sys.dm_db_persisted_sku_features 可能会列出以下数据库更改的功能，这些功能仅限于特定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本：  
  
-   **ChangeCapture**：指示数据库已启用更改数据捕获。 若要删除变更数据捕获，请使用 [sys.sp_cdc_disable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) 存储过程。 有关详细信息，请参阅[关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)。  
  
-   **ColumnStoreIndex**：指示至少有一个表具有列存储索引。 若要使数据库移到不 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持此功能的版本，请使用 [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) 或 [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) 语句删除列存储索引。 有关详细信息，请参阅 [列存储索引](../../relational-databases/indexes/columnstore-indexes-overview.md)。  
  
    **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）。  
  
-   **压缩**：指示至少一个表或索引使用数据压缩或 vardecimal 存储格式。 若要使数据库移到不 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持此功能的版本，请使用 [alter TABLE](../../t-sql/statements/alter-table-transact-sql.md) 或 [alter INDEX](../../t-sql/statements/alter-index-transact-sql.md) 语句删除数据压缩。 若要删除 vardecimal 存储格式，请使用 sp_tableoption 语句。 有关详细信息，请参阅 [Data Compression](../../relational-databases/data-compression/data-compression.md)。  
  
-   **MultipleFSContainers**：指示数据库使用多个 FILESTREAM 容器。 数据库具有一个 FILESTREAM 文件组，其中包含多个容器 (文件) 。 有关详细信息，请参阅 [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md)。  
  
-   **InMemoryOLTP**：指示数据库使用 In-Memory OLTP。 数据库具有 MEMORY_OPTIMIZED_DATA 文件组。 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
  **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）。 
  
-   **分割.** 指示数据库包含已分区表、已分区索引、分区方案或分区函数。 若要使数据库能够移动到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise 或 Developer 以外的版本，将表修改到单个分区上是不够的。 必须删除相应的已分区表。 如果该表包含数据，请使用 SWITCH PARTITION 将每个分区转换成无分区表。 然后删除已分区表、分区方案和分区函数。  
  
-   **TransparentDataEncryption。** 指示使用透明数据加密对数据库进行加密。 若要删除透明数据加密，请使用 ALTER DATABASE 语句。 有关详细信息，请参阅[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)。  

> [!NOTE]
> 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Service Pack 1 开始，这些功能除外， **TransparentDataEncryption 除外。** 跨多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本提供，并不限于 Enterprise edition 或 Developer edition。

 若要确定数据库是否使用仅限于特定版本的任何功能，请对数据库执行下面的语句：  
  
```sql  
SELECT feature_name FROM sys.dm_db_persisted_sku_features;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [SQL Server 2016 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [版本和 SQL Server 2017 支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
