---
title: 'SQLXML (的 SQLXMLOLEDB 提供程序简介) '
description: 了解 SQLXMLOLEDB 提供程序，该提供程序是通过 ActiveX 数据对象 (ADO) 公开 SQLXML 功能的 OLE DB 提供程序。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, properties
- adExecuteStream flag
- SQLXMLOLEDB Provider, about SQLXMLOLEDB Provider
ms.assetid: 2e3f3817-4209-4bf4-9f46-248c95bc6f1b
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6b3d36e6aae398c8b480a800cdd2dc6064eb220e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462888"
---
# <a name="introduction-to-the-sqlxmloledb-provider-sqlxml-40"></a>SQLXMLOLEDB 访问接口简介 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  SQLXMLOLEDB 访问接口是通过 ActiveX 数据对象 (ADO) 公开 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 功能的 OLE DB 访问接口。 但是，访问接口只能在 ADO 的“写入输出流”模式下执行命令。 SQLXMLOLEDB 访问接口不是行集访问接口。 执行命令时，必须指定 adExecuteStream 标志，该标志指示 ADO 使用您指定的输出流。  
  
 下面的示例演示了用于指定 adExecuteStream 标志的 Execute 命令的语法：  
  
```  
Dim oTestCommand As New ADODB.Command  
...  
oTestCommand.Properties("Output Stream").Value = oTestStream  
oTestCommand.Execute , , adExecuteStream  
...  
```  
  
## <a name="sqlxmloledb-provider-specific-properties"></a>特定于访问接口的 SQLXMLOLEDB 属性  
 SQLXMLOLEDB 访问接口公开下列特定于访问接口的连接属性。  
  
|连接<br /><br /> property|默认<br /><br /> （如果有）|说明|  
|-----------------------------|----------------------------|-----------------|  
|数据提供程序||提供 SQLXMLOLEDB 执行命令所使用的 OLE DB 访问接口的 PROGID。 从 SQLXML 4.0 和 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 开始，此访问接口包含在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中；因此，此属性值限制为“SQLNCLI11”。 有关详细信息，请参阅 [SQL Server Native Client 编程](../../../relational-databases/native-client/sql-server-native-client-programming.md)。|  
  
 SQLXMLOLEDB 访问接口公开下列特定于访问接口的命令属性。  
  
