---
description: sys.database_permissions (Transact-SQL)
title: sys.database_permissions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- database_permissions
- sys.database_permissions_TSQL
- database_permissions_TSQL
- sys.database_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_permissions catalog view
ms.assetid: c1e261f8-6cb0-4759-b5f1-5ec233602655
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0af3adae81e4f0bb9489e3534427dfe03efebf09
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475248"
---
# <a name="sysdatabase_permissions-transact-sql"></a>sys.database_permissions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为数据库中的每个权限或列异常权限返回一行。 对于列，每个权限有与相应的对象级别权限不同的一行。 如果列权限与相应的对象权限相同，则没有该行，并且应用的权限是对象的。  
  
> [!IMPORTANT]  
>  列级别权限的优先级高于同一实体的对象级别权限。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|标识权限所在的类。<br /><br /> 0 = 数据库<br />1 = 对象或列<br />3 = 架构<br />4 = 数据库主体<br />5 = 程序集- **适用** 于： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />6 = 类型<br />10 = XML 架构集合- <br />                      **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br />15 = 消息类型- **适用** 于： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />16 = 服务约定- **适用于**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />17 = 服务- **适用** 于： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />18 = 远程服务绑定- **适用于**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />19 = 路由- **适用** 于： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />23 = 全文目录- **适用于**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />24 = 对称密钥- **适用于**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />25 = 证书- **适用于**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。<br />26 = 非对称密钥- **适用于**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。|  
|**class_desc**|**nvarchar(60)**|权限所针对的类的说明。<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN<br /><br /> SCHEMA<br /><br /> DATABASE_PRINCIPAL<br /><br /> ASSEMBLY<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> MESSAGE_TYPE<br /><br /> SERVICE_CONTRACT<br /><br /> SERVICE<br /><br /> REMOTE_SERVICE_BINDING<br /><br /> ROUTE<br /><br /> FULLTEXT_CATALOG<br /><br /> SYMMETRIC_KEYS<br /><br /> CERTIFICATE<br /><br /> ASYMMETRIC_KEY|  
|**major_id**|**int**|存在权限的对象的 ID，根据类解释。 通常， **major_id** 只是适用于类所表示内容的 id 类型。 <br /><br /> 0 = 数据库本身 <br /><br /> >0 = 用于用户对象的 Object-IDs <br /><br /> \<0 = 系统对象 Object-IDs |  
|**minor_id**|**int**|存在权限的对象的辅助 ID，根据类解释。 通常， **minor_id** 为零，因为对象的类没有可用的子类别。 否则，它是表的列 ID。|  
|**grantee_principal_id**|**int**|向其授予权限的数据库主体 ID。|  
|**grantor_principal_id**|**int**|这些权限的授权者的数据库主体 ID。|  
|**type**|**char (4)**|数据库权限类型。 有关权限类型的列表，请参阅下一个表。|  
|**permission_name**|**nvarchar(128)**|权限名称。|  
|**state**|**char(1)**|权限状态：<br /><br /> D = 拒绝<br /><br /> R = 撤消<br /><br /> G = 授予<br /><br /> W = 带授权选项的授权|  
|**state_desc**|**nvarchar(60)**|权限状态的说明：<br /><br /> DENY<br /><br /> REVOKE<br /><br /> GRANT<br /><br /> GRANT_WITH_GRANT_OPTION|  

## <a name="database-permissions"></a>数据库权限   
下面是可能的权限类型。
  
