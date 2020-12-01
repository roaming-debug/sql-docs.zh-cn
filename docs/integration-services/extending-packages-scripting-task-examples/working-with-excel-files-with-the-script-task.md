---
description: 使用脚本任务处理 Excel 文件
title: 使用脚本任务处理 Excel 文件 | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], Excel files
- Script task [Integration Services], examples
- Excel [Integration Services]
ms.assetid: b8fa110a-2c9c-4f5a-8fe1-305555640e44
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1016c9746ba1cfe266ef944eeb66f7c0327d69dd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88430349"
---
# <a name="working-with-excel-files-with-the-script-task"></a>使用脚本任务处理 Excel 文件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供了 Excel 连接管理器、Excel 源和 Excel 目标，用于处理以 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel 文件格式存储在电子表格中的数据。 本主题中介绍的技术使用脚本任务获取有关可用的 Excel 数据库（工作簿文件）和表（工作表和指定范围）的信息。
  
> [!IMPORTANT]
> 有关连接到 Excel 文件的详细信息，以及从 Excel 文件加载数据或将数据加载到 Excel 文件的限制和已知问题，请参阅[使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)。
 
> [!TIP]  
>  如果希望创建可重用于多个包的任务，请考虑以此脚本任务示例中的代码为基础，创建自定义任务。 有关详细信息，请参阅 [开发自定义任务](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)。  

##  <a name="configuring-a-package-to-test-the-samples"></a><a name="configuring"></a> 配置用于测试示例的包  
 您可以配置单个包来测试本主题中的所有示例。 这些示例使用许多相同的包变量和相同的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 类。  
  
### <a name="to-configure-a-package-for-use-with-the-examples-in-this-topic"></a>将包配置为用于本主题中的示例  
  
1.  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中创建新的 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 项目，并打开默认的包进行编辑。  
  
2.  **变量**。 打开“变量”窗口并定义以下变量：  
  
    -   `ExcelFile`，类型为字符串。 输入现有 Excel 工作簿的完整路径和文件名。  
  
    -   `ExcelTable`，类型为字符串。 输入以 `ExcelFile` 变量值命名的工作簿中的现有工作簿或指定范围的名称。 此值区分大小写。  
  
    -   `ExcelFileExists`，类型为布尔  。  
  
    -   `ExcelTableExists`，类型为布尔。  
  
    -   `ExcelFolder`，类型为字符串。 输入至少包含一个 Excel 工作簿的文件夹的完整路径。  
  
    -   `ExcelFiles`，类型为对象  。  
  
    -   `ExcelTables`，类型为对象。  
  
3.  Imports 语句。 多数代码示例都要求您在脚本文件的开头处导入一个或两个下列 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 命名空间：  
  
    -   System.IO，用于文件系统操作。  
  
    -   System.Data.OleDb，用于将 Excel 文件作为数据源打开。  
  
4.  引用。 从 Excel 文件读取构架信息的代码示例在脚本项目中需要对 System.Xml 命名空间的附加引用。  
  
5.  设置脚本组件的默认脚本语言，方法是使用“选项”对话框的“常规”页上的“脚本语言”选项。 有关详细信息，请参阅 [General Page](../general-page-of-integration-services-designers-options.md)。  
  
##  <a name="example-1-description-check-whether-an-excel-file-exists"></a><a name="example1"></a> 示例 1 说明：检查 Excel 文件是否存在  
 本示例可确定 `ExcelFile` 变量中指定的 Excel 工作簿文件是否存在，然后根据该结果设置 `ExcelFileExists` 变量的布尔值。 可以使用此布尔值在包的工作流中进行分支。  
  
### <a name="to-configure-this-script-task-example"></a>配置此脚本任务示例  
  
1.  将新的脚本添加到包中，并将其名称更改为 ExcelFileExists。  
  
2.  在“脚本任务编辑器”的“脚本”选项卡上，单击“ReadOnlyVariables”，然后使用下列任一方法输入属性值：  
  
    -   键入 ExcelFile。  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“选择变量”对话框中选择“ExcelFile”变量  。  
  
3.  单击“ReadWriteVariables”，然后使用下列任一方法输入属性值：  
  
    -   键入 ExcelFileExists。  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“选择变量”对话框中选择“ExcelFileExists”变量。  
  
4.  单击“编辑脚本”以打开脚本编辑器。  
  
5.  在脚本文件的顶部添加针对 System.IO 命名空间的 Imports 语句。  
  
