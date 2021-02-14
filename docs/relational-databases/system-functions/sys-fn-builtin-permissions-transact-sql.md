---
description: sys.fn_builtin_permissions (Transact-SQL)
title: sys.fn_builtin_permissions (Transact-SQL)
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_builtin_permissions
- sys.fn_builtin_permissions_TSQL
- fn_builtin_permissions_TSQL
- sys.fn_builtin_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- compact permissions types
- viewing permission hierarchy
- hierarchies [SQL Server], permissions
- built-in permissions hierarchy [SQL Server]
- fn_builtin_permissions function
- permissions [SQL Server], hierarchy
- displaying permission hierarchy
- sys.fn_builtin_permissions function
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9b037f61c1cbbefbcbd441ef978e233b9ca91aac
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100338139"
---
# <a name="sysfn_builtin_permissions-transact-sql"></a>sys.fn_builtin_permissions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回服务器内置权限层次结构的说明。 `sys.fn_builtin_permissions` 只能在和上调用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ，它将返回所有权限，不管当前平台上是否支持它们。 大多数权限适用于所有平台，但有些则不适用。 例如，不能对 SQL 数据库授予服务器级别权限。 有关哪些平台支持每个权限的信息，请参阅 [&#40;数据库引擎&#41;的权限 ](../../relational-databases/security/permissions-database-engine.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_builtin_permissions ( [ DEFAULT | NULL ]  
    | empty_string | '<securable_class>' } )  
  
<securable_class> ::=   
      APPLICATION ROLE | ASSEMBLY | ASYMMETRIC KEY | AVAILABILITY GROUP  
    | CERTIFICATE | CONTRACT | DATABASE | DATABASE SCOPED CREDENTIAL    
    | ENDPOINT | FULLTEXT CATALOG | FULLTEXT STOPLIST | LOGIN      
    | MESSAGE TYPE | OBJECT | REMOTE SERVICE BINDING | ROLE | ROUTE    
    | SCHEMA | SEARCH PROPERTY LIST | SERVER | SERVER ROLE | SERVICE    
    | SYMMETRIC KEY | TYPE | USER | XML SCHEMA COLLECTION  
