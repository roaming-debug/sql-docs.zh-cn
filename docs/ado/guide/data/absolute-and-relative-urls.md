---
description: 绝对和相对 URL
title: 绝对和相对 Url |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- relative URLs [ADO]
- absolute URLs [ADO]
- URLs [ADO]
ms.assetid: 6a34a7ef-50cc-4c3d-82f7-106b9a8f3caf
author: rothja
ms.author: jroth
ms.openlocfilehash: 50535a71c1062bc04aa009bea1aab08d3cf1faf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028692"
---
# <a name="absolute-and-relative-urls"></a>绝对和相对 URL
URL 指定本地计算机或联网计算机上存储的目标位置。 目标可以是文件、目录、HTML 页面、图像、程序等。  
  
 *绝对 URL* 包含查找资源所需的所有信息。  
  
 *相对 url* 使用绝对 url 作为起始点查找资源。 实际上，通过连接绝对和相对 Url 来指定目标的 "完整 URL"。  
  
 *绝对 URL* 使用以下格式： *scheme://server/path/resource*  
  
 相对 URL 通常只包含 *路径* 和 *资源*，但不包括 *方案* 或 *服务器*。 下表定义完整 URL 格式的各个部分。  
  
 scheme  
 指定访问 *资源* 的方式。  
  
 服务器  
 指定 *资源* 所在的计算机的名称。  
  
 *路径*  
 指定通向目标的目录的序列。 如果省略了 *资源* ，则目标是 *路径* 中的最后一个目录。  
  
 *resource*  
 如果包含，则 *资源* 是目标，通常为文件的名称。 它可以是一个 *简单文件，* 其中包含单个二进制字节流或 *结构化文档，* 其中包含一个或多个存储和字节流。  
  
## <a name="url-scheme-registration"></a>URL 方案注册  
 如果提供程序支持 Url，则提供程序将注册一个或多个 URL 方案。 注册意味着使用方案的任何 Url 都将自动调用已注册的提供程序。 例如，将 *http* 方案注册到 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 ADO 假定所有以 "http" 为前缀的 Url 表示要与 Internet 发布提供程序一起使用的 Web 文件夹或文件。 有关提供程序注册的方案的信息，请参阅提供程序文档。  
  
## <a name="defining-context-with-a-url"></a>使用 URL 定义上下文  
 [连接](../../reference/ado-api/connection-object-ado.md)对象表示的打开连接的一个功能是，将后续操作限制为该连接所表示的数据源。 也就是说，该连接定义了用于后续操作的上下文。  
  
 使用 ADO 2.7 或更高版本时，绝对 URL 还可以定义上下文。 例如，使用绝对 URL 打开 [记录](../../reference/ado-api/record-object-ado.md) 对象时，将隐式创建一个 **连接** 对象来表示由 URL 指定的资源。  
  
 可在 **Record** 对象 [Open](../../reference/ado-api/open-method-ado-record.md)方法的 *ActiveConnection* 参数中指定定义上下文的绝对 URL。 绝对 URL 还可以指定为 **连接** 对象 [打开](../../reference/ado-api/open-method-ado-connection.md) 方法 *CONNECTIONSTRING* 参数中的 "URL =" 关键字的值，并且 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象 [打开](../../reference/ado-api/open-method-ado-recordset.md) 方法 *ActiveConnection* 参数。  
  
 还可以通过打开表示目录的 **记录** 或 **记录集** 对象来定义上下文，因为这些对象已具有指定上下文的隐式或显式声明的 **连接** 对象。  
  
## <a name="scoped-operations"></a>作用域操作  
 上下文还定义作用域，即，可参与后续操作的目录及其子目录。 **Record** 对象具有多个作用域方法，这些方法对目录及其所有子目录进行操作。 这些方法包括 [CopyRecord](../../reference/ado-api/copyrecord-method-ado.md)、 [MoveRecord](../../reference/ado-api/moverecord-method-ado.md)和 [DeleteRecord](../../reference/ado-api/deleterecord-method-ado.md)。  
  
## <a name="relative-urls-as-command-text"></a>相对 Url 作为命令文本  
 您可以通过在 **连接** 对象的 [Execute](../../reference/ado-api/execute-method-ado-connection.md)方法的 *CommandText* 参数中键入字符串，并在 **Recordset** 对象的 [Open](../../reference/ado-api/open-method-ado-recordset.md)方法的 *source* 参数中键入字符串，来指定要对数据源执行的命令。  
  
 可以在 *CommandText* 或 *Source* 参数中指定相对 URL。 相对 URL 并不实际表示一个命令，如 SQL 命令;它仅指定参数。 活动连接的上下文必须是绝对 URL，并且 *选项* 参数必须设置为 **adCmdTableDirect**。  
  
 例如，下面的代码示例演示如何在 Winnt/system32 目录的 Readme25.txt 文件中打开 **记录集** ：  
  
```  
recordset.Open "system32/Readme25.txt", "URL=https://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
 连接字符串中的绝对 URL 指定服务器 (`YourServer`) ，路径 (`Winnt`) 。 此 URL 还定义上下文。  
  
 命令文本中的相对 URL 使用绝对 URL 作为起点，并指定路径的其余部分 (`system32`) 和要打开的文件 (`Readme25.txt`) 。  
  
 选项字段 (`adCmdTableDirect`) 指示命令类型为相对 URL。  
  
 作为另一个示例，以下代码将打开该目录的内容的 **记录集** `Winnt` ：  
  
```  
recordset.Open "", "URL=https://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
## <a name="ole-db-provider-supplied-url-schemes"></a>OLE DB Provider-Supplied URL 方案  
 完全限定 URL 的主要部分是用于访问由 URL 的其余部分标识的资源的 *方案* 。 例如 HTTP (超文本传输协议) 和 FTP (文件传输协议) 。  
  
 ADO 支持可识别自己的 URL 方案 OLE DB 提供程序。 例如， [用于 Internet 发布的 Microsoft OLE DB 提供程序](../appendixes/microsoft-ole-db-provider-for-internet-publishing.md)*（* 访问 "已发布的" Windows 2000 文件）可以识别现有的 HTTP 方案。  
  
## <a name="see-also"></a>另请参阅  
 [ADO) 的连接对象 (](../../reference/ado-api/connection-object-ado.md)   
 [ (ADO 记录对象) ](../../reference/ado-api/record-object-ado.md)   
 [记录集对象 (ADO)](../../reference/ado-api/recordset-object-ado.md)