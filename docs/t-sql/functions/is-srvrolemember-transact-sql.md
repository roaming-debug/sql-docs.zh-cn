---
description: IS_SRVROLEMEMBER (Transact-SQL)
title: IS_SRVROLEMEMBER (Transact-SQL)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_SRVROLEMEMBER_TSQL
- IS_SRVROLEMEMBER
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_SRVROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 3241a44a-6958-415b-b8b7-2a1207c36ab3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4a3832b4f9e250c2ff3368d0a94dbf77843f16a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350396"
---
# <a name="is_srvrolemember-transact-sql"></a>IS_SRVROLEMEMBER (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名是否为指定服务器角色的成员。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
IS_SRVROLEMEMBER ( 'role' [ , 'login' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'** *role* **'**  
 要检查的服务器角色的名称。 role 为 sysname   。  
  
 role 的有效值是用户定义的服务器角色和以下固定服务器角色  ：  

- sysadmin
- serveradmin
- dbcreator
- setupadmin  
- bulkadmin
- securityadmin  
- diskadmin
- 公共  
- processadmin
  
 **'** *login* **'**  
 要检查的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录的名称。 login 的数据类型为 sysname，默认值为 NULL。 如果未指定值，则结果视当前执行上下文而定。 如果此参数包含词 NULL，将返回 NULL。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
|返回值|说明|  
|------------------|-----------------|  
|0|login 不是 role 的成员 。<br /><br /> 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中，此语句始终返回 0。|  
|1|login 是 role 的成员 。|  
|Null|role 或 login 无效，或者没有查看角色成员身份的权限 。|  
  
## <a name="remarks"></a>备注  
 使用 IS_SRVROLEMEMBER 可确定当前用户是否可以执行需要服务器角色权限的操作。  
  
 如果为 login 指定了 Windows 登录名（例如 Contoso\Mary5），那么除非针对该登录名授予或拒绝了对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的直接访问权限，否则 IS_SRVROLEMEMBER 将返回 NULL 。  
  
 如果未提供可选的 login 参数，且 login 是 Windows 域登录名，则它可以通过 Windows 组中的成员身份作为固定服务器角色的成员 。 要解析这种间接成员身份，IS_SRVROLEMEMBER 将从域控制器中请求 Windows 组成员身份信息。 如果无法访问域控制器或域控制器没有响应，则 IS_SRVROLEMEMBER 在返回角色成员身份信息时只考虑用户及其本地组。 如果指定的用户不是当前用户，则 IS_SRVROLEMEMBER 返回的值可能不同于验证器（例如 Active Directory）对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进行的最后一次数据更新。  
  
 如果提供了可选的 login 参数，则 sys.server_principals 中必须存在要查询的 Windows 登录名，否则 IS_SRVROLEMEMBER 将返回 NULL。 这指示该登录名无效。  
  
 如果登录名参数为域登录名或基于 Windows 组，且无法访问域控制器，则对 IS_SRVROLEMEMBER 的调用将失败，并可能返回不正确或不完整的数据。  
  
 如果域控制器不可用，则当 Windows 主体可在本地进行身份验证时（例如本地 Windows 帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名），对 IS_SRVROLEMEMBER 的调用将返回准确的信息。  
  
 在某一 Windows 组用作登录参数，并且此 Windows 组是另一个 Windows 组的成员，而该组又是指定服务器角色的成员时，IS_SRVROLEMEMBER 始终返回 0。  
  
 用户帐户控制 (UAC) 设置也可能导致返回不同的结果。 这取决于用户在访问服务器时是作为 Windows 组成员还是特定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户。  
  
 此函数计算角色成员身份，而不是基础权限。 例如，sysadmin 固定服务器角色具有 CONTROL SERVER 权限 。 如果用户具有 CONTROL SERVER 权限但不是该角色的成员，此函数将正确报告用户不是 sysadmin 角色的成员，即使用户具有相同的权限也是如此 。  
  
## <a name="related-functions"></a>相关函数  
 若要确定当前用户是否为指定 Windows 组或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库角色的成员，请使用 [IS_MEMBER (Transact-SQL)](../../t-sql/functions/is-member-transact-sql.md)。 若要确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名是否为数据库角色的成员，请使用 [IS_ROLEMEMBER (Transact-SQL)](../../t-sql/functions/is-rolemember-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 要求具有服务器角色的 VIEW DEFINITION 权限。  
  
## <a name="examples"></a>示例  
 以下示例指示当前用户的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名是 `sysadmin` 固定服务器角色的成员。  
  
```sql  
IF IS_SRVROLEMEMBER ('sysadmin') = 1  
   print 'Current user''s login is a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') = 0  
   print 'Current user''s login is NOT a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') IS NULL  
   print 'ERROR: The server role specified is not valid.';  
```  
  
 以下示例指示域登录名 Pat 是否是 diskadmin 固定服务器角色的成员。  
  
```sql  
SELECT IS_SRVROLEMEMBER('diskadmin', 'Contoso\Pat');  
```  
  
## <a name="see-also"></a>另请参阅  
 [IS_MEMBER (Transact-SQL)](../../t-sql/functions/is-member-transact-sql.md)   
 [安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
