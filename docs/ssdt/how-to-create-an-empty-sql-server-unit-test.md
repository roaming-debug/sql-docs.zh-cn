---
title: 创建空的 SQL Server 单元测试
description: 了解如何创建 SQL Server 单元测试。 请参阅如何使用其他测试使用的相同 TestInitialize 和 TestCleanup 脚本，以及如何使用不同的脚本。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.createtest
ms.assetid: b6f3cd5a-3389-42d6-a93f-97b3ddf31b95
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 510d669d780a7f7b6f71f5a84eab87e9b70bf4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018317"
---
# <a name="how-to-create-an-empty-sql-server-unit-test"></a>如何：创建空的 SQL Server 单元测试

在数据库项目中包含单元测试来验证您对数据库对象所做的更改不会破坏现有功能。 以下过程解释如何针对任何数据库对象创建 SQL Server 单元测试。 SQL Server Data Tools 包括对数据库函数、触发器和存储过程的某些其他支持。 有关详细信息，请参阅[操作说明：为函数、触发器和存储过程创建 SQL Server 单元测试](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)。  
  
当你使用第一个过程创建 SQL Server 单元测试时，如果不存在测试项目，则会自动为你创建测试项目。 如果测试项目已经存在，则您可以选择将新测试添加到其中的某个项目，或者您可以创建新的测试项目。 有关测试项目的更多信息，请参见[如何：为 SQL Server 数据库单元测试创建测试项目](../ssdt/how-to-create-a-test-project-for-sql-server-database-unit-testing.md)。  
  
有两种方法可创建 SQL Server 单元测试：  
  
-   在新的测试类中创建新的 SQL Server 单元测试。  
  
    给定的测试类中的所有 SQL Server 单元测试将使用相同的 TestInitialize 和 TestCleanup 脚本。 如果您希望您的单元测试使用与其他单元测试不同的 TestInitialize 和 TestCleanup 脚本，则可创建新的测试类。 有关详细信息，请参阅 [SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)。  
  
-   在现有测试类中创建新的 SQL Server 单元测试。  
  
    如果您的单元测试使用与类中的其他单元测试相同的 TestInitialize 和 TestCleanup 脚本，则选择此选项。  
  
### <a name="to-create-a-sql-server-unit-test-inside-a-new-test-class"></a>在新的测试类中创建 SQL Server 单元测试  
  
1.  在“测试”菜单上，单击“新建测试”。  
  
    此时将出现“添加新测试”对话框。  
  
2.  在“模板”下，单击“SQL Server 单元测试”。  
  
3.  在“测试名称”下面，输入测试的名称。  
  
4.  在“添加到测试项目”下面，选择一个要将此测试添加到的现有测试项目。 如果不存在测试项目或者你要创建新的测试项目，则选择“创建新的 <language> 测试项目”。  
  
5.  单击“确定”。  
  
    如果测试项目是新项目，将出现“新建测试项目”对话框。 给该项目命名，然后单击“确定”。  
  
    如果测试项目是新的或尚未配置，则将出现“SQL Server 测试配置” **<ProjectName>** 对话框。 可以使用此对话框为测试项目配置下列信息：  
  
    -   用于执行测试的数据库连接。  
  
    -   用于验证测试结果、部署数据库和生成数据的数据库连接。  
  
    -   在运行单元测试之前对某个给定的项目配置进行的数据库项目和任何相关的架构更改的自动部署。  
  
    有关详细信息，请参阅[操作说明：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)。  
  
6.  提供项目配置信息并单击“确定”。  
  
    \- 或 -  
  
    单击“取消”以创建单元测试而不配置测试项目。  
  
    空白测试将出现在“SQL Server 单元测试设计器”中。 根据指定的用于创建测试项目的语言的不同，将 Visual Basic 或 Visual C\# 源代码文件添加到测试项目中。 此文件包含 SQL Server Data Tools 为你刚创建的单元测试生成的 SQL Server 单元测试类。 此测试类可包含一个或多个可通过 SQL Server 单元测试设计器或代码作为测试类中的新测试方法进行添加的单元测试。  
  
    此外，还可以通过以下方式添加其他测试：  
  
    -   在“解决方案资源管理器”中右键单击一个测试项目，依次选择“添加”、“新建测试”和“SQL Server 单元测试”。  
  
    -   在 SQL Server 对象资源管理器中，选择“创建单元测试”。  
  
    当你在“解决方案资源管理器”中选择此文件时，默认情况下，它将显示在 SQL Server 单元测试设计器中。 若要查看代码或自定义此代码以便将更多功能添加到你的单元测试中，请选择此文件，右键单击并选择“查看代码”。  
  
### <a name="to-create-a-sql-server-unit-test-inside-an-existing-test-class"></a>要在现有测试类中创建新的 SQL Server 单元测试，请执行以下操作  
  
1.  在“SQL Server 单元测试设计器”中打开现有 SQL Server 单元测试类。 可以通过在“解决方案资源管理器”中双击单元测试源代码文件，访问“SQL Server 单元测试设计器”。  
  
2.  单击导航栏中的加号 (+) 符号以便显示“指定单元测试名称”对话框。  
  
3.  键入名称，然后单击“确定”。  
  
    新的 SQL Server 单元测试将会出现在导航栏中的下拉列表中。 它也会作为新的测试方法添加到测试类中。 若要查看代码中的测试方法，请选择类文件，右键单击并选择“查看代码”。 当前测试类文件的名称将显示在“SQL Server 单元测试设计器”顶部的选项卡中。  
  
在配置测试项目并创建单元测试之后，接下来的步骤是：  
  
-   添加 Transact\-SQL 测试脚本。  
  
-   定义预先测试和后期测试操作。  
  
-   添加测试条件或其他断言语句以验证脚本结果。  
  
> [!NOTE]  
> 无结论的测试条件是添加到每个测试中的默认条件。 包含此测试条件是为了指示尚未执行测试验证。 在您添加其他测试条件之后，请将此测试条件从测试中删除。 有关详细信息，请参阅[操作说明：向数据库单元测试添加测试条件](/previous-versions/visualstudio/visual-studio-2010/aa833242(v=vs.100))。  
  
## <a name="see-also"></a>另请参阅  
[如何：运行 SQL Server 单元测试](../ssdt/how-to-run-sql-server-unit-tests.md)  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[创建单元测试](/previous-versions/visualstudio/visual-studio-2008/ms182523(v=vs.90))  
