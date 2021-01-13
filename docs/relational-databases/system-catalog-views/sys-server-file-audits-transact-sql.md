---
description: sys.server_file_audits (Transact-SQL)
title: sys.server_file_audits (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_file_audits_TSQL
- sys.server_file_audits_TSQL
- sys.server_file_audits
- server_file_audits
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_file_audits catalog view
ms.assetid: 553288a0-be57-4d79-ae53-b7cbd065e127
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b72b618559f02fed01e1760168f72ea4a12065ae
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151278"
---
# <a name="sysserver_file_audits-transact-sql"></a>sys.server_file_audits (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务器实例上审核中的文件审核类型的扩展信息。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|audit_id|**int**|审核的 ID。|  
|name|**sysname**|审核的名称。|  
|audit_guid|**uniqueidentifier**|审核的 GUID。|  
|create_date|**datetime**|创建文件审核的 UTC 日期。|  
|modify_date|**datatime**|上次修改文件审核的 UTC 日期。|  
|principal_id|**int**|在服务器上注册的审核的所有者的 ID。|  
|type|**char(2)**|审核类型：<br /><br /> SL = NT 安全事件日志<br /><br /> AL = NT 应用程序事件日志<br /><br /> FL = 文件系统上的文件|  
|type_desc|**nvarchar(60)**|审核类型说明。|  
|on_failure|**tinyint**|失败时的状态：<br /><br /> 0 = 继续<br /><br /> 1 = 关闭服务器实例<br /><br /> 2 = 失败操作|  
|on_failure_desc|**nvarchar(60)**|失败时要写入的操作项：<br /><br /> CONTINUE<br /><br /> SHUTDOWN SERVER INSTANCE<br /><br /> FAIL OPERATION|  
|is_state_enabled|**tinyint**|0 - 禁用<br /><br /> 1 = 启用|  
|queue_delay|**int**|写入磁盘前建议等待的最长时间（以毫秒为单位）。 如果为 0，则审核将确保在事件可以继续之前进行写入。|  
|predicate|**nvarchar (8000)**|应用于事件的谓词表达式。|  
|max_file_size|**bigint**|审核的最大大小（以 MB 为单位）：<br /><br /> 0 = 无限/不适用于所选的审核类型。|  
|max_rollover_files|**int**|要用于滚动更新选项的文件的最大数目。|  
|max_files|**int**|在没有滚动更新选项时要使用的文件的最大数目。|  
|reserved_disk_space|**int**|按文件保留的磁盘空间量。|  
|log_file_path|**nvarchar(260)**|审核所在的路径。 对于文件审核为文件路径，对于应用程序日志审核为应用程序日志路径。|  
|log_file_name|**nvarchar(260)**|CREATE AUDIT DDL 中提供的日志文件的基名称。 创建日志文件名时向 base_log_name 文件中添加一个递增数字作为后缀。|  
  
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
 [sys.server_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-sql) ](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [创建服务器审核和服务器审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
