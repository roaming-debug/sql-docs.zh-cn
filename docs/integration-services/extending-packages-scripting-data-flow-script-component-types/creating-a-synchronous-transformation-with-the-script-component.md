---
description: 使用脚本组件创建同步转换
title: 使用脚本组件创建同步转换 | Microsoft Docs
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
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- Script component [Integration Services], transformation components
ms.assetid: aa1bee1a-ab06-44d8-9944-4bff03d73016
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 08c7e20120ef5ff746b495535843bc93a46fea16
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344892"
---
# <a name="creating-a-synchronous-transformation-with-the-script-component"></a>使用脚本组件创建同步转换

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的数据流中使用转换组件可以在数据从源传递到目标时修改和分析该数据。 具有同步输出的转换在每个输入行传递给该组件时对该行进行处理。 具有异步输出的转换在等到接收所有输入行之后才能完成处理。 本主题讨论同步转换。 有关异步转换的信息，请参阅[使用脚本组件创建异步转换](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)。 有关同步组件和异步组件之间的差异的详细信息，请参阅[了解同步和异步转换](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)。  
  
 有关脚本组件的概述，请参阅[使用脚本组件扩展数据流](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md)。  
  
 脚本组件及其生成的基础结构代码可以大大简化自定义数据流组件的开发过程。 但是，若要了解脚本组件的工作方式，读取以下步骤很有帮助，这些步骤是在[开发自定义数据流组件](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)部分，特别是在[开发具有同步输出的自定义转换组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)部分开发自定义数据流组件必须遵循的。  
  
## <a name="getting-started-with-a-synchronous-transformation-component"></a>开始一个同步转换组件  
 向 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器的“数据流”窗格添加脚本组件时，“选择脚本组件类型”  对话框将打开，并提示选择源、目标或转换组件类型。 在此对话框中，选择“转换”  。  
  
## <a name="configuring-a-synchronous-transformation-component-in-metadata-design-mode"></a>在元数据设计模式下配置同步转换组件  
 选择创建转换组件的选项后，可使用“脚本转换编辑器”  配置该组件。 有关详细信息，请参阅[在脚本组件编辑器中配置脚本组件](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md)。  
  
 若要设置脚本组件的脚本语言，请在“脚本转换编辑器”的“脚本”页上设置“ScriptLanguage”属性。  
  
> [!NOTE]  
>  若要设置脚本组件的默认脚本语言，请使用“选项”对话框的“常规”页上的“脚本语言”选项。 有关详细信息，请参阅[常规页](../general-page-of-integration-services-designers-options.md)。  
  
 数据流转换组件有一个输入并支持一个或多个输出。 在写入自定义脚本之前，必须在元数据设计模式下完成的一个步骤是使用“脚本转换编辑器”  配置组件的输入和输出。  
  
### <a name="configuring-input-columns"></a>配置输入列  
 转换组件具有一个输入。  
  
 在“脚本转换编辑器”的“输入列”页中，列列表显示数据流上游组件的输出中可用的列。 选择要转换或传递的列。 将要就地转换的所有列标记为读/写。  
  
 有关“脚本转换编辑器”的“输入列”页的详细信息，请参阅[脚本转换编辑器（“输入列”页）](../data-flow/transformations/script-component.md)。  
  
### <a name="configuring-inputs-outputs-and-output-columns"></a>配置输入、输出和输出列  
 转换组件支持一个或多个输出。  
  
 在“脚本转换编辑器”的“输入和输出”页中，可以看到已创建一个输出，但是还没有创建任何输出列。 在编辑器的这一页，可能需要或希望配置以下各项。  
  
-   创建一个或多个附加输出，如包含意外值的行的模拟错误输出。 使用“添加输出”  和“删除输出”  按钮以管理同步转换组件的输出。 所有输入行都定向到所有可用输出，除非您表示希望将每一行重定向到一个输出或其他输出。 可通过为输出上的 ExclusionGroup  属性指定一个非零的整数值来指示希望重定向行。 在 ExclusionGroup  中输入的用于标识输出的特定整数值并不重要，但是必须对指定的输出组使用同一个整数。  
  
    > [!NOTE]  
    >  如果不希望输出所有行，还可以对单个输出使用非零 ExclusionGroup 属性值  。 但是，在这种情况下，你必须为想要发送给输出的每一行显式调用 DirectRowTo\<outputbuffer> 方法。  
  
-   为输入和输出指定一个更具说明性的名称。 脚本组件可以使用这些名称来生成类型化取值函数属性，这些属性用于在脚本中引用输入和输出。  
  
-   对于同步转换，将列保持原样。 通常，同步转换不会向数据流添加列。 会在缓冲区中就地修改数据，然后将缓冲区传递到数据流的下一个组件中。 如果是这种情况，您不需要对转换的输出显式添加和配置输出列。 输出显示在编辑器中，并且不包含任何显式定义的列。  
  
