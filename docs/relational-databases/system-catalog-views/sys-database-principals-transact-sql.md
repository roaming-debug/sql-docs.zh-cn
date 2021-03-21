---
description: sys.database_principals (Transact-SQL)
title: sys.database_principals (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_principals
- database_principals_TSQL
- sys.database_principals
- sys.database_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_principals catalog view
ms.assetid: 8cb239e9-eb8c-4109-9cec-0d35de95fa0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed859905d902edc2d6244425c473a0f6755a888c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752237"
---
# <a name="sysdatabase_principals-transact-sql"></a>sys.database_principals (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的每个安全主体返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|主体名称，在数据库中唯一。|  
|principal_id|**int**|主体 ID，在数据库中唯一。|  
|type |**char(1)**|主体类型：<br /><br /> A = 应用程序角色<br /><br /> C = 映射到证书的用户<br /><br /> E = 来自 Azure Active Directory 的外部用户<br /><br /> G = Windows 组<br /><br /> K = 映射到非对称密钥的用户<br /><br /> R = 数据库角色<br /><br /> S = SQL 用户<br /><br /> U = Windows 用户<br /><br /> X = Azure Active Directory 组或应用程序中的外部组|  
|**type_desc**|**nvarchar(60)**|主体类型的说明。<br /><br /> APPLICATION_ROLE<br /><br /> CERTIFICATE_MAPPED_USER<br /><br /> EXTERNAL_USER<br /><br /> WINDOWS_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_USER<br /><br /> DATABASE_ROLE<br /><br /> SQL_USER<br /><br /> WINDOWS_USER<br /><br /> EXTERNAL_GROUPS|  
|**default_schema_name**|**sysname**|SQL 名称未指定架构时要使用的名称。 对于非 S、U 或 A 类型的主体，为 Null。|  
|create_date|**datetime**|主体的创建时间。|  
|modify_date|**datetime**|上次修改主体的时间。|  
|**owning_principal_id**|**int**|拥有此主体的主体的 ID。 默认情况下，所有固定数据库角色均归 **dbo** 所有。|  
|**sid**|**varbinary(85)**|SID (主体的安全标识符) 。  SYS 和 INFORMATION SCHEMAS 为 NULL。|  
|**is_fixed_role**|**bit**|如果为 1，则该行表示与下面的某个固定数据库角色对应的条目：db_owner、db_accessadmin、db_datareader、db_datawriter、db_ddladmin、db_securityadmin、db_backupoperator、db_denydatareader、db_denydatawriter。|  
|**authentication_type**|**int**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指示身份验证类型。 下面是可能的值及其说明。<br /><br /> 0：无身份验证<br />1：实例身份验证<br />2：数据库身份验证<br />3： Windows 身份验证<br />4： Azure Active Directory 身份验证|  
|**authentication_type_desc**|**nvarchar(60)**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 身份验证类型说明。 下面是可能的值及其说明。<br /><br /> 无：无身份验证<br />实例：实例身份验证<br />数据库：数据库身份验证<br />WINDOWS： Windows 身份验证<br />外部： Azure Active Directory 身份验证|  
|**default_language_name**|**sysname**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指示此主体的默认语言。|  
|**default_language_lcid**|**int**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指示此主体的默认 LCID。|  
|**allow_encrypted_value_modifications**|**bit**|**适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 取消在大容量复制操作期间对服务器进行加密元数据检查。 这样，用户便可以使用 Always Encrypted、在表或数据库之间对数据进行大容量复制，而无需解密数据。 默认为 OFF。 |      
  
## <a name="remarks"></a>备注  
 *PasswordLastSetTime* 属性可用于 SQL Server 的所有受支持的配置，但其他属性仅在 SQL Server 在 Windows Server 2003 或更高版本上运行并且启用了 CHECK_POLICY 和 CHECK_EXPIRATION 时才可用。 有关详细信息，请参阅 [密码策略](../../relational-databases/security/password-policy.md) 。
如果主体已删除，因此可以重复使用 principal_id 的值，因此不一定会不断增加。
  
## <a name="permissions"></a>权限  
 任何用户都可以查看自己的用户名称、系统用户和固定的数据库角色。 要查看其他用户，需要获取 ALTER ANY USER 或相关的用户权限。 要查看用户定义的角色，需要获取 ALTER ANY ROLE 或相关的角色成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-all-the-permissions-of-database-principals"></a>A：列出数据库主体的所有权限  
 以下查询将列出明确对数据库主体授予或拒绝的权限。  
  
> [!IMPORTANT]  
>  固定数据库角色的权限不会出现在 sys.database_permissions 中。 因此，数据库主体可能具有此处未列出的其他权限。  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="b-listing-permissions-on-schema-objects-within-a-database"></a>B：列出对数据库中架构对象的权限  
 以下查询将 sys.database_principals 和 sys.database_permissions 与 sys.objects 和 sys.schemas 联接，以列出对特定架构对象授予或拒绝的权限。  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-listing-all-the-permissions-of-database-principals"></a>C：列出数据库主体的所有权限  
 以下查询将列出明确对数据库主体授予或拒绝的权限。  
  
> [!IMPORTANT]  
>  固定数据库角色的权限不显示在中 `sys.database_permissions` 。 因此，数据库主体可能具有此处未列出的其他权限。  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="d-listing-permissions-on-schema-objects-within-a-database"></a>D：列出数据库中架构对象的权限  
 下面的查询联接 `sys.database_principals` 和 `sys.database_permissions` 以 `sys.objects` `sys.schemas` 列出向特定架构对象授予或拒绝的权限。  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="see-also"></a>另请参阅  
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [包含的数据库用户-使你的数据库可移植](../../relational-databases/security/contained-database-users-making-your-database-portable.md)   
 [使用 Azure Active Directory 身份验证连接到 SQL 数据库](/azure/azure-sql/database/authentication-aad-overview)  
  
