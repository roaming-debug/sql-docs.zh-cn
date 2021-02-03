---
description: GRANT 对象权限 (Transact-SQL)
title: GRANT 对象权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], objects
- GRANT statement, objects
ms.assetid: c001c2e7-d092-43d4-8fa6-693b3ec4c3ea
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 681cb64aa6ef4874d331d61e2d8c07df2ed06ac1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161239"
---
# <a name="grant-object-permissions-transact-sql"></a>GRANT 对象权限 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  授予对表、视图、表值函数、存储过程、扩展存储过程、标量函数、聚合函数、服务队列或同义词的权限。  
  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GRANT <permission> [ ,...n ] ON   
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]  
    TO <database_principal> [ ,...n ]   
    [ WITH GRANT OPTION ]  
    [ AS <database_principal> ]  
  
<permission> ::=  
    ALL [ PRIVILEGES ] | permission [ ( column [ ,...n ] ) ]  
  
<database_principal> ::=   
        Database_user   
    | Database_role   
    | Application_role   
    | Database_user_mapped_to_Windows_User   
    | Database_user_mapped_to_Windows_Group   
    | Database_user_mapped_to_certificate   
    | Database_user_mapped_to_asymmetric_key   
    | Database_user_with_no_login  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可以授予的对架构包含的对象的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ALL  
 授予 ALL 不会授予所有可能的权限。 授予 ALL 等同于授予适用于指定对象的所有 [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92 权限。 对于不同权限，ALL 的含义有所不同：  
  
- 标量值函数权限：EXECUTE、REFERENCES。  
- 表值函数权限：DELETE、INSERT、REFERENCES、SELECT、UPDATE。  
- 存储过程权限：EXECUTE。  
- 表权限：DELETE、INSERT、REFERENCES、SELECT、UPDATE。  
- 视图权限：DELETE、INSERT、REFERENCES、SELECT、UPDATE。  
  
PRIVILEGES  
 包含此参数以符合 [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92 标准。 请不要更改 ALL 的行为。  
  
*column*  
 指定表、视图或表值函数中要授予对其权限的列的名称。 需要使用括号 ( )。 只能授予对列的 SELECT、REFERENCES 及 UPDATE 权限。 可以在权限子句中或在安全对象名之后指定 column。  
  
> [!CAUTION]  
>  表级 DENY 并不优先于列级 GRANT。 保留了权限层次结构中的这种不一致性以保持向后兼容。  
  
 ON [ OBJECT :: ] [ schema_name ] . *object_name*  
 指定要授予对其权限的对象。 如果指定了 schema_name，则 OBJECT 短语是可选的。 如果使用了 OBJECT 短语，则需要作用域限定符 (::)。 如果未指定 schema_name，则使用默认架构。 如果指定了 schema_name，则需要使用架构作用域限定符 (.)。  
  
 TO \<database_principal>  
 指定要向其授予权限的主体。  
  
 WITH GRANT OPTION  
 指示该主体还可以向其他主体授予所指定的权限。  
  
 AS \<database_principal>：指定一个主体，执行该查询的主体从中获得授予权限的权利。  
  
 Database_user  
 指定数据库用户。  
  
 Database_role  
 指定数据库角色。  
  
 Application_role  
 指定应用程序角色。  
  
 Database_user_mapped_to_Windows_User  
 指定映射到 Windows 用户的数据库用户。  
  
 Database_user_mapped_to_Windows_Group  
 指定映射到 Windows 组的数据库用户。  
  
 Database_user_mapped_to_certificate  
 指定映射到证书的数据库用户。  
  
 Database_user_mapped_to_asymmetric_key  
 指定映射到非对称密钥的数据库用户。  
  
 Database_user_with_no_login  
 指定无相应服务器级主体的数据库用户。  
  
## <a name="remarks"></a>备注  
  
> [!IMPORTANT]  
>  在某些情况下，如果同时拥有 ALTER 权限和 REFERENCE 权限，被授权者将可以查看数据或执行未经授权的函数。 例如：对表拥有 ALTER 权限和对函数拥有 REFERENCE 权限的用户可对函数创建计算列并执行该函数。 在此情况下，用户可能需要对计算列具有 SELECT 权限。  
  
 可以在各种目录视图中查看对象的有关信息。 有关详细信息，请参阅[对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)。  
  
 对象是一个架构级的安全对象，包含于权限层次结构中作为其父级的架构中。 下表列出了可授予的对对象最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|对象权限|对象权限隐含的权限|架构权限隐含的权限|  
|-----------------------|----------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|DELETE|CONTROL|DELETE|  
|EXECUTE|CONTROL|EXECUTE|  
|INSERT|CONTROL|INSERT|  
|RECEIVE|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|RECEIVE|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|VIEW CHANGE TRACKING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 授权者（或用 AS 选项指定的主体）必须具有带 GRANT OPTION 的相同权限，或具有隐含所授予权限的更高权限。  
  
 若要使用 AS 选项，还必须满足以下附加要求：  
  
|AS|所需的其他权限|  
|--------|------------------------------------|  
|数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|映射到 Windows 登录名的数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|映射到 Windows 组的数据库用户|Windows 组的成员身份、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|映射到证书的数据库用户|db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|映射到非对称密钥的数据库用户|db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|未映射到任何服务器主体的数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|数据库角色|对角色的 ALTER 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|应用程序角色|对角色的 ALTER 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
  
## <a name="examples"></a>示例  
  
### <a name="a-granting-select-permission-on-a-table"></a>A. 授予表的 SELECT 权限  
 以下示例授予用户 `SELECT` 对 `RosaQdM` 数据库中表 `Person.Address` 的 `AdventureWorks2012` 权限。  
  
```sql  
GRANT SELECT ON OBJECT::Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="b-granting-execute-permission-on-a-stored-procedure"></a>B. 授予对存储过程的 EXECUTE 权限  
 以下示例授予名为 `EXECUTE` 的应用程序角色对存储过程 `HumanResources.uspUpdateEmployeeHireInfo` 的 `Recruiting11` 权限。  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO   
```  
  
### <a name="c-granting-references-permission-on-a-view-with-grant-option"></a>C. 使用 GRANT OPTION 授予对视图的 REFERENCES 权限  
 以下示例使用 `REFERENCES`，授予用户 `BusinessEntityID` 对视图 `HumanResources.vEmployee` 中列 `Wanida` 的 `GRANT OPTION` 权限。  
  
```sql  
GRANT REFERENCES (BusinessEntityID) ON OBJECT::HumanResources.vEmployee   
    TO Wanida WITH GRANT OPTION;  
GO  
```  
  
### <a name="d-granting-select-permission-on-a-table-without-using-the-object-phrase"></a>D. 不使用 OBJECT 短语对表授予 SELECT 权限  
 以下示例授予用户 `SELECT` 对 `RosaQdM` 数据库中表 `Person.Address` 的 `AdventureWorks2012` 权限。  
  
```sql  
GRANT SELECT ON Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="e-granting-select-permission-on-a-table-to-a-domain-account"></a>E. 向域帐户授予对表的 SELECT 权限  
 以下示例授予用户 `SELECT` 对 `AdventureWorks2012\RosaQdM` 数据库中表 `Person.Address` 的 `AdventureWorks2012` 权限。  
  
```sql  
GRANT SELECT ON Person.Address TO [AdventureWorks2012\RosaQdM];  
GO  
```  
  
### <a name="f-granting-execute-permission-on-a-procedure-to-a-role"></a>F. 将针对过程的 EXECUTE 权限授予角色  
 以下示例创建一个角色，然后针对 `EXECUTE` 数据库中的过程 `uspGetBillOfMaterials`，将 `AdventureWorks2012` 权限授予该角色。  
  
```sql  
CREATE ROLE newrole ;  
GRANT EXECUTE ON dbo.uspGetBillOfMaterials TO newrole ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DENY 对象权限 (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [安全对象](../../relational-databases/security/securables.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)  
  
  

