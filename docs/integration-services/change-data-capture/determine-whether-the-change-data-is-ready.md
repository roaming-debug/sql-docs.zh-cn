---
description: 确定变更数据是否已准备就绪
title: 确定变更数据是否已准备就绪 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],determining readiness
ms.assetid: 04935f35-96cc-4d70-a250-0fd326f8daff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9fd82f56cb1edb459e662e31778fcb8341a6887c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353035"
---
# <a name="determine-whether-the-change-data-is-ready"></a>确定变更数据是否已准备就绪

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在用于执行变更数据的增量加载的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的控制流中，第二个任务是确保所选间隔的变更数据已准备就绪。 此步骤是必需的，因为异步捕获进程可能尚未处理完到达所选端点的所有更改。  
  
> [!NOTE]  
>  控制流的第一个任务是计算更改间隔的端点。 有关此任务的详细信息，请参阅 [指定变更数据的间隔](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md)。 有关设计控制流的总体过程的说明，请参阅[变更数据捕获 (SSIS)](../../integration-services/change-data-capture/change-data-capture-ssis.md)。  
  
## <a name="understanding-the-components-of-the-solution"></a>了解解决方案的组件  
 本主题中介绍的解决方案使用以下 4 个 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件：  
  
-   一个 For 循环容器，用于重复地计算执行 SQL 任务的输出。  
  
-   一个执行 SQL 任务，用于查询变更数据捕获进程维护的特殊表并使用该信息来确定数据是否已准备就绪。  
  
-   一个用于在数据未准备就绪时实现处理延迟的组件。 这可以是脚本任务，也可以是执行 SQL 任务。  
  
-   一个可选的组件，用于在执行 SQL 任务返回的值指示错误或超时情况时报告错误或超时。  
  
 这些组件设置或读取几个包变量的值，以控制循环内以及之后包中的执行流程。  
  
#### <a name="to-set-up-package-variables"></a>设置包变量  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，在 **“变量”** 窗口中创建以下变量：  
  
    1.  创建一个数据类型为 integer 的变量，以保存执行 SQL 任务返回的状态值。  
  
         此示例使用变量名 DataReady，其初始值为 0。  
  
    2.  创建一个变量以保存数据未准备就绪时的延迟时间。 如果您计划使用脚本任务来实现延迟，则该变量的数据类型应为 integer。 如果您计划使用含有 WAITFOR 语句的执行 SQL 任务，则该变量的数据类型应为 string 以便能够接受诸如 00:00:10 之类的值。  
  
         此示例使用变量名 DelaySeconds，其初始值为 10。  
  
    3.  创建一个数据类型为 integer 的变量，以保存循环的当前迭代。  
  
         此示例使用变量名 TimeoutCount，其初始值为 0。  
  
    4.  创建一个数据类型为 integer 的变量，以指定循环在报告超时情况之前应对数据执行的测试次数。  
  
         此示例使用变量名 TimeoutCeiling，其初始值为 20。  
  
    5.  （可选）创建一个数据类型为 integer 的变量以指示变更数据的首次加载。  
  
         此示例使用变量名 IntervalID，并且仅检查 0 值以指示首次加载。  
  
## <a name="configuring-a-for-loop-container"></a>配置 For 循环容器  
 根据上述变量集，For 循环容器是首先要添加的组件。  
  
#### <a name="to-configure-a-for-loop-container-to-wait-until-change-data-is-ready"></a>将 For 循环容器配置为等待变更数据准备就绪  
  
1.  在 **设计器的** “控制流” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 选项卡上，向控制流中添加 For 循环容器。  
  
2.  将用于计算间隔端点的执行 SQL 任务连接到 For 循环容器。  
  
3.  在 **“For 循环编辑器”** 中，选择以下选项：  
  
    1.  对于 **InitExpression**，输入 `@DataReady = 0`。  
  
         此表达式设置循环变量的初始值。  
  
    2.  对于 **EvalExpression**，输入 `@DataReady == 0`。  
  
         如果此表达式的计算结果为 **False**，则执行将跳出循环，开始进行增量加载。  
  
