---
description: KILL STATS JOB (Transact-SQL)
title: KILL STATS JOB (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- KILL STATS JOB
- KILL_STATS_JOB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending statistics update jobs [SQL Server]
- stopping statistics update jobs
- terminating statistics update jobs
- asynchronous statistics updates [SQL Server]
- KILL STATS JOB statement
- statistics update jobs [SQL Server]
ms.assetid: 55a8f9f1-3259-45c0-8ab9-60b9c088b4b4
author: cawrites
ms.author: chadam
ms.openlocfilehash: d52e417bef5c998d381ee9720d626d39430d8fee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99113123"
---
# <a name="kill-stats-job-transact-sql"></a>KILL STATS JOB (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  终止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的异步统计信息更新作业。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
KILL STATS JOB job_id   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *job_id*  
 job_id 字段，由该作业的 sys.dm_exec_background_job_queue 动态管理视图返回。  
  
## <a name="remarks"></a>备注  
 job_id 与在其他形式的 KILL 语句中所使用的 session_id 或工作单元无关。  
  
## <a name="permissions"></a>权限  
 若要从 sys.dm_exec_background_job_queue 动态管理视图访问信息，用户必须具有 VIEW SERVER STATE 权限。  
  
 默认情况下，sysadmin 和 processadmin 固定数据库角色的成员具有 KILL STATS JOB 权限，并且该权限不可转移。  
  
## <a name="examples"></a>示例  
 以下示例演示如何终止与 *job_id* = `53` 的作业关联的统计信息更新。  
  
```sql  
KILL STATS JOB 53;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [KILL (Transact-SQL)](../../t-sql/language-elements/kill-transact-sql.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION (Transact-SQL)](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
 [sys.dm_exec_background_job_queue (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md)   
 [统计信息](../../relational-databases/statistics/statistics.md)  
  
  
