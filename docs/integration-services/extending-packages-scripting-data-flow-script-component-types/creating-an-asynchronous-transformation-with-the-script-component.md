---
description: 使用脚本组件创建异步转换
title: 使用脚本组件创建异步转换 | Microsoft Docs
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
- asynchronous outputs [Integration Services]
- transformation components [Integration Services]
- Script component [Integration Services], transformation components
ms.assetid: 0d814404-21e4-4a68-894c-96fa47ab25ae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d271139edbde0f1c67467b6fe880f916fb6c5142
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122996"
---
# <a name="creating-an-asynchronous-transformation-with-the-script-component"></a>使用脚本组件创建异步转换

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的数据流中使用转换组件可以在数据从源传递到目标时修改和分析该数据。 具有同步输出的转换在每个输入行传递给该组件时对该行进行处理。 具有异步输出的转换可能要等收到所有输入行之后才完成其处理，也可能在收到所有输入行之前输出某些行。 本主题讨论异步转换。 如果你的处理需要同步转换，请参阅[使用脚本组件创建同步转换](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)。 有关同步组件和异步组件之间的差异的详细信息，请参阅[了解同步和异步转换](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)。  
  
 有关脚本组件的概述，请参阅[使用脚本组件扩展数据流](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md)。  
  
 脚本组件及其生成的基础结构代码可以简化自定义数据流组件的开发过程。 但是，若要了解脚本组件的工作方式，通读以下步骤很有帮助，这些步骤是在[开发自定义数据流组件](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)部分，特别是在[开发具有同步输出的自定义转换组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)部分开发自定义数据流组件必须遵循的。  
  
## <a name="getting-started-with-an-asynchronous-transformation-component"></a>开始一个异步转换组件  
 向 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的“数据流”选项卡添加脚本组件时，“选择脚本组件类型”对话框会出现，提示用户将组件预配置为源、转换或目标。 在此对话框中，选择“转换”  。  
  
## <a name="configuring-an-asynchronous-transformation-component-in-metadata-design-mode"></a>在元数据设计模式下配置异步转换组件  
 选择创建转换组件的选项后，可使用“脚本转换编辑器”  配置该组件。 有关详细信息，请参阅[在脚本组件编辑器中配置脚本组件](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md)。  
  
 若要选择脚本组件使用的脚本语言，请在“脚本转换编辑器”对话框的“脚本”页设置 **ScriptLanguage** 属性。  
  
> [!NOTE]  
>  若要设置脚本组件的默认脚本语言，请使用“选项”对话框的“常规”页上的“脚本语言”选项。 有关详细信息，请参阅 [General Page](../general-page-of-integration-services-designers-options.md)。  
  
 数据流转换组件有一个输入并支持一个或多个输出。 在编写自定义脚本之前，必须在元数据设计模式下完成的一个步骤是使用“脚本转换编辑器”配置组件的输入和输出。  
  
### <a name="configuring-input-columns"></a>配置输入列  
 使用脚本组件创建的转换组件只有一个输入。  
  
 在“脚本转换编辑器”的“输入列”页上，列列表显示数据流上游组件输出中的可用列。 选择要转换或传递的列。 将要就地转换的所有列标记为读/写。  
  
 有关“脚本转换编辑器”的“输入列”页的详细信息，请参阅[脚本转换编辑器（“输入列”页）](../data-flow/transformations/script-component.md)。  
  
### <a name="configuring-inputs-outputs-and-output-columns"></a>配置输入、输出和输出列  
 转换组件支持一个或多个输出。  
  
 通常，具有异步输出的转换有两个输出。 例如，在您对位于特定城市中的地址进行计数时，可以将地址数据传递给一个输出，同时将聚合结果发送给另一个输出。 聚合输出还需要一个新的输出列。  
  
 在“脚本转换编辑器”的“输入和输出”页中，可以看到已经默认创建了一个输出，但是还没有创建任何输出列。 可以在编辑器的这一页配置下列各项：  
  
-   您可能希望创建一个或多个附加输出，如聚合结果的输出。 使用“添加输出”和“删除输出”按钮管理异步转换组件的输出。 将每个输出的 **SynchronousInputID** 属性设置为零，以指示该输出不只是传递来自上游组件的数据或在现有行和列中就地转换该数据。 此设置使输出和输入异步。  
  
-   您可以为输入和输出指定一个友好名称。 脚本组件可以使用这些名称来生成类型化取值函数属性，这些属性用于在脚本中引用输入和输出。  
  
-   通常，异步转换会向数据流添加列。 当输出的 **SynchronousInputID** 属性为零，表示该输出不只是传递来自上游组件的数据或在现有行和列中就地转换该数据时，必须对该输出显式添加和配置输出列。 输出列不必与其所映射到的输入列同名。  
  
-   您可以添加更多列来包含其他信息。 您必须编写自己的代码来向这些附加列填充数据。 有关重现标准错误输出行为的信息，请参阅[模拟脚本组件的错误输出](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md)。  
  
 有关“脚本转换编辑器”  的“输入和输出”  页上的详细信息，请参阅[脚本转换编辑器（“输入和输出”页）](../data-flow/transformations/script-component.md)。  
  