## <a name="configuring-the-execute-sql-task-that-queries-for-change-data"></a>配置用于查询变更数据的执行 SQL 任务  
 在 For 循环容器内，添加一个执行 SQL 任务。 此任务在数据库中查询变更数据捕获进程维护的表。 此查询的结果是一个指示变更数据是否已准备就绪的状态值。  
  
 在下表中，第一列显示 Transact-SQL 示例查询中从执行 SQL 任务返回的值。 第二列显示其他组件如何响应这些值。  
  
|返回值|含义|响应|  
|------------------|-------------|--------------|  
|0|指示变更数据未准备就绪。<br /><br /> 在所选间隔的结束点之后没有变更数据捕获记录。|执行过程通过实现延迟的组件继续执行。 然后该控件返回 For 循环容器，只要返回值为 0，就会继续检查执行 SQL 任务。|  
|1|可能指示没有捕获到整个间隔的变更数据，或者变更数据已删除。 这被视为错误情况。<br /><br /> 在所选间隔的起始点之前没有变更数据捕获记录。|执行过程通过记录错误的可选组件继续执行。|  
|2|指示数据已准备就绪。<br /><br /> 在所选间隔的起始点之前和结束点之后都存在变更数据捕获记录。|执行将跳出 For 循环容器并且增量加载开始进行。|  
|3|指示所有可用的变更数据的首次加载。<br /><br /> 条件逻辑从仅用于此目的特殊包变量中获取此值。|执行将跳出 For 循环容器并且增量加载开始进行。|  
|5|指示已达到 TimeoutCeiling。<br /><br /> 循环已对数据执行了指定次数的测试，数据仍不可用。 如果没有此测试或类似的测试，该包可能会无限期地运行。|执行过程通过记录超时的可选组件继续执行。|  
  
#### <a name="to-configure-an-execute-sql-task-to-query-whether-change-data-is-ready"></a>配置执行 SQL 任务，以查询变更数据是否已准备就绪  
  
1.  在 For 循环容器内，添加一个执行 SQL 任务。  
  
2.  在 **“执行 SQL 任务编辑器”** 中的 **“常规”** 页上，选择以下选项：  
  
    1.  对于 **ResultSet**，选择 **“单行”**。  
  
    2.  配置到源数据库的有效连接。  
  
    3.  对于 **SQLSourceType**，选择 **“直接输入”**。  
  
    4.  对于 **SQLStatement**，输入以下 SQL 语句：  
  
        ```  
        declare @DataReady int, @TimeoutCount int  
  
        if not exists (select tran_end_time from cdc.lsn_time_mapping  
                where tran_end_time > ?  )  
            select @DataReady = 0  
        else  
            if ? = 0  
                select @DataReady = 3   
        else  
            if not exists (select tran_end_time from cdc.lsn_time_mapping  
                    where tran_end_time <= ? )  
                select @DataReady = 1   
        else  
            select @DataReady = 2  
  
        select @TimeoutCount = ?  
        if (@DataReady = 0)  
            select @TimeoutCount = @TimeoutCount + 1  
        else  
            select @TimeoutCount = 0  
  
        if (@TimeoutCount > ?)  
            select @DataReady = 5  
  
        select @DataReady as DataReady, @TimeoutCount as TimeoutCount  
  
        ```  
  
3.  在 **“执行 SQL 任务编辑器”** 的 **“参数映射”** 页上，进行以下映射：  
  
    1.  将 ExtractEndTime 变量映射到参数 0。  
  
    2.  将 IntervalID 变量映射到参数 1。  
  
    3.  将 ExtractStartTime 变量映射到参数 2。  
  
    4.  将 TimeoutCount 变量映射到参数 3。  
  
    5.  将 TimeoutCeiling 变量映射到参数 4。  
  
4.  在 **“执行 SQL 任务编辑器”** 的 **“结果集”** 页上，将 DataReady 结果映射到 DataReady 变量，并将 TimeoutCount 结果映射到 TimeoutCount 变量。  
  
## <a name="waiting-until-the-change-data-is-ready"></a>等到变更数据准备就绪  
 当变更数据未准备就绪时，您可以使用多种方法中的一种来实现延迟。 下面的两个过程演示如何使用脚本任务或执行 SQL 任务来实现延迟。  
  
> [!NOTE]  
>  预编译的脚本产生的开销小于执行 SQL 任务。  
  