-   向行级错误的模拟错误输出添加新列。 通常，同一 ExclusionGroup 中的多个输出具有相同的输出列集  。 但是，如果要创建模拟的错误输出，则可能要添加多个列来包含错误信息。 有关数据流引擎如何处理错误行的信息，请参阅[在数据流组件中使用错误输出](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md)。 请注意，在脚本组件中，必须编写您自己的代码以便使用适当的错误信息填充这些附加列。 有关详细信息，请参阅[模拟脚本组件的错误输出](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md)。  
  
 有关“脚本转换编辑器”  的“输入和输出”  页上的详细信息，请参阅[脚本转换编辑器（“输入和输出”页）](../data-flow/transformations/script-component.md)。  
  
### <a name="adding-variables"></a>添加变量  
 如果要在脚本中使用现有的变量，可以在“脚本转换编辑器”的“脚本”页上的 ReadOnlyVariables 和 ReadWriteVariables 属性字段中添加这些变量     。  
  
 在属性字段中添加多个变量时，请用逗号将变量名隔开。 还可以选择多个变量，方法是单击 ReadOnlyVariables 和 ReadWriteVariables 属性字段旁的省略号 (...) 按钮，然后在“选择变量”对话框中选择变量     。  
  
 有关如何在脚本组件中使用变量的常规信息，请参阅[在脚本组件中使用变量](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md)。  
  
 有关“脚本转换编辑器”的“脚本”页的详细信息，请参阅[脚本转换编辑器（“脚本”页）](../data-flow/transformations/script-component.md)。  
  
## <a name="scripting-a-synchronous-transformation-component-in-code-design-mode"></a>在代码设计模式下编写同步转换组件脚本  
 为组件配置完元数据后，可以编写自定义脚本。 在“脚本转换编辑器”的“脚本”页面中，单击“编辑脚本”打开 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) IDE，可在其中添加自定义脚本    。 编写脚本所使用的语言取决于为“脚本”页上的 **ScriptLanguage** 属性选择 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic 还是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# 作为脚本语言。  
  
 有关适用于使用脚本组件创建的所有组件类型的重要信息，请参阅[脚本组件的编码和调试](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)。  
  
### <a name="understanding-the-auto-generated-code"></a>了解自动生成的代码  
 创建并配置转换组件后打开 VSTA IDE 时，可编辑的 ScriptMain  类会显示在代码编辑器中，其中有 ProcessInputRow  方法的存根。 在 ScriptMain 类中可编写自定义代码，ProcessInputRow 是转换组件中最重要的方法   。  
  
 如果在 VSTA 中打开“项目资源管理器”  窗口，可以看到脚本组件还生成了只读的 **BufferWrapper** 和 **ComponentWrapper** 项目项。 ScriptMain 类继承自 ComponentWrapper 项目项中的 UserComponent 类。  
  
 在运行时，数据流引擎调用 UserComponent 类中的 ProcessInput 方法，该方法替代 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> 父类的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A> 方法。 而 ProcessInput  方法遍历输入缓冲区中的所有行并为每一行调用一次 ProcessInputRow  方法。  
  
### <a name="writing-your-custom-code"></a>编写自定义代码  
 具有同步传输的转换组件是要编写的所有数据流组件中最简单的部分。 例如，本主题中稍后演示的单个输出示例包含以下自定义代码：  
  
```vb  
Row.City = UCase(Row.City)  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
```  
  
 若要完成创建自定义同步转换组件，使用已重写的 ProcessInputRow  方法来转换输入缓冲区的每行中的数据。 缓冲区写满后，数据流引擎会将此缓冲区传递给数据流中的下一个组件。  
  
 根据用户的要求，可能还需要在 ScriptMain 类中可用的 PreExecute 和 PostExecute 方法中编写脚本来执行预处理或最终处理    。  
  
### <a name="working-with-multiple-outputs"></a>使用多个输出  
 将输入行定向到两个或多个可能的输出中的一个输出时所需的自定义代码比之前讨论的单个输出方案所需的代码要少得多。 例如，本主题中稍后演示的两个输出示例包含以下自定义代码：  
  
```vb  
Row.City = UCase(Row.City)  
If Row.City = "REDMOND" Then  
    Row.DirectRowToMyRedmondAddresses()  
Else  
    Row.DirectRowToMyOtherAddresses()  
End If  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
if (Row.City=="REDMOND")  
{  
    Row.DirectRowToMyRedmondAddresses();  
}  
else  
{  
    Row.DirectRowToMyOtherAddresses();  
}  
```  
  
 在此示例中，脚本组件将基于已配置的输出名称生成 DirectRowTo\<OutputBufferX> 方法。 您可以使用类似的代码将错误行定向到模拟的错误输出。  
  
