---
title: XEvents 相关系统对象
description: 这些资源与扩展事件相关，包括系统对象如何支持它们、SQL Server 如何使用它们以及 Azure SQL 数据库的特定方面。
ms.date: 03/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jukoesma
ms.technology: xevents
ms.topic: reference
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ef31edf367eacd41135bb4db9a58cd8ba4950de8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346723"
---
# <a name="system-objects-that-support-extended-events"></a>支持扩展事件的系统对象

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

目前，本文提供与扩展事件相关的其他文章的链接。 这些文章说明了以下内容：

- 支持扩展事件功能的系统对象。
- SQL Server 本身使用扩展事件的那些部分。
- 特定于云中 Azure SQL 数据库的扩展事件的特性。

此列表内容不一定全面。

## <a name="system-tables"></a>系统表

- [扩展事件表 - trace_xe_action_map](../system-tables/extended-events-tables-trace-xe-action-map.md)

- [扩展事件表 - trace_xe_event_map](../system-tables/extended-events-tables-trace-xe-event-map.md)

## <a name="system-catalog-views"></a>系统目录视图

- [扩展事件目录视图 (Transact-SQL)](../system-catalog-views/extended-events-catalog-views-transact-sql.md)

- [sys.server_event_sessions (Transact-SQL)](../system-catalog-views/sys-server-event-sessions-transact-sql.md)

- [sys.server_event_session_actions (Transact-SQL)](../system-catalog-views/sys-server-event-session-actions-transact-sql.md)

- [sys.server_event_session_events (Transact-SQL)](../system-catalog-views/sys-server-event-session-events-transact-sql.md)

- [sys.server_event_session_fields (Transact-SQL)](../system-catalog-views/sys-server-event-session-fields-transact-sql.md)

- [sys.server_event_session_targets (Transact-SQL)](../system-catalog-views/sys-server-event-session-targets-transact-sql.md)

## <a name="other-system-objects"></a>其他系统对象

- [扩展事件动态管理视图](../system-dynamic-management-views/extended-events-dynamic-management-views.md)

## <a name="uses-of-extended-events-by-sql-server-itself"></a>SQL Server 自行使用扩展事件

此列表内容可能不全面。

- [访问扩展事件日志中的诊断信息](../native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)

- [配置 AlwaysOn 可用性组扩展事件](../../database-engine/availability-groups/windows/always-on-extended-events.md)

- [Stretch Database 扩展事件](../../sql-server/stretch-database/extended-events-for-stretch-database.md)

## <a name="azure-sql-database-and-extended-events"></a>Azure SQL 数据库和扩展事件

- [Azure SQL 数据库中的扩展事件](/azure/sql-database/sql-database-xevent-db-diff-from-svr)

- [sys.database_event_session_targets（Azure SQL 数据库）](../system-catalog-views/sys-database-event-session-targets-azure-sql-database.md)

- [sys.database_event_session_fields（Azure SQL 数据库）](../system-catalog-views/sys-database-event-session-fields-azure-sql-database.md)

- [sys.database_event_session_events（Azure SQL 数据库）](../system-catalog-views/sys-database-event-session-events-azure-sql-database.md)

- [sys.database_event_session_actions（Azure SQL 数据库）](../system-catalog-views/sys-database-event-session-actions-azure-sql-database.md)
