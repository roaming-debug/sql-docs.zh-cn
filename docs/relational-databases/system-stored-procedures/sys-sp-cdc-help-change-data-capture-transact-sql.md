---
description: sys.sp_cdc_help_change_data_capture (Transact-SQL)
title: sys.sp_cdc_help_change_data_capture (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_help_change_data_capture_TSQL
- sys.sp_cdc_help_change_data_capture_TSQL
- sp_cdc_help_change_data_capture
- sys.sp_cdc_help_change_data_capture
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], querying metadata
- sys.sp_cdc_help_change_data_capture
- sp_cdc_help_change_data_capture
ms.assetid: 91fd41f5-1b4d-44fe-a3b5-b73eff65a534
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 79c12ab4b1bdfb3a955c83ef3d724a026f24f1ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159775"
---
# <a name="syssp_cdc_help_change_data_capture-transact-sql"></a>sys.sp_cdc_help_change_data_capture (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回当前数据库中为变更数据捕获启用的每个表的变更数据捕获配置。 最多可为每个源表返回两行，为每个捕获实例返回一行。 并非在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的每个版本中均提供变更数据捕获功能。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.sp_cdc_help_change_data_capture   
  [ [ @source_schema = ] 'source_schema' ]  
  [, [ @source_name = ] 'source_name' ]  
```  
  
## <a name="arguments"></a>参数  
 [ @source_schema =] "*source_schema*"  
 源表所属架构的名称。 *source_schema* 的默认值为 **sysname**，默认值为 NULL。 指定 *source_schema* 时，还必须指定 *source_name* 。  
  
 如果非 NULL，则当前数据库中必须存在 *source_schema* 。  
  
 如果 *source_schema* 为非 null，则 *source_name* 也必须为非 null。  
  
 [ @source_name =] "*source_name*"  
 源表的名称。 *source_name* 的默认值为 **sysname**，默认值为 NULL。 指定 *source_name* 时，还必须指定 *source_schema* 。  
  
 如果非 NULL，则当前数据库中必须存在 *source_name* 。  
  
 如果 *source_name* 为非 null，则 *source_schema* 也必须为非 null。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|源表架构的名称。|  
|source_table|**sysname**|源表的名称。|  
|capture_instance|**sysname**|捕获实例的名称。|  
|object_id|**int**|与源表关联的更改表的 ID。|  
|source_object_id|**int**|源表的 ID。|  
|start_lsn|**binary(10)**|日志序列号 (LSN)，表示用于查询更改表的低端点。<br /><br /> NULL = 尚未建立低端点。|  
|end_lsn|**binary(10)**|LSN，表示用于查询更改表的高端点。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中，此列始终为 NULL。|  
|supports_net_changes|**bit**|已启用净更改支持。|  
|has_drop_pending|**bit**|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中不使用。|  
|role_name|**sysname**|用于控制对更改数据的访问的数据库角色的名称。<br /><br /> NULL = 未使用角色。|  
|index_name|**sysname**|用于唯一标识源表中的行的索引名称。|  
|filegroup_name|**sysname**|更改表所驻留的文件组的名称。<br /><br /> NULL = 更改表在数据库的默认文件组中。|  
|create_date|**datetime**|启用捕获实例的日期。|  
|index_column_list|**nvarchar(max)**|用于唯一标识源表中的行的索引列的列表。|  
|captured_column_list|**nvarchar(max)**|已捕获的源列的列表。|  
  
## <a name="remarks"></a>备注  
 如果 *source_schema* 和 *source_name* 都默认为 null，或者显式设置为 null，则此存储过程将返回调用方有权访问的所有数据库捕获实例的信息。 当 *source_schema* 且 *SOURCE_NAME* 为非 NULL 时，只返回有关特定命名启用表的信息。  
  
## <a name="permissions"></a>权限  
 当 *source_schema* 和 *source_name* 为 NULL 时，调用方的授权将确定哪些启用的表包含在结果集中。 调用方必须对捕获实例的所有捕获列拥有 SELECT 权限，还要有任何所定义的门户角色中的成员身份，才能获得要包括的表信息。 db_owner 数据库角色的成员可以查看有关所有定义的捕获实例的信息。 在请求特定的启用表的信息时，相同的 SELECT 和成员身份条件将应用于命名表。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-change-data-capture-configuration-information-for-a-specified-table"></a>A. 返回指定表的变更数据捕获配置信息  
 下面的示例返回 `HumanResources.Employee` 表的变更数据捕获配置。  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_help_change_data_capture   
    @source_schema = N'HumanResources',   
    @source_name = N'Employee';  
GO  
```  
  
### <a name="b-returning-change-data-capture-configuration-information-for-all-tables"></a>B. 返回所有表的变更数据捕获配置信息  
 以下示例返回数据库中包含调用方已获访问授权的更改数据的所有启用表的配置信息。  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_help_change_data_capture;  
GO  
```  
  
