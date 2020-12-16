---
title: 安全对象 | Microsoft Docs
description: 了解安全对象范围，SQL Server 数据库引擎授权系统用它们来控制对安全对象的访问。
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.selectobject.f1
helpviewer_keywords:
- securables [SQL Server]
- schemas [SQL Server], securables
- database securables [SQL Server]
- hierarchies [SQL Server], securables
- server securables [SQL Server]
ms.assetid: bfa748f0-70b0-453c-870a-04b7b205b9ff
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8d780aa878779bf25a3854dbd0c7c392800f7039
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432202"
---
# <a name="securables"></a>安全对象
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  安全对象是 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 授权系统控制对其进行访问的资源。 例如，表是安全对象。 通过创建可以为自己设置安全性的名为“范围”的嵌套层次结构，可以将某些安全对象包含在其他安全对象中。 安全对象范围有 **服务器**、 **数据库** 和 **架构**。  
  
## <a name="securable-scope-server"></a>安全对象范围：服务器  
 **服务器** 安全对象范围包含以下安全对象：  
  
-   可用性组 (availability group)  
  
-   端点  
  
-   登录  
  
-   服务器角色  
  
-   数据库  
  
## <a name="securable-scope-database"></a>安全对象范围：数据库  
 **数据库** 安全对象范围包含以下安全对象：  
  
-   应用程序角色  
  
-   Assembly  
  
-   非对称密钥  
  
-   证书  
  
-   合约  
  
-   全文目录  
  
-   全文非索引字表  
  
-   消息类型  
  
-   远程服务绑定  
  
-   （数据库）角色  
  
-   路由  
  
-   架构  
  
-   搜索属性列表  
  
-   服务  
  
-   对称密钥  
  
-   用户  
  
## <a name="securable-scope-schema"></a>安全对象范围：架构  
 **架构** 安全对象范围包含以下安全对象：  
  
-   类型  
  
-   XML 架构集合  
  
-   对象 - 对象类包含以下成员：  
  
    -   聚合  
  
    -   函数  
  
    -   过程  
  
    -   队列  
  
    -   同义词  
  
    -   表  
  
    -   查看 
    
    -   外部表 
  
## <a name="controlling-access-to-a-securable"></a>控制对安全对象的访问  
 接收对安全对象的权限的实体称为主体。 最常见的主体是登录名和数据库用户。 对安全对象的访问通过授予或拒绝权限进行控制，或者通过将登录名和用户添加到有权访问的角色进行控制。 有关控制权限的信息，请参阅 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)、[REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)、[DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)、[sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) 和 [sp_droprolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)。  
  
> [!CAUTION]  
>  安装期间授予系统对象的默认权限已针对可能的威胁进行了仔细评估，并且作为强化 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的一部分，无需进行更改。 对系统对象权限的任何更改都可能限制或破坏功能，并且可能让你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装处于不受支持的状态。  
  
## <a name="related-content"></a>相关内容  
 [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
 [保护 SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
 [sys.database_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)  
  
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
 [sys.server_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
 [sys.sql_logins (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)  
  
  
