---
description: sysdbmaintplans (Transact-SQL)
title: sysdbmaintplans (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdbmaintplans_TSQL
- sysdbmaintplans
dev_langs:
- TSQL
helpviewer_keywords:
- sysdbmaintplans system table
ms.assetid: 0363296a-3082-48a9-9eb5-a1020b2f541a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2aec8e8b94dc341225a5e34e35f2f168fa83e56e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098062"
---
# <a name="sysdbmaintplans-transact-sql"></a>sysdbmaintplans (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中包含此表，以保留已从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本升级而来的实例的现有信息。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不更改此表的内容。 该表存储在 **msdb** 数据库中。  
  
 [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]  

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**plan_id**|**uniqueidentifier**|数据库维护计划 ID。|  
|**plan_name**|**sysname**|数据库维护计划名称。|  
|**date_created**|**datetime**|创建数据库维护计划的日期。|  
|**owner**|**sysname**|数据库维护计划的所有者。|  
|**max_history_rows**|**int**|在系统表中，为记录数据库维护计划的历史所分配的最大行数。|  
|**remote_history_server**|**sysname**|远程服务器的名称，可以将历史报表写入该远程服务器中。|  
|**max_remote_history_rows**|**int**|远程服务器上的系统表中所分配的最大行数，可以将历史报表写入该远程服务器中。|  
|**user_defined_1**|**int**|默认值为 NULL。|  
|**user_defined_2**|**nvarchar (100)**|默认值为 NULL。|  
|**user_defined_3**|**datetime**|默认值为 NULL。|  
|**user_defined_4**|**uniqueidentifier**|默认值为 NULL。|  
|**log_shipping**|**bit**|日志传送状态：<br /><br /> **0** = 禁用 **1** = 已启用|  
  
  
