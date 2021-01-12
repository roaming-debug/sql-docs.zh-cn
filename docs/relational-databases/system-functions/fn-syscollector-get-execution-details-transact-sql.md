---
description: fn_syscollector_get_execution_details (Transact-SQL)
title: fn_syscollector_get_execution_details (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_syscollector_get_execution_details_TSQL
- fn_syscollector_get_execution_details
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_details function
ms.assetid: d59ddf0c-72c0-4c57-bc83-aef260e4e105
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 40201cb1fed3938da075b3fd5e97380eb0dddfc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093841"
---
# <a name="fn_syscollector_get_execution_details-transact-sql"></a>fn_syscollector_get_execution_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回与给定包的 package_execution_id 相匹配的 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 日志 (sysssislog) 部分。 由包或其任务和容器在运行时生成的每个日志记录项在表中各占一行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_syscollector_get_execution_details ( log_id )  
```  
  
## <a name="arguments"></a>参数  
 *log_id*  
 执行日志的本地唯一标识符。 *log_id* 是 **int**。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|id|**int**|日志记录项的唯一标识符。|  
|event|**sysname**|生成日志记录项的事件的名称。|  
|computer|**nvarchar**|生成日志记录条目时运行包的计算机。|  
|operator|**nvarchar**|运行生成日志记录项的包的人员或代理的用户名。|  
|source|**nvarchar**|生成日志记录项的可执行文件的名称。|  
|sourceid|**uniqueidentifier**|生成日志记录项的可执行文件的 GUID。|  
|executionid|**uniqueidentifier**|生成日志记录条目的可执行文件的执行实例的 GUID。|  
|starttime|**datetime**|包开始运行的时间。|  
|endtime|**datetime**|包完成的时间。|  
|datacode|**int**|用于标识与日志项关联的事件的整数值。 “0”指示事件未提供标识符。|  
|databytes|**图像**|用于标识返回值的字节数组。|  
|message|**nvarchar**|事件以及与事件关联的信息的说明。|  
  
## <a name="permissions"></a>权限  
 需要 **dc_operator** 的 SELECT 权限。  
  
## <a name="see-also"></a>另请参阅  
 [在 SQL Server Data Tools 中启用包日志记录](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)   
 [“数据收集”](../../relational-databases/data-collection/data-collection.md)  
  
  