6.  添加以下代码。  
  
### <a name="example-1-code"></a>示例 1 代码  
  
```vb  
Public Class ScriptMain  
  Public Sub Main()  
    Dim fileToTest As String  
  
    fileToTest = Dts.Variables("ExcelFile").Value.ToString  
    If File.Exists(fileToTest) Then  
      Dts.Variables("ExcelFileExists").Value = True  
    Else  
      Dts.Variables("ExcelFileExists").Value = False  
    End If  
  
    Dts.TaskResult = ScriptResults.Success  
  End Sub  
End Class  
```  
  
```csharp  
public class ScriptMain  
{  
  public void Main()  
  {  
    string fileToTest;  
  
    fileToTest = Dts.Variables["ExcelFile"].Value.ToString();  
    if (File.Exists(fileToTest))  
    {  
      Dts.Variables["ExcelFileExists"].Value = true;  
    }  
    else  
    {  
      Dts.Variables["ExcelFileExists"].Value = false;  
    }  
  
    Dts.TaskResult = (int)ScriptResults.Success;  
  }  
}  
```  
  
##  <a name="example-2-description-check-whether-an-excel-table-exists"></a><a name="example2"></a> 示例 2 说明：检查 Excel 表是否存在  
 本示例可确定 `ExcelTable` 变量中指定的 Excel 工作表或命名范围是否存在于 `ExcelFile` 变量中指定的 Excel 工作簿文件中，然后根据该结果设置 `ExcelTableExists` 变量的布尔值。 可以使用此布尔值在包的工作流中进行分支。  
  
### <a name="to-configure-this-script-task-example"></a>配置此脚本任务示例  
  
1.  将新的脚本任务添加到包，并将其名称更改为 ExcelTableExists。  
  
2.  在“脚本任务编辑器”的“脚本”选项卡上，单击“ReadOnlyVariables”，然后使用下列任一方法输入属性值：  
  
    -   键入用逗号. 分隔的 ExcelTable 和 ExcelFile。  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“选择变量”对话框中选择“ExcelTable”和“ExcelFile”变量。  
  
3.  单击“ReadWriteVariables”，然后使用下列任一方法输入属性值：  
  
    -   键入 ExcelTableExists。  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“选择变量”对话框中选“ExcelTableExists”变量。  
  
4.  单击“编辑脚本”以打开脚本编辑器。  
  
5.  在脚本项目中添加对 System.Xml 程序集的引用。  
  
6.  在脚本文件的顶部添加针对 System.IO 和 System.Data.OleDb 命名空间的 Imports 语句。  
  
7.  添加以下代码。  
  
### <a name="example-2-code"></a>示例 2 代码  
  
```vb  
Public Class ScriptMain  
  Public Sub Main()  
    Dim fileToTest As String  
    Dim tableToTest As String  
    Dim connectionString As String  
    Dim excelConnection As OleDbConnection  
    Dim excelTables As DataTable  
    Dim excelTable As DataRow  
    Dim currentTable As String  
  
    fileToTest = Dts.Variables("ExcelFile").Value.ToString  
    tableToTest = Dts.Variables("ExcelTable").Value.ToString  
  
    Dts.Variables("ExcelTableExists").Value = False  
    If File.Exists(fileToTest) Then  
      connectionString = "Provider=Microsoft.ACE.OLEDB.12.0;" & _  
        "Data Source=" & fileToTest & _  
        ";Extended Properties=Excel 12.0"  
      excelConnection = New OleDbConnection(connectionString)  
      excelConnection.Open()  
      excelTables = excelConnection.GetSchema("Tables")  
      For Each excelTable In excelTables.Rows  
        currentTable = excelTable.Item("TABLE_NAME").ToString  
        If currentTable = tableToTest Then  
          Dts.Variables("ExcelTableExists").Value = True  
        End If  
      Next  
    End If  
  
    Dts.TaskResult = ScriptResults.Success  
  End Sub  
End Class  
```  
  
