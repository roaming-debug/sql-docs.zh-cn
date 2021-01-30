---
description: 'Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VB) '
title: 错误对象属性示例 (VB) |Microsoft Docs
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
- Number property [ADO], Visual Basic example
- Source property [ADO], Visual Basic example
- NativeError property [ADO], Visual Basic example
- Description property [ADO], Visual Basic example
- HelpFile property [ADO], Visual Basic example
- SQLState property [ADO], Visual Basic example
- HelpContext property [ADO], Visual Basic example
ms.assetid: 5c728458-d85c-497c-afcf-2cfa36c3342a
author: rothja
ms.author: jroth
ms.openlocfilehash: 38f6d333da8e2473255c8335e5b888824b66ff84
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167533"
---
# <a name="description-helpcontext-helpfile-nativeerror-number-source-and-sqlstate-properties-example-vb"></a>Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VB) 
此示例将触发错误，对其进行捕获，并显示生成的[error](../../../ado/reference/ado-api/error-object.md)对象的[Description](../../../ado/reference/ado-api/description-property.md)、 [HelpContext](../../../ado/reference/ado-api/helpcontext-helpfile-properties.md)、[帮助](../../../ado/reference/ado-api/helpcontext-helpfile-properties.md)程序、 [NativeError](../../../ado/reference/ado-api/nativeerror-property-ado.md)、 [Number](../../../ado/reference/ado-api/number-property-ado.md)、 [Source](../../../ado/reference/ado-api/source-property-ado-error.md)和[SQLState](../../../ado/reference/ado-api/sqlstate-property.md)属性。  
  
```  
'BeginDescriptionVB  
Public Sub Main()  
  
    Dim Cnxn As ADODB.Connection  
    Dim Err As ADODB.Error  
    Dim strError As String  
  
    On Error GoTo ErrorHandler  
  
    ' Intentionally trigger an error  
    Set Cnxn = New ADODB.Connection  
    Cnxn.Open "nothing"  
  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
  
    ' Enumerate Errors collection and display  
    ' properties of each Error object  
    For Each Err In Cnxn.Errors  
        strError = "Error #" & Err.Number & vbCr & _  
            "   " & Err.Description & vbCr & _  
            "   (Source: " & Err.Source & ")" & vbCr & _  
            "   (SQL State: " & Err.SQLState & ")" & vbCr & _  
            "   (NativeError: " & Err.NativeError & ")" & vbCr  
        If Err.HelpFile = "" Then  
            strError = strError & "   No Help file available"  
        Else  
            strError = strError & _  
               "   (HelpFile: " & Err.HelpFile & ")" & vbCr & _  
               "   (HelpContext: " & Err.HelpContext & ")" & _  
               vbCr & vbCr  
        End If  
  
        Debug.Print strError  
    Next  
  
    Resume Next  
End Sub  
'EndDescriptionVB  
```  
  
## <a name="see-also"></a>另请参阅  
 [Description 属性](../../../ado/reference/ado-api/description-property.md)   
 [Error 对象](../../../ado/reference/ado-api/error-object.md)   
 [HelpContext，帮助的属性](../../../ado/reference/ado-api/helpcontext-helpfile-properties.md)   
 [HelpContext，帮助的属性](../../../ado/reference/ado-api/helpcontext-helpfile-properties.md)   
 [NativeError 属性 (ADO) ](../../../ado/reference/ado-api/nativeerror-property-ado.md)   
 [ADO (的数字属性) ](../../../ado/reference/ado-api/number-property-ado.md)   
 [ (ADO 错误的源属性) ](../../../ado/reference/ado-api/source-property-ado-error.md)   
 [SQLState 属性](../../../ado/reference/ado-api/sqlstate-property.md)
