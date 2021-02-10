---
description: 命令流
title: 命令流 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- command streams [ADO]
- streams [ADO], command
ms.assetid: 0ac09dbe-2665-411e-8fbb-d1efe6c777be
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c0872f20754d093020ece2830d389de32f09032
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037737"
---
# <a name="command-streams"></a>命令流
ADO 始终支持由 **CommandText** 属性指定的字符串格式的命令输入。 作为替代方法，使用 ADO 2.7 或更高版本，还可以通过将流分配给 **CommandStream** 属性，来使用信息流进行命令输入。 可以分配 ADO **流** 对象，也可以指定支持 COM **IStream** 接口的任何对象。  
  
 命令流的内容只是从 ADO 传递到提供程序，因此提供程序必须支持流的命令输入才能使此功能正常工作。 例如，SQL Server 支持 XML 模板形式的查询或 Transact-sql 的 OpenXML 扩展。  
  
 由于流的详细信息必须由提供程序进行解释，因此必须通过设置 **方言** 属性来指定命令方言。 **方言** 的值是包含 GUID 的字符串，由提供程序定义。 有关提供程序支持的 **方言** 的有效值的信息，请参阅提供程序文档。  
  
## <a name="xml-template-query-example"></a>XML 模板查询示例  
 下面的示例通过 VBScript 写入 Northwind 数据库。  
  
 首先，初始化并打开将用于包含查询流的 **流** 对象：  
  
```  
Dim adoStreamQuery  
Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
adoStreamQuery.Open  
```  
  
 查询流的内容将是 XML 模板查询。  
  
 模板查询需要引用由标记的 sql： prefix 标识的 XML 命名空间 \<sql:query> 。 SQL SELECT 语句作为 XML 模板的内容包含，并分配给字符串变量，如下所示：  
  
```  
sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'>  
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME </sql:query>  
</ROOT>"  
```  
  
 接下来，将字符串写入流：  
  
```  
adoStreamQuery.WriteText sQuery, adWriteChar  
adoStreamQuery.Position = 0  
```  
  
 将 adoStreamQuery 分配给 ADO **命令** 对象的 **CommandStream** 属性：  
  
```  
Dim adoCmd  
Set adoCmd  = Server.CreateObject("ADODB.Command"")  
adoCmd.CommandStream = adoStreamQuery  
```  
  
 指定命令语言方言，此 **方言** 指示 SQL Server OLE DB 提供程序应如何解释命令流。 特定于提供程序的 GUID 指定的方言：  
  
```  
adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
```  
  
 最后，执行查询并将结果返回到 **Recordset** 对象：  
  
```  
Set objRS = adoCmd.Execute  
```
