---
description: 获取有关 DDL 触发器的信息
title: 获取有关 DDL 触发器的信息 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- metadata [SQL Server], triggers
- status information [SQL Server], DDL triggers
- DDL triggers, metadata
ms.assetid: 462becea-292a-4b9e-bb98-533e89733911
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 676baf26aa98a240503465bf5060a37e46a1d1a2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466698"
---
# <a name="get-information-about-ddl-triggers"></a>获取有关 DDL 触发器的信息
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本节列出的目录视图可用于获取有关 DDL 触发器的信息。  
  
 **获取有关 DDL 触发器可触发的事件或事件组的信息。**  
  
-   [sys.trigger_event_types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-trigger-event-types-transact-sql.md)  
  
 **查看触发器的依赖关系**  
  
-   [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
-   [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)  
  
-   [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
## <a name="database-scoped-ddl-triggers"></a>数据库范围内的 DDL 触发器  
 **获取有关数据库范围内的触发器的信息**  
  
-   [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
 **获取有关激发触发器的数据库事件的信息**  
  
-   [sys.trigger_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)  
  
 **查看数据库范围内的触发器的定义**  
  
-   [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)  
  
 **获取有关 CLR 数据库范围内的触发器的信息**  
  
-   [sys.assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)  
  
## <a name="server-scoped-ddl-triggers"></a>服务器范围内的 DDL 触发器  
 **获取有关服务器范围内的触发器的信息**  
  
-   [sys.server_triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)  
  
 **获取有关激发触发器的服务器事件的信息**  
  
-   [sys.server_trigger_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)  
  
 **查看服务器范围内的触发器的定义**  
  
-   [sys.server_sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)  
  
 **获取有关 CLR 服务器范围内的触发器的信息**  
  
-   [sys.server_assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [DDL 触发器](../../relational-databases/triggers/ddl-triggers.md)  
  
  
