---
title: 使用 SQL Server 单元测试验证数据库代码
description: 了解 SQL Server 单元测试。 查看哪些 Visual Studio 版本提供测试支持，并在使用测试来验证数据库代码时查看相关资源。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 003713e2-de6b-4277-a0a8-7d1f2f4ffb39
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 4c05b9e3a3330b395ee3cd8c2cb3da5aa7d50508
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066695"
---
# <a name="verifying-database-code-by-using-sql-server-unit-tests"></a>使用 SQL Server 单元测试验证数据库代码

你可以使用 SQL Server 单元测试来确定数据库的基准状态，然后验证对数据库对象进行的任何后续更改。  
  
为了确定数据库的基准状态，创建一个测试项目并编写针对数据库对象运行的一组 Transact\-SQL。 通过使用这些测试，无论这些对象是否按预期运行，你都可以在隔离的开发环境中进行验证。 SQL Server 单元测试与使用 SQL Server 数据库项目的脱机数据库开发配合使用时效果良好（有关详细信息，请参阅[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)）。 有了一组基准 SQL Server 单元测试后，在将更改信息签入版本控制前可以使用这些测试来验证数据库是否正常工作。  
  
可以创建验证对任何数据库对象所做更改的测试。 另外，还可以自动生成用于测试数据库函数、触发器和存储过程的 Transact\-SQL 代码的存根。  
  
> [!NOTE]  
> 可以创建和运行 SQL Server 单元测试，而无需打开数据库项目。 不过，如果您要自动生成测试脚本以测试项目中的特定数据库对象，则必须打开包含要测试对象的数据库项目。  
  
在您或您的团队成员更改数据库架构时，您可以使用这些测试来验证这些更改是否影响现有功能。 可以创建 SQL Server 单元测试来对软件开发人员创建的软件单元测试进行补充。 您必须完成这两组测试才能验证应用程序的整体行为。  
  
单元测试可以验证过程是否按照预期成功以及是否按照预期失败。 测试相应的失败是否发生也称为负面测试。  
  
## <a name="visual-studio-editions-support-for-sql-server-unit-tests"></a>Visual Studio 版本对 SQL Server 单元测试的支持  
SQL Server 单元测试功能（在 SQL Server Data Tools 的 2012 年 12 月更新中添加）允许你在 Visual Studio 2010 Professional 和 Visual Studio 2012 Professional 及更高版本中创建、修改和运行 SQL Server 单元测试。  
  
要确保安装最新的 SQL Server Data Tools 更新，请访问[“检查更新”对话框](../ssdt/check-for-updates-dialog-box.md)。  
  
集成了 SQL Server Data Tools shell 的 Visual Studio 2010 和 Visual Studio 2012 不支持 SQL Server 单元测试。  
  
## <a name="common-tasks"></a>常见任务  
在下表中，可以找到支持此方案的常规任务的说明，以及指向有关如何成功完成这些任务的详细信息的链接。  
  
|常见任务|支持内容|  
|----------------|----------------------|  
|**进行动手实践：** 可以按照介绍性演练来熟悉如何创建和运行简单的 SQL Server 单元测试。 此演练包括一个负面 SQL Server 单元测试示例。|[演练：创建和运行 SQL Server 单元测试](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)|  
|**定义 SQL Server 单元测试：** 必须在单元测试的项目中创建 SQL Server 单元测试。 可以为该项目配置设置并为每个测试定义一个或多个测试条件。|[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)<br /><br />[在 SQL Server 单元测试中使用测试条件](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)|  
|运行 SQL Server 单元测试：在定义一个或多个单元测试后，可以运行这些测试、调试所有问题以及检查测试结果。|[运行 SQL Server 单元测试](../ssdt/running-sql-server-unit-tests.md)|  
|**管理测试组 (Visual Studio 2010)：** 如果要同时运行多个测试，可以将它们划分到多个组中。 测试列表仍受支持，但对于新测试组，您应该考虑使用测试类别。 例如，你可以为针对触发器或特定架构中的所有对象的测试创建测试类别。|[定义测试类别以对测试进行分组](/previous-versions/visualstudio/visual-studio-2010/dd286595(v=vs.100))<br /><br />[定义测试列表以对测试进行分组](/previous-versions/visualstudio/visual-studio-2010/dd286584(v=vs.100))|  
|将测试项目和测试签入版本控制中：在运行测试并验证它们是否正常运行之后，应将测试项目和所有关联文件签入版本控制中，以便你的所有团队成员都可以运行这些测试。 通过将测试项目与 SQL Server 数据库项目一起签入版本控制，你可以轻松还原数据库和数据库测试的兼容版本。|[将文件添加到版本控制](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100))<br /><br />[使用“签入”和“挂起的更改”窗口](/previous-versions/visualstudio/visual-studio-2010/ms245462(v=vs.100))|  
|定义自定义测试条件：如果必须测试默认测试条件组没有包括的行为，则可以创建自定义测试条件。 您必须将这些条件分发给要运行使用新条件的测试的所有团队成员。|[场景：定义 SQL Server 单元测试的自定义测试条件](/previous-versions/visualstudio/visual-studio-2010/dd193282(v=vs.100))|  
|更新现有单元测试：如果你有一些在以前版本的 Visual Studio 中创建的数据库单元测试，则必须升级这些测试，然后它们才能在此版本中成功生成和运行。<br /><br />注意：如果打开的解决方案同时包含以前版本的 Visual Studio 中的数据库项目和数据库单元测试项目，则系统会提示你升级数据库项目。 系统不会提示您升级数据库单元测试项目，它必须手动升级。|[升级包含数据库单元测试的较旧的测试项目](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md)|  
|**可扩展性：** 可以通过创建功能扩展来扩展 SQL Server Data Tools。|[SQL Server 单元测试的自定义测试条件](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)|  
|**解决问题：** 可以了解有关如何解决 SQL Server 单元测试常见问题的详细信息。|[解决 SQL Server 数据库单元测试问题](../ssdt/troubleshooting-sql-server-database-unit-testing-issues.md)|  
  
## <a name="related-scenarios"></a>相关方案  
[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)  
与使用 SQL Server 数据库项目的脱机项目开发一起使用时，数据库单元测试特别有效。  
  
## <a name="see-also"></a>另请参阅  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
