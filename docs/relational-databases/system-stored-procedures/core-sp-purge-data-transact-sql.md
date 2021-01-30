---
description: core.sp_purge_data (Transact-SQL)
title: core.sp_purge_data (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_purge_data_TSQL
- sp_purge_data
dev_langs:
- TSQL
helpviewer_keywords:
- sp_purge_data
- management data warehouse, data collector stored procedures
- core.sp_purge_data stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 056076c3-8adf-4f51-8a1b-ca39696ac390
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a4f198f3aa51d42b3dbc22d040fe9b3325f4361f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190056"
---
# <a name="coresp_purge_data-transact-sql"></a>core.sp_purge_data (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  基于保留策略从管理数据仓库中删除数据。 此过程由 mdw_purge_data [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业对与指定的实例关联的管理数据仓库每天执行。 可以使用此存储过程从管理数据仓库中执行数据的按需删除。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
core.sp_purge_data  
    [ [ @retention_days = ] retention_days ]  
    [ , [ @instance_name = ] 'instance_name' ]  
    [ , [ @collection_set_uid = ] 'collection_set_uid' ]  
    [ , [ @duration = ] duration ]  
```  
  
## <a name="arguments"></a>参数  
 [ @retention_days =] *retention_days*  
 管理数据仓库表中的数据的保留天数。 删除时间戳早于 *retention_days* 的数据。 *retention_days* 为 **smallint**，默认值为 NULL。 如果指定，该值必须是正数。 为 NULL 时，core.snapshots 视图中的 valid_through 列中的值决定了符合删除条件的行。  
  
 [ @instance_name =] "*instance_name*"  
 收集组实例的名称。 *instance_name* 的默认值为 **sysname**，默认值为 NULL。  
  
 *instance_name* 必须是完全限定的实例名称，由计算机名称和实例名称组成，格式为 *computername* \\ *instancename*。 为 NULL 时，使用本地服务器上的默认实例。  
  
 [ @collection_set_uid =] "*collection_set_uid*"  
 收集组的 GUID。 *collection_set_uid* 的值为 **uniqueidentifier**，默认值为 NULL。 为 NULL 时，将删除所有收集组中的限定行。 若要获取此值，请查询 syscollector_collection_sets 目录视图。  
  
 [ @duration =] *持续时间*  
 清除操作应当运行的最长分钟数。 *持续时间* 为 **smallint**，默认值为 NULL。 如果指定，则该值必须是零或正整数。 为 NULL 时，操作将一直运行，直到删除所有符合条件的行或手动停止操作。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 此过程将基于保留期选择 core.snapshots 视图中符合删除条件的行。 符合删除条件的所有行将从 core.snapshots_internal 表中删除。 删除位于前面的行将在所有管理数据仓库表中触发级联删除操作。 通过使用 ON DELETE CASCADE 子句可以完成此操作，该子句是为用于存储收集的数据的所有表定义的。  
  
 每个快照及其关联的数据都将在显式事务中删除，然后提交。 因此，如果手动停止清除操作，或超出为指定的值 @duration ，则只保留未提交的数据。 此数据可以在下一次运行作业时删除。  
  
 必须在管理数据仓库数据库的上下文中执行此过程。  
  
## <a name="permissions"></a>权限  
 需要具有 EXECUTE 权限的 **mdw_admin** (中的成员资格) 固定数据库角色。  
  
## <a name="examples"></a>示例  
  
### <a name="a-running-sp_purge_data-with-no-parameters"></a>A. 无参数运行 sp_purge_data  
 以下示例在不指定任何参数的情况下执行 core.sp_purge_data。 因此，默认值 NULL 及关联行为将用于所有参数。  
  
```  
USE <management_data_warehouse>;  
EXECUTE core.sp_purge_data;  
GO  
```  
  
### <a name="b-specifying-retention-and-duration-values"></a>B. 指定保留期和持续时间值  
 下面的示例从管理数据仓库中删除超过 7 天的数据。 此外，还 @duration 指定了参数，以便运行的运行时间不超过5分钟。  
  
```  
USE <management_data_warehouse>;  
EXECUTE core.sp_purge_data @retention_days = 7, @duration = 5;  
GO  
```  
  
### <a name="c-specifying-an-instance-name-and-collection-set"></a>C. 指定实例名称和收集组  
 下面的示例从指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的给定收集组的管理数据仓库中删除数据。 由于 @retention_days 未指定，因此将使用 "核心快照视图" 中 "valid_through" 列中的值来确定符合删除条件的收集组的行。  
  
```  
USE <management_data_warehouse>;  
GO  
-- Get the collection set unique identifier for the Disk Usage system collection set.  
DECLARE @disk_usage_collection_set_uid uniqueidentifier = (SELECT collection_set_uid   
    FROM msdb.dbo.syscollector_collection_sets WHERE name = N'Disk Usage');   
  
EXECUTE core.sp_purge_data @instance_name = @@SERVERNAME, @collection_set_uid = @disk_usage_collection_set_uid;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)  
  
  
