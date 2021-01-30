---
description: sys.dm_db_objects_impacted_on_version_change（Azure SQL 数据库）
title: sys.dm_db_objects_impacted_on_version_change
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_db_objects_impacted_on_version_change_TSQL
- dm_db_objects_impacted_on_version_change
- dm_db_objects_impacted_on_version_change_TSQL
- sys.dm_db_objects_impacted_on_version_change
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_objects_impacted_on_version_change
- sys.dm_db_objects_impacted_on_version_change
ms.assetid: b94af834-c4f6-4a27-80a6-e8e71fa8793a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 6e04a46841fe9e1df60133c9f3ff9495e4375abc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206270"
---
# <a name="sysdm_db_objects_impacted_on_version_change-azure-sql-database"></a>sys.dm_db_objects_impacted_on_version_change（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  此数据库范围内的系统视图旨在提供一个早期警告系统，以确定 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中的主要版本升级所影响的对象。 可以在升级之前或之后使用此视图以获得受影响对象的完整枚举。 您需要在每个数据库中查询此视图，以获取有关整个服务器的完整帐户信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|class|**int** NOT NULL|将受影响的对象的类：<br /><br /> **1** = 约束<br /><br /> **7** = 索引和堆|  
|class_desc|**nvarchar (60)** NOT NULL|类的说明：<br /><br /> **OBJECT_OR_COLUMN**<br /><br /> **INDEX**|  
|major_id|**int** NOT NULL|约束的对象 ID，或包含索引或堆的表的对象 ID。|  
|minor_id|**int** 无效|对于约束为 **NULL**<br /><br /> 索引和堆的 Index_id|  
|dependency|**nvarchar (60)** NOT NULL|导致约束或索引受影响的依赖项的说明。 同一值也用于在升级期间生成的警告。<br /><br /> 示例:<br /><br /> **space**（用于内部）<br /><br /> **geometry**（用于系统 UDT）<br /><br /> **geography::Parse**（用于系统 UDT 方法）|  
  
## <a name="permissions"></a>权限  
 需要拥有 VIEW DATABASE STATE 权限。  
  
## <a name="example"></a>示例  
 下面的示例显示针对 **sys.dm_db_objects_impacted_on_version_change** 的查询，以便查找受升级到下一主要服务器版本影响的对象。  
  
```  
SELECT * FROM sys.dm_db_objects_disabled_on_version_change;  
GO  
```  
  
```  
class  class_desc        major_id    minor_id    dependency                       
------ ----------------- ----------- ----------- ----------   
1      OBJECT_OR_COLUMN  181575685   NULL        geometry                        
7      INDEX             37575172    1           geometry                        
7      INDEX             2121058592  1           geometry                        
1      OBJECT_OR_COLUMN  101575400   NULL        geometry     
```  
  
## <a name="remarks"></a>备注  
  
### <a name="how-to-update-impacted-objects"></a>如何升级受影响的对象  
 以下有序步骤介绍将在六月份服务版本升级后要采取的纠正措施。  
  
|订单|受影响的对象|纠正措施|  
|-----------|---------------------|-----------------------|  
|1|**索引**|重新生成由 **sys.dm_db_objects_impacted_on_version_change** 标识的任何索引，例如：  `ALTER INDEX ALL ON <table> REBUILD`<br />或<br />`ALTER TABLE <table> REBUILD`|  
|2|**对象**|在重新计算基础表中的 geometry 和 geography 数据后，必须重新验证 **sys.dm_db_objects_impacted_on_version_change** 标识的所有约束。 对于约束，使用 ALTER TABLE 进行重新验证。 <br />例如： <br />`ALTER TABLE <tab> WITH CHECK CHECK CONSTRAINT <constraint name>`<br />或<br />`ALTER TABLE <tab> WITH CHECK CONSTRAINT ALL`|  
  
  
