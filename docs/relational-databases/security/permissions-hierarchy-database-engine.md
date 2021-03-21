---
title: 权限层次结构（数据库引擎）| Microsoft Docs
description: 了解可在 SQL Server 数据库引擎中用权限保护的实体（称为安全对象）的层次结构。
ms.custom: ''
ms.date: 03/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.server.permissions.f1--May use common.permissions
helpviewer_keywords:
- security [SQL Server], denying access
- hierarchies [SQL Server], permissions
- securables [SQL Server]
- security [SQL Server], permissions
- permissions [SQL Server], hierarchy
- security [SQL Server], granting access
ms.assetid: f6d20a55-ef03-4e14-85f9-009902889866
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd021a3306d7bca7d72f02430d52d787c06a23a2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748057"
---
# <a name="permissions-hierarchy-database-engine"></a>权限层次结构（数据库引擎）
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssDE](../../includes/ssde-md.md)] 管理着可以通过权限进行保护的实体的分层集合。 这些实体称为“安全对象” 。 最主要的安全对象是服务器和数据库，但可以在更细化的级别设置各种权限。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过验证主体是否已被授予适当权限来控制主体对安全对象的操作。  
  
 下图显示了 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 权限层次结构之间的关系。  
  
 权限系统在所有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]、 [!INCLUDE[ssDW](../../includes/ssdw-md.md)]、 [!INCLUDE[ssAPS](../../includes/ssaps-md.md)]版本中的工作方式相同，但是有些功能并不是在所有版本中都可用。 例如，不能在 Azure 产品中配置服务器级权限。  
  
 ![数据库引擎权限层次结构图](../../relational-databases/security/media/wj-security-layers.gif "数据库引擎权限层次结构图")  
  
## <a name="chart-of-sql-server-permissions"></a>SQL Server 权限图表  
 若要获取 pdf 格式的所有 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 权限的海报大小的图表，请参阅 [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster)。  
  
## <a name="working-with-permissions"></a>使用权限  
 可以使用常见的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询 GRANT、DENY 和 REVOKE 来操作权限。 有关权限的信息，可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 和 [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) 目录视图中看到。 也可以使用内置函数来查询权限信息。  
  
 有关设计权限系统的信息，请参阅 [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。  
  
## <a name="see-also"></a>另请参阅  
 [保护 SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [安全对象](../../relational-databases/security/securables.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.server_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
