---
description: sp_cycle_errorlog (Transact-SQL)
title: sp_cycle_errorlog (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cycle_errorlog_TSQL
- sp_cycle_errorlog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cycle_errorlog
ms.assetid: 61a12cbf-78a3-4052-8604-3b29d07573fd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b75b9e0c056a86a8ad67d1a739214a46f88533f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203164"
---
# <a name="sp_cycle_errorlog-transact-sql"></a>sp_cycle_errorlog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  关闭当前的错误日志文件，并循环错误日志扩展编号（就像重新启动服务器）。 新错误日志包含版本和版权信息，以及表明新日志已创建的一行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cycle_errorlog  
```  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 每次 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动时，当前错误日志都会重命名为 **错误日志。 1**; **错误日志** 将变成 **错误日志。 2**， **错误日志 2** 变成 **错误日志。 3**，依此类推。 **sp_cycle_errorlog** 使你能够在不停止和启动服务器的情况下循环错误日志文件。  
  
## <a name="permissions"></a>权限  
 **Sp_cycle_errorlog** 的执行权限仅限于 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例将循环 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志。  
  
```  
EXEC sp_cycle_errorlog ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_cycle_agent_errorlog &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cycle-agent-errorlog-transact-sql.md)  
  
  
