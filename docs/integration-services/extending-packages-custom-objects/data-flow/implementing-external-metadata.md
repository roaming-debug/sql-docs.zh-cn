---
description: 实现外部元数据
title: 实现外部元数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- disconnected validation [Integration Services]
- connected validation [Integration Services]
- custom data flow components [Integration Services], validating
- validation [Integration Services], components
- metadata [Integration Services]
- data flow components [Integration Services], validating
- data flow components [Integration Services], external metadata
- custom data flow components [Integration Services], external metadata
- external metadata [Integration Services]
ms.assetid: 8f5bd3ed-3e79-43a4-b6c1-435e4c2cc8cc
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 76296ff7dc7c5084a2c62613182c4d3e0fa6f80b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88484183"
---
# <a name="implementing-external-metadata"></a>实现外部元数据

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  组件与其数据源断开连接后，可以使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100> 接口，针对组件的外部数据源中的列验证输入和输出列集合中的列。 使用此接口可以保留一份外部数据源中的列的快照，并可以将这些列映射到组件的输入和输出列集合中的列。  
  
 实现外部元数据列会增加组件开发的开销和复杂度，因为您必须额外保留和验证列集合，但这却能节省往返于服务器进行验证的昂贵开销，从而使得这项开发工作是值得的。  
  
## <a name="populating-external-metadata-columns"></a>填充外部元数据列  
 通常，外部元数据列在创建相应的输入或输出列时即添加到集合中。 新列是通过调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.New%2A> 方法创建的。 然后，设置列的属性，使其与外部数据源相匹配。  
  
 通过向输入或输出列的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.ExternalMetadataColumnID%2A> 属性分配外部元数据列的 ID，外部元数据列即可映射到相应的输入或输出列。 这使您可以使用集合的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.GetObjectByID%2A> 方法，轻松地查找与特定输入或输出列对应的外部元数据列。  
  
 下面的示例演示如何创建一个外部元数据列，然后通过设置 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.ExternalMetadataColumnID%2A> 属性将其映射到一个输出列。  
  
```csharp  
public void CreateExternalMetaDataColumn(IDTSOutput100 output, int outputColumnID )  
{  
    IDTSOutputColumn100 oColumn = output.OutputColumnCollection.GetObjectByID(outputColumnID);  
    IDTSExternalMetadataColumn100 eColumn = output.ExternalMetadataColumnCollection.New();  
  
    eColumn.DataType = oColumn.DataType;  
    eColumn.Precision = oColumn.Precision;  
    eColumn.Scale = oColumn.Scale;  
    eColumn.Length = oColumn.Length;  
    eColumn.CodePage = oColumn.CodePage;  
  
    oColumn.ExternalMetadataColumnID = eColumn.ID;  
}  
```  
  
```vb  
Public Sub CreateExternalMetaDataColumn(ByVal output As IDTSOutput100, ByVal outputColumnID As Integer)   
 Dim oColumn As IDTSOutputColumn100 = output.OutputColumnCollection.GetObjectByID(outputColumnID)   
 Dim eColumn As IDTSExternalMetadataColumn100 = output.ExternalMetadataColumnCollection.New   
 eColumn.DataType = oColumn.DataType   
 eColumn.Precision = oColumn.Precision   
 eColumn.Scale = oColumn.Scale   
 eColumn.Length = oColumn.Length   
 eColumn.CodePage = oColumn.CodePage   
 oColumn.ExternalMetadataColumnID = eColumn.ID   
End Sub  
```  
  
## <a name="validating-with-external-metadata-columns"></a>使用外部元数据列进行验证  
 对于保留外部元数据列集合的组件，验证还需要完成其他步骤，因为必须验证附加的列集合。 验证可分为连接下的验证和断开连接下的验证。  
  
### <a name="connected-validation"></a>连接下的验证  
 组件与外部数据源处于连接状态时，输入或输出集合中的列是直接与外部数据源相验证的。 另外，还必须验证外部元数据集合中的列。 如此要求的原因是，外部元数据集合可使用 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 中的“高级编辑器”来修改，而且对该集合所做的更改是无法检测的。 因此，处于连接状态时，组件必须确保外部元数据列集合中的列始终反映外部数据源中的列。  
  
 可以选择在“高级编辑器”中隐藏外部元数据集合，方法是将该集合的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.IsUsed%2A> 属性设置为 **false**。 但这也会隐藏编辑器的“列映射”选项卡，该选项卡允许用户将输入或输出集合中的列映射到外部元数据列集合中的列。 将此属性设置为 **false** 不会阻止开发人员以编程方式修改外部元数据列集合，而会为在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 中独占使用的组件的外部元数据列集合提供一定程度的保护。  
  
### <a name="disconnected-validation"></a>断开连接下的验证  
 组件与外部数据源断开连接后，验证即可简化，因为输入或输出集合中的列将直接与外部元数据集合中的列相验证，而不会验证外部源。 如果组件与其外部数据源之间的连接尚未建立，或者 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> 属性为 **false**，则组件应执行断开连接下的验证。  
  
 下面的代码示例演示了执行与其外部元数据列集合相验证的组件的实现。  
  
```csharp  
public override DTSValidationStatus Validate()  
{  
    if( this.isConnected && ComponentMetaData.ValidateExternalMetaData )  
    {  
        // TODO: Perform connected validation.  
    }  
    else  
    {  
        // TODO: Perform disconnected validation.  
    }  
}  
```  
  
```vb  
Public  Overrides Function Validate() As DTSValidationStatus   
 If Me.isConnected AndAlso ComponentMetaData.ValidateExternalMetaData Then   
  ' TODO: Perform connected validation.  
 Else   
  ' TODO: Perform disconnected validation.  
 End If   
End Function  
```  

## <a name="see-also"></a>另请参阅  
 [数据流](../../../integration-services/data-flow/data-flow.md)  
  
  
