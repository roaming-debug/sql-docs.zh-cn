---
description: dbo.sysproxies (Transact-SQL)
title: " (Transact-sql) dbo.sys代理 |Microsoft Docs"
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysproxies_TSQL
- sysproxies_TSQL
- dbo.sysproxies
- sysproxies
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxies system table
ms.assetid: a73da875-be22-45fc-b5e2-ea7ebd48e2d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3d04cd1822a9c7e0edc309a3500e800b2df58fd0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098309"
---
# <a name="dbosysproxies-transact-sql"></a>dbo.sysproxies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  定义 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户的属性。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|代理帐户的 ID。|  
|name|**sysname**|代理帐户的名称。|  
|**credential_id**|**int**|代理帐户使用的凭证的 ID。|  
|**enabled**|**tinyint**|代理帐户的状态。<br /><br /> **0** = 禁用。 **1** = 已启用。|  
|description|**nvarchar(512)**|创建代理帐户时用户输入的说明。|  
|**user_sid**|**varbinary(85)**|与代理凭据关联的用户或组的 Microsoft Windows *security_identifier* 。|  
|**credential_date_created**|**datetime**|凭证创建的日期和时间。|  
  
## <a name="remarks"></a>备注  
 只有 **sysadmin** 固定服务器角色的成员才能访问 **sysproxies** 表。  
  
## <a name="see-also"></a>另请参阅  
 [dbo.sysproxylogin &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)   
 [dbo.sysproxysubsystem &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [dbo.sys子系统 &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)  
  
  
