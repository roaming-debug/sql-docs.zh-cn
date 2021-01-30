---
description: sp_droprolemember (Transact-SQL)
title: sp_droprolemember (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droprolemember_TSQL
- sp_droprolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droprolemember
ms.assetid: c2f19ab1-e742-4d56-ba8e-8ffd40cf4925
ms.author: vanto
author: VanMSFT
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d6a44c56c53502613f665da3e96d7e8abb38d5aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158141"
---
# <a name="sp_droprolemember-transact-sql"></a>sp_droprolemember (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从当前数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 角色中删除安全帐户。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  

### <a name="syntax-for-both-sql-server-and-azure-sql-database"></a>SQL Server 和 Azure SQL 数据库的语法

```syntaxsql  
sp_droprolemember [ @rolename = ] 'role' ,   
     [ @membername = ] 'security_account'  
```  

### <a name="syntax-for-both-azure-synapse-analytics-and-parallel-data-warehouse"></a>适用于 Azure Synapse 分析和并行数据仓库的语法

```syntaxsql  
sp_droprolemember 'role' ,  
     'security_account'  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>参数  
`[ @rolename = ] 'role'` 要从中删除成员的角色的名称。 *role* 的值为 **sysname**，无默认值。 *角色* 必须存在于当前数据库中。  
  
`[ @membername = ] 'security_account'` 要从角色中删除的安全帐户的名称。 *security_account* **sysname**，无默认值。 *security_account* 可以是数据库用户、其他数据库角色、windows 登录名或 windows 组。 当前数据库中必须存在 *security_account* 。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 sp_droprolemember 通过从 sysmembers 表中删除行来删除数据库角色的成员。 在从角色中删除一个成员后，该成员将失去作为该角色的成员所拥有的任何权限。  
  
 若要删除固定服务器角色的用户，请使用 sp_dropsrvrolemember。 不能删除 public 角色的用户，也不能从任何角色中删除 dbo。  
  
 使用 sp_helpuser 查看角色的成员 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并使用 ALTER role 将成员添加到角色。  
  
## <a name="permissions"></a>权限  
 要求具有角色的 ALTER 权限。  
  
## <a name="examples"></a>示例  
 以下示例将删除角色 `JonB` 中的用户 `Sales`。  
  
```sql
EXEC sp_droprolemember 'Sales', 'Jonb';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例将删除角色 `JonB` 中的用户 `Sales`。  
  
```sql
EXEC sp_droprolemember 'Sales', 'JonB'  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprole (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droprole-transact-sql.md)   
 [sp_dropsrvrolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)   
 [sp_helpuser &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

