---
description: 执行进程任务
title: 执行进程任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executeprocesstask.f1
- sql13.dts.designer.executeprocesstask.general.f1
- sql13.dts.designer.executeprocesstask.process.f1
helpviewer_keywords:
- Execute Process task [Integration Services]
ms.assetid: aca5a0b5-34a9-45bc-a234-8e63ea51a1ee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 98409c2a75cc76589a705579ff02f1be34c9c7b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352116"
---
# <a name="execute-process-task"></a>执行进程任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  执行进程任务在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包工作流中运行应用程序或批处理文件。 虽然可以使用执行进程任务打开任意标准应用程序（例如 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 或 [!INCLUDE[ofprword](../../includes/ofprword-md.md)]），但通常还是使用它来运行针对数据源执行的业务应用程序或批处理文件。 例如，可以使用执行进程任务来展开一个压缩的文本文件。 然后，包可将该文本文件用作包中数据流的数据源。 再举一个例子，您可以使用执行进程任务运行生成日销售额报表的自定义 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 应用程序。 然后，可以将该报表附加到发送邮件任务，并将其转发给通讯组列表。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 还包含一些执行工作流操作的其他任务（如执行包）。 有关详细信息，请参阅 [Execute Package Task](../../integration-services/control-flow/execute-package-task.md)  
  
## <a name="custom-log-entries-available-on-the-execute-process-task"></a>执行进程任务可用的自定义日志项  
 下表列出了执行进程任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|提供所配置任务要运行的进程的信息。<br /><br /> 写入两个日志条目。 一个日志条目包含有关任务所运行可执行文件的名称和位置的信息，另一个条目则记录从可执行文件退出的信息。|  
|**ExecuteProcessVariableRouting**|提供有关哪些变量被路由到可执行文件的输入和输出的信息。 将为 stdin（输入）、stdout（输出）和 stderr（错误输出）写入日志条目。|  
  
## <a name="configuration-of-the-execute-process-task"></a>执行进程任务的配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="property-settings"></a>属性设置  
 当执行进程任务运行自定义应用程序时，该任务将通过以下一种或两种方法为该应用程序提供输入：  
  
-   在 **StandardInputVariable** 属性设置中指定的变量。 有关变量的详细信息，请参阅 [Integration Services (SSIS) 变量](../../integration-services/integration-services-ssis-variables.md)和[在包中使用变量](../integration-services-ssis-variables.md)。  
  
-   在 **Arguments** 属性设置中指定的参数。 （例如，如果该任务打开一个 Word 格式的文档，该参数就可指定该 .doc 文件的名称。）  
  
 若要在一个执行进程任务中向自定义应用程序传递多个参数，请使用空格将这些参数隔开。 参数中不能包含空格，否则该任务将不运行。 可以使用表达式将变量值作为参数传递。 在下面的示例中，表达式将两个变量值作为参数传递并使用空格将参数隔开：  
  
 `@variable1 + " " + @variable2`  
  
 可以使用表达式设置执行进程任务的各种属性。  
  
 当使用 **StandardInputVariable** 属性配置执行进程任务以提供输入时，请从该应用程序中调用 **Console.ReadLine** 方法来读取输入。 有关详细信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 类库中的 [Console.ReadLine 方法](/dotnet/api/system.console.readline)主题。  
  
 当使用 **Arguments** 属性配置执行进程任务以提供输入时，请执行下列步骤之一以获得参数：  
  
-   如果使用 Microsoft Visual Basic 编写应用程序，请设置 **My.Application.CommandLineArgs** 属性。 下面的示例设置 **My.Application.CommandLineArgs** 属性以检索两个参数：  
  
    ```vb  
    Dim variable1 As String = My.Application.CommandLineArgs.Item(0)  
    Dim variable2 As String = My.Application.CommandLineArgs.Item(1)   
    ```  
  
     有关详细信息，请参阅 [参考中的](https://go.microsoft.com/fwlink/?LinkId=129200)My.Application.CommandLineArgs 属性 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 这一主题。  
  
-   如果使用 Microsoft Visual C# 编写应用程序，请使用 **Main** 方法。  
  
     有关详细信息，请参阅 C# 编程指南中的 [命令行参数（C# 编程指南）](/dotnet/csharp/programming-guide/main-and-command-args/command-line-arguments)这一主题。  
  
 执行进程任务还包括 **StandardOutputVariable** 和 **StandardErrorVariable** 属性，用来分别指定使用该应用程序的标准输出和错误输出的变量。  
  
 另外，您还可以配置执行进程任务来指定工作目录、超时期限或表示可执行文件成功运行的值。 您还可以对该任务进行配置，使其在可执行文件的返回代码与指示成功的值不匹配时或者在指定位置找不到可执行文件时失败。  
  
## <a name="programmatic-configuration-of-the-execute-process-task"></a>执行进程任务的编程配置  
 有关以编程方式设置这些属性的详细信息，请单击以下主题：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ExecuteProcess.ExecuteProcess>  
  
## <a name="execute-process-task-editor-general-page"></a>执行进程任务编辑器（“常规”页）
  可以使用“执行进程任务编辑器”对话框的“常规”页，对执行进程任务进行命名和说明。  
  
### <a name="options"></a>选项  
 **名称**  
 为执行进程任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入对执行进程任务的说明。  
  
## <a name="execute-process-task-editor-process-page"></a>执行进程任务编辑器（“进程”页）
  可以使用 **“执行进程任务编辑器”** 对话框的 **“进程”** 页配置执行进程的选项。 这些选项包括要运行的可执行文件、该可执行文件的位置、命令提示符参数以及提供输入及捕获输出的变量。  
  
### <a name="options"></a>选项  
 **RequireFullFileName**  
 指示在指定位置未找到可执行文件时该任务是否应失败。  
  
 **可执行文件**  
 键入要运行的可执行文件的名称。  
  
 **参数**  
 提供命令提示符参数。  
  
 **WorkingDirectory**  
 键入包含可执行文件的文件夹的路径，或单击浏览 (…) 按钮定位到该文件夹  。  
  
 **StandardInputVariable**  
 选择为进程提供输入的变量，或单击 \<**New variable...**> 创建新的变量：  
  
 **相关主题：** [添加变量](../integration-services-ssis-variables.md)  
  
 **StandardOutputVariable**  
 选择捕获进程输出的变量，或单击 \<**New variable...**> 创建新的变量。  
  
 **StandardErrorVariable**  
 选择捕获处理器的错误输出的变量，或单击 \<**New variable...**> 创建新的变量。  
  
 **FailTaskIfReturnCodeIsNotSuccessValue**  
 指示在进程退出代码与 **SuccessValue** 中指定的值不同时任务是否失败。  
  
 **SuccessValue**  
 指定可执行文件返回的用来指示进程成功的值。 默认情况下，此值设置为 **0**。  
  
 **TimeOut**  
 指定进程可以运行的秒数。 如果值为 **0** ，则表示不使用超时值，进程一直运行到处理完毕或出错为止。  
  
 **TerminateProcessAfterTimeOut**  
 指示在 **TimeOut** 选项指定的超时期限过后是否强制结束进程。 只有在 **TimeOut** 不为 **0** 时，此选项才可用。  
  
 **WindowStyle**  
 指定用于运行进程的窗口样式。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
