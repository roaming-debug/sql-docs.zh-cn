---
title: 创建和定义 SQL Server 单元测试
description: 了解 SQL Server 单元测试。 查看相关信息，了解如何创建和运行单元测试、排查问题以及其他相关任务。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 3c082177-a2b1-4fde-8833-b49b2a351815
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a6aab4b3ce10b59633d13ccadf3fb8ead67e375a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079608"
---
# <a name="creating-and-defining-sql-server-unit-tests"></a>创建和定义 SQL Server 单元测试

可运行 SQL Server 单元测试，以验证对架构中的一个或多个数据库对象进行的更改是否已对数据库应用程序中的现有功能产生了影响。 这些测试是对软件开发人员创建的单元测试的补充。 您必须运行这两种测试来验证应用程序的行为。  
  
可以通过添加 SQL Server 单元测试和添加 Transact\-SQL 脚本以测试该对象来验证架构中任何对象的行为。 此外，如果要验证特定函数、触发器或存储过程的行为，还可以自动生成 Transact\-SQL 脚本的存根。 在生成存根后，必须自定义它才能获得有意义的结果。  
  
> [!NOTE]  
> 可以创建一个空测试，向其中添加代码，然后运行它，而无需打开 SQL Server 数据库项目。 不过，如果没有打开包含要测试的对象的项目，则无法自动生成测试函数、触发器或存储过程的 Transact\-SQL 存根。  
  
## <a name="common-tasks"></a>常见任务  
在下表中，可以找到支持此方案的常规任务的说明，以及指向有关如何成功完成这些任务的详细信息的链接。  
  
|常见任务|支持内容|  
|----------------|----------------------|  
|进行动手实践：可以按照介绍性演练来熟悉如何创建和运行简单的 SQL Server 单元测试。|-   [演练：创建和运行 SQL Server 单元测试](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)|  
|**了解有关 SQL Server 单元测试的详细信息**：可以了解有关构成 SQL Server 单元测试的文件和脚本的详细信息。 还可以了解如何在单元测试中使用测试条件和 Transact\-SQL 断言。|-   [SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)<br />-   [SQL Server 单元测试文件](../ssdt/sql-server-unit-test-files.md)<br />-   [在 SQL Server 单元测试中使用测试条件](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)<br />-   [在 SQL Server 单元测试中使用 Transact-SQL 断言](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)|  
|**创建一个或多个测试项目**：必须在测试项目中创建 SQL Server 单元测试。 如果在创建测试项目前使用 SQL Server 对象资源管理器创建了 SQL Server 单元测试，将为你创建一个测试项目。 例如，如果您要在不同的测试组中使用不同的数据生成计划或不同的部署配置，可以创建多个测试项目。 在创建测试项目时，您可以配置用于该项目的测试设置（例如连接字符串）、部署设置和数据生成计划。|-   [如何：为 SQL Server 数据库单元测试创建测试项目](../ssdt/how-to-create-a-test-project-for-sql-server-database-unit-testing.md)<br />-|  
|配置单元测试的运行方式：可以指定被测数据库的连接字符串、数据生成计划和部署设置。 在首次将 SQL Server 单元测试添加到项目时配置这些设置，但是以后还可以修改它们。|-   [如何：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)<br />-   [连接字符串和权限概述](../ssdt/overview-of-connection-strings-and-permissions.md)|  
|创建 SQL Server 单元测试：可以为验证函数、触发器或存储过程行为的 SQL Server 单元测试自动创建 Transact\-SQL 代码存根。 还可以创建空 SQL Server 单元测试，然后添加 Transact\-SQL 代码以测试其他类型的数据库对象。|-   [如何：为函数、触发器和存储过程创建 SQL Server 单元测试](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)<br />-   [如何：创建空的 SQL Server 单元测试](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)|  
|为 SQL Server 单元测试编写代码：在创建单元测试后，需要修改或编写 Transact\-SQL 代码来测试数据库对象。 对于每个测试，需要定义一个或多个用于确定测试成功与否的测试条件。 对于较复杂的测试，可以修改数据库项目中的 Visual Basic 或 Visual C\# 代码。 例如，可以编写在单个事务范围内运行的单元测试。|-   [如何：打开 SQL Server 单元测试以进行编辑](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)<br />-   [如何：向 SQL Server 单元测试添加测试条件](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md)<br />-   [如何：编写在单个事务范围内运行的 SQL Server 单元测试](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md)<br />-   [SQL Server 单元测试设计器的键盘快捷键](../ssdt/keyboard-shortcuts-for-sql-server-unit-test-designer.md)|  
|**解决问题**：可以了解有关如何解决 SQL Server 常见问题的详细信息。|-   [解决 SQL Server 数据库单元测试问题](../ssdt/troubleshooting-sql-server-database-unit-testing-issues.md)|  
  
## <a name="related-scenarios"></a>相关方案  
[运行 SQL Server 单元测试](../ssdt/running-sql-server-unit-tests.md)  
在创建 SQL Server 单元测试后，可以从“测试视图”窗口、SQL Server 单元测试设计器或通过使用 Team Foundation Build 来运行这些测试。  
  
[场景：定义数据库单元测试的自定义测试条件](/previous-versions/visualstudio/visual-studio-2010/dd193282(v=vs.100))  
您可以创建自定义测试条件来测试默认测试条件无法验证的行为。  
  
## <a name="see-also"></a>另请参阅  
[使用 SQL Server 单元测试验证数据库代码](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
