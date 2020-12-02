---
description: 脚本任务
title: 脚本任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.scripttask.f1
- sql13.dts.designer.scripttask.general.f1
- sql13.dts.designer.scripttask.script.f1
helpviewer_keywords:
- scripts [Integration Services], tasks
- Script task [Integration Services], about Script task
- Script task [Integration Services]
ms.assetid: f6cce7df-4bd6-4b75-9f89-6c37b4bb5558
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4f5f18f3306256906d6c419957aa3d97d3506e7d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92197126"
---
# <a name="script-task"></a>脚本任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  脚本任务提供代码来执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 内置任务和转换中不可用的函数。 脚本任务还可将函数组合到一个脚本中，而不必使用多个任务和转换。 脚本任务用于执行必须在包中一次完成（或每个枚举对象一次完成）、而不是每个数据行一次完成的工作。  
  
 可以将脚本任务用于下列目的：  
  
-   用内置连接类型不支持的其他技术访问数据。 例如，脚本可使用 Active Directory 服务接口 (ADSI) 访问 Active Directory，并从中提取用户名。  
  
-   创建特定于包的性能计数器。 例如，脚本可以创建一个性能计数器，在运行复杂任务或性能差的任务时，该计数器将被更新。  
  
-   标识是否指定的文件为空或它们包含多少行，然后基于该信息影响包中的控制流。 例如，如果文件包含零行，则将变量的值设置为 0，然后计算该值的优先约束将防止文件系统任务复制此文件。  
  
 如果必须使用脚本对集中的每个数据行执行相同的工作，则应当使用脚本组件而不是脚本任务。 例如，如果希望评估邮资金额的合理性，从而跳过金额极高或极低的数据行，则应当使用脚本组件。 有关详细信息，请参阅 [Script Component](../../integration-services/data-flow/transformations/script-component.md)。  
  
 如果多个包使用一个脚本，请考虑使用自定义任务而不要使用脚本任务。 有关详细信息，请参阅 [开发自定义任务](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)。  
  
 确定脚本任务是包的正确选择之后，必须开发该任务使用的脚本，还必须配置任务本身。  
  
## <a name="writing-and-running-the-script-that-the-task-uses"></a>编写和运行任务使用的脚本  
 脚本任务将 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 用作编写脚本的环境和运行这些脚本的引擎。  
  
 VSTA 提供 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 环境的所有标准功能，如具有颜色编码的 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 编辑器、IntelliSense 和“对象资源管理器”。 VSTA 还可以使用与其他 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 开发工具使用的调试器。 脚本中的断点与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 任务和容器上的断点可以无缝集合。 VSTA 支持 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic 和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# 编程语言。  
  
 若要运行脚本，运行包的计算机上必须安装有 VSTA。 运行包时，任务将加载脚本引擎并运行脚本。 可以通过向项目中的程序集添加引用的方式来访问脚本中的外部 .NET 程序集。 目前，我们不支持 .NET Core 和 .NET 标准程序集引用。  
  
> [!NOTE]  
>  在早期版本中您可以指示是否对脚本进行预编译，而在 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 和更高版本中，不同的是，所有脚本都要预编译。 脚本进行预编译后，运行时将不加载语言引擎，因此包运行得更快。 但是，预编译二进制文件占用了大量的磁盘空间。  
  
## <a name="configuring-the-script-task"></a>配置脚本任务  
 可以采用下列方法来配置脚本任务：  
  
-   提供任务运行的自定义脚本。  
  
-   在 VSTA 项目中指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 运行时作为脚本任务代码入口点调用的方法。  
  
-   指定脚本语言。  
  
-   根据需要提供脚本中使用的只读和读/写变量列表。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置这些属性。  
  
### <a name="configuring-the-script-task-in-the-designer"></a>在设计器中配置脚本任务  
 下表描述可以为脚本任务进行记录的 **ScriptTaskLogEntry** 事件。 在 **“配置 SSIS 日志”** 对话框的 **“详细信息”** 选项卡上选择 **ScriptTaskLogEntry** 事件，以便进行日志记录。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|报告在脚本中实现日志记录的结果。 该任务在每次调用 **Log** 对象的 **Dts** 方法时都编写一个日志条目。 然后，在运行代码时编写这些条目。 有关详细信息，请参阅 [Logging in the Script Task](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md)。|  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请参阅以下主题：  
  
-   [脚本任务编辑器（“常规”页）]()  
  
-   [脚本任务编辑器（“脚本”页）]()  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请参阅以下主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="configuring-the-script-task-programmatically"></a>以编程方式配置脚本任务  
 有关以编程方式设置这些属性的详细信息，请参阅以下主题：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask>  
  
## <a name="script-task-editor-general-page"></a>脚本任务编辑器（“常规”页）
  可以使用 **“脚本任务编辑器”** 对话框的 **“常规”** 页命名和描述脚本任务。  
  
 若要了解有关脚本任务的详细信息，请参阅 [Script Task](../../integration-services/control-flow/script-task.md) 和 [在脚本任务编辑器中配置脚本任务](../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md)。 若要了解如何对脚本任务进行编程，请参阅 [Extending the Package with the Script Task](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md)。  
  
### <a name="options"></a>选项  
 **名称**  
 为脚本任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入脚本任务的说明。  
  
## <a name="script-task-editor-script-page"></a>脚本任务编辑器（“脚本”页）
  使用 **“脚本任务编辑器”** 对话框的 **“脚本”** 页，可以设置脚本属性并指定脚本可以访问的变量。  
  
> [!NOTE]  
>  在 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 和更高版本中，所有脚本都将预编译。 而在早期版本中，要设置 **PrecompileScriptIntoBinaryCode** 属性来指定是否对脚本进行预编译。  
  
 若要了解有关脚本任务的详细信息，请参阅 [Script Task](../../integration-services/control-flow/script-task.md) 和 [在脚本任务编辑器中配置脚本任务](../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md)。 若要了解如何对脚本任务进行编程，请参阅 [Extending the Package with the Script Task](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md)。  
  
### <a name="options"></a>选项  
 **ScriptLanguage**  
 选择任务的脚本语言， [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#。  
  
 创建任务的脚本后，将无法更改 **ScriptLanguage** 属性的值。  
  
 若要设置脚本任务的默认脚本语言，请使用 **“选项”** 对话框的 **“常规”** 页上的 **“脚本语言”** 选项。 有关详细信息，请参阅 [General Page]()。  
  
 **EntryPoint**  
 指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 运行时作为脚本任务代码入口点调用的方法。 指定的方法必须在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 项目的 ScriptMain 类中。ScriptMain 类是脚本模板生成的默认类。  
  
 若要更改 VSTA 项目中方法的名称，则必须更改 **EntryPoint** 属性的值。  
  
 **ReadOnlyVariables**  
 以逗号分隔的形式键入一列可供脚本使用的只读变量，或单击省略号 (…) 按钮，然后在“选择变量”对话框中选择变量。  
  
> [!NOTE]  
>  变量名称区分大小写。  
  
 **ReadWriteVariables**  
 以逗号分隔的形式键入一列可供脚本使用的读/写变量，或单击省略号 (…) 按钮，然后在“选择变量”对话框中选择变量。  
  
> [!NOTE]  
>  变量名称区分大小写。  
  
 **编辑脚本**  
 打开 VSTA IDE，您可以在其中创建或修改脚本。  
  
## <a name="related-content"></a>相关内容  
  
-   shareourideas.com 上的技术文章 [如何在 C# 中发送具有传递通知的电子邮件](https://go.microsoft.com/fwlink/?LinkId=237625)（如何在 C# 中发送具有传递通知的电子邮件）  
  