#### <a name="to-implement-a-delay-by-using-a-script-task"></a>使用脚本任务实现延迟  
  
1.  在 For 循环容器内，添加一个脚本任务。  
  
2.  将进行查询以确定变更数据是否准备就绪的执行 SQL 任务连接到新的脚本任务。  
  
3.  对于用于将执行 SQL 任务连接到脚本任务的优先约束，打开 **“优先约束编辑器”** 并选择以下选项：  
  
    1.  对于 **“求值运算”**，选择 **“表达式和约束”**。  
  
    2.  对于 **“值”**，选择 **“成功”**。  
  
         约束值 **“成功”** 指上一任务成功。 在此例中，指执行 SQL 任务成功。  
  
    3.  对于 **“表达式”**，输入 `@DataReady == 0 && @TimeoutCount <= @TimeoutCeiling`。  
  
    4.  选择 **“逻辑与。所有约束的计算结果都必须为 True**（如果尚未选择）。  
  
4.  在 **“脚本任务编辑器”** 的 **“脚本”** 页上，对于 **ReadOnlyVariables**，从列表中选择 **User::DelaySeconds** 整数变量。  
  
5.  在 **“脚本任务编辑器”** 的 **“脚本”** 页上，单击 **“编辑脚本”** 以打开脚本开发环境。  
  
6.  在 Main 过程中，输入下面的代码行之一：  
  
    -   如果您是使用 C# 进行编程，请输入下面的代码行：  
  
        ```  
        System.Threading.Thread.Sleep((int)Dts.Variables["DelaySeconds"].Value * 1000);  
        ```  
  
         \- 或 -  
  
    -   如果您是使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]进行编程，请输入下面的代码行：  
  
        ```  
        System.Threading.Thread.Sleep(Ctype(Dts.Variables("DelaySeconds").Value, Integer) * 1000)  
  
        ```  
  
        > [!NOTE]  
        >  **Thread.Sleep** 方法要求指定参数时以毫秒为单位。  
  
7.  保留从脚本执行过程返回 **DtsExecResult.Success** 的默认代码行。  
  
8.  关闭脚本开发环境和 **“脚本任务编辑器”**。  
  
#### <a name="to-implement-a-delay-by-using-an-execute-sql-task"></a>使用执行 SQL 任务实现延迟  
  
1.  在 For 循环容器内，添加一个执行 SQL 任务。  
  
2.  将进行查询以确定变更数据是否准备就绪的执行 SQL 任务连接到新的执行 SQL 任务。  
  
3.  对于用于连接两个执行 SQL 任务的优先约束，打开 **“优先约束编辑器”** 并选择以下选项：  
  
    1.  对于 **“求值运算”**，选择 **“表达式和约束”**。  
  
    2.  对于 **“值”**，选择 **“成功”**。  
  
         约束值 **“成功”** 指上一执行 SQL 任务成功。  
  
    3.  对于 **“表达式”**，输入 `@DataReady == 0`。  
  
    4.  选择 **“逻辑与。所有约束的计算结果都必须为 True**（如果尚未选择）。  
  
         此选项要求条件、约束和表达式都必须为 true。  
  
4.  在 **“执行 SQL 任务编辑器”** 中的 **“常规”** 页上，选择以下选项：  
  
    1.  对于 **ResultSet**，选择 **“单行”**。  
  
    2.  配置到源数据库的有效连接。  
  
    3.  对于 **SQLSourceType**，选择 **“直接输入”**。  
  
    4.  对于 **SQLStatement**，输入以下 SQL 语句：  
  
        ```  
        WAITFOR DELAY ?  
  
        ```  
  
5.  在编辑器的 **“参数映射”** 页上，将 DelaySeconds 字符串变量映射到参数 0。  
  
## <a name="handling-an-error-condition"></a>处理错误情况  
 您可以选择在循环内配置额外的组件以记录错误或超时情况：  
  
-   当 DataReady 变量的值 = 1 时，此组件可以记录错误情况。 此值指示在所选间隔开始前没有可用的变更数据。  
  
-   达到 TimeoutCeiling 变量的值时，此组件还可以记录超时情况。 此值指示循环已对数据执行了指定次数的测试，但数据仍不可用。 如果没有此测试或类似的测试，该包可能会无限期地运行。  
  
