---
description: Transactions 事件类别
title: “事务”事件类别 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL Server event classes, Transactions event category
- event classes [SQL Server], Transactions event category
- Transactions event category [SQL Server]
ms.assetid: bfc75c5b-7115-49d8-9148-a0c84ee66a9a
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: be136efcc9f46af5a3b1f25b6e42033d8c0d08e0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188076"
---
# <a name="transactions-event-category"></a>Transactions 事件类别
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Transactions 事件类可用于监视事务的状态。 带有 **TM:** 前缀的事件类名称可用于跟踪通过事务管理界面发送的、与事务相关的操作。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|说明|  
|-----------|-----------------|  
|[DTCTransaction 事件类](../../relational-databases/event-classes/dtctransaction-event-class.md)|跟踪由 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 协调的事务。 这些是在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的两个或两个以上的数据库或实例之间分布的事务。|  
|[SQLTransaction 事件类](../../relational-databases/event-classes/sqltransaction-event-class.md)|跟踪 [!INCLUDE[tsql](../../includes/tsql-md.md)] BEGIN TRAN、COMMIT TRAN、SAVE TRAN 和 ROLLBACK TRAN 语句。|  
|[TM：Begin Tran Completed 事件类](../../relational-databases/event-classes/tm-begin-tran-completed-event-class.md)|指明已完成 BEGIN TRANSACTION 请求。|  
|[TM：Begin Tran Starting 事件类](../../relational-databases/event-classes/tm-begin-tran-starting-event-class.md)|指明正在启动 BEGIN TRANSACTION 请求。|  
|[TM：Commit Tran Completed 事件类](../../relational-databases/event-classes/tm-commit-tran-completed-event-class.md)|指明已完成 COMMIT TRANSACTION 请求。|  
|[TM：Commit Tran Starting 事件类](../../relational-databases/event-classes/tm-commit-tran-starting-event-class.md)|指明正在启动 COMMIT TRANSACTION 请求。|  
|[TM：Promote Tran Completed 事件类](../../relational-databases/event-classes/tm-promote-tran-completed-event-class.md)|指明已完成 PROMOTE TRANSACTION 请求。|  
|[TM：Promote Tran Starting 事件类](../../relational-databases/event-classes/tm-promote-tran-starting-event-class.md)|指明正在启动 PROMOTE TRANSACTION 请求。|  
|[TM：Rollback Tran Completed 事件类](../../relational-databases/event-classes/tm-rollback-tran-completed-event-class.md)|指明已完成 ROLLBACK TRANSACTION 请求。|  
|[TM：Rollback Tran Starting 事件类](../../relational-databases/event-classes/tm-rollback-tran-starting-event-class.md)|指明正在启动 ROLLBACK TRANSACTION 请求。|  
|[TM：Save Tran Completed 事件类](../../relational-databases/event-classes/tm-save-tran-completed-event-class.md)|指明已完成 SAVE TRANSACTION 请求。|  
|[TM：Save Tran Starting 事件类](../../relational-databases/event-classes/tm-save-tran-starting-event-class.md)|指明正在启动 SAVE TRANSACTION 请求。|  
|[TransactionLog 事件类](../../relational-databases/event-classes/transactionlog-event-class.md)|跟踪事务何时写入数据库事务日志。|  
  
  
