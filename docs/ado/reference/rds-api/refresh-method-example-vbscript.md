---
description: Refresh 方法示例 (VBScript)
title: " (VBScript) 的 Refresh 方法示例 |Microsoft Docs"
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Refresh method [ADO], VBScript example
ms.assetid: f2926578-bc60-464b-916e-ddfdb8014253
author: rothja
ms.author: jroth
ms.openlocfilehash: e6af828ea67719386d2807e6608d8d3f7872f664
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052948"
---
# <a name="refresh-method-example-vbscript"></a>Refresh 方法示例 (VBScript)
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 下面的示例演示如何设置 RDS 的必需参数 [。](./datacontrol-object-rds.md) 在运行时 DataControl。 使用[Refresh](../ado-api/refresh-method-ado.md)方法检索[记录集](../ado-api/recordset-object-ado.md)的方式由[ExecuteOptions](./executeoptions-property-rds.md)和[FetchOptions](./fetchoptions-property-rds.md)属性的设置确定。 若要测试此示例，请将以下代码剪切并粘贴到普通的 ASP 文档中，然后将其命名为 **RefreshVBS**。 使用 " **查找** " 找到文件 Adovbs，并将其放入计划使用的目录中。 ASP 脚本将标识您的服务器。  
  
```  
<!-- BeginRefreshVBS -->  
<%@ Language=VBScript %>  
<!--use the following META tag instead of adovbs.inc-->  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
<html>  
<head>  
    <meta name="VI60_DefaultClientScript"  content=VBScript>  
    <meta name="GENERATOR" content="Microsoft Visual Studio 6.0">  
    <title>Refresh Method Example (VBScript)</title>  
<style>  
<!--  
body {  
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
  
<body>  
<h1>Refresh Method Example (VBScript)</h1>  
  
<H2>RDS API Code Examples </H2>  
<HR>  
<TABLE DATASRC=#RDC>  
   <TR>  
      <TD> <INPUT DATAFLD="FirstName" SIZE=15> </TD>  
      <TD> <INPUT DATAFLD="LastName" SIZE=15> </TD>  
      <TD> <INPUT DATAFLD="Title" SIZE=15> </TD>  
      <TD> <INPUT DATAFLD="HireDate" SIZE=15> </TD>  
   </TR>  
</TABLE>  
  
<!-- RDS.DataControl with no parameters set at design time -->  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=RDC HEIGHT=1 WIDTH=1>  
</OBJECT>  
<HR>  
Server: <Input Size=70 Name="txtServer" Value="https://<%=Request.ServerVariables("SERVER_NAME")%>"><BR>  
Connect: <Input Size=70 Name="txtConnect" Value="Provider='sqloledb';Integrated Security='SSPI';Initial Catalog='Northwind'"><BR>  
SQL: <Input Size=70 Name="txtSQL" Value="Select * from Employees">  
<HR>  
<TABLE BORDER=1 WIDTH="60%">  
<TR>  
   <TD COLSPAN=3 BGCOLOR=silver>  
   Choose if you want the Recordset brought back Synchronously on the   
   current calling thread or Asynchronously on another thread.   
   </TD>  
</TR>  
<TR>  
   <TD>Synchronously: <BR>  
      <Input Type="Radio" Name="optExecuteOptions" Checked OnClick="SetExO('adcExecSync')">  
   </TD>  
   <TD>Asynchronously: <BR>  
      <Input Type="Radio" Name="optExecuteOptions"  OnClick="SetExO('adcExecAsync')">  
   </TD>  
   <TD> </TD>  
</TR>  
<TR>  
   <TD COLSPAN=3 BGCOLOR=silver>  
   Fetch Up Front, Background Fetch with Blocking or Background Fetch   
   without Blocking   
   </TD>  
<TR>  
   <TD>Up Front:<BR>  
       <Input Type="Radio" Name="optFetchOptions"  OnClick="SetFO('adcFetchUpFront')">  
   </TD>  
   <TD>Background w/ Blocking:<BR>  
       <Input Type="Radio" Name="optFetchOptions" Checked OnClick="SetFO('adcFetchBackground')">  
   </TD>  
   <TD>Background w/o Blocking:<BR>  
      <Input Type="Radio" Name="optFetchOptions"  OnClick="SetFO('adcFetchAsync')">  
   </TD>  
</TR>  
</TABLE>  
  
<INPUT TYPE=BUTTON NAME="Run" VALUE="Run"><BR>  
  
<Script Language="VBScript">  
<!--  
Dim EO         'ExecuteOptions  
Dim FO         'FetchOptions  
EO = "adcExecSync"   'Default value  
FO = "adcFetchBackground"   'Default value  
  
Sub SetExO(NewEO)  
   EO = NewEO  
End Sub  
  
Sub SetFO(NewFO)  
   FO = NewFO  
End Sub  
  
' Set parameters of RDS.DataControl at Run Time  
Sub Run_OnClick  
   RDC.Server = txtServer.Value  
   RDC.SQL = txtSQL.Value  
   RDC.Connect = txtConnect.Value  
   If EO = "adcExecSync" Then   'Determine which ExecuteOption chosen  
      RDC.ExecuteOptions = adcExecSync  
      MsgBox "Recordset brought in on current calling thread Syncronously"  
   Else  
      RDC.ExecuteOptions = adcExecAsync  
      MsgBox "Recordset brought in on another thread Asyncronously"  
   End If  
  
   If FO = "adcFetchBackground" Then      'Determine                 which FetchOption chosen  
      RDC.FetchOptions = adcFetchBackground  
      MsgBox "Control goes back to user after first batch of records returned"  
   ElseIf FO = " adcFetchUpFront" Then  
      RDC.FetchOptions = adcFetchUpFront  
      MsgBox "All records returned before control goes back to user"  
   Else  
      RDC.FetchOptions = adcFetchAsync  
      MsgBox "Control goes back to user immediately"  
   End If  
  
   RDC.Refresh  
End Sub  
-->  
</Script>  
  
</body>  
</html>  
<!-- EndRefreshVBS -->  
```  
  
## <a name="see-also"></a>另请参阅  
 [DataControl 对象 (RDS) ](./datacontrol-object-rds.md)   
 [ExecuteOptions 属性 (RDS) ](./executeoptions-property-rds.md)   
 [FetchOptions 属性 (RDS) ](./fetchoptions-property-rds.md)   
 [ADO)  (Recordset 对象 ](../ado-api/recordset-object-ado.md)   
 [Refresh 方法 (ADO)](../ado-api/refresh-method-ado.md)