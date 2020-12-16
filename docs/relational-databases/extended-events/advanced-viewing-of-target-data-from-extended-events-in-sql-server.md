---
title: 扩展事件的目标数据的高级视图
description: 使用 SQL Server Management Studio 的高级功能来详细查看扩展事件的目标数据。 你可以查看、导出和操纵数据。
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
ms.assetid: b2e839d7-1872-46d9-b7b7-6dcb3984829f
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f6bfcd07a238c94938793d3c25769057eb058fde
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465628"
---
# <a name="advanced-viewing-of-target-data-from-extended-events-in-sql-server"></a>SQL Server 中扩展事件的目标数据的高级查看功能

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


本文将阐释如何使用 SQL Server Management Studio (SSMS.exe) 的高级功能来详细查看扩展事件的目标数据。 本文说明了如何：


- 以各种方式打开并查看目标数据。
- 使用扩展事件的专用菜单或工具栏将目标数据导出为各种格式。
- 查看时或在导出前对数据进行操作。



### <a name="prerequisites"></a>先决条件

本文假定你已了解如何创建和启动事件会话。 有关如何创建事件会话的说明之前在下面的文章中已演示：

[快速入门：SQL Server 中的扩展事件](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)


本文还假定你已安装 SSMS 的最新月版本。 安装帮助位于：

- [下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)



### <a name="differences-with-azure-sql-database"></a>与 Azure SQL 数据库之间的差异


Microsoft SQL Server 和 Azure SQL 数据库这两个产品的扩展事件的实现与功能方面具有很大程度的一致性。 但仍存在一些差异会影响 SSMS UI（用户界面）。


- 对于 SQL 数据库，package0.event_file 目标不能是本地磁盘驱动器上的文件， 而必须是 Azure 存储容器。 因此，当连接到 SQL 数据库时，SSMS UI 将寻求存储容器，而不是本地路径和文件名称。


- 在 SSMS UI 中如果你看到复选框“查看实时数据”  为灰色且被禁用，这是因为该功能不可用于 SQL 数据库。


- 一些扩展事件与 SQL Server 一同安装。 在“会话”  节点下面我们可以看到 **AlwaysOn_health** 及其他几个会话。 连接到 SQL 数据库时这些会话不可见，因为它们对于 SQL 数据库来说并不存在。


本文是从 SQL Server 的角度来撰写的。 本文使用的 event_file 目标是差异的一个方面。 进一步提及的任何差异仅限于重要的或不明显的差异。


有关特定于 Azure SQL 数据库的扩展事件的文档，请参阅：

- [Azure SQL 数据库中的扩展事件](/azure/azure-sql/database/xevent-db-diff-from-svr)



## <a name="a-general-options"></a>A. 常规选项


通常，通过以下方式访问高级选项：


- 单击常规菜单项：“文件” > “打开” > “文件”。   
- 在“对象资源管理器”的“管理” > “扩展事件”下方右键单击。   
- **扩展事件** 的专用菜单，和扩展事件的专用工具栏。
- 在显示目标数据的选项卡式窗格中右键单击。



## <a name="b-bring-target-data-into-ssms-for-display"></a>B. 将目标数据引入 SSMS 进行显示


有多种方法可以将 event_file 目标数据引入 SSMS UI。 当指定 event_file 目标时，将设置它的文件路径和名称：

- .XEL 是文件扩展名。


- 每次启动该事件会话时，系统会在新的文件名中嵌入一个非常大的整数，以使文件名唯一，且不同于以前启动会话时用的数字。
  - *示例：* Checkpoint_Begins_ES_0_131103935140400000.xel


- .XEL 中的内容不是可以使用 Notepad.exe 来查看的纯文本。
  - 如果需要，你可以使用菜单“文件” > “打开” > “合并扩展事件文件”同时附加几个 .XEL 文件。   



SSMS 可以显示来自任何目标的数据。 但是对于不同的目标显示也有所不同：

- *event_file：* 可以很好地显示来自 event_file 目标的数据，随附有丰富的功能。


- *ring_buffer：* 来自 ring-buffer 目标的数据显示为原始 XML。


