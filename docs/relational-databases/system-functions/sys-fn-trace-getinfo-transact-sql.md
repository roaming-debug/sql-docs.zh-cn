---
description: sys.fn_trace_getinfo (Transact-SQL)
title: sys.fn_trace_getinfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_trace_getinfo
- fn_trace_getinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- traces [SQL Server], status information
- status information [SQL Server], traces
- sys.fn_trace_getinfo function
- fn_trace_getinfo function
ms.assetid: 04b140fe-110a-47b8-98b5-e4c161beb6c9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c7426552dd52732b6b22f9947862b8607418f482
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093805"
---
# <a name="sysfn_trace_getinfo-transact-sql"></a>sys.fn_trace_getinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关指定跟踪或所有现有跟踪的信息。  
  
> **重要说明！** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用扩展事件。    
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_trace_getinfo ( { trace_id | NULL | 0 | DEFAULT } )  
```  
  
## <a name="arguments"></a>参数  
 *trace_id*  
 跟踪的 ID。 *trace_id* 是 **int**。 有效输入是跟踪的 ID 号、NULL、0或 DEFAULT。 在此上下文中，NULL、0 和 DEFAULT 是等效值。 指定 NULL、0 或 DEFAULT 可返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中所有跟踪的信息。  
  
## <a name="tables-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|traceid|**int**|跟踪的 ID。|  
|property|**int**|跟踪的属性：<br /><br /> 1= 跟踪选项。 有关详细信息，请 @options 参阅 [&#40;transact-sql&#41;sp_trace_create ](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)。<br /><br /> 2 = 文件名<br /><br /> 3 = 最大大小<br /><br /> 4 = 停止时间<br /><br /> 5 = 当前跟踪状态。 0 = 停止。 1 = 正在运行。|  
|值|**sql_variant**|有关指定跟踪的属性的信息。|  
  
## <a name="remarks"></a>备注  
 当传递特定跟踪的 ID 时，fn_trace_getinfo 将返回有关该跟踪的信息。 传递无效 ID 时，此函数将返回空行集。  
  
 fn_trace_getinfo 会将 .trc 扩展名附加到其结果集中包含的所有跟踪文件名。 有关定义跟踪的信息，请参阅 [&#40;transact-sql&#41;sp_trace_create ](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)。 有关跟踪筛选器的类似信息，请参阅 [&#40;transact-sql&#41;sys.fn_trace_getfilterinfo ](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)。  
  
 有关使用跟踪存储过程的完整示例，请参阅 [Create a trace &#40;transact-sql&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 要求对服务器具有 ALTER TRACE 权限。  
  
## <a name="examples"></a>示例  
 以下示例返回有关所有活动跟踪的信息。  
  
```  
SELECT * FROM sys.fn_trace_getinfo(0) ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [创建跟踪 (Transact-SQL)](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_create (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_getfilterinfo (Transact-SQL)](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sys.fn_trace_geteventinfo &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)  
  
  
