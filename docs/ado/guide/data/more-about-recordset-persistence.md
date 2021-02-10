---
description: 更多有关记录集暂留的信息
title: 有关记录集持久性的详细信息 |Microsoft Docs
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
- updating data [ADO], persisting data
ms.assetid: a9b287f5-04b0-4514-8143-f67879ca9842
author: rothja
ms.author: jroth
ms.openlocfilehash: ce3b22ecd5913db0045f82e72aa20b0833a5109b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037257"
---
# <a name="more-about-recordset-persistence"></a>更多有关记录集暂留的信息
ADO 记录集对象支持使用其 [Save](../../reference/ado-api/save-method.md)方法在文件中存储 **记录集** 对象的内容。 持久存储的文件可能位于本地驱动器、服务器或网站上的 URL。 稍后，可以通过 **Recordset** 对象的 [Open](../../reference/ado-api/open-method-ado-recordset.md)方法或 [连接](../../reference/ado-api/connection-object-ado.md)对象的 [Execute](../../reference/ado-api/execute-method-ado-connection.md)方法来还原文件。  
  
 此外， [GetString](../../reference/ado-api/getstring-method-ado.md) 方法将 **记录集** 对象转换为一个窗体，其中的列和行用指定的字符分隔。  
  
 若要保存 **记录集**，请首先将其转换为可以存储在文件中的窗体。 **记录集** 对象可以存储在专用的高级数据 TABLEGRAM (ADTG) 格式或 open 可扩展标记语言 (XML) 格式。 下一节显示了 ADTG 示例。 有关 XML 持久性的详细信息，请参阅 [以 xml 格式保存记录](./persisting-records-in-xml-format.md)。  
  
 保存持久文件中的任何挂起的更改。 这样一来，您就可以发出返回 **recordset** 对象的查询，编辑 **记录集**，保存并保存挂起的更改，然后再还原 **记录集**，然后使用保存的挂起更改更新数据源。  
  
 有关永久存储 **流** 对象的信息，请参阅 [流和暂留](./streams-and-persistence.md)。  
  
 有关 **记录集** 持久性的示例，请参阅 XML 记录集持久性方案。  
  
## <a name="example"></a>示例  
  
### <a name="save-a-recordset"></a>保存记录集：  
  
```  
Dim rs as New ADODB.Recordset  
rs.Save "c:\yourFile.adtg", adPersistADTG  
```  
  
### <a name="open-a-persisted-file-with-recordsetopen"></a>使用 Recordset 打开持久化文件。打开：  
  
```  
Dim rs as New ADODB.Recordset  
rs.Open "c:\yourFile.adtg", "Provider=MSPersist",,,adCmdFile  
```  
  
 如果 **记录集** 没有活动连接，则可以选择接受以下所有默认值和代码：  
  
```  
Dim rs as New ADODB.Recordset  
rs.Open "c:\yourFile.adtg"  
```  
  
### <a name="open-a-persisted-file-with-connectionexecute"></a>使用 Connection.Exe漂亮的方式打开一个持久文件：  
  
```  
Dim conn as New ADODB.Connection  
Dim rs as ADODB.Recordset  
conn.Open "Provider=MSPersist"  
Set rs = conn.execute("c:\yourFile.adtg")  
```  
  
### <a name="open-a-persisted-file-with-rdsdatacontrol"></a>使用 RDS 打开一个持久文件。DataControl  
 在这种情况下，不设置 **服务器** 属性。  
  
```  
Dim dc as New RDS.DataControl  
dc.Connection = "Provider=MSPersist"  
dc.SQL = "c:\yourFile.adtg"  
dc.Refresh  
```  
  
## <a name="see-also"></a>另请参阅  
 [GetString 方法 (ADO) ](../../reference/ado-api/getstring-method-ado.md)   
 [Microsoft OLE DB 永久性提供程序 (ADO 服务提供程序) ](../appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md)   
 [ADO)  (Recordset 对象 ](../../reference/ado-api/recordset-object-ado.md)   
 [流和暂留](./streams-and-persistence.md)