---
description: ADO 事件实例化：Visual Basic
title: ADO 事件实例化： Visual Basic |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
ms.assetid: dce0a2a3-326f-4aaf-a822-6c5549833afa
author: rothja
ms.author: jroth
ms.openlocfilehash: 764c1d06be70c808881d74c8e458b74a432ddfe3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033347"
---
# <a name="ado-event-instantiation-visual-basic"></a>ADO 事件实例化：Visual Basic
若要在 Microsoft® Visual Basic®中处理 ADO 事件，必须使用 **WithEvents** 关键字声明模块级变量。 变量只能声明为类模块的一部分，并且必须在模块级别声明。 但这并不像看起来一样严格，因为 Visual Basic **窗体** 对象也是类。 处理 ADO 事件的最简单方法是使用 **WithEvents** 声明变量。 下面的示例处理 **连接** 对象的 **ConnectComplete** 事件：  
  
```  
' BeginEventExampleVB02  
Dim WithEvents connEvent As Connection  
Attribute connEvent.VB_VarHelpID = -1  
  
Private Sub Form_Load()  
Dim strConn As String  
  
   ' Create a new object with event  
   ' handling enabled.  
   strConn = "Provider=sqloledb;" & _  
      "Data Source=MyServer;" & _  
      "Initial Catalog=Northwind;" & _  
      "Integrated Security=SSPI;"  
   Set connEvent = New ADODB.Connection  
   connEvent.Open strConn  
End Sub  
  
Private Sub connEvent_ConnectComplete(ByVal pError As ADODB.Error, _  
    adStatus As ADODB.EventStatusEnum, _  
    ByVal pConnection As ADODB.Connection)  
Dim strMsg As String  
  
   If adStatus = adStatusErrorsOccurred Then  
      Select Case pError.Number  
         Case adErrOperationCancelled  
            ' The operation was cancelled in the  
            ' Will event. Notify the user and exit.  
            strMsg = "I'm sorry you can't connect right now." & vbCrLf  
            strMsg = strMsg & "Click OK to exit."  
            Unload Me  
         Case Else  
            strMsg = "Error " & Format(pError.Number) & vbCrLf  
            strMsg = strMsg & pError.Description  
            strMsg = strMsg & "Click OK to exit."  
            Unload Me  
      End Select  
   End If  
   frmWait.btnOK.Enabled = True  
End Sub  
' EndEventExampleVB02  
```  
  
 使用 **WithEvents** 关键字在 **窗体** 级别声明 **连接** 对象，以启用事件处理。 Form_Load 事件处理程序实际上通过向 *connEvent* 分配新的 **连接** 对象，然后打开连接来创建对象。 当然，实际的应用程序会在 Form_Load 事件处理程序中执行更多处理，而不是在此处显示。
