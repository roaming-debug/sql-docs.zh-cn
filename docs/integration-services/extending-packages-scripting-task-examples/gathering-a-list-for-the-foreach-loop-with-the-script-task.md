---
description: 使用脚本任务为 Foreach 循环收集列表
title: 使用脚本任务为 ForEach 循环收集列表 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Foreach Loop containers
- Script task [Integration Services], Foreach loops
- Script task [Integration Services], examples
- SSIS Script task, Foreach loops
ms.assetid: 694f0462-d0c5-4191-b64e-821b1bdef055
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e8ff797225d3a97673bf0f1b108ce15eb6e8bd52
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193097"
---
# <a name="gathering-a-list-for-the-foreach-loop-with-the-script-task"></a>使用脚本任务为 Foreach 循环收集列表

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  变量枚举器的 Foreach 枚举通过变量传递给它的各列表项，并对每一项执行相同的任务。 您可以在脚本任务中使用自定义代码来填充用于此目的的列表。 有关枚举器的详细信息，请参阅 [Foreach 循环容器](../../integration-services/control-flow/foreach-loop-container.md)。  
  
> [!NOTE]  
>  如果希望创建可更方便地重用于多个包的任务，请考虑以此脚本任务示例中的代码为基础，创建自定义任务。 有关详细信息，请参阅 [开发自定义任务](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)。  
  
## <a name="description"></a>说明  
 下面的示例使用 **System.IO** 命名空间中的方法，收集计算机中 Excel 工作簿的一个列表，其中的工作簿存在天数大于或小于用户在变量中指定的天数。 它会以递归方式搜索驱动器 C 的各目录中扩展名为 .xls 的文件，并检查每个文件的最新修改日期，以确定该文件是否属于此列表。 它会将符合要求的文件添加到 **ArrayList** 中，并将 **ArrayList** 保存到一个变量中，供以后在 Foreach 循环容器中使用。 Foreach 循环容器配置为使用变量枚举器的 Foreach。  
  
> [!NOTE]  
>  用于变量枚举器的 Foreach 的变量必须为 **Object** 类型。 放入该变量中的对象必须实现以下接口之一：**System.Collections.IEnumerable**、**System.Runtime.InteropServices.ComTypes.IEnumVARIANT**、**System.ComponentModel IListSource** 或 **Microsoft.SqlServer.Dts.Runtime.Wrapper.ForEachEnumeratorHost**。 通常会使用 **Array** 或 **ArrayList**。 **ArrayList** 需要引用 **System.Collections** 命名空间，因此需要针对该命名空间的 **Imports** 语句。  
  
 您可以对 `FileAge` 包变量使用不同的正值和负值来试用此任务。 例如，可以输入 5 以搜索最近 5 天内创建的文件，或者输入 -3 以搜索 3 天以前创建的文件。 对于要搜索较多文件夹的驱动器，此任务可能会花费一两分钟时间。  
  
#### <a name="to-configure-this-script-task-example"></a>配置此脚本任务示例  
  
1.  创建一个名为 `FileAge` 的 Integer 类型包变量，并输入一个正整数值或负整数值。 该值为正时，代码将搜索指定天数之内创建的文件；该值为负时，代码将搜索指定天数之前创建的文件。  
  
2.  创建一个名为 `FileList` 的 **Object** 类型的包变量，以接收脚本任务收集的文件列表，供变量枚举器的 Foreach 以后使用。  
  
3.  将 `FileAge` 变量添加到脚本任务的 **ReadOnlyVariables** 属性中，将 `FileList` 变量添加到 **ReadWriteVariables** 属性中。  
  
4.  在代码中，导入 **System.Collections** 和 **System.IO** 命名空间。  
  
### <a name="code"></a>代码  
  
