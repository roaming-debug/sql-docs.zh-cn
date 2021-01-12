---
description: dbo.syssubsystems (Transact-SQL)
title: dbo.sys子系统 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.syssubsystems
- syssubsystems
- syssubsystems_TSQL
- dbo.syssubsystems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syssubsystems system table
ms.assetid: 114b3d55-1ad6-4777-b868-8ef0c86ba596
author: cawrites
ms.author: chadam
ms.openlocfilehash: ebd56d0ad3847aea411c27af5852d652b84e360e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098268"
---
# <a name="dbosyssubsystems-transact-sql"></a>dbo.syssubsystems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含有关所有可用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理子系统的信息。 **Syssubsystems** 表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|子系统的 ID。|  
|**适用**|**nvarchar(40)**|子系统的名称。|  
|**description_id**|**int**|**Sys.databases** 目录视图中包含子系统说明的行的消息 ID。|  
|**subsystem_dll**|**nvarchar(255)**|子系统 DLL 的位置。|  
|**agent_exe**|**nvarchar(255)**|使用子系统的可执行文件的完整路径。|  
|**start_entry_point**|**nvarchar(30)**|初始化子系统时调用的函数。|  
|**event_entry_point**|**nvarchar(30)**|运行子系统步骤时调用的函数。|  
|**stop_entry_point**|**nvarchar(30)**|子系统运行完成时调用的函数。|  
|**max_worker_threads**|**int**|给定子系统的最大并发步骤数。|  
  
## <a name="remarks"></a>备注  
 只有 **sysadmin** 固定服务器角色的成员才能访问此表。  
  
## <a name="see-also"></a>另请参阅  
 [dbo.sysproxysubsystem &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [ &#40;Transact-sql 的dbo.sys代理&#41;](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)   
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)  
  
  
