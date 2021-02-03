---
description: MSSQLSERVER_41368
title: MSSQLSERVER_41368 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41368 (Database Engine error)
ms.assetid: abc71559-4c4d-4cce-a08f-3299dd167842
author: MashaMSFT
ms.author: mathoma
robots: noindex,nofollow
ms.openlocfilehash: 97fec0c4cb8f24fe7e11b0399fa60289d68b7b07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179500"
---
# <a name="mssqlserver_41368"></a>MSSQLSERVER_41368
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|41368|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SQL_IMPLICIT_AND_EXPLICIT_TX_NOT_SUPPORTED|  
|消息正文|只支持对自动提交事务使用 READ COMMITTED 隔离级别访问内存优化表。 显式或隐式事务不支持此隔离级别。 使用表提示（例如 WITH (SNAPSHOT)）为内存优化表提供一种支持的隔离级别。|  
  
## <a name="explanation"></a>说明  
只支持对自动提交事务使用 READ COMMITTED 隔离级别访问内存优化表。 有关详细信息，请参阅[内存中表和过程的事务](~/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)。  
  
当从使用 BEGIN TRANSACTION 启动的显式事务或从隐式事务访问内存优化表时，如果将 IMPLICIT_TRANSACTIONS 设置为 ON，则不支持 READ COMMITTED 隔离级别。  
  
## <a name="user-action"></a>用户操作  
从显式或隐式 READ COMMITTED 事务访问内存优化表时，使用 SNAPSHOT 来访问该表。 这可以通过使用表提示 WITH (SNAPSHOT)（有关详细信息，请参阅[内存中表和过程的事务](~/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)）或通过将数据库选项 MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT 设置为 ON（有关详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](~/t-sql/statements/alter-database-transact-sql.md)）来实现。  
  
## <a name="see-also"></a>另请参阅  
[内存中 OLTP（内存中优化）](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
