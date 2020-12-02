---
description: 使用脚本任务检测空平面文件
title: 使用脚本任务检测空平面文件 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- flat files
- Script task [Integration Services], empty flat files
- SSIS Script task, empty flat files
- Script task [Integration Services], examples
ms.assetid: 1b4defb8-886a-483d-8056-d1b91d37bc90
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d5c1c5473eb307480b772d2cecd31b3ebb2a9bed
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122991"
---
# <a name="detecting-an-empty-flat-file-with-the-script-task"></a>使用脚本任务检测空平面文件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  平面文件源在尝试处理平面文件之前不确定该平面文件是否包含数据行。 您可能希望跳过不含任何数据行的文件，从而提高包的效率，尤其是循环访问大量平面文件的包。 脚本任务可以在包开始处理数据流之前查找空平面文件。  
  
> [!NOTE]  
>  如果希望创建可更方便地重用于多个包的任务，请考虑以此脚本任务示例中的代码为基础，创建自定义任务。 有关详细信息，请参阅 [开发自定义任务](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)。  
  
## <a name="description"></a>说明  
 下面的示例使用 **System.IO** 命名空间中的方法来测试在平面文件连接管理器中指定的平面文件，以确定该文件是否为空，或者是否只包含预期的非数据行，比如列标题或空行。 该脚本先检查文件的大小，如果大小为零字节，则该文件是空的。 如果文件大小大于零，该脚本会从文件中读取行，直到没有行可读为止，或者直到行数超过预期的非数据行数为止。 如果该文件中的行数小于或等于预期的非数据行数，则认为该文件是空的。 结果将以布尔值的形式在用户变量中返回，该变量的值可用于在包控制流中进行分支跳转。 FireInformation 方法还将结果显示在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 的“输出”窗口中。  
  
#### <a name="to-configure-this-script-task-example"></a>配置此脚本任务示例  
  
1.  创建并配置一个名为 **EmptyFlatFileTest** 的平面文件连接管理器。  
  
2.  创建一个名为 `FFNonDataRows` 的整数变量，并将其值设置为预期在平面文件中出现的非数据行数。  
  
3.  创建一个名为 `FFIsEmpty` 的布尔变量。  
  
4.  将 `FFNonDataRows` 变量添加到脚本任务的 **ReadOnlyVariables** 属性中。  
  
5.  将 `FFIsEmpty` 变量添加到脚本任务的 **ReadWriteVariables** 属性中。  
  
6.  在代码中，导入 **System.IO** 命名空间。  
  
 如果使用的是 Foreach 文件枚举器来循环访问文件，而不是使用单个平面文件连接管理器，则需要修改下面的代码，以便从存储枚举值的变量中而不是从连接管理器获取文件名和路径。  
  
### <a name="code"></a>代码  
  
```vb  
Public Sub Main()  
  
  Dim nonDataRows As Integer = _  
      DirectCast(Dts.Variables("FFNonDataRows").Value, Integer)  
  Dim ffConnection As String = _  
      DirectCast(Dts.Connections("EmptyFlatFileTest").AcquireConnection(Nothing), _  
      String)  
  Dim flatFileInfo As New FileInfo(ffConnection)  
  ' If file size is 0 bytes, flat file does not contain data.  
  Dim fileSize As Long = flatFileInfo.Length  
  If fileSize > 0 Then  
    Dim lineCount As Integer = 0  
    Dim line As String  
    Dim fsFlatFile As New StreamReader(ffConnection)  
    Do Until fsFlatFile.EndOfStream  
      line = fsFlatFile.ReadLine  
      lineCount += 1  
      ' If line count > expected number of non-data rows,  
      '  flat file contains data (default value).  
      If lineCount > nonDataRows Then  
        Exit Do  
      End If  
      ' If line count <= expected number of non-data rows,  
      '  flat file does not contain data.  
      If lineCount <= nonDataRows Then  
        Dts.Variables("FFIsEmpty").Value = True  
      End If  
    Loop  
  Else  
    Dts.Variables("FFIsEmpty").Value = True  
  End If  
  
  Dim fireAgain As Boolean = False  
  Dts.Events.FireInformation(0, "Script Task", _  
      String.Format("{0}: {1}", ffConnection, _  
      Dts.Variables("FFIsEmpty").Value.ToString), _  
      String.Empty, 0, fireAgain)  
  
  Dts.TaskResult = ScriptResults.Success  
  
End Sub  
```  
  
```csharp  
public void Main()  
        {  
  
            int nonDataRows = (int)(Dts.Variables["FFNonDataRows"].Value);  
            string ffConnection = (string)(Dts.Connections["EmptyFlatFileTest"].AcquireConnection(null) as String);  
            FileInfo flatFileInfo = new FileInfo(ffConnection);  
            // If file size is 0 bytes, flat file does not contain data.  
            long fileSize = flatFileInfo.Length;  
            if (fileSize > 0)  
            {  
  
                int lineCount = 0;  
                string line;  
                StreamReader fsFlatFile = new StreamReader(ffConnection);  
                while (!(fsFlatFile.EndOfStream))  
                {  
                    Console.WriteLine (fsFlatFile.ReadLine());  
                    lineCount += 1;  
                    // If line count > expected number of non-data rows,  
                    //  flat file contains data (default value).  
                    if (lineCount > nonDataRows)  
                    {  
                        break;  
                    }  
                    // If line count <= expected number of non-data rows,  
                    //  flat file does not contain data.  
                    if (lineCount <= nonDataRows)  
                    {  
                        Dts.Variables["FFIsEmpty"].Value = true;  
                    }  
                }  
            }  
            else  
            {  
                Dts.Variables["FFIsEmpty"].Value = true;  
            }  
  
            bool fireAgain = false;  
            Dts.Events.FireInformation(0, "Script Task", String.Format("{0}: {1}", ffConnection, Dts.Variables["FFIsEmpty"].Value), String.Empty, 0, ref fireAgain);  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
```  
  
## <a name="see-also"></a>另请参阅  
 [脚本任务示例](../../integration-services/extending-packages-scripting-task-examples/script-task-examples.md)  
  
  
