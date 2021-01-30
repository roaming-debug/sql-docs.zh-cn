---
description: dbo.sysnotifications (Transact-SQL)
title: " (Transact-sql) dbo.sys通知 |Microsoft Docs"
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysnotifications_TSQL
- sysnotifications
- sysnotifications_TSQL
- dbo.sysnotifications
dev_langs:
- TSQL
helpviewer_keywords:
- sysnotifications system table
ms.assetid: c5150d18-e8b7-48a7-ada7-77c583af6e41
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0a1cd54f7f2cafb73bba1c74674810e2a1362327
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209623"
---
# <a name="dbosysnotifications-transact-sql"></a>dbo.sysnotifications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对每个通知包含一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|警报的 ID。|  
|**operator_id**|**int**|应向其发送此通知的操作员 ID。|  
|**notification_method**|**tinyint**|通知方法：<br /><br /> **1** = 电子邮件<br /><br /> **2** = 寻呼<br /><br /> **4**  = **netsend**<br /><br /> **7** = 全部|  
  
  
