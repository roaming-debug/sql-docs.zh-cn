---
description: 使用脚本组件创建目标
title: 使用脚本组件创建目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script component [Integration Services], destination components
- destinations [Integration Services], components
- input columns [Integration Services]
ms.assetid: 214e22e8-7e7d-4876-b690-c138e5721b81
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f02b74a6e5e28fc44a1bab9eb3f101b91610d481
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123064"
---
# <a name="creating-a-destination-with-the-script-component"></a>使用脚本组件创建目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的数据流中的目标组件用于将从上游源和转换接收的数据保存到数据源。 目标组件通常通过现有连接管理器连接数据源。  
  
 有关脚本组件的概述，请参阅[使用脚本组件扩展数据流](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md)。  
  
 脚本组件及其生成的基础结构代码可以大大简化自定义数据流组件的开发过程。 但是，若要了解脚本组件的工作方式，通读[开发自定义数据流组件](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)部分，特别是[开发自定义目标组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md)部分中的自定义数据流组件开发步骤很有帮助。  
  
## <a name="getting-started-with-a-destination-component"></a>开始一个目标组件  
 向 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的“数据流”选项卡添加脚本组件时，“选择脚本组件类型”对话框将打开并提示用户选择“源”、“目标”或“转换”脚本。 在此对话框中选择“目标”。  
  
 然后，将转换的输出连接到 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中的目标组件。 出于测试目的，可以将源直接连接到目标，而不经任何转换。  
  
## <a name="configuring-a-destination-component-in-metadata-design-mode"></a>在元数据设计模式下配置目标组件  
 选择创建目标组件的选项后，可使用“脚本转换编辑器”配置该组件。 有关详细信息，请参阅[在脚本组件编辑器中配置脚本组件](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md)。  
  
 若要选择脚本目标使用的脚本语言，请在“脚本转换编辑器”对话框的“脚本”页上设置 **ScriptLanguage** 属性。  
  
> [!NOTE]  
>  若要设置脚本组件的默认脚本语言，请使用“选项”对话框的“常规”页上的“脚本语言”选项。 有关详细信息，请参阅 [General Page](../general-page-of-integration-services-designers-options.md)。  
  
 数据流目标组件有一个输入，没有输出。 在编写自定义脚本之前，必须在元数据设计模式下完成的一个步骤是使用“脚本转换编辑器”配置组件的输入。  
  
### <a name="adding-connection-managers"></a>添加连接管理器  
 目标组件通常使用现有连接管理器连接将数据流中的数据保存到的数据源。 在“脚本转换编辑器”  的“连接管理器”  页中，单击“添加”  以添加适当的连接管理器。  
  
 但是，连接管理器只是一个封装和存储连接特定类型数据源所需信息的便利单元。 您必须编写自己的自定义代码才能加载或保存数据，并且才有可能打开和关闭与数据源的连接。  
  
 有关如何在脚本组件中使用连接管理器的常规信息，请参阅[在脚本组件中连接数据源](../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md)。  
  
 有关“脚本转换编辑器”  的“连接管理器”  页的详细信息，请参阅[脚本转换编辑器（“连接管理器”页）](../data-flow/transformations/script-component.md)。  
  
### <a name="configuring-inputs-and-input-columns"></a>配置输入和输入列  
 目标组件有一个输入，没有输出。  
  
 在“脚本转换编辑器”的“输入列”页中，列列表显示数据流上游组件的输出中可用的列。 选择要保存的列。  
  
 有关“脚本转换编辑器”的“输入列”页的详细信息，请参阅[脚本转换编辑器（“输入列”页）](../data-flow/transformations/script-component.md)。  
  
 “脚本转换编辑器”的“输入和输出”页显示单一输入，可以将其重命名。 通过使用在自动生成的代码中创建的取值函数属性，在脚本中将通过输入的名称来引用输入。  
  
 有关“脚本转换编辑器”  的“输入和输出”  页上的详细信息，请参阅[脚本转换编辑器（“输入和输出”页）](../data-flow/transformations/script-component.md)。  
  
### <a name="adding-variables"></a>添加变量  
 如果要在脚本中使用现有的变量，可以在“脚本转换编辑器”的“脚本”页上的 ReadOnlyVariables 和 ReadWriteVariables 属性字段中添加这些变量     。  
  
 在属性字段中添加多个变量时，请用逗号将变量名隔开。 还可以选择多个变量，方法是单击 ReadOnlyVariables 和 ReadWriteVariables 属性字段旁的省略号 (...) 按钮，然后在“选择变量”对话框中选择变量     。  
  
 有关如何在脚本组件中使用变量的常规信息，请参阅[在脚本组件中使用变量](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md)。  
  
 有关“脚本转换编辑器”的“脚本”页的详细信息，请参阅[脚本转换编辑器（“脚本”页）](../data-flow/transformations/script-component.md)   。  
  
