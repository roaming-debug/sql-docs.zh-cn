---
description: sp_monitor (Transact-SQL)
title: sp_monitor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_monitor_TSQL
- sp_monitor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_monitor
ms.assetid: cb628496-2f9b-40e4-b018-d0831c4cb018
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f046c3e6fe81c7dac489fcdd88237ecb2d3747d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195470"
---
# <a name="sp_monitor-transact-sql"></a>sp_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示有关的统计信息 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_monitor  
```  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|说明|  
|-----------------|-----------------|  
|**last_run**|上次运行时间 **sp_monitor** 。|  
|**current_run**|正在运行的时间 **sp_monitor** 。|  
|**seconds**|自运行 **sp_monitor** 以来经过的秒数。|  
|**cpu_busy**|服务器计算机的 CPU 处理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工作所用的秒数。|  
|**io_busy**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在输入和输出操作上花费的秒数。|  
|**时间**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已空闲的秒数。|  
|**packets_received**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 读取的输入数据包数。|  
|**packets_sent**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已写入的输出数据包数。|  
|**packet_errors**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在读取和写入数据包时遇到的错误数。|  
|**total_read**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 读取的次数。|  
|**total_write**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 写入的次数。|  
|**total_errors**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在读取和写入时遇到的错误数。|  
|**连接**|登录或尝试登录 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的次数。|  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过一系列函数跟踪其完成的工作量。 执行 **sp_monitor** 显示这些函数返回的当前值，并显示自上次运行该过程以来它们的更改量。  
  
 对于每一列，统计信息将以 *数字* 形式显示 (*number*) 号 *% 或* *number* (*number*) 。 第一个 *数字* 是指在重新启动后，为 **cpu_busy**、 **io_busy** 和 **空闲**)  (的秒数，或其他变量 (总数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 括号中的 *数字* 是指自上次运行 **sp_monitor** 以来的秒数或总数。 百分比是自上次运行 **sp_monitor** 以来的时间百分比。 例如，如果报表显示 **cpu_busy** 为 4250 (215) -68%，则自上次启动以来，cpu 已忙4250秒 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，自上次运行 **sp_monitor** 以来的215秒，以及自上次运行 **sp_monitor** 以来的总时间的68百分比。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例报告有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 繁忙程度的信息。  
  
```console
USE master  
EXEC sp_monitor  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```console
last_run       current_run                   seconds
-----------    --------------------------    ---------
Mar 29 1998    11:55AM Apr 4 1998 2:22 PM    561

cpu_busy           io_busy     idle
---------------    ---------   --------------
190(0)-0%          187(0)-0%   148(556)-99%

packets_received       packets_sent    packet_errors
----------------       ------------    -------------
16(1)                  20(2)           0(0)

total_read     total_write   total_errors    connections
-----------    -----------   -------------   -----------
141(0)         54920(127)    0(0)            4(0)
```
  
## <a name="see-also"></a>另请参阅  
 [sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
