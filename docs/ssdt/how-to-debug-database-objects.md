---
title: 调试数据库对象
description: 了解如何调试存储过程、函数和触发器。 了解如何在调试模式下启用调试、设置断点以及运行 SQL Server 单元测试。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f5d4584f-e85f-4558-b056-83681c365978
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: b77373adb76921ee526aea70528399970bda9246
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597018"
---
# <a name="how-to--debug-database-objects"></a>如何：调试数据库对象

SQL Server 单元测试由以下内容组成：  
  
-   以 Visual C\# 或 Visual Basic 编写的单元测试代码。 此代码（由 SQL Server 单元测试设计器生成）负责提交构成测试主体的 Transact\-SQL 脚本。  
  
-   一个或多个用 Visual C\# 或 Visual Basic 编写的测试条件。 若要调试测试条件，请按照[如何：在运行测试时进行调试 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182484(v=vs.100)) 或[如何：在运行测试时进行调试 (Visual Studio 2012)](/previous-versions/ms182484(v=vs.140)) 中介绍的调试单元测试的过程进行操作。  
  
-   一个或多个在所测试的数据库中的对象上运行的 Transact\-SQL 脚本。 不能调试这些 Transact\-SQL 脚本。  
  
本主题中的过程介绍如何调试特定数据库对象，例如您要测试的数据库中的存储过程、函数和触发器。 若要调试数据库对象，请按以下顺序执行这些过程：  
  
1.  在测试项目上启用 SQL Server 调试。  
  
2.  在承载你要测试的数据库的 SQL Server 实例上启用应用程序调试。  
  
3.  在你要调试的数据库对象的 Transact\-SQL 脚本中设置断点。  
  
4.  调试您的单元测试。 在此过程中，您将在调试模式中运行测试。  
  
### <a name="to-enable-sql-debugging-on-your-test-project"></a>在测试项目上启用 SQL 调试  
  
1.  打开“解决方案资源管理器”。  
  
2.  在“解决方案资源管理器”中，右键单击测试项目，然后单击“属性”。  
  
    将打开与测试项目具有相同名称的属性页。  
  
3.  在属性页上单击“调试”。  
  
4.  在“启用调试器”下，单击“启用 SQL Server 调试”。  
  
5.  保存所做更改。  
  
### <a name="to-set-an-increased-execution-context-timeout-to-enable-debugging-for-your-test-project"></a>设置增加的执行上下文超时以便为测试项目启用调试  
  
1.  在“文件”菜单上指向“打开”，再单击“文件”。  
  
2.  浏览至包含测试项目的文件夹，然后双击 app.config 文件。  
  
    app.config 文件将在编辑器中打开。  
  
3.  修改 ExecutionContext 节点以添加一个命令超时，如下面的示例中所示：  
  
    ```  
    <ExecutionContext CommandTimeout ="300" Provider="System.Data.SqlClient" ConnectionString="Data Source=TargetServerName\TargetInstanceName;Initial Catalog=TargetDatabaseName;Integrated Security=True;Pooling=False" />  
    ```  
  
4.  保存所做更改。  
  
5.  重新生成您的单元测试项目。  
  
> [!IMPORTANT]  
> 如果您没有重新生成项目，则当您运行单元测试时将不会应用您对 app.config 所做的更改，并且调试将失败。  
  
### <a name="to-add-breakpoints-to-your-transact-sql-script"></a>在你的 Transact\-SQL 脚本中添加断点  
  
1.  在“视图”菜单上，打开“SQL Server 对象资源管理器”。  
  
2.  在“数据连接”下，展开你想要测试的数据库节点。  
  
3.  如果数据库图标旁出现一个小的红色“x”，则与该数据库的连接已关闭。 在这种情况下，右键单击数据库，然后单击“刷新”。 您可能必须提供凭据才能打开与该数据库的连接。  
  
4.  展开“视图”、“存储过程”或“函数”节点以找到你想要调试的对象。  
  
5.  双击要调试的对象。  
  
6.  单击灰色侧栏以设置断点。  
  
### <a name="to-debug-your-sql-server-unit-test"></a>调试你的 SQL Server 单元测试  
  
1.  在 Visual Studio 2010 中，打开（测试 -> Windows）“测试视图”窗口。 在 Visual Studio 2012 中，打开“测试资源管理器”窗口。  
  
2.  右键单击对其中设置了断点的数据库对象执行 Transact\-SQL 脚本的测试并且选择“调试选择”。  
  
    测试将在调试模式中运行，直至遇到数据库对象中的断点。  
  
3.  （可选）若要打开另一个调试窗口，请打开“调试”菜单，指向“窗口”，然后单击“断点”、“输出”或“即时”。  
  
## <a name="see-also"></a>另请参阅  
[运行 SQL Server 单元测试](../ssdt/running-sql-server-unit-tests.md)  
[调试 Transact-SQL (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/zefbf0t6(v=vs.100))  