|权限类型|权限名称|适用于安全对象|  
|---------------------|---------------------|--------------------------|  
|AADS |ALTER ANY DATABASE EVENT SESSION |DATABASE |  
|AAMK |ALTER ANY MASK |DATABASE |  
|AEDS |ALTER ANY EXTERNAL DATA SOURCE |DATABASE |  
|AEFF |ALTER ANY EXTERNAL FILE FORMAT |DATABASE |  
|AL|ALTER|APPLICATION ROLE、ASSEMBLY、ASYMMETRIC KEY、CERTIFICATE、CONTRACT、DATABASE、FULLTEXT CATALOG、MESSAGE TYPE、OBJECT、REMOTE SERVICE BINDING、ROLE、ROUTE、SCHEMA、SERVICE、SYMMETRIC KEY、USER、XML SCHEMA COLLECTION|  
|ALAK|ALTER ANY ASYMMETRIC KEY|DATABASE|  
|ALAR|ALTER ANY APPLICATION ROLE|DATABASE|  
|ALAS|ALTER ANY ASSEMBLY|DATABASE|  
|ALCF|ALTER ANY CERTIFICATE|DATABASE|  
|ALDS|ALTER ANY DATASPACE|DATABASE|  
|ALED|ALTER ANY DATABASE EVENT NOTIFICATION|DATABASE|  
|ALFT|ALTER ANY FULLTEXT CATALOG|DATABASE|  
|ALMT|ALTER ANY MESSAGE TYPE|DATABASE|  
|ALRL|ALTER ANY ROLE|DATABASE|  
|ALRT|ALTER ANY ROUTE|DATABASE|  
|ALSB|ALTER ANY REMOTE SERVICE BINDING|DATABASE|  
|ALSC|ALTER ANY CONTRACT|DATABASE|  
|ALSK|ALTER ANY SYMMETRIC KEY|DATABASE|  
|ALSM|ALTER ANY SCHEMA|DATABASE|  
|ALSV|ALTER ANY SERVICE|DATABASE|  
|ALTG|ALTER ANY DATABASE DDL TRIGGER|DATABASE|  
|ALUS|ALTER ANY USER|DATABASE|  
|AUTH|AUTHENTICATE|DATABASE|  
|BADB|BACKUP DATABASE|DATABASE|  
|BALO|BACKUP LOG|DATABASE|  
|CL|CONTROL|APPLICATION ROLE、ASSEMBLY、ASYMMETRIC KEY、CERTIFICATE、CONTRACT、DATABASE、FULLTEXT CATALOG、MESSAGE TYPE、OBJECT、REMOTE SERVICE BINDING、ROLE、ROUTE、SCHEMA、SERVICE、SYMMETRIC KEY、TYPE、USER、XML SCHEMA COLLECTION|  
|CO|CONNECT|DATABASE|  
|CORP|CONNECT REPLICATION|DATABASE|  
|CP|CHECKPOINT|DATABASE|  
|CRAG|CREATE AGGREGATE|DATABASE|  
|CRAK|CREATE ASYMMETRIC KEY|DATABASE|  
|CRAS|CREATE ASSEMBLY|DATABASE|  
|CRCF|CREATE CERTIFICATE|DATABASE|  
|CRDB|CREATE DATABASE|DATABASE|  
|CRDF|CREATE DEFAULT|DATABASE|  
|CRED|CREATE DATABASE DDL EVENT NOTIFICATION|DATABASE|  
|CRFN|CREATE FUNCTION|DATABASE|  
|CRFT|CREATE FULLTEXT CATALOG|DATABASE|  
|CRMT|CREATE MESSAGE TYPE|DATABASE|  
|CRPR|CREATE PROCEDURE|DATABASE|  
|CRQU|CREATE QUEUE|DATABASE|  
|CRRL|CREATE ROLE|DATABASE|  
|CRRT|CREATE ROUTE|DATABASE|  
|CRRU|CREATE RULE|DATABASE|  
|CRSB|CREATE REMOTE SERVICE BINDING|DATABASE|  
|CRSC|CREATE CONTRACT|DATABASE|  
|CRSK|CREATE SYMMETRIC KEY|DATABASE|  
|CRSM|CREATE SCHEMA|DATABASE|  
|CRSN|CREATE SYNONYM|DATABASE|  
|CRSO|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> CREATE SEQUENCE|DATABASE|  
|CRSV|CREATE SERVICE|DATABASE|  
|CRTB|CREATE TABLE|DATABASE|  
|CRTY|CREATE TYPE|DATABASE|  
|CRVW|CREATE VIEW|DATABASE|  
|CRXS|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。<br /><br /> CREATE XML SCHEMA COLLECTION|DATABASE|  
|DABO |ADMINISTER DATABASE BULK OPERATIONS | DATABASE |
|DL|DELETE|DATABASE、OBJECT、SCHEMA|  
|EAES |EXECUTE ANY EXTERNAL SCRIPT |DATABASE |
|EX|EXECUTE|ASSEMBLY、DATABASE、OBJECT、SCHEMA、TYPE、XML SCHEMA COLLECTION|  
|IM|IMPERSONATE|USER|  
|IN|INSERT|DATABASE、OBJECT、SCHEMA|  
|RC|RECEIVE|OBJECT|  
|RF|REFERENCES|ASSEMBLY、ASYMMETRIC KEY、CERTIFICATE、CONTRACT、DATABASE、FULLTEXT CATALOG、MESSAGE TYPE、OBJECT、SCHEMA、SYMMETRIC KEY、TYPE、XML SCHEMA COLLECTION|  
|SL|SELECT|DATABASE、OBJECT、SCHEMA|  
|SN|SEND|SERVICE|  
|SPLN|SHOWPLAN|DATABASE|  
|SUQN|SUBSCRIBE QUERY NOTIFICATIONS|DATABASE|  
|TO|TAKE OWNERSHIP|ASSEMBLY、ASYMMETRIC KEY、CERTIFICATE、CONTRACT, DATABASE、FULLTEXT CATALOG、MESSAGE TYPE、OBJECT、REMOTE SERVICE BINDING、ROLE、ROUTE、SCHEMA、SERVICE、SYMMETRIC KEY、TYPE、XML SCHEMA COLLECTION|  
|UP|UPDATE|DATABASE、OBJECT、SCHEMA|  
|VW|VIEW DEFINITION|APPLICATION ROLE、ASSEMBLY、ASYMMETRIC KEY、CERTIFICATE、CONTRACT、DATABASE、FULLTEXT CATALOG、MESSAGE TYPE、OBJECT、REMOTE SERVICE BINDING、ROLE、ROUTE、SCHEMA、SERVICE、SYMMETRIC KEY、TYPE、USER、XML SCHEMA COLLECTION|  
|VWCK |VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|DATABASE |  
|VWCM |VIEW ANY COLUMN MASTER KEY DEFINITION|DATABASE |  
|VWCT|VIEW CHANGE TRACKING|TABLE、SCHEMA|  
|VWDS|VIEW DATABASE STATE|DATABASE|  
  
## <a name="permissions"></a>权限  
 任何用户都可以查看自己的权限。 要查看其他用户的权限，需要获取 VIEW DEFINITION、ALTER ANY USER 或任何相关的用户权限。 要查看用户定义的角色，需要获取 ALTER ANY ROLE 或相关的角色（如公共）成员身份。  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
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
    
  
## <a name="see-also"></a>另请参阅  
 [安全对象](../../relational-databases/security/securables.md)   
 [权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  


