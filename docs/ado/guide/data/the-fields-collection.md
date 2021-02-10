---
description: 字段集合
title: Fields 集合 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Field object [ADO], fields collection
- Fields collection [ADO]
ms.assetid: 574cf36e-e5f5-403b-983c-749ef93c108f
author: rothja
ms.author: jroth
ms.openlocfilehash: 7dee12a0cdb274f58bb3dc79fddeb057e4913e81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032409"
---
# <a name="the-fields-collection"></a>字段集合
**字段** 集合是 ADO 的内部集合之一。 集合是一组有序项，可作为一个单元来引用。 有关 ADO 集合的详细信息，请参阅 [Ado 对象模型](../../../ado/guide/data/ado-objects-and-collections.md)。  
  
 **Fields** 集合包含 **记录集中** (列) 的每个字段的 **字段** 对象。 与所有 ADO 集合一样，它具有 **计数** 和 **项** 属性，以及 **追加** 和 **刷新** 方法。 它还具有 **CancelUpdate**、 **Delete**、 **Resync** 和 **UPDATE** 方法，这些方法对于其他 ADO 集合不可用。  
  
## <a name="examining-the-fields-collection"></a>检查字段集合  
 请考虑本节中引入的示例 **记录集** 的 **字段** 集合。 示例 **记录集** 派生自 SQL 语句  
  
```  
SELECT ProductID, ProductName, UnitPrice FROM Products WHERE CategoryID = 7  
```  
  
 因此，您应发现 **记录集字段** 集合包含三个字段。  
  
```  
'BeginWalkFields  
    Dim objFields As ADODB.Fields  
    Dim intLoop As Integer  
  
    objRs.Open strSQL, strConnStr, adOpenForwardOnly, adLockReadOnly, adCmdText  
  
    Set objFields = objRs.Fields  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
    Next  
'EndWalkFields  
```  
  
 此代码只是使用 **Count** 属性来确定 **字段** 集合中 **字段** 对象的数目，并循环遍历该集合，返回每个 **字段** 对象的 **Name** 属性的值。 您可以使用更多 **字段** 属性来获取有关字段的信息。 有关查询 **字段** 的详细信息，请参阅 [field 对象](../../../ado/guide/data/the-field-object.md)。  
  
## <a name="counting-columns"></a>计算列数  
 正如您所料， **Count** 属性返回 **字段** 集合中 **字段** 对象的实际数目。 由于集合成员的编号以零开始，因此，您始终应始终从零个成员开始编写循环，并以 **Count** 属性的值减1结束。 如果使用的是 Microsoft Visual Basic 并且想要循环遍历集合的成员而不检查 **Count** 属性，请使用 **For Each .。。下一个** 命令。  
  
 如果 **Count** 属性为零，则集合中不存在任何对象。  
  
## <a name="getting-to-the-field"></a>转到字段  
 与任何 ADO 集合一样， **项** 属性是集合的默认属性。 它返回由传递给它的名称或索引指定的单个 **Field** 对象。 因此，下面的语句对于示例 **记录集** 是等效的：  
  
```  
objField = objRecordset.Fields.Item("ProductID")  
objField = objRecordset.Fields("ProductID")  
objField = objRecordset.Fields.Item(0)  
objField = objRecordset.Fields(0)  
```  
  
 如果这两种方法是等效的，最好这样做？ 视情况而定。 使用索引从集合中检索 **字段** 的速度更快，因为它直接访问 **字段** ，而无需执行字符串查找。 另一方面，集合中 **字段** 的顺序必须是已知的，并且如果顺序发生更改，则在任何情况下都必须更改对 **字段的** 索引的引用。 虽然稍微慢一些，但使用 **字段** 的名称更灵活，因为它不依赖于集合中 **字段** 的顺序。  
  
## <a name="using-the-refresh-method"></a>使用 Refresh 方法  
 与其他一些 ADO 集合不同，对 **字段** 集合使用 **Refresh** 方法不会产生任何效果。 若要从基础数据库结构中检索更改，必须使用 **Requery** 方法，或者如果 **记录集** 对象不支持书签，则为 **MoveFirst** 方法，这将导致再次对提供程序执行该命令。  
  
## <a name="adding-fields-to-a-recordset"></a>将字段添加到记录集  
 **Append** 方法用于向 **记录集** 添加字段。  
  
 您可以使用 **Append** 方法以编程方式创建 **记录集** ，而无需打开与数据源的连接。 如果对打开的 **记录集** 的 **字段** 集合或已设置 **ActiveConnection** 属性的 **记录集** 调用 **Append** 方法，将会发生运行时错误。 您只能向未打开且尚未连接到数据源的 **记录集** 追加字段。 但是，若要指定新追加 **字段** 的值，必须先打开该 **记录集** 。  
  
 开发人员通常需要一个位置来临时存储一些数据，或者希望某些数据像来自服务器一样，使其可以参与用户界面中的数据绑定。 ADO (与 [用于 OLE DB 的 Microsoft 游标服务](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)) 允许开发人员通过指定列信息并调用 **Open** 来生成一个空的 **记录集** 对象。 在下面的示例中，向新的 **记录集** 对象追加了三个新字段。 然后，将打开该 **记录集** ，添加两条新记录，并将 **记录集** 保存到文件中。  (有关 **记录集** 持久性的详细信息，请参阅 [更新和保存数据](../../../ado/guide/data/updating-and-persisting-data.md)。 )   
  
```  
'BeginFabricate  
    Dim objRs As ADODB.Recordset  
    Set objRs = New ADODB.Recordset  
  
    With objRs.Fields  
        .Append "StudentID", adChar, 11, adFldUpdatable  
        .Append "FullName", adVarChar, 50, adFldUpdatable  
        .Append "PhoneNmbr", adVarChar, 20, adFldUpdatable  
    End With  
  
    With objRs  
        .Open  
  
        .AddNew  
        .Fields(0) = "123-45-6789"  
        .Fields(1) = "John Doe"  
        .Fields(2) = "(425) 555-5555"  
        .Update  
  
        .AddNew  
        .Fields(0) = "123-45-6780"  
        .Fields(1) = "Jane Doe"  
        .Fields(2) = "(615) 555-1212"  
        .Update  
    End With  
  
    objRs.Save App.Path & "FabriTest.adtg", adPersistADTG  
  
    objRs.Close  
'EndFabricate  
```  
  
 **字段 Append** 方法的用法在 **Recordset** 对象和 **Record** 对象之间有所不同。 有关 **Record** 对象的详细信息，请参阅 [记录和流](../../../ado/guide/data/records-and-streams.md)。  
  
## <a name="see-also"></a>另请参阅  
 [构造分层记录集](../../../ado/guide/data/fabricating-hierarchical-recordsets.md)
