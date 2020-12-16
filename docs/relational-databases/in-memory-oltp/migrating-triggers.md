---
title: 迁移触发器 | Microsoft Docs
description: 了解内存优化表和 DDL 触发器，这些触发器在 SQL Server 实例上针对 CREATE、ALTER、DROP、GRANT、DENY、REVOKE 或 UPDATE STATISTICS 触发。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ad5385c5-5a50-40ca-a319-97d5606b8511
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4147c69de6eb360820ed7bd47c7ee7c8c867d52b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465358"
---
# <a name="migrating-triggers"></a>迁移触发器
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题论述 DDL 触发器以及内存优化表。  
  
 DML 触发器在内存优化表中受支持，但仅适用于 FOR | AFTER 触发器事件。 有关示例，请参阅 [执行包含 FROM 或子查询的 UPDATE](../../relational-databases/in-memory-oltp/implementing-update-with-from-or-subqueries.md)。 
  
 LOGON 触发器是定义为对 LOGON 事件触发的触发器。 LOGON 触发器不会影响内存优化表。  
  
## <a name="ddl-triggers"></a>DDL 触发器  
 DDL 触发器定义为在对其定义的数据库或服务器上执行 CREATE、ALTER、DROP、GRANT、DENY、REVOKE 或 UPDATE STATISTICS 语句时触发的触发器。  
  
 如果数据库或服务器具有对 CREATE_TABLE 或者包含它的任何事件组定义的一个或多个 DDL 触发器，则不能创建内存优化表。 如果数据库或服务器具有对 DROP_TABLE 或者包含它的任何事件组定义的一个或多个 DDL 触发器，则不能删除内存优化表。  
  
 如果对 CREATE_PROCEDURE、DROP_PROCEDURE 或包含这些事件的任何事件组有一个或多个 DDL 触发器，则不能创建本机编译的存储过程。  
  
## <a name="see-also"></a>另请参阅  
 [迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
