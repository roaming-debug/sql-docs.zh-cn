---
description: 字段对象
title: Field 对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Field object [ADO]
ms.assetid: 7d1c4ad5-4be3-42ab-b516-e7133ca300bc
author: rothja
ms.author: jroth
ms.openlocfilehash: bf3a704b26e35c77d9ba7ba059978091002cc424
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036936"
---
# <a name="the-field-object"></a>字段对象
每个 **字段** 对象通常对应于数据库表中的一个列。 但是， **字段** 还可以表示指向另一 **记录集** 的指针，称为章节。 在本指南的后面部分将介绍一些例外（如章节列）。  
  
 使用 **Field** 对象的 **Value** 属性可以设置或返回当前记录的数据。 根据提供程序公开的功能， **字段** 对象的某些集合、方法或属性可能不可用。  
  
 使用 **Field** 对象的集合、方法和属性，可以执行以下操作：  
  
-   使用 " **名称** " 属性返回字段的名称。  
  
-   使用 " **值** " 属性查看或更改字段中的数据。 **值** 是 **Field** 对象的默认属性。  
  
-   使用 **类型**、 **精度** 和 **NumericScale** 属性返回字段的基本特征。  
  
-   使用 **DefinedSize** 属性返回字段的已声明大小。  
  
-   使用 **ActualSize** 属性返回给定字段中数据的实际大小。  
  
-   使用 **特性** 属性和 **属性** 集合确定给定字段支持的功能类型。  
  
-   使用 **AppendChunk** 和 **GetChunk** 方法操作包含长二进制或长字符数据的字段的值。  
  
 如果访问接口支持批更新，请使用 **OriginalValue** 和 **UnderlyingValue** 属性解决在批处理更新期间字段值的差异。  
  
## <a name="describing-a-field"></a>描述字段  
 下面的主题将讨论 [field](../../../ado/reference/ado-api/field-object.md) 对象的属性，这些属性表示描述 **字段** 对象本身的信息（即有关字段的元数据）。 此信息可用于确定 **记录集** 的架构。 这些属性包括 **类型**、 **DefinedSize** 和 **ActualSize**、 **名称** 以及 **NumericScale** 和 **精度**。  
  
### <a name="discovering-the-data-type"></a>发现数据类型  
 **Type** 属性指示字段的数据类型。 Ado 支持的数据类型枚举常量在 *Ado 程序员参考* 中的 [DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md)中进行了介绍。  
  
 对于浮点数值类型（例如 **adNumeric**），可以获取详细信息。 **NumericScale** 属性指示小数点右边的位数将用来表示 **字段** 的值。 **Precision** 属性指定用于表示 **字段** 值的最大位数。  
  
### <a name="determining-field-size"></a>确定字段大小  
 使用 **DefinedSize** 属性可确定 **Field** 对象的数据容量。  
  
 使用 **ActualSize** 属性可以返回 **字段** 对象的值的实际长度。 对于所有字段， **ActualSize** 属性是只读的。 如果 ADO 无法确定 **字段** 对象的值的长度，则 **ActualSize** 属性将返回 **adUnknown**。  
  
 **DefinedSize** 和 **ActualSize** 属性具有不同的用途。 例如，假设某个 **字段** 对象的类型为 **adVarChar** ， **DefinedSize** 属性值为50，其中包含单个字符。 它返回的 **ActualSize** 属性值是单个字符的长度（以字节为单位）。  
  
### <a name="determining-field-contents"></a>确定字段内容  
 数据源中列的标识符由 **字段** 的 **Name** 属性表示。 **Field** 对象的 **Value** 属性返回或设置该字段的实际数据内容。 这是默认属性。  
  
 若要更改字段中的数据，请将 " **值** " 属性设置为等于正确类型的新值。 你的游标类型必须支持更新以更改字段内容。 在批处理模式下不会执行数据库验证， **因此在这** 种情况下，需要检查是否有错误。 某些提供程序还支持 ADO **字段** 对象的 **UnderlyingValue** 和 **OriginalValue** 属性，以帮助在您尝试执行批更新时解决冲突。 有关如何解决此类冲突的详细信息，请参阅 [编辑数据](../../../ado/guide/data/editing-data.md)。  
  
> [!NOTE]
>  在将新 **字段** 追加到 **记录集** 时，不能设置 **记录集字段** 的值。 而是可以将新 **字段** 追加到关闭的 **记录集**。 然后，必须打开 **记录集** ，然后才能将值分配给这些 **字段**。  
  
