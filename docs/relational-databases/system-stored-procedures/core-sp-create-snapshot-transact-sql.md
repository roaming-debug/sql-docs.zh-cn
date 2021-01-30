---
description: core.sp_create_snapshot (Transact-SQL)
title: core.sp_create_snapshot (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_snapshot
- sp_create_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- management data warehouse, data collector stored procedures
- data collector [SQL Server], stored procedures
- core.sp_create_snapshot stored procedure
- sp_create_snapshot
ms.assetid: ff297bda-0ee2-4fda-91c8-7000377775e3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 737739cfa627e6668d95e6453d66ed1bad4ad637
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210506"
---
# <a name="coresp_create_snapshot-transact-sql"></a>core.sp_create_snapshot (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在管理数据仓库的 core.snapshots 视图中插入一行。 每当上载包开始向管理数据仓库上载数据时，都会调用此过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
core.sp_create_snapshot [ @collection_set_uid = ] 'collection_set_uid'  
    , [ @collector_type_uid = ] 'collector_type_uid'  
    ,[ @machine_name = ] 'machine_name'  
    , [ @named_instance = ] 'named_instance'  
    , [ @log_id = ] log_id  
    , [ @snapshot_id = ] snapshot_id OUTPUT  
```  
  
## <a name="arguments"></a>参数  
 [ @collection_set_uid =] "*collection_set_uid*"  
 收集组的 GUID。 *collection_set_uid* 为 **uniqueidentifier** ，无默认值。 若要获取 GUID，请查询 msdb 数据库中的 dbo.syscollector_collection_sets 视图。  
  
 [ @collector_type_uid =] "*collector_type_uid*"  
 收集器类型的 GUID。 *collector_type_uid* 为 **uniqueidentifier** ，无默认值。 若要获取 GUID，请查询 msdb 数据库中的 dbo.syscollector_collector_types 视图。  
  
 [ @machine_name =] "*machine_name*"  
 收集组所在的服务器的名称。 *machine_name* 是 **sysname**，没有默认值。  
  
 [ @named_instance =] "*named_instance*"  
 收集组实例的名称。 *named_instance* 是 **sysname**，没有默认值。  
  
 [ @log_id =] *log_id*  
 映射到收集数据的服务器上的收集组事件日志的唯一标识符。 *log_id* 为 **bigint** ，无默认值。 若要获取 *log_id* 的值，请查询 msdb 数据库中的 dbo.syscollector_execution_log 视图。  
  
 [ @snapshot_id =] *snapshot_id*  
 插入到 core 快照视图中的行的唯一标识符。 *snapshot_id* 为 **int** ，并作为 OUTPUT 返回。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 每当上载包开始向管理数据仓库上载数据时，数据收集器运行时组件都会调用 core.sp_create_snapshot。  
  
 此过程进行以下检查：  
  
-   collection_set_uid 是否与 core.source_info_internal 表中的某个现有条目匹配。  
  
-   collector_type_uid 是否与 core.supported_collector_types 视图中的某个现有条目匹配。  
  
 如果未通过上述任一检查，该过程将失败并返回一个错误。  
  
## <a name="permissions"></a>权限  
 需要具有 EXECUTE 权限的 **mdw_writer** (中的成员资格) 固定数据库角色。  
  
## <a name="examples"></a>示例  
 下面的示例为“磁盘使用情况”收集组创建一个快照，将该快照添加到管理数据仓库中，并返回快照标识符。 在本示例中，使用默认实例。  
  
```  
USE <management_data_warehouse>;  
DECLARE @snapshot_id int;  
EXEC core.sp_create_snapshot   
    @collection_set_uid = '7B191952-8ECF-4E12-AEB2-EF646EF79FEF',   
    @collector_type_uid = '302E93D1-3424-4BE7-AA8E-84813ECF2419',  
    @machine_name = '<computername>',  
    @named_instance = 'MSSQLSERVER',  
    @log_id = 11, -- ID of the log for the collection set  
    @snapshot_id = @snapshot_id OUTPUT;  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [管理数据仓库](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