- 对于其他目标，显示的能力介于 event_file 和 ring_buffer 之间。
  - 此类的其他目标包括 event_counter、histogram 和 pair_matching。


- *etw_classic_sync_target：* SSMS 无法显示来自目标类型 etw_classic_sync_target 的数据。



### <a name="b1-open-xel-with-menu-file--open--file"></a>B.1 通过菜单“文件”>“打开”>“文件”打开 .XEL


你可以通过标准菜单“文件” > “打开” > “文件”打开单个 .XEL 文件。   

你还可以将 XEL 文件拖放到 SSMS UI 中的选项卡栏。



### <a name="b2-view-target-data"></a>B.2 查看目标数据


**查看目标数据** 选项显示到目前为止已捕获的数据。


在“对象资源管理器”  窗格中，可以展开节点，然后右键单击：

- “管理” > “扩展事件” > “会话” > “[your-session]” > “[your-target-node]” > “查看目标数据”。      


目标数据显示在 SSMS 中的选项卡式窗格中。 如下面的屏幕截图所示。


![你的目标 > 查看目标数据](../../relational-databases/extended-events/media/xevents-ssms-ui20-viewtargetdata.png)


> [!NOTE] 
> **查看目标数据** 显示指定事件会话的多个 .XEL 文件的累积数据。  每个 **启动**-**停止** 周期都将创建一个文件，并且在文件名中嵌入由时间转换而来的整数，但是每个文件共享相同的根名称。



#### <a name="b3-watch-live-data"></a>B.3 查看实时数据


当事件会话当前处于活动状态时，你可能想要在目标接收数据的同时查看实时的事件数据。


- “管理”   > “扩展事件”   > “会话”   > “[your-session]”   > “查看实时数据”  。


![你的会话 > 查看实时数据](../../relational-databases/extended-events/media/xevents-ssms-ui55-watchlivedata.png)


数据显示按你指定的时间间隔更新。 请在以下位置查看 **最大调度滞后时间** ：

- “扩展事件”   > “会话”   > “[your-session]”   > “属性”   > “高级”   > “最大调度滞后时间” 



### <a name="b4-view-xel-with-sysfn_xe_file_target_read_file-function"></a>B.4 使用 sys.fn_xe_file_target_read_file 函数查看 .XEL


对于批处理，以下系统函数可为 .XEL 文件中的记录生成 XML：

- [sys.fn\_xe\_file\_target\_read\_file](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md)



## <a name="c-export-the-target-data"></a>C. 导出目标数据


在 SSMS 中拥有目标数据之后，可以通过以下操作将数据导出为各种格式：


1. 将焦点移至数据显示。

    - 此时将突然显示用于扩展事件的一个新工具栏和一个新菜单项。

    ![导出显示的数据，“扩展事件”>“导出为”>（.csv、.xel 或 表）](../../relational-databases/extended-events/media/xevents-ssms-ui75-menuextevent-exportto-xel.png)

2. 单击新的菜单项“扩展事件”  。
3. 单击“导出为”  ，然后选择一种格式。



## <a name="d-manipulate-data-in-the-display"></a>D. 操作显示中的数据


SSMS UI 提供了几种方式来操作数据，而不只是查看数据。



### <a name="d1-context-menus-in-the-data-display"></a>D.1 数据显示中的上下文菜单


右键单击时，数据显示中的不同位置的上下文菜单各不相同。



#### <a name="d11-right-click-a-data-cell"></a>D.1.1 右键单击数据单元格


以下屏幕截图显示右键单击数据显示中的单元格时获得的内容菜单。 该屏幕截图中还显示了“复制”菜单项的扩展项。 


![右键单击数据显示中的一个单元格](../../relational-databases/extended-events/media/xevents-ssms-ui25-rightclickcell.png)



#### <a name="d12-right-click-a-column-header"></a>D.1.2 右键单击列标题


以下屏幕截图显示右键单击“时间戳”  标题时获得的上下文菜单。


![右键单击数据显示中的一个列标题。 此外，还显示了“详细信息”网格。](../../relational-databases/extended-events/media/xevents-ssms-ui40-toolbar.png)


