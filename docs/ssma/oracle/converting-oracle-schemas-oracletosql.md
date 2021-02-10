---
title: 将 Oracle 架构转换 (OracleToSQL) |Microsoft Docs
description: 在设置选项并连接到 Oracle 和 SQL Server 后，了解如何将 Oracle 数据库对象转换 SQL Server 为 SSMA for Oracle 的数据库对象。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Conversion Results
ms.assetid: e021182d-31da-443d-b110-937f5db27272
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8e2ee101ec916a920d4b72d924456a52850e8bc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058752"
---
# <a name="converting-oracle-schemas-oracletosql"></a>转换 Oracle 架构 (OracleToSQL)
连接到 Oracle、连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并设置项目和数据映射选项后，可以将 Oracle 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象。  
  
## <a name="the-conversion-process"></a>转换过程  
转换数据库对象将获取 Oracle 中的对象定义，将它们转换为类似 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象，然后将此信息加载到 SSMA 元数据。 它不会将信息加载到的实例中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 然后，您可以使用 " [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据资源管理器" 查看对象及其属性。  
  
在转换过程中，SSMA 会将输出消息打印到 "输出" 窗格，并将错误消息打印到 "错误列表" 窗格。 使用输出和错误信息来确定是否必须修改 Oracle 数据库或转换过程以获取所需的转换结果。  
  
