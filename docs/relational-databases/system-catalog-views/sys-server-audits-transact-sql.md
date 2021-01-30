---
description: sys.server_audits (Transact-SQL)
title: sys.server_audits (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_audits_TSQL
- sys.server_audits_TSQL
- sys.server_audits
- server_audits
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_audits catalog view
ms.assetid: c2c4a000-1127-46a8-b1e9-947fd1136e1e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6c7dba3282bf75e3c707be7d861f2051b56c6e0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172701"
---
# <a name="sysserver_audits-transact-sql"></a>sys.server_audits (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  服务器实例中每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 审核都各占一行。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**audit_id**|**int**|审核的 ID。|  
|name|**sysname**|审核的名称。|  
|**audit_guid**|**uniqueidentifier**|用于在服务器启动和数据库附加操作期间使用成员服务器枚举审核&#124;数据库审核规范的审核的 GUID。|  
|create_date|**datetime**|创建审核的 UTC 日期。|  
|modify_date|**datetime**|上次修改审核的 UTC 日期。|  
|principal_id|**int**|向服务器注册的审核所有者的 ID。|  
|type|**char(2)**|审核类型：<br /><br /> SL-NT 安全事件日志<br /><br /> AL-NT 应用程序事件日志<br /><br /> 文件系统上的 FL 文件|  
|**type_desc**|**nvarchar(60)**|SECURITY LOG<br /><br /> APPICATION LOG<br /><br /> FILE|  
|**on_failure**|**tinyint**|失败时要写入的操作项：<br /><br /> 0-继续<br /><br /> 1-关闭服务器实例<br /><br /> 2-失败操作|  
|**on_failure_desc**|**nvarchar(60)**|失败时要写入的操作项：<br /><br /> CONTINUE<br /><br /> SHUTDOWN SERVER INSTANCE<br /><br /> FAIL_OPERATION|  
|**is_state_enabled**|**tinyint**|0-已禁用<br /><br /> 1 - 启用|  
|**queue_delay**|**int**|写入磁盘前等待的最长时间（以毫秒为单位）。 如果为0，则审核将保证写入，然后事件才能继续。|  
|**predicate**|**nvarchar (3000)**|应用于事件的谓词表达式。|  
  
## <a name="permissions"></a>权限  
 具有 **ALTER ANY SERVER AUDIT** 或 **VIEW any DEFINITION** 权限的主体有权访问此目录视图。 此外，不能拒绝主体 **VIEW ANY DEFINITION** 权限。  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
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
 [sys.fn_get_audit_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [创建服务器审核和服务器审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
