---
title: 启动跟踪
titleSuffix: SQL Server Profiler
description: 了解如何在 SQL Server Profiler 中定义新跟踪或创建模板之后，启动跟踪并查找其数据。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: aeeb38eb-229a-4c8b-ad66-57e9ce45fb6a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: df7bc3602d03cf6467a022f060db6b5a63f1c747
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341990"
---
# <a name="start-a-trace"></a>启动跟踪

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]定义新跟踪或创建模板之后，可以使用新的跟踪定义或模板来启动、暂停或停止捕获数据。  
  
## <a name="starting-a-trace"></a>启动跟踪

当启动跟踪并且已定义的源为 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的实例时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将创建一个队列，为捕获的服务器事件提供临时存放位置。  
  
使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 访问 SQL 跟踪时，启动跟踪将打开一个新的跟踪窗口（如果没有窗口打开），并立即捕获数据。  
  
使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 系统存储过程访问 SQL 跟踪时，每次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例都必须启动跟踪，以便捕获数据。 启动跟踪后，只能修改跟踪的名称。  
  
> [!NOTE]  
>  在使用现有跟踪时，可以查看属性，但是不能修改属性。 若要修改属性，请停止或暂停跟踪。  
  
## <a name="see-also"></a>另请参阅

[连接到服务器后自动启动跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/start-a-trace-automatically-after-connecting-to-a-server-sql-server-profiler.md)   

[暂停跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/pause-a-trace-sql-server-profiler.md)   

[停止跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/stop-a-trace-sql-server-profiler.md)   

[清除跟踪窗口 (SQL Server Profiler)](../../tools/sql-server-profiler/clear-a-trace-window-sql-server-profiler.md)   

[在跟踪暂停或停止之后运行跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/run-a-trace-after-it-has-been-paused-or-stopped-sql-server-profiler.md)