```csharp  
public class ScriptMain  
{  
    public void Main()  
        {  
            string fileToTest;  
            string tableToTest;  
            string connectionString;  
            OleDbConnection excelConnection;  
            DataTable excelTables;  
            string currentTable;  
  
            fileToTest = Dts.Variables["ExcelFile"].Value.ToString();  
            tableToTest = Dts.Variables["ExcelTable"].Value.ToString();  
  
            Dts.Variables["ExcelTableExists"].Value = false;  
            if (File.Exists(fileToTest))  
            {  
                connectionString = "Provider=Microsoft.ACE.OLEDB.12.0;" +  
                "Data Source=" + fileToTest + ";Extended Properties=Excel 12.0";  
                excelConnection = new OleDbConnection(connectionString);  
                excelConnection.Open();  
                excelTables = excelConnection.GetSchema("Tables");  
                foreach (DataRow excelTable in excelTables.Rows)  
                {  
                    currentTable = excelTable["TABLE_NAME"].ToString();  
                    if (currentTable == tableToTest)  
                    {  
                        Dts.Variables["ExcelTableExists"].Value = true;  
                    }  
                }  
            }  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
}  
```  
  
##  <a name="example-3-description-get-a-list-of-excel-files-in-a-folder"></a><a name="example3"></a> 示例 3 说明：获取文件夹中的 Excel 文件的列表  
 本示例使用由 `ExcelFolder` 变量值指定的文件夹中找到的 Excel 文件的列表来填充数组，然后将该数组复制到 `ExcelFiles` 变量。 您可以使用 Foreach 源变量枚举器循环访问数组中的文件。  
  
### <a name="to-configure-this-script-task-example"></a>配置此脚本任务示例  
  
1.  将新的脚本任务添加到包，并将其名称更改为 GetExcelFiles。  
  
2.  打开“脚本任务编辑器”，在“脚本”选项卡上单击“ReadOnlyVariables”，然后使用下列任一方法输入属性值：  
  
    -   键入 ExcelFolder  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“选择变量”对话框中选择“ExcelFolder”变量。  
  
3.  单击“ReadWriteVariables”，然后使用下列任一方法输入属性值：  
  
    -   键入 ExcelFiles。  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“选择变量”对话框中选择“ExcelFile”变量。  
  
4.  单击“编辑脚本”以打开脚本编辑器。  
  
5.  在脚本文件的顶部添加针对 System.IO 命名空间的 Imports 语句。  
  
6.  添加以下代码。  
  
### <a name="example-3-code"></a>示例 3 代码  
  
```vb  
Public Class ScriptMain  
  Public Sub Main()  
    Const FILE_PATTERN As String = "*.xlsx"  
  
    Dim excelFolder As String  
    Dim excelFiles As String()  
  
    excelFolder = Dts.Variables("ExcelFolder").Value.ToString  
    excelFiles = Directory.GetFiles(excelFolder, FILE_PATTERN)  
  
    Dts.Variables("ExcelFiles").Value = excelFiles  
  
    Dts.TaskResult = ScriptResults.Success  
  End Sub  
End Class  
```  
  
```csharp  
public class ScriptMain  
{  
  public void Main()  
  {  
    const string FILE_PATTERN = "*.xlsx";  
  
    string excelFolder;  
    string[] excelFiles;  
  
    excelFolder = Dts.Variables["ExcelFolder"].Value.ToString();  
    excelFiles = Directory.GetFiles(excelFolder, FILE_PATTERN);  
  
    Dts.Variables["ExcelFiles"].Value = excelFiles;  
  
    Dts.TaskResult = (int)ScriptResults.Success;  
  }  
}  
```  
  
### <a name="alternate-solution"></a>备用解决方案  
 如果不使用脚本任务将 Excel 文件列表收集到数组中，则还可以使用 ForEach 文件枚举器来循环访问文件夹中的所有 Excel 文件。 有关详细信息，请参阅[使用 Foreach 循环容器循环遍历 Excel 文件和表](../../integration-services/control-flow/loop-through-excel-files-and-tables-by-using-a-foreach-loop-container.md)。  
  
##  <a name="example-4-description-get-a-list-of-tables-in-an-excel-file"></a><a name="example4"></a> 示例 4 说明：获取 Excel 文件中的表的列表  
 本示例使用在 Excel 工作簿文件中找到的由 `ExcelFile` 变量值指定的工作表和指定范围列表来填充数组，然后将该数组复制到 `ExcelTables` 变量。 您可以使用 Foreach 源变量枚举器循环访问数组中的表。  
  
> [!NOTE]  
>  Excel 工作簿中的表列表同时包括工作表（具有 $ 后缀）和指定范围。 如果要从列表中只筛选出工作表或指定范围，则必须添加其他代码来实现这一点。  
  
### <a name="to-configure-this-script-task-example"></a>配置此脚本任务示例  
  
