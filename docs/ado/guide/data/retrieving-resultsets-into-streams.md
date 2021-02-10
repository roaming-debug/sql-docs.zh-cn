---
description: 检索流中的结果集
title: 检索流中的结果集 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/20/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- streams [ADO], retrieving query results
- query results into stream [ADO]
- retrieving results into stream [ADO]
ms.assetid: 996c1321-c926-4f57-8297-85c8c20de974
author: rothja
ms.author: jroth
ms.openlocfilehash: f625a4f96aa5ec083057af4118315d9df494530e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032539"
---
# <a name="retrieving-resultsets-into-streams"></a>检索流中的结果集
ADO 不必接收传统 **记录集** 对象中的结果，而是可以将查询结果检索到流中。 ADO **流** 对象 (或支持 COM **IStream** 接口的其他对象，如 ASP **请求** 和 **响应** 对象) 可用于包含这些结果。 此功能的一种用途是检索 XML 格式的结果。 例如，使用 SQL Server，可以通过多种方式返回 XML 结果，例如，使用带有 SQL SELECT 查询的 FOR XML 子句或使用 XPath 查询。  
  
 若要以流格式而不是在 **记录集中** 接收查询结果，必须将 **ExecuteOptionEnum** 中的 **adExecuteStream** 常量指定为 **命令** 对象的 **Execute** 方法的参数。 如果提供程序支持此功能，则在执行时将在流中返回结果。 可能需要在代码执行之前指定其他特定于访问接口的属性。 例如，对于 SQL Server 的 Microsoft OLE DB 提供程序，必须指定 **Command** 对象的 **properties** 集合中的 **Output Stream** 之类的属性。 有关与此功能相关 SQL Server 特定的动态属性的详细信息，请参阅 SQL Server 联机丛书中的 XML-Related 属性。  
  
## <a name="for-xml-query-example"></a>FOR XML 查询示例  
 下面的示例通过 VBScript 写入 Northwind 数据库：  
  
```html
<!-- BeginRecordAndStreamVBS -->  
<%@ LANGUAGE = VBScript %>  
<%  Option Explicit      %>  
  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Developer Studio"/>  
<META HTTP-EQUIV="Content-Type" content="text/html"; charset="iso-8859-1">  
<TITLE>FOR XML Query Example</TITLE>  
  
<STYLE>  
   BODY  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
  
   H3  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
</STYLE>  
  
<!-- #include file="adovbs.inc" -->  
<%  
   Response.Write "<H3>Server-side processing</H3>"  
  
   Response.Write "Page Generated @ " & Now() & "<BR/>"  
  
   Dim adoConn  
   Set adoConn = Server.CreateObject("ADODB.Connection")  
  
   Dim sConn  
   sConn = "Provider=SQLOLEDB;Data Source=" & _  
      Request.ServerVariables("SERVER_NAME") & ";" & _  
      Initial Catalog=Northwind;Integrated Security=SSPI;"  
  
   Response.write "Connect String = " & sConn & "<BR/>"  
  
   adoConn.ConnectionString = sConn  
   adoConn.CursorLocation = adUseClient  
  
   adoConn.Open  
  
   Response.write "ADO Version = " & adoConn.Version & "<BR/>"  
   Response.write "adoConn.State = " & adoConn.State & "<BR/>"  
  
   Dim adoCmd  
   Set adoCmd = Server.CreateObject("ADODB.Command")  
   Set adoCmd.ActiveConnection = adoConn  
  
   Dim sQuery  
   sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'><sql:query>SELECT * FROM PRODUCTS WHERE ProductName='Gumbr Gummibrchen' FOR XML AUTO</sql:query></ROOT>"  
  
   Response.write "Query String = " & sQuery & "<BR/>"  
  
   Dim adoStreamQuery  
   Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
   adoStreamQuery.Open  
   adoStreamQuery.WriteText sQuery, adWriteChar  
   adoStreamQuery.Position = 0  
  
   adoCmd.CommandStream = adoStreamQuery  
   adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
  
   Response.write "Pushing XML to client for processing "  & "<BR/>"  
  
   adoCmd.Properties("Output Stream") = Response  
   Response.write "<XML ID='MyDataIsle'>"  
   adoCmd.Execute , , 1024  
   Response.write "</XML>"  
  
%>  
  
<SCRIPT language="VBScript" For="window" Event="onload">  
   Dim xmlDoc  
   Set xmlDoc = MyDataIsle.XMLDocument  
   xmlDoc.resolveExternals=false  
   xmlDoc.async=false  
  
   If xmlDoc.parseError.Reason <> "" then  
      Msgbox "parseError.Reason = " & xmlDoc.parseError.Reason  
   End If  
  
   Dim root, child  
   Set root = xmlDoc.documentElement  
   For each child in root.childNodes  
      dim OutputXML  
      OutputXML = document.all("log").innerHTML  
      document.all("log").innerHTML = OutputXML & "<LI>" & child.getAttribute("ProductName") & "</LI>"  
   Next  
</SCRIPT>  
  
</HEAD>  
  
<BODY>  
  
   <H3>Client-side processing of XML Document MyDataIsle</H3>  
   <UL id=log>  
   </UL>  
  
</BODY>  
</HTML>  
<!-- EndRecordAndStreamVBS -->  
  
```  
  
 FOR XML 子句指示 SQL Server 以 XML 文档的形式返回数据。  
  
### <a name="for-xml-syntax"></a>FOR XML 语法  
  
```syntax
FOR XML [RAW|AUTO|EXPLICIT]  
```  
  
 FOR XML RAW 生成将列值作为属性的通用行元素。 FOR XML AUTO 使用试探法根据表名称生成包含元素名称的层次结构树。 FOR XML EXPLICIT 会生成一个通用表，其中的关系由元数据完全描述。  
  
 下面是 SQL SELECT FOR XML 语句的示例：  
  
```sql
SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO  
```  
  
 此命令可以在字符串中指定，如前面所示，分配给 **CommandText**，或指定给 **CommandStream** 的 XML 模板查询的形式。 有关 XML 模板查询的详细信息，请参阅 ADO 中的 [命令流](../../../ado/guide/data/command-streams.md) 或使用 SQL Server 联机丛书中的命令输入流。  
  
 作为 XML 模板查询，FOR XML 查询如下所示：  
  
```xml
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO </sql:query>  
```  
  
 此示例为 **输出流** 属性指定 ASP **响应** 对象：  
  
```vb
adoCmd.Properties("Output Stream") = Response  
```  
  
 接下来，指定 **Execute** 的 **adExecuteStream** 参数。 此示例包装 XML 标记中的流以创建 XML 数据岛：  
  
```vb
Response.write "<XML ID=MyDataIsle>"  
adoCmd.Execute , , adExecuteStream  
Response.write "</XML>"  
```  
  
### <a name="remarks"></a>备注  
 此时，XML 已流式传输到客户端浏览器，并已准备好显示。 为此，可使用客户端 VBScript 将 XML 文档绑定到 DOM 实例，并循环遍历每个子节点以在 HTML 中生成产品列表。