```  
  
## <a name="arguments"></a>参数  
 DEFAULT  
 如果使用默认选项调用 (不使用引号) ，则函数将返回内置权限的完整列表。  
  
 Null  
 等效于 DEFAULT。  
  
 *empty_string*  
 等效于 DEFAULT。  
  
 **"**<securable_class>**"**  
 当使用一个安全对象类的名称调用时，sys.fn_builtin_permissions 将返回应用于该类的所有权限。 <securable_class> 是需要用引号引起来的字符串文字。 **nvarchar(60)**  
  
## <a name="tables-returned"></a>返回的表  
  
|列名称|数据类型|排序规则|描述|  
|-----------------|---------------|---------------|-----------------|  
|class_desc|**nvarchar(60)**|服务器的排序规则|安全对象类的说明。|  
|permission_name|**nvarchar(60)**|服务器的排序规则|权限名称。|  
|类型|**varchar(4)**|服务器的排序规则|压缩权限类型代码。 请参阅后面的表。|  
|covering_permission_name|**nvarchar(60)**|服务器的排序规则|如果不为 NULL，则为该类的权限名称（隐含该类的其他权限）。|  
|parent_class_desc|**nvarchar(60)**|服务器的排序规则|如果不为 NULL，则为包含当前类的父类的名称。|  
|parent_covering_permission_name|**nvarchar(60)**|服务器的排序规则|如果不为 NULL，则为父类的权限名称（隐含该类的所有其他权限）。|  
  
### <a name="permission-types"></a>权限类型  
  
|权限类型|权限名称|应用于安全对象或类|  
|---------------------|---------------------|-----------------------------------|  
|AADS|ALTER ANY DATABASE EVENT SESSION<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|DATABASE|  
|AAES|ALTER ANY EVENT SESSION|SERVER|  
|AAMK|ALTER ANY MASK<br /> [!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|ADBO|ADMINISTER BULK OPERATIONS|SERVER|  
|AEDS|ALTER ANY EXTERNAL DATA SOURCE<br /> [!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|AEFF|ALTER ANY EXTERNAL FILE FORMAT<br /> [!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|AL|ALTER|APPLICATION ROLE|  
|AL|ALTER|ASSEMBLY|  
|AL|ALTER<br />**适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|AVAILABILITY GROUP|  
|AL|ALTER|ASYMMETRIC KEY|  
|AL|ALTER|CERTIFICATE|  
|AL|ALTER|CONTRACT|  
|AL|ALTER|DATABASE|  
|AL|ALTER<br /> 适用范围：[!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。 |DATABASE SCOPED CREDENTIAL|
|AL|ALTER|ENDPOINT|  
|AL|ALTER|FULLTEXT CATALOG|  
|AL|ALTER|FULLTEXT STOPLIST|  
|AL|ALTER|LOGIN|  
|AL|ALTER|MESSAGE TYPE|  
|AL|ALTER|OBJECT|  
|AL|ALTER|REMOTE SERVICE BINDING|  
|AL|ALTER|ROLE|  
|AL|ALTER|ROUTE|  
|AL|ALTER|SCHEMA|  
|AL|ALTER|SEARCH PROPERTY LIST|  
|AL|ALTER<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER ROLE|  
|AL|ALTER|SERVICE|  
|AL|ALTER|SYMMETRIC KEY|  
|AL|ALTER|USER|  
|AL|ALTER|XML SCHEMA COLLECTION|  
|ALAA|ALTER ANY SERVER AUDIT|SERVER|  
|ALAG|ALTER ANY AVAILABILITY GROUP<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER|  
|ALAK|ALTER ANY ASYMMETRIC KEY|DATABASE|  
|ALAR|ALTER ANY APPLICATION ROLE|DATABASE|  
|ALAS|ALTER ANY ASSEMBLY|DATABASE|  
|ALCD|ALTER ANY CREDENTIAL|SERVER|  
|ALCF|ALTER ANY CERTIFICATE|DATABASE|  
|ALCK|ALTER ANY COLUMN ENCRYPTION KEY<br />[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|ALCM|ALTER ANY COLUMN MASTER KEY<br />[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|ALCO|ALTER ANY CONNECTION|SERVER|  
|ALDA|ALTER ANY DATABASE AUDIT|DATABASE|  
|ALDB|ALTER ANY DATABASE|SERVER|  
|ALDC|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> [!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|ALDS|ALTER ANY DATASPACE|DATABASE|  
|ALED|ALTER ANY DATABASE EVENT NOTIFICATION|DATABASE|  
|ALES|ALTER ANY EVENT NOTIFICATION|SERVER|  
|ALFT|ALTER ANY FULLTEXT CATALOG|DATABASE|  
|ALHE|ALTER ANY ENDPOINT|SERVER|  
|ALLG|ALTER ANY LOGIN|SERVER|  
|ALLS|ALTER ANY LINKED SERVER|SERVER|  
|ALMT|ALTER ANY MESSAGE TYPE|DATABASE|  
|ALRL|ALTER ANY ROLE|DATABASE|  
|ALRS|ALTER RESOURCES|SERVER|  
|ALRT|ALTER ANY ROUTE|DATABASE|  
|ALSB|ALTER ANY REMOTE SERVICE BINDING|DATABASE|  
|ALSC|ALTER ANY CONTRACT|DATABASE|  
|ALSK|ALTER ANY SYMMETRIC KEY|DATABASE|  
|ALSM|ALTER ANY SCHEMA|DATABASE|  
|ALSP|更改任何安全策略<br />[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|ALSR|ALTER ANY SERVER ROLE<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER|  
|ALSS|ALTER SERVER STATE|SERVER|  
|ALST|ALTER SETTINGS|SERVER|  
|ALSV|ALTER ANY SERVICE|DATABASE|  
|ALTG|ALTER ANY DATABASE DDL TRIGGER|DATABASE|  
|ALTR|ALTER TRACE|SERVER|  
|ALUS|ALTER ANY USER|DATABASE|  
|AUTH|AUTHENTICATE|DATABASE|  
|AUTH|AUTHENTICATE SERVER|SERVER|  
|BADB|BACKUP DATABASE|DATABASE|  
|BALO|BACKUP LOG|DATABASE|  
|CADB|CONNECT ANY DATABASE<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER|  
|CL|CONTROL|APPLICATION ROLE|  
|CL|CONTROL|ASSEMBLY|  
|CL|CONTROL|ASYMMETRIC KEY|  
|CL|CONTROL<br />**适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|AVAILABILITY GROUP|  
|CL|CONTROL|CERTIFICATE|  
|CL|CONTROL|CONTRACT|  
|CL|CONTROL|DATABASE|  
|CL|CONTROL<br /> 适用范围：[!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。 |DATABASE SCOPED CREDENTIAL|
|CL|CONTROL|ENDPOINT|  
|CL|CONTROL|FULLTEXT CATALOG|  
|CL|CONTROL|FULLTEXT STOPLIST|  
|CL|CONTROL|LOGIN|  
|CL|CONTROL|MESSAGE TYPE|  
|CL|CONTROL|OBJECT|  
|CL|CONTROL|REMOTE SERVICE BINDING|  
|CL|CONTROL|ROLE|  
|CL|CONTROL|ROUTE|  
|CL|CONTROL|SCHEMA|  
|CL|CONTROL|SEARCH PROPERTY LIST|  
|CL|CONTROL SERVER|SERVER|  
|CL|CONTROL<br />**适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER ROLE|  
|CL|CONTROL|SERVICE|  
|CL|CONTROL|SYMMETRIC KEY|  
|CL|CONTROL|TYPE|  
|CL|CONTROL|USER|  
|CL|CONTROL|XML SCHEMA COLLECTION|  
|CO|CONNECT|DATABASE|  
|CO|CONNECT|ENDPOINT|  
|CORP|CONNECT REPLICATION|DATABASE|  
|COSQ|CONNECT SQL|SERVER|  
|CP|CHECKPOINT|DATABASE|  
|CRAC|CREATE AVAILABILITY GROUP<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER|  
|CRAG|CREATE AGGREGATE|DATABASE|  
|CRAK|CREATE ASYMMETRIC KEY|DATABASE|  
|CRAS|CREATE ASSEMBLY|DATABASE|  
|CRCF|CREATE CERTIFICATE|DATABASE|  
|CRDB|CREATE ANY DATABASE|SERVER|  
|CRDB|CREATE DATABASE|DATABASE|  
|CRDE|CREATE DDL EVENT NOTIFICATION|SERVER|  
|CRDF|CREATE DEFAULT|DATABASE|  
|CRED|CREATE DATABASE DDL EVENT NOTIFICATION|DATABASE|  
|CRFN|CREATE FUNCTION|DATABASE|  
|CRFT|CREATE FULLTEXT CATALOG|DATABASE|  
|CRHE|CREATE ENDPOINT|SERVER|  
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
|CRSO|CREATE SEQUENCE|SCHEMA|  
|CRSR|CREATE SERVER ROLE<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER|  
|CRSV|CREATE SERVICE|DATABASE|  
|CRTB|CREATE TABLE|DATABASE|  
|CRTE|CREATE TRACE EVENT NOTIFICATION|SERVER|  
|CRTY|CREATE TYPE|DATABASE|  
|CRVW|CREATE VIEW|DATABASE|  
|CRXS|CREATE XML SCHEMA COLLECTION|DATABASE|  
|DABO|ADMINISTER DATABASE BULK OPERATIONS<br /> **适用于**： [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。|DATABASE|  
|DL|DELETE|DATABASE|  
|DL|DELETE|OBJECT|  
|DL|DELETE|SCHEMA|  
|EAES|EXECUTE ANY EXTERNAL SCRIPT<br />[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|EX|EXECUTE|DATABASE|  
|EX|EXECUTE|OBJECT|  
|EX|EXECUTE|SCHEMA|  
|EX|EXECUTE|TYPE|  
|EX|EXECUTE|XML SCHEMA COLLECTION|  
|IAL|IMPERSONATE ANY LOGIN<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER|  
|IM|IMPERSONATE|LOGIN|  
|IM|IMPERSONATE|USER|  
|IN|INSERT|DATABASE|  
|IN|INSERT|OBJECT|  
|IN|INSERT|SCHEMA|  
|KIDC|KILL DATABASE CONNECTION<br />**适用于**： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|DATABASE|  
|RC|RECEIVE|OBJECT|  
|RF|REFERENCES|ASSEMBLY|  
|RF|REFERENCES|ASYMMETRIC KEY|  
|RF|REFERENCES|CERTIFICATE|  
|RF|REFERENCES|CONTRACT|  
|RF|REFERENCES|DATABASE|  
|RF|REFERENCES<br /> 适用范围：[!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。 |DATABASE SCOPED CREDENTIAL|
|RF|REFERENCES|FULLTEXT CATALOG|  
|RF|REFERENCES|FULLTEXT STOPLIST|  
|RF|REFERENCES|SEARCH PROPERTY LIST|  
|RF|REFERENCES|MESSAGE TYPE|  
|RF|REFERENCES|OBJECT|  
|RF|REFERENCES|SCHEMA|  
|RF|REFERENCES|SYMMETRIC KEY|  
|RF|REFERENCES|TYPE|  
|RF|REFERENCES|XML SCHEMA COLLECTION|  
|SHDN|SHUTDOWN|SERVER|  
|SL|SELECT|DATABASE|  
|SL|SELECT|OBJECT|  
|SL|SELECT|SCHEMA|  
|SN|SEND|SERVICE|  
|SPLN|SHOWPLAN|DATABASE|  
|SUQN|SUBSCRIBE QUERY NOTIFICATIONS|DATABASE|  
|SUS|SELECT ALL USER SECURABLES<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER|  
|TO|TAKE OWNERSHIP|ASSEMBLY|  
|TO|TAKE OWNERSHIP|ASYMMETRIC KEY|  
|TO|TAKE OWNERSHIP<br />**适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|AVAILABILITY GROUP|  
|TO|TAKE OWNERSHIP|CERTIFICATE|  
|TO|TAKE OWNERSHIP|CONTRACT|  
|TO|TAKE OWNERSHIP|DATABASE|  
|TO|TAKE OWNERSHIP<br /> 适用范围：[!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。 |DATABASE SCOPED CREDENTIAL|
|TO|TAKE OWNERSHIP|ENDPOINT|  
|TO|TAKE OWNERSHIP|FULLTEXT CATALOG|  
|TO|TAKE OWNERSHIP|FULLTEXT STOPLIST|  
|TO|TAKE OWNERSHIP|SEARCH PROPERTY LIST|  
|TO|TAKE OWNERSHIP|MESSAGE TYPE|  
|TO|TAKE OWNERSHIP|OBJECT|  
|TO|TAKE OWNERSHIP|REMOTE SERVICE BINDING|  
|TO|TAKE OWNERSHIP|ROLE|  
|TO|TAKE OWNERSHIP|ROUTE|  
|TO|TAKE OWNERSHIP|SCHEMA|  
|TO|TAKE OWNERSHIP<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER ROLE|  
|TO|TAKE OWNERSHIP|SERVICE|  
|TO|TAKE OWNERSHIP|SYMMETRIC KEY|  
|TO|TAKE OWNERSHIP|TYPE|  
|TO|TAKE OWNERSHIP|XML SCHEMA COLLECTION|  
|UMSK|UNMASK<br /> [!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|UP|UPDATE|DATABASE|  
|UP|UPDATE|OBJECT|  
|UP|UPDATE|SCHEMA|  
|VW|VIEW DEFINITION|APPLICATION ROLE|  
|VW|VIEW DEFINITION|ASSEMBLY|  
|VW|VIEW DEFINITION|ASYMMETRIC KEY|  
|VW|VIEW DEFINITION<br />**适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|AVAILABILITY GROUP|  
|VW|VIEW DEFINITION|CERTIFICATE|  
|VW|VIEW DEFINITION|CONTRACT|  
|VW|VIEW DEFINITION|DATABASE|  
|VW|VIEW DEFINITION<br /> 适用范围：[!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。 |DATABASE SCOPED CREDENTIAL|
|VW|VIEW DEFINITION|ENDPOINT|  
|VW|VIEW DEFINITION|FULLTEXT CATALOG|  
|VW|VIEW DEFINITION|FULLTEXT STOPLIST|  
|VW|VIEW DEFINITION|LOGIN|  
|VW|VIEW DEFINITION|MESSAGE TYPE|  
|VW|VIEW DEFINITION|OBJECT|  
|VW|VIEW DEFINITION|REMOTE SERVICE BINDING|  
|VW|VIEW DEFINITION|ROLE|  
|VW|VIEW DEFINITION|ROUTE|  
|VW|VIEW DEFINITION|SCHEMA|  
|VW|VIEW DEFINITION|SEARCH PROPERTY LIST|  
|VW|VIEW DEFINITION<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|SERVER ROLE|  
|VW|VIEW DEFINITION|SERVICE|  
|VW|VIEW DEFINITION|SYMMETRIC KEY|  
|VW|VIEW DEFINITION|TYPE|  
|VW|VIEW DEFINITION|USER|  
|VW|VIEW DEFINITION|XML SCHEMA COLLECTION|  
|VWAD|VIEW ANY DEFINITION|SERVER|  
|VWCK|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION<br /> [!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|VWCM|VIEW ANY COLUMN MASTER KEY DEFINITION<br /> [!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)].|DATABASE|  
|VWCT|VIEW CHANGE TRACKING|OBJECT|  
|VWCT|VIEW CHANGE TRACKING|SCHEMA|  
|VWDB|VIEW ANY DATABASE|SERVER|  
|VWDS|VIEW DATABASE STATE|DATABASE|  
|VWSS|VIEW SERVER STATE|SERVER|  
|XA|EXTERNAL ACCESS ASSEMBLY|SERVER|  
|XU|UNSAFE ASSEMBLY|SERVER|  
  
## <a name="remarks"></a>备注  
 `sys.fn_builtin_permissions` 是一个表值函数，它可以产生预定义权限层次结构的副本。 此层次结构包含涵盖权限。 `DEFAULT`结果集描述了权限层次结构的有向非循环图形，其中 root (类 = SERVER，权限 = CONTROL SERVER) 。  
  
 `sys.fn_builtin_permissions` 不接受相关参数。  
  
 使用无效类名调用 `sys.fn_builtin_permissions` 时，它返回一个空集。  
 
[!INCLUDE[database-engine-permissions](../../includes/paragraph-content/database-engine-permissions.md)]
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-all-built-in-permissions"></a>A. 列出所有内置权限   
使用 `DEFAULT` 或空字符串返回所有权限。   
```sql  
SELECT * FROM sys.fn_builtin_permissions(DEFAULT);
SELECT * FROM sys.fn_builtin_permissions('');  
```  
  
### <a name="b-listing-permissions-that-can-be-set-on-a-symmetric-key"></a>B. 列出可针对对称密钥设置的权限   
指定类以返回该类的所有可能的权限。   
```sql  
SELECT * FROM sys.fn_builtin_permissions(N'SYMMETRIC KEY');  
```  
  
### <a name="c-listing-classes-on-which-there-is-a-select-permission"></a>C. 列出对其拥有 SELECT 权限的类   
  
```sql  
SELECT * FROM sys.fn_builtin_permissions(DEFAULT)   
    WHERE permission_name = 'SELECT';  
```  
  
## <a name="see-also"></a>另请参阅  
 [权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [CREATE SCHEMA (Transact-SQL)](../../t-sql/statements/create-schema-transact-sql.md)   
 [DROP SCHEMA (Transact-SQL)](../../t-sql/statements/drop-schema-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
