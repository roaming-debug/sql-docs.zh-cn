---
description: TSQL 事件类别
title: TSQL 事件类别 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- SQL Server event classes, TSQL event category
- TSQL event category [SQL Server]
- event classes [SQL Server], TSQL event category
ms.assetid: 215f8747-64b5-4bf3-9845-d476b10cda3a
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c8c162dcc6321c158363edc2dd35a34a9e8430d4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483439"
---
# <a name="tsql-event-category"></a>TSQL 事件类别
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  **TSQL** 事件类别包含一般的 TSQL 事件。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|说明|  
|-----------|-----------------|  
|[Exec Prepared SQL 事件类](../../relational-databases/event-classes/exec-prepared-sql-event-class.md)|指示 SqlClient、ODBC、OLE DB 或 DB-Library 执行了已准备好的一条或多条 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|[Prepare SQL 事件类](../../relational-databases/event-classes/prepare-sql-event-class.md)|指示 SqlClient、ODBC、OLE DB 或 DB-Library 已准备好了一条或多条要使用的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|[SQL:BatchCompleted 事件类](../../relational-databases/event-classes/sql-batchcompleted-event-class.md)|指示已完成 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理。|  
|[SQL:BatchStarting 事件类](../../relational-databases/event-classes/sql-batchstarting-event-class.md)|指示正在启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理。|  
|[SQL:StmtCompleted 事件类](../../relational-databases/event-classes/sql-stmtcompleted-event-class.md)|指示已完成 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|[SQL:StmtRecompile 事件类](../../relational-databases/event-classes/sql-stmtrecompile-event-class.md)|指示由下列所有类型的批处理引起的语句级重新编译：存储过程、触发器、即席批查询和查询。|  
|[SQL:StmtStarting 事件类](../../relational-databases/event-classes/sql-stmtstarting-event-class.md)|指示正在启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|[Unprepare SQL 事件类](../../relational-databases/event-classes/unprepare-sql-event-class.md)|指示 SqlClient、ODBC、OLE DB 或 DB-Library 删除了已准备好的一条或多条 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。|  
|[XQuery Static Type 事件类](../../relational-databases/event-classes/xquery-static-type-event-class.md)|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行 XQuery 表达式时出现。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-SQL 参考（数据库引擎）](../../t-sql/language-reference.md)  
  