#### <a name="to-configure-an-optional-script-task-to-log-an-error-condition"></a>配置可选的脚本任务以记录错误情况  
  
1.  如果想要通过将消息写入日志记录来报告错误或超时，请配置包的日志记录功能。 有关详细信息，请参阅 [在 SQL Server Data Tools 中启用包日志记录](../../integration-services/performance/integration-services-ssis-logging.md#ssdt)。  
  
2.  在 For 循环容器内，添加一个脚本任务。  
  
3.  将进行查询以确定变更数据是否准备就绪的执行 SQL 任务连接到新的脚本任务。  
  
4.  对于用于将执行 SQL 任务连接到脚本任务的优先约束，打开 **“优先约束编辑器”** 并选择以下选项：  
  
    1.  对于 **“求值运算”**，选择 **“表达式和约束”**。  
  
    2.  对于 **“值”**，选择 **“成功”**。  
  
         约束值 **“成功”** 指上一任务成功。 在此例中，指执行 SQL 任务成功。  
  
    3.  对于 **“表达式”**，输入 `@DataReady == 1 || @DataReady == 5`。  
  
    4.  选择 **“逻辑与。所有约束的计算结果都必须为 True**（如果尚未选择）。  
  
         此选项要求条件、约束和表达式都必须为 true。  
  
5.  在 **“脚本任务编辑器”** 的 **“脚本”** 页上，对于 **ReadOnlyVariables**，从列表中选择 **User::DataReady** 和 **User::ExtractStartTime** 以便脚本可以使用它们的值。  
  
     如果要在写入日志记录的信息中包含特定的系统变量信息（例如 System::PackageName），还可选择这些变量。  
  
6.  在 **“脚本任务编辑器”** 的 **“脚本”** 页上，单击 **“编辑脚本”** 以打开脚本开发环境。  
  
7.  在 Main 过程中，输入代码以通过调用 **Dts.Log** 方法来记录错误，或者通过调用 **Dts.Events** 接口的方法之一来引发事件。 通过返回 `Dts.TaskResult = Dts.Results.Failure`向包发出错误通知。  
  
     下面的示例显示如何将消息写入日志。 有关详细信息，请参阅 [Logging in the Script Task](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md)、 [Raising Events in the Script Task](../../integration-services/extending-packages-scripting/task/raising-events-in-the-script-task.md)和 [Returning Results from the Script Task](../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md)。  
  
    ```  
    ' User variables.  
    Dim dataReady As Integer = _  
      CType(Dts.Variables("DataReady").Value, Integer)  
    Dim extractStartTime As Date = _  
      CType(Dts.Variables("ExtractStartTime").Value, DateTime)  
  
    ' System variables.  
    Dim packageName As String = _  
      Dts.Variables("PackageName").Value.ToString()  
    Dim executionStartTime As Date = _  
      CType(Dts.Variables("StartTime").Value, DateTime)  
  
    Dim eventMessage As New System.Text.StringBuilder()  
  
    If dataReady = 1 OrElse dataReady = 5 Then  
  
      If dataReady = 1 Then  
        eventMessage.AppendLine("Start Time Error")  
      Else  
        eventMessage.AppendLine("Timeout Error")  
      End If  
  
      With eventMessage  
        .Append("The package ")  
        .Append(packageName)  
        .Append(" started at ")  
        .Append(executionStartTime.ToString())  
        .Append(" and ended at ")  
        .AppendLine(DateTime.Now().ToString())  
        If dataReady = 1 Then  
          .Append("The specified ExtractStartTime was ")  
          .AppendLine(extractStartTime.ToString())  
        End If  
      End With  
  
      System.Windows.Forms.MessageBox.Show(eventMessage.ToString())  
  
      Dts.Log(eventMessage.ToString(), 0, Nothing)  
  
      Dts.TaskResult = Dts.Results.Failure  
  
    Else  
  
      Dts.TaskResult = Dts.Results.Success  
  
    End If  
  
    ```  
  
8.  关闭脚本开发环境和 **“脚本任务编辑器”**。  
  
## <a name="next-step"></a>下一步  
 在确定变更数据已准备就绪之后，下一步就是准备查询变更数据。  
  
 **下一主题：** [准备查询变更数据](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md)  
  
  
