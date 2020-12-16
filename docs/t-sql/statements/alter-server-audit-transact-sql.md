---
description: ALTER SERVER AUDIT (Transact-SQL)
title: ALTER SERVER AUDIT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SERVER_AUDIT_TSQL
- ALTER SERVER AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- server audit [SQL Server]
- audits [SQL Server], specification
- ALTER SERVER AUDIT statement
ms.assetid: 63426d31-7a5c-4378-aa9e-afcf4f64ceb3
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 0239db3ed1005d3238853ffd5f058174be823b4e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462198"
---
# <a name="alter-server-audit--transact-sql"></a>ALTER SERVER AUDIT (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit 功能更改服务器审核对象。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER SERVER AUDIT audit_name  
{  
    [ TO { { FILE ( <file_options> [, ...n] ) } | APPLICATION_LOG | SECURITY_LOG } | URL]  
    [ WITH ( <audit_options> [ , ...n] ) ]   
    [ WHERE <predicate_expression> ]  
}  
| REMOVE WHERE  
| MODIFY NAME = new_audit_name  
[ ; ]  
  
<file_options>::=  
{  
      FILEPATH = 'os_file_path'   
    | MAXSIZE = { max_size { MB | GB | TB } | UNLIMITED }   
    | MAX_ROLLOVER_FILES = { integer | UNLIMITED }   
    | MAX_FILES = integer   
    | RESERVE_DISK_SPACE = { ON | OFF }   
}  
  
<audit_options>::=  
{  
      QUEUE_DELAY = integer   
    | ON_FAILURE = { CONTINUE | SHUTDOWN | FAIL_OPERATION }   
    | STATE = = { ON | OFF }   
}  
  
<predicate_expression>::=  
{  
    [NOT ] <predicate_factor>   
    [ { AND | OR } [NOT ] { <predicate_factor> } ]   
    [,...n ]  
}  
  
<predicate_factor>::=   
    event_field_name { = | < > | ! = | > | > = | < | < = } { number | ' string ' }  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 TO { FILE \| APPLICATION_LOG \| SECURITY \|URL}  
 确定审核目标的位置。 可用的选项包括二进制文件、Windows 应用程序日志或 Windows 安全日志。  

> [!IMPORTANT]
> 在 Azure SQL 托管实例中，SQL 审核功能在服务器层级运行，并在 Azure Blob 存储中存储 `.xel` 文件。
  
 FILEPATH **= '** os\_file\_path **'**  
 审核记录的路径。 文件名是基于审核名称和审核 GUID 生成的。  
  
 MAXSIZE **=** max\_size  
 指定审核文件可增大到的最大大小。 max_size 值必须是后跟 MB、GB、TB 或 UNLIMITED 的整数   。 可以为 max_size 指定的最小大小为 2 MB，最大大小为 2,147,483,647 TB 。 如果指定为 UNLIMITED，则文件将增长到磁盘变满为止。 指定一个小于 2 MB 的值将引发错误 MSG_MAXSIZE_TOO_SMALL。 默认值为 UNLIMITED。  
  
 MAX_ROLLOVER_FILES =integer | UNLIMITED  
 指定要保留在文件系统中的最大文件数。 设置为 MAX_ROLLOVER_FILES=0 时，可创建的滚动更新文件的数量不受任何限制。 默认值为 0。 可以指定的最大文件数为 2,147,483,647。  
  
 MAX_FILES = integer  
 指定可创建的审核文件的最大数目。 当达到此限制时，不滚动更新到第一个文件。 在达到 MAX_FILES 限制时，导致生成附加审核事件的任何操作都会失败并报告错误。  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 RESERVE_DISK_SPACE = { ON | OFF }  
 此选项会按 MAXSIZE 值为磁盘上的文件预先分配大小。 仅在 MAXSIZE 不等于 UNLIMITED 时适用。 默认值为 OFF。  
  
 QUEUE_DELAY = integer  
 确定在强制处理审核操作之前可能经过的时间（以毫秒为单位）。 值 0 指示同步传递。 可设置的最小延迟值为 1000（1 秒），这是默认值。 最大值为 2,147,483,647（2,147,483.647 秒或者 24 天 20 小时 31 分钟 23.647 秒）。 指定无效数字将引发 MSG_INVALID_QUEUE_DELAY 错误。  
  
 ON_FAILURE = { CONTINUE | SHUTDOWN | FAIL_OPERATION}  
 指示在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法写入审核日志时写入目标的实例是应失败、继续还是停止。  
  
 CONTINUE  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 操作将继续。 审核记录将不会保留。 审核将继续尝试将事件记入日志，并且在故障条件得到解决后恢复。 选择继续选项可以允许未经审核的活动，但可能违反了你的安全策略。 在[!INCLUDE[ssDE](../../includes/ssde-md.md)]的继续操作比维护完整审核更重要时，使用此选项。  
  