## <a name="setting-conversion-options"></a>设置转换选项  
转换对象之前，请在 " **项目设置** " 对话框中查看项目转换选项。 使用此对话框，可以设置 SSMA 如何转换函数和全局变量。 有关详细信息，请参阅 [OracleToSQL&#41;&#40;转换&#41; &#40;项目设置 ](../../ssma/oracle/project-settings-conversion-oracletosql.md)。  
  
## <a name="conversion-results"></a>转换结果  
下表显示了转换哪些 Oracle 对象以及生成的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象：  
  
|Oracle 对象|生成的 SQL Server 对象|  
|-|-|  
|函数|如果函数可以直接转换为，则 [!INCLUDE[tsql](../../includes/tsql-md.md)] SSMA 将创建一个函数。<br /><br />在某些情况下，该函数必须转换为存储过程。 在这种情况下，SSMA 创建存储过程和调用存储过程的函数。|  
|过程|如果可以将该过程直接转换为 [!INCLUDE[tsql](../../includes/tsql-md.md)] ，则 SSMA 将创建一个存储过程。<br /><br />在某些情况下，必须在自治事务中调用存储过程。 在这种情况下，SSMA 创建两个存储过程：一个用于实现过程，另一个用于调用实现存储过程。|  
|包|SSMA 创建一组由类似对象名称统一的存储过程和函数。|  
|序列|SSMA SQL Server 2012 或 SQL Server 2014) 或模拟 Oracle 序列 (创建序列对象。|  
|具有依赖对象（如索引和触发器）的表|SSMA 创建具有依赖对象的表。|  
|具有依赖对象（如触发器）的视图|SSMA 创建具有依赖对象的视图。|  
|具体化视图|**SSMA 在 SQL server 上创建索引视图，但有一些例外情况。如果具体化视图包含一个或多个以下构造，转换将失败：**<br /><br />用户定义函数<br /><br />SELECT、WHERE 或 GROUP BY 子句中的非确定性字段/函数/表达式<br /><br />在 SELECT *、WHERE 或 GROUP BY 子句中使用 Float 列 (上一问题的特殊情况) <br /><br />自定义数据类型 (包括嵌套表) <br /><br />计数 (非重复 &lt; 字段 &gt;) <br /><br />FETCH<br /><br />OUTER 联接（LEFT、RIGHT 或 FULL）<br /><br />子查询，其他视图<br /><br />过度、排名、潜在顾客、日志<br /><br />MIN、MAX<br /><br />UNION、减法、INTERSECT<br /><br />HAVING|  
|触发器|**SSMA 基于以下规则创建触发器：**<br /><br />在触发器转换为 INSTEAD of 触发器之前。<br /><br />AFTER 触发器转换为 AFTER 触发器。<br /><br />INSTEAD of 触发器会转换为 INSTEAD of 触发器。 在同一操作中定义的多个 INSTEAD of 触发器合并为一个触发器。<br /><br />使用游标模拟行级触发器。<br /><br />级联触发器将转换为多个单独的触发器。|  
|同义词|**为以下对象类型创建同义词：**<br /><br />表和对象表<br /><br />视图和对象视图<br /><br />存储过程<br /><br />函数<br /><br />**以下对象的同义词由直接对象引用解析和替换：**<br /><br />序列<br /><br />包<br /><br />Java 类架构对象<br /><br />用户定义的对象类型<br /><br />不能迁移其他同义词的同义词，并将其标记为错误。<br /><br />不会为具体化视图创建同义词。|  
|用户定义的类型|**SSMA 不支持转换用户定义的类型。用户定义的类型（包括其在 PL/SQL 程序中的用法）标记有以下规则所述的特殊转换错误：**<br /><br />用户定义类型的表列转换为 VARCHAR (8000) 。<br /><br />存储过程或函数的用户定义类型的参数转换为 VARCHAR (8000) 。<br /><br />PL/SQL 块中用户定义类型的变量转换为 VARCHAR (8000) 。<br /><br />对象表转换为标准表。<br /><br />对象视图转换为标准视图。|  
  
## <a name="converting-oracle-database-objects"></a>转换 Oracle Database 对象  
若要转换 Oracle 数据库对象，请首先选择要转换的对象，然后让 SSMA 执行转换。 若要在转换过程中查看输出消息，请在 " **视图** " 菜单上选择 " **输出**"。  
  
**将 Oracle 对象转换为 SQL Server 语法**  
  
1.  在 Oracle 元数据资源管理器中，展开 Oracle 服务器，然后展开 " **架构**"。  
  
2.  选择要转换的对象：  
  
    -   若要转换所有架构，请选中 " **架构**" 旁边的复选框。  
  
    -   若要转换或省略数据库，请选中该架构名称旁边的复选框。  
  
    -   若要转换或省略对象的类别，请展开一个架构，然后选中或清除该类别旁边的复选框。  
  
    -   若要转换或省略单个对象，请展开 category 文件夹，然后选中或清除该对象旁边的复选框。  
  
3.  若要转换所有选定的对象，请右键单击 " **架构** "，然后选择 " **转换架构**"。  
  
    您还可以通过右键单击对象或其父文件夹，然后选择 " **转换架构**"，来转换各个对象或对象类别。  
  
## <a name="viewing-conversion-problems"></a>查看转换问题  
某些 Oracle 对象可能不会转换。 您可以通过查看摘要转换报告来确定转换成功率。  
  
**查看摘要报表**  
  
1.  在 Oracle 元数据资源管理器中，选择 " **架构**"。  
  
2.  在右侧窗格中，选择 " **报表** " 选项卡。  
  
    此报表显示已评估或转换的所有数据库对象的摘要评估报告。 您还可以查看单个对象的摘要报表：  
  
    -   若要查看单个架构的报表，请在 Oracle 元数据资源管理器中选择该架构。  
  
    -   若要查看单个对象的报表，请在 "Oracle 元数据资源管理器" 中选择该对象。 具有转换问题的对象具有红色错误图标。  
  
对于失败转换的对象，可以查看导致转换失败的语法。  
  
**查看各个转换问题**  
  
1.  在 Oracle 元数据资源管理器中，展开 " **架构**"。  
  
2.  展开显示红色错误图标的架构。  
  
3.  在该架构下，展开一个包含红色错误图标的文件夹。  
  
4.  选择包含红色错误图标的对象。  
  
5.  在右侧窗格中，单击 " **报表** " 选项卡。  
  
6.  在 " **报表** " 选项卡的顶部是一个下拉列表。 如果列表显示 " **统计信息**"，请将所选内容更改为 " **源**"。  
  
    SSMA 将显示源代码，并将多个按钮直接显示在代码上方。  
  
7.  单击 " **下一问题** " 按钮。 这是一个红色的错误图标，其中有一个指向右侧的箭头。  
  
    SSMA 将突出显示在当前对象中找到的第一个有问题的源代码。  
  
对于无法转换的每个项，必须确定要对该对象执行哪些操作：  
  
-   您可以在 " **SQL** " 选项卡上修改过程的源代码。  
  
-   您可以修改 Oracle 数据库中的对象，以删除或修改有问题的代码。 若要将更新的代码加载到 SSMA 中，必须更新元数据。 有关详细信息，请参阅 [连接到 Oracle Database &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md)。  
  
-   可以从迁移中排除对象。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] "元数据资源管理器" 和 "Oracle 元数据资源管理器" 中，清除项旁的复选框，然后将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Oracle 并从 Oracle 迁移数据。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是将已 [转换的对象加载到 SQL Server 中](loading-converted-database-objects-into-sql-server-oracletosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 Oracle 数据库迁移到 SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