## <a name="examples"></a>示例  
 此示例演示在 ScriptMain  类中创建同步转换组件所需的自定义代码。  
  
> [!NOTE]  
>  这些示例使用 AdventureWorks 示例数据库中的 Person.Address 表并在数据流中传递它的第一列和第四列，即 intAddressID 和 nvarchar(30)City 列     。 在本节中，在源、转换和目标示例中使用相同的数据。 每个示例的其他前提条件和假设都记录在文档中。  
  
### <a name="single-output-synchronous-transformation-example"></a>单个输出同步转换示例  
 此示例演示具有单个输出的同步转换组件。 此转换将传递 AddressID  列，并将 City  列转换为大写。  
  
 如果要运行此示例代码，必须按照如下方式配置包和组件：  
  
1.  向数据流设计器图面添加新的脚本组件并将其配置为转换。  
  
2.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中将源或另一转换的输出连接到新转换组件。 此输出应提供 AdventureWorks 示例数据库的 Person.Address 表中的数据，其中包含 AddressID 和 City 列。  
  
3.  打开“脚本转换编辑器”  。 在“输入列”  页中，选择 AddressID  和 City  列。 将 City  列标记为读取/写入。  
  
4.  在“输入和输出”  页上，使用更具说明性的名称（如 MyAddressInput  和 MyAddressOutput  重命名输入和输出。 请注意，输出的 SynchronousInputID  与输入的 ID  相对应。 因此，您不需要添加和配置输出列。  
  
5.  在“脚本”页中，单击“编辑脚本”并输入下面的脚本   。 然后关闭脚本开发环境和“脚本转换编辑器”  。  
  
6.  创建并配置目标组件，如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标或 [使用脚本组件创建目标](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) 中演示的示例目标组件，该目标组件需要 AddressID 和 City 列。 然后，将转换的输出连接到目标组件。 在 AdventureWorks 数据库中运行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令，以创建目标表：  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  运行该示例。  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
{  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
    }  
  
}  
```  
  
### <a name="two-output-synchronous-transformation-example"></a>两个输出同步转换示例  
 此示例演示具有两个输出的同步转换组件。 此转换将传递 AddressID  列，并将 City  列转换为大写。 如果城市名为 Redmond，则将行定向到一个输出；将所有其他行定向到另一个输出。  
  
 如果要运行此示例代码，必须按照如下方式配置包和组件：  
  
1.  向数据流设计器图面添加新的脚本组件并将其配置为转换。  
  
2.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中将源或另一转换的输出连接到新转换组件。 此输出应提供 AdventureWorks 示例数据库的 Person.Address 表中的数据，其中至少包含 AddressID 和 City 列。  
  
3.  打开“脚本转换编辑器”  。 在“输入列”  页中，选择 AddressID  和 City  列。 将 City  列标记为读取/写入。  
  
4.  在“输入和输出”  页中，创建第二个输出。 添加新的输出后，请确保将该输出的 SynchronousInputID  设置为输入的 ID  。 已对第一个输出设置此属性，该输出是默认创建的。 对于每个输出，将 ExclusionGroup  属性设置为同一非零值，以指示将在两个互相排斥的输出间拆分输入行。 无需向输出添加任何输出列。  
  
5.  使用更具说明性的名称（如 MyAddressInput  、MyRedmondAddresses  和 MyOtherAddresses  ）重命名输入和输出。  
  
6.  在“脚本”页中，单击“编辑脚本”并输入下面的脚本   。 然后关闭脚本开发环境和“脚本转换编辑器”  。  
  
7.  创建并配置两个目标组件，如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标、平面文件目标或在[使用脚本组件创建目标](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)中演示的示例目标组件，这两个目标组件需要 AddressID 和 City 列。 然后，将转换的每个输出连接到任一目标组件。 可在 AdventureWorks 数据库中运行与下列命令（具有唯一表名）类似的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令来创建目标表：  
  
    ```sql
    CREATE TABLE [Person].[Address2](  
        [AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL  
    ```  
  
8.  运行该示例。  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
        If Row.City = "REDMOND" Then  
            Row.DirectRowToMyRedmondAddresses()  
        Else  
            Row.DirectRowToMyOtherAddresses()  
        End If  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
        if (Row.City == "REDMOND")  
        {  
            Row.DirectRowToMyRedmondAddresses();  
        }  
        else  
        {  
            Row.DirectRowToMyOtherAddresses();  
        }  
  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [了解同步和异步转换](~/integration-services/understanding-synchronous-and-asynchronous-transformations.md)  
 [使用脚本组件创建异步转换](~/integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
 [开发具有同步输出的自定义转换组件](~/integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)
