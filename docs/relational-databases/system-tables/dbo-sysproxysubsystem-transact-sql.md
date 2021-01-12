---
description: dbo.sysproxysubsystem (Transact-SQL)
title: dbo.sysproxysubsystem (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysproxysubsystem_TSQL
- dbo.sysproxysubsystem
- sysproxysubsystem_TSQL
- sysproxysubsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxysubsystem system table
ms.assetid: 6d7713f5-1253-4a19-b1fb-635c377c95c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 700d55e45dc153834cfb12aa9883f8faa6fac909
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098295"
---
# <a name="dbosysproxysubsystem-transact-sql"></a>dbo.sysproxysubsystem (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  记录每个代理帐户所用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理子系统。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|子系统的 ID。 此值对应于 **syssubsystems** 表中的 **subsystem_id** 列。|  
|**proxy_id**|**int**|代理帐户的 ID。 此值对应于 **sysproxies** 表中的 **proxy_id** 列。|  
  
## <a name="remarks"></a>备注  
 只有 **sysadmin** 固定服务器角色的成员才能访问此表。  
  
## <a name="see-also"></a>另请参阅  
 [dbo.sys子系统 &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)   
 [ &#40;Transact-sql 的dbo.sys代理&#41;](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
  
  
