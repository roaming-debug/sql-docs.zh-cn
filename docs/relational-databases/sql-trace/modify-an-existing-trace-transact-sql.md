---
description: 修改现有跟踪 (Transact-SQL)
title: 修改现有跟踪 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- traces [SQL Server], modifying
- modifying traces
ms.assetid: 8792b43f-2510-44e3-9239-e73ad8227b89
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d6f706093032867835830d0cf98afb86c07e93f7
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88455345"
---
# <a name="modify-an-existing-trace-transact-sql"></a>修改现有跟踪 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍了如何使用存储过程修改现有跟踪。  
  
### <a name="to-modify-an-existing-trace"></a>修改现有跟踪  
  
1.  如果跟踪已在运行，请在执行 **sp_trace_setstatus** 时通过指定 **@status = 0** 停止跟踪。  
  
2.  若要修改跟踪事件，请执行 **sp_trace_setevent** ，并通过参数指定更改。 下面按顺序列出了参数：  

    -   **\@traceid** （跟踪 ID）  
  
    -   **\@eventid**（事件 ID）  
  
    -   **\@columnid**（列 ID）  
  
    -   **\@on**（启用）  
  
     修改“\@on”参数时，请记住它与“\@columnid”参数是相互作用的 ：  
  
    |ON|列 ID|结果|  
    |--------|---------------|------------|  
    |ON (**1**)|Null|事件打开， 所有列被清除。|  
    ||NOT NULL|指定事件的列打开。|  
    |OFF (**0**)|Null|事件关闭， 所有列被清除。|  
    ||NOT NULL|指定事件的列关闭。|  
  
> [!IMPORTANT]
>  与常规的存储过程不同，所有 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 存储过程 (<strong>sp_trace_xx</strong>) 参数的类型都受到严格限制，不支持自动的数据类型转换。 如果这些参数不是使用正确的输入参数数据类型（正如参数说明中指定的一样）调用的，则存储过程会返回错误。  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setstatus (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [SQL Server Profiler 存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)  
  
  