前面的屏幕截图还显示了扩展事件的专用工具栏。 “详细信息”按钮的亮度指示该按钮处于活动状态。 因此图片中还显示了“详细信息”  选项卡，该网格显示为数据显示的第二部分。



### <a name="d2-choose-columns-merge-columns"></a>D.2 选择列，合并列


可以使用“选择列”  选项控制哪些数据列显示，哪些不显示。 你可以在不同位置找到“选择列”  菜单项：

- 在“扩展事件”菜单中。 
- 在“扩展事件”工具栏上。
- 在数据显示的标题的上下文菜单中。


单击“选择列”  后，将显示相同名称的对话框。


![“选择列”对话框也提供了“合并列”选项](../../relational-databases/extended-events/media/xevents-ssms-ui35-choosecolumns.png)



#### <a name="d21-merge-columns"></a>D.2.1 合并列


 “选择列”对话框中有一部分专门用于将多个列合并为一个列，以用于：

- 显示。
- 导出。



### <a name="d3-filters"></a>D.3 筛选器


在扩展事件的区域中可以指定两种主要类型的筛选器：

- *目标前筛选器：* 可以减少事件引擎发送到目标的数据量的筛选器。

- *目标后筛选器：* 可以在 SSMS UI 中选择的筛选器，用于排除要显示的某些目标记录。


SSMS 显示筛选器如下所示：

-  时间范围筛选器，用于检测“时间戳”  列。
- 列值筛选器。 


时间筛选器和列筛选器在逻辑上是“与”的关系  。


![“筛选器”对话框中的时间范围和列筛选器](../../relational-databases/extended-events/media/xevents-ssms-ui45-filters.png)



### <a name="d4-grouping-and-aggregation"></a>D.4 分组和聚合


通过匹配指定列的值将行组合在一起是对数据进行聚合的第一步。



#### <a name="d41-grouping"></a>D.4.1 分组


在扩展事件工具栏中，“分组”  按钮可启动一个对话框，你可以在该对话框中按给定的列对显示数据进行分组。 下面的屏幕截图显示了按“名称”列进行分组的对话框。 

![显示已选择“分组”的工具栏和“分组”对话框的屏幕截图。](../../relational-databases/extended-events/media/xevents-ssms-ui53-grouping.png)

实现分组后，将以新的方式显示，如下所示。

![分组后的新的显示方式](../../relational-databases/extended-events/media/xevents-ssms-ui54-grouped.png)



#### <a name="d42-aggregation"></a>D.4.2 聚合


在对显示数据进行分组后，可以继续使用其他列对数据进行聚合。  以下屏幕截图显示了按“计数”对分组数据进行聚合。 

![显示已选择“聚合”的工具栏和“聚合”对话框的屏幕截图。](../../relational-databases/extended-events/media/xevents-ssms-ui51-aggregdialogcount.png)

实现聚合后，将以新的方式显示，如下所示。

![展示 COUNT 值已添加的显示屏幕截图。](../../relational-databases/extended-events/media/xevents-ssms-ui52-aggregnewdisplay.png)



### <a name="d5-view-run-time-query-plan"></a>D.5 查看运行时查询计划


使用 **Query_post_execution_showplan** 事件可以查看 SSMS UI 中的实际查询计划。 当显示“详细信息”  窗格时，可以在“查询计划”选项卡中看到查询计划图。  将鼠标悬停在查询计划的一个节点上方时，可以看到该节点的属性名称及其值的列表。


![查询计划及一个节点的属性列表](../../relational-databases/extended-events/media/xevents-ssms-ui60-showplangraph.png)

## <a name="see-also"></a>另请参阅

[XELite:跨平台库，用于从 XEL 文件或实时 SQL 流中读取 XEvent](https://www.nuget.org/packages/Microsoft.SqlServer.XEvent.XELite/)，发布于 2019 年 5 月。

[Read-SQLXEvent PowerShell cmdlet](https://www.powershellgallery.com/packages/SqlServer)，发布于 2019 年 7 月。