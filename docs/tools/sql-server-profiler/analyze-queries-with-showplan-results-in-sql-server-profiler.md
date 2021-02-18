---
title: 使用 SHOWPLAN 结果来分析查询
titleSuffix: SQL Server Profiler
description: 了解如何从跟踪中提取显示计划事件，以便 SQL Server Profiler 显示查询计划信息。 查看显示计划跟踪事件表。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6a2f7727-141c-4f59-8613-2e452bc78467
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 31771ddc584d72dee2b3c859b349dd5752e4d4d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349451"
---
# <a name="analyze-queries-with-showplan-results-in-sql-server-profiler"></a>在 SQL Server Profiler 中使用 SHOWPLAN 结果来分析查询

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  可以将导致 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 在跟踪中收集和显示查询计划信息的 Showplan 事件类添加到跟踪定义。 也可能从跟踪中收集的其他事件中提取显示计划事件，并将这些显示计划事件保存在单独的 XML 文件中。  
  
 可以用下列一种方法从跟踪中提取显示计划事件：  
  
-   在配置跟踪时，使用 **“事件提取设置”** 选项卡。注意，此选项卡只有在选择了 **“事件选择”** 选项卡上的一个显示计划事件后才会显示。  
  
-   使用 **“文件”** 菜单上的 **“提取 SQL Server 事件”** 选项。  
  
-   通过右键单击特定事件并选择“提取事件数据”，提取并保存单个事件。  
  
## <a name="showplan-events"></a>显示计划事件  
 下表中列出并说明了显示计划跟踪事件。  
  
|事件名称|说明|  
|----------------|-----------------|  
|**Performance statistics**|指明编写的显示计划第一次保存到缓存中、何时对其重新编写以及何时将其从计划缓存中删除。 **TextData** 列包含 XML 格式的显示计划。 有关详细信息，请参阅 [Performance Statistics 事件类](../../relational-databases/event-classes/performance-statistics-event-class.md)。|  
|**Showplan All**|显示查询计划，列出已执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的完整编写时详细信息。 例如，可能显示估计开销值和列列表。 有关详细信息，请参阅 [Showplan All Event Class](../../relational-databases/event-classes/showplan-all-event-class.md)。|  
|**Showplan All For Query Compile**|当查询在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]上编译或重新编译时发生。 它是 **Showplan All** 事件的编译时对等事件。 **Showplan All** 在执行查询时发生。 编写查询时发生 **Showplan All For Query Compile** 。 有关详细信息，请参阅 [Showplan All for Query Compile Event Class](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md)。|  
|**Showplan Statistics Profile**|显示查询计划，列出正在执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的完整运行时详细信息（包括通过每个操作传递的实际行数）。 有关详细信息，请参阅 [Showplan Statistics Profile Event Class](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md)。|  
|**Showplan Text**|将正在执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的查询计划树显示为二进制数据。 有关详细信息，请参阅 [Showplan Text Event Class](../../relational-databases/event-classes/showplan-text-event-class.md)。|  
|**Showplan Text (Unencoded)**|将正在执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的查询计划树显示为文本。 除了此事件类显示文本而不是二进制数据以外，此事件类显示的信息与 Showplan Text 相同。 有关详细信息，请参阅 [Showplan Text (Unencoded) 事件类](../../relational-databases/event-classes/showplan-text-unencoded-event-class.md)。|  
|**Showplan XML**|显示查询计划，列出在查询优化期间收集的完整数据。 仅当优化查询计划时才生成此事件。 有关详细信息，请参阅 [Showplan XML Event Class](../../relational-databases/event-classes/showplan-xml-event-class.md)。|  
|**Showplan XML For Query Compile**|编写完查询后，显示查询计划。 有关详细信息，请参阅 [Showplan XML for Query Compile Event Class](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md)。|  
|**Showplan XML Statistics Profile**|显示查询计划，列出 XML 格式的完整运行时详细信息。 例如，此事件类捕获通过已执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的每个运算符的行数。 有关详细信息，请参阅 [Showplan XML Statistics Profile Event Class](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md)。|  
  
## <a name="see-also"></a>另请参阅  
 [“性能”事件类别](../../relational-databases/event-classes/performance-event-category.md)  
  
  
