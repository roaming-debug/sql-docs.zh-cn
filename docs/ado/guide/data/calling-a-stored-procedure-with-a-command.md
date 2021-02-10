---
description: 使用命令调用存储过程
title: 使用命令调用存储过程 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- calling stored procedures [ADO]
- stored procedures [ADO]
- commands [ADO]
ms.assetid: 685f7652-2271-4ede-b552-2eeb8c756b4c
author: rothja
ms.author: jroth
ms.openlocfilehash: c34dbecd5e078e19029161b29b66187b02514166
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033327"
---
# <a name="calling-a-stored-procedure-with-a-command"></a>使用命令调用存储过程
您可以使用命令来调用存储过程。 本主题末尾的代码示例引用 Northwind 示例数据库中名为 CustOrdersOrders 的存储过程，该存储过程按如下方式定义。  
  
```  
CREATE PROCEDURE CustOrdersOrders @CustomerID nchar(5) AS  
SELECT OrderID, OrderDate, RequiredDate, ShippedDate  
FROM Orders  
WHERE CustomerID = @CustomerID  
ORDER BY OrderID  
```  
  
 有关如何定义和调用存储过程的详细信息，请参阅您的 SQL Server 文档。  
  
 此存储过程类似于 [命令对象参数](./command-object-parameters.md)中使用的命令。 它采用客户 ID 参数并返回有关该客户的订单的信息。 下面的代码示例使用此存储过程作为 ADO **记录集** 的源。  
  
 使用存储过程可以访问 ADO 的另一功能： **Parameters** 收集 **Refresh** 方法。 通过使用此方法，ADO 可以自动填写有关运行时命令所需的参数的所有信息。 使用此方法会对性能产生负面影响，因为 ADO 必须查询数据源以获取有关参数的信息。  
  
 下面的代码示例与 [命令对象参数](./command-object-parameters.md)中的代码之间存在其他重要差异，其中的参数是手动输入的。 首先，此代码不会将 **准备好** 的属性设置为 **True** ，因为它是 SQL Server 的存储过程，并按定义进行预编译。 其次，在第二个示例中，**命令** 对象的 **CommandType** 属性更改为 **adCmdStoredProc** ，以通知 ADO 该命令为存储过程。  
  
 最后，在第二个示例中设置值时，必须通过索引引用参数，因为在设计时可能不知道参数的名称。 如果知道参数的名称，则可以将 **Command** 对象的 new [NamedParameters](../../reference/ado-api/namedparameters-property-ado.md)属性设置为 True，并引用该属性的名称。 您可能想知道在存储过程 (中提到的第一个参数的位置 @CustomerID) 为1而不是 0 (`objCmd(1) = "ALFKI"`) 。 这是因为参数0包含 SQL Server 存储过程的返回值。  
  
```  
'BeginAutoParamCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objParm1 As New ADODB.Parameter  
    Dim objRs As New ADODB.Recordset  
  
    ' Set CommandText equal to the stored procedure name.  
    objCmd.CommandText = "CustOrdersOrders"  
    objCmd.CommandType = adCmdStoredProc  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
    objCmd.ActiveConnection = objConn  
  
    ' Automatically fill in parameter info from stored procedure.  
    objCmd.Parameters.Refresh  
  
    ' Set the param value.  
    objCmd(1) = "ALFKI"  
  
    ' Execute once and display...  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    ' ...then set new param value, re-execute command, and display.  
    objCmd(1) = "CACTU"  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    'clean up  
    objRs.Close  
    objConn.Close  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
    Exit Sub  
  
ErrHandler:  
    'clean up  
    If objRs.State = adStateOpen Then  
        objRs.Close  
    End If  
  
    If objConn.State = adStateOpen Then  
        objConn.Close  
    End If  
  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
'EndAutoParamCmd  
  
'BeginNewConnection  
Private Function GetNewConnection() As ADODB.Connection  
    Dim oCn As New ADODB.Connection  
    Dim sCnStr As String  
  
    sCnStr = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    oCn.Open sCnStr  
  
    If oCn.State = adStateOpen Then  
        Set GetNewConnection = oCn  
    End If  
  
End Function  
'EndNewConnection  
```  
  
## <a name="see-also"></a>另请参阅  
 [知识库文章117500](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/185125)