## <a name="scripting-a-destination-component-in-code-design-mode"></a>在代码设计模式下编写目标组件脚本  
 为组件配置完元数据后，可以编写自定义脚本。 在“脚本转换编辑器”的“脚本”页面中，单击“编辑脚本”打开 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) IDE，可在其中添加自定义脚本    。 编写脚本所使用的语言取决于为“脚本”页上的 **ScriptLanguage** 属性选择 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic 还是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# 作为脚本语言。  
  
 有关适用于使用脚本组件创建的所有组件类型的重要信息，请参阅[脚本组件的编码和调试](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)。  
  
### <a name="understanding-the-auto-generated-code"></a>了解自动生成的代码  
 创建并配置目标组件后打开 VSTA IDE 时，可编辑的 **ScriptMain** 类会显示在代码编辑器中，其中有 **ProcessInputRow** 方法的存根。 在 **ScriptMain** 类中可编写自定义代码，**ProcessInputRow** 是目标组件中最重要的方法。  
  
 如果在 VSTA 中打开“项目资源管理器”  窗口，可以看到脚本组件还生成了只读的 **BufferWrapper** 和 **ComponentWrapper** 项目项。 **ScriptMain** 类继承自 **ComponentWrapper** 项目项中的 **UserComponent** 类。  
  
 在运行时，数据流引擎调用 UserComponent 类中的 ProcessInput 方法，该方法替代 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> 父类的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A> 方法。 而 ProcessInput  方法遍历输入缓冲区中的所有行并为每一行调用一次 ProcessInputRow  方法。  
  
### <a name="writing-your-custom-code"></a>编写自定义代码  
 若要完成自定义目标组件的创建，可能需要在 **ScriptMain** 类的以下方法中编写脚本。  
  
1.  重写 **AcquireConnections** 方法以连接到外部数据源。 从连接管理器中提取连接对象或者需要的连接信息。  
  
2.  重写 **PreExecute** 方法以准备保存数据。 例如，可能需要在此方法中创建和配置 **SqlCommand** 及其参数。  
  
3.  使用重写的 **ProcessInputRow** 方法将每个输入行复制到外部数据源。 例如，对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标，可以将列值复制到 **SqlCommand** 的参数中，并为每一行执行一次该命令。 对于平面文件目标，可以将每列的值写入 **StreamWriter**，用列分隔符将这些值隔开。  
  
4.  如果需要，重写 **PostExecute** 方法以断开与外部数据源的连接，并执行任何其他必需的清理操作。  
  
## <a name="examples"></a>示例  
 下面的示例演示在 **ScriptMain** 类中创建目标组件所需的代码。  
  
> [!NOTE]
>  这些示例使用 AdventureWorks 示例数据库中的 Person.Address 表并在数据流中传递它的第一列和第四列，即int*AddressID*和 nvarchar(30)City 列。 在本节中，在源、转换和目标示例中使用相同的数据。 每个示例的其他前提条件和假设都记录在文档中。  
  
### <a name="adonet-destination-example"></a>ADO.NET 目标示例  
 本示例演示了使用现有 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器将数据流中的数据保存到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的目标组件。  
  
 如果要运行此示例代码，必须按照如下方式配置包和组件：  
  
1.  创建使用 **SqlClient** 提供程序连接 **AdventureWorks** 数据库的 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器。  
  
2.  在 **AdventureWorks** 数据库中运行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令，以创建目标表：  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
3.  向数据流设计器图面添加新的脚本组件并将其配置为目标。  
  
4.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中将上游源或转换的输出连接到目标组件。 （无需任何转换，即可将源直接连接到目标。）此输出应提供 AdventureWorks 示例数据库的 Person.Address 表中的数据，其中至少包含 AddressID 和 City 列。  
  
5.  打开“脚本转换编辑器”  。 在“输入列”页中，选择 **AddressID** 和 **City** 输入列。  
  
6.  在“输入和输出”页中，用更具说明性的名称（如 **MyAddressInput**）重命名输入。  
  
7.  在“连接管理器”页中，添加或创建 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器，并以诸如 **MyADONETConnectionManager** 之类的名称命名。  
  
8.  在“脚本”页中，单击“编辑脚本”并输入下面的脚本   。 然后关闭脚本开发环境。  
  
9. 关闭“脚本转换编辑器”并运行该示例。  
  
