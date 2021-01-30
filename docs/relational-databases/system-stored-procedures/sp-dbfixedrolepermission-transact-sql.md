---
description: sp_dbfixedrolepermission (Transact-SQL)
title: sp_dbfixedrolepermission (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbfixedrolepermission
- sp_dbfixedrolepermission_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbfixedrolepermission
ms.assetid: b8c30191-f532-49cd-83f3-c271f63ce572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 78f1381d05c5841ce48e395f8cc1cd477b820e01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201273"
---
# <a name="sp_dbfixedrolepermission-transact-sql"></a>sp_dbfixedrolepermission (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示固定数据库角色的权限。 **sp_dbfixedrolepermission** 将在中返回正确的信息 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。 该输出不反映对 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中实现的权限层次结构的更改。 有关详细信息，请参阅 [数据库级角色](../../relational-databases/security/authentication-access/database-level-roles.md#fixed-database-roles)，其中显示固定数据库角色的列表及其相应权限。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dbfixedrolepermission [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @rolename = ] 'role'` 有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 固定数据库角色的名称。 *role* 的值为 **sysname**，默认值为 NULL。 如果未指定 *role* ，将显示所有固定数据库角色的权限。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|固定数据库角色的名称|  
|**权限**|**nvarchar (70)**|与 **DbFixedRole** 关联的权限|  
  
## <a name="remarks"></a>备注  
 若要显示固定数据库角色的列表，请执行 **sp_helpdbfixedrole**。 下表显示了固定数据库角色。  
  
|固定数据库角色|说明|  
|-------------------------|-----------------|  
|**db_owner**|数据库所有者|  
|**db_accessadmin**|数据库访问管理员|  
|**db_securityadmin**|数据库安全管理员|  
|**db_ddladmin**|数据库数据定义语言 (DDL) 管理员|  
|**db_backupoperator**|数据库备份操作员|  
|**db_datareader**|数据库数据读取者|  
|**db_datawriter**|数据库数据写入者|  
|**db_denydatareader**|数据库拒绝数据读取者|  
|**db_denydatawriter**|数据库拒绝数据写入者|  
  
 **Db_owner** 固定数据库角色的成员拥有所有其他固定数据库角色的权限。 若要显示固定服务器角色的权限，请执行 **sp_srvrolepermission**。  
  
 结果集包括可执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句和该数据库角色的成员可执行的其他特殊操作。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下查询将返回所有固定数据库角色的权限，因为该查询未指定固定数据库角色。  
  
```  
EXEC sp_dbfixedrolepermission;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helpdbfixedrole &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)   
 [sp_srvrolepermission &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
