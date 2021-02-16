---
description: 调试控制流
title: 调试控制流 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.setbreakpoints.f1
helpviewer_keywords:
- progress reporting [Integration Services]
- breakpoints [Integration Services]
- debugging [Integration Services], control flow
- control flow [Integration Services], debugging
- color-coded progress reporting [Integration Services]
- Set Breakpoints dialog box
ms.assetid: 54a458cc-9f4f-4b48-8cf2-db2e0fa7756c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 53fc539f46751cf6073aee0d5cd3aefae1971283
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338404"
---
# <a name="debugging-control-flow"></a>调试控制流

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含一些功能和工具，你可以使用这些功能和工具对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中的控制流进行故障排除。  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 支持容器和任务上的断点。  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器在运行时提供进度报告。  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 提供了调试窗口。  
  
## <a name="breakpoints"></a>断点  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供了 **“设置断点”** 对话框，您可以在其中对断点进行设置：启用中断条件，指定发生多少次断点后就将包的执行挂起。 断点可在包级或单个组件级启用。 如果在任务或容器级上启用中断条件，则在 **“控制流”** 选项卡的设计图面上，相应的任务或容器旁边会显示断点图标。如果在包一级启用中断条件，则在 **“控制流”** 选项卡的标签上显示断点图标。  
  
 在命中断点时，断点图标将会发生改变以帮助识别断点源。 包运行时，可以添加、删除和更改断点。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供了十个可以在所有任务和容器上启用的中断条件。 在 **“设置断点”** 对话框中，可根据下列条件启用断点：  
  
|中断条件|说明|  
|---------------------|-----------------|  
|当任务或容器收到 **OnPreExecute** 事件时。|任务将要执行时调用。 此事件由任务或容器在其运行前一刻引发。|  
|当任务或容器收到 **OnPostExecute** 事件时。|任务的执行逻辑完成后立即调用。 此事件由任务或容器在其运行后引发。|  
|当任务或容器收到 **OnError** 事件时。|出现错误时由任务或容器调用。|  
|当任务或容器收到 **OnWarning** 事件时。|当任务处于不能证明出错但有必要发出警告的状态时调用。|  
|当任务或容器收到 **OnInformation** 事件时。|需要任务提供信息时调用。|  
|当任务或容器收到 **OnTaskFailed** 事件时。|任务宿主失败时由其调用。|  
|当任务或容器收到 **OnProgress** 事件时。|要更新任务执行的进度时调用。|  
|当任务或容器收到 **OnQueryCancel** 事件时。|在可以取消执行的任务处理中，随时可以调用。|  
|当任务或容器收到 **OnVariableValueChanged** 事件时。|变量值更改时，由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 运行时调用。 变量的 RaiseChangeEvent 必须设置为 **true** 才能引发此事件。<br /><br /> &#42;&#42; 警告 &#42;&#42; 必须在容器范围定义与此断点关联的变量   。 如果在包作用域定义变量，则不会命中断点。|  
|当任务或容器收到 **OnCustomEvent** 事件时。|由任务调用，用于引发自定义的任务定义事件。|  
  
 除了对所有任务和容器可用的中断条件外，有些任务和容器还包含用来设置断点的特殊中断条件。 例如，可以在 For 循环容器上启用中断条件，设置一个在循环的每个迭代开始时挂起执行的断点。  
  
 若要增加断点的灵活性和能力，可通过指定下列选项来修改断点的行为：  
  
-   命中计数，即执行挂起之前中断条件出现的最大次数。  
  
-   命中计数类型，或指定中断条件何时触发断点的规则。  
  
 命中计数类型（除“始终”类型以外）由命中计数进行进一步限定。 例如，如果类型是“命中计数等于”并且命中计数是 5，则在中断条件第六次出现时挂起执行。  
  
 下表介绍命中计数类型。  
  
|命中计数类型|说明|  
|--------------------|-----------------|  
|始终|断点命中时始终挂起执行。|  
|命中计数等于|断点发生的次数等于命中计数时挂起执行。|  
|命中计数大于或等于|断点发生的次数等于或大于命中计数时挂起执行。|  
|命中计数倍增基数|断点发生的次数为命中计数的倍数时挂起执行。 例如，如果将此选项设置为 5，则每命中五次就挂起执行。|  
  
#### <a name="to-set-breakpoints"></a>设置断点  
  