### <a name="adding-variables"></a>添加变量  
 如果要在脚本中使用任何现有变量的值，可以在“脚本转换编辑器”的“脚本”页上的 ReadOnlyVariables 和 ReadWriteVariables 属性字段中添加这些变量。  
  
 在属性字段中添加多个变量时，请用逗号将变量名隔开。 还可以选择多个变量，方法是单击 ReadOnlyVariables 和 ReadWriteVariables 属性字段旁的省略号 (...) 按钮，然后在“选择变量”对话框中选择变量     。  
  
 有关如何在脚本组件中使用变量的常规信息，请参阅[在脚本组件中使用变量](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md)。  
  
 有关“脚本转换编辑器”的“脚本”页的详细信息，请参阅[脚本转换编辑器（“脚本”页）](../data-flow/transformations/script-component.md)   。  
  
## <a name="scripting-an-asynchronous-transformation-component-in-code-design-mode"></a>在代码设计模式下编写异步转换组件脚本  
 为组件配置完所有元数据后，可以编写自定义脚本。 在“脚本转换编辑器”的“脚本”页面中，单击“编辑脚本”打开 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) IDE，可在其中添加自定义脚本    。 编写脚本所使用的语言取决于为“脚本”页上的 **ScriptLanguage** 属性选择 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic 还是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# 作为脚本语言。  
  
 有关适用于使用脚本组件创建的所有组件类型的重要信息，请参阅[脚本组件的编码和调试](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)。  
  
### <a name="understanding-the-auto-generated-code"></a>了解自动生成的代码  
 创建并配置转换组件后打开 VSTA IDE 时，可编辑的 **ScriptMain** 类显示在代码编辑器中，其中有 ProcessInputRow 和 CreateNewOutputRows 方法的存根。 在 ScriptMain 类中可编写自定义代码，ProcessInputRow 是转换组件中最重要的方法。 **CreateNewOutputRows** 方法更常用于源组件，该组件与异步转换相似，因为这两个组件都必须创建自己的输出行。  
  
 如果打开 VSTA 的“项目资源管理器”窗口，可以看到脚本组件还生成了只读的 **BufferWrapper** 和 **ComponentWrapper** 项目项。 ScriptMain 类继承自 **ComponentWrapper** 项目项中的 UserComponent 类。  
  
 在运行时，数据流引擎调用 **UserComponent** 类中的 PrimeOutput 方法，该方法替代 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> 父类的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.PrimeOutput%2A> 方法。 PrimeOutput 方法又调用 CreateNewOutputRows 方法。  
  
 随后，数据流引擎调用 UserComponent 类中的 ProcessInput 方法，该方法替代 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> 父类的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A> 方法。 而 ProcessInput 方法遍历输入缓冲区中的所有行并为每一行调用一次 ProcessInputRow 方法。  
  
### <a name="writing-your-custom-code"></a>编写自定义代码  
 若要完成自定义异步转换组件的创建，必须使用已重写的 ProcessInputRow 方法来处理输入缓冲区每一行中的数据。 由于输出与输入不同步，因此您必须向输出显式写入数据行。  
  
 在异步转换中，可以根据需要在 ProcessInputRow 或 ProcessInput 方法中使用 AddRow 方法向输出添加行。 不一定要使用 CreateNewOutputRows 方法。 如果要将单行结果（如聚合结果）写入特定输出，可以使用 CreateNewOutputRows 方法预先创建该输出行，然后在处理完所有输入行之后填充其值。 但是，在 CreateNewOutputRows 方法中创建多个行是没有任何用处的，因为脚本组件只允许使用输入或输出中的当前行。 CreateNewOutputRows 方法在源组件中更重要，源组件没有任何输入行要处理。  
  
 可能还需要重写 ProcessInput 方法本身，以便在遍历输入缓冲区并为每行调用 ProcessInputRow 之前或之后执行附加的预处理或最终处理。 例如，本主题的其中一个代码示例重写 ProcessInput，以便在 ProcessInputRow 遍历各行时对特定城市中的地址进行计数 **。** 该示例在所有行被处理完后将汇总值写入到第二个输出中。 该示例在 ProcessInput 中完成输出，原因是调用 PostExecute 时，输出缓冲区不再可用。  
  
 根据用户的要求，可能还需要在 ScriptMain 类中可用的 PreExecute 和 PostExecute 方法中编写脚本来执行任何预处理或最终处理。  
  
> [!NOTE]  
>  如果是从头开始开发自定义数据流组件的，有一点很重要，即，重写 PrimeOutput 方法来缓存对输出缓冲区的引用，以便稍后能向这些缓冲区添加数据行。 在脚本组件中这不是必需的，因为 **BufferWrapper** 项目项中有自动生成的表示每个输出缓冲区的类。  
  