|命令<br /><br /> property|默认<br /><br /> （如果有）|说明|  
|--------------------------|----------------------------|-----------------|  
|基路径|""|指定基本文件路径。 基本文件路径用于指定 XML 样式表语言 (XSL) 或映射架构文件的位置。 基文件路径还用于解析 xsl 或映射架构文件的相对路径，该路径已在 XSL 或 Mapping 架构属性中指定。<br /><br /> 有关使用此属性的示例，请参阅 [&#40;SQLXMLOLEDB Provider&#41;执行 XPath 查询 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-sqlxmloledb-provider.md)。|  
|ClientSideXML|False|如果希望在客户端上执行行集到 XML 的转换过程，而不是在服务器上执行，请将此属性设置为 True。 在希望将性能负载移到中间层时，此操作很有用。<br /><br /> 有关使用此属性的示例，请参阅 [&#40;SQLXMLOLEDB 提供程序中执行 Sql 查询&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-sql-queries-sqlxmloledb-provider.md) 或 [执行包含 &#40;SQLXMLOLEDB PROVIDER&#41;的 Sql 查询的模板 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-templates-that-contain-sql-queries-sqlxmloledb-provider.md)。|  
|内容类型||返回输出内容类型。 这是一个 READ ONLY 属性。<br /><br /> 该属性向浏览器提供有关内容类型的信息（例如 TEXT/XML、TEXT/HTML、image/jpeg 等）。 此属性的值将成为 HTTP 标头中发送到浏览器的 **content 类型** 字段，其中包含作为正文发送的文档) 的 MIME 类型 (多用途 Internet 邮件扩展。|  
|映射架构|Null|如果客户端应用程序针对映射架构（XDR 或 XSD）执行 XPath 查询，该属性则用于指定映射架构的名称。<br /><br /> 指定的路径可以是相对路径 (xyz/abc/MySchema.xml)，也可以是绝对路径 (C:\MyFolder\abc\MySchema.xml)。<br /><br /> 如果指定了相对路径，则使用 "基路径" 属性指定的基路径来解析相对路径。 如果未在 "基路径" 属性中指定路径，则相对路径是相对于当前目录的路径。<br /><br /> 在为 "映射架构" 属性指定值的情况下，您可以指定本地目录路径或 (https://) 的 URL。如果指定 URL，则必须将 WinHTTP 配置为通过代理服务器访问 HTTP 和 HTTPS 服务器。 执行 Proxycfg.exe 实用工具可实现这一目的。 有关详细信息，请参阅 MSDN 库中的“使用 WinHTTP 代理配置实用工具”。<br /><br /> 有关使用此属性的示例，请参阅 [&#40;SQLXMLOLEDB Provider&#41;执行 XPath 查询 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-sqlxmloledb-provider.md)。|  
|namespaces||该属性允许执行使用命名空间的 XPath 查询。 有关使用此属性的示例，请参阅 [使用命名空间执行 XPath 查询 &#40;SQLXMLOLEDB 提供程序&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-with-namespaces-sqlxmloledb-provider.md)。|  
|ss Stream Flags||该属性用于指定特定类型的安全性限制。 例如，您可能希望禁用对文件的 URL 引用或文件的绝对路径（如外部站点）， 或者您可能不希望在模板中进行查询。<br /><br /> 可为此属性分配以下值：<br /><br /> 1 = STREAM_FLAGS_DISALLOW_URL 2 = STREAM_FLAGS_DISALLOW_ABSOLUTE_PATH 4 = STREAM_FLAGS_DISALLOW_QUERY 8 = STREAM_FLAGS_       DONTCACHEMAPPINGSCHEMA 16 = STREAM_FLAGS_DONTCACHETEMPLATE 32 = STREAM_FLAGS_DONTCACHEXSL<br /><br /> 下一个表中提供了有关这些值的其他信息。|  
|xml root||该属性用于定义生成的 XML 的根标记。 例如，如果针对数据库执行 SQL 查询，并且生成的 XML 文档没有任何单个根元素，则使用该属性的值向文档添加单个根元素。<br /><br /> 有关使用此属性的示例，请参阅 [执行 SQL 查询 &#40;SQLXMLOLEDB 提供程序&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-sql-queries-sqlxmloledb-provider.md)。|  
|xsl||此属性用于在对查询返回的 XML 文档应用 XSL 转换时指定 XSL 文件名。<br /><br /> 指定的路径可以是相对路径 (xyz/abc/MyXSL.xsl)，也可以是绝对路径 (C:\MyFolder\abc\MyXSL.xsl)。<br /><br /> 如果指定了相对路径，则使用 "基路径" 属性指定的基路径来解析相对路径。 如果未在 "基路径" 属性中指定路径，则相对路径是相对于当前目录的路径。<br /><br /> 有关使用此属性的示例，请参阅应用 XSL 转换 (SQLXMLOLEDB 提供程序) 。|  
  
 下表包含 ss 流标志属性值的说明。  
  
|属性值|说明|  
|--------------------|-----------------|  
|STREAM_FLAGS_DISALLOW_URL|映射架构或 XSL 不接受 URL。|  
|STREAM_FLAGS_DISALLOW_ABSOLTE_PATH|为映射架构或 XSL 指定的路径必须是相对于模板自身的基本路径的相对路径。|  
|STREAM_FLAGS_DISALLOW_QUERY|禁止在模板中执行查询。|  
|STREAM_FLAGS_DONTCACHEMAPPINGSCHEMA|未缓存映射架构。 当需要更改数据库架构时，该属性值在数据库开发阶段非常有用。|  
|STREAM_FLAGS_DONTCACHETEMPLATE|未缓存模板。|  
|STREAM_FLAGS_DONTCACHEXSL|未缓存 XSL。|  
  
  
