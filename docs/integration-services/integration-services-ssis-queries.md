---
description: Integration Services (SSIS) 查询
title: Integration Services (SSIS) 查询 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.querybuilder.f1
helpviewer_keywords:
- Query Builder [Integration Services]
- queries [Integration Services]
- statements [Integration Services]
- queries [Integration Services], about queries in packages
ms.assetid: 8822bd29-4575-46c8-92a0-1a39bc2604c1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 54a577a2a94c64eafe3817ccd9a041125629f846
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193839"
---
# <a name="integration-services-ssis-queries"></a>Integration Services (SSIS) 查询

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  执行 SQL 任务、OLE DB 源、OLE DB 目标以及查找转换都可以使用 SQL 查询。 在执行 SQL 任务中，SQL 语句可以创建、更新和删除数据库对象与数据；运行存储过程以及执行 SELECT 语句。 在 OLE DB 源和查找转换中，SQL 语句通常为 SELECT 语句或 EXEC 语句。 后者经常用于运行可返回结果集的存储过程。  
  
 可以对查询进行分析，以确定它是否有效。 如果分析的查询使用到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的连接，则在分析、执行该查询后，执行结果（成功或失败）将分配给分析结果。 如果该查询使用的是 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]之外的数据连接，则仅分析该语句。  
  
可以使用以下方式提供 SQL 语句：
1.   直接在设计器中输入该语句。
2.   指定与包含该语句的文件之间的连接。
3.   指定包含该语句的变量。  
  
## <a name="direct-input-sql"></a>直接输入 SQL  
 查询生成器可用于执行 SQL 任务的用户界面中、OLE DB 源、OLE DB 目标和查找转换中。 查询生成器提供下列优势：  
  
-   以直观方式工作或使用 SQL 命令。  
  
     查询生成器包括多个图形窗格和一个文本窗格，前者用于直观地编写查询，后者用于显示查询的 SQL 文本。 您既可以在图形窗格中工作，也可以在文本窗格中工作。 查询生成器会同步这两个视图，因此查询文本和图形表示形式始终匹配。  
  
-   联接相关表。  
  
     如果将多个表添加到查询中，查询生成器将自动确定这些表之间的关系，并构造适当的联接命令。  
  
-   查询或更新数据库。  
  
     可以使用查询生成器利用 Transact-SQL SELECT 语句来返回数据，或使用查询生成器来创建可更新、添加或删除数据库记录的查询。  
  
-   即时查看和编辑结果。  
  
     可以执行查询，使用网格中可滚动浏览和编辑数据库中记录的记录集。  
  
 尽管表面上看查询生成器的作用仅限于创建 SELECT 查询，但您可以在文本窗格中键入其他类型的 SQL 语句，例如 DELETE 和 UPDATE。 图形窗格将自动更新，以反映您所键入的 SQL 语句。  
  
 通过在任务流或数据流组件对话框中或“属性”窗口中键入查询，您还可以提供直接输入。  
  
 有关详细信息，请参阅 [Query Builder]()。  
  
## <a name="sql-in-files"></a>文件形式的 SQL  
 执行 SQL 任务的 SQL 语句还可以驻留在单独的文件中。 例如，您可以使用 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]中的查询编辑器等工具来编写查询，将查询保存到文件，然后在运行包时从该文件读取查询。 该文件可以只包含要运行的 SQL 语句以及注释。 若要使用存储在文件中的 SQL 语句，您必须提供指定文件名和位置的文件连接。 有关详细信息，请参阅 [File Connection Manager](../integration-services/connection-manager/file-connection-manager.md)。  
  
## <a name="sql-in-variables"></a>变量形式的 SQL  
 如果执行 SQL 任务中 SQL 语句的源是一个变量，您需要提供包含该查询的变量的名称。 该变量的 Value 属性包含查询文本。 可以将该变量的 ValueType 属性设置为字符串数据类型，然后键入 SQL 语句或将该语句复制到 Value 属性中。 有关详细信息，请参阅 [Integration Services (SSIS) 变量](../integration-services/integration-services-ssis-variables.md)和[在包中使用变量](./integration-services-ssis-variables.md)。  

## <a name="query-builder-dialog-box"></a>“查询生成器”对话框
可以使用 **“查询生成器”** 对话框，创建在执行 SQL 任务、OLE DB 源和 OLE DB 目标以及查找转换中使用的查询。  
  
 可以使用查询生成器来执行下列任务：  
  
