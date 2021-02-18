---
title: 修改筛选器
titleSuffix: SQL Server Profiler
description: 了解如何在 SQL Server Profiler 中修改跟踪筛选器，以便可以收集所需信息。 了解跟踪筛选器如何影响数据库性能。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 8b317813-4918-4485-b930-77b1951aa00c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: dc2171ea2aaf418eae0fa277a8f4144dc4e7ca9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345047"
---
# <a name="modify-a-filter-sql-server-profiler"></a>修改筛选器 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

可以通过将筛选器添加到包含跟踪定义的跟踪模板，来限制跟踪所收集的事件数。 限制收集的事件数能够减少跟踪对性能的影响。 如果已设置了跟踪模板的筛选器，并发现该跟踪没有收集所需类型的信息，则可以对该筛选器进行编辑。  
  
### <a name="to-modify-a-filter"></a>修改筛选器  
  
1.  在 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]中，打开要修改的跟踪筛选器的模板。 在 **“文件”** 菜单上，单击 **“模板”** ，然后选择 **“编辑模板”** 。  
  
2.  在 **“跟踪模板属性”** 对话框的 **“常规”** 选项卡中，选择 **“选择模板名称”** 列表中的一个模板。  
  
3.  单击 **“事件选择”** 选项卡。  
  
     **“事件选择”** 选项卡包含一个网格控件。 网格控件是包含所有可跟踪事件类的表。 每个事件类在表中占一行。 事件类可能略有不同，这取决于所连接服务器的类型和版本。 事件类由网格的“事件” **“事件”** 列进行标识，并按事件类别进行分组。 其余列则列出每个事件类可以返回的数据列。  
  
4.  单击 **“列筛选器”** 。  
  
5.  在 **“编辑筛选器”** 对话框中，单击要编辑的比较运算符旁边的值，然后键入新值或删除一个值。 此外，还可以添加其他筛选器。  
  
6.  单击 **“确定”** 保存模板。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
