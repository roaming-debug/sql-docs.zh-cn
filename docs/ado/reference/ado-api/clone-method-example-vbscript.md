---
description: Clone 方法示例 (VBScript)
title: " (VBScript) 的 Clone 方法示例 |Microsoft Docs"
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
- Clone method [ADO], VBScript example
ms.assetid: 36b96e3d-8cb0-4b79-bd93-ea5e0eb5679f
author: rothja
ms.author: jroth
ms.openlocfilehash: f13ee1086b1b8ead1ca66ea537ee4f4942f46fde
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027095"
---
# <a name="clone-method-example-vbscript"></a>Clone 方法示例 (VBScript)
此示例使用 [Clone](./clone-method-ado.md) 方法创建 [记录集](./recordset-object-ado.md) 的副本，然后让用户单独定位每个副本的记录指针。  
  
 在 (ASP) 的活动服务器页中使用以下示例。 此示例使用通过 Microsoft Access 分发的 **Northwind** 数据库。 剪切下面的代码并将其粘贴到记事本或其他文本编辑器中，并将其保存为 CloneVBS。 您可以在任何客户端浏览器中查看结果。  
  
 若要执行此示例，请将行更改 `RsCustomerList.Source = "Customers"` 为， `RsCustomerList.Source = "Products"` 以对较大的表进行计数。  
  
```  
<!-- BeginCloneVBS -->  
<% Language = VBScript %>  
<%' use this meta tag instead of adovbs.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
<HTML>  
<HEAD>  
<TITLE>ADO Clone Method</TITLE>  
</HEAD>  
  
<BODY>  
  
<H1 align="center">ADO Clone Method</H1>  
<HR>  
<% ' to integrate/test this code replace the   
   ' Data Source value in the Connection string%>  
<%   
    ' connection and recordset variables  
    Dim Cnxn, strCnxn  
    Dim rsCustomers, strSQLCustomers  
    Dim rsFirst, rsLast, rsCount  
    Dim rsClone  
    Dim CloneFirst, CloneLast, CloneCount  
  
    ' open connection  
    Set Cnxn = Server.CreateObject("ADODB.Connection")  
    strCnxn = "Provider='sqloledb';Data Source=" & _  
            Request.ServerVariables("SERVER_NAME") & ";" & _  
            "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    Cnxn.Open strCnxn  
  
    ' create and open Recordset using object refs  
    Set rsCustomers = Server.CreateObject("ADODB.Recordset")  
    strSQLCustomers = "Customers"  
  
    rsCustomers.ActiveConnection = Cnxn  
    rsCustomers.CursorLocation = adUseClient  
    rsCustomers.CursorType = adOpenKeyset  
    rsCustomers.LockType = adLockOptimistic  
    rsCustomers.Source = strSQLCustomers  
    rsCustomers.Open  
  
    rsCustomers.MoveFirst  
    rsCount = rsCustomers.RecordCount  
    rsFirst = rsCustomers("CompanyName")  
    rsCustomers.MoveLast  
    rsLast = rsCustomers("CompanyName")  
  
    ' create clone  
    Set rsClone = rsCustomers.Clone  
    rsClone.MoveFirst  
    CloneCount = rsClone.RecordCount  
    CloneFirst = rsClone("CompanyName")  
    rsClone.MoveLast  
    CloneLast = rsClone("CompanyName")  
%>  
  
<!-- Display Results -->  
<H3>There Are <%=rsCount%> Records in the original recordset</H3>  
<H3>The first record is <%=rsFirst%> and the last record is <%=rsLast%></H3>  
<BR><HR>  
<H3>There Are <%=CloneCount%> Records in the original recordset</H3>  
<H3>The first record is <%=CloneFirst%> and the last record is <%=CloneLast%></H3>  
<BR><HR>  
<H4>Location of OLEDB Database</H4>  
  
<%  
    ' Show location of DSN data source  
    Response.Write(Cnxn)  
  
    ' Clean up  
    If rsCustomers.State = adStateOpen then  
       rsCustomers.Close  
    End If  
    If rsClone.State = adStateOpen then  
       rsClone.Close  
    End If  
    If Cnxn.State = adStateOpen then  
       Cnxn.Close  
    End If  
    Set rsCustomers = Nothing  
    Set rsClone = Nothing  
    Set Cnxn = Nothing  
%>  
</BODY>  
</HTML>  
<!-- EndCloneVBS -->  
```  
  
## <a name="see-also"></a>另请参阅  
 [ADO)  (克隆方法 ](./clone-method-ado.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)