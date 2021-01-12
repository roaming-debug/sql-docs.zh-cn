---
description: dbo.sysjobstepslogs (Transact-SQL)
title: dbo.sysjobstepslogs (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysjobstepslogs_TSQL
- sysjobstepslogs_TSQL
- sysjobstepslogs
- dbo.sysjobstepslogs
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobstepslogs system table
ms.assetid: 128c25db-0b71-449d-bfb2-38b8abcf24a0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 815133f093a108d7f1e4b9841ffbd3310b3dd758
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097410"
---
# <a name="dbosysjobstepslogs-transact-sql"></a>dbo.sysjobstepslogs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤的作业步骤日志，这些作业步骤配置为将作业步骤输出写入表中。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**log_id**|**int**|作业步骤日志的 ID。|  
|**log**|**nvarchar(max)**|作业步骤日志内容。|  
|**date_created**|**datetime**|创建作业步骤日志的日期和时间。|  
|**date_modified**|**datetime**|上次修改作业步骤日志的日期和时间。|  
|**log_size**|**int**|作业步骤日志的大小（以字节为单位）。|  
|**step_uid**|**uniqueidentifier**|作业步骤的唯一标识符。|  
  
## <a name="see-also"></a>另请参阅  
 [sp_help_jobsteplog &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-jobsteplog-transact-sql.md)   
 [sp_delete_jobsteplog &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobsteplog-transact-sql.md)  
  
  
