---
title: 向 SQL Server 单元测试添加测试条件
description: 了解如何向 SQL Server 单元添加测试条件。 查看如何为此任务使用 SQL Server 单元测试设计器。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 85ba2e56-a0b2-489c-aea2-fb135cce0cfc
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9920430615d8391ef069078272ed8abbfcef6a00
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063592"
---
# <a name="how-to-add-test-conditions-to-sql-server-unit-tests"></a>如何：向 SQL Server 单元测试添加测试条件

可以使用“SQL Server单元测试设计器”将测试条件添加到 SQL Server 单元测试中。 在保存测试类时，测试条件会自动保存在测试项目中，保存形式为包含测试类的源代码文件中的 Visual C\# 或 Visual Basic 代码。 在保存测试条件后，可以在“SQL Server 单元测试设计器”中或在其源代码文件中编辑测试条件。  
  
### <a name="to-add-test-conditions-to-a-sql-server-unit-test"></a>向 SQL Server 单元测试添加测试条件  
  
1.  在“SQL Server 单元测试设计器”中打开 SQL Server 单元测试。  
  
    打开的测试的名称将显示在 SQL Server 单元测试设计器顶部的导航栏中。 通过使用导航栏，您可以选择测试类中的不同测试方法。  
  
2.  在导航栏中，单击要向其中添加测试条件的测试方法，或单击“公用脚本”。  
  
    > [!NOTE]  
    > 公用脚本不属于特定单元测试。 它们位于测试类中的单元测试之前或之后。 有关详细信息，请参阅 [SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)。  
  
3.  在导航栏中，单击要向其中添加测试条件的 Transact\-SQL 脚本。 您可以将测试条件添加到预先测试、测试或后期测试脚本中。  
  
    该测试的 \- 脚本将显示在 Transact\-SQL 编辑器中，并且其测试条件将显示在“测试条件”窗格中。  
  
4.  在“测试条件”选择列表中，单击测试条件，然后单击“添加测试条件”(+)。  
  
    测试条件将添加到单元测试方法中。  
  
    > [!NOTE]  
    > 通过单击测试条件，然后单击“测试条件”窗格中的向上和向下箭头，可以对测试方法中的测试条件重新排序。  
  
5.  选择刚添加的测试条件并查看“属性”窗口。  
  
    在“属性”窗口中配置测试条件。 例如，可以更改“执行时间”测试条件的“执行时间”属性。 在设置此属性后，如果 Transact\-SQL 脚本没有在指定的时间内执行，则测试将失败。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[如何：创建空的 SQL Server 单元测试](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
[如何：为函数、触发器和存储过程创建 SQL Server 单元测试](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)  
[在 SQL Server 单元测试中使用测试条件](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
[SQL Server 单元测试中的脚本](../ssdt/scripts-in-sql-server-unit-tests.md)  
[解释 SQL Server 单元测试结果](../ssdt/interpreting-sql-server-unit-test-results.md)  
[如何：运行 SQL Server 单元测试](../ssdt/how-to-run-sql-server-unit-tests.md)  
  
