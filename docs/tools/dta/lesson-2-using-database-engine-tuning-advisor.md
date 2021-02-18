---
title: 使用数据库引擎优化顾问
description: 了解 SQL Server 数据库引擎优化顾问 GUI 如何优化数据库、管理优化会话以及显示优化建议。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 3317d4f8-ed9e-4f2e-b5f1-a6bf3a9d6c8d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 10d32bb3267e79f4f97ce7a4832c1a999751904c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353516"
---
# <a name="lesson-2-using-database-engine-tuning-advisor"></a>第 2 课：使用数据库引擎优化顾问

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

通过数据库引擎优化顾问，您可以优化数据库、管理优化会话并查看优化建议。 对物理设计结构很熟悉的用户可使用此工具执行探索性数据库优化分析。 数据库优化初学者也可使用此工具为其优化的工作负荷找到最佳物理设计结构配置。 本课程为那些对于数据库引擎优化顾问图形用户界面陌生的数据库管理员和不太熟悉物理设计结构的系统管理员提供了基本练习。  

## <a name="prerequisites"></a>先决条件 

若要完成本教程，需要 SQL Server Management Studio、针对运行 SQL Server 的服务器的访问权限以及 AdventureWorks 数据库。

- 安装 [SQL Server Management Studio。](../../ssms/download-sql-server-management-studio-ssms.md)
- 安装 [SQL Server 2017 Developer Edition。](https://www.microsoft.com/sql-server/sql-server-downloads)
- 下载 [AdventureWorks2017 示例数据库。](../../samples/adventureworks-install-configure.md)


此处提供在 SSMS 中还原数据库的说明：[还原数据库。](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > 本教程适用于熟悉使用 SQL Server Management Studio 和基本数据库管理任务的用户。 
  
## <a name="tuning-a-workload"></a>优化工作负载
可以使用数据库引擎优化顾问，针对您选择进行优化的数据库和表来找到查询性能最佳的物理数据库设计。  

1.  复制示例 [SELECT](../../t-sql/queries/select-examples-transact-sql.md) 语句，并将此语句粘贴到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的查询编辑器中。 将该文件保存为 MyScript.sql，并存储在可以轻松找到的目录中。 下面的示例使用的是 AdventureWorks2017 数据库。  

 ```sql
 Use [Adventureworks2017]; -- may need to modify database name to match database
 GO
 SELECT DISTINCT pp.LastName, pp.FirstName 
 FROM Person.Person pp JOIN HumanResources.Employee e
 ON e.BusinessEntityID = pp.BusinessEntityID WHERE pp.BusinessEntityID IN 
 (SELECT SalesPersonID 
 FROM Sales.SalesOrderHeader
 WHERE SalesOrderID IN 
 (SELECT SalesOrderID 
 FROM Sales.SalesOrderDetail
 WHERE ProductID IN 
 (SELECT ProductID 
 FROM Production.Product p 
 WHERE ProductNumber = 'BK-M68B-42')));
 GO
 ```

  ![保存 SQL 查询](media/dta-tutorials/dta-save-query.png)
  
2.  启动数据库引擎优化顾问。 在 SQL Server Management Studio (SSMS) 的“工具”菜单中，选择“数据库优化顾问”。  有关详细信息，请参阅[启动数据库引擎优化顾问](lesson-1-basic-navigation-in-database-engine-tuning-advisor.md#launch-database-tuning-advisor)。 在“连接到服务器”对话框中，连接到 SQL Server。  
  
3.  在数据库引擎优化顾问 GUI 右窗格的“常规”选项卡的“会话名称”中，键入 MySession。 
  
4.  选中“工作负载”下的“文件”，然后选择双筒望远镜图标，以浏览工作负载文件。 查找在第 1 步中保存的“MyScript.sql”文件。  

   ![查找之前保存的脚本](media/dta-tutorials/dta-script.png)
  
5.  在“用于工作负载分析的数据库”列表中选择 AdventureWorks2017，在“选择要优化的数据库和表”网格中选择 AdventureWorks2017，并选择“保存优化日志”。 “用于工作负荷分析的数据库”指定数据库引擎优化顾问在优化工作负荷时连接到的第一个数据库。 优化开始之后，数据库引擎优化顾问连接到由工作负荷中包含的 `USE DATABASE` 语句所指定的数据库。  

  ![用于 db 的 DTA 选项](media/dta-tutorials/dta-select-db.png)
  
6.  单击“优化选项”选项卡。不必为本练习设置任何优化选项，但请花些时间来查看默认的优化选项。 按 F1 键可查看该选项卡式页面的帮助。 单击“高级选项”可查看其他的优化选项。 请在“高级优化选项”对话框中单击“帮助”，以了解有关此处所显示的优化选项的信息。 单击“取消”关闭“高级优化选项”对话框，并保留选中默认选项。  

  ![DTA 优化选项](media/dta-tutorials/dta-tuning-options.png)
  
7.  在工具栏中，单击 **“开始分析”** 按钮。 在数据库引擎优化顾问分析工作负荷时，可以监视“进度”选项卡上的状态。优化完成后，“建议”选项卡随即显示。  
  
    如果收到有关优化结束日期和时间的错误，请检查主“优化选项”选项卡上的“结束时间”。请确保“结束时间”的日期和时间晚于当前的日期和时间，必要时可进行更改。  

  ![启动 DTA 分析](media/dta-tutorials/dta-start-analysis.png)

  
8.  分析完成之后，在“操作”菜单中，单击“保存建议”，将建议保存为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 在“另存为”对话框中，导航到要保存建议脚本的目录，然后键入文件名 **MyRecommendations**。  

  ![保存 DTA 建议](media/dta-tutorials/dta-save-recommendations.png)

## <a name="view-tuning-recommendations"></a>查看优化建议
  
1.  在“建议”选项卡上，使用选项卡式页面底部的滚动条可以查看所有“索引建议”列。 每一行代表 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问建议删除或创建的一个数据库对象（索引或索引视图）。 滚动到最右边的列，并单击“定义”。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]优化顾问将显示“SQL 脚本预览”窗口，从中可以查看创建或删除该行中的数据库对象的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 单击“关闭”按钮以关闭预览窗口。  
  
    如果难以找到包含链接的“定义”，则请单击以清除选项卡式页面底部的“显示现有对象”复选框，从而减少所显示的行数。 如果清除此复选框，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问将仅显示已为其生成建议的对象。 选中“显示现有对象”复选框，可以查看 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中当前存在的所有数据库对象。 使用选项卡式页面右侧的滚动条可以查看所有对象。

  ![DTA 索引建议](media/dta-tutorials/dta-recommendation.png)  
  
2.  在“索引建议”窗格中右键单击网格。 在右键单击后出现的菜单中，您可以选择或取消选择建议。 您还可以使用此菜单更改网格文本的字体。  
 
   ![索引建议的选择菜单](media/dta-tutorials/dta-index-recommendation-options.png)
  
3.  单击“操作”菜单中的“保存建议”，将所有建议保存到一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本中。 将脚本命名为 **MySessionRecommendations.sql**。  
  
    在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的查询编辑器中打开 MySessionRecommendations.sql 脚本进行查看。 通过在查询编辑器中执行脚本，可将建议应用于 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库。但现在不要执行该操作。 不运行该脚本，直接在查询编辑器中将其关闭。  
  
    另外，也可以通过单击[!INCLUDE[ssDE](../../includes/ssde-md.md)]优化顾问“操作”菜单上的“应用建议”选项来应用建议。但现在不要在本练习中应用这些建议。  
  
4.  如果“建议”选项卡上存在多个建议，请清除“索引建议”网格中列出数据库对象的某些行。  
  
5.  在 **“操作”** 菜单上，单击 **“评估建议”** 。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问将创建一个新的优化会话，从中可以评估 MySession 原有建议的子集。  
  
6.  输入新的“会话名称” **EvaluateMySession**，然后单击工具栏中的“开始分析”按钮。 可以对新的优化会话重复步骤 2 和步骤 3 以查看其建议。  
  
### <a name="summary"></a>总结  
如果在运行会话之后必须更改优化选项，则可能有必要评估优化建议的子集。 例如，如果在指定会话的优化选项时要求 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问考虑索引视图，但在生成了建议后又决定不使用索引视图。 那么，可以使用“操作”菜单上的“评估建议”选项让 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问在不考虑索引视图的情况下重新评估会话。 使用“评估建议”选项时，将假设将以前生成的建议应用于当前物理设计，以获得第二个优化会话的物理设计。  
  
在“报告”选项卡中可以查看更多优化结果信息，这将在本课程的下一个任务中介绍。  

## <a name="view-tuning-reports"></a>查看优化报告
尽管查看可用于实现优化结果的脚本很有用，但数据库引擎优化顾问仍提供了许多可供查看的有用报告。 这些报告提供了有关正在优化的数据库中现有物理设计结构的信息，以及有关建议的结构的信息。 通过单击“报告”选项卡可以查看优化报告，如以下练习中所述。


1. 在数据库优化顾问中，选择“报告”选项卡。 
2. 在“优化摘要”窗格中，可以查看有关此优化会话的信息。 使用滚动条可以查看所有窗格内容。 请注意查看“预期的提高百分比”和“建议使用的空间”信息。 设置优化选项时，可以限制建议使用的空间。 在“优化选项”选项卡上，选择“高级选项”。 选中“定义建议所用的最大空间”，并以 MB 为单位指定建议配置可以使用的最大空间。 使用帮助浏览器中的“后退”按钮可返回到本教程。 

    ![DTA 优化摘要](media/dta-tutorials/dta-tuning-summary.png)
  
3.  在“优化报告”窗格中，单击“选择报告”列表中的“语句开销报告”。 如果需要更多空间以查看报表，则将“会话监视器”窗格边框拖动到左侧。 对数据库中的表执行的每个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句都会有相关的性能开销。 通过对表中经常访问的列创建有效的索引，可以降低此性能开销。 此报告显示了在工作负荷中执行语句的原有开销与实现优化建议后的开销相比，估计的提高百分比。 请注意，报告中包含的信息量取决于工作负荷的长度和复杂性。  

    ![DTA 报告 - 语句开销](media/dta-tutorials/dta-statement-cost.png)
  
4.  右键单击网格区域中的“语句开销报告”窗格，再单击“导出到文件”按钮。 将报告另存为 **MyReport**。 文件名后会自动附加 .xml 扩展名。 可以在常用的 XML 编辑器或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中打开 MyReport.xml 以查看报告内容。  
  
5.  返回数据库引擎优化顾问的“报告”选项卡，并再次右键单击“语句开销报告”。 查看其他可用选项。 请注意，您可以更改当前查看的报告的字体。 更改此处的字体也会更改其他选项卡式页面上的字体。  
  
6.  单击“选择报告”列表中的其他报告，了解相关内容。  
  
### <a name="summary"></a>总结  
现在，你已经浏览了 MySession 优化会话的数据库引擎优化顾问 GUI 的“报告”选项卡。 可以执行相同的步骤来浏览为 EvaluateMySession 优化会话生成的报告。 双击“会话监视器”窗格中的 **EvaluateMySession** 开始该会话。  


 ## <a name="next-lesson"></a>下一课  
[第 3 课：使用 DTA 命令提示符实用工具](../../tools/dta/lesson-3-using-the-dta-command-prompt-utility.md)  
   
  
