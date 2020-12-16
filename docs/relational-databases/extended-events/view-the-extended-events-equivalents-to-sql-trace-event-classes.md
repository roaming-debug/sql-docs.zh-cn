---
title: 与 SQL 跟踪事件类等效的扩展事件
description: 本文介绍如何查看与各 SQL 跟踪事件及其关联列等效的扩展事件操作和事件。
ms.date: 03/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
helpviewer_keywords:
- SQL Trace, extended events equivalents
- extended events [SQL Server], SQL Trace equivalents
- extended events [SQL Server], user configurable events
ms.assetid: 7f24104c-201d-4361-9759-f78a27936011
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0db57b62a6b6e8c2e9be53ce97fd445d7deee6ea
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481308"
---
# <a name="view-the-extended-events-equivalents-to-sql-trace-event-classes"></a>查看与 SQL 跟踪事件类等效的扩展事件

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/applies-to-version/sql-asdb.md)]

  如果您想要使用扩展事件来收集与 SQL 跟踪事件类和列等效的事件数据，则理解 SQL 跟踪事件映射到扩展事件的事件和操作的方式将很有用。  
  
 您可以使用以下过程查看与各 SQL 跟踪事件及其关联列等效的扩展事件的事件和操作。  
  
## <a name="to-view-the-extended-events-equivalents-to-sql-trace-events-using-query-editor"></a>使用查询编辑器查看与 SQL 跟踪事件等效的扩展事件

- 从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的查询编辑器中，运行以下查询：

   ```sql
   USE MASTER;
   GO
   SELECT DISTINCT
      tb.trace_event_id,
      te.name            AS 'Event Class',
      em.package_name    AS 'Package',
      em.xe_event_name   AS 'XEvent Name',
      tb.trace_column_id,
      tc.name            AS 'SQL Trace Column',
      am.xe_action_name  AS 'Extended Events action'
   FROM
                sys.trace_events         te
      LEFT JOIN sys.trace_xe_event_map   em ON te.trace_event_id  = em.trace_event_id
      LEFT JOIN sys.trace_event_bindings tb ON em.trace_event_id  = tb.trace_event_id
      LEFT JOIN sys.trace_columns        tc ON tb.trace_column_id = tc.trace_column_id
      LEFT JOIN sys.trace_xe_action_map  am ON tc.trace_column_id = am.trace_column_id
   ORDER BY te.name, tc.name
   ```

在查看结果时，请注意以下事项：  

- 如果除“事件类”列之外的所有列都返回 NULL，则表示该事件类不是从 SQL 跟踪迁移的。  
  
-   如果只有“扩展事件操作”列中的值为 NULL，则表示以下两个条件之一成立：  
  
    -   SQL 跟踪列映射到与扩展事件的事件相关联的数据字段之一。  
  
        > [!NOTE]  
        > 每个扩展事件的事件都具有在结果集中自动包括的数据字段的默认集合。  
  
    -   操作列不具有等效的有意义的扩展事件。 与此有关的一个示例就是 SQL 跟踪中的“事件类”列。 该列不是扩展事件中所必需的，因为事件名称起到相同作用。  
  
-   对于用户可配置 SQL 跟踪事件类（UserConfigurable:1 到 UserConfigurable:9），扩展事件使用单个事件来代替它们。 该事件名为 user_event。 该事件通过使用 sp_trace_generateevent 引发，与 SQL 跟踪所使用的相同的存储过程。 无论哪一事件 ID 传递到该存储过程，都会返回 user_event 事件。 但是，event_id 字段作为事件数据的一部分返回。 这使您可以生成基于事件 ID 的谓词。 例如，如果你在代码中使用 UserConfigurable:0（事件 ID = 82），则可以将 user_event 事件添加到会话，并且指定谓词“event_id = 82”。 因此，你不必更改代码，因为 sp_trace_generateevent 存储过程生成扩展事件 user_event 事件以及等效的 SQL 跟踪事件类。  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_generateevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)  
  
  
