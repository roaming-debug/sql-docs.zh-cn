---
description: sys.database_role_members (Transact-SQL)
title: sys.database_role_members (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.database_role_members_TSQL
- sys.database_role_members
- database_role_members_TSQL
- database_role_members
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_role_members catalog view
ms.assetid: ed1b019d-ca48-4db3-85df-cf6d2db591cf
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 603ecd155e076b4f8798e7d5259eee902e4eab79
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97405416"
---
# <a name="sysdatabase_role_members-transact-sql"></a>sys.database_role_members (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为每个数据库角色的每个成员返回一行。  数据库用户、应用程序角色和其他数据库角色可以是数据库角色的成员。 若要向角色添加成员，请将 [ALTER role](../../t-sql/statements/alter-role-transact-sql.md) 语句与选项一起使用 `ADD MEMBER` 。 与 [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) 联接以返回值的名称 `principal_id` 。
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**role_principal_id**|**int**|角色的数据库主体 ID。|  
|**member_principal_id**|**int**|成员的数据库主体 ID。|  
  
## <a name="permissions"></a>权限  
 任何用户都可以查看自己的成员身份。 若要查看其他角色成员身份，需要具有 `db_securityadmin` 固定数据库角色的成员身份或 `VIEW DEFINITION` 数据库。  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="example"></a>示例  
 下面的查询返回数据库角色的成员。  
  
```  
SELECT DP1.name AS DatabaseRoleName,   
   isnull (DP2.name, 'No members') AS DatabaseUserName   
 FROM sys.database_role_members AS DRM  
 RIGHT OUTER JOIN sys.database_principals AS DP1  
   ON DRM.role_principal_id = DP1.principal_id  
 LEFT OUTER JOIN sys.database_principals AS DP2  
   ON DRM.member_principal_id = DP2.principal_id  
WHERE DP1.type = 'R'
ORDER BY DP1.name;  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
[ALTER ROLE (SQLL) ](../../t-sql/statements/alter-role-transact-sql.md)      
[sys.server_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
  


