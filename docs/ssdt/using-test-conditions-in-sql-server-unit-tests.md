---
title: 在 SQL Server 单元测试中使用测试条件
description: 了解 SQL Server 单元测试中的测试条件。 请参阅如何使用预定义条件和负测试，并查看有关自定义条件的信息。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconditions
ms.assetid: e3d1c86c-1e58-4d2c-b625-d1b591b221aa
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 18021d85c9b490664411b7bb98f1b55972a13022
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082408"
---
# <a name="using-test-conditions-in-sql-server-unit-tests"></a>在 SQL Server 单元测试中使用测试条件

在 SQL Server 单元测试中，将执行一个或多个 Transact\-SQL 测试脚本。 可以在 Transact\-SQL 脚本内对结果进行评估，并且使用 THROW 或 RAISERROR 来返回错误和使测试失败，或者可以在测试中对测试条件进行定义以便评估结果。 该测试返回 [SqlExecutionResult](/previous-versions/sql/sql-server-data-tools/jj856590(v=vs.103)) 类的实例。 此类的实例包含一个或多个数据集、执行时间和受脚本影响的行。 所有这些信息都是在脚本的执行过程中收集的。 这些结果可使用测试条件进行评估。 SQL Server Data Tools 提供一组预定义的测试条件。 还可以创建和使用自定义条件；请参阅 [SQL Server 单元测试的自定义测试条件](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)。  
  
## <a name="predefined-test-conditions"></a>预定义的测试条件  
下表列出了可以使用 SQL Server 单元测试设计器中的“测试条件”窗格添加的预定义测试条件。  
  
