---
description: 在脚本任务中使用变量
title: 在脚本任务中使用变量 | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Foreach Loop containers
- Script task [Integration Services], variables
- scripts [Integration Services], variables
- Variables property
- Variable object
- SSIS Script task, variables
- variables [Integration Services], Script task
ms.assetid: 593b5961-4bfa-4ce1-9531-a251c34e89d3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6e84b1f0293680d593a2427b9d95e72c36fedff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347821"
---
# <a name="using-variables-in-the-script-task"></a>在脚本任务中使用变量

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  通过变量，脚本任务可以与包中的其他对象交换数据。 有关详细信息，请参阅 [Integration Services (SSIS) 变量](../../../integration-services/integration-services-ssis-variables.md)。  
  
 脚本任务使用 Dts 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> 属性，从包中的 <xref:Microsoft.SqlServer.Dts.Runtime.Variable> 对象读取数据或向其中写入数据。  
  
> [!NOTE]  
>  <xref:Microsoft.SqlServer.Dts.Runtime.Variable> 类的 <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Value%2A> 属性的类型为 Object。 由于脚本任务启用了 Option Strict，因此在使用 <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Value%2A> 属性之前，必须先将其转换为适当的类型。  
  
 可以在“脚本任务编辑器”中向 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask.ReadOnlyVariables%2A> 和 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask.ReadWriteVariables%2A> 列表添加现有变量，以使它们可用于自定义脚本。 请注意，变量名称区分大小写。 在脚本内，可以通过 Dts 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> 属性访问这两种类型的变量。 使用 Value 属性可以从单个变量读取数据或向其中写入数据。 脚本任务在脚本读取和修改变量的值时，透明地管理锁定。  
  
 在代码中使用某个变量之前，可以使用 <xref:Microsoft.SqlServer.Dts.Runtime.Variables.Contains%2A> 属性返回的 <xref:Microsoft.SqlServer.Dts.Runtime.Variables> 集合的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> 方法来检查该变量是否存在。  
  
 还可以使用 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A> 属性 (Dts.VariableDispenser) 在脚本任务中处理变量。 使用 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A> 时，您必须在您自己的代码中处理锁定语义和变量值的数据类型转换。 如果要使用在设计时不可用，而是以编程方式在运行时创建的变量，可能需要使用 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A> 属性，而不是 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> 属性。  
  
## <a name="using-the-script-task-within-a-foreach-loop-container"></a>在 Foreach 循环容器中使用脚本任务  
 当脚本任务在 Foreach 循环容器内重复运行时，脚本通常需要使用枚举器中当前项的内容。 例如，使用 Foreach 文件枚举器时，脚本需要知道当前文件名；使用 Foreach ADO 枚举器时，脚本需要知道当前数据行中各列的内容。  
  
 Foreach 循环容器与脚本任务之间的这种通信可以通过变量实现。 在“Foreach 循环编辑器”的“变量映射”页中，可以向单个枚举项返回的每个数据项分配变量。 例如，Foreach 文件枚举器仅返回索引 0 处的一个文件名，因此只需要一个变量映射，而在每行中返回多个数据列的枚举器需要将不同的变量映射到要在脚本任务中使用的每一列。  
  
 向变量映射完枚举项后，还必须在“脚本任务编辑器”的“脚本”页中，将已建立映射的变量添加到 ReadOnlyVariables 属性，以使这些变量可用于脚本中。 有关在 Foreach 循环容器中处理文件夹中的图像文件的脚本任务示例，请参阅[使用脚本任务处理图像](../../../integration-services/extending-packages-scripting-task-examples/working-with-images-with-the-script-task.md)。  
  
## <a name="variables-example"></a>变量示例  
 下面的示例演示如何在脚本任务中访问并使用变量，以确定包工作流的路径。 该示例假定已经创建了名为 `CustomerCount` 和 `MaxRecordCount` 的整数变量，且已在“脚本任务编辑器”中将它们添加到了 ReadOnlyVariables 集合中。 `CustomerCount` 变量包含要导入的客户记录的数目。 如果其值大于 `MaxRecordCount` 的值，则脚本任务将报告失败。 如果因超过 `MaxRecordCount` 阈值而导致失败，则工作流的错误路径可实现任何所需的清除操作。  
  
 若要成功编译该示例，需要添加对 Microsoft.SqlServer.ScriptTask 程序集的引用。  
  
```vb  
Public Sub Main()  
  
    Dim customerCount As Integer  
    Dim maxRecordCount As Integer  
  
    If Dts.Variables.Contains("CustomerCount") = True AndAlso _  
        Dts.Variables.Contains("MaxRecordCount") = True Then  
  
        customerCount = _  
            CType(Dts.Variables("CustomerCount").Value, Integer)  
        maxRecordCount = _  
            CType(Dts.Variables("MaxRecordCount").Value, Integer)  
  
    End If  
  
    If customerCount > maxRecordCount Then  
            Dts.TaskResult = ScriptResults.Failure  
    Else  
            Dts.TaskResult = ScriptResults.Success  
    End If  
  
End Sub  
```  
  
```csharp  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class ScriptMain  
{  
  
    public void Main()  
    {  
        int customerCount;  
        int maxRecordCount;  
  
        if (Dts.Variables.Contains("CustomerCount")==true&&Dts.Variables.Contains("MaxRecordCount")==true)  
  
        {  
            customerCount = (int) Dts.Variables["CustomerCount"].Value;  
            maxRecordCount = (int) Dts.Variables["MaxRecordCount"].Value;  
  
        }  
  
        if (customerCount>maxRecordCount)  
        {  
            Dts.TaskResult = (int)ScriptResults.Failure;  
        }  
        else  
        {  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
  
    }  
  
}  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 变量](../../../integration-services/integration-services-ssis-variables.md)   
 [在包中使用变量](../../integration-services-ssis-variables.md)  
  
