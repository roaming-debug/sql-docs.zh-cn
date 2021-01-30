---
description: sp_trace_setstatus (Transact-SQL)
title: sp_trace_setstatus (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_setstatus_TSQL
- sp_trace_setstatus
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setstatus
ms.assetid: 29e7a7d7-b9c1-414a-968a-fc247769750d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 69090ce81bf7a70d28cf12959c412d88f270cfdd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209706"
---
# <a name="sp_trace_setstatus-transact-sql"></a>sp_trace_setstatus (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  修改指定跟踪的当前状态。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用扩展事件。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_trace_setstatus [ @traceid = ] trace_id , [ @status = ] status  
```  
  
## <a name="arguments"></a>参数  
`[ @traceid = ] trace_id` 要修改的跟踪的 ID。 *trace_id* 为 **int**，没有默认值。 用户使用此 *trace_id* 值标识、修改和控制跟踪。 有关检索 *trace_id* 的信息，请参阅 [sys.fn_trace_getinfo &#40;transact-sql&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)。  
  
`[ @status = ] status` 指定要在跟踪上实现的操作。 *status* 为 **int**，没有默认值。  
  
 下表列出了可以指定的状态。  
  
|状态|说明|  
|------------|-----------------|  
|**0**|停止指定的跟踪。|  
|**1**|启动指定的跟踪。|  
|**2**|关闭指定的跟踪并从服务器中删除其定义。|  
  
> [!NOTE]  
>  在关闭跟踪前首先必须先停止它。 在查看跟踪前首先必须先停止并关闭它。  
  
## <a name="return-code-values"></a>返回代码值  
 下表说明在存储过程完成后用户可能获得的代码值。  
  
|返回代码|描述|  
|-----------------|-----------------|  
|**0**|无错误。|  
|**1**|未知错误。|  
|**8**|指定的状态无效。|  
|**9**|指定的跟踪句柄无效。|  
|**13**|内存不足。 在没有足够内存执行指定的操作时返回此代码。|  
  
 如果跟踪已处于指定的状态， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 则将返回 **0**。  
  
## <a name="remarks"></a>备注  
 严格类型化)  (**sp_trace_xx** 的所有 SQL 跟踪存储过程的参数。 如果没有用正确的输入参数数据类型（参数说明中指定的类型）来调用这些参数，则存储过程将返回错误。  
  
 有关使用跟踪存储过程的示例，请参阅[创建跟踪 (Transact-SQL)](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 用户必须拥有 ALTER TRACE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.fn_trace_geteventinfo &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getfilterinfo (Transact-SQL)](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sp_trace_generateevent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [SQL 跟踪](../../relational-databases/sql-trace/sql-trace.md)  
  
  
