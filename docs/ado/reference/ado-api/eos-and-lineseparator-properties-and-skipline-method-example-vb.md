---
description: 'LineSeparator 属性和 SkipLine 方法示例 (VB) '
title: LineSeparator 属性和 SkipLine 方法示例 (VB) |Microsoft Docs
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
- LineSeparator property [ADO], Visual Basic example
- Skipline method [ADO], Visual Basic example
- EOS property [ADO], Visual Basic example
ms.assetid: 77ce3042-9ebc-44ba-a4ff-0f1b1fd4a9c4
author: rothja
ms.author: jroth
ms.openlocfilehash: 00bb06c8ea24b622278bb3202f2e0500c4594627
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167498"
---
# <a name="eos-and-lineseparator-properties-and-skipline-method-example-vb"></a>LineSeparator 属性和 SkipLine 方法示例 (VB) 
此示例演示如何一次对一行的文本流进行操作。 会显示将线条分隔符从默认回车符/换行符 (**adCRLF**) 更改为简单 (**adLF**) 或回车符 (**adCR**) 的效果。  
  
```  
'BeginSkipLineVB  
Private Sub cmdSkipLine_Click()  
    On Error GoTo ErrorHandler  
  
    'Declare variables  
    Dim i As Integer  
    Dim objStream As Stream  
    Dim strLine, strChar As String  
  
    'Instantiate and open stream  
    Set objStream = New Stream  
    objStream.Open  
  
    'Set line separator to line feed  
    objStream.LineSeparator = adLF  
  
    'Load text content of list box into stream  
    'One line at a time  
    For i = 0 To (List1.ListCount - 1)  
        objStream.WriteText List1.List(i), adWriteLine  
    Next  
  
    'Display the entire stream  
    Debug.Print "Whole Stream:"  
    objStream.Position = 0  
    Debug.Print objStream.ReadText  
  
    'Display the first line  
    Debug.Print "First Line:"  
    objStream.Position = 0  
    strLine = objStream.ReadText(adReadLine)  
    Debug.Print strLine  
    Debug.Print "Line length: " + Str(Len(strLine))  
  
    'Skip a line, then display another line  
    Debug.Print "Third Line:"  
    objStream.SkipLine  
    strLine = objStream.ReadText(adReadLine)  
    Debug.Print strLine  
    Debug.Print "Line length: " + Str(Len(strLine))  
  
    'Switch line separator to carriage return  
    'All items from list will be considered one line  
    'Assuming no CRs have been loaded into stream  
    Debug.Print "Whole Stream/First Line:"  
    objStream.Position = 0  
    objStream.LineSeparator = adCR  
    strLine = objStream.ReadText(adReadLine)  
    Debug.Print strLine  
    Debug.Print "Line length: " + Str(Len(strLine))  
    Debug.Print "Stream size: " + Str(objStream.Size)  
  
    'Use EOS to Determine End of Stream  
    Debug.Print "Character by character:"  
    objStream.Position = 0  
    Do Until objStream.EOS  
        strChar = objStream.ReadText(1)  
        Debug.Print strChar  
    Loop  
  
    ' clean up  
    objStream.Close  
    Set objStream = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not objStream Is Nothing Then  
        If objStream.State = adStateOpen Then objStream.Close  
    End If  
    Set objStream = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
  
Private Sub Form_Load()  
    List1.AddItem "This is the first line"  
    List1.AddItem "This is the second line"  
    List1.AddItem "This is the third line"  
End Sub  
'EndSkipLineVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [EOS 属性](../../../ado/reference/ado-api/eos-property.md)   
 [LineSeparator 属性 (ADO) ](../../../ado/reference/ado-api/lineseparator-property-ado.md)   
 [SkipLine 方法](../../../ado/reference/ado-api/skipline-method.md)
