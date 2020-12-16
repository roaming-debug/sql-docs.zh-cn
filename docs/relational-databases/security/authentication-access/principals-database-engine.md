---
title: 主体（数据库引擎）| Microsoft Docs
description: 了解数据库引擎中的主体，它们是可以请求 SQL Server 资源的实体。 这些主体分为 SQL Server 级别和数据库级别。
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.selectroll.f1
- sql13.swb.databaseuser.permissions.user.f1--May use common.permissions
helpviewer_keywords:
- certificates [SQL Server], principals
- roles [SQL Server], principals
- permissions [SQL Server], principals
- '##MS_SQLAuthenticatorCertificate##'
- principals [SQL Server]
- '##MS_SQLResourceSigningCertificate##'
- groups [SQL Server], principals
- '##MS_AgentSigningCertificate##'
- authentication [SQL Server], principals
- schemas [SQL Server], principals
- principals [SQL Server], about principals
- security [SQL Server], principals
- users [SQL Server], principals
- '##MS_SQLReplicationSigningCertificate##'
ms.assetid: 3f7adbf7-6e40-4396-a8ca-71cbb843b5c2
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e0fe888ab35d160b99d99823d8be5eb91388b779
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463148"
---
# <a name="principals-database-engine"></a>主体（数据库引擎）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  “主体” 是可以请求 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源的实体。 与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 授权模型的其他组件一样，主体也可以按层次结构排列。 主体的影响范围取决于主体的定义范围：Windows、服务器、数据库；以及主体是不可分割还是集合。 例如，Windows 登录名就是一个不可分主体，而 Windows 组则是一个集合主体。 每个主体都具有一个安全标识符 (SID)。 本主题适用于所有版本的 SQL Server，但在 SQL 数据库或 Azure Synapse Analytics 的服务器级别主体上有一些限制。 
  
## <a name="sql-server-level-principals"></a>SQL Server 级的主体  
  
- [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证登录名   
- Windows 用户的 Windows 身份验证登录名  
- Windows 组的 Windows 身份验证登录名   
- AD 用户的 Azure Active Directory 身份验证登录名
- AD 组的 Azure Active Directory 身份验证登录名
- 服务器角色  
  
## <a name="database-level-principals"></a>数据库级的主体
  
- 数据库用户（有 12 个类型的用户。 有关详细信息，请参阅 [CREATE USER](../../../t-sql/statements/create-user-transact-sql.md)。）
- 数据库角色
- 应用程序角色
  
## <a name="sa-login"></a>sa 登录名  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] `sa` 登录名是服务器级别主体。 默认情况下，该登录名是在安装实例时创建的。 从 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]开始，sa 的默认数据库为“master”。 这是对早期版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的行为的更改。 `sa` 登录名是 `sysadmin` 固定服务器级别角色的成员。 `sa` 登录名具有服务器上的所有权限，并且不能受到限制。 `sa` 登录名无法删除，但可以禁用，以便任何人都无法使用它。

## <a name="dbo-user-and-dbo-schema"></a>dbo 用户和 dbo 架构

`dbo` 用户是每个数据库中的特殊用户主体。 所有 SQL Server 管理员、`sysadmin` 固定服务器角色成员、`sa` 登录名和数据库所有者，均以 `dbo` 用户身份进入数据库。 `dbo` 用户有数据库中的所有权限，并且不能被限制或删除。 `dbo` 代表数据库所有者，但 `dbo` 用户帐户与 `db_owner` 固定数据库角色不同，并且 `db_owner` 固定数据库角色与作为数据库所有者记录的用户帐户不同。     
`dbo` 用户拥有 `dbo` 架构。 `dbo` 架构是所有用户的默认架构，除非指定了其他某个架构。  `dbo` 架构无法删除。
  
## <a name="public-server-role-and-database-role"></a>公共服务器角色和数据库角色  
每个登录名都属于 `public` 固定服务器角色，并且每个数据库用户都属于 `public` 数据库角色。 当尚未为某个登录名或用户授予或拒绝为其授予对安全对象的特定权限时，该登录名或用户将继承已授予该安全对象的公共角色的权限。 `public` 固定服务器角色和 `public` 固定服务器角色无法删除。 但是，可以从 `public` 角色撤消权限。 默认情况下有许多权限已分配给 `public` 角色。 这些权限中的大部分是执行数据库中的日常操作（每个人都应能够执行的操作类型）所需的。 从公共登录名或用户撤消权限时应十分小心，因为这将影响所有登录名/用户。 通常不应拒绝公共登录名或用户的权限，因为 Deny 语句会覆盖你可能对个别登录名或用户设定的任何 Grant 语句。 
  
## <a name="information_schema-and-sys-users-and-schemas"></a>INFORMATION_SCHEMA 和 sys 用户与架构 
 每个数据库都包含两个实体，并且这些实体都作为用户显示在目录视图中：`INFORMATION_SCHEMA` 和 `sys`。 这些实体供数据库引擎内部使用。 它们无法修改或删除。  
  
## <a name="certificate-based-sql-server-logins"></a>基于证书的 SQL Server 登录名  
 名称由双井号 (##) 括起来的服务器主体仅供内部系统使用。 下列主体是在安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时从证书创建的，不应删除。  
  
-   \##MS_SQLResourceSigningCertificate##    
-   \##MS_SQLReplicationSigningCertificate##    
-   \##MS_SQLAuthenticatorCertificate##    
-   \##MS_AgentSigningCertificate##   
-   \##MS_PolicyEventProcessingLogin##   
-   \##MS_PolicySigningCertificate##   
-   \##MS_PolicyTsqlExecutionLogin##   
 
 管理员不可更改这些主体帐户的密码，因为这些密码基于颁发给 Microsoft 的证书。
  
## <a name="the-guest-user"></a>guest 用户  
 每个数据库包括一个 `guest`的行为的更改。 授予 `guest` 用户的权限由对数据库具有访问权限，但在数据库中没有用户帐户的用户继承。 无法删除 `guest` 用户，但可通过撤消其 CONNECT 权限将其禁用。 可以通过在 `master` 或 `tempdb` 以外的任何数据库中执行 `REVOKE CONNECT FROM GUEST;` 来撤消 CONNECT 权限。  
  
  
## <a name="related-tasks"></a>Related Tasks  
 有关设计权限系统的信息，请参阅 [Getting Started with Database Engine Permissions](../../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。  
  
 下列主题包括在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书的本节中：  
  
-   [管理登录名、用户和架构操作指南主题](./create-a-login.md)  
  
-   [服务器级角色](../../../relational-databases/security/authentication-access/server-level-roles.md)  
  
-   [数据库级别的角色](../../../relational-databases/security/authentication-access/database-level-roles.md)  
  
-   [应用程序角色](../../../relational-databases/security/authentication-access/application-roles.md)  
  
## <a name="see-also"></a>另请参阅  
 [保护 SQL Server](../../../relational-databases/security/securing-sql-server.md)   
 [sys.database_principals (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.sql_logins (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)   
 [sys.database_role_members (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [服务器级别角色](../../../relational-databases/security/authentication-access/server-level-roles.md)   
 [数据库级别的角色](../../../relational-databases/security/authentication-access/database-level-roles.md)  
  