|**测试条件**|**测试条件说明**|  
|----------------------|----------------------------------|  
|数据校验和|如果从 Transact\-SQL 脚本返回的结果集的校验和与预期的校验和不一致，则将失败。 有关更多信息，请参见 [指定数据校验和](#SpecifyDataChecksum)。<br /><br />**注意：** 如果要返回的数据在不同的测试运行之间是不同的，则不建议使用此测试条件。 例如，如果结果集包含生成日期或时间或者包含标识列，则测试将失败，因为每次运行的校验和都会不同。|  
|空结果集|如果从 Transact\-SQL 脚本返回的结果集不为空，则将失败。|  
|执行时间|如果 Transact\-SQL 测试脚本所用时间长于预期执行时间，则将失败。 默认执行时间为 30 秒。<br /><br />执行时间仅适用于测试脚本的测试，而不适用于预先测试脚本或后期测试脚本。|  
|预期架构|如果结果集的列和数据类型与为测试条件指定的不匹配，则将失败。 您必须通过测试条件的属性指定架构。 有关更多信息，请参见 [指定预期架构](#SpecifyExpectedSchema)。|  
|无结论|始终产生结果为“无结论”的测试。 这是向每个测试添加的默认条件。 包含此测试条件是为了指示尚未执行测试验证。 在您添加其他测试条件之后，请将此测试条件从测试中删除。|  
|非空结果集|如果结果集为空，则将失败。 可在测试脚本中将此测试条件或 EmptyResultSet 与 Transact\-SQL @@RAISERROR 函数配合使用，以测试更新是否工作正常。 例如，您可保存更新前的值，运行更新，比较更新后的值，并在未获得预期结果的情况下引发错误。|  
|行计数|如果结果集未包含预期的行数，则将失败。|  
|标量值|如果结果集中的某个特定值不等于指定值，则将失败。 默认“预期值”  为 null。|  
  
> [!NOTE]  
> 执行时间测试条件指定运行 Transact\-SQL 测试脚本的时间限制。 如果超过此时间限制，则测试将失败。 测试结果还包括持续时间统计信息，这不同于执行时间测试条件。 持续时间统计信息不仅包含执行时间，还包含两次连接到数据库的时间；运行任何其他测试脚本（如预先测试脚本和后期测试脚本）的时间；以及运行测试条件的时间。 因此，即使测试的持续时间超过其执行时间，测试也可通过。  
>   
> 报告的持续时间不包括用于数据生成和架构部署的时间，因为它们发生在测试运行之前。 若要查看测试持续时间，请在“测试结果”窗口中选择测试运行，右键单击并选择“查看测试结果详细信息”。  
  
可使用 SQL Server 单元测试设计器的“测试条件”窗格向 SQL Server 单元测试添加测试条件。 有关详细信息，请参阅[操作说明：向 SQL Server 单元测试添加测试条件](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md)。  
  
您还可直接编辑测试方法代码以添加更多功能。 有关详细信息，请参阅[如何：打开 SQL Server 单元测试以进行编辑](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)和[如何：编写在单个事务范围内运行的 SQL Server 单元测试](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md)。 例如，您可通过添加 Assert 语句向测试方法添加功能。 有关详细信息，请参阅[在 SQL Server 单元测试中使用 Transact-SQL 断言](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)。  
  
## <a name="expected-failures"></a>预期的失败  
可创建 SQL Server 单元测试以测试应该不会成功的行为。 这些预期的失败有时称为“负测试”。 一些示例包括：  
  
-   验证删除客户数据的存储过程在您指定无效客户 ID 的情况下是否会失败。  
  
-   验证填写订单的存储过程在某个订单从未订立或者已经填写的情况下是否会失败。  
  
-   验证取消订单的存储过程是否无法取消已完成或已取消的订单。  
  
可为引发预期异常的存储过程定义 SQL Server 单元测试。 您可向单元测试方法添加一个属性以指示应出现哪些异常。 通过执行此操作，您可防止在出现异常时测试失败。  
  
若要用预期异常来标记 SQL Server 单元测试方法，请添加以下属性：  
  
```  
[ExpectedSqlException(MessageNumber = nnnnn, Severity = x, MatchFirstError = false, State = y)]  
```  
  
其中：  
  
-   *nnnnn* 是预期消息的编号，例如 14025  
  
-   *x* 是预期异常的严重级别  
  
-   *y* 是预期异常的状态  
  
将忽略任何未指定的参数。 将这些参数传递给您的数据库代码中的 **THROW** 语句。 如果您指定 MatchFirstError = false，则此属性将与异常中的任何 SqlErrors 匹配。 默认行为 (MatchFirstError = true) 是指仅与出现的第一个错误匹配。  
  
有关如何使用预期异常和负 SQL Server 单元测试的示例，请参阅[演练：创建和运行 SQL Server 单元测试](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)。  
  
## <a name="specifying-a-data-checksum"></a><a name="SpecifyDataChecksum"></a>指定数据校验和  
若要显示 SQL Server 单元测试设计器，可以在 “解决方案资源管理器”中双击单元测试源代码文件。  
  
向您的数据库单元测试添加“数据校验和”测试条件之后，您必须使用以下过程配置所需校验和：  
  
#### <a name="to-specify-an-expected-checksum"></a>指定所需校验和  
  
1.  在测试条件列表中，单击要为其指定校验和的“数据校验和”测试条件。  
  
2.  按 F4 打开“属性”  窗口。 还可打开“视图”  菜单，然后单击“属性”  窗口。  
  
3.  （可选）你可能需要更改测试条件的“(名称)”属性以使其更具描述性。  
  
4.  在“配置”属性中，单击浏览 (…) 按钮 。  
  
    此时将显示“TestConditionName 的配置”  对话框。  
  
5.  指定与要测试的数据库的连接。 有关详细信息，请参阅[操作说明：创建数据库连接](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100))。  
  
6.  默认情况下，测试的 Transact\-SQL 正文将出现在编辑窗格中。 您可修改代码（如有必要）以产生预期的结果。 例如，如果您的测试具有预先测试代码，则您可能必须添加该代码。  
  
    > [!IMPORTANT]  
    > 如果您修改了之前已指定了校验和的“校验和”条件，则不会保存您在编辑窗格中做出的任何更改。 在单击“检索” 之前，您必须再次进行这些更改。  
  
7.  单击“检索” 。  
  
    将对指定的数据库连接执行 Transact\-SQL，结果将显示在对话框中。  
  
8.  如果结果与测试的预期结果匹配，则单击“确定” 。 否则，请修改 Transact\-SQL 正文并重复步骤 6、7 和 8，直至结果与预期相符。  
  
    测试条件的“值”  列将显示预期校验和的值。  
  
## <a name="specifying-an-expected-schema"></a><a name="SpecifyExpectedSchema"></a>指定预期架构  
向 SQL Server 单元测试添加“预期架构”测试条件之后，必须使用以下过程配置预期架构：  
  
#### <a name="to-specify-an-expected-schema"></a>指定预期架构  
  
1.  在测试条件列表中，单击要为其指定架构的“预期架构”测试条件。  
  
2.  按 F4 打开“属性”  窗口。 还可打开“视图”  菜单，然后单击“属性”  窗口。  
  
3.  （可选）你可能需要更改测试条件的“(名称)”属性以使其更具描述性。  
  
4.  在“配置”属性中，单击浏览 (…) 按钮 。  
  
    此时将显示“TestConditionName 的配置”  对话框。  
  
5.  指定与要测试的数据库的连接。 有关详细信息，请参阅[操作说明：创建数据库连接](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100))。  
  
6.  默认情况下，测试的 Transact\-SQL 正文将出现在编辑窗格中。 您可修改代码（如有必要）以产生预期的结果。 例如，如果您的测试具有预先测试代码，则您可能必须添加该代码。  
  
    > [!IMPORTANT]  
    > 如果您修改了之前已指定了架构的“预期架构”条件，则不会保存您在编辑窗格中做出的任何更改。 在单击“检索” 之前，您必须再次进行这些更改。  
  
7.  单击“检索” 。  
  
    将对指定的数据库连接执行 Transact\-SQL，结果将显示在对话框中。 因为您要验证结果集的架构或形状而不是结果的值，因此不必查看返回的结果中的任何数据，只要各个列以您希望显示的方式出现即可。  
  
8.  如果结果与测试的预期结果匹配，则单击“确定” 。 否则，请修改 Transact\-SQL 正文并重复步骤 6、7 和 8，直至结果与预期相符。  
  
    测试条件的“值”  列将显示有关预期架构的信息。 例如，它可能显示“应为：2 个表”。  
  
## <a name="extensible-test-conditions"></a>可扩展的测试条件  
除了六个预定义的测试条件之外，您还可编写您自己的新测试条件。 这些测试条件将显示在 SQL Server 单元测试设计器的“测试条件”窗格中。 有关详细信息，请参阅 [SQL Server 单元测试的自定义测试条件](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[在 SQL Server 单元测试中使用 Transact-SQL 断言](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)  
[SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)  
