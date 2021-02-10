---
description: 以 XML 格式保留记录
title: 用 XML 格式保存记录 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- XML persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: f3113ec4-ae31-428f-89c6-bc1024f128ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 6f29f624fe9f55287dcd8944fd04da0426d9552c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032619"
---
# <a name="persisting-records-in-xml-format"></a>以 XML 格式保留记录
与 ADTG 格式一样，XML 格式的 **记录集** 持久性是通过 Microsoft OLE DB 永久性提供程序实现的。 此提供程序从保存的 XML 文件或包含 ADO 生成的架构信息的流中生成只进只读行集。 同样，它可以采用 ADO **记录集**，生成 XML，并将其保存到文件或实现 COM **IStream** 接口的任何对象。  (事实上，文件只是支持 **IStream** 的对象的另一个示例。 ) 对于版本2.5 及更高版本，ADO 依赖 Microsoft XML PARSER (MSXML) 将 XML 加载到 **记录集中**;因此 msxml.dll 是必需的。  
  
> [!NOTE]
>  将分层 **记录集** 保存 () 为 XML 格式的数据形状时，有一些限制。 如果分层 **记录集** 包含挂起的更新，则无法保存到 XML，并且无法保存参数化分层 **记录集**。 有关详细信息，请参阅 [持久化筛选的和分层记录集](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md)。  
  
 将数据保存到 XML 并通过 ADO 再次加载数据的最简单方法是分别使用 **Save** 和 **Open** 方法。 下面的 ADO 代码示例演示如何将 **Titles** 表中的数据保存到名为 sav 的文件中。  
  
```  
Dim rs as new Recordset  
Dim rs2 as new Recordset  
Dim c as new Connection  
Dim s as new Stream  
  
' Query the Titles table.  
c.Open "provider=sqloledb;data source=MySQLServer;initial catalog=pubs;Integrated Security='SSPI'"  
rs.cursorlocation = adUseClient  
rs.Open "select * from titles", c, adOpenStatic  
  
' Save to the file in the XML format. Note that if you don't specify   
' adPersistXML, a binary format (ADTG) will be used by default.  
rs.Save "titles.sav", adPersistXML  
  
' Save the recordset into the ADO Stream object.  
rs.save s, adPersistXML  
rs.Close  
c.Close  
  
set rs = nothing  
  
' Reopen the file.  
rs.Open "titles.sav",,,,adCmdFile  
' Open the Stream back into a Recordset.  
rs2.open s  
```  
  
 ADO 始终保留整个 **记录集** 对象。 如果要保存 **Recordset** 对象的一部分行，请使用 **筛选器** 方法缩小行或更改选择子句。 但是，必须使用客户端光标打开 **记录集** 对象 (**CursorLocation**  =  **adUseClient**) ，才能使用 **筛选器** 方法保存行的子集。 例如，若要检索以字母 "b" 开头的标题，可以将筛选器应用于打开的 **Recordset** 对象：  
  
```  
rs.Filter "title_id like 'B*'"  
rs.Save "btitles.sav", adPersistXML  
```  
  
 ADO 始终使用客户端游标引擎行集在永久性提供程序生成的只进数据上生成可滚动的 bookmarkable **记录集** 对象。  
  
 本部分包含以下主题。  
  
-   [XML 暂留格式](../../../ado/guide/data/xml-persistence-format.md)  
  
-   [命名空间](../../../ado/guide/data/namespaces.md)  
  
-   [架构部分](../../../ado/guide/data/schema-section.md)  
  
-   [数据部分](../../../ado/guide/data/data-section.md)  
  
-   [XML 中的分层记录集](../../../ado/guide/data/hierarchical-recordsets-in-xml.md)  
  
-   [XML 中的记录集动态属性](../../../ado/guide/data/recordset-dynamic-properties-in-xml.md)  
  
-   [XSLT 转换](../../../ado/guide/data/xslt-transformations.md)  
  
-   [保存到 XML DOM 对象](../../../ado/guide/data/saving-to-the-xml-dom-object.md)  
  
-   [XML 安全注意事项](../../../ado/guide/data/xml-security-considerations.md)  
  
-   [XML 记录集暂留方案](../../../ado/guide/data/xml-recordset-persistence-scenario.md)
