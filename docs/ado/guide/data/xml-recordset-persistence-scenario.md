---
description: XML 记录集暂留方案
title: XML 记录集持久性方案 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML persistence [ADO], persistence scenario
ms.assetid: 353d569a-043a-4397-9ee6-564c4af8d5f6
author: rothja
ms.author: jroth
ms.openlocfilehash: bbad3ddcfd8c34be6ea911f4cef580832f1c55b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032219"
---
# <a name="xml-recordset-persistence-scenario"></a>XML 记录集暂留方案
在这种情况下，你将创建一个 (ASP) 应用程序的活动服务器页面，该应用程序将记录集对象的内容直接保存到 ASP 响应对象。  
  
> [!NOTE]
>  此方案要求服务器将 Internet Information Server 5.0 (IIS) 或更高版本。  
  
 在 Internet Explorer 中，使用 [DataControl 对象 (RDS) ](../../reference/rds-api/datacontrol-object-rds.md)来显示返回的记录集。  
  
 若要创建此方案，需要执行以下步骤：  
  
-   设置应用程序  
  
-   获取数据  
  
-   发送数据  
  
-   接收并显示数据  
  
## <a name="step-1-set-up-the-application"></a>步骤1：设置应用程序  
 使用脚本权限创建名为 "XMLPersist" 的 IIS 虚拟目录。 在虚拟目录指向的文件夹中创建两个新的文本文件，一个名为 "XMLResponse"，另一个名为 "Default.htm"。  
  
## <a name="step-2-get-the-data"></a>步骤2：获取数据  
 在此步骤中，你将编写用于打开 ADO 记录集的代码，并准备将其发送到客户端。 使用文本编辑器（如记事本）打开文件 XMLResponse，并插入以下代码。  
  
```  
<%@ language="VBScript" %>  
  
<!-- #include file='adovbs.inc' -->  
  
<%  
  Dim strSQL, strCon  
  Dim adoRec   
  Dim adoCon   
  Dim xmlDoc   
  
  ' You will need to change "MySQLServer" below to the name of the SQL   
  ' server machine to which you want to connect.  
  strCon = "Provider=sqloledb;Data Source=MySQLServer;Initial Catalog=Pubs;Integrated Security=SSPI;"  
  Set adoCon = server.createObject("ADODB.Connection")  
  adoCon.Open strCon  
  
  strSQL = "SELECT Title, Price FROM Titles ORDER BY Price"  
  Set adoRec = Server.CreateObject("ADODB.Recordset")  
  adoRec.Open strSQL, adoCon, adOpenStatic, adLockOptimistic, adCmdText  
```  
  
 请确保将中参数的值更改 `Data Source` `strCon` 为 Microsoft SQL Server 计算机的名称。  
  
 使文件保持打开状态，然后继续执行下一步。  
  
## <a name="step-3-send-the-data"></a>步骤3：发送数据  
 现在您有了一个记录集，您必须将它作为 XML 保存到 ASP 响应对象，才能将其发送到客户端。 将以下代码添加到 XMLResponse 的底部。  
  
```  
  Response.ContentType = "text/xml"  
  Response.Expires = 0  
  Response.Buffer = False  
  
  Response.Write "<?xml version='1.0'?>" & vbNewLine  
  adoRec.save Response, adPersistXML  
  adoRec.Close  
  Set adoRec=Nothing  
%>  
```  
  
 请注意，ASP 响应对象被指定为记录集 [保存方法](../../reference/ado-api/save-method.md)的目标。 Save 方法的目标可以是支持 IStream 接口的任何对象，例如 ADO [Stream 对象 (ado) ](../../reference/ado-api/stream-object-ado.md)，或包含要将记录集保存到的完整路径的文件名称。  
  
 在转到下一步之前，保存并关闭 XMLResponse。 同时将 adovbs 文件从默认 ADO 库安装文件夹复制到保存 XMLResponse 文件的相同文件夹中。  
  
## <a name="step-4-receive-and-display-the-data"></a>步骤4：接收并显示数据  
 在此步骤中，将创建一个 HTML 文件，其中包含嵌入的 [DataControl 对象 (RDS) ](../../reference/rds-api/datacontrol-object-rds.md) 对象，该对象指向 XMLResponse 文件以获取记录集。 使用文本编辑器（如记事本）打开 default.htm，然后添加以下代码。 将 URL 中的 "sqlserver" 替换为服务器的名称。  
  
```  
<HTML>  
<HEAD><TITLE>ADO Recordset Persistence Sample</TITLE></HEAD>  
<BODY>  
  
<TABLE DATASRC="#RDC1" border="1">  
  <TR>  
<TD><SPAN DATAFLD="title"></SPAN></TD>  
<TD><SPAN DATAFLD="price"></SPAN></TD>  
  </TR>  
</TABLE>  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="RDC1">  
   <PARAM NAME="URL" VALUE="XMLResponse.asp">  
</OBJECT>  
  
</BODY>  
</HTML>  
```  
  
 关闭 default.htm 文件，并将其保存到保存 XMLResponse 的同一文件夹中。 使用 Internet Explorer 4.0 或更高版本，打开 URL https://*sqlserver*/XMLPersist/default.htm 并观察结果。 数据显示在绑定的 DHTML 表中。 现在打开 URL https:// *sqlserver* /XMLPersist/XMLResponse.asp 并观察结果。 将显示 XML。  
  
## <a name="see-also"></a>另请参阅  
 [Save 方法](../../reference/ado-api/save-method.md)   
 [以 XML 格式保留记录](./persisting-records-in-xml-format.md)