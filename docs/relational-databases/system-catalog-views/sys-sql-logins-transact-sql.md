---
description: sys.sql_logins (Transact-SQL)
title: sys.sql_logins (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sql_logins_TSQL
- sql_logins_TSQL
- sys.sql_logins
- sql_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_logins catalog view
ms.assetid: 0d9c5b09-86fe-40ff-baab-00b7c051402f
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bb0fc659d82024dbbc52dc777b9f6ae5da3062ac
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477328"
---
# <a name="syssql_logins-transact-sql"></a>sys.sql_logins (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  为每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|--|继承自 **sys.server_principals**。|  
|**is_policy_checked**|**bit**|检查密码策略。|  
|**is_expiration_checked**|**bit**|检查密码过期。|  
|**password_hash**|**varbinary(256)**|SQL 登录密码的哈希。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，存储的密码信息使用 SHA-512 加盐密码进行计算。|  
  
 有关此视图所继承的列的列表，请参阅 [&#40;transact-sql&#41;sys.server_principals ](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)。 列 `owning_principal_id` 和 `is_fixed_role` 不是继承自 sys.server_principals。
  
## <a name="remarks"></a>备注  
 若要查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名和 Windows 身份验证登录名，请参阅 [&#40;transact-sql&#41;sys.server_principals ](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)。  
  
 如果启用包含的数据库用户，则无需登录即可建立连接。 若要标识这些帐户，请参阅  [&#40;transact-sql&#41;sys.database_principals ](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录都会显示各自的登录名称和 sa 登录信息。 要查看其他登录，需要获取 ALTER ANY LOGIN 或有关登录的权限。  
 若要查看 password_hash 列的内容，则需要 CONTROL SERVER 权限。
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [密码策略](../../relational-databases/security/password-policy.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
