---
description: 'AbsolutePage、PageCount 和 PageSize 属性示例 (JScript) '
title: AbsolutePage、PageCount 和 PageSize 属性示例 (JScript) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- JScript
helpviewer_keywords:
- PageCount property [ADO], JScript example
- AbsolutePage property [ADO], JScript example
- PageSize property [ADO], JScript example
ms.assetid: 2db6dd3f-5a9c-438c-ae62-d09242906c98
author: rothja
ms.author: jroth
ms.openlocfilehash: 0acbdc3a9a24c00ac83ae4a28bf9abcd85ca7cfe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031679"
---
# <a name="absolutepage-pagecount-and-pagesize-properties-example-jscript"></a>AbsolutePage、PageCount 和 PageSize 属性示例 (JScript) 
此示例演示了 AbsolutePage、PageCount 和 PageSize 属性。 剪切下面的代码并将其粘贴到记事本或其他文本编辑器中，并将其保存为 **AbsolutePageJS**。  
  
```  
<!-- BeginAbsolutePageJS -->  
<%@LANGUAGE="JScript" %>  
<%// use this meta tag instead of adojavas.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
  
<html>  
  
<head>  
<title>AbsolutePage, PageSize, and PageSize Properties (JScript)</title>  
<style>  
<!--  
BODY {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
.thead2 {  
   background-color: #800000;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.tbody {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
    }  
-->  
</style>  
</head>  
  
<body bgcolor="White">  
<h1>AbsolutePage, PageSize, and PageSize Properties (JScript)</h1>  
<%  
    // connection and recordset variables  
    var Cnxn = Server.CreateObject("ADODB.Connection")  
    var strCnxn = "Provider='sqloledb';Data Source=" + Request.ServerVariables("SERVER_NAME") + ";" +  
            "Initial Catalog='Northwind';Integrated Security='SSPI';";  
    var rsEmployee = Server.CreateObject("ADODB.Recordset");  
    // display variables  
    var strMessage, iRecord, iPageCount;  
  
    try  
    {  
        // open connection  
        Cnxn.Open(strCnxn);  
  
        // Open a recordset on the Employee table using  
        // a client-side cursor to enable AbsolutePage property.  
        rsEmployee.CursorLocation = adUseClient;  
        rsEmployee.Open("employees", strCnxn, adOpenStatic, adLockOptimistic, adCmdTable);  
  
        // Set PageSize to five to display names and hire dates of five employees at a time  
        rsEmployee.PageSize = 5;   
        iPageCount = rsEmployee.PageCount;  
  
        // Write header information to the document  
        Response.Write('<p align="center">There are ' + iPageCount);  
        Response.Write(" pages, each containing ");  
        Response.Write(rsEmployee.PageSize + " or fewer records.</p>");  
        Response.Write('<table border="0" align="center">');  
        Response.Write('<tr class="thead2">');  
        Response.Write("<th>Page</th><th>Name</th><th>Hire Date</th></tr>");  
  
        for (var i=1; i<=iPageCount; i++)  
        {  
            rsEmployee.AbsolutePage = i;  
  
            for (iRecord = 1; iRecord <= rsEmployee.PageSize; iRecord++)  
            {  
                strMessage = "";  
  
                    // Start a new table row.  
                    strMessage = '<tr class="tbody">';  
  
                    // First column in row contains page number on  
                    // first record of each page. Otherwise, the column  
                    // contains a non-breaking space.  
                    if (iRecord == 1)  
                        strMessage += "<td>Page " + i + " of " + rsEmployee.PageCount + "</td>"  
                    else  
                        strMessage += "<td> </td>";  
  
                    // First and last name are in first column.  
                    strMessage += "<TD>" + rsEmployee.Fields("FirstName") + " ";  
                    strMessage += rsEmployee.Fields("LastName") + " " + "</td>";  
  
                    // Hire date in second column.  
                    strMessage += "<td>" + rsEmployee.Fields("HireDate") + "</td>";  
  
                    // End the row.  
                    strMessage += "</tr>";  
  
                    // Write line to document.  
                    Response.Write(strMessage);  
  
                    // Get next record.  
                    rsEmployee.MoveNext;  
  
                    if (rsEmployee.EOF)  
                        break;  
            }  
        }  
  
        // Finish writing table.  
        Response.Write("</table>");  
    }  
    catch (e)  
    {  
        Response.Write(e.message);  
    }  
    finally  
    {  
        // 'clean up  
        if (rsEmployee.State == adStateOpen)  
            rsEmployee.Close;  
        if (Cnxn.State == adStateOpen)  
            Cnxn.Close;  
        rsEmployee = null;  
        Cnxn = null;  
    }  
%>  
  
</body>  
  
</html>  
<!-- EndAbsolutePageJS -->  
```  
  
## <a name="see-also"></a>另请参阅  
 [AbsolutePage 属性 (ADO) ](./absolutepage-property-ado.md)   
 [PageCount 属性 (ADO) ](./pagecount-property-ado.md)   
 [PageSize 属性 (ADO) ](./pagesize-property-ado.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)