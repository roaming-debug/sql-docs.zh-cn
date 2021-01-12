---
description: sys.database_service_objectives（Azure SQL 数据库）
title: sys.database_service_objectives
titleSuffix: Azure SQL Database
ms.date: 03/21/2018
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
ms.reviewer: ''
ms.topic: conceptual
keywords:
- 弹性池
- 弹性池，管理
f1_keywords:
- DATABASE_SERVICE_OBJECTIVES_TSQL
ms.assetid: cecd8c31-06c0-4aa7-85d3-ac590e6874fa
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 861e8297a54cff399425e521591cd983afbb0cc6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098002"
---
# <a name="sysdatabase_service_objectives-azure-sql-database"></a>sys.database_service_objectives（Azure SQL 数据库）
[!INCLUDE [asdb-asdbmi-asa](../../includes/applies-to-version/asdb-asdbmi-asa.md)]

为 Azure SQL 数据库或 Azure Synapse 分析返回 service 层) 、服务目标 (定价层) 和弹性池名称（如果有）的版本 (。 如果已登录到 Azure SQL 数据库服务器中的 master 数据库，则会返回所有数据库的相关信息。 对于 Azure Synapse Analytics，必须连接到 master 数据库。  
  
  
 有关定价的信息，请参阅 [sql 数据库选项和性能： Sql Database 定价](https://azure.microsoft.com/pricing/details/sql-database/) 和 [Azure Synapse Analytics 定价](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)。  
  
 若要更改服务设置，请参阅 [ALTER database (AZURE SQL DATABASE) ](../../t-sql/statements/alter-database-transact-sql.md) 并 [Alter Database (azure Synapse Analytics) ](../../t-sql/statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)。  
  
 Sys.database_service_objectives 视图包含以下列。  
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|int|数据库的 ID，在 Azure SQL 数据库服务器的实例中是唯一的。 可加入与 [sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)。|  
|edition|sysname|数据库或数据仓库的服务层： **基本**、 **标准**、 **高级** 或 **数据仓库**。|  
|service_objective|sysname|数据库的定价层。 如果数据库位于弹性池中，则返回 **ElasticPool**。<br /><br /> 在 **基本** 层上，返回 **basic**。<br /><br /> **标准服务层中的单一数据库** 返回以下项之一： S0、S1、S2、S3、S4、S6、S7、S9 或 S12。<br /><br /> **高级层中的单一数据库** 返回以下各项： P1、P2、P4、P6、P11 或 P15。<br /><br /> **Azure Synapse Analytics** 通过 DW30000C 返回 DW100。<br /><br /> 有关详细信息，请参阅 [单一数据库](/azure/sql-database/sql-database-dtu-resource-limits-single-databases/)、 [弹性池](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools/)、 [数据仓库](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu/)|  
|elastic_pool_name|sysname|数据库所属的 [弹性池](/azure/azure-sql/database/elastic-pool-overview) 的名称。 如果数据库为单一数据库或数据仓库，则返回 **NULL** 。|  
  
## <a name="permissions"></a>权限  
 需要对 master 数据库的 **dbManager** 权限。  在数据库级别，用户必须是创建者或所有者。  
  
## <a name="examples"></a>示例  
 此示例可在 master 数据库或 Azure SQL 数据库用户数据库上运行。 查询将返回数据库的名称、服务和性能层信息 () 。  
  
```sql  
SELECT  d.name,   
     slo.*    
FROM sys.databases d   
JOIN sys.database_service_objectives slo    
ON d.database_id = slo.database_id;  
  
```  
  
