---
description: Integration Services (SSIS) 变量
title: Integration Services (SSIS) 变量 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- variables [Integration Services], passing between packages
- user-defined variables [Integration Services]
- scope [Integration Services]
- system variables [Integration Services]
- variables [Integration Services]
- variables [Integration Services], about variables
- values [Integration Services]
ms.assetid: c1e81ad6-628b-46d4-9b09-d2866517b6ca
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1dfeeecf62ad33ab5d2d66e0fdf454f89036d047
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193820"
---
# <a name="integration-services-ssis-variables"></a>Integration Services (SSIS) 变量

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  变量存储 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包及其容器、任务和事件处理程序在运行时可以使用的值。 脚本任务和脚本组件中的脚本也可以使用变量。 将任务和容器按顺序组织为工作流的优先约束在其约束定义包含表达式时可以使用变量。  
  
 可以将 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包中的变量用于下列目的：  
  
-   在运行时更新包元素的属性。 例如，可以动态设置 Foreach 循环容器所允许的并发可执行文件数。  
  
-   包含内存中的查找表。 例如，包可以运行加载带数据值的变量的执行 SQL 任务。  
  
-   加载带数据值的变量，然后使用这些变量指定 WHERE 子句中的搜索条件。 例如，脚本任务中的脚本可以更新执行 SQL 任务中的 Transact-SQL 语句所使用的变量的值。  
  
-   加载带整数的变量，然后使用该值控制包控制流中的循环。 例如，可以在 For 循环容器的计算表达式中使用变量来控制迭代。  
  
-   在运行时填充 Transact-SQL 语句的参数值。 例如，包可以运行执行 SQL 任务，然后使用变量动态设置 Transact-SQL 语句中的参数。  
  
-   生成包含变量值的表达式。 例如，派生列转换可以用变量值乘以列值所得结果来填充列。  
  
## <a name="system-and-user-defined-variables"></a>系统变量和用户定义变量  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 支持两种类型的变量：用户定义变量和系统变量。 用户定义变量由包开发人员定义，系统变量由 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]定义。 可以创建包所需数量的用户定义变量，但不能另外创建系统变量。  
  
 在执行 SQL 任务用来在 SQL 语句中将变量映射到参数的参数绑定中，可以使用所有变量（系统和用户定义）。 有关详细信息，请参阅 [执行 SQL 任务](../integration-services/control-flow/execute-sql-task.md) 和 [执行 SQL 任务中的参数和返回代码](./control-flow/execute-sql-task.md)。  
  
> [!NOTE]  
>  用户定义变量和系统变量的名称是区分大小写的。  
  
 可以为所有 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 容器类型创建用户定义变量，这些容器类型包括包、Foreach 循环容器、For 循环容器、序列容器、任务以及事件处理程序。 用户定义变量是容器中 Variables 集合的成员。  
  
 使用 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器创建包时， **设计器** “包资源管理器” **选项卡上的** “变量” [!INCLUDE[ssIS](../includes/ssis-md.md)] 文件夹中可以看到 Variables 集合的成员。 这些文件夹列出了用户定义变量和系统变量。  
  
 可以使用下列方式配置用户定义变量：  
  
-   提供变量的名称和说明。  
  
-   指定变量的命名空间。  
  
-   指示变量是否在其值更改时引发事件。  
  
-   指示变量是只读还是读/写。  
  
-   使用表达式的计算结果设置变量值。  
  
-   在包或包对象（如任务）的范围内创建变量。  
  
-   指定变量的值和数据类型。  
  
 对系统变量唯一可配置的选项是指定变量在更改值时是否引发事件。  
  
 不同的容器类型有一组不同的系统变量可用。 有关包及其元素所使用的系统变量的详细信息，请参阅 [System Variables](../integration-services/system-variables.md)。  
  
 有关变量的实际使用情况的详细信息，请参阅 [在包中使用变量]()。  
  
## <a name="properties-of-variables"></a>变量属性  
 你可通过在“变量”窗口或“属性”窗口中设置以下属性来配置用户定义变量。 某些属性仅在“属性”窗口中提供。  
  
