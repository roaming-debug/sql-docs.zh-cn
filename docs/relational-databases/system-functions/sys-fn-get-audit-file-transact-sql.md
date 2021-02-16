---
description: sys.fn_get_audit_file (Transact-SQL)
title: sys.fn_get_audit_file (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 02/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_audit_file_TSQL
- sys.fn_get_audit_file_TSQL
- fn_get_audit_file
- sys.fn_get_audit_file
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_get_audit_file function
- fn_get_audit_file function
ms.assetid: d6a78d14-bb1f-4987-b7b6-579ddd4167f5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: 013586570ce43b8270ae2613c05ced8bc9bdc48b
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525211"
---
# <a name="sysfn_get_audit_file-transact-sql"></a>sys.fn_get_audit_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]    

  从服务器审核在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中创建的审核文件返回信息。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
fn_get_audit_file ( file_pattern,   
    { default | initial_file_name | NULL },   
    { default | audit_record_offset | NULL } )  
```  
  
## <a name="arguments"></a>自变量  
 *file_pattern*  
 指定要读取的审核文件集的目录（或路径）和文件名。 类型为 **nvarchar (260)**。 
 
 - **SQL Server**：
    
    此参数必须包括路径（驱动器盘符或网络共享）和文件名，可以包含通配符。 单个星号 ( * ) 可用于从审核文件集中收集多个文件。 例如：  
  
    -   **\<path>\\\*** -收集指定位置中的所有审核文件。  
  
    -   **\<path> \ LOGINSAUDIT_ {GUID}***-收集具有指定名称和 GUID 对的所有审核文件。  
  
    -   **\<path> \ LOGINSAUDIT_ {GUID} _00_29384. .Sqlaudit** -收集特定的审核文件。  
  
 - **AZURE SQL 数据库**：
 
    此参数用于指定 (包括存储终结点和容器) 的 blob URL。 虽然它不支持星号通配符，但你可以使用部分文件 (blob) 名称前缀 (而不是完整的 blob 名称) 来收集以此前缀开头 (blob) 的多个文件。 例如：
 
      - **\<Storage_endpoint\>/\<Container\>/\<ServerName\>/\<DatabaseName\>/** -收集特定数据库 (blob) 的所有审核文件。    
      
      - **\<Storage_endpoint\> / \<Container\> / \<ServerName\> / \<DatabaseName\> / \<AuditName\> / \<CreationDate\> / \<FileName\> . .xel** - (blob) 收集特定的审核文件。
  
> [!NOTE]  
>  在无文件名模式的情况下传递路径将生成错误。  
  
 *initial_file_name*  
 指定审核文件集中要开始读取审核记录的特定文件的路径和名称。 类型为 **nvarchar (260)**。  
  
> [!NOTE]  
>  *Initial_file_name* 参数必须包含有效条目，或者必须包含默认值 |NULL 值。  
  
 *audit_record_offset*  
 指定一个已知位置，该位置包含为 initial_file_name 指定的文件。 使用此参数时，函数将从缓冲区中紧跟指定偏移量之后的第一个记录开始读取。  
  
> [!NOTE]  
>  *Audit_record_offset* 参数必须包含有效条目，或者必须包含默认值 |NULL 值。 类型为 **bigint**。  
  
## <a name="tables-returned"></a>返回的表  
 下表描述此函数可返回的审核文件内容。  
  
| 列名称 | 类型 | 描述 |  
|-------------|------|-------------|  
| action_id | **varchar(4)** | 操作的 ID。 不可为 Null。 |  
| additional_information | **nvarchar(4000)** | 仅适用于单个事件的唯一信息，以 XML 的形式返回。 有少量的可审核操作包含此类信息。<br /><br /> 对于具有与操作相关联的 TSQL 堆栈的操作，将以 XML 格式显示一个级别的 TSQL 堆栈。 该 XML 格式如下：<br /><br /> `<tsql_stack><frame nest_level = '%u' database_name = '%.*s' schema_name = '%.*s' object_name = '%.*s' /></tsql_stack>`<br /><br /> Frame nest_level 指示框架的当前嵌套级别。 模块名称表示为由三部分组成的格式（database_name、schema_name 和 object_name）。  模块名称将被解析为对无效的 xml 字符（如、、、）进行转义 `'\<'` `'>'` `'/'` `'_x'` 。 它们将被转义为 `_xHHHH\_` 。 HHHH 代表该字符对应的四位十六进制 UCS-2 代码。<br /><br /> 可以为 Null。 如果事件没有报告其他信息，则返回 NULL。 |
| affected_rows | **bigint** | **适用** 于：仅适用于 Azure SQL 数据库<br /><br /> 受执行语句影响的行数。 |  
| application_name | **nvarchar(128)** | **适用** 于： Azure SQL 数据库 + SQL Server 从2017开始 () <br /><br /> 执行导致审核事件的语句的客户端应用程序的名称 |  
| audit_file_offset | **bigint** | **适用** 于：仅 SQL Server<br /><br /> 包含审核记录的文件中的缓冲区偏移量。 不可为 null。 |  
| audit_schema_version | **int** | 始终为 1 |  
| class_type | **varchar(2)** | 发生审核的可审核实体的类型。 不可为 null。 |  
| client_ip | **nvarchar(128)** | **适用** 于： Azure SQL 数据库 + SQL Server 从2017开始 () <br /><br />  客户端应用程序的源 IP |  
| connection_id | GUID | **适用** 于： Azure SQL 数据库和 SQL 托管实例<br /><br /> 服务器中的连接的 ID |
| data_sensitivity_information | nvarchar(4000) | **适用** 于：仅适用于 Azure SQL 数据库<br /><br /> 受审核查询根据数据库中分类的列返回的信息类型和敏感度标签。 详细了解 [Azure SQL 数据库数据发现和分类](/azure/sql-database/sql-database-data-discovery-and-classification) |
| database_name | **sysname** | 发生此操作的数据库上下文。 可以为 Null。 对于在服务器级别发生的审核，返回 NULL。 |  
| database_principal_id | **int** |在其中执行操作的数据库用户上下文 ID。 不可为 null。 如果此不适用，则返回0。 例如，如果是服务器操作，则返回 0。|
| database_principal_name | **sysname** | 当前用户。 可以为 Null。 如果不可用，则返回 NULL。 |  
| duration_milliseconds | **bigint** | **适用** 于： Azure SQL 数据库和 SQL 托管实例<br /><br /> 查询执行持续时间，以毫秒为单位 |
| event_time | **datetime2** | 触发可审核操作的日期和时间。 不可为 null。 |  
| file_name | **varchar(260)** | 作为记录来源的审核日志文件的路径和名称。 不可为 null。 |
| is_column_permission | **bit** | 指示是否为列级权限的标志。 不可为 null。 当 permission_bitmask = 0 时返回 0。<br /> 1 = true<br /> 0 = false |
| object_id | **int** | 发生审核的实体的 ID。 这包括：<br /> 服务器对象<br /> 数据库<br /> 数据库对象<br /> 架构对象<br /> 不可为 null。 如果实体是服务器本身或者没有在对象级别执行审核，则返回 0。 例如，对于 Authentication，则返回 NULL。 |  
| object_name | **sysname** | 发生审核的实体的名称。 这包括：<br /> 服务器对象<br /> 数据库<br /> 数据库对象<br /> 架构对象<br /> 可以为 Null。 如果实体是 Server 自身或者没有在对象级别执行审核，则返回 NULL。 例如，对于 Authentication，则返回 NULL。 |
| permission_bitmask | **varbinary(16)** | 在某些操作中，这是授予、拒绝或撤消的权限。 |
| response_rows | **bigint** | **适用** 于： Azure SQL 数据库和 SQL 托管实例<br /><br /> 在结果集中返回的行数。 |  
| schema_name | **sysname** | 在其中执行操作的架构上下文。 可以为 Null。 对于在架构外发生的审核，返回 NULL。 |  
| sequence_group_id | **varbinary** | **适用** 于：仅 SQL Server 从2016开始 () <br /><br />  唯一标识符 |  
| sequence_number | **int** | 跟踪单个审核记录中的记录顺序，该记录太大而无法放在写入缓冲区中以进行审核。 不可为 null。 |  
| server_instance_name | **sysname** | 发生审核的服务器实例的名称。 使用标准服务器\实例格式。 |  
| server_principal_id | **int** | 在其中执行操作的登录上下文 ID。 不可为 null。 |  
| server_principal_name | **sysname** | 当前登录名。 可以为 Null。 |  
| server_principal_sid | **varbinary** | 当前登录名 SID。 可以为 Null。 |  
| session_id | **smallint** | 发生该事件的会话的 ID。 不可为 null。 |  
| session_server_principal_name | **sysname** | 会话的服务器主体。 可以为 Null。 如果存在显式或隐式上下文切换，则返回连接到 SQL Server 实例的原始登录名的标识。|  
| statement | **nvarchar(4000)** | TSQL 语句（如果存在）。 可以为 Null。 如果不适用，则返回 NULL。 |  
| 成功 | **bit** | 指示触发事件的操作是否成功。 不可为 null。 对于除登录事件之外的所有事件，它仅报告权限检查（而不是操作）成功或失败。<br /> 1 = success<br /> 0 = 失败 |
| target_database_principal_id | **int** | 执行 GRANT/DENY/REVOKE 操作的数据库主体。 不可为 null。 如果不适用，则返回 0。 |  
| target_database_principal_name | **sysname** | 操作的目标用户。 可以为 Null。 如果不适用，则返回 NULL。 |  
| target_server_principal_id | **int** | 执行 GRANT/DENY/REVOKE 操作的服务器主体。 不可为 null。 如果不适用，则返回 0。 |  
| target_server_principal_name | **sysname** | 操作的目标登录名。 可以为 Null。 如果不适用，则返回 NULL。 |  
| target_server_principal_sid | **varbinary** | 目标登录名的 SID。 可以为 Null。 如果不适用，则返回 NULL。 |  
| transaction_id | **bigint** | **适用** 于：仅 SQL Server 从2016开始 () <br /><br /> 用于在一个事务中标识多个审核事件的唯一标识符 |  
| user_defined_event_id | **smallint** | **适用** 于： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本、Azure SQL 数据库和 SQL 托管实例<br /><br /> 作为参数传递给 **sp_audit_write** 的用户定义事件 id。 对于系统事件为 NULL (默认) 值为 **NULL** ，对于用户定义的事件则为非零。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_audit_write ](../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md)。 |  
| user_defined_information | **nvarchar(4000)** | **适用** 于： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本、Azure SQL 数据库和 SQL 托管实例<br /><br /> 用于记录用户要使用 **sp_audit_write** 存储过程在审核日志中记录的任何其他信息。 |  

  
## <a name="remarks"></a>备注  
- 如果传递到 **fn_get_audit_file** 的 *file_pattern* 参数引用的路径或文件不存在，或者该文件不是审核文件，则返回 **MSG_INVALID_AUDIT_FILE** 错误消息。  
- 使用 **APPLICATION_LOG**、 **SECURITY_LOG** 或 **EXTERNAL_MONITOR** 选项创建审核时，不能使用 **fn_get_audit_file** 。

## <a name="permissions"></a>权限

- **SQL Server**：需要 **CONTROL Server** 权限。  
- **AZURE SQL 数据库**：需要 **CONTROL Database** 权限。     
  - 服务器管理员可以访问服务器上所有数据库的审核日志。
  - 非服务器管理员只能从当前数据库访问审核日志。
  - 将跳过不满足上述条件的 blob， (将在查询输出消息) 中显示跳过的 blob 的列表，并且函数将仅返回允许访问的 blob 中的日志。  
  
## <a name="examples"></a>示例

- **SQL Server**

  此示例从名为 `\\serverName\Audit\HIPAA_AUDIT.sqlaudit` 的文件读取。  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('\\serverName\Audit\HIPAA_AUDIT.sqlaudit',default,default);  
  GO  
  ```  

- **Azure SQL 数据库**

  下面的示例从名为的文件中读取 `ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel` ：  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default);
  GO  
  ```  

  此示例从同一文件中读取，但使用其他 T-sql 子句 (**TOP**、 **ORDER BY** 和 **WHERE** 子句来筛选函数返回的审核记录) ：
  
  ```  
  SELECT TOP 10 * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default)
  WHERE server_principal_name = 'admin1'
  ORDER BY event_time
  GO
  ```  

  此示例从以开头的服务器读取所有审核日志 `Sh` ： 
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/Sh',default,default);
  GO  
  ```

有关如何创建审核的完整示例，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。

有关设置 Azure SQL 数据库审核的信息，请参阅 [SQL 数据库审核入门](/azure/sql-database/sql-database-auditing)。
  
## <a name="see-also"></a>另请参阅  
 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.server_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [创建服务器审核和服务器审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