-   **使用查询的图形表示形式或 SQL 命令** 查询生成器包括以图形方式显示查询的窗格和以 SQL 文本方式显示查询的窗格。 您既可以在图形窗格中工作，也可以在文本窗格中工作。 查询生成器可以同步视图，使其始终保持为最新状态。  
  
-   **联接相关表** 如果将多个表添加到查询中，查询生成器将自动确定这些表之间的关系，并构造相应的联接命令。  
  
-   **查询或更新数据库** 可以使用查询生成器通过 Transact-SQL SELECT 语句来返回数据，还可使用它来创建查询以便更新、添加或删除数据库中的记录。  
  
-   **立即查看和编辑结果** 您可以运行查询，在可滚动和编辑数据库中各记录的网格中对记录集进行操作。  
  
 “查询生成器”对话框中的图形工具使你可以使用拖放操作来构造查询。 默认情况下，“查询生成器”对话框将生成 SELECT 查询，但也可以生成 INSERT、UPDATE 或 DELETE 查询。 还可以在 **“查询生成器”** 对话框中分析和运行所有类型的 SQL 语句。 有关包中 SQL 语句的详细信息，请参阅 [Integration Services (SSIS) 查询](../integration-services/integration-services-ssis-queries.md)。  
  
 若要了解关于 Transact-SQL 语言及其语法的详细信息，请参阅 [Transact-SQL 引用（数据库引擎）](../t-sql/language-reference.md)。  
  
 还可以在查询中使用变量，以便为输入参数提供值、捕获输出参数的值并存储返回代码。 若要了解包所使用的查询中使用变量的详细信息，请参阅[执行 SQL Task](../integration-services/control-flow/execute-sql-task.md)[OLE DB 源](../integration-services/data-flow/ole-db-source.md) 和 [Integration Services (SSIS) 查询](../integration-services/integration-services-ssis-queries.md)。 若要了解有关在执行 SQL 任务中使用变量的详细信息，请参阅 [执行 SQL 任务中的参数和返回代码](./control-flow/execute-sql-task.md) 和 [执行 SQL 任务中的结果集](./control-flow/execute-sql-task.md)。  
  
 查找转换和模糊查找转换也可以使用带有参数和返回代码和变量。 有关 OLE DB 源的信息也适用于上述两种转换。  
  
### <a name="options"></a>选项  
 **工具栏**  
 使用工具栏可以管理数据集、选择要显示的窗格以及控制查询函数。  
  
|值|说明|  
|-----------|-----------------|  
|**显示/隐藏关系图窗格**|显示或隐藏 **“关系图”** 窗格。|  
|**显示/隐藏网格窗格**|显示或隐藏 **“网格”** 窗格。|  
|**显示/隐藏 SQL 窗格**|显示或隐藏 **SQL** 窗格。|  
|**显示/隐藏结果窗格**|显示或隐藏 **“结果”** 窗格。|  
|**Run**|运行查询。 结果将显示在结果窗格中。|  
|**验证 SQL**|验证 SQL 语句是否有效。|  
|**升序排序**|依据网格窗格中的所选列对输出行按升序排序。|  
|**降序排序**|依据网格窗格中的所选列对输出行按降序排序。|  
|**删除筛选器**|在网格窗格中选择列名，再单击 **“删除筛选器”** 可以删除列的排序条件。|  
|**使用 Group By**|向查询中添加 GROUP BY 功能。|  
|**添加表**|向查询中添加新表。|  
  
 **查询定义**  
 查询定义提供可用来定义和测试查询的工具栏和窗格。  
  
|窗格|说明|  
|----------|-----------------|  
|**“关系图”** 窗格|在关系图中显示查询。 关系图可显示查询中包含的表以及这些表的联接方式。 选中或清除表中某列旁边的复选框，即可在查询输出中添加或删除该列。<br /><br /> 当您向查询添加表时，查询生成器将根据表和表中的键在表之间创建联接。 若要添加联接，请将一个表中的字段拖到另一个表中的字段上。 若要管理联接，请右键单击该联接，再选择菜单选项。<br /><br /> 右键单击“关系图”窗格，可以添加或删除表，选择所有表，以及显示或隐藏窗格。|  
|**“网格”** 窗格|在网格中显示查询。 使用此窗格可以在查询中添加和删除列，以及更改每个列的设置。|  
|**SQL** 窗格|以 SQL 文本的形式显示查询。 在 **“关系图”** 窗格和 **“网格”** 窗格中所做的更改将显示在此窗格中，在此窗格中所做的更改也将显示在 **“关系图”** 窗格和 **“网格”** 窗格中。|  
|**“结果”** 窗格|在您单击工具栏上的 **“运行”** 时显示查询的结果。| 