SHUTDOWN  
如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 出于任何原因无法将数据写入到审核目标，则强制关闭 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 执行 `ALTER` 语句的登录名必须具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内的 `SHUTDOWN` 权限。 即使稍后从执行的登录名中撤销 `SHUTDOWN` 权限，关闭行为仍然存在。 如果用户不具有此权限，则语句失败且无法修改审核。 在审核失败可能损害系统的安全或完整性时，使用此选项。 有关详细信息，请参阅 [SHUTDOWN](../../t-sql/language-elements/shutdown-transact-sql.md)。 
  
 FAIL_OPERATION  
 如果数据库操作会导致审核的事件，则数据库操作将失败。 不会导致审核事件的操作可以继续，但也不会发生审核的事件。 审核将继续尝试将事件记入日志，并且在故障条件得到解决后恢复。 在维护完整审核比对[!INCLUDE[ssDE](../../includes/ssde-md.md)]的完全访问权限更重要时，使用此选项。  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。   
  
 STATE = { ON | OFF }  
 启用或禁用审核收集记录。 更改运行的审核的状态（从 ON 到 OFF）将创建审核停止时的审核项、停止审核的主体以及停止审核的时间。  
  
 MODIFY NAME = new_audit_name  
 更改审核的名称。 不能与任何其他选项一起使用。  
  
 predicate_expression  
 指定用于确定是否应处理事件的谓词表达式。 谓词表达式限制在 3000 个字符，这限制了字符串参数。  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 event_field_name  
 表示标识谓词源的事件字段的名称。 [ sys.fn_get_audit_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md) 中描述审核字段。 除 `file_name` 和 `audit_file_offset` 之外的所有字段都可以进行审核。  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 数字  
 任何数值类型，包括 decimal。 局限性在于缺少可用物理内存，或数值过大而无法用 64 位整数表示。  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 ' string '  
 进行谓词比较所需的 ANSI 字符串或 Unicode 字符串。 不为谓词比较函数执行隐式字符串类型转换。 传递错误类型会导致出错。  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
## <a name="remarks"></a>备注  
 调用 ALTER AUDIT 时，必须指定至少一个 TO、WITH 或 MODIFY NAME 子句。  
  
 为了更改审核，必须将审核的状态设置为 OFF 选项。 使用 STATE=OFF 以外的任何选项启用审核时，如果 ALTER AUDIT 正在运行，将接收到 MSG_NEED_AUDIT_DISABLED 错误消息。  
  
 无需停止审核即可添加、更改和删除审核规范。  
  
 创建审核后无法更改审核的 GUID。  
 
 在用户事务内不能使用 ALTER SERVER AUDIT 语句。
 
## <a name="permissions"></a>权限  
 若要创建、更改或删除服务器审核主体，必须具有 ALTER ANY SERVER AUDIT 或 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-a-server-audit-name"></a>A. 更改服务器审核名称  
 下面的示例将服务器审核 `HIPAA_Audit` 的名称更改为 `HIPAA_Audit_Old`。  
  
```sql  
USE master  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = OFF);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
MODIFY NAME = HIPAA_Audit_Old;  
GO  
ALTER SERVER AUDIT HIPAA_Audit_Old  
WITH (STATE = ON);  
GO  
```  
  
### <a name="b-changing-a-server-audit-target"></a>B. 更改服务器审核目标  
 下面的示例将名为 `HIPAA_Audit` 的服务器审核更改为以文件为目标。  
  
```sql  
USE master  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = OFF);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
TO FILE (FILEPATH ='\\SQLPROD_1\Audit\',  
          MAXSIZE = 1000 MB,  
          RESERVE_DISK_SPACE=OFF)  
WITH (QUEUE_DELAY = 1000,  
       ON_FAILURE = CONTINUE);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = ON);  
GO  
```  
  
### <a name="c-changing-a-server-audit-where-clause"></a>C. 更改服务器审核 WHERE 子句  
 下面的示例修改在 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md) 的示例 C 中创建的 where 子句。 如果为 27，则新的 WHERE 子句将筛选用户定义的事件。  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
WHERE user_defined_event_id = 27;  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = ON);  
GO  
```  
  
### <a name="d-removing-a-where-clause"></a>D. 删除 WHERE 子句  
 下面的示例将删除 WHERE 子句谓词表达式。  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
REMOVE WHERE;  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = ON);  
GO  
```  
  
### <a name="e-renaming-a-server-audit"></a>E. 重命名服务器审核  
 下面的示例将服务器审核名称从 `FilterForSensitiveData` 更改为 `AuditDataAccess`。  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
MODIFY NAME = AuditDataAccess;  
GO  
ALTER SERVER AUDIT [AuditDataAccess] WITH (STATE = ON);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [创建服务器审核和服务器审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
