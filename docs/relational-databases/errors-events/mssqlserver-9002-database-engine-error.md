---
description: MSSQLSERVER_9002
title: MSSQLSERVER_9002 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9002 (Database Engine error)
ms.assetid: 2e50841f-2b99-45f4-aec5-aa4add70cbeb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6e46db0dadfa36155ac36670a13a0cc621fa74b2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161985"
---
# <a name="mssqlserver_9002"></a>MSSQLSERVER_9002
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|9002|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LOG_IS_FULL|  
|消息正文|数据库 '%.*ls' 的事务日志已满。 若要查明无法重用日志中的空间的原因，请参阅 sys.databases 中的 log_reuse_wait_desc 列。|  
  
## <a name="explanation"></a>说明  
数据库日志空间不足。 **sys.databases** 中的 **log_reuse_wait_desc** 列说明无法重用日志中的空间的原因。  
  
## <a name="user-action"></a>用户操作  
使用 **sys.databases** 确定日志已满的原因，然后更正该情况。 有关详细信息，请参阅 Server SQL 联机丛书中的“解决事务日志已满的问题（错误 9002）”。  
  
## <a name="see-also"></a>另请参阅  
[解决事务日志已满的问题（SQL Server 错误 9002）](~/relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
[sys.databases (Transact-SQL)](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