1.  将新的脚本任务添加到包中，并将其名称更改为 GetExcelTables。  
  
2.  打开“脚本任务编辑器”，在“脚本”选项卡上单击“ReadOnlyVariables”，然后使用下列任一方法输入属性值：  
  
    -   键入 ExcelFile。  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“选择变量”对话框中选择“ExcelFile”变量。  
  
3.  单击“ReadWriteVariables”，然后使用下列任一方法输入属性值：  
  
    -   键入 ExcelTables。  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“选择变量”对话框中选择“ExcelTables”变量。  
  
4.  单击“编辑脚本”以打开脚本编辑器。  
  
5.  在脚本项目中添加对 System.Xml 命名空间的引用。  
  
6.  在脚本文件的顶部添加针对 System.Data.OleDb 命名空间的 Imports 语句。  
  
7.  添加以下代码。  
  
### <a name="example-4-code"></a>示例 4 代码  
  
```vb  
Public Class ScriptMain  
  Public Sub Main()  
    Dim excelFile As String  
    Dim connectionString As String  
    Dim excelConnection As OleDbConnection  
    Dim tablesInFile As DataTable  
    Dim tableCount As Integer = 0  
    Dim tableInFile As DataRow  
    Dim currentTable As String  
    Dim tableIndex As Integer = 0  
  
    Dim excelTables As String()  
  
    excelFile = Dts.Variables("ExcelFile").Value.ToString  
    connectionString = "Provider=Microsoft.ACE.OLEDB.12.0;" & _  
        "Data Source=" & excelFile & _  
        ";Extended Properties=Excel 12.0"  
    excelConnection = New OleDbConnection(connectionString)  
    excelConnection.Open()  
    tablesInFile = excelConnection.GetSchema("Tables")  
    tableCount = tablesInFile.Rows.Count  
    ReDim excelTables(tableCount - 1)  
    For Each tableInFile In tablesInFile.Rows  
      currentTable = tableInFile.Item("TABLE_NAME").ToString  
      excelTables(tableIndex) = currentTable  
      tableIndex += 1  
    Next  
  
    Dts.Variables("ExcelTables").Value = excelTables  
  
    Dts.TaskResult = ScriptResults.Success  
  End Sub  
End Class  
```  
  
```csharp  
public class ScriptMain  
{  
  public void Main()  
        {  
            string excelFile;  
            string connectionString;  
            OleDbConnection excelConnection;  
            DataTable tablesInFile;  
            int tableCount = 0;  
            string currentTable;  
            int tableIndex = 0;  
  
            string[] excelTables = new string[5];  
  
            excelFile = Dts.Variables["ExcelFile"].Value.ToString();  
            connectionString = "Provider=Microsoft.ACE.OLEDB.12.0;" +  
                "Data Source=" + excelFile + ";Extended Properties=Excel 12.0";  
            excelConnection = new OleDbConnection(connectionString);  
            excelConnection.Open();  
            tablesInFile = excelConnection.GetSchema("Tables");  
            tableCount = tablesInFile.Rows.Count;  
  
            foreach (DataRow tableInFile in tablesInFile.Rows)  
            {  
                currentTable = tableInFile["TABLE_NAME"].ToString();  
                excelTables[tableIndex] = currentTable;  
                tableIndex += 1;  
            }  
  
            Dts.Variables["ExcelTables"].Value = excelTables;  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
}  
```  
  
### <a name="alternate-solution"></a>备用解决方案  
 如果不使用脚本任务将 Excel 表列表收集到数组中，则还可以使用 ForEach ADO.NET 架构行集枚举器来循环访问 Excel 工作簿文件中的所有表（即，工作表和指定范围）。 有关详细信息，请参阅[使用 Foreach 循环容器循环遍历 Excel 文件和表](../../integration-services/control-flow/loop-through-excel-files-and-tables-by-using-a-foreach-loop-container.md)。  
  
##  <a name="displaying-the-results-of-the-samples"></a><a name="testing"></a> 显示示例的结果  
 如果已在同一个包中配置本主题的所有示例，则可以将所有脚本任务连接到用于显示所有示例输出的其他脚本任务。  
  
### <a name="to-configure-a-script-task-to-display-the-output-of-the-examples-in-this-topic"></a>配置用于显示本主题中的示例输出的脚本任务  
  
1.  将新的脚本任务添加到包中，并将其名称更改为 DisplayResults。  
  
