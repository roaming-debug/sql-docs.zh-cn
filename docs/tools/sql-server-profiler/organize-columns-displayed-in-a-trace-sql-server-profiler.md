---
title: 组织跟踪中显示的列
titleSuffix: SQL Server Profiler
description: 在定义或查看跟踪时通过对事件进行分组和聚合，更轻松地分析 SQL Server Profiler 跟踪输出。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6b923f94-0eb1-467e-82f6-ceed43f77017
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 2727aa7ed5e95881e21eddb73a94ddef97795c08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353414"
---
# <a name="organize-columns-displayed-in-a-trace-sql-server-profiler"></a>组织跟踪中显示的列 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

通过在跟踪表或“跟踪文件属性”对话框中选择“组织列”，或者在定义跟踪时，都可以将跟踪中的数据列分组。 将数据列分组可以更好地分析 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 跟踪输出。 有关详细信息，请参阅 [使用 SQL Server Profiler 查看和分析跟踪](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)。  
  
 **“组织列”** 可用于将跟踪事件分组，或者按所选的数据列将跟踪事件分组和聚合。  
  
-   为分组选择多个数据列，可仅对跟踪事件进行分组。 当为分组选择多个数据列时，跟踪窗口中所显示的事件将按您为分组选择的数据列中的值进行分组。 以下示例显示如果为分组选择 **Duration** 和 **StartTime** 数据列，将如何显示跟踪窗口网格。 请注意，跟踪窗口网格将先按升序显示 **Duration** 列值，然后再显示 **StartTime** 值。  
  
|Duration|StartTime|EventClass|ClientProcessID|  
|--------------|---------------|----------------|---------------------|  
||12/12/2006 3:16:43 PM|SQL:StmtStarting|2124|  
|0|12/12/2006 5:39:23 PM|审核登录|648|  
|1|12/12/2006 5:24:44 PM|SQL:StmtStarting|2124|  
|25|12/12/2006 5:24:44 PM|SQL:StmtCompleted|648|  
  
-   仅为分组选择一列，可对跟踪事件进行分组和聚合。 当仅为分组选择一个数据列时，跟踪窗口中所显示的事件将按数据列中的值分组并折叠在该数据列下。 在为分组选择的数据列中的事件左侧会显示加号 ( **+** )，且事件右侧的括号中会显示折叠在数据列下的事件数。 以下示例显示如果仅为分组选择 **EventClass** 数据列，将如何显示跟踪窗口网格。 请注意，所有事件都组织在 **EventClass** 数据列下。 若要查看所有事件，请单击加号来展开和显示该类型的所有事件类。  
  
|EventClass|StartTime|Duration|ClientProcessID|  
|----------------|---------------|--------------|---------------------|  
|+ ExistingConnection (6)||||  
|+ SQL:BatchStarting (25)||||  
|+ SQL:StmtCompleted (11)||||  
|+ SQL:SmtStarting (21)||||  
  
### <a name="to-group-data-columns-displayed-in-a-trace"></a>将跟踪中显示的数据列分组  
  
1.  打开现有的跟踪文件或表。  
  
2.  在 **“文件”** 菜单上，单击 **“属性”** 。  
  
3.  在 **“跟踪文件属性”** 或 **“跟踪表属性”** 对话框中，单击 **“事件选择”** 选项卡。  
  
4.  在 **“事件选择”** 选项卡上，单击 **“组织列”** 。  
  
5.  在 **“组织列”** 对话框中，选择要显示在组中的列，并单击 **“向上”** 将这些列移到 **“组”** 下。 在将所有要移动的列移到 **“组”** 下之后，就可以使用 **“向上”** 和 **“向下”** 按钮重新排列这些列的顺序。  
  
     将数据列的名称移到“组”列表中，意味着首先按显示在“组”列表顶部的数据列的值来组织跟踪，然后按“组”列表中第二个数据列的值来组织跟踪，依此类推。  
  
6.  单击 **“组织列”** 对话框中的 **“确定”** ，再单击 **“跟踪表属性”** 或 **“跟踪文件属性”** 对话框中的 **“确定”** 。  
  
     在单击 **“跟踪表属性”** 或 **“跟踪文件属性”** 对话框中的 **“确定”** 后，将在显示的跟踪中重新组织数据列。 在从左到右查看网格时，移到“组”列表顶部的数据列将显示在跟踪中的第一个数据列的位置。 跟踪中的行按 **“组”** 列表中数据列所包含的值的升序进行组织。 为分组选择的列在显示中保持固定，但可以左右滚动以查看其他列。  
  
7.  若要对所显示的跟踪数据取消分组，请单击 **“视图”** 菜单上的 **“分组视图”** 来取消选择。 若要恢复为分组视图，请再次单击 **“视图”** 菜单上的 **“分组视图”** 来重新选择。  
  
### <a name="to-group-and-aggregate-data-columns-in-a-trace"></a>分组和聚合跟踪中的数据列  
  
1.  打开现有的跟踪文件或表。  
  
2.  在 **“文件”** 菜单上，单击 **“属性”** 。  
  
3.  在 **“跟踪文件属性”** 或 **“跟踪表属性”** 对话框中，单击 **“事件选择”** 选项卡。  
  
4.  在 **“事件选择”** 选项卡上，单击 **“组织列”** 。  
  
5.  在 **“组织列”** 对话框中，选择要用于对所显示的跟踪事件进行分组和聚合的一个列。 单击 **“向上”** 将该列的名称移到 **“组”** 下。 如果需要，可以使用 **“向上”** 和 **“向下”** 按钮重新排列 **“列”** 下的其余列。  
  
6.  单击 **“组织列”** 对话框中的 **“确定”** ，再单击 **“跟踪表属性”** 或 **“跟踪文件属性”** 对话框中的 **“确定”** 。  
  
     在单击 **“跟踪表属性”** 或 **“跟踪文件属性”** 对话框中的 **“确定”** 后，将在显示的跟踪中重新组织数据列。 所有其他数据列事件都被聚合到已移到 **“组”** 列表中的数据列下。 单击为聚合选择的数据列中事件左侧的加号 ( **+** ) 将其展开，可以查看此类型的所有事件。 为聚合选择的列在显示时保持固定，但可以左右滚动以查看其他列。  
  
7.  若要使跟踪数据恢复为正常视图，请单击 **“视图”** 菜单上的 **“聚合视图”** 来取消选择。 如果要恢复为聚合视图，请再次单击 **“视图”** 菜单上的 **“聚合视图”** 来重新选择。 请注意，也可以单击 **“视图”** 菜单上的 **“分组视图”** 来显示分组的跟踪事件，而不折叠这些跟踪事件。  
  
## <a name="see-also"></a>另请参阅  
 [创建跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)   
 [打开跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)   
 [打开跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)  
  
  
