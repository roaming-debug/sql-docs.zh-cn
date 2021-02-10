---
description: 单元集示例 (VB)
title: " (VB) 的单元集示例 |Microsoft Docs"
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
- Cellset object [ADO MD], Visual Basic example
ms.assetid: 2666ad1c-b48e-4b2c-b269-5a9f4e4a7810
author: rothja
ms.author: jroth
ms.openlocfilehash: ef52436b4bcd585f489f79a99664378d479421d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056816"
---
# <a name="cellset-example-vb"></a>单元集示例 (VB)
此 Visual Basic 项目演示了使用 ADO MD 访问多维数据集数据的基础知识。 它显示列标题和行标题的成员标题，然后在单元集内显示特定单元格的格式化值。  
  
```  
Private Sub cmdCellSettoDebugWindow_Click()  
Dim cat As New ADOMD.Catalog  
Dim cst As New ADOMD.Cellset  
Dim i As Integer  
Dim j As Integer  
Dim strServer As String  
Dim strSource As String  
Dim strColumnHeader As String  
Dim strRowText As String  
  
On Error GoTo Error_cmdCellSettoDebugWindow_Click  
Screen.MousePointer = vbHourglass  
'*-----------------------------------------------------------------------  
'* Set Server to Local Host  
'*-----------------------------------------------------------------------  
    strServer = "LOCALHOST"  
  
'*-----------------------------------------------------------------------  
'* Set MDX query string Source  
'*-----------------------------------------------------------------------  
    strSource = strSource & "SELECT "  
    strSource = strSource & "{[Measures].members} ON COLUMNS,"  
    strSource = strSource & _  
        "NON EMPTY [Store].[Store City].members ON ROWS"  
    strSource = strSource & " FROM Sales"  
  
'*-----------------------------------------------------------------------  
'* Set Active Connection  
'*-----------------------------------------------------------------------  
        cat.ActiveConnection = "Data Source=" & strServer & _  
            ";Provider=msolap;"  
  
'*-----------------------------------------------------------------------  
'* Set Cell Set source to MDX query string  
'*-----------------------------------------------------------------------  
        cst.Source = strSource  
  
'*-----------------------------------------------------------------------  
'* Set Cell Sets active connection to current connection  
'*-----------------------------------------------------------------------  
    Set cst.ActiveConnection = cat.ActiveConnection  
  
'*-----------------------------------------------------------------------  
'* Open Cell Set  
'*-----------------------------------------------------------------------  
    cst.Open  
  
'*-----------------------------------------------------------------------  
'* Allow space for Row Header Text  
'*-----------------------------------------------------------------------  
strColumnHeader = vbTab & vbTab & vbTab & vbTab & vbTab & vbTab  
  
'*-----------------------------------------------------------------------  
'* Loop through Column Headers  
'*-----------------------------------------------------------------------  
       For i = 0 To cst.Axes(0).Positions.Count - 1  
            strColumnHeader = strColumnHeader & _  
                cst.Axes(0).Positions(i).Members(0).Caption & vbTab & _  
                    vbTab & vbTab & vbTab  
       Next  
       Debug.Print vbTab & strColumnHeader & vbCrLf  
  
'*-----------------------------------------------------------------------  
'* Loop through Row Headers and Provide data for each row  
'*-----------------------------------------------------------------------  
        strRowText = ""  
        For j = 0 To cst.Axes(1).Positions.Count - 1  
            strRowText = strRowText & _  
                cst.Axes(1).Positions(j).Members(0).Caption & vbTab & _  
                    vbTab & vbTab & vbTab  
            For k = 0 To cst.Axes(0).Positions.Count - 1  
                strRowText = strRowText & cst(k, j).FormattedValue & _  
                    vbTab & vbTab & vbTab & vbTab  
            Next  
            Debug.Print strRowText & vbCrLf  
            strRowText = ""  
        Next  
  
    Screen.MousePointer = vbDefault  
Exit Sub  
  
Error_cmdCellSettoDebugWindow_Click:  
   Beep  
   Screen.MousePointer = vbDefault  
   MsgBox "The Following Error has occurred:" & vbCrLf & _  
      Err.Description, vbCritical, " Error!"  
   Exit Sub  
End Sub  
```
