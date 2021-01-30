---
description: sp_helpdbfixedrole (Transact-SQL)
title: sp_helpdbfixedrole (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdbfixedrole
- sp_helpdbfixedrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdbfixedrole
ms.assetid: ad87e9a0-b901-4e37-9950-aa517d680fc3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 817947901ca7f35ba2972004d23ed8d7f85215ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176429"
---
# <a name="sp_helpdbfixedrole-transact-sql"></a>sp_helpdbfixedrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回固定数据库角色的列表。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpdbfixedrole [ [ @rolename = ] 'role' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @rolename = ] 'role'` 固定数据库角色的名称。 *role* 的值为 **sysname**，默认值为 NULL。 如果指定了 *role* ，则仅返回有关该角色的信息;否则，将返回所有固定数据库角色的列表和说明。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|固定数据库角色的名称。|  
|**说明**|**nvarchar (70)**|DbFixedRole 的说明 **。**|  
  
## <a name="remarks"></a>备注  
 固定数据库角色（如下表所示）在数据库级上定义，具有执行特定数据库级的管理活动的权限。 无法添加或删除固定数据库角色。 无法更改授予固定数据库角色的权限。  
  
|固定数据库角色|说明|  
|-------------------------|-----------------|  
|**db_owner**|数据库所有者|  
|**db_accessadmin**|数据库访问管理员|  
|**db_securityadmin**|数据库安全管理员|  
|**db_ddladmin**|数据库 DDL 管理员|  
|**db_backupoperator**|数据库备份操作员|  
|**db_datareader**|数据库数据读取者|  
|**db_datawriter**|数据库数据写入者|  
|**db_denydatareader**|数据库拒绝数据读取者|  
|**db_denydatawriter**|数据库拒绝数据写入者|  
  
 下表显示了用于修改数据库角色的存储过程。  
  
|存储过程|操作|  
|----------------------|------------|  
|**sp_addrolemember**|将数据库用户添加到固定数据库角色中。|  
|**sp_helprole**|显示固定数据库角色的成员列表。|  
|**sp_droprolemember**|从固定数据库角色中删除成员。|  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
 返回的信息取决于对元数据的访问权限的限制。 主体对其不具有权限的实体将不会显示。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
 以下示例显示了所有固定数据库角色的列表。  
  
```  
EXEC sp_helpdbfixedrole;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_dbfixedrolepermission &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)   
 [sp_droprolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
