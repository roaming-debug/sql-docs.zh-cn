---
description: 提供程序错误
title: 提供程序错误 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors collection [ADO]
- provider errors [ADO]
- event-related errors [ADO]
- errors [ADO], provider
- Error object [ADO], provider errors
ms.assetid: cc7d6ff9-2034-45c6-9d61-90b177010054
author: rothja
ms.author: jroth
ms.openlocfilehash: a13bcc83c360533b1137af69b688221157dc0dc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032600"
---
# <a name="provider-errors"></a>提供程序错误
发生提供程序错误时，将返回运行时错误-2147467259。 如果收到此错误，请检查活动 **连接** 对象的 **错误** 集合，该集合包含一个或多个描述发生的错误的错误。  
  
## <a name="the-ado-errors-collection"></a>ADO 错误集合  
 由于特定 ADO 操作可能会产生多个提供程序错误，因此 ADO 通过 **连接** 对象公开错误对象的集合。 如果操作成功完成，则此集合不包含任何对象，如果出现错误，则包含一个或多个 **错误** 对象，提供程序引发一个或多个错误。 检查每个错误对象以确定错误的确切原因。  
  
 处理完任何已发生的错误后，就可以通过调用 **clear** 方法清除集合。 尤其重要的是，在对 **记录集** 对象（**连接** 对象上的 **Open** 方法）调用 **Resync**、 **UpdateBatch** 或 **CancelBatch** 方法之前显式清除 **错误** 集合，或在 **recordset** 对象上设置 **筛选器** 属性。 通过显式清除集合，可以确定集合中的任何 **错误** 对象不会从上一个操作中遗留。  
  
 除了错误外，某些操作还可以生成警告。 **错误** 集合中的 **错误** 对象也表示警告。 当提供程序向集合添加警告时，它不会生成运行时错误。 检查 "**错误**" 集合的 "**计数**" 属性，以确定特定操作是否生成了警告。 如果计数为1或更大，则已将 **错误** 对象添加到集合中。 一旦你确定 **错误** 集合包含错误或警告，你就可以循环访问该集合并检索有关它所包含的每个 **错误** 对象的信息。 下面的简短 Visual Basic 示例演示了这一点：  
  
```  
' BeginErrorHandlingVB02  
Private Function DeleteCustomer(ByVal CompanyName As String) As Long  
    On Error GoTo DeleteCustomerError  
  
    rst.Find "CompanyName='" & CompanyName & "'"  
DeleteCustomerError:  
Dim objError As ADODB.Error  
Dim strError As String  
  
    If cnn.Errors.Count > 0 Then  
        For Each objError In cnn.Errors  
            strError = strError & "Error #" & objError.Number & _  
                " " & objError.Description & vbCrLf & _  
                "NativeError: " & objError.NativeError & vbCrLf & _  
                "SQLState: " & objError.SQLState & vbCrLf & _  
                "Reported by: " & objError.Source & vbCrLf & _  
                "Help file: " & objError.HelpFile & vbCrLf & _  
                "Help Context ID: " & objError.HelpContext  
        Next  
        MsgBox strError  
    End If  
End Function  
' EndErrorHandlingVB02  
```  
  
 错误处理例程包含一个 **For each** 循环，用于检查 **错误** 集合中的每个对象。 在此示例中，它将累积一条消息以供显示。 在工作程序中，您将编写代码以便为每个错误执行相应的任务，例如关闭所有打开的文件并以有序方式关闭程序。  
  
## <a name="the-error-object"></a>Error 对象  
 通过检查 **错误** 对象，您可以确定发生了什么错误，更重要的是哪个应用程序或哪个对象导致了该错误。 **错误** 对象具有以下属性：  
  
|属性名称|说明|  
|-------------------|-----------------|  
|**说明**|发生的错误的文本说明。|  
|**HelpContext、帮助**|引用包含所发生错误的说明的帮助主题和帮助文件。|  
|**NativeError**|特定于提供程序的错误号。|  
|**数字**|一个长整型，表示在发生的错误的 **ErrorValueEnum**) 中列出 (的数目。|  
|**Source**|指示生成错误的对象或应用程序的名称。|  
|**SQLState**|提供程序在 SQL 语句过程中返回的五个字符的错误代码。|  
  
 ADO **错误** 对象与标准 Visual Basic **Err** 对象非常类似。 其属性描述发生的错误。 除了错误编号，还会收到两个相关的信息片段。 **NativeError** 属性包含特定于所使用的提供程序的错误号。 在上面的示例中，提供程序是用于 SQL Server 的 Microsoft OLE DB 提供程序，因此 **NativeError** 将包含特定于 SQL Server 的错误。 **SQLState** 属性包含一个由五个字母组成的代码，用于描述 SQL 语句中的错误。  
  
## <a name="event-related-errors"></a>Event-Related 错误  
 如果发生事件相关错误，也可以使用 **Error** 对象。 通过检查作为事件参数传递的 **error** 对象，可以确定引发 ADO 事件的进程中是否发生了错误。  
  
 如果导致事件的操作成功结束，事件处理程序的 *adStatus* 参数将设置为 *adStatusOK*。 另一方面，如果引发事件的操作未成功，则会将 *adStatus* 参数设置为 *adStatusErrorsOccurred*。 在这种情况下， *pError* 参数将包含描述错误的 **错误** 对象。