```vb  
Imports System.Data.SqlClient  
...  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Dim connMgr As IDTSConnectionManager100  
    Dim sqlConn As SqlConnection  
    Dim sqlCmd As SqlCommand  
    Dim sqlParam As SqlParameter  
  
    Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
        connMgr = Me.Connections.MyADONETConnectionManager  
        sqlConn = CType(connMgr.AcquireConnection(Nothing), SqlConnection)  
  
    End Sub  
  
    Public Overrides Sub PreExecute()  
  
        sqlCmd = New SqlCommand("INSERT INTO Person.Address2(AddressID, City) " & _  
            "VALUES(@addressid, @city)", sqlConn)  
        sqlParam = New SqlParameter("@addressid", SqlDbType.Int)  
        sqlCmd.Parameters.Add(sqlParam)  
        sqlParam = New SqlParameter("@city", SqlDbType.NVarChar, 30)  
        sqlCmd.Parameters.Add(sqlParam)  
  
    End Sub  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
        With sqlCmd  
            .Parameters("@addressid").Value = Row.AddressID  
            .Parameters("@city").Value = Row.City  
            .ExecuteNonQuery()  
        End With  
    End Sub  
  
    Public Overrides Sub ReleaseConnections()  
  
        connMgr.ReleaseConnection(sqlConn)  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System.Data.SqlClient;  
public class ScriptMain:  
    UserComponent  
  
{  
    IDTSConnectionManager100 connMgr;  
    SqlConnection sqlConn;  
    SqlCommand sqlCmd;  
    SqlParameter sqlParam;  
  
    public override void AcquireConnections(object Transaction)  
    {  
  
        connMgr = this.Connections.MyADONETConnectionManager;  
        sqlConn = (SqlConnection)connMgr.AcquireConnection(null);  
  
    }  
  
    public override void PreExecute()  
    {  
  
        sqlCmd = new SqlCommand("INSERT INTO Person.Address2(AddressID, City) " +  
            "VALUES(@addressid, @city)", sqlConn);  
        sqlParam = new SqlParameter("@addressid", SqlDbType.Int);  
        sqlCmd.Parameters.Add(sqlParam);  
        sqlParam = new SqlParameter("@city", SqlDbType.NVarChar, 30);  
        sqlCmd.Parameters.Add(sqlParam);  
  
    }  
  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
        {  
            sqlCmd.Parameters["@addressid"].Value = Row.AddressID;  
            sqlCmd.Parameters["@city"].Value = Row.City;  
            sqlCmd.ExecuteNonQuery();  
        }  
    }  
  
    public override void ReleaseConnections()  
    {  
  
        connMgr.ReleaseConnection(sqlConn);  
  
    }  
  
}  
```  
  
### <a name="flat-file-destination-example"></a>平面文件目标示例  
 本示例演示了使用现有平面文件连接管理器将数据流中的数据保存到平面文件中的目标组件。  
  
 如果要运行此示例代码，必须按照如下方式配置包和组件：  
  
1.  创建连接目标文件的平面文件连接管理器。 该文件不必一定存在，目标组件会创建它。 将目标文件配置为包含 **AddressID** 和 **City** 列的逗号分隔文件。  
  
2.  向数据流设计器图面添加新的脚本组件并将其配置为目标。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中将上游源或转换的输出连接到目标组件。 （无需任何转换，即可将源直接连接到目标。）此输出应提供 **AdventureWorks** 示例数据库的 **Person.Address** 表中的数据，而且至少应包含 **AddressID** 和 **City** 列。  
  
4.  打开“脚本转换编辑器”  。 在“输入列”  页中，选择 AddressID  和 City  列。  
  
5.  在“输入和输出”页中，用更具说明性的名称（如 **MyAddressInput**）重命名输入。  
  
6.  在“连接管理器”页中，添加或创建平面文件连接管理器，并以说明性的名称命名，如 **MyFlatFileDestConnectionManager**。  
  
7.  在“脚本”页中，单击“编辑脚本”并输入下面的脚本   。 然后关闭脚本开发环境。  
  
8.  关闭“脚本转换编辑器”并运行该示例。  
  
```vb  
Imports System.IO  
...  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Dim copiedAddressFile As String  
    Private textWriter As StreamWriter  
    Private columnDelimiter As String = ","  
  
    Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
        Dim connMgr As IDTSConnectionManager100 = _  
            Me.Connections.MyFlatFileDestConnectionManager  
        copiedAddressFile = CType(connMgr.AcquireConnection(Nothing), String)  
  
    End Sub  
  
    Public Overrides Sub PreExecute()  
  
        textWriter = New StreamWriter(copiedAddressFile, False)  
  
    End Sub  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        With textWriter  
            If Not Row.AddressID_IsNull Then  
                .Write(Row.AddressID)  
            End If  
            .Write(columnDelimiter)  
            If Not Row.City_IsNull Then  
                .Write(Row.City)  
            End If  
            .WriteLine()  
        End With  
  
    End Sub  
  
    Public Overrides Sub PostExecute()  
  
        textWriter.Close()  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System.IO;  
public class ScriptMain:  
    UserComponent  
  
{  
    string copiedAddressFile;  
    private StreamWriter textWriter;  
    private string columnDelimiter = ",";  
  
    public override void AcquireConnections(object Transaction)  
    {  
  
        IDTSConnectionManager100 connMgr = this.Connections.MyFlatFileDestConnectionManager;  
        copiedAddressFile = (string) connMgr.AcquireConnection(null);  
  
    }  
  
    public override void PreExecute()  
    {  
  
        textWriter = new StreamWriter(copiedAddressFile, false);  
  
    }  
  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        {  
            if (!Row.AddressID_IsNull)  
            {  
                textWriter.Write(Row.AddressID);  
            }  
            textWriter.Write(columnDelimiter);  
            if (!Row.City_IsNull)  
            {  
                textWriter.Write(Row.City);  
            }  
            textWriter.WriteLine();  
        }  
  
    }  
  
    public override void PostExecute()  
    {  
  
        textWriter.Close();  
  
    }  
  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用脚本组件创建源](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-source-with-the-script-component.md)   
 [开发自定义目标组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md)  
  
