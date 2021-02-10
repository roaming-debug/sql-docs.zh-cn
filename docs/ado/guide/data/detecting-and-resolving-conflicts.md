---
description: 检测和解决冲突
title: 检测和解决冲突 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- conflicts [ADO], detecting and resolving
- ADO, detecting and resolving conflicts
ms.assetid: b28fdd26-c1a4-40ce-a700-2b0c9d201514
author: rothja
ms.author: jroth
ms.openlocfilehash: 4c92b847b95aacd5cf642c9646f4618dc5986abf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037627"
---
# <a name="detecting-and-resolving-conflicts"></a>检测和解决冲突
如果在即时模式下处理记录集，则可能会有很少的并发问题发生。 另一方面，如果应用程序使用批处理模式更新，则可能会有很好的机会，让一个用户在另一个用户编辑相同记录所做的更改保存之前更改记录。 在这种情况下，你将希望应用程序妥善处理冲突。 你可能希望最后一位用户将更新发送到服务器 "wins"。 或者，您可能想让最近的用户通过在两个冲突值之间进行选择来决定哪个更新的优先级。  
  
 无论何种情况，ADO 都提供 Field 对象的 UnderlyingValue 和 OriginalValue 属性来处理这些类型的冲突。 将这些属性与记录集的 Resync 方法和 Filter 属性结合使用。  
  
## <a name="remarks"></a>备注  
 当 ADO 在批更新过程中遇到冲突时，将在错误集合中添加一条警告。 因此，在调用 BatchUpdate 之后，应始终立即检查是否存在错误，如果找到这些错误，则开始测试假设您遇到了冲突。 第一步是将记录集的 "筛选器" 属性设置为等于 "adFilterConflictingRecords"。 这会将记录集上的视图限制为仅存在发生冲突的记录。 如果在执行此步骤后，RecordCount 属性等于零，则表明该错误是由冲突以外的内容引发的。  
  
 调用 BatchUpdate 时，ADO 和提供程序正在生成 SQL 语句，以便对数据源执行更新。 请记住，某些数据源对于 WHERE 子句中可以使用哪些类型的列有限制。  
  
 接下来，在 AffectRecords 参数设置为 adAffectGroup 且 ResyncValues 参数设置等于 adResyncUnderlyingValues 的记录集中调用 Resync 方法。 Resync 方法从基础数据库更新当前记录集对象中的数据。 通过使用 adAffectGroup，你可以确保只有与当前筛选器设置一起显示的记录才会与数据库重新同步。 如果正在处理大型记录集，这可能会显著提高性能。 通过将 ResyncValues 参数设置为 adResyncUnderlyingValues 时，在调用重新同步时，你可以确保 UnderlyingValue 属性包含数据库中的 (冲突) 值，该值属性将保留用户输入的值，并且 OriginalValue 属性将保留该字段的原始值 (在上次成功的 UpdateBatch 调用) 之前的值。 然后，你可以使用这些值以编程方式解决冲突，或者要求用户选择将使用的值。  
  
 下面的代码示例演示了此方法。 示例人为在调用 UpdateBatch 之前，通过使用单独的记录集更改基础表中的值来创建冲突。  
  
```  
'BeginConflicts  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT * FROM Shippers WHERE ShipperID = 2"  
  
    'Open Rs and change a value  
    Set objRs1 = New ADODB.Recordset  
    Set objRs2 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Introduce a conflict at the db...  
    objRs2.Open strSQL, strConn, adOpenKeyset, adLockOptimistic, adCmdText  
    objRs2("Phone") = "(999) 555-9999"  
    objRs2.Update  
    objRs2.Close  
    Set objRs2 = Nothing  
  
    On Error Resume Next  
    objRs1.UpdateBatch  
  
    If objRs1.ActiveConnection.Errors.Count <> 0 Then  
        Dim intConflicts As Integer  
  
        intConflicts = 0  
  
        objRs1.Filter = adFilterConflictingRecords  
  
        intConflicts = objRs1.RecordCount  
  
        'Resync so we can see the UnderlyingValue and offer user a choice.  
        'This sample only displays all three values and resets to original.  
        objRs1.Resync adAffectGroup, adResyncUnderlyingValues  
  
        If intConflicts > 0 Then  
            strMsg = "A conflict occurred with updates for " & intConflicts & _  
                     " record(s)." & vbCrLf & "The values will be restored" & _  
                     " to their original values." & vbCrLf & vbCrLf  
  
            objRs1.MoveFirst  
            While Not objRs1.EOF  
              strMsg = strMsg & "SHIPPER = " & objRs1("CompanyName") & vbCrLf  
              strMsg = strMsg & "Value = " & objRs1("Phone").Value & vbCrLf  
              strMsg = strMsg & "UnderlyingValue = " & _  
                                 objRs1("Phone").UnderlyingValue & vbCrLf  
              strMsg = strMsg & "OriginalValue = " & _  
                                 objRs1("Phone").OriginalValue & vbCrLf  
              strMsg = strMsg & vbCrLf & "Original value has been restored."  
  
              MsgBox strMsg, vbOKOnly, _  
                    "Conflict " & objRs1.AbsolutePosition & _  
                    " of " & intConflicts  
  
              objRs1("Phone").Value = objRs1("Phone").OriginalValue  
              objRs1.MoveNext  
            Wend  
  
            objRs1.UpdateBatch adAffectGroup  
        Else  
            'Other error occurred. Minimal handling in this example.  
             strMsg = "Errors occurred during the update. " & _  
                        objRs1.ActiveConnection.Errors(0).Number & " " & _  
                        objRs1.ActiveConnection.Errors(0).Description  
        End If  
  
        On Error GoTo 0  
    End If  
  
    objRs1.MoveFirst  
    objRs1.Close  
    Set objRs1 = Nothing  
'EndConflicts  
```  
  
 您可以使用当前记录或特定字段的 Status 属性来确定发生了哪种冲突。  
  
 有关错误处理的详细信息，请参阅 [错误处理](./error-handling.md)。  
  
## <a name="see-also"></a>另请参阅  
 [批处理模式](./batch-mode.md)