---
description: SHUTDOWN (Transact-SQL)
title: SHUTDOWN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHUTDOWN_TSQL
- SHUTDOWN
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server, stopping
- shutting down SQL Server
- SHUTDOWN statement
- stopping SQL Server
- immediately stopping SQL Server
ms.assetid: c8b03ff9-688c-4fe8-86e8-bd6bd401c9a4
author: rothja
ms.author: jroth
ms.openlocfilehash: 0b2d327df39cb0c3a891fc5f4bc624619e7964e8
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195449"
---
# <a name="shutdown-transact-sql"></a>SHUTDOWN (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  立即停止 SQL Server。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SHUTDOWN [ WITH NOWAIT ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 WITH NOWAIT  
 可选。 在不对每个数据库执行检查点操作的情况下关闭 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在尝试终止全部用户进程后退出。 服务器重新启动时，将针对未完成事务执行回滚操作。  
  
## <a name="remarks"></a>备注  
 除非使用 WITHNOWAIT 选项，否则 SHUTDOWN 通过下列操作关闭 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]：  
  
1.  禁用登录（sysadmin 和 serveradmin 固定服务器角色成员除外）。  
  
    > [!NOTE]  
    >  若要显示当前所有用户的列表，请运行 sp_who。  
  
2.  等待当前正在运行的 Transact-SQL 语句或存储过程完成。 若要显示所有活动进程和锁的列表，请分别执行 sp_who 和 sp_lock。  
  
3.  在每个数据库中插入检查点。  
  
 当 sysadmin 固定服务器角色成员重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，使用 SHUTDOWN 语句可以将所需的自动恢复工作量减到最少。  
  
 还可以使用其他工具和方法停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 每个工具或方法都在所有数据库内执行检查点。 您可以从数据缓存中刷新已提交的数据，然后停止服务器：  
  
-   通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器。  
  
-   通过在命令提示符处针对默认实例运行 net stop mssqlserver，或在命令提示符处针对命名实例运行 net stop mssql$ instancename。  
  
-   使用“控制面板”中的“服务”应用程序。  
  
 如果是从命令提示符下启动 sqlservr.exe，按 Ctrl+C 可以关闭 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 然而，按 Ctrl+C 键将不插入检查点。  
  
> [!NOTE]  
>  使用上述任何方法停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 都会向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发送 `SERVICE_CONTROL_STOP` 消息。  
  
## <a name="permissions"></a>权限  
 SHUTDOWN 权限分配给 sysadmin 和 serveradmin 固定服务器角色的成员，且不可转让。  
  
## <a name="see-also"></a>另请参阅  
 [检查点 (Transact-SQL)](../../t-sql/language-elements/checkpoint-transact-sql.md)   
 [sp_lock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [sp_who (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [sqlservr 应用程序](../../tools/sqlservr-application.md)   
 [启动、停止、暂停、继续、重新启动数据库引擎、SQL Server 代理或 SQL Server Browser 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
  
