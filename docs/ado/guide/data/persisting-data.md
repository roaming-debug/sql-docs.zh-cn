---
description: 保留数据
title: 保存数据 |Microsoft Docs
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
ms.assetid: 21c162ca-2845-4dd8-a49d-e715aba8c461
author: rothja
ms.author: jroth
ms.openlocfilehash: f9a2ad809efbc847a2dfaf2292a15c1c7376e9b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037207"
---
# <a name="persisting-data"></a>保留数据
可移植计算 (例如，使用便携式计算机) 生成了可在已连接和已断开连接的状态下运行的应用程序。 ADO 为开发人员提供了将客户端游标记录集保存到磁盘并稍后重新加载该 **记录集** 的功能，从而增加了对此功能的支持。  
  
 在以下几种情况下，你可以使用此类功能，其中包括：  
  
-   **旅行：** 在路上获取应用程序时，必须提供更改和添加新记录的功能，以后再将这些记录重新连接到数据库并提交。  
  
-   不 **经常更新的查找：** 通常情况下，在应用程序中，表用作查找，例如州税务表。 它们不经常更新并且是只读的。 应用程序无需在每次启动应用程序时从服务器中重新读取这些数据，只需从本地保存的 **记录集中** 加载数据即可。  
  
 在 ADO 中，若要保存和加载 **记录集**，请使用 **记录集. 保存** 和记录集。在 ado **记录集** 对象上 **打开 (,,,, adCmdFile)** 方法。  
  
 您可以使用 **Recordset Save** 方法将 ADO **记录集** 保存到磁盘上的文件中。  (您还可以将 **记录集** 保存到 ADO **流** 对象中。 稍后将在本指南中讨论 **流** 对象。 ) 稍后，你可以使用 **Open** 方法在你准备好使用 **记录集** 时重新打开它。 默认情况下，ADO 将 **记录集** 保存到专有的 Microsoft Advanced Data TABLEGRAM (ADTG) 格式。 此二进制格式使用 **AdPersistADTG PersistFormatEnum** 值指定。 或者，可以选择使用 **adPersistXML** 将 **记录集** 作为 XML 来保存。 有关将记录集保存为 XML 格式的详细信息，请参阅 [以 Xml 格式保存记录](../../../ado/guide/data/persisting-records-in-xml-format.md)。  
  
 **Save** 方法的语法如下所示：  
  
```  
  
recordset.  
Save  
Destination, PersistFormat  
  
```  
  
 第一次保存 **记录集** 时，可以选择指定 *目标*。 如果省略 "*目标*"，则将创建一个新文件，其名称设置为 **记录集** 的 "[源](../../../ado/reference/ado-api/source-property-ado-recordset.md)" 属性的值。  
  
 如果随后在第一次保存后调用 **save** 或发生运行时错误，则省略 *目标*。 如果随后对新 *目标* 调用 **Save** ，则会将 **记录集** 保存到新目标。 但是，新目标和原始目标都将打开。  
  
 **保存** 不会关闭 **记录集** 或 *目标*，因此可以继续使用 **记录集** 并保存最近的更改。 在关闭 **记录集** 之前 *目标* 保持打开状态，在这段时间内，其他应用程序可以读取但不能写入 *目标*。  
  
 出于安全原因， **Save** 方法只允许使用由 Microsoft Internet Explorer 执行的脚本中的低和自定义安全设置。  
  
 如果在异步 **记录集** 提取、执行或更新操作正在进行时调用 **save** 方法，则 **将** 等待，直到异步操作完成。  
  
 记录从 **记录集** 的第一行开始保存。 当 **Save** 方法完成后，当前行位置将移到 **记录集** 的第一行。  
  
 为获得最佳结果，请将 [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) 属性设置为 **AdUseClient** ，并 **保存**。 如果提供程序不支持保存 **记录集** 对象所需的所有功能，则游标服务将提供该功能。  
  
 当 **记录集** 的 **CursorLocation** 属性设置为 **adUseServer** 时， **记录集** 的更新功能会受到限制。 通常，只允许 (依赖于) 提供程序功能的单表更新、插入和删除操作。 此配置中也不能使用 [Resync](../../../ado/reference/ado-api/resync-method.md) 方法。  
  
 由于 *目标* 参数可以接受支持 OLE DB **IStream** 接口的任何对象，因此可以将 **记录集** 直接保存到 ASP **响应** 对象。  
  
 在下面的示例中， **保存** 和 **打开** 方法用于保存 **记录集** ，稍后再将其重新打开：  
  
```  
'BeginPersist  
   conn.ConnectionString = _  
   "Provider=SQLOLEDB;Data Source=MySQLServer;" _  
      & "Integrated Security=SSPI;Initial Catalog=pubs"  
   conn.Open  
  
   conn.Execute "create table testtable (dbkey int " & _  
      "primary key, field1 char(10))"  
   conn.Execute "insert into testtable values (1, 'string1')"  
  
   Set rst.ActiveConnection = conn  
   rst.CursorLocation = adUseClient  
  
   rst.Open "select * from testtable", conn, adOpenStatic, _  
      adLockBatchOptimistic  
  
   'Change the row on the client  
   rst!field1 = "NewValue"  
  
   'Save to a file--the .dat extension is an example; choose  
   'your own extension. The changes will be saved in the file  
   'as well as the original data.  
   MyFile = Dir("c:\temp\temptbl.dat")  
   If MyFile <> "" Then  
       Kill "c:\temp\temptbl.dat"  
   End If  
  
   rst.Save "c:\temp\temptbl.dat", adPersistADTG  
   Set rst = Nothing  
  
   'Now reload the data from the file  
   Set rst = New ADODB.Recordset  
   rst.Open "c:\temp\temptbl.dat", , adOpenStatic, _  
      adLockBatchOptimistic, adCmdFile  
  
   Debug.Print "After Loading the file from disk"  
   Debug.Print "   Current Edited Value: " & rst!field1.Value  
   Debug.Print "   Value Before Editing: " & rst!field1.OriginalValue  
  
   'Note that you can reconnect to a connection and  
   'submit the changes to the data source  
   Set rst.ActiveConnection = conn  
   rst.UpdateBatch  
'EndPersist  
```  
  
## <a name="remarks"></a>备注  
 本部分包含以下主题。  
  
-   [更多有关记录集暂留的信息](../../../ado/guide/data/more-about-recordset-persistence.md)  
  
-   [保留筛选记录集和分层记录集](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md)  
  
-   [以 XML 格式保留记录](../../../ado/guide/data/persisting-records-in-xml-format.md)