2.  将四个示例脚本任务彼此连接，以便每个任务在前一个任务成功完成后运行，然后将第四个示例任务连接到 DisplayResults 任务。  
  
3.  在“脚本任务编辑器”中打开 DisplayResults 任务。  
  
4.  在“脚本”选项卡中单击“ReadOnlyVariables”然后使用下列任一方法添加[配置用于测试示例的包](#configuring)中列出的七个变量：  
  
    -   键入用逗号分隔的每个变量的名称。  
  
         - 或 -  
  
    -   单击属性字段旁的省略号 (…) 按钮，然后在“”选择变量对话框中选择变量。  
  
5.  单击“编辑脚本”以打开脚本编辑器。  
  
6.  在脚本文件的顶部添加针对 Microsoft.VisualBasic 和 System.Windows.Forms 命名空间的 Imports 语句。  
  
7.  添加以下代码。  
  
8.  运行包，然后检查消息框中显示的结果。  
  
### <a name="code-to-display-the-results"></a>编写显示结果的代码  
  
```vb  
Public Class ScriptMain  
  Public Sub Main()  
    Const EOL As String = ControlChars.CrLf  
  
    Dim results As String  
    Dim filesInFolder As String()  
    Dim fileInFolder As String  
    Dim tablesInFile As String()  
    Dim tableInFile As String  
  
    results = _  
      "Final values of variables:" & EOL & _  
      "ExcelFile: " & Dts.Variables("ExcelFile").Value.ToString & EOL & _  
      "ExcelFileExists: " & Dts.Variables("ExcelFileExists").Value.ToString & EOL & _  
      "ExcelTable: " & Dts.Variables("ExcelTable").Value.ToString & EOL & _  
      "ExcelTableExists: " & Dts.Variables("ExcelTableExists").Value.ToString & EOL & _  
      "ExcelFolder: " & Dts.Variables("ExcelFolder").Value.ToString & EOL & _  
      EOL  
  
    results &= "Excel files in folder: " & EOL  
    filesInFolder = DirectCast(Dts.Variables("ExcelFiles").Value, String())  
    For Each fileInFolder In filesInFolder  
      results &= " " & fileInFolder & EOL  
    Next  
    results &= EOL  
  
    results &= "Excel tables in file: " & EOL  
    tablesInFile = DirectCast(Dts.Variables("ExcelTables").Value, String())  
    For Each tableInFile In tablesInFile  
      results &= " " & tableInFile & EOL  
    Next  
  
    MessageBox.Show(results, "Results", MessageBoxButtons.OK, MessageBoxIcon.Information)  
  
    Dts.TaskResult = ScriptResults.Success  
  End Sub  
End Class  
```  
  
```csharp  
public class ScriptMain  
{  
  public void Main()  
        {  
            const string EOL = "\r";  
  
            string results;  
            string[] filesInFolder;  
            //string fileInFolder;  
            string[] tablesInFile;  
            //string tableInFile;  
  
            results = "Final values of variables:" + EOL + "ExcelFile: " + Dts.Variables["ExcelFile"].Value.ToString() + EOL + "ExcelFileExists: " + Dts.Variables["ExcelFileExists"].Value.ToString() + EOL + "ExcelTable: " + Dts.Variables["ExcelTable"].Value.ToString() + EOL + "ExcelTableExists: " + Dts.Variables["ExcelTableExists"].Value.ToString() + EOL + "ExcelFolder: " + Dts.Variables["ExcelFolder"].Value.ToString() + EOL + EOL;  
  
            results += "Excel files in folder: " + EOL;  
            filesInFolder = (string[])(Dts.Variables["ExcelFiles"].Value);  
            foreach (string fileInFolder in filesInFolder)  
            {  
                results += " " + fileInFolder + EOL;  
            }  
            results += EOL;  
  
            results += "Excel tables in file: " + EOL;  
            tablesInFile = (string[])(Dts.Variables["ExcelTables"].Value);  
            foreach (string tableInFile in tablesInFile)  
            {  
                results += " " + tableInFile + EOL;  
            }  
  
            MessageBox.Show(results, "Results", MessageBoxButtons.OK, MessageBoxIcon.Information);  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)  
 [使用 Foreach 循环容器循环遍历 Excel 文件和表](../../integration-services/control-flow/loop-through-excel-files-and-tables-by-using-a-foreach-loop-container.md)  
  
  