## <a name="example"></a>示例  
 此示例演示在 ScriptMain 类中创建异步转换组件所需的自定义代码。  
  
> [!NOTE]  
>  这些示例使用 AdventureWorks 示例数据库中的 Person.Address 表并在数据流中传递它的第一列和第四列，即 intAddressID 和 nvarchar(30)City 列     。 在本节中，在源、转换和目标示例中使用相同的数据。 每个示例的其他前提条件和假设都记录在文档中。  
  
 此示例演示具有两个输出的异步转换组件。 此转换将 **AddressID** 和 **City** 列传递给一个输出，同时它对位于特定城市 (Redmond, Washington, U.S.A.) 中的地址进行计数，然后将结果值输出到另一个输出。  
  
 如果要运行此示例代码，必须按照如下方式配置包和组件：  
  
1.  向数据流设计器图面添加新的脚本组件并将其配置为转换。  
  
2.  在设计器中将源或另一转换的输出连接到新转换组件。 此输出应提供 AdventureWorks 示例数据库的 Person.Address 表中的数据，其中至少包含 AddressID 和 City 列。  
  
3.  打开“脚本转换编辑器”  。 在“输入列”  页中，选择 AddressID  和 City  列。  
  
4.  在“输入和输出”页中，向第一个输出添加 **AddressID** 和 **City** 输出列并进行配置。 添加另一个输出，然后向第二个输出添加汇总值的输出列。 将第一个输出的 SynchronousInputID 属性设置为 0，因为此示例显式将每个输入行复制到第一个输出中。 新创建的输出的 SynchronousInputID 属性已经设置为 0。  
  
5.  重命名输入、输出和新输出列，使其名称更具说明性。 该示例使用 **MyAddressInput** 作为输入的名称，**MyAddressOutput** 和 **MySummaryOutput** 作为输出的名称，**MyRedmondCount** 作为第二个输出中的输出列的名称。  
  
6.  在“脚本”页中，单击“编辑脚本”并输入下面的脚本   。 然后关闭脚本开发环境和“脚本转换编辑器”  。  
  
7.  为需要 **AddressID** 和 **City** 列的第一个输出创建和配置一个目标组件，例如，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标或 [使用脚本组件创建目标](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)中演示的示例目标组件。 然后将该转换的第一个输出，即 **MyAddressOutput**，连接到目标组件。 在 AdventureWorks 数据库中运行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令，以创建目标表：  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
8.  为第二个输出创建和配置另一个目标组件。 然后将该转换的第二个输出，即 **MySummaryOutput**，连接到该目标组件。 由于第二个输出写入只带有一个值的一行，因此可以轻松对目标配置一个连接到只含一列的新文件的平面文件连接管理器。 在该示例中，此目标列名为 **MyRedmondCount**。  
  
9. 运行该示例。  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Private myRedmondAddressCount As Integer  
  
    Public Overrides Sub CreateNewOutputRows()  
  
        MySummaryOutputBuffer.AddRow()  
  
    End Sub  
  
    Public Overrides Sub MyAddressInput_ProcessInput(ByVal Buffer As MyAddressInputBuffer)  
  
        While Buffer.NextRow()  
            MyAddressInput_ProcessInputRow(Buffer)  
        End While  
  
        If Buffer.EndOfRowset Then  
            MyAddressOutputBuffer.SetEndOfRowset()  
            MySummaryOutputBuffer.MyRedmondCount = myRedmondAddressCount  
            MySummaryOutputBuffer.SetEndOfRowset()  
        End If  
  
    End Sub  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        With MyAddressOutputBuffer  
            .AddRow()  
            .AddressID = Row.AddressID  
            .City = Row.City  
        End With  
  
        If Row.City.ToUpper = "REDMOND" Then  
            myRedmondAddressCount += 1  
        End If  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
{  
    private int myRedmondAddressCount;  
  
    public override void CreateNewOutputRows()  
    {  
  
        MySummaryOutputBuffer.AddRow();  
  
    }  
  
    public override void MyAddressInput_ProcessInput(MyAddressInputBuffer Buffer)  
    {  
  
        while (Buffer.NextRow())  
        {  
            MyAddressInput_ProcessInputRow(Buffer);  
        }  
  
        if (Buffer.EndOfRowset())  
        {  
            MyAddressOutputBuffer.SetEndOfRowset();  
            MySummaryOutputBuffer.MyRedmondCount = myRedmondAddressCount;  
            MySummaryOutputBuffer.SetEndOfRowset();  
        }  
  
    }  
  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        {  
            MyAddressOutputBuffer.AddRow();  
            MyAddressOutputBuffer.AddressID = Row.AddressID;  
            MyAddressOutputBuffer.City = Row.City;  
        }  
  
        if (Row.City.ToUpper() == "REDMOND")  
        {  
            myRedmondAddressCount += 1;  
        }  
  
    }  
  
}  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [了解同步和异步转换](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [使用脚本组件创建同步转换](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)   
 [开发具有异步输出的自定义转换组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)  
  
