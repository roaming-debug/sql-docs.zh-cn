---
description: Status 属性示例（字段）(VB)
title: 状态属性示例 ( (VB) ) 字段 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Status property [ADO Field], Visual Basic example
ms.assetid: fdd09b60-39c7-44be-8008-e891a031f80e
author: rothja
ms.author: jroth
ms.openlocfilehash: 706be79c9a910a8f6f5df5635acf0d5f0ad3a8c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040437"
---
# <a name="status-property-example-field-vb"></a>Status 属性示例（字段）(VB)
下面的示例使用 [Internet 发布提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)从读/写文件夹打开文档。 [记录](./record-object-ado.md)的 [Field](./field-object.md)对象的 [Status](./status-property-ado-field.md)属性将首先设置为 **AdFieldPendingInsert**，然后将其更新为 **adFieldOk**。  
  
```  
'BeginStatusFieldVB  
  
 ' to integrate this code replace the values in the source string  
  
Sub Main()  
  
   Dim File As ADODB.Record  
   Dim strFile As String  
   Dim Cnxn As ADODB.Connection  
   Dim strCnxn As String  
  
   Set Cnxn = New ADODB.Connection  
   strCnxn = "url=https://MyServer/"  
   Cnxn.Open strCnxn  
  
   Set File = New ADODB.Record  
   strFile = "Folder/FileName"  
   ' Open a read/write document  
   File.Source = strFile  
   File.ActiveConnection = Cnxn  
   File.Mode = adModeReadWrite  
   File.Open  
  
   Debug.Print "Append a couple of fields"  
  
   File.Fields.Append "chektest:fld1", adWChar, 42, adFldUpdatable, "fld1"  
   File.Fields.Append "chektest:fld2", adWChar, 42, adFldUpdatable, "fld2"  
  
   Debug.Print "status for the fields"  
   Debug.Print File.Fields("chektest:fld1").Status 'adfldpendinginsert  
   Debug.Print File.Fields("chektest:fld2").Status 'adfldpendinginsert  
  
    'turn off error-handling to verify field status  
   On Error Resume Next  
  
   File.Fields.Update  
  
   Debug.Print "Update succeeds"  
   Debug.Print File.Fields("chektest:fld1").Status 'adfldpendinginsert + adFieldUnavailable  
   Debug.Print File.Fields("chektest:fld2").Status 'adfldpendinginsert + adFieldUnavailable  
  
    ' resume default error-handling  
   On Error GoTo 0  
  
     ' clean up  
    File.Close  
    Cnxn.Close  
    Set File = Nothing  
    Set Cnxn = Nothing  
  
End Sub  
'EndStatusFieldVB  
```  
  
 下面的示例从文档中打开的 **记录** 中删除已知 **字段**。 **Status** 属性将首先设置为 **adFieldOK**，然后设置为 **adFieldPendingUnknown**。  
  
```  
Attribute VB_Name = "StatusField"  
```  
  
 下面的代码从在只读文档上打开的 **记录** 中删除 **字段**。 **状态** 将设置为 **adFieldPendingDelete**。 在 [更新](./update-method.md)时，删除将失败，并且 **状态** 将为 **adFieldPendingDelete** + **adFieldPermissionDenied**。 [CancelUpdate](./cancelupdate-method-ado.md) 清除挂起 **状态** 设置。  
  
```  
Attribute VB_Name = "StatusField"  
```  
  
## <a name="see-also"></a>另请参阅  
 [Field 对象](./field-object.md)   
 [ (ADO 记录对象) ](./record-object-ado.md)   
 [Status 属性（ADO 字段）](./status-property-ado-field.md)