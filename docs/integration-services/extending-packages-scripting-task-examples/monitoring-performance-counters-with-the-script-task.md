---
description: 使用脚本任务监视性能计数器
title: 使用脚本任务监视性能计数器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- performance counters [Integration Services]
- SSIS Script task, performance counters
- custom performance counters [Integration Services]
- Script task [Integration Services], examples
- Script task [Integration Services], performance counters
- counters [Integration Services]
ms.assetid: 86609bf1-cae6-435e-a58d-41bdfc521e94
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 58ca41882200dbf8689b3d3b093b2191d904dc40
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88430409"
---
# <a name="monitoring-performance-counters-with-the-script-task"></a>使用脚本任务监视性能计数器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  管理员可能需要监视对大量数据执行复杂转换的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的性能。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 的 System.Diagnostics 命名空间不但提供使用现有性能计数器的类，还提供用于创建自己性能计数器的类。  
  
 性能计数器存储应用程序的性能信息，您可以利用这些信息分析软件在一段时间内的性能。 使用“性能监视器”工具可以在本地或远程监视性能计数器。 您可以将性能计数器的值存储在变量中，用于以后在包中进行控制流分支跳转。  
  
 除了使用性能计数器，还可以通过 Dts 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A> 属性引发 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A> 事件。 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A> 事件向 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 运行时返回增量进度和完成的百分比信息。  
  
> [!NOTE]  
>  如果希望创建可更方便地重用于多个包的任务，请考虑以此脚本任务示例中的代码为基础，创建自定义任务。 有关详细信息，请参阅 [开发自定义任务](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)。  
  
## <a name="description"></a>说明  
 下面的示例创建一个自定义性能计数器并递增该计数器。 首先，该示例确定性能计数器是否已存在。 如果尚未创建性能计数器，则脚本会调用 PerformanceCounterCategory 对象的 Create 方法来创建它。 创建性能计数器后，脚本将递增该计数器。 最后，该示例依照最佳实践，在不再需要该性能计数器时，对其调用 Close 方法。  
  
> [!NOTE]  
>  创建新的性能计数器类别和性能计数器需要管理权限。 此外，新类别和计数器在创建后将持久存在于计算机中。  
  
#### <a name="to-configure-this-script-task-example"></a>配置此脚本任务示例  
  
-   使用代码中的 Imports 语句导入 System.Diagnostics 命名空间。  
  
### <a name="example-code"></a>示例代码  
  
```vb  
Public Sub Main()  
  
    Dim myCounter As PerformanceCounter  
  
    Try  
        'Create the performance counter if it does not already exist.  
        If Not _  
        PerformanceCounterCategory.Exists("TaskExample") Then  
            PerformanceCounterCategory.Create("TaskExample", _  
                "Task Performance Counter Example", "Iterations", _  
                "Number of times this task has been called.")  
        End If  
  
        'Initialize the performance counter.  
        myCounter = New PerformanceCounter("TaskExample", _  
            "Iterations", String.Empty, False)  
  
        'Increment the performance counter.  
        myCounter.Increment()  
  
         myCounter.Close()  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        Dts.Events.FireError(0, _  
            "Task Performance Counter Example", _  
            ex.Message & ControlChars.CrLf & ex.StackTrace, _  
            String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
End Sub  
```  
  
```csharp  
  
public class ScriptMain  
{  
  
public void Main()  
        {  
  
            PerformanceCounter myCounter;  
  
            try  
            {  
                //Create the performance counter if it does not already exist.  
                if (!PerformanceCounterCategory.Exists("TaskExample"))  
                {  
                    PerformanceCounterCategory.Create("TaskExample", "Task Performance Counter Example", "Iterations", "Number of times this task has been called.");  
                }  
  
                //Initialize the performance counter.  
                myCounter = new PerformanceCounter("TaskExample", "Iterations", String.Empty, false);  
  
                //Increment the performance counter.  
                myCounter.Increment();  
  
                myCounter.Close();  
                Dts.TaskResult = (int)ScriptResults.Success;  
            }  
            catch (Exception ex)  
            {  
                Dts.Events.FireError(0, "Task Performance Counter Example", ex.Message + "\r" + ex.StackTrace, String.Empty, 0);  
                Dts.TaskResult = (int)ScriptResults.Failure;  
            }  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
  
```  