> [!NOTE]  
>  对系统变量唯一可配置的选项是指定变量在更改值时是否引发事件。  
  
 **Description**    
 指定对变量的描述。  
  
 **EvaluateAsExpression**    
 将此属性设置为 **True** 时，所提供的表达式可用于设置变量值。  
  
 **表达式**    
 指定分配给该变量的表达式。  
  
 **名称**    
 指定变量名称。  
  
 **Namespace**  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 提供了两个命名空间：User 和 System。 默认情况下，自定义变量位于 **User** 命名空间中，系统变量位于 **System** 命名空间中。 你可以为用户定义变量创建其他命名空间，并可以更改 **User** 命名空间的名称，但不能更改 **System** 命名空间的名称，也不能向 **System** 命名空间添加变量或将系统变量分配给其他命名空间。  
  
**RaiseChangedEvent**  
 将此属性设置为 **True** 时，变量值的改变将会引发 **OnVariableValueChanged** 事件。  
  
 **ReadOnly**  
 将此属性设置为 **False** 时，该变量可读\写。  
  
**范围**    
 > [!NOTE]  
>   可通过单击 **“变量”** 窗口中的 **“移动变量”** 来更改此属性设置。  
  
 变量在包的作用域内或者包中的容器、任务或事件处理程序的作用域内创建。 因为包容器位于容器层次结构的顶部，所以包作用域内的变量所起作用类似于全局变量，而且这些变量可由包中所有容器使用。 同样，在 For 循环容器等容器的范围内定义的变量可由 For 循环容器中所有任务或容器使用。  
  
 如果包使用执行包任务运行其他包，则可以使用父包变量配置类型，使在调用的包或执行包任务范围内定义的变量可供被调用的包使用。 有关详细信息，请参阅 [Package Configurations](./packages/legacy-package-deployment-ssis.md)。  
  
**IncludeInDebugDump**  
 指示调试转储文件中是否包括变量值。  
  
 对于用户定义变量和系统变量， **InclueInDebugDump** 选项的默认值为 **true**。  
  
 但是，对于用户定义变量，在满足以下条件时系统会将 **IncludeInDebugDump** 选项重置为 **false** ：  
  
-   如果 **EvaluateAsExpression** 变量属性设置为 **true**，则系统将 **IncludeInDebugDump** 选项重置为 **false**。  
  
     若要在调试转储文件中包括表达式文本作为变量值，请将 **IncludeInDebugDump** 选项设置为 **true**。  
  
-   如果变量数据类型更改为字符串，则系统将 **IncludeInDebugDump** 选项重置为 **false**。  
  
 在系统将 **“IncludeInDebugDump”** 选项重置为 **false** 时，该设置可能会覆盖用户选择的值。  
  
**“值”**     
用户定义变量的值可以是文字或表达式。 变量值不能为 null。 变量具有以下默认值：

| 数据类型 | 默认值 |
|---|---|
| 布尔 | False |
| 数字和二进制数据类型 | 0（零） |
| 字符型和字符串数据类型 | (空字符串) |
| Object | System.Object |
| | |

变量包含设置变量值和值的数据类型的选项。 这两种属性必须兼容：例如，字符串值不能与整数数据类型一起使用。  
  
 如果将变量配置为作为表达式进行计算，则必须提供表达式。 在运行时计算表达式，而该变量将设置为计算结果。 例如，如果变量使用表达式 `DATEPART("month", GETDATE())` ，则变量的值与当前日期月份部分的数字相同。 表达式必须是使用 [!INCLUDE[ssIS](../includes/ssis-md.md)] 表达式语法的有效表达式。 表达式和变量一起使用时，表达式可以使用文字以及表达式语法所提供的运算符和函数，但表达式不能引用包中数据流的列。 表达式的最大长度为 4000 个字符。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
**ValueType**    
 > [!NOTE]  
>   此属性值出现在 **“变量”** 窗口中的 **“数据类型”** 列中。  
  
 指定变量值的数据类型。  

