---
description: 流和暂留
title: 流和持久性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisted streams [ADO]
- streams [ADO], persistence
ms.assetid: ad5bf52c-fd10-4cfa-bf7d-fcedcaa41eea
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b554afa949acea4f816ce66e92e897477c2fe00
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032419"
---
# <a name="streams-and-persistence"></a>流和暂留
[Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)对象 [保存](../../../ado/reference/ado-api/save-method.md)方法存储或 *保留* 文件中的 **记录集**，而 [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md)方法则从该文件还原 **记录集**。  
  
 使用 ADO 2.7 或更高版本， **保存** 和 **打开** 方法也可以将 **记录集** 保存到 [流](../../../ado/reference/ado-api/stream-object-ado.md) 对象。 使用远程数据服务时，此功能尤其有用 (RDS) 和 Active Server Pages (ASP) 。  
  
 有关如何在 ASP 页上自行使用持久性的详细信息，请参阅当前 ASP 文档。  
  
 下面是一些演示如何使用 **流** 对象和持久性的方案。  
  
## <a name="scenario-1"></a>方案 1  
 此方案只是将 **记录集** 保存到文件，然后保存到 **流** 中。 然后，它会将保存的流打开到另一个 **记录集中**。  
  
```  
Dim rs1 As ADODB.Recordset  
Dim rs2 As ADODB.Recordset  
Dim stm As ADODB.Stream  
  
Set rs1 = New ADODB.Recordset  
Set rs2 = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
rs1.Open   "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;""", adopenStatic, adLockReadOnly, adCmdText  
rs1.Save "c:\myfolder\mysubfolder\myrs.xml", adPersistXML  
rs1.Save stm, adPersistXML  
rs2.Open stm  
```  
  
## <a name="scenario-2"></a>方案 2  
 此方案将 **记录集** 保存到 XML 格式的 **流** 中。 然后，它会将 **流** 读入一个字符串，可以检查、操作或显示该字符串。  
  
```  
Dim rs As ADODB.Recordset  
Dim stm As ADODB.Stream  
Dim strRst As String  
  
Set rs = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
' Open, save, and close the recordset.   
rs.Open "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
rs.Save stm, adPersistXML  
rs.Close  
Set rs = nothing  
  
' Put saved Recordset into a string variable.  
strRst = stm.ReadText(adReadAll)  
  
' Examine, manipulate, or display the XML data.  
...  
```  
  
## <a name="scenario-3"></a>方案 3  
 此示例代码演示了 ASP 代码将 **记录集** 作为 XML 直接保存到 **响应** 对象：  
  
```  
...  
<%  
response.ContentType = "text/xml"  
  
' Create and open a Recordset.  
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select * from Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
  
' Save Recordset directly into output stream.  
rs.Save Response, adPersistXML   
  
' Close Recordset.  
rs.Close  
Set rs = nothing  
%>  
...  
```  
  
## <a name="scenario-4"></a>方案 4  
 在此方案中，ASP 代码会将 ADTG 格式的 **记录集** 内容写入客户端。 [适用于 OLE DB 的 Microsoft 游标服务](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)可以使用此数据创建断开连接的 **记录集**。  
  
 RDS [DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md)， [URL](../../../ado/reference/rds-api/url-property-rds.md)上的新属性指向生成 **记录集** 的 .asp 页面。 这意味着，可以使用服务器端 [DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)对象或用户编写业务对象来获取 **记录集** 对象。 这大大简化了 RDS 编程模型。  
  
 服务器端代码，名为 https://server/directory/recordset.asp:  
  
```  
<%  
Dim rs   
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select au_fname, au_lname, phone from Authors", ""& _  
        "Provider=sqloledb;Data Source=MyServer;" & _  
        "Initial Catalog=Pubs;Integrated Security=SSPI;"  
response.ContentType = "multipart/mixed"  
rs.Save response, adPersistADTG  
%>  
```  
  
 客户端代码：  
  
```  
<HTML>  
<HEAD>  
<TITLE>RDS Query Page</TITLE>  
</HEAD>  
<body>  
<CENTER>  
<H1>Remote Data Service 2.5</H1>  
<TABLE DATASRC="#DC1">  
   <TR>   
      <TD><SPAN DATAFLD="au_fname"></SPAN></TD>  
      <TD><SPAN DATAFLD="au_lname"></SPAN></TD>  
      <TD><SPAN DATAFLD="phone"></SPAN></TD>  
   </TR>  
</TABLE>  
<BR>  
  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
    ID=DC1 HEIGHT=1 WIDTH = 1>  
    <PARAM NAME="URL" VALUE="https://server/directory/recordset.asp">  
</OBJECT>  
  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 开发人员还可以选择在客户端上使用 **Recordset** 对象：  
  
```  
...  
function GetRs()   
    {  
    rs = CreateObject("ADODB.Recordset");  
    rs.Open "https://server/directory/recordset.asp"  
    DC1.SourceRecordset = rs;  
    }  
...  
```  
  
## <a name="see-also"></a>另请参阅  
 [ADO 记录集 (打开方法) ](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [ (ADO 记录对象) ](../../../ado/reference/ado-api/record-object-ado.md)   
 [Save 方法](../../../ado/reference/ado-api/save-method.md)
