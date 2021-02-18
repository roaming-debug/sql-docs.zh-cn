---
title: 指定跟踪文件的事件和数据列
titleSuffix: SQL Server Profiler
description: 了解如何指定 SQL Server Profiler 捕获跟踪中的事件数据时所包括的事件类和数据列。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 7da715a3-2f03-4063-b6a4-20fd7b44e675
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 660917a46529e62a33565c75ca92501ddfb8a140
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345738"
---
# <a name="specify-events-and-data-columns-for-a-trace-file-sql-server-profiler"></a>指定跟踪文件的事件和数据列 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍了如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]指定跟踪的事件类和数据列。  
  
### <a name="to-specify-events-and-data-columns-for-a-trace"></a>指定跟踪的事件和数据列  
  
1.  在 **“跟踪属性”** 或 **“跟踪模板属性”** 对话框中，单击 **“事件选择”** 选项卡。  
  
     **“事件选择”** 选项卡包含一个网格控件。 网格控件是包含所有可跟踪事件类的表。 每个事件类在表中占一行。 根据您所连接的服务器的类型和版本的不同，事件类会略有不同。 事件类由网格的“事件” **“事件”** 列进行标识，并按事件类别进行分组。 其余列则列出每个事件类可以返回的数据列。  
  
2.  在 **“事件选择”** 选项卡上，使用网格控件在跟踪文件中添加或删除事件和数据列。  
  
3.  若要从跟踪中删除事件，请清除每个事件类的 **“事件”** 列中的复选框。  
  
4.  若要在跟踪中包括事件，请选中每个事件类的 **“事件”** 列中的复选框，或者选中对应于事件的数据列。  
  
> [!IMPORTANT]  
>  如果跟踪要与系统监视器或性能监视器数据关联，则必须在跟踪中包括“开始时间”和“结束时间”数据列。  
  
 如果已选中对应于事件的复选框，在包括事件类时，跟踪中也将包括关联的数据列。 如果选中某个特定列的复选框，跟踪中将只包括该列。  
  
1.  若要从某个事件类删除数据列，请从事件类行中的数据列清除复选框，或右键单击列标题并选择“取消选择列”选项。  
  
2.  或者，对跟踪应用筛选器。 有关详细信息，请参阅[在跟踪中筛选事件 (SQL Server Profiler)](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