### <a name="getting-more-field-information"></a>获取更多字段信息  
 ADO 对象具有两种类型的属性：内置和动态。 至此，只讨论了 **Field** 对象的内置属性。  
  
 内置属性是在 ADO 中实现并使用语法立即可用于任何新对象的属性 `MyObject.Property` 。 它们不作为 **属性** 对象显示在对象的 **属性** 集合中。  
  
 动态属性由基础数据提供程序定义，并出现在相应 ADO 对象的 **properties** 集合中。 例如，特定于提供程序的属性可能指示 **Recordset** 对象是否支持事务或更新。 这些附加属性将显示为该 **记录集** 对象的 **属性** 集合中的 **属性** 对象。 动态属性只能通过使用语法或的集合来引用 `MyObject.Properties(0)` `MyObject.Properties("Name")` 。  
  
 不能删除任何一种属性。  
  
 动态 **属性** 对象具有自己的四个内置属性：  
  
-   **Name** 属性是用于标识属性的字符串。  
  
-   **Type** 属性是指定属性数据类型的整数。  
  
-   **值** 属性是包含属性设置的变量。 **值** 是 **属性** 对象的默认属性。  
  
-   " **属性** " 属性是一个 **Long** 值，指示特定于提供程序的属性的特征。  
  
 **Field** 对象的 **Properties** 集合包含有关此字段的其他元数据。 此集合的内容因提供程序而异。 下面的代码示例检查本部分开头部分引入的示例 **记录集** 的 **Properties** 集合。 它首先查看集合的内容。 此代码使用 [SQL Server 的 OLE DB 提供程序](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md)，因此 **Properties** 集合包含与该提供程序相关的信息。  
  
```  
'BeginFieldProps  
    Dim objProp As ADODB.Property  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
  
        For Each objProp In objFields(intLoop).Properties  
            Debug.Print vbTab & objProp.Name & " = " & objProp.Value  
        Next objProp  
    Next intLoop  
'EndFieldProps  
```  
  
### <a name="dealing-with-binary-data"></a>处理二进制数据  
 对 **字段** 对象使用 [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md)方法，以使用较长的二进制或字符数据填充它。 在系统内存有限的情况下，可以使用 **AppendChunk** 方法在部分中而不是完整地操作长值。  
  
 如果 **字段** 对象的 **Attributes** 属性中的 **adFldLong** 位设置为 **True**，则可以对该字段使用 **AppendChunk** 方法。  
  
 **Field** 对象上的第一个 **AppendChunk** 调用将数据写入字段，并覆盖任何现有数据。 后续 **AppendChunk** 调用将添加到现有数据。 如果要将数据追加到一个字段，然后设置或读取当前记录中另一个字段的值，则 ADO 会假定您已完成将数据追加到第一个字段。 如果再次对第一个字段调用 **AppendChunk** 方法，则 ADO 会将调用解释为新的 **AppendChunk** 操作，并覆盖现有数据。 访问不是第一个 **记录集** 对象的克隆的其他 **记录集** 对象中的字段将不会中断 **AppendChunk** 操作。  
  
 使用 **Field** 对象上的 **GetChunk** 方法检索其长整型或字符数据的部分或全部。 在系统内存有限的情况下，可以使用 **GetChunk** 方法在部分中（而不是在整个中）操作长值。  
  
 **GetChunk** 调用返回的数据将分配给 *变量*。 如果 *Size* 大于剩余数据，则 **GetChunk** 方法仅返回不含空格的空白 *变量* 的剩余数据。 如果该字段为空，则 **GetChunk** 方法返回 null 值。  
  
 每个后续的 **GetChunk** 调用都从上一个 **GetChunk** 调用停止的位置检索数据。 但是，如果您要从一个字段中检索数据，然后设置或读取当前记录中另一个字段的值，则 ADO 假设您已经完成了从第一个字段中检索数据的操作。 如果再次对第一个字段调用 **GetChunk** 方法，则 ADO 会将调用解释为新的 **GetChunk** 操作，并从数据的开头开始读取。 访问不是第一个 **记录集** 对象的克隆的其他 **记录集** 对象中的字段将不会中断 **GetChunk** 操作。  
  
 如果 **字段** 对象的 **Attributes** 属性中的 **adFldLong** 位设置为 **True**，则可以对该字段使用 **GetChunk** 方法。  
  
 如果对 **字段** 对象使用 **GetChunk** 或 **AppendChunk** 方法时没有当前记录，则错误 3021 () 会出现当前记录。  
  
 有关使用这些方法来处理二进制数据的示例，请参阅 *ADO 程序员参考* 中的 [AppendChunk 方法](../../../ado/reference/ado-api/appendchunk-method-ado.md)和 [GetChunk 方法](../../../ado/reference/ado-api/getchunk-method-ado.md)示例。
