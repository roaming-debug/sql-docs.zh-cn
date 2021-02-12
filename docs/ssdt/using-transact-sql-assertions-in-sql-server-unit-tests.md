---
title: 在 SQL Server 单元测试中使用 Transact-SQL 断言
description: 了解 Transact-SQL 断言。 了解何时在 SQL Server 单元测试中使用断言，以及何时使用测试条件并查看断言用法的示例。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 55d8be9c-9282-47d3-be7f-e2c26f00c95e
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 23dff8f246d741896a042486f0dbe6c95067a8d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066758"
---
# <a name="using-transact-sql-assertions-in-sql-server-unit-tests"></a>在 SQL Server 单元测试中使用 Transact-SQL 断言

在 SQL Server 单元测试中，Transact\-SQL 测试脚本将运行并返回结果。 有时，结果以结果集的形式返回。 您可以使用测试条件来验证结果。 例如，您可以使用测试条件来检查特定结果集中返回的行数，或验证特定测试的运行时长。 有关测试条件的详细信息，请参阅[在 SQL Server 单元测试中使用测试条件](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)。  
  
除了使用测试条件之外，还可以使用 Transact\-SQL 断言，也就是 Transact\-SQL 脚本中的 THROW 或 RAISERROR 语句。 在某些情况下，你可能更愿意使用 Transact\-SQL 断言，而不是测试条件。  
  
## <a name="using-transact-sql-assertions"></a>使用 Transact-SQL 断言  
在决定是使用 Transact\-SQL 断言还是测试条件来验证数据之前，应考虑以下几点。  
  
-   性能。 与首先将数据移动到客户端计算机并在本地处理这些数据相比，在服务器上运行 Transact\-SQL 断言的速度要更快。  
  
-   语言熟悉程度。 你可能更愿意根据你当前的专业知识来选择具体语言，因此选择 Transact\-SQL 断言、Visual C\# 或 Visual Basic 测试条件。  
  
-   复杂验证。 在某些情况下，你可以在 Visual C\# 或 Visual Basic 中生成更复杂的测试，并在客户端上验证测试。  
  
-   简单性。 使用预先定义的测试条件通常比在 Transact\-SQL 中撰写等效脚本更为简单。  
  
-   旧验证库。 如果已有执行验证的代码，则可以将其用于 SQL Server 单元测试，而不使用测试条件。  
  
## <a name="mark-unit-test-methods-with-the-expected-exception"></a>使用预期异常标记单元测试方法  
若要用预期异常来标记 SQL Server 单元测试方法，请添加以下属性：  
  
```vb  
<ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)> _  
```  
  
```csharp  
[ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)]  
```  
  
其中：  
  
-   *nnnnn* 是预期消息的编号，例如 14025  
  
-   *x* 是预期异常的严重级别  
  
-   *y* 是预期异常的状态  
  
将忽略任何未指定的参数。 将这些参数传递给您的数据库代码中的 RAISERROR 语句。 如果指定 MatchFirstError = false，则此属性将与异常中的任何 SqlErrors 匹配。 默认行为 (MatchFirstError = true) 是指仅与出现的第一个错误匹配。  
  
有关如何使用预期异常和负 SQL Server 单元测试的示例，请参阅[演练：创建和运行 SQL Server 单元测试](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md)。  
  
## <a name="the-raiserror-statement"></a>RAISERROR 语句  
  
> [!NOTE]  
> 使用 THROW 来代替 RAISERROR。 现在不推荐使用 RAISERROR。  
  
可以通过在 Transact\-SQL 脚本中使用 RAISERROR 语句来在服务器上直接使用 Transact\-SQL 断言。 其语法为：  
  
**RAISERROR (\@ErrorMessage, \@ErrorSeverity, \@ErrorState)**  
  
其中：  
  
@ErrorMessage 任何用户定义的错误消息。 可以设置此消息字符串的格式，使其类似于 printf_s 函数。  
  
@ErrorSeverity 是用户定义的严重级别（从 0 到 18）。  
  
> [!NOTE]  
> 严重级别值为“0”和“10”时不会导致 SQL Server 单元测试失败。 可以使用范围 0 - 18 中的任何其他值使测试失败。  
  
@ErrorState 是介于 1 和 127 之间的任一整数。 您可以使用此整数来区分在代码中的不同位置引发的同一个错误。  
  
有关详细信息，请参见 [RAISERROR (Transact-SQL)](../t-sql/language-elements/raiserror-transact-sql.md)。 在 SQL Server 单元测试中使用 RAISERROR 的示例在主题[如何：编写在单个事务范围内运行的 SQL Server 单元测试](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md)中提供。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[在 SQL Server 单元测试中使用测试条件](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
[使用 SQL Server 单元测试验证数据库代码](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[如何：打开 SQL Server 单元测试以进行编辑](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)  