```vb  
Imports System  
Imports System.Data  
Imports System.Math  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports System.Collections  
Imports System.IO  
  
Public Class ScriptMain  
  
  Private Const FILE_AGE As Integer = -50  
  
  Private Const FILE_ROOT As String = "C:\"  
  Private Const FILE_FILTER As String = "*.xls"  
  
  Private isCheckForNewer As Boolean = True  
  Dim fileAgeLimit As Integer  
  Private listForEnumerator As ArrayList  
  
  Public Sub Main()  
  
    fileAgeLimit = DirectCast(Dts.Variables("FileAge").Value, Integer)  
  
    ' If value provided is positive, we want files NEWER THAN n days.  
    '  If negative, we want files OLDER THAN n days.  
    If fileAgeLimit < 0 Then  
      isCheckForNewer = False  
    End If  
    ' Extract number of days as positive integer.  
    fileAgeLimit = Math.Abs(fileAgeLimit)  
  
    listForEnumerator = New ArrayList  
  
    GetFilesInFolder(FILE_ROOT)  
  
    ' Return the list of files to the variable  
    '  for later use by the Foreach from Variable enumerator.  
    System.Windows.Forms.MessageBox.Show("Matching files: " & listForEnumerator.Count.ToString, "Results", Windows.Forms.MessageBoxButtons.OK, Windows.Forms.MessageBoxIcon.Information)  
    Dts.Variables("FileList").Value = listForEnumerator  
  
    Dts.TaskResult = ScriptResults.Success  
  
  End Sub  
  
  Private Sub GetFilesInFolder(ByVal folderPath As String)  
  
    Dim localFiles() As String  
    Dim localFile As String  
    Dim fileChangeDate As Date  
    Dim fileAge As TimeSpan  
    Dim fileAgeInDays As Integer  
    Dim childFolder As String  
  
    Try  
      localFiles = Directory.GetFiles(folderPath, FILE_FILTER)  
      For Each localFile In localFiles  
        fileChangeDate = File.GetLastWriteTime(localFile)  
        fileAge = DateTime.Now.Subtract(fileChangeDate)  
        fileAgeInDays = fileAge.Days  
        CheckAgeOfFile(localFile, fileAgeInDays)  
      Next  
  
      If Directory.GetDirectories(folderPath).Length > 0 Then  
        For Each childFolder In Directory.GetDirectories(folderPath)  
          GetFilesInFolder(childFolder)  
        Next  
      End If  
  
    Catch  
      ' Ignore exceptions on special folders such as System Volume Information.  
    End Try  
  
  End Sub  
  
  Private Sub CheckAgeOfFile(ByVal localFile As String, ByVal fileAgeInDays As Integer)  
  
    If isCheckForNewer Then  
      If fileAgeInDays <= fileAgeLimit Then  
        listForEnumerator.Add(localFile)  
      End If  
    Else  
      If fileAgeInDays > fileAgeLimit Then  
        listForEnumerator.Add(localFile)  
      End If  
    End If  
  
  End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Data;  
using System.Math;  
using Microsoft.SqlServer.Dts.Runtime;  
using System.Collections;  
using System.IO;  
  
public partial class ScriptMain : Microsoft.SqlServer.Dts.Tasks.ScriptTask.VSTARTScriptObjectModelBase  
    {  
  
        private const int FILE_AGE = -50;  
  
        private const string FILE_ROOT = "C:\\";  
        private const string FILE_FILTER = "*.xls";  
  
        private bool isCheckForNewer = true;  
        int fileAgeLimit;  
        private ArrayList listForEnumerator;  
  
        public void Main()  
  {  
  
    fileAgeLimit = (int)(Dts.Variables["FileAge"].Value);  
  
    // If value provided is positive, we want files NEWER THAN n days.  
    // If negative, we want files OLDER THAN n days.  
    if (fileAgeLimit<0)  
    {  
      isCheckForNewer = false;  
    }  
    // Extract number of days as positive integer.  
    fileAgeLimit = Math.Abs(fileAgeLimit);  
  
    listForEnumerator = new ArrayList();  
  
    GetFilesInFolder(FILE_ROOT);  
  
    // Return the list of files to the variable  
    // for later use by the Foreach from Variable enumerator.  
    System.Windows.Forms.MessageBox.Show("Matching files: "+ listForEnumerator.Count, "Results",   
MessageBoxButtons.OK, MessageBoxIcon.Information);  
    Dts.Variables["FileList"].Value = listForEnumerator;  
  
    Dts.TaskResult = (int)ScriptResults.Success;  
  
  }  
  
        private void GetFilesInFolder(string folderPath)  
        {  
  
            string[] localFiles;  
            DateTime fileChangeDate;  
            TimeSpan fileAge;  
            int fileAgeInDays;  
  
            try  
            {  
                localFiles = Directory.GetFiles(folderPath, FILE_FILTER);  
                foreach (string localFile in localFiles)  
                {  
                    fileChangeDate = File.GetLastWriteTime(localFile);  
                    fileAge = DateTime.Now.Subtract(fileChangeDate);  
                    fileAgeInDays = fileAge.Days;  
                    CheckAgeOfFile(localFile, fileAgeInDays);  
                }  
  
                if (Directory.GetDirectories(folderPath).Length > 0)  
                {  
                    foreach (string childFolder in Directory.GetDirectories(folderPath))  
                    {  
                        GetFilesInFolder(childFolder);  
                    }  
                }  
  
            }  
            catch  
            {  
                // Ignore exceptions on special folders, such as System Volume Information.  
            }  
  
        }  
  
        private void CheckAgeOfFile(string localFile, int fileAgeInDays)  
        {  
  
            if (isCheckForNewer)  
            {  
                if (fileAgeInDays <= fileAgeLimit)  
                {  
                    listForEnumerator.Add(localFile);  
                }  
            }  
            else  
            {  
                if (fileAgeInDays > fileAgeLimit)  
                {  
                    listForEnumerator.Add(localFile);  
                }  
            }  
  
        }  
  
    }  
```  
  
## <a name="see-also"></a>另请参阅  
 [Foreach 循环容器](../../integration-services/control-flow/foreach-loop-container.md)   
 [配置 Foreach 循环容器](../control-flow/foreach-loop-container.md)  
  