## <a name="scenarios-for-using-variables"></a>变量使用方案  
 变量在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包中有多种不同的使用方式。 您会发现，如果不向包中添加用户定义的变量以实现解决方案所要求的灵活性和可管理性，包的开发将无法进行下去。 根据方案的不同，通常还会用到系统变量。  
  
 **属性表达式** ：在设置包和包对象属性的属性表达式中使用变量来提供值。 例如，表达式 `SELECT * FROM @varTableName` 中包含变量 `varTableName` ，该变量可更新“执行 SQL 任务”所运行的 SQL 语句。 表达式 `DATEPART("d", GETDATE()) == 1? @[User::varPackageFirst]:@[User::varPackageOther]`" 通过在月份的第一天运行 `varPackageFirst` 变量指定的包而在其他天中运行 `varPackageOther` 变量指定的包，来更新“执行包”任务所运行的包。 有关详细信息，请参阅 [在包中使用属性表达式](../integration-services/expressions/use-property-expressions-in-packages.md)。  
  
 **数据流表达式** ：在派生列转换和有条件拆分转换用于填充列的表达式中使用变量来提供值，或将数据行定向到不同的转换输出中。 例如，表达式 `@varSalutation + LastName`连接 `VarSalutation` 变量和 `LastName` 列中的值。 表达式 `Income < @HighIncome`将 `Income` 列值小于 `HighIncome` 变量值的数据行定向到一个输出。 有关详细信息，请参阅 [派生列转换](../integration-services/data-flow/transformations/derived-column-transformation.md)、[有条件拆分转换](../integration-services/data-flow/transformations/conditional-split-transformation.md)和 [Integration Services (SSIS) 表达式](../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
 **优先约束表达式** ：提供要在优先约束中用来确定受约束的可执行文件是否运行的值。 这些表达式可以和执行结果（成功、失败、完成）一起使用，也可代替执行结果。 例如，如果表达式 `@varMax > @varMin`的计算结果为 **true**，则运行可执行文件。 有关详细信息，请参阅 [将表达式添加到优先约束](./control-flow/precedence-constraints.md)。  
  
 **参数和返回代码** ：为输入参数提供值，或存储输出参数和返回代码的值。 可通过将变量映射到参数和返回值来执行上述操作。 例如，如果将变量 `varProductId` 设置为 23 并运行 SQL 语句 `SELECT * from Production.Product WHERE ProductID = ?`，查询将检索 `ProductID` 为 23 的产品。 有关详细信息，请参阅 [执行 SQL 任务](../integration-services/control-flow/execute-sql-task.md) 和 [执行 SQL 任务中的参数和返回代码](./control-flow/execute-sql-task.md)。  
  
 **For 循环表达式** ：提供要在 FOR 循环的初始化表达式、求值表达式和赋值表达式中使用的值。 例如，如果变量 `varCount` 为 2， `varMaxCount` 为 10，初始化表达式为 `@varCount`，求值表达式为  `@varCount < @varMaxCount`，赋值表达式为 `@varCount =@varCount +1`，则循环将重复 8 次。 有关详细信息，请参阅 [For 循环容器](../integration-services/control-flow/for-loop-container.md)。  
  
 **父包变量配置** ：将值由父包传递给子包。 子包可通过使用父包变量配置来访问父包中的变量。 例如，如果子包必须与父包使用相同日期，则子包可以定义一个父包变量配置，该配置将指定 GETDATE 函数在父包中设置的变量。 有关详细信息，请参阅 [Execute Package Task](../integration-services/control-flow/execute-package-task.md) 和 [Package Configurations](./packages/legacy-package-deployment-ssis.md)。  
  
 **脚本任务和脚本组件** 为脚本任务或脚本组件提供只读和可读/写变量的列表，在脚本内更新读/写变量，然后在该脚本内或该脚本外使用更新的值。 例如，在代码 `numberOfCars = CType(Dts.Variables("NumberOfCars").Value, Integer)`中，脚本变量 `numberOfCars` 由 `NumberOfCars`变量中的值更新。 有关详细信息，请参阅 [Using Variables in the Script Task](../integration-services/extending-packages-scripting/task/using-variables-in-the-script-task.md)。  

## <a name="add-a-variable"></a>添加变量  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开要使用的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，若要定义变量的作用域，请执行下列操作之一：  
  
    -   若要设置包的范围，请单击 **“控制流”** 选项卡设计图面上的任何位置。  
  
    -   若要设置事件处理程序的作用域，请在 **“事件处理程序”** 选项卡的设计图面上，选择一个可执行文件和一个事件处理程序。  
  
    -   若要设置任务或容器的范围，请在 **“控制流”** 选项卡或 **“事件处理程序”** 选项卡的设计图面上，单击一个任务或容器。  
  
4.  在 **SSIS** 菜单上单击 **“变量”** 。 您可以通过将 View.Variables 命令映射到在 **“选项”** 对话框的 **“键盘”** 页上选择的组合键来显示 **“变量”** 窗口。  
  
5.  在 **“变量”** 窗口中，单击 **“添加变量”** 图标。 新的变量将添加到该列表中。  
  
6.  也可以单击 **“网格选项”** 图标，选择要在 **“变量网格选项”** 对话框中显示的其他列，然后单击 **“确定”**。  
  
7.  或者，设置变量属性。 有关详细信息，请参阅 [设置用户定义变量的属性]()定义。  
  
8.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  

### <a name="add-variable-dialog-box"></a>“添加变量”对话框
可以使用 **“添加变量”** 对话框指定新变量的属性。  
  
#### <a name="options"></a>选项  
 **容器**  
 在列表中选择容器。 容器定义了变量的作用域。 容器可以是包，也可以是包中的可执行文件。  
  
 **名称**  
 键入变量名称。  
  
 **Namespace**  
 指定变量的命名空间。 默认情况下，用户定义变量位于 **User** 命名空间中。  
  
 **值类型**  
 选择数据类型。  
  
 **值**  
 键入值。 该值必须与 **“值类型”** 选项中指定的数据类型相符。  
  
 **只读**  
 如果选择此项，则变量将是只读的。  
   
## <a name="delete-a-variable"></a>删除变量  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，右键单击该包将其打开。  
  
3.  在 **SSIS** 菜单上单击 **“变量”** 。 您可以通过将 View.Variables 命令映射到在 **“选项”** 对话框的 **“键盘”** 页上选择的组合键来显示 **“变量”** 窗口。  
  
4.  选择要删除的变量，然后单击 **“删除变量”**。  
  
     如果在“变量”窗口中没有看到该变量，请单击“网格选项”，然后选择“显示所有作用域的变量” 。  
  
5.  如果 **“确认删除变量”** 对话框打开，请单击 **“是”** 确认删除。  
  
6.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="change-the-scope-of-a-variable"></a>更改变量的作用域  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，右键单击该包将其打开。  
  
3.  在 **SSIS** 菜单上单击 **“变量”** 。 您可以通过将 View.Variables 命令映射到在 **“选项”** 对话框的 **“键盘”** 页上选择的组合键来显示 **“变量”** 窗口。  
  
4.  选择该变量，然后单击 **“移动变量”**。  
  
     如果在“变量”窗口中没有看到该变量，请单击“网格选项”，然后选择“显示所有作用域的变量” 。  
  
5.  在 **“选择新作用域”** 对话框中，选择包或包中的容器、任务或事件处理程序，以更改变量作用域。  
  
6.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  

## <a name="set-the-properties-of-a-user-defined-variable"></a>设置用户定义变量的属性
 若要在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]中设置用户定义的变量的属性，可以使用下列功能之一：  
  
