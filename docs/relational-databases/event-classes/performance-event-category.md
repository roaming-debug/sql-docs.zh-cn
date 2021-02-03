---
description: Performance 事件类别
title: “性能”事件类别 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL Server event classes, Performance event category
- Performance event category [SQL Server]
- event classes [SQL Server], Performance event category
ms.assetid: 708f3585-d8be-4980-bbff-672d7c59397e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 10e44ce46f927495abadeb03a40607e9d5e679fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165434"
---
# <a name="performance-event-category"></a>Performance 事件类别
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  使用 Performance 事件类别可以监视 Showplan 事件类以及在执行 SQL 数据操作语言 (DML) 运算符时生成的事件类。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|说明|  
|-----------|-----------------|  
|[Auto Stats 事件类](../../relational-databases/event-classes/auto-stats-event-class.md)|指示已自动更新索引统计信息和列统计信息。|  
|[Degree of Parallelism (7.0 Insert) 事件类](../../relational-databases/event-classes/degree-of-parallelism-7-0-insert-event-class.md)|指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已经使用串行或并行计划执行 SELECT、INSERT、UPDATE 或 DELETE 语句。 此外，还会报告用来执行该操作的 CPU 数。|  
|[Performance Statistics 事件类](../../relational-databases/event-classes/performance-statistics-event-class.md)|监视正在执行的查询的性能。|  
|[Showplan All 事件类](../../relational-databases/event-classes/showplan-all-event-class.md)|标识 SQL 语句中的 **Showplan** 运算符。|  
|[Showplan All for Query Compile 事件类](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md)|显示 **Showplan** 运算符的编写时数据。|  
|[Showplan Statistics Profile 事件类](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md)|显示查询的估计开销。|  
|[Showplan XML 事件类](../../relational-databases/event-classes/showplan-xml-event-class.md)|标识 SQL 语句中的 **Showplan** 运算符。 该事件类将每个事件作为定义好的 XML 文档进行存储。|  
|[Showplan XML For Query Compile 事件类](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md)|以 XML 格式显示 **Showplan** 运算符的编写时数据。|  
|[Showplan XML Statistics Profile 事件类](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md)|标识与 SQL 语句关联的 **Showplan** 运算符。 将输出一个 XML 文档。|  
|[SQL:FullTextQuery 事件类](../../relational-databases/event-classes/sql-fulltextquery-event-class.md)|指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已执行全文查询。|  
|[Plan Guide Successful 事件类](../../relational-databases/event-classes/plan-guide-successful-event-class.md)|指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已成功为计划指南中包含的查询或批处理生成执行计划。|  
|[Plan Guide Unsuccessful 事件类](../../relational-databases/event-classes/plan-guide-unsuccessful-event-class.md)|指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法为计划指南中包含的查询或批处理生成执行计划。|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)  
  
  
