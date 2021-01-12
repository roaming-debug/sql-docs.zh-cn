---
description: dbo.sysjobs (Transact-SQL)
title: " (Transact-sql) dbo.sys作业 |Microsoft Docs"
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysjobs
- sysjobs_TSQL
- dbo.sysjobs
- dbo.sysjobs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobs system table
ms.assetid: e244a6a5-54c2-47a6-8039-dd1852b0ae59
author: cawrites
ms.author: chadam
ms.openlocfilehash: e36afae06020e9e970359cb09bc79a9bdf346b03
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091495"
---
# <a name="dbosysjobs-transact-sql"></a>dbo.sysjobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  存储将由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理执行的各个预定作业的信息。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|作业的唯一 ID。|  
|**originating_server_id**|**int**|发出作业的服务器的 ID。|  
|name|**sysname**|作业的名称。|  
|**enabled**|**tinyint**|指示是否启用要执行的作业。|  
|description|**nvarchar(512)**|对作业的说明。|  
|**start_step_id**|**int**|执行作业的起始步骤的 ID。|  
|**category_id**|**int**|作业类别的 ID。|  
|**owner_sid**|**varbinary(85)**|作业所有者的安全标识号 (SID)。|  
|**notify_level_eventlog**|**int**|**位掩码** ，指示在何种情况下，通知事件应记录到 Microsoft Windows 应用程序日志中：<br /><br /> **0** = 从不<br /><br /> **1** = 作业成功时<br /><br /> **2** = 作业失败时<br /><br /> **3** = 无论作业完成与否 (，无论作业结果如何) |  
|**notify_level_email**|**int**|位掩码，指示在何种情况下应在作业完成时发送通知电子邮件：<br /><br /> **0** = 从不<br /><br /> **1** = 作业成功时<br /><br /> **2** = 作业失败时<br /><br /> **3** = 无论作业完成与否 (，无论作业结果如何) |  
|**notify_level_netsend**|**int**|位掩码，指示在何种情况下应在作业完成时发送网络消息：<br /><br /> **0** = 从不<br /><br /> **1** = 作业成功时<br /><br /> **2** = 作业失败时<br /><br /> **3** = 无论作业完成与否 (，无论作业结果如何) |  
|**notify_level_page**|**int**|位掩码，指示在何种情况下应在作业完成时发送寻呼：<br /><br /> **0** = 从不<br /><br /> **1** = 作业成功时<br /><br /> **2** = 作业失败时<br /><br /> **3** = 无论作业完成与否 (，无论作业结果如何) |  
|**notify_email_operator_id**|**int**|被通知的操作员的电子邮件名称。|  
|**notify_netsend_operator_id**|**int**|发送网络消息时使用的计算机或用户的 ID。|  
|**notify_page_operator_id**|**int**|发送寻呼时使用的计算机或用户的 ID。|  
|**delete_level**|**int**|**位掩码** ，指示在何种情况下应在作业完成时删除作业：<br /><br /> **0** = 从不<br /><br /> **1** = 作业成功时<br /><br /> **2** = 作业失败时<br /><br /> **3** = 无论作业完成与否 (，无论作业结果如何) |  
|**date_created**|**datetime**|作业的创建日期。|  
|**date_modified**|**datetime**|上次修改作业的日期。|  
|**version_number**|**int**|作业版本。|  
  
  
