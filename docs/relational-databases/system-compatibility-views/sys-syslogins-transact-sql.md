---
description: sys.syslogins (Transact-SQL)
title: " (Transact-sql) sys.sys登录名 |Microsoft Docs"
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syslogins_TSQL
- syslogins
- sys.syslogins
- sys.syslogins_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.syslogins compatibility view
- syslogins system table
ms.assetid: 4cb34f17-a4bb-469f-a218-71f074e6308f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f051ffe3a01c4d2dabccfcc43c806dc21e4b8785
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201364"
---
# <a name="syssyslogins-transact-sql"></a>sys.syslogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个登录帐户在表中对应一行。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到[当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**sid**|**varbinary(85)**|安全标识符。|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**createdate**|**datetime**|添加登录的日期。|  
|**updatedate**|**datetime**|更新登录的日期。|  
|**accdate**|**datetime**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**totcpu**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**totio**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**spacelimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**timelimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**resultlimit**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|name|**sysname**|用户的登录名。|  
|**dbname**|**sysname**|建立连接时，用户的默认数据库名。|  
|**password**|**nvarchar(128)**|返回 NULL。|  
|**language**|**sysname**|默认的用户语言。|  
|**denylogin**|**int**|1 = 登录名是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户或组且已被拒绝访问。|  
|**hasaccess**|**int**|1 = 已授权登录名访问服务器。|  
|**isntname**|**int**|1 = 登录名是 Windows 用户或组。<br /><br /> 0 = 登录名是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。|  
|**isntgroup**|**int**|1 = 登录名是 Windows 组。|  
|**isntuser**|**int**|1 = 登录名是 Windows 用户。|  
|**sysadmin**|**int**|1 = 登录名是 **sysadmin** 服务器角色的成员。|  
|**securityadmin**|**int**|1 = 登录名是 **securityadmin** 服务器角色的成员。|  
|**serveradmin**|**int**|1 = 登录名是 **serveradmin** 固定服务器角色的成员。|  
|**setupadmin**|**int**|1 = 登录名是 **setupadmin** 固定服务器角色的成员。|  
|**processadmin**|**int**|1 = 登录名是 **processadmin** 固定服务器角色的成员。|  
|**diskadmin**|**int**|1 = 登录名是 **diskadmin** 固定服务器角色的成员。|  
|**dbcreator**|**int**|1 = 登录名是 **dbcreator** 固定服务器角色的成员。|  
|**bulkadmin**|**int**|1 = 登录名是 **bulkadmin** 固定服务器角色的成员。|  
|**loginname**|**nvarchar(128)**|用户的登录名。 提供该列是为了向后兼容。|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
