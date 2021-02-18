---
title: 从跟踪提取脚本
titleSuffix: SQL Server Profiler
description: 了解如何从 SQL Server Profiler 中的跟踪文件或表中提取 Transact-SQL 事件，并将其另存为 Transact-SQL 脚本文件。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 431126a6-ff91-4818-8763-4442152bd571
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: c42ee61b5a97f7634104cdda579d522501d77c96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345890"
---
# <a name="extract-a-script-from-a-trace-sql-server-profiler"></a>从跟踪提取脚本 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题说明如何使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 从跟踪文件或表提取 [!INCLUDE[tsql](../../includes/tsql-md.md)] 事件并将它们保存为 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]脚本文件。  
  
### <a name="to-extract-a-transact-sql-script-from-a-trace-file-or-table"></a>从跟踪文件或表提取 Transact-SQL 脚本  
  
1.  打开一个包含要将其保存到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本文件的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 事件的跟踪文件或表。 有关详细信息，请参阅 [打开跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) 或 [打开跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)一起提供的预定义优化模板。  
  
2.  在“文件”菜单上，指向“导出”，再指向“提取 SQL Server 事件”，然后单击“提取 Transact-SQL 事件”。  
  
3.  在 **“另存为”** 对话框中，键入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 文件的名称，然后单击 **“保存”** 。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
