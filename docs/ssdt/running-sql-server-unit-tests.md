---
title: 运行 SQL Server 单元测试
description: 熟悉 SQL Server 单元测试。 查看有关创建测试、创建自定义测试条件、运行测试及解释结果的资源。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconfig
ms.assetid: febcc87f-eb18-4c12-ba30-82ef0d49aaa3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5983d00806f895d8112ac111312309cf93aa48ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063492"
---
# <a name="running-sql-server-unit-tests"></a>运行 SQL Server 单元测试

若要改进和保持代码质量，你可创建并运行验证任何数据库对象行为的 SQL Server 单元测试，然后将这些测试签入到版本控制中。 当你或你团队的任何成员更改数据库架构时，运行 SQL Server 单元测试和软件单元测试来验证这些更改是否破坏了现有功能。 您可运行单独的测试，也可运行测试组（称为“测试列表”）。 有关详细信息，请参阅[使用测试列表 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182461(v=vs.100))。  
  
## <a name="ways-to-run-sql-server-unit-tests"></a>运行 SQL Server 单元测试的方式  
可根据安装的软件通过若干不同的方式运行 SQL Server 单元测试，如下所示：  
  
-   使用 Visual Studio 2010“测试视图”窗口运行测试。 有关详细信息，请参阅[操作说明：运行 SQL Server 单元测试](../ssdt/how-to-run-sql-server-unit-tests.md)和[如何：从 Microsoft Visual Studio 2010 运行自动测试](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100))。 对于 Visual Studio 2012，请参阅[如何：从 Microsoft Visual Studio 2012 运行自动测试](/previous-versions/ms182470(v=vs.140))。  
  
-   在命令提示符处使用 MSTest.exe 命令运行测试。 有关详细信息，请参阅[如何：使用 MSTest 从命令行运行自动测试 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182487(v=vs.100)) 或[如何：使用 MSTest 从命令行运行自动测试 (Visual Studio 2012)](/previous-versions/ms182487(v=vs.140))。  
  
-   通过运行测试项目从“解决方案资源管理器”运行测试。 有关详细信息，请参阅[如何：从 Microsoft Visual Studio 2010 运行自动测试](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100))或[如何：从 Microsoft Visual Studio 2012 运行自动测试](/previous-versions/ms182470(v=vs.140))。  
  
-   从“测试结果”窗口重新运行测试。 有关详细信息，请参阅[操作说明：重新运行测试 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182472(v=vs.100))。  
  
-   从“测试列表编辑器”窗口运行单独的测试或测试列表 (Visual Studio 2010)。 有关详细信息，请参阅[如何：从 Microsoft Visual Studio 2010 运行自动测试](/previous-versions/visualstudio/visual-studio-2010/ms182470(v=vs.100))或[如何：从 Microsoft Visual Studio 2012 运行自动测试](/previous-versions/ms182470(v=vs.140))。  
  
-   在 Team Foundation Build 中生成项目时运行测试。 有关详细信息，请参阅[如何：在生成应用程序后配置和运行计划的测试 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182465(v=vs.100)) 或[如何：在生成应用程序后配置和运行计划的测试 (Visual Studio 2012)](/previous-versions/visualstudio/visual-studio-2012/ms182465(v=vs.110))。  
  
你可使用顺序测试来按特定顺序运行 SQL Server 单元测试。 有关详细信息，请参阅[操作说明：创建顺序测试 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182631(v=vs.100)) 或[如何：创建顺序测试 (Visual Studio 2012)](/previous-versions/ms182631(v=vs.140))。  
  
## <a name="interpreting-tests-results"></a>解释测试结果  
运行测试后，“测试结果”窗口将显示哪些测试已通过或失败。 有关详细信息，请参阅[解释 SQL Server 单元测试结果](../ssdt/interpreting-sql-server-unit-test-results.md)。 有关如何诊断意外失败的更多信息，请参阅[如何：调试数据库对象](../ssdt/how-to-debug-database-objects.md)。  
  
## <a name="topics-in-this-section"></a>本节主题  
本节包含下列主题：  
  
-   [如何：调试数据库对象](../ssdt/how-to-debug-database-objects.md)  
  
-   [如何：从 Team Foundation Build 运行 SQL Server 单元测试](../ssdt/how-to-run-sql-server-unit-tests-from-team-foundation-build.md)  
  
-   [如何：运行 SQL Server 单元测试](../ssdt/how-to-run-sql-server-unit-tests.md)  
  
-   [解释 SQL Server 单元测试结果](../ssdt/interpreting-sql-server-unit-test-results.md)  
  
## <a name="related-scenarios"></a>相关方案  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
您可定义单元测试来验证数据库对象的行为并将每个测试项目与不同的数据生成计划、部署配置和连接字符串相关联。  
  
[SQL Server 单元测试的自定义测试条件](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
对于您无法使用默认测试条件进行验证的任何条件，您可创建自定义的测试条件进行测试。  
  
## <a name="see-also"></a>另请参阅  
[使用 SQL Server 单元测试验证数据库代码](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
