---
description: sp_cycle_agent_errorlog (Transact-SQL)
title: sp_cycle_agent_errorlog (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cycle_agent_errorlog
- sp_cycle_agent_errorlog_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cycle_agent_errorlog
ms.assetid: 8aa96182-60b7-4d7b-b2a7-ccce70378c6e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c8f1a115aeab29a783fcb7aa4df2b2613052b5a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203187"
---
# <a name="sp_cycle_agent_errorlog-transact-sql"></a>sp_cycle_agent_errorlog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  关闭当前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理错误日志文件，并循环 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理错误日志扩展编号（就像重新启动服务器）。 新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理错误日志包含一个表示已创建新日志的行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cycle_agent_errorlog  
```  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 每次 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动代理时，当前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理错误日志都会重命名为 **SQLAgent**; **SQLAgent** 将变为 **SQLAgent**， **SQLAgent** 将变为 **SQLAgent**，依此类推。 **sp_cycle_agent_errorlog** 使你能够在不停止和启动服务器的情况下循环错误日志文件。  
  
 必须从 **msdb** 数据库运行此存储过程。  
  
## <a name="permissions"></a>权限  
 **Sp_cycle_agent_errorlog** 的执行权限仅限于 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例将循环 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理错误日志。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_cycle_agent_errorlog ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_cycle_errorlog &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cycle-errorlog-transact-sql.md)  
  
  
