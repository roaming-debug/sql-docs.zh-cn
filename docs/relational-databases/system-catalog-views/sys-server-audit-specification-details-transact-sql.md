---
description: sys.server_audit_specification_details (Transact-SQL)
title: sys.server_audit_specification_details (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_audit_specification_details
- sys.server_audit_specification_details_TSQL
- server_audit_specification_details_TSQL
- sys.server_audit_specification_details
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_audit_specification_details catalog view
ms.assetid: 792724dc-402e-4b17-9f2c-029d910bf88e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 714a6804a75342b732d88bd0442997fc8be362f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097938"
---
# <a name="sysserver_audit_specification_details-transact-sql"></a>sys.server_audit_specification_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含服务器实例上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 审核中的服务器审核规范详细信息（操作）的相关信息。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。 有关所有 audit_action_id 及其名称的列表，请 [&#40;transact-sql&#41;查询 sys.dm_audit_actions ](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|server_specification_id|**int**|审核服务器规范的 ID|  
|audit_action_id|**int**|审核操作的 ID|  
|audit_action_name|**sysname**|组名称或审核操作名称|  
|class|**tinyint**|保留|  
|class_desc|**nvarchar(60)**|保留|  
|major_id|**int**|保留|  
|minor_id|**int**|保留|  
|audited_principal_id|**int**|保留|  
|audited_result|**nvarchar(60)**|审核结果：<br /><br /> - SUCCESS AND FAILURE<br /><br /> - SUCCESS<br /><br /> - FAILURE|  
|is_group|**bit**|审核对象是否为组：<br /><br /> 0 - 不是组<br /><br /> 1 - 组|  
  
## <a name="permissions"></a>权限  
 具有 **ALTER ANY SERVER AUDIT** 或 **VIEW any DEFINITION** 权限的主体有权访问此目录视图。 此外，不能拒绝主体 **VIEW ANY DEFINITION** 权限。  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
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
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [创建服务器审核和服务器审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
