---
description: IS_ROLEMEMBER (Transact-SQL)
title: IS_ROLEMEMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IS_ROLEMEMBER
- IS_ROLEMEMBER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_ROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 73efa688-ae91-4014-98bc-1cabe47321f7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 856265d1ba66eb2cfae29b12ec23b432cb4c8e3f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91116720"
---
# <a name="is_rolemember-transact-sql"></a>IS_ROLEMEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  指示指定的数据库主体是否为指定数据库角色的成员。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
IS_ROLEMEMBER ( 'role' [ , 'database_principal' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'** *role* **'**  
 要检查的数据库角色的名称。 role 为 sysname   。  
  
 **'** *database_principal* **'**  
 要检查的数据库用户、数据库角色或应用程序角色的名称。 database_principal 的数据类型为 sysname，默认值为 NULL。 如果未指定值，则结果视当前执行上下文而定。 如果此参数包含词 NULL，将返回 NULL。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
|返回值|描述|  
|------------------|-----------------|  
|0|database_principal 不是 role 的成员。|  
|1|database_principal 是 role 的成员。|  
|Null|database_principal 或 role 无效，或者你没有查看角色成员身份的权限。|  
  
## <a name="remarks"></a>注解  
 请使用 IS_ROLEMEMBER 确定当前用户是否可以执行需要数据库角色权限的操作。  
  
 如果 database_principal 基于 Windows 登录名，如 Contoso\Mary5，则 IS_ROLEMEMBER 返回 NULL，除非为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 授予或拒绝了 database_principal 的直接访问权限。  
  
 如果未提供可选的 database_principal 参数，并且 database_principal 基于 Windows 域登录名，则它可能是通过 Windows 组成员身份而成为的数据库角色成员。 要解析这种间接成员身份，IS_ROLEMEMBER 将从域控制器中请求 Windows 组成员身份信息。 如果无法访问域控制器或域控制器没有响应，则 IS_ROLEMEMBER 在返回角色成员身份信息时只考虑用户及其本地组。 如果指定的用户不是当前用户，则 IS_ROLEMEMBER 返回的值可能不同于验证器（如 Active Directory）上次进行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据更新。  
  
 如果提供了可选的 database_principal 参数，则要查询的数据库主体必须在 sys.database_principals 中存在，否则，IS_ROLEMEMBER 将返回 NULL。 这表明 database_principal 在此数据库中无效。  
  
 如果 database_principal 参数基于域登录名或 Windows 组，并且无法访问域控制器，则对 IS_ROLEMEMBER 的调用将失败，并且可能会返回不正确或不完整的数据。  
  
 如果域控制器不可用，并且可以在本地对 Windows 主体进行身份验证时（例如本地 Windows 帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名），对 IS_ROLEMEMBER 的调用将返回准确的信息。  
  
 在某一 Windows 组用作数据库主体参数，并且此 Windows 组是另一个 Windows 组的成员，而该组又是指定数据库角色的成员时，IS_ROLEMEMBER 始终返回 0。  
  
 [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] 和 Windows Server 2008 中的用户帐户控制 (UAC) 可能也会返回不同的结果。 这取决于用户在访问服务器时是作为 Windows 组成员还是特定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户。  
  
 此函数计算角色成员身份，而不是基础权限。 例如，db_owner 固定数据库角色具有 CONTROL DATABASE 权限。 如果用户具有 CONTROL DATABASE 权限，但不是该角色的成员，此函数将正确报告用户不是 db_owner 角色的成员，即使用户具有相同的权限也是如此。  
  
## <a name="related-functions"></a>相关函数  
 若要确定当前用户是否为指定 Windows 组或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库角色的成员，请使用 [IS_MEMBER (Transact-SQL)](../../t-sql/functions/is-member-transact-sql.md)。 若要确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名是否为服务器角色的成员，请使用 [IS_SRVROLEMEMBER (Transact-SQL)](../../t-sql/functions/is-srvrolemember-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 要求具有数据库角色的 VIEW DEFINITION 权限。  
  
## <a name="examples"></a>示例  
 以下示例指示当前用户是否为 `db_datareader` 固定数据库角色的成员。  
  
```sql  
IF IS_ROLEMEMBER ('db_datareader') = 1  
   print 'Current user is a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') = 0  
   print 'Current user is NOT a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') IS NULL  
   print 'ERROR: The database role specified is not valid.';  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
 [ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE (Transact-SQL)](../../t-sql/statements/drop-role-transact-sql.md)   
 [CREATE SERVER ROLE (Transact-SQL)](../../t-sql/statements/create-server-role-transact-sql.md)   
 [ALTER SERVER ROLE (Transact-SQL)](../../t-sql/statements/alter-server-role-transact-sql.md)   
 [DROP SERVER ROLE (Transact-SQL)](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [IS_MEMBER (Transact-SQL)](../../t-sql/functions/is-member-transact-sql.md)   
 [IS_SRVROLEMEMBER (Transact-SQL)](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
