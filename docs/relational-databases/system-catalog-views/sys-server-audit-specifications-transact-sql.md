---
description: sys.server_audit_specifications (Transact-SQL)
title: sys.server_audit_specifications (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- server_audit_specifications_TSQL
- sys.server_audit_specifications_TSQL
- server_audit_specifications
- sys.server_audit_specifications
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_audit_specifications catalog view
ms.assetid: fa496c6c-2a54-4fda-a238-db490c6b3afd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bb9a5ccce39364cad6083b1fd9ca317dc4ba6aa0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097939"
---
# <a name="sysserver_audit_specifications-transact-sql"></a>sys.server_audit_specifications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含有关服务器实例上 SQL Server 审核中的服务器审核规范的信息。 有关 SQL Server Audit 的详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**Sysname**|服务器规范的名称。|  
|**server_specification_id**|**Int**|**Server_specification** 的 ID。|  
|create_date|**日期时间**|审核服务器规范的创建日期。|  
|**modified_date**|**日期时间**|审核服务器规范的上次修改日期。|  
|**is_state_enabled**|**tinyint**|审核规范状态：<br /><br /> 0-已禁用<br /><br /> 1-已启用|  
|**audit_GUID**|**uniqueidentifier**|包含此规范的审核的 GUID。 在服务器启动期间成员服务器审核规范的枚举过程中使用。|  
  
## <a name="permissions"></a>权限  
 具有 **ALTER ANY SERVER AUDIT**  或 **VIEW any DEFINITION** 权限的主体有权访问此目录视图。 此外，不能拒绝主体 **VIEW ANY DEFINITION** 权限。  
  
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
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [创建服务器审核和服务器审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