-   “变量”窗口。  
  
-   属性窗口。 “属性”窗口中列出了用于配置“变量”窗口中不可用变量的属性：Description、EvaluateAsExpression、Expression、ReadOnly、ValueType 和 IncludeInDebugDump。  
  
> [!NOTE]  
>  [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 还提供了一组无法更新属性的系统变量，但 RaiseChangedEvent 属性例外。  
  
### <a name="set-expressions-on-variables"></a>对变量设置表达式  
  
 使用“属性”窗口对用户定义变量设置表达式时：  
  
-   可通过 Value 属性或 Expression 属性来设置变量的值。 默认情况下，EvaluateAsExpression 属性设置为 **False** ，变量的值由 Value property 属性进行设置。 若要使用表达式来设置值，必须首先将 EvaluateAsExpression 设置为 **True**，然后在 Expression property 属性中提供一个表达式。 Value 属性自动设置为该表达式的计算结果。  
  
-   ValueType 属性包含 Value 属性中的值的数据类型。 通过表达式设置 Value 时，ValueType 将自动更新为与该表达式的计算结果兼容的数据类型。 例如，如果 Value 包含 0，ValueType 属性包含 **Int32** ，并将 Expression 设置为 GETDATE()，则 Value 包含当前日期和时间，并且 ValueType 会设置为 **DateTime**。  
  
-   通过变量的 **“属性”** 窗口，可以访问 **“表达式生成器”** 对话框。 使用该工具可以生成、验证和计算表达式。 有关详细信息，请参阅[表达式生成器](../integration-services/expressions/expression-builder.md)和 [Integration Services (SSIS) 表达式](../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
 使用“变量”窗口对用户定义变量设置表达式时：  
  
-   若要使用表达式来设置变量值，首先请确认变量数据类型与表达式的计算结果一致，然后在 **“变量”** 窗口的 **“表达式”** 列中提供表达式。 “属性”窗口中的 EvaluateAsExpression 属性会自动设置为 **True**。  
  
-   如果为变量指定了表达式，则该变量旁边将显示一个特殊图标标记。 这个特殊的图标标记还显示在设置有表达式的连接管理器和任务旁边。  
  
-   通过变量的 **“变量”** 窗口，可以访问 **“表达式生成器”** 对话框。 使用该工具可以生成、验证和计算表达式。 有关详细信息，请参阅[表达式生成器](../integration-services/expressions/expression-builder.md)和 [Integration Services (SSIS) 表达式](../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
 在 **“变量”** 和 **“属性”** 窗口中，如果为变量分配了表达式，并且 **EvaluateAsExpression** 设置为 **True**，则无法更改变量数据类型。  
  
### <a name="set-the-namespace-and-name-properties"></a>设置 Namespace 和 Name 属性
  
 **Name** 和 **Namespace** 属性的值必须以 Unicode 标准 2.0 定义的字母字符或下划线 (_) 开头。 后续字符可以是在 Unicode 标准 2.0 中定义的字母或数字，或是下划线 (\_)。  
  
### <a name="set-variable-properties-in-the-variables-window"></a>在变量窗口中设置变量属性   
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，右键单击该包将其打开。  
  
3.  在 **SSIS** 菜单上单击 **“变量”** 。  
  
     您可以通过将 View.Variables 命令映射到在 **“选项”** 对话框的 **“键盘”** 页上选择的组合键来显示 **“变量”** 窗口。  
  
4.  或者，在 **“变量”** 窗口中单击 **“网格选项”**，然后选择要出现在 **“变量”** 窗口中的列，并选择要应用到变量列表的筛选器。  
  
5.  在列表中选择变量，然后更新 **“名称”**、 **“数据类型”**、 **“值”**、 **“命名空间”**、 **“引发更改事件”**、 **“描述”** 和 **“表达式”** 列的值。  
  
6.  在列表中选择变量，然后单击 **“移动变量”** 以更改作用域。  
  
7.  若要保存已更新的包，请在 **“文件”** 菜单中单击 **“保存选定项”** 。  
  
### <a name="set-variable-properties-in-the-properties-window"></a>在属性窗口中设置变量属性  

1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，右键单击该包将其打开。  
  
3.  在 **“视图”** 菜单上，单击 **“属性窗口”** 。  
  
4.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，单击 **“包资源管理器”** 选项卡，并展开“包”节点。  
  
5.  若要修改包范围内的变量，请展开“变量”节点，如果看不到该节点，请展开“事件处理程序”或“可执行文件”节点，直到找到包含要修改的变量的“变量”节点。  
  
6.  单击要修改其属性的变量。  
  
7.  在“属性”窗口中，更改读/写变量属性。 对于用户定义的变量而言，某些属性为可读/只读。  
  
     有关属性的详细信息，请参阅 [Integration Services (SSIS) 变量](../integration-services/integration-services-ssis-variables.md)。  
  
8.  若要保存已更新的包，请在 **“文件”** 菜单中单击 **“保存选定项”** 。  

## <a name="update-a-variable-dynamically-with-configurations"></a>使用配置以动态方式更新变量  
 若要动态更新变量，可以为变量创建配置，将这些配置部署到包中，然后在部署包时更新配置文件中的变量值。 在运行时，包使用更新后的变量值。 有关详细信息，请参阅 [创建包配置](./packages/legacy-package-deployment-ssis.md)。  

## <a name="related-tasks"></a>Related Tasks  
 [在子包中使用变量和参数的值](../integration-services/packages/legacy-package-deployment-ssis.md#child)  
  
 [将查询参数映射到数据流组件中的变量](../integration-services/data-flow/map-query-parameters-to-variables-in-a-data-flow-component.md)