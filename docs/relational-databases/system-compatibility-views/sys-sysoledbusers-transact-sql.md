---
description: sys.sysoledbusers (Transact-SQL)
title: sys.sysoledbusers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysoledbusers
- sys.sysoledbusers_TSQL
- sysoledbusers
- sysoledbusers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysoledbusers system table
- sys.sysoledbusers compatibility view
ms.assetid: fe924c17-9cad-4b2b-8124-1e0fd82931e3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4e8c134ad74b24704696b586bb362ab253dedb84
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99148537"
---
# <a name="syssysoledbusers-transact-sql"></a>sys.sysoledbusers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

    
> [!IMPORTANT]  
>  [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 中包括此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统表只为了向后兼容。 建议改用 [目录视图](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md) 。  
  
 指定的链接服务器的每个用户和密码映射在表中对应一行。 **sysoledbusers** 存储在 **master** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**rmtsrvid**|**smallint**|服务器的安全标识号 (SID)。|  
|**rmtloginame**|**nvarchar (** 128 **)**|**Loginsid** 映射到的链接 **rmtservid** 的远程登录名。|  
|**rmtpassword**|**nvarchar (** 128 **)**|返回 NULL。|  
|**loginsid**|**varbinary (** 85 **)**|要映射的本地登录 SID。|  
|**status**|**smallint**|如果为 1，则映射应使用用户凭据。|  
|**changedate**|**datetime**|最近更改映射信息的日期。|  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