-   [通过在任务或容器上设置断点调试包](#debug)  
  
## <a name="progress-reporting"></a>进度报告  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器包含两种类型进度报告：“控制流”选项卡设计图面上的颜色编码和“进度”选项卡上的进度消息   。  
  
 运行包时， [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器使用可指示执行状态的颜色显示每个任务或容器，以此来描绘执行进度。 可根据颜色判断元素是否正在等待运行、是否当前正在运行、是否已成功完成或者已经以失败结束。 停止包执行后，颜色编码将消失。  
  
 下表介绍用于描绘执行状态的各种颜色。  
  
|Color|执行状态|  
|-----------|----------------------|  
|灰色|等待运行|  
|Yellow|正在运行|  
|绿色|成功运行|  
|已突出显示|运行，但有错误|  
  
 **“进度”** 选项卡上按执行顺序列出了任务和容器，并包含了开始时间和结束时间、警告以及错误信息。 停止包执行后，在 **“执行结果”** 选项卡上的进度信息仍然可用。  
  
> [!NOTE]  
>  若要允许或禁止在 **“进度”** 选项卡上显示消息，请在 **SSIS** 菜单上切换 **“调试进度报告”** 选项。  
  
 下面的关系图显示 **“进度”** 选项卡。  
  
 ![SSIS 设计器的“进度”选项卡](../../integration-services/troubleshooting/media/mw-dtsflow04.gif "SSIS 设计器的“进度”选项卡")  
  
## <a name="debug-windows"></a>调试窗口  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 包含多个窗口，可以用来处理断点，以及用来调试包含断点的包。 若要了解每个窗口的更多信息，请打开该窗口，然后按 F1 显示该窗口的帮助。  
  
 若要在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中打开这些窗口，请单击 **“调试”** 菜单，指向 **“窗口”** ，然后单击 **“断点”** 、 **“输出”** 或 **“即时”** 。  
  
 下表介绍这些窗口。  
  
|窗口|说明|  
|------------|-----------------|  
|断点|列出包中的断点并提供启用和删除断点的选项。|  
|输出|显示 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中各功能的状态消息。|  
|即时|用于调试和评估表达式，并打印变量值。|  

## <a name="debug-a-package-by-setting-breakpoints-on-a-task-or-a-container"></a><a name="debug"></a> 通过在任务或容器上设置断点调试包
  本过程介绍如何在包、任务、For 循环容器、Foreach 循环容器或序列容器中设置断点。  
  
### <a name="to-set-breakpoints-in-a-package-a-task-or-a-container"></a>在包、任务或容器中设置断点  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  双击包含要在其中设置断点的包。  
  
3.  在 SSIS 设计器中，执行下列操作：  
  
    -   若要在包对象中设置断点，请单击“控制流”选项卡，将光标置于设计图面背景的任意位置，右键单击，再单击“编辑断点”   。  
  
    -   若要在包控制流中设置断点，请单击“控制流”选项卡，右键单击任务、For 循环容器、Foreach 循环容器或序列容器，再单击“编辑断点”   。  
  
    -   若要在事件处理程序中设置断点，请单击“事件处理程序”选项卡，右键单击任务、For 循环容器、Foreach 循环容器或序列容器，再单击“编辑断点”   。  
  
4.  在“设置断点 \<container name>”对话框中，选择要启用的断点。  
  
5.  还可以修改每个断点的命中计数类型和命中计数数量。  
  
6.  若要保存包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  

## <a name="set-breakpoints"></a>“设置断点”
  可以使用 **“设置断点”** 对话框，指定要启用断点和控制断点行为的事件。  
  
### <a name="options"></a>选项  
 **已启用**  
 选择此选项可以对事件启用断点。  
  
 **Break Condition**  
 查看可设置断点的事件列表。  
  
 **Hit Count Type**  
 指定断点生效的时间。  
  
|值|说明|  
|-----------|-----------------|  
|**始终**|断点命中时始终挂起执行。|  
|**命中计数等于**|断点发生的次数等于命中计数时挂起执行。|  
|**命中计数大于或等于**|断点发生的次数等于或大于命中计数时挂起执行。|  
|**命中计数倍增基数**|断点发生的次数为命中计数的倍数时挂起执行。 例如，如果将此选项设置为 5，则每命中五次就挂起执行。|  
  
 **命中计数**  
 指定触发中断的命中次数。 如果断点始终有效，则此选项将不可用。  
  
## <a name="see-also"></a>另请参阅  
 [包开发的疑难解答工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)  
 [通过在脚本任务和脚本组件中设置断点来调试脚本](../../integration-services/extending-packages-scripting/debug-a-script-by-setting-breakpoints-in-a-script-task-and-script-component.md)   
