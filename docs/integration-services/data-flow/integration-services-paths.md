---
description: Integration Services 路径
title: Integration Services 路径 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.patheditor.general.f1
- sql13.dts.designer.patheditor.metadata.f1
- sql13.dts.designer.patheditor.visualizers.f1
helpviewer_keywords:
- paths [Integration Services], about paths
- data flow [Integration Services], paths
- paths [Integration Services]
- destinations [Integration Services], paths
- sources [Integration Services], paths
ms.assetid: 6c4629a9-2ede-4011-9101-3b342249640e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a69d70b9e58458303dbba3444b6a8c38fb40ccc0
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495849"
---
# <a name="integration-services-paths"></a>Integration Services 路径

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  路径将一个数据流组件的输出连接到另一个组件的输入，以此连接数据流中的两个组件。 路径具有源和目标。 例如，如果路径连接一个 OLE DB 源和一个排序转换，那么 OLE DB 源就是路径的源，而排序转换就是路径的目标。 源是路径开始处的组件，而目标是路径结束处的组件。  
  
 如果在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中运行包，您可以将数据查看器附加到一个路径，以此来查看数据流中的数据。 数据查看器可配置为在网格中显示数据。 数据查看器是个很有用的调试工具。 有关详细信息，请参阅 [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md)。  
  
## <a name="configure-the-path"></a>配置路径  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供 **“数据流路径编辑器”** 对话框，用于设置路径属性、查看通过该路径的数据列的元数据以及配置数据查看器。  
  
 可配置的路径属性有路径的名称、说明和批注。 还可以用编程的方式配置路径。 有关详细信息，请参阅 [以编程方式连接数据流组件](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)。  
  
 路径批注显示路径源的名称或 **设计器中** “数据流” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 选项卡的设计图面上的路径名称。 路径批注与可以添加到数据流、控制流和事件处理程序的批注类似， 唯一区别是路径批注附加到路径，而其他批注则显示在 **设计器的**“数据流” **、**“控制流” **和**“事件处理程序” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 选项卡上。  
  
 元数据显示前一组件的输出中每一列的名称、数据类型、精度、小数位数、长度、代码页和源组件。 源组件是创建此列的数据流组件。 这可能是数据流中的第一个组件，也可能不是。 例如，Union All 和排序转换创建自己的列，并且是其输出列的源。 反之，复制列转换则可以不加更改地传递列，或者通过复制输入列来创建新列。 复制列转换仅是新列的源组件。  

## <a name="set-the-properties-of-a-path-with-the-data-flow-path-editor"></a>使用数据流路径编辑器设置路径属性
路径连接两个数据流组件。 数据流必须包含至少两个已连接的数据流组件，才能设置路径属性。
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击“数据流”选项卡，然后双击路径。  
  
4.  在 **“数据流路径编辑器”** 中，单击 **“常规”**。 然后，可以编辑默认的路径名称并提供路径说明。 还可以修改 PathAnnotation 属性。  
  
5.  单击“确定”。  
  
6.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  

## <a name="general-page---data-flow-path-editor"></a>“常规”页 - 数据流路径编辑器
可以使用 **“数据流路径编辑器”** 对话框设置路径属性，查看列元数据以及管理附加到路径的数据查看器。  
  
 使用 **“数据流路径编辑器”** 对话框的 **“常规”** 节点可以对路径进行命名和说明以及指定路径批注选项。  
  
### <a name="options"></a>选项  
 **名称**  
 为路径提供唯一的名称。  
  
 **ID**  
 路径的沿袭标识符。 此属性为只读。  
  
 **IdentificationString**  
 用于标识路径的字符串。 从上面输入的名称自动生成。  
  
 **说明**  
 描述该路径。  
  
 **PathAnnotation**  
 指定要使用的批注类型。 选择 **Never** 可以禁用批注；选择 **AsNeeded** 可以启用按需批注；选择 **SourceName** 可以使用 **SourceName** 选项的值自动进行批注；选择 **PathName** 可以使用 **Name** 属性的值自动进行批注。  
  
 **DestinationName**  
 显示作为路径结尾的输入。  
  
 **SourceName**  
 显示作为路径开头的输出。  
 
## <a name="metadata-page---data-flow-path-editor"></a>“元数据”页 - 数据流路径编辑器
可以使用 **“数据流路径编辑器”** 对话框的 **“元数据”** 页查看路径列的元数据。  
  
### <a name="options"></a>选项  
 **路径元数据**  
 列出列元数据。 单击列标题可以对列数据进行排序。  
  
 **名称**  
 列出列的名称。  
  
 **数据类型**  
 列出列的数据类型。  
  
 **精度**  
 列出数值的位数。  
  
 **缩放**  
 列出数值的小数点后的位数。  
  
 **长度**  
 列出列的当前长度。  
  
 **代码页**  
 列出列的代码页。 如果值为 **0** ，则表示该列不使用代码页。 当数据为 Unicode 字符，或者数据为数值、日期或时间数据类型时，则会发生上述情况。  
  
 **排序键位置**  
 列出列的排序键位置。 如何值为 **0** ，则表示未对该列进行排序。  
  
> [!NOTE]  
>  减号 (-) 前缀表示该列按降序排序。  
  
 **比较标志**  
 列出应用于该列的比较标志。  
  
 **源组件**  
 列出作为列的源的数据流组件。  
  
 **复制到剪贴板**  
 将列的元数据复制到剪贴板。 默认情况下，按当前显示的顺序复制所有元数据行。  
 
## <a name="data-viewers-page---data-flow-path-editor"></a>“数据查看器”页 - 数据流路径编辑器
可以使用 **“数据流路径编辑器”** 对话框的 **“数据查看器”** 页管理附加到路径的数据查看器。  
  
### <a name="options"></a>选项  
 **名称**  
 列出数据查看器。  
  
 **数据查看器类型**  
 列出数据查看器的类型。  
  
 **添加**  
 单击此项可使用“配置数据查看器”对话框来添加数据查看器。  
  
 **删除**  
 单击此项可删除所选数据查看器。  
  
 **配置**  
 单击此项可使用“配置数据查看器”对话框来配置所选数据查看器。  
 
## <a name="path-properties"></a>Path Properties
[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中的数据流对象在组件级、输入和输出级以及输入列和输出列级具有通用属性和自定义属性。 其中许多属性的值是只读的，由数据流引擎在运行时分配。  
  
 本主题列出并描述了连接数据流对象的路径的自定义属性。  
  
### <a name="custom-properties-of-a-path"></a>路径的自定义属性  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中，数据流中连接组件的路径实现 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> 接口。  
  
 下表介绍了数据流中路径的可配置属性。 数据流引擎还为此处未列出的其他只读属性赋值。  
  
|属性名称|数据类型|说明|  
|-------------------|---------------|-----------------|  
|PathAnnotation|Integer（枚举）|用于指示在设计器图面上显示路径时是否应显示批注的值。 可能的值为 **AsNeeded**、 **SourceName**、 **PathName** 和 **Never**。 默认值为 **AsNeeded**。|  
|DestinationName|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100>|与路径关联的输入。|  
|SourceName|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100>|与路径关联的输出。|  
