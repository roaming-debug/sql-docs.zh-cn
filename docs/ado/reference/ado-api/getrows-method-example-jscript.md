---
description: GetRows 方法示例 (JScript)
title: " (JScript) 的 GetRows 方法示例 |Microsoft Docs"
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
- Getrows method [ADO], JScript example
ms.assetid: d33467a5-5a56-450d-98c1-c3ce6f9f103c
author: rothja
ms.author: jroth
ms.openlocfilehash: fa40f6a65ee059115830832bbc7762d7bc98f22a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021023"
---
# <a name="getrows-method-example-jscript"></a>GetRows 方法示例 (JScript)
此示例使用 [GetRows](./getrows-method-ado.md)方法从 [记录集中](./recordset-object-ado.md)检索 *Custiomers* 表的所有行，并使用生成的数据来填充数组。 在以下两种情况下， **GetRows** 方法将返回小于所需的行数：如果已达到 [EOF](./bof-eof-properties-ado.md) ，或者 **getrows** 试图检索其他用户删除的记录，则为。 仅当发生第二种情况时，函数才返回 **False** 。 剪切下面的代码并将其粘贴到记事本或其他文本编辑器中，并将其保存为 **GetRowsJS**。  
  
```  
<!-- BeginGetRowsJS -->  
<%@ LANGUAGE="JScript" %>  
<%// use this meta tag instead of adojavas.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
  
<html>  
  
<head>  
<title>ADO Recordset.GetRows Example (JScript)</title>  
<style>  
<!--  
BODY {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
.thead {  
   background-color: #008080;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
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
  
<body bgcolor="white">  
  
<h1>ADO Recordset.GetRows Example (JScript)</h1>  
    <!-- Page text goes here -->  
<%  
        var Connect = "Provider='sqloledb';Data Source=" + Request.ServerVariables("SERVER_NAME") + ";" +  
            "Initial Catalog='Northwind';Integrated Security='SSPI';";  
    var mySQL = "select * from customers;";  
    var showblank = " ";  
    var shownull = "-null-";  
  
    var connTemp = Server.CreateObject("ADODB.Connection");  
  
    try  
    {  
        connTemp.Open(Connect);  
        var rsTemp = Server.CreateObject("ADODB.Recordset");  
        rsTemp.ActiveConnection = connTemp;  
        rsTemp.CursorLocation = adUseClient;  
        rsTemp.CursorType = adOpenKeyset;  
        rsTemp.LockType = adLockOptimistic;  
        rsTemp.Open(mySQL);  
  
        rsTemp.MoveFirst();  
  
        if (rsTemp.RecordCount == 0)  
        {  
            Response.Write("No records matched ");  
            Response.Write (mySQL & "So cannot make table...");  
            connTemp.Close();  
            Response.End();  
        } else  
        {  
            Response.Write('<table width="100%" border="2">');  
            Response.Write('<tr class="thead2">');  
  
            //  Headings On The Table for each Field Name  
            for (var i=0; i<rsTemp.Fields.Count; i++)  
            {  
                fieldObject = rsTemp.fields(i);  
                Response.Write('<td width="' + Math.floor(100 / rsTemp.Fields.Count) + '%">' + fieldObject.name + "</td>");  
            }  
  
            Response.Write("</tr>");  
  
            // JScript doesn't support multi-dimensional arrays  
            // so we'll convert the returned array to a single  
            // dimensional JScript array and then display the data.  
            tempArray = rsTemp.GetRows();  
            recArray = tempArray.toArray();  
  
            var col = 1;  
            var maxCols = rsTemp.Fields.Count;  
  
            for (var thisField=0; thisField<recArray.length; thisField++)  
            {  
                if (col == 1)  
                    Response.Write('<tr class="tbody">');  
                if (recArray[thisField] == null)  
                        recArray[thisField] = shownull;  
                if (recArray[thisField] == "")  
                        recArray[thisField] = showblank;  
                Response.Write("<td>" + recArray[thisField] + "</td>");  
                col++  
                if (col > maxCols)  
                {  
                    Response.Write("</tr>");  
                    col = 1;  
                }  
            }  
            Response.Write("</table>");  
        }  
    }  
    catch (e)  
    {  
        Response.Write(e.message);  
    }  
    finally  
    {  
        // clean up  
        if (rsTemp.State == adStateOpen)  
            rsTemp.Close;  
        if (connTemp.State == adStateOpen)  
            connTemp.Close;  
        rsTemp = null;  
        connTemp = null;  
    }  
%>  
  
</body>  
  
</html>  
<!-- EndGetRowsJS -->  
```  
  
## <a name="see-also"></a>另请参阅  
 [ (ADO 的 GetRows 方法) ](./getrows-method-ado.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